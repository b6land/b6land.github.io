---
layout: post
title: C# 用空值結合運算子處理 Null
date: 2023-01-20 12:00:00 +0800
categories: [C#]
---

`??`、`??=` 在 C# 中，被稱為空值結合運算子 (null coalescing operator)，C# 8.0 以後支援。

### `??` 的用法

可以用 `??` (兩個問號) 快速設定 null 時要如何處理：

``` cs
StringBuilder log = oldLog ?? new StringBuilder();
```

相當於

```cs
StringBuilder log = oldLog != null ? oldLog : new StringBuilder();
```

也可拆解為

```cs
StringBuilder log;
if(oldLog != null){
    log = oldLog;
}
else{
    log = new StringBuilder();
}
```

### `??=` 的用法

可以用 ??= (二問號一等號)  指定空值時的預設值，例如：

```csharp
a ??= 10;
```

相當於：

```csharp
if (a == null) {
  a = 10;
}
```

### 參考資料

- 官方說明：[?? and ??= 運算子 - Null 聯合運算子 - C# reference - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/operators/null-coalescing-operator)
- 相關問答: [null coalescing operator - What do two question marks together mean in C#? - Stack Overflow](https://stackoverflow.com/questions/446835/what-do-two-question-marks-together-mean-in-c)