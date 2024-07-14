---
layout: post
title: Confusion Matrix 是什麼，如何從 Continuous-Multioutput Label 轉換
date: 2018-07-03 12:00:00 +0800
categories:  [Machine Learning]
---

這篇文章將介紹什麼是 Confusion Matrix，接著說明 Keras 的 NN 模型推導時，如何從連續的輸出值，轉為類別數字。

### Confusion Matrix 是什麼

Confusion Matrix (混淆矩陣) 可以用來檢查實際資料和預測結果之間的差異，是判斷機器學習模型的指標之一。除了最常見的 True/False 表格以外，我們也可以建立多個類別的表格。

以下是一個手寫字母 a, b, c 圖片的判斷結果，使用 Confusion Matrix 來呈現：

|  | 預測 a | 預測 b | 預測 c |
| --- | --- | --- | --- |
| 實際 a | 46 |  | 4 |
| 實際 b |  | 50 |  |
| 實際 c | 2 |  | 48 |

上述的結果可以看出 b 的預測完全正確，有 4 張 a 圖被預測為 c 類別，2 張 c 圖被預測為 a 類別，為 0 時不顯示。

### 轉換標籤類型以產生 Confusion Matrix

Keras 產生的類神經網路架構，以 `Dense(2, activation='softmax')` 輸出計算結果的話，其輸出標籤的型態將會是 continuous-multioutput，即連續的輸出值。然而若與原本標籤型態不符合，則無法以 scikit-learn 內建的 `confusion_matrix()` 函式計算 Confusion Matrix。

使用以下的程式碼，可以將預測結果以類別數字的型式 class-vector (integer) 輸出，並輸出 Confusion Matrix。輸出型式和 binary class vector (multiclass-indicator, one-hot-encoding) 不相同。

``` py
from sklearn.metrics import confusion_matrix
prediction = model.predict_classes(X_test)
confusion_matrix(y_true, y_pred)
```

### 參考

- [Confusion matrix - Wikipedia](https://en.wikipedia.org/wiki/Confusion_matrix)
- [Simple guide to confusion matrix terminology](http://www.dataschool.io/simple-guide-to-confusion-matrix-terminology/)
- [ValueError: Can't handle mix of multilabel-indicator and continuous-multioutput accuracy_score()](https://stackoverflow.com/questions/41724680/valueerror-cant-handle-mix-of-multilabel-indicator-and-continuous-multioutput)
- [學習使用Keras建立卷積神經網路](https://chtseng.wordpress.com/2017/09/23/學習使用keras建立卷積神經網路/)
- 另一種轉換標籤的方法: [Get class labels from Keras functional model](https://stackoverflow.com/questions/38971293/get-class-labels-from-keras-functional-model)