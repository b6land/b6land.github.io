---
layout: post
title: C# 字串比較和轉換
date: 2026-09-09 22:30:00 +0800
categories: [C#]
---  

以下是字串比較和轉換一些要注意的地方。

### 字串比較

忽略大小寫的幾種選項：

| 選項  | 描述  | 用途  |
| --- | --- | --- |
| `CurrentCultureIgnoreCase`<br> | 根據作業系統目前的**區域性 (Culture)** 設定（例如使用者使用的是繁體中文、德文或土耳其文）來決定字串的比對規則。 | 適合用在**需要呈現給使用者**、且符合當地語言習慣的文字比對。<br> |
| `InvariantCultureIgnoreCase`<br> | 基於英文語系的規則來處理字串比對，不受目前作業系統的**區域性 (Culture)** 設定（例如使用者使用的是繁體中文、德文或土耳其文）影響。<br> | 跨文化環境下需保持一致性邏輯的字串比對。<br> |
| `OrdinalIgnoreCase`<br> | 直接比對位元組程式碼的方式，執行速度更快且更安全。<br> | 系統內部識別碼或檔案路徑。<br> |

參考：[C# 字串比較 - Vincent 筆記 - 點部落](https://dotblogs.com.tw/Walila/2020/12/15/124808)  

### 轉成 string 的兩種方式比較

```csharp
Page.Theme = Session["SessionTheme"] as string;
Page.Theme = Session["SessionTheme"].ToString();
```

`Session["SessionTheme"] as string` 如果物件不是 string 型別，會回傳 null；如果物件本身是 null，則回傳 null。

`Session["SessionTheme"].ToString()`如果不是 string 型別，會嘗試輸出 string；如果物件本身是 null，則拋出 null exception。

參考：[Difference between .ToString and "as string" in C# - Stack Overflow](https://stackoverflow.com/questions/2099900/difference-between-tostring-and-as-string-in-c-sharp)