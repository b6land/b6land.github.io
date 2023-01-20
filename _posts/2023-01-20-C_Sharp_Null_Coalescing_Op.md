---
layout: post
title: C# 空值結合運算子 - ??
date: 2023-01-20 12:00:00 +0800
categories: [C#]
---

`??` 在 C# 中，被稱為空值結合運算子 (null coalescing operator)。

### 用法

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

- 參考資料: [null coalescing operator - What do two question marks together mean in C#? - Stack Overflow](https://stackoverflow.com/questions/446835/what-do-two-question-marks-together-mean-in-c)