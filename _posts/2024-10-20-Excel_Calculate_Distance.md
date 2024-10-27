---
layout: post
title: Excel 計算兩個經緯度之間的距離
date: 2024-10-20 17:00:00 +0800
categories: [Programming]
--- 

本文介紹如何用 Excel 計算兩個經緯度之間的距離，並附帶公式的說明。

### 利用 Excel 計算距離

以下公式 (來源：[calculate distance using latitude and longitude excel : r/excel](https://www.reddit.com/r/excel/comments/xe7j3d/calculate_distance_using_latitude_and_longitude/)) 可以帶入至 Excel 後使用：

```plaintext
=ACOS(COS(RADIANS(90-Lat1)) * COS(RADIANS(90-Lat2)) + SIN(RADIANS(90-Lat1)) * SIN(RADIANS(90-Lat2)) * COS(RADIANS(Long1-Long2))) * 6371 
```

將 `Lat1` 和 `Lon1` 分別帶入第一點的緯度、經度，`Lat2` 和 `Lon2` 分別帶入第二點的緯度、經度即可。

可以從距離計算機 ([Latitude/Longitude Distance Calculator](https://www.nhc.noaa.gov/gccalc.shtml)) 的引用來源 ([Aviation Formulary V1.47](http://edwilliams.org/avform147.htm#Dist))，找到原本的計算公式：

```plaintext
d=acos(sin(lat1)*sin(lat2)+cos(lat1)*cos(lat2)*cos(lon1-lon2))
```

(原本公式中的 `lon1`、`lon2`、`lat1` 和 `lat2` 需要使用 `RADIANS()` 或 `PI() / 180` 轉換成弧度)

公式可大概解釋如下：

- 這個公式是直接使用經緯度來計算兩點之間的球面距離，也叫做大圓距離公式 (great-circle distance formula)。
- 公式中的 `RADIANS` 將度數轉換成弧度。
- 最後將求得的弧度乘以 6371 (因為地球的半徑約為 6371 公里)，得到實際距離。

由於電腦計算的精度誤差、座標轉換時的誤差、球體模型視為正圓 (實際上地球是些微的橢圓形)，因此會有一點點誤差，但通常可忽略不計。如果需要非常精確的距離，如導航或地理資訊系統，可以用橢球體模型的 Vincenty 公式或 Haversine 公式計算。

### 其它參考資料

- ChatGPT
- Excel 公式的中文說明：[使用 Excel 計算2個地點之間的直線距離](http://ricky0512.blogspot.com/2015/01/excel-2.html)
- 大圓距離公式說明：[經緯度計算距離公式－別搗蛋 - 痞客邦](https://wywu.pixnet.net/blog/post/26533759)
- 另一個說明公式網頁：[Calculating Distance Between 2 Locations Of Latitude & Longitude (PHP, Python, MySQL)](https://martech.zone/calculate-great-circle-distance/)
- Excel 中角度的計算：[SIN 函數 - Microsoft 支援服務](https://support.microsoft.com/zh-tw/office/sin-%E5%87%BD%E6%95%B8-cf0e3432-8b9e-483c-bc55-a76651c95602)