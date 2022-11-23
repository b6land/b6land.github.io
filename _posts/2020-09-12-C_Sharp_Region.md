---
layout: post
title: C# 使用 Region 的優缺點
date: 2020-09-12 12:00:00 +0800
categories:  [C#]
--- 

本篇文章介紹 Region 的使用方式，與不使用 Region 分隔程式的理由。(本文於 2022/11/23 從自己的鐵人賽[文章](https://ithelp.ithome.com.tw/articles/10303516)更新)

### Region 是什麼，使用的理由

可以使用 `#region` 和 `#endregion` 來包圍程式碼區域。

```cs
#region
// 加入你的程式碼
#endregion
```

`#region` 後方也可以加入一段文字作為標籤。

包圍以後，左方就有 + 或 - 按鈕可以摺疊程式碼，可以增加整體程式碼的可讀性，也能瞭解這段程式碼的大致用途。


### StackExchange 上的回答，不使用的理由

- 以下大略翻譯連結內的答案： [c# - Why are people so strongly opposed to #region tags in methods? - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/118818/why-are-people-so-strongly-opposed-to-region-tags-in-methods)
- 你可以單獨測試一個方法，但是不能單獨測試一個 Region。
- Region 和原本的語法可能會互相衝突 (例如 `region` 插入在 `if` 前面，`endregion` 插入在 `if` 內。)。
- 遇到沒有程式碼折疊的編輯器時，只能看到一大串又臭又長的程式碼。因此在寫程式碼時，應時常改寫，將程式碼分隔開來。
- 重構方法以後，方法至少會和使用 Region 分隔的結果一樣好。
- SOLID 原則中的單一職責原則，建議一個模組 (方法、類別 …) 只應該負責一件事。(這應該是要求撰寫邏輯時就實行此原則，而非用 Region 處理) 

### 其它

- 實務上，仍能看到不少程式碼用 region 來分隔程式，還會順便當註解用。是否要使用，應該也要一併考慮團隊的程式編寫風格。 *(個人觀察)*
- 可考慮使用類別等模組來代替 Region，來自 [C# Region and endregion - Dot Net Perls](https://www.dotnetperls.com/region)。