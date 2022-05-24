---
layout: post
title: Hyper Parameter Search
date: 2019-04-20 12:00:00 +0800
categories:  [Deep Learning]
---

本文介紹 Hyper Parameter Search 的類型與使用方式，以增加類神經網路模型的預測準確率。

## Hyper Parameter Search

Hyperparameter 最佳化的三個方式 (參考資料 1)：

1. 網格搜尋：列出所有的參數排列組合，然後逐一去找出最佳參數。最慢。
2. 隨機搜尋：隨機挑選參數組合，缺點是無法依據先前嘗試的結果改善參數組合。
3. 貝氏機率搜尋：透過貝氏機率公式找出最佳參數。尋找的過程會使用高斯過程計算新的參數組合 (參考資料 2, 3, 4)。

### 參考資料

1. [Overview of Hyperparameter Tuning](https://cloud.google.com/ml-engine/docs/tensorflow/hyperparameter-tuning-overview)
2. [Hyperparameter tuning in Cloud Machine Learning Engine using Bayesian Optimization](https://cloud.google.com/blog/products/gcp/hyperparameter-tuning-cloud-machine-learning-engine-using-bayesian-optimization)
3. [如何通俗易懂地介绍 Gaussian Process？](https://www.zhihu.com/question/46631426)
4. [贝叶斯优化: 一种更好的超参数调优方式](https://zhuanlan.zhihu.com/p/29779000)
5. 維基百科 : [Hyperparameter optimization](https://en.wikipedia.org/wiki/Hyperparameter_optimization)

### 可使用的 Library

1. [Hyperparameter Optimization for Keras Models ](https://github.com/autonomio/talos)
2. [EpistasisLab/tpot: A Python Automated Machine Learning tool that optimizes machine learning pipelines using genetic programming.](https://github.com/EpistasisLab/tpot)

## 利用 GridSearchCV 搜尋類神經網路的 Hyper Parameter

**GridSearchCV** 是 SciKit-Learn 所提供的網格搜尋函式。

由於個人偏好以 Keras 建立類神經網路模型，因此在搜尋參數前，需要重寫原本的 Keras Training Code：先利用 SciKit-Learn 包裝 Keras Model，再呼叫 GridSearchCV。

使用上要注意的地方 : 

- 在不能平行處理的機器上，應該要修改參數為 `n_jobs=1`
- 將資料放入 `grid.fit(X, Y)` ，會自己分隔出 Training Set 和 Testing Set

### 參考資料
[How to Grid Search Hyperparameters for Deep Learning Models in Python With Keras](https://machinelearningmastery.com/grid-search-hyperparameters-deep-learning-models-python-keras/)