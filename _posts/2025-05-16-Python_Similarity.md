---
layout: post
title: Python 計算語意相似度
date: 2025-05-16 21:00:00 +0800
categories: [Python]
--- 

要怎麼用計算的方式，來測量兩組文字間的相似度呢？有兩種簡單的方法。

### 1\. 編輯距離（Edit Distance / Levenshtein Distance）

- 計算兩個字串的最小編輯（插入、刪除、替換）步驟數。
- 適用場景：適用於簡單的拼寫變化，如「Xiao Ming」和「Hsiao Ming」。
- 詳細解說：[白話文解說 Levenshtein Distance（萊文斯坦距離） - MyApollo](https://myapollo.com.tw/blog/levenshtein-distance-explanation/)

### 2\. Jaccard 相似度（Jaccard Similarity）

- 計算兩個字串的「字元組（n-grams）」或「詞語」的交集與聯集比例。
- 適用場景：適用於比較數個詞語相似度，且不必考慮順序的情形，如「TRA Taipei Station」和「Taipei Station TRA」是完全相同的。
- 詳細解說：[用白話文談數學公式 - Jaccard Index (雅卡爾指數) - MyApollo](https://myapollo.com.tw/blog/jaccard-index-explaination/)

### 範例

以上兩個方法可以在 Python 的 [textdistance](https://github.com/life4/textdistance) 套件裡找到，首先先安裝套件：

```
pip install textdistance
```

接著用以下語法測試：

```python
import textdistance

print(textdistance.jaccard("TRA Taipei Station", "Taipei Station TRA"))  
print(textdistance.levenshtein.normalized_similarity("Xiao Ming", "Hsiao Ming"))
```

也可以參考：[textdistance，一个神奇的 Python 库！ - 知乎](https://zhuanlan.zhihu.com/p/686490503)