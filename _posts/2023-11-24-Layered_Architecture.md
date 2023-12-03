---
layout: post
title: 分層模式簡介
date: 2023-11-24 12:00:00 +0800
categories:  [Programming, Interview]
--- 

分層模式表示軟體結構分成多層，不過沒有規定一定要分成幾層，以及要如何分層。以下將簡單的介紹分層模式。

### 分層模式

常見的分層模式可分為：

1. 展示層 (Presentation Layer): 使用者介面、檢視。 (可能也包含路由的控制)
2. 業務層 (Business Layer): 負責將從持久層取得資料，執行商業邏輯，將其輸出至展示層。
3. 持久層 (Persistence Layer): 存取資料庫或檔案的永久性的儲存媒介，進行更新、刪除、讀取、建立。
4. 資料存取層 (Data Access Layer): 資料庫、第三方 API 等外部資料來源，通常獨立於應用程式。

**優點：**

1. 便於單獨對各層的元件測試。
2. 易於理解與開發，資料流主要由上往下，便於提出需求與修改。
3. 各元件容易分隔出來修改。

**缺點：**

1. 如果每一層都只有處理很少的邏輯，只是單純讓資料從儲存媒介流至使用者介面，使用分層模式只會增加複雜度 (常見於小型的專案)。
2. 當專案越來越大時，會成為一個巨大的單體應用 (Monolithic application)，各層之間仍然有一定的耦合度。


### 參考資料

- [軟體架構：分層架構模式 ( Layered Architecture ) - 程式愛好者 - Medium](https://medium.com/程式愛好者/軟體架構-分層架構模式-layered-architecture-a959da09d1c6)
- [軟體架構-關於分層 - 老E隨手寫 - 點部落](https://dotblogs.com.tw/bda605/2021/01/19/103355)
- 另一種分層的方式: [[Day 16] 剖析分層架構 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10291824)