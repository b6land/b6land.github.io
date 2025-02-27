---
layout: post
title: 單元測試介紹
date: 2023-11-13 12:00:00 +0800
categories:  [Unit Test]
--- 

雖然之前寫過了[單元測試入門](/First_Time_Unit_Test/)，但這一篇會有更整體的介紹。

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


### 系列文章

- C# 的單元測試教學：[【Unit Test】Day 1 - 為何要寫單元測試 - 程式隨筆](https://toyo0103.github.io/2017/04/17/【Unit-Test】Day-1-為何要寫單元測試/)
- ASP.NET MVC 怎麼寫測試：[ASP.NET MVC 單元測試系列 (1)：新手上路 / 開始撰寫！ - The Will Will Web](https://blog.miniasp.com/post/2010/09/14/ASPNET-MVC-Unit-Testing-Part-01-Kick-off)
- Legacy Code 的插管治療法 (最少調整的狀況下，使既有程式具備可測試性)：[C# Test Legacy Code（1）Isolated by Inheritance and Override by joeychen - CodeData](https://www.codedata.com.tw/social-coding/csharp-legacy-code-test-1-isolated-by-inheritance-override/)


### 其他參考資料

- 觀念、如何在 Java 進行單元測試：[【單元測試】改變了我程式設計的思維方式 by pcbill - CodeData](https://www.codedata.com.tw/java/unit-test-the-way-changes-my-programming/)
- 如何寫出好的測試程式，特別可以看 Stub 和 Mock 的章節：[撰寫單元測試的最佳做法 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/core/testing/unit-testing-best-practices) 
- 測試靜態方法：[3 Amazing Ways to Unit Test Code That Calls Static Method in C#](https://methodpoet.com/unit-test-static-method/)

### C# API 單元測試問題

如果執行單元測試時，遇到「找不到 app.config」的問題時，有兩種解決方法：

1. 正規解法：建立一個 web.config 的 mock class。
2. 替代解法：將原本的 web.config 重命名為 app.config，放置於單元測試專案的執行目錄內。

- 參考資料：[c# - How to read Web.Config file while Unit Test Case Debugging? - Stack Overflow](https://stackoverflow.com/questions/23513355/how-to-read-web-config-file-while-unit-test-case-debugging)