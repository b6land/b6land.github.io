---
layout: post
title: 從 Continuous-Multioutput Label 建立 Confusion Matrix
date: 2018-07-03 12:00:00 +0800
categories: DeepLearning
tags: [Deep Learning]
---

這篇文章介紹 Keras 的 NN 模型，如何從連續的輸出值，轉為類別數字。

### 轉換標籤類型以產生 Confusion matrix

Keras 產生的類神經網路架構，以 `Dense(2, activation='softmax')` 輸出計算結果的話，其輸出標籤的型態將會是 continuous-multioutput，即連續的輸出值。然而若與原本標籤型態不符合，則無法以 scikit-learn 內建的 `confusion_matrix()` 函式計算 confusion matrix。

使用以下的程式碼，可以將預測結果以類別數字的型式 class-vector (integer) 輸出，並輸出 confusion matrix。輸出型式和 binary class vector (multiclass-indicator, one-hot-encoding) 不相同。

``` py
from sklearn.metrics import confusion_matrix
prediction = model.predict_classes(X_test)
confusion_matrix(y_true, y_pred)
```

### 參考

[Simple guide to confusion matrix terminology](http://www.dataschool.io/simple-guide-to-confusion-matrix-terminology/)

[ValueError: Can't handle mix of multilabel-indicator and continuous-multioutput accuracy_score()](https://stackoverflow.com/questions/41724680/valueerror-cant-handle-mix-of-multilabel-indicator-and-continuous-multioutput)

[學習使用Keras建立卷積神經網路](https://chtseng.wordpress.com/2017/09/23/%e5%ad%b8%e7%bf%92%e4%bd%bf%e7%94%a8keras%e5%bb%ba%e7%ab%8b%e5%8d%b7%e7%a9%8d%e7%a5%9e%e7%b6%93%e7%b6%b2%e8%b7%af/)

### 另一種轉換標籤的方法

[Get class labels from Keras functional model](https://stackoverflow.com/questions/38971293/get-class-labels-from-keras-functional-model)