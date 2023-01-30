---
layout: post
title: 機器學習與訊號判斷的簡介
date: 2023-01-30 12:00:00 +0800
categories:  [Machine Learning]
---

如何利用機器學習判斷訊號 (例如：聲音) 並分類 ? 以下介紹大致的方向。

### 簡介

- 可以使用的演算法：SVM, PCA, 與各種不同的類神經網路(NN)。
- 可使用類神經網路 (NN) 和 卷積類神經網路 (CNN) 辨識聲音。根據以下參考資料的描述，將聲音轉成**頻域**會有較佳的預測正確率。
- RNN、LSTM 適合在**時域**上的輸入，其目標之一為根據學習內容**序列**的結果，預測內容**序列**的下一個值為何。也適合用在如內容序列翻譯等等的場合。
- 可以多嘗試看看不同的演算法，找出最佳的解答。

### 注意

- 留意收集的內容，若是較長的連續訊號，如何分段 (分類) ? 
- 取樣的品質如何 ? 是否在不同的訊號上，有著明顯的區別 ? 這與你的取樣器材、取樣格式有著一定關係。
- 找到合適的演算法後，留意其他人分析該種訊號時，通常需要到多高的準確率才算合理 ?

### 參考資料

以下連結可以告訴你，什麼狀況下要用什麼樣的演算法處理問題。

- [五種可以用機器學習回答的問題](https://brohrer.mcknote.com/zh-Hant/using_machine_learning/five_questions_data_science_answers.html)
- [演算法秘笈](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-algorithm-cheat-sheet)
- [Recognizing Sounds (A Deep Learning Case Study)](https://medium.com/@awjuliani/recognizing-sounds-a-deep-learning-case-study-1bc37444d44d)