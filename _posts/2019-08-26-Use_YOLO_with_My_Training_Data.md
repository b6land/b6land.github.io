---
layout: post
title: 使用 YOLO 訓練自定義資料
date: 2019-08-26 12:00:00 +0800
categories:  [Machine Learning]
---

本文是使用 YOLO 視覺辨識類神經網路模型，訓練自定義資料的紀錄。

## 成功的部分：使用 Darknet 與 YOLO v3

### 在 Windows 下安裝 Darknet 和使用

首先，下載和編譯 Darknet：[windows下darknet之yolo(gpu版本）安装 - 知乎](https://zhuanlan.zhihu.com/p/45845454)

並且於 VS2015 中調整編譯選項：[OpenCV3 + VS 配置 (有道雲筆記)](https://note.youdao.com/ynoteshare1/index.html?id=04fb326760a726f23cbd9ae8ff6b1fc6&type=note#/)

上述過程操作完後，便可以將 Darknet 安裝於 Windows 上並執行物件偵測。

需要自訂訓練時，則需要準備自訂的資料和下載預訓練的模型： [GitHub - AlexeyAB/darknet: Windows and Linux version of Darknet Yolo v3 & v2 Neural Networks for object detection (Tensor Cores are used)](https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects)

### Darknet 參考資料

- [使用darknet（windows GPU 版本） yolov3 训练自己的第一个检测模型（皮卡丘检测） - 简书](https://www.jianshu.com/p/98aa75b0532f)
- 關於資料內容的結構與訓練過程，可以參考：[建立自己的YOLO辨識模型 – 以柑橘辨識為例 – CH.Tseng](https://chtseng.wordpress.com/2018/09/01/%E5%BB%BA%E7%AB%8B%E8%87%AA%E5%B7%B1%E7%9A%84yolo%E8%BE%A8%E8%AD%98%E6%A8%A1%E5%9E%8B-%E4%BB%A5%E6%9F%91%E6%A9%98%E8%BE%A8%E8%AD%98%E7%82%BA%E4%BE%8B/)

### YOLO 參考資料

- [How to build a custom object detector using YOLOv3 in Python](http://emaraic.com/blog/yolov3-custom-object-detector)
- [Tommy Huang – Medium](https://medium.com/@chih.sheng.huang821)

## 失敗的部分：使用 Darkflow 與 YOLO v2

### 教學

基本上是參考這個網頁的教學操作：[[機器學習 ML NOTE]YOLO!!!如何簡單使用YOLO訓練出自己的物件偵測!!! (Windows+Anaconda)](https://medium.com/雞雞與兔兔的工程世界/3ad34a4cac70)

### 疑難排解

> ImportError: numpy.core._multiarray_umath failed to import
- 如果遇到以上的錯誤，可使用 `pip install --upgrade numpy` 指令升級 numpy，可能是舊版沒有該結構造成。參考：[ImportError: numpy.core._multiarray_umath failed to import · Issue #11871 · numpy/numpy](https://github.com/numpy/numpy/issues/11871)
 
 - 訓練儲存的權重會存在 `ckpt` 目錄底下。可以在參數內加入 `--load -1` 載入最新的權重。
參考： [Where are the new weights stored after training a new model? · Issue #256 · thtrieu/darkflow](https://github.com/thtrieu/darkflow/issues/256)

### BoundingBox 未出現原因的猜測

以下是使用 Darkflow 自行訓練不成功的原因，在其它自行訓練物件偵測類神經網路時，可供參考。

- **標籤**：使用 Pascal 格式，不會有標籤未正確輸入的問題。
- **Bounding Box 大小**：是否與 Bounding Box 的大小有關連？可能關係不大。因為只使用包含較小 Bounding Box 的資料，仍然有問題。
- **資料載入**：確認 Darkflow 有載入資料，排除此狀況。
- **訓練的 Epoch 過少**：預設為 1000 epochs，自行訓練時最高僅使用到 400 epochs，可能是原因。檢測時可以看 loss 是否有小於 0.1。
- **資料過少**：目前使用的資料僅有 60 張照片，在不同的網站教學中，都使用至少 200 張以上的照片。

### Darkflow 參考資料

- 如何為資料加上標籤：可參考 **LabelImg** 的 [Github Page](https://github.com/tzutalin/labelImg#labelimg)
- Darkflow [Github Page](https://github.com/thtrieu/darkflow)


