---
layout: post
title: C# 在字串裡尋找多個字元 - IndexOfAny
date: 2024-11-16 11:00:00 +0800
categories: [C#]
--- 

C# 裡可以使用字串的 `IndexOfAny()` 方法，來一次尋找多個字，並回傳第一次找到的位置。

### 使用方法

例如今天想要從字串裡是否包含 `^` 、`&` 、`%`  等字元，或想找到第一次出現的位置，可以使用以下的語法：

```csharp
public int IndexOfAny (char[] anyOf);
public int IndexOfAny (char[] anyOf, int startIndex); // 可以從指定的字串位置開始查詢
```  

以此例子來說 `position`  = 5：

```csharp
string str = "Hello^_^World";
int position = str.IndexOfAny(new char[] { '^', '&', '%' });
```

找不到時會回傳 -1 給 `postition` ：

```csharp
string str = "Hello World";
int position = str.IndexOfAny(new char[] { '^', '&', '%' });
```

此外，還有另一個 `LastIndexOfAny()`  方法，可以取得最後一次出現的位置，以剛剛的例子再試一次：  

```csharp
string str = "Hello^_^World";
int position = str.LastIndexOfAny(new char[] { '^', '&', '%' });
```

`position`  = 7。

### 參考資料

- StackOverflow 的範例：[c# - Check if a string contains one of 10 characters - Stack Overflow](https://stackoverflow.com/questions/1390749/check-if-a-string-contains-one-of-10-characters )
- 官方說明：[String.IndexOfAny 方法 (System) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.string.indexofany?view=net-8.0)、[String.LastIndexOfAny 方法 (System) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.string.lastindexofany?view=net-8.0)