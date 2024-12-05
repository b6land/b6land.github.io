---
layout: post
title: C# 用匿名型別產生好讀的 JSON
date: 2024-10-02 17:00:00 +0800
categories: [C#]
--- 

在 C 中，匿名型別 (Anonymous Types) 不須預先定義資料型別和結構，由編譯器產生型別名稱，常用於儲存臨時性的資料。

### 匿名型別的使用方式

直接使用 `new { var = value, var2 = value2}`  產生匿名類別，如果要產生陣列，則可以使用 `new[] { new {name = "alice"}, new {name = "bob"}}` 語法。

如果出現編譯器無法判斷型別的情形，例如要指定為 int 型別，可以使用 `new int {var = 1}`  的語法指定為 int 型別。

⚠️請注意，產生的匿名物件是唯讀的，沒辦法再修改內部屬性。

### 轉成 JSON 的範例

一般來說，要在 C# 內建立 Web Request，而且要附帶 JSON 格式的 Body，使用 string 存放時，真的不太好閱讀或修改。透過建立匿名型別，我們可以建立結構化的請求資料，再使用 Newtonsoft.Json 序列化轉成 JSON Body。以下是範例：

```csharp
int N = 15;
string Keyword = "蘋果";

var query = new
{
    size = N,
    query = new
    {
        query = Keyword,
        fields = new[] { "title", "content" },
        parameter = new object[]
        {
            new {
                Category = new {
                    name = "fruit"
                }
            },
            new {
                Options = new {
                    Highlight = false
                }
            }
        }
    }
};
string body = JsonConvert.SerializeObject(query);
Console.WriteLine(body);
```

輸出內容如下 (經換行整理過)：

```json
{
    "size": 15,
    "query": {
        "query": "蘋果",
        "fields": [
            "title",
            "content"
        ],
        "parameter": [
            {
                "Category": {
                    "name": "fruit"
                }
            },
            {
                "Options": {
                    "Highlight": false
                }
            }
        ]
    }
}
```

### 參考資料

- [c# - How to create JSON from strings without creating a custom class using JSON.Net library - Stack Overflow](https://stackoverflow.com/questions/44895545)  
- [匿名類型 - C# - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/csharp/fundamentals/types/anonymous-types)