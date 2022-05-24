---
layout: post
title: C# 的動態參考與反射
date: 2018-07-03 12:00:00 +0800
categories:  [C#, DLL]
---

這篇文章介紹在專案中使用第三方 DLL 時，常用的動態參考與反射。

### 動態加入參考

- 可以使用 `Assembly.LoadFrom("MyAssembly.dll")` 載入特定的 DLL 檔。
- `Is64BitOperatingSystem` 可以用來檢查作業環境是否為 64 bit 的作業系統。

參考 : 
[Adding references dynamically in .NET](https://stackoverflow.com/questions/19398748/adding-references-dynamically-in-net)
[Reflection c#- 反射](https://dotblogs.com.tw/kevin_date/2017/11/08/143829)

### 使用反射 (Reflection)

- 使用轉型，可以將 `Assembly.LoadFrom()` 載入的 Object 視為指定的型態。在 DaqAdapter 中，將如 ItriApplyZ2 的 DaqAdapter sub class 轉為 DaqAdapter class。
- 僅接受沒有參數的建構子，參數要以額外函式傳入 class 內。
- `Assembly.GetTypes(string name)` 使用的 name，是 namespace + class name (命名空間 + 類別名稱)。
- **和 Abstract class 結合：**不能直接建立 Abstract class 的實例，但是可以建構繼承類別的實例，再當成該 Abstract class 來使用。

參考：
[C# - 實作載入外部 DLL 外並使用 Method](https://dotblogs.com.tw/dc690216/2010/08/01/16939)
[Can we create instance to abstract class?](https://www.quora.com/Can-we-create-instance-to-abstract-class)