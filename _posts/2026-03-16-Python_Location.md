---
layout: post
title: 用 Python 建立查詢經緯度所在行政區 API
date: 2026-03-16 22:30:00 +0800
categories: [Python]
--- 

這篇文章教你怎麼用簡單的步驟建立 API，來查詢經緯度所在的行政區。

## 安裝

從官網安裝最新版本的 python，安裝完成後執行以下指令：

```powershell
pip install shapely
pip install fiona
pip install flask
```

### 步驟

1. 建立 Python 檔案
2. 將下載來的行政區資料「全部」放置於 Python 所在的目錄
3. 撰寫程式碼
4. 執行 Python 程式

### 程式碼

```python
import fiona
from shapely.geometry import shape, Point
from flask import Flask, request, jsonify

# 使用 Fiona 載入地理資料
collection = fiona.open('TOWN_MOI//TOWN_MOI_1130718.shp')

shapes = {}
properties = {}

# 指定地理形狀集合，並建立以 TOWNCODE 為索引的集合
def init_geometric():
    for f in collection:
        town_id = int(f['properties']['TOWNCODE'])
        shapes[town_id] = shape(f['geometry'])
        properties[town_id] = f['properties']

# 透過 Shapely 查詢是否包含在某個形狀內，並回傳
def search(x, y):
    return next((town_id
                 for town_id in shapes
                 if shapes[town_id].contains(Point(x, y))), None)

# 建立伺服器
app = Flask(__name__)

# 建立 POST API
@app.route('/SearchGeoPoint', methods=['POST'])
def postInput():
    # 取得前端傳過來的數值
    insertValues = request.get_json()
    lng=insertValues['Lng']
    lat=insertValues['Lat']

    # 查詢位置並回傳結果
    towncode = search(lng, lat)
    townname = properties[towncode]['TOWNNAME']
    countycode = properties[towncode]['COUNTYCODE']
    countyname = properties[towncode]['COUNTYNAME']

    result = {
        "towncode": towncode,
        "townname": townname,
        "countycode": countycode,
        "countyname": countyname
    }

    return jsonify(result)

# 載入資料並啟動 API 伺服器
if __name__ == '__main__':
    init_geometric()
    app.run(host='0.0.0.0', port=3000, debug=True)
```

### NOTE: 解決在本地端送出請求間隔很長的問題

使用 Flask 建立查詢經緯度所在縣市 API 後，應該就會想要用來批次處理資料。但是使用程式批次呼叫時，可能會發現：API 怎麼隔 1 ~ 2 秒才重複執行 ? 明明單次執行的時候，只要幾 ms 就完成查詢了！

如果你使用 `localhost:3000/SearchGeoPoint` 來呼叫 API，那問題可能出在 `Localhost` 上，因為這可能和預設使用 IPv6 或 hosts 檔案內複雜的路由設定有關。

解決方法是改用 `127.0.0.1:3000/SearchGeoPoint` 呼叫，就可以在幾 ms 內呼叫下一次 API 了。

### 參考資料

- 查詢經緯度所在行政區：[用 Python 計算經緯度對應的鄉鎮市區](https://op8867555.github.io/posts/2017-09-16-python-getting-town-from-gps-coord.html)
- 用 Flask 快速建立 API：[\[Day 29\] 使用 Python Flask 架設 API 吧！ - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10280422)
- 資料來源：[鄉鎮市區界線(TWD97經緯度) - 政府資料開放平臺](https://data.gov.tw/dataset/7441)
- 資料說明：[內政地理資訊圖資雲整合服務平台 (我國各鄉(鎮、市、區)行政區域界線圖資)](https://www.tgos.tw/MDE/MetaData/CRUD/ViewMetaData?GUID=08cccb3b-2ac9-4459-a3f0-8db0af3c04fc&VIEW_TYPE=only&SOURCE=1)
- 解決重複呼叫很慢的問題：[Why is python-requests to localhost slow? - Stack Overflow](https://stackoverflow.com/questions/47766158/why-is-python-requests-to-localhost-slow)