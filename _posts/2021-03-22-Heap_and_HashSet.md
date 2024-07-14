---
layout: post
title: Heap、HashSet 和 Quick Sort
date: 2021-03-22 12:00:00 +0800
categories:  [Programming, Interview]
--- 

本篇介紹 Heap、HashSet 和 Quick Sort 的基本概念。

### 內文

- HashSet：一種集合，集合內的每個物件都包含 Hash (雜湊) 值，故 HashSet 內只會有一個相同的物件（數值）。當資料量大且不需要儲存重複資料時，計算雜湊值並放入集合，會比使用迴圈尋找、放在一般的資料結構中來得更快。
- 參考資料：[C# hashset 如何使用 - iT 邦幫忙](https://ithelp.ithome.com.tw/m/questions/10191489)
- Heap：中文可稱為堆積，是一種特殊的完全二元樹。分為最大堆積（父節點都比子節點大）和最小堆積（父節點都比子節點小），搜尋時的時間複雜度為 O(N)。
- 參考資料：[[資料結構] 堆積 (Heap) - iT 邦幫忙](https://ithelp.ithome.com.tw/m/articles/10206479)
- 快速排序：採用分而治之的方法排序陣列，有多種不同的實作版本，大致的過程為：

```
1. 從陣列中選出一個值作為基準值 (Pivot)，接著由資料的左方往陣列中間找比基準值大的值、右方往陣列中間找比基準值小的數值。找到後讓這兩個值交換位置。
2. 反覆地尋找並重複以上過程，直到左方和右方索引指到相同的位置。
3. 將基準值與索引位置上的值交換。
4. 從該位置將陣列分為較小和較大的兩個陣列，並回到步驟 1 繼續進行排序。
```

- 參考資料：[[演算法] 快速排序法 (Quick Sort) - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10202330)