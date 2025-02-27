---
layout: post
title: ElasticAPM Transactions 頁面圖解
date: 2025-01-28 11:00:00 +0800
categories: [Other]
---  

ElasticAPM 的 Transactions 頁面，有很多可以判讀的資訊與圖表，知道這些圖表要表達的意思，有助於找出效能的癥結或錯誤原因。

## 內文
  

![查詢條件](/assets/imgs/2025-01-28/query.png)  

上方可以用許多條件過濾要查詢的 Transaction，最常見的是時間，也可以自己輸入條件 (條件可參考 [APM Transaction fields - APM User Guide \[7.17\] - Elastic](https://www.elastic.co/guide/en/apm/guide/7.17/exported-fields-apm-transaction.html))；此外還能在圖表背景和上一段時間範圍做比較。

![指定時間範圍](/assets/imgs/2025-01-28/date_pick.png)  

開始和結束的時間範圍，可以指定相對、絕對時間或現在。

![延遲、吞吐量與耗時比例](/assets/imgs/2025-01-28/latency.png)  

下面有 Transaction 的圖表，說明如下：

- Latency：延遲，單位為 ms，可查詢平均值、在 95 百分位數和 99 百分位數的延遲時間。(百分位數表示分布於 X 百分比的數值，定義請參考[第P百分位數的求法 - 翰林雲端學院](https://www.ehanlin.com.tw/app/keyword/%E5%9C%8B%E4%B8%AD/%E6%95%B8%E5%AD%B8/%E7%AC%ACP%E7%99%BE%E5%88%86%E4%BD%8D%E6%95%B8%E7%9A%84%E6%B1%82%E6%B3%95.html))
- Throughput：吞吐量，單位為 TPM (Transaction Per Minute，每分鐘交易量)。
- Time spent by span type：花費的時間比例，由 APM 判斷服務的類型，常見的包含 App、Http、SQL 等。

![錯誤率](/assets/imgs/2025-01-28/fail_rate.png)  

如果有請求失敗的話，則會在 Failed transaction rate 顯示出比例，並在旁邊的 Top 5 errors 列出錯誤原因。

  