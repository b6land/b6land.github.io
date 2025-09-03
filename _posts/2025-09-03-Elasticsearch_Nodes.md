---
layout: post
title: Elasticsearch 如何設定節點，與合適配置
date: 2025-09-03 09:00:00 +0800
categories: [Elasticsearch]
---  

本篇介紹 Elasticsearch 的節點建立方式、概念和一些疑難雜症。

### 查詢節點資訊

```powershell
GET /_nodes
```

可以查詢節點資訊。

```
GET _cat/nodes?v=true
```

則能列出節點負載狀態。

### 節點的角色

以下列出配置 1 ~ 3 個節點時，常見的節點角色：

| 節點角色 | 描述  |
| --- | --- |
| Master-eligible | 可成為 Master 的節點 (候選)<br>負責管理 Data 角色的索引<br> |
| Voting only | 負責選出誰是 Master |
| Data | 專門負責索引查詢和 CRUD |

更多的節點角色介紹，可以進一步參考：

1. [ES Node 的種類 - 喬叔的 Elastic Stack 專業教育訓練](https://training.onedoggo.com/tech-sharing/uncle-joe-teach-es-elasticsearch/elastic-cloud-ru-he-jian-li-deployment/es-node-de-zhong-lei)  
2. [Nodes - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-node.html)  

### 節點的配置與數量

使用單一節點時，具備所有角色，有「節點失效就無法處理請求」的問題。至少要有 3 個節點，才有辦法解決單一節點失效的問題。

只有兩個節點時，不要讓兩個節點都是 Master-eligible Node。若網路若發生問題，導致兩邊無法溝通時，兩個節點都會選自己為 master，此時刪減資料時，可能會導致資料不一致 (Split-Brain 問題)。

解決的方法有兩種，一種是設定一個 Master-eligible, Data 節點 (主)、一個 Data 節點 (副)，此狀況下可以接受副節點失效；另外一種方法是增加一個 Vote-only 節點，負責決定哪個節點是 Master 節點。另外兩個皆為 Master-eligible, Data 節點。查詢的請求可以指定任一個包含 Data 屬性的節點。

相關介紹：

1. 官方說明：[Resilience in small clusters - Elasticsearch Guide \[8.14\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/high-availability-cluster-small-clusters.html)
2. 相關的討論：[Avoiding the Split Brain - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/avoiding-the-split-brain/128746/5)
3. [Elasticsearch 系統介紹與評估](https://yuanchieh.page/posts/2020/2020-07-08-elasticsearch-%E7%B3%BB%E7%B5%B1%E4%BB%8B%E7%B4%B9%E8%88%87%E8%A9%95%E4%BC%B0/)
4. [Elasticsearch 介紹 – 基本概念篇 - 歐立威科技](https://www.omniwaresoft.com.tw/techcolumn/elastic-techcolumn/elasticsearch-basic-concept/)
5. [如何開始搭建Elastic Stack及使用方式介紹-上 - 偉康科技洞察室](https://www.webcomm.com.tw/blog/elk_1/)

### 設定節點連線資訊

以下步驟需要在所有節點設定。

1 開啟 `elasticsearch.yml` ，設定叢集名稱、節點名稱、節點角色和所在的位址。

```yml
cluster.name: my_cluster_name
node.name: my_node_1
node.roles: [master, data]
network.host: 192.168.1.10
```

2 設定所有 Master-eligible 角色的節點，使其可被發現。

```yml
discovery.seed_hosts:
   - 192.168.1.2 
   - 192.168.1.3
   - 192.168.1.4 

cluster.initial_master_nodes:
   - master_node_1
   - master_node_2
   - master_node_3
```

3 啟動 Elasticsearch。

最後從 Master 節點建立索引，設定分片數量。

改寫自：[Elasticsearch Cluster Setup: A Step-by-Step Guide](https://opster.com/guides/elasticsearch/operations/elasticsearch-cluster-setup/)

附註：如果在本機建立多個節點測試，要額外指定 `http.port` (HTTP 請求，預設為 9200) 和 `transport.port` (Elasticsearch 之間的傳輸，預設為 9300) 設定值，避免 Port 被占用。


可以進一步參考：
1.  7.x 版本適用：[2-1、ElasticSearch 集群配置. 單一 ElasticSearch server… - by 十鱷魚 - twelvefish - Medium](https://medium.com/twelvefish/2-1-elasticsearch-%E9%9B%86%E7%BE%A4%E9%85%8D%E7%BD%AE-ebf1a1160755)
2. 8.x 版的設定方式：[Networking - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/8.15/modules-network.html#transport-settings)

### 設定節點安全連線

從 8.x 版本開始，預設啟用安全連線。因此需要設定 TLS (Transport Layer Security，和 SSL 相同)，才能建立安全連線，在各個加密的 Elasticsearch 節點之間溝通。

下列步驟 1 ~ 2 可以在任一個 Elasticsearch 節點上執行。

1\. 如果還沒設定過 Elasticsearch 的話，需要先設定 CA (憑證頒發機構)。(預設在第一次 Elasticsearch 啟動時會一起設定)

```
./bin/elasticsearch-certutil ca
```

2\. 建立憑證，建議輸入密碼。

```
./bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12
```

下列步驟 1  ~ 4 則需要在每一個 Elasticsearch 節點上執行。

1\. 將產生的 `elastic-stack-ca.p12`  憑證放到 config 目錄底下。

1-1\. 可能也要將 `elastic-certificates.p12`  憑證放到 config 目錄底下，但這樣做會導致每個節點使用同一個密碼 (和憑證)。

2\. 設定 `elasticsearch.yml` 的節點相關資訊，分別指定對應的 node.name、相同的 `cluster.name`  (若上方步驟尚未設定)，並設定加密傳輸。

```yml
cluster.name: my-cluster
node.name: node-1
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate 
xpack.security.transport.ssl.client_authentication: required
xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
xpack.security.transport.ssl.truststore.path: elastic-certificates.p12
```

3\. 若剛剛建立憑證時有輸入密碼，要執行以下指令輸入密碼：

```
./bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password
```

```
./bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password
```


4\. 啟動 Elasticsearch。

以上為官方說明的步驟，可進一步參考：[Set up basic security for the Elastic Stack - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-basic-setup.html)  

### 錯誤訊息

若 Elasticsearch 的命令視窗或 Log 出現「Received plaintext http traffic on an https channel」訊息，表示收到的訊息未使用安全連線。

[Received plaintext http traffic on an https channel - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/received-plaintext-http-traffic-on-an-https-channel/324074)  

### 節點退出舊叢集與加入新叢集

```plaintext
This node previously joined a cluster with UUID [ooo] and is now trying to join a different cluster with UUID [xxx]
```

表示先前已經加入別的叢集，一般狀況下不能臨時加入其它叢集，要先使用 `elasticsearch-node detach-cluster`，才能加入其它叢集。

請參考：[Elastic node reads previous cluster uuid from data folder even if it is re-configured to join another cluster - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/elastic-node-reads-previous-cluster-uuid-from-data-folder-even-if-it-is-re-configured-to-join-another-cluster/179471)

```plaintext
master not discovered yet, this node has not previously joined a bootstrapped (v7+) cluster, and this node must discover master-eligible nodes
```

表示找不到新的 master 節點，此時可以檢查 `elasticsearch.yml 的 discovery.seed_hosts`  是否正確設定，如果本身是 master 節點的話，應該執行 `elasticsearch-node unsafe-bootstrap` 登錄節點。

請參考：[Master not discovered yet, this node has not previously joined a bootstrapped - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/master-not-discovered-yet-this-node-has-not-previously-joined-a-bootstrapped/289075)

為既有節點重新設定新的叢集 master 和 data 節點，可以透過設定 Unsafe cluster bootstrapping，先登錄新的 master 節點，並退出叢集其它節點：

[elasticsearch-node - Elasticsearch Guide \[7.16\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/node-tool.html#node-tool-unsafe-bootstrap)