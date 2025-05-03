---
layout: post
title: Elasticsearch 儲存經緯度與計算距離
date: 2025-05-03 17:00:00 +0800
categories: [Elasticsearch]
---  

如果要用到 Elasticsearch 建立基於位置的服務 (LBS, Location Based Service )，可以閱讀以下的文章，了解索引裡儲存經緯度的方式，以及如何在查詢時加入經緯度作為條件。

### Elasticsearch 儲存經緯度的方式

可以用 `geo_point`  和 `geo_shape`  兩種類別來儲存地理資訊，前者用來儲存單一點座標的經緯度，後者用來儲存特定地理範圍，後續可以用來計算距離與對應的分數。以下介紹 `geo_point` 。

可以用以下的請求建立 `geo_point`  類別的欄位：

```json
PUT my-index-000001
{
  "mappings": {
    "properties": {
      "location": {
        "type": "geo_point"
      }
    }
  }
}
```

`geo_point`  以陣列表示時，格式為：`"lat, lng"` ；表示為陣列時，`lng`  經度在前，`lat`  緯度在後。請參考以下範例。

```json
PUT my-index-000001/_doc/3
{
  "text": "Geopoint as an object with 'lat' and 'lon' keys",
  "location": {
    "lat": 41.12,
    "lon": -71.34
  }
}

PUT my-index-000001/_doc/4
{
  "text": "Geopoint as an array",
  "location": [ -71.34, 41.12 ]
}
```

請求的內容來自：[Geopoint field type - Elasticsearch Guide \[8.14\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-point.html)。

### 在查詢時輸入所在地經緯度，根據距離遠近輸出結果

與 `geo_point`  相關的計算方式有許多種，例如：

1. `distance_feature` 可以用來「提高」特定經緯度附近的資料分數：[Distance feature query - Elasticsearch Guide \[8.14\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-distance-feature-query.html)
2. `geo_distance` 則可用來找尋「符合」特定經緯度範圍內的資料：[Geo-distance query - Elasticsearch Guide \[8.14\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-geo-distance-query.html)
3. 使用 `function_score` 評分函數的線性函數 (linear) 計算查詢經緯度與資料裡的經緯度的距離：[How to Set Up Geolocation Search in Your App with Elasticsearch](https://www.freecodecamp.org/news/geolocation-search-elasticsearch/)。
    - 原有的 query (和 query\_string) ，必須要放在 function\_score 內。

### 註記

關於 LBS，可以參考：[Location-based service - Wikipedia](https://en.wikipedia.org/wiki/Location-based_service)。常用於表示使用基地台或定位系統等提供的服務，例如：

- 檢索附近的商店資料
- 商店附近的訊息通知

其它 Elasticsearch 地理位置的應用，可以參考：

- [ElasticSearch - 地理座標點 geo\_point](https://kucw.io/blog/2019/12/elasticsearch-geo-point/) 
- [地理位置 · Elasticsearch权威指南中文版](https://wjw465150.github.io/Elasticsearch/6_Geolocation.html ) 
- [elasticsearch地理位置查询 - 掘金](https://juejin.cn/post/6953547277172473892)