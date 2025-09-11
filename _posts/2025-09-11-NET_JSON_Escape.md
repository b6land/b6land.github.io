---
layout: post
title: ASP.NET 輸出 JSON 時將非 ASCII 字元 escape 成 `\uXXXX` 格式的解法
date: 2025-09-11 22:45:00 +0800
categories: [ASP.NET]
---  

在 .NET / ASP.NET Core 預設使用 `System.Text.Json`序列化，它在輸出 JSON 時，會將非 ASCII 字元 (像是中文) escape (逸出) 成 `\uXXXX` 格式，例如「中壢」會變成 `\u4E2D\u58E2`。雖然在反序列化時，通常也會還原回原本的文字，但是遇到不會還原的狀況時，要怎麼設定直接輸出中文呢？

### 設定 JSON 輸出不 escape 中文

如果希望**直接輸出中文**，而不是 `\uXXXX` 格式，可以在 .NET API 客戶端呼叫時，修改 `System.Text.Json` 的設定：

```csharp
var options = new JsonSerializerOptions
{
    Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping, // 可輸出非 ASCII 字元
    WriteIndented = true // 要不要換行、縮排
};

var json = JsonSerializer.Serialize(obj, options); // obj 是被序列化的 JSON
```

這樣輸出的 JSON 就會是中文：

```json
{ "name": "中壢" }
```

而不是 escape 後的文字：

```json
{ "name": "\u4E2D\u58E2" }
```

### 參考資料

- 官方說明：[如何使用 System.Text.Json 自訂字元編碼 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/standard/serialization/system-text-json/character-encoding)
- 設定 Controller 預設的序列化行為：[.NET Core 3.1 API 的 JSON 編碼設定](https://greens2314.blogspot.com/2020/12/net-core-31-api-json1-visual-studio-2019.html)
- 設定 Refit 的序列化行為：[uwp - How to set a custom JSON Serializer in Refit - Stack Overflow](https://stackoverflow.com/questions/59027874/how-to-set-a-custom-json-serializer-in-refit)