---
layout: post
title: C# 的 Task.Delay 和 Thread.Sleep 有什麼不同
date: 2022-12-22 12:00:00 +0800
categories: [C#]
---

想要在執行緒內不斷的定時執行程式時，通常會想到要在迴圈內用 `Thread.Sleep()` 來暫停程式，其實還有 `Task.Delay()` 可以使用，且各自有不同的特性。  

### Task.Delay 和 Thread.Sleep 介紹

**Thread.Sleep(int milliseconds)** 會使目前的執行緒進入睡眠的狀態。

若在 UI 執行緒下使用 `Thread.Sleep()`，在睡眠的期間會導致 UI 執行緒沒有任何回應。

**Task.Delay(int milliseconds)** 會在目前的執行緒下建立一個新的工作 (Task)，計算延遲時間。

這個方法支援以非同步的方式運作，當使用 `await Task.Delay()` 語法時，會延長目前的工作；當不使用 await 時，將會建立一個新的 Task 並計算延遲時間，不會延長目前工作。

它使用的是 timer 來控制延遲時間，因此在使用 `await Task.Delay(milliseconds, tokenSource.Token)` 時，可以隨時透過發送停止信號 (token) 結束延遲工作。

**Task.Delay().Wait()** 會在目前執行緒建立一個新的工作 (Task)，並等待該工作結束。

### 參考資料

- [Visual C#: Thread.Sleep vs. Task.Delay](https://social.technet.microsoft.com/wiki/contents/articles/21177.visual-c-thread-sleep-vs-task-delay.aspx)

- [Thread.Sleep與Task.Delay是完全不一樣的東西，請勿亂用](http://slashlook.com/archive2016/20160201.html)

- [await Task.Delay() vs. Task.Delay().Wait()](https://stackoverflow.com/questions/26798845/await-task-delay-vs-task-delay-wait)