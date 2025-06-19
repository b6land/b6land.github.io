---
layout: post
title: C# 單元測試 (1) 介紹與參考系列文
date: 2023-11-13 12:00:00 +0800
categories:  [Unit Test]
--- 

雖然之前寫過了[C# 單元測試 (0) 概念篇](/First_Time_Unit_Test/)，但這一篇會有更整體的介紹。

### 何時寫測試

- 對於已經上線的系統來說，單元測試是重要但不那麼急迫的事，線上的使用者已經幫你測試過邏輯。
- 在修正 Bug 時，或需要修改程式時，再加入 Unit Test，會是比較有效率的方式。
- 只關注真正需要測試的部分。 (怎麼分辨哪些需要測試，可能需要更多的經驗)
- 盡量簡化邏輯，簡單到不需要測試。

參考資料：[蜀道難，難於寫測試？ — 關於測試的三點提示 - Kuma老師的軟體工程教室 - Medium](https://medium.com/kuma老師的軟體工程教室/蜀道難-難於寫測試-e42977843a70)


### 測試的類型有哪些

- 單元測試：針對單一方法進行的測試，保護程式不會在系統維護的過程中被破壞。如果程式的架構寫得漂亮，盡可能符合 SOLID 原則，那麼單元測試會比較容易撰寫。
- 整合測試：整合多種資源進行測試，確保模組與模組之間能正常溝通與運作。
- 端對端測試：從使用者 (客戶端) 的角度，對真實系統 (系統端) 進行測試。

參考資料：[一次搞懂單元測試、整合測試、端對端測試之間的差異 - The Will Will Web](https://blog.miniasp.com/post/2019/02/18/Unit-testing-Integration-testing-e2e-testing)


### 可測試性：SOCK 的觀念

- S: Simplicity，受測程式越簡單越好。
- O: Observability，受測程式可以透過外部觀察狀態。
- C: Control，測試程式對受測程式控制的能力，例如可以注入模擬物件。
- K: Knowledge of the expected result，知道受測程式應該要取得什麼結果，才寫得出需驗證的答案。


參考資料：
1. [程式碼的可測性 - iThome](https://www.ithome.com.tw/voice/88062 "https://www.ithome.com.tw/voice/88062")  
2. [搞笑談軟工: 如何提升軟體的可測試性（一）](https://teddy-chen-tw.blogspot.com/2013/04/blog-post_4.html)

### 測試的顆粒度

假如受測試的 Public 方法很大、有呼叫很多內部模組或方法，要怎麼測試呢？

有幾種方法：

1. 拆分成多個單一職責的模組，針對模組建立單元測試。
2. 對較細部的 Private 方法建立單元測試，需要設定 InternalVisibleTo 屬性。
3. 只對 Public 方法測試行為，因為裡面的邏輯外部使用者根本不介意。

切分單元測試和受測方法，可以盡量符合單一職責原則，但是會遇到問題：切太細會顯得多餘，切太粗則職責不單純。需要自行拿捏或與專案成員討論。

參考資料：

1. [Day 26 - 測試的基礎：Unit Test - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10338217 ) 
2. [是否該針對非 public method 進行單元測試？ - 最好的 TDD 學習資源](https://tdd.best/blog/do-not-invoke-private-function-in-test/)  
3. [如何提高程式碼的可測試性 (Testability)](https://kaisheng714.github.io/articles/testability)

### 系列文章

- C# 的單元測試教學：[【Unit Test】Day 1 - 為何要寫單元測試 - 程式隨筆](https://toyo0103.github.io/2017/04/17/【Unit-Test】Day-1-為何要寫單元測試/)
- ASP.NET MVC 怎麼寫測試：[ASP.NET MVC 單元測試系列 (1)：新手上路 / 開始撰寫！ - The Will Will Web](https://blog.miniasp.com/post/2010/09/14/ASPNET-MVC-Unit-Testing-Part-01-Kick-off)
- Legacy Code 的插管治療法 (最少調整的狀況下，使既有程式具備可測試性)：[C# Test Legacy Code（1）Isolated by Inheritance and Override by joeychen - CodeData](https://www.codedata.com.tw/social-coding/csharp-legacy-code-test-1-isolated-by-inheritance-override/)
- 前端寫測試：[單元測試的基本概念 - 單元測試的藝術 第 3 版 - 閱讀筆記 - Summer。桑莫。夏天](https://www.cythilya.tw/2024/03/28/the-basics-of-unit-testing/)
- [單元測試 - 1 - Sian](https://sunnyday0932.github.io/2021/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6-1/)


### 其他參考資料

- 觀念、如何在 Java 進行單元測試：[【單元測試】改變了我程式設計的思維方式 by pcbill - CodeData](https://www.codedata.com.tw/java/unit-test-the-way-changes-my-programming/)
- 如何寫出好的測試程式，特別可以看 Stub 和 Mock 的章節：[撰寫單元測試的最佳做法 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/core/testing/unit-testing-best-practices) 
- 測試靜態方法：[3 Amazing Ways to Unit Test Code That Calls Static Method in C#](https://methodpoet.com/unit-test-static-method/)

### C# API 單元測試問題

如果執行單元測試時，遇到「找不到 app.config」的問題時，有兩種解決方法：

1. 正規解法：建立一個 web.config 的 mock class。
2. 替代解法：將原本的 web.config 重命名為 app.config，放置於單元測試專案的執行目錄內。

- 參考資料：[c# - How to read Web.Config file while Unit Test Case Debugging? - Stack Overflow](https://stackoverflow.com/questions/23513355/how-to-read-web-config-file-while-unit-test-case-debugging)