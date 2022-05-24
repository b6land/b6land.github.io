---
layout: post
title: C# 關鍵字 2 - out var, readonly
date: 2019-12-15 12:00:00 +0800
categories:  [C#]
---

以下介紹 out var 的使用方式，和 readonly、const 的不同。 

### out var

在 C# 中，常使用以下的方式將變數試圖剖析為特定型別的變數：

```
bool value;
bool.TryParse(object input, out object value);
```

在 C# 7.0 中，可以使用以下的語法，減少程式碼的使用：

```
int.TryParse(input, out var value);
```

`out var` 會幫你宣告該 `value` 變數。
效果沒有改變，是一種語法糖。

### readonly vs. const

`const` 的特色：
- 只能在宣告時給值
- 無法修改內容
- 只能是基本型別或字串

`readonly` 的特色：

- 只能在宣告時或建構子給值
- 建構子執行完畢後，無法再修改內容
- 跟一般變數一樣可以是任意型別
- 宣告 static readonly 只能在靜態建構子修改內容

### 參考資料

[C# Language - out var聲明 - c# Tutorial](https://riptutorial.com/zh-TW/csharp/example/6326/out-var%E8%81%B2%E6%98%8E)

[C# - const vs static readonly - John Wu's Blog](https://blog.johnwu.cc/article/c-sharp-const-vs-static-readonly.html)