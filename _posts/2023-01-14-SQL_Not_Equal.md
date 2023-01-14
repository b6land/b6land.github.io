---
layout: post
title: SQL Not Equal 的效能影響
date: 2023-01-14 12:00:00 +0800
categories: [SQL]
---

在 SQL 中，可用 `<>` 找出不等於特定值的資料，可是當有很多資料時，它的效能可能不如 `=` 或 `IN` 來得好。

### 內文

在 SQL 中，在條件式中用 `<>` 運算子，可找出不等於特定值的資料，並且也是 **SARGable** 的運算子之一，這表示它可以運用索引減少查詢的時間。

然而，在 [Sargable - Wikipedia](https://en.wikipedia.org/wiki/Sargable) 中，`<>` 被歸類在「Sargable operators that rarely improve performance」，對此也有網友在 [sql - Is <> SARGable or not? If not, what can I use instead? - Stack Overflow](https://stackoverflow.com/questions/15483508/is-sargable-or-not-if-not-what-can-i-use-instead) 提問，下方有網友提出的解答：

> 使用 `=` 的話，通常只需要從索引中取得單一或有限的資料，但 `<>` 通常會需要掃描整個索引，以取得相關的資料，因此效能幫助不如其他運算子來得多。

[Avoid Using Not Equal in WHERE Clause](https://www.mssqltips.com/sqlservertutorial/3203/avoid-using-not-equal-in-where-clause/) 這篇文章內可以看到，在同樣的查詢，只調整運算子，但查詢效果相同的語法，使用 `<>` 或 `IN` 都會去掃描索引。但是使用 `<>` 的查詢語法，明顯花了較多的成本去尋找索引，導致查詢速度較慢。

因此，應減少 `<>` 的使用，並考慮用其它運算子替代。


