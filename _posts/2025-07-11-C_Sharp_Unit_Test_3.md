---
layout: post
title: C# 單元測試 (3) 程式碼涵蓋度
date: 2025-07-11 23:00:00 +0800
categories: [C#, Unit Test]
--- 

用 Visual Studio 2022 (下稱 VS 2022) 的話，有幾種方法可以產生程式碼涵蓋度的報告，檢查單元測試的涵蓋範圍和比例。一是從延伸模組安裝 Fine Code Coverage (下稱 FCC，[Github 官方說明](https://github.com/FortuneN/FineCodeCoverage ))，在 Visual Studio 內顯示報告。FCC 支援 MsTest, NUnit and xUnit 等測試框架；二是使用 NuGet 安裝 `dotnet-coverage` 和 `report-generator` 產生網頁報告。

### 安裝和使用 FCC

在 VS 2022 內，開啟「管理延伸模組」的視窗，輸入 Fine Code Coverage 安裝，安裝後須重新啟動 VS 2022。

除了從延伸模組安裝，也能從 Visual Studio Marketplace 下載檔案安裝 (VS 2022 和 VS 2019 須分別下載)：[Fine Code Coverage - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=FortuneNgwenya.FineCodeCoverage2022)

安裝前建議將 VS 2022 更新到最新版本，避免在延伸模組內找不到 FCC，或安裝失敗的狀況。可參考：[I cannot install Fine Code Coverage on Visual Studio 2022 Professional · Issue #513 · FortuneN/FineCodeCoverage](https://github.com/FortuneN/FineCodeCoverage/issues/513)

使用方式如下：

1. 開啟檢視>其它視窗> Fine Code Coverage (FCC)。
2. 在 Visual Studio 內先執行一次測試。
3. 接著 FCC 的視窗就會顯示程式碼覆蓋率。

可參考 [單元測試 - 10工具介紹(查看涵蓋率、複雜度) - Sian](https://sunnyday0932.github.io/2021/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6-10%E5%B7%A5%E5%85%B7%E4%BB%8B%E7%B4%B9%E6%9F%A5%E7%9C%8B%E6%B6%B5%E8%93%8B%E7%8E%87%E8%A4%87%E9%9B%9C%E5%BA%A6/)、[為什麼程式需要單元測試？ - 視覺化篇：叡揚部落格](https://www.gss.com.tw/blog/why-program-need-unit-test-intro-visualization)。

### 安裝和使用 dotnet-coverage 和 report-generator

開啟終端機，並輸入以下指令：

```plaintext
dotnet tool install -g dotnet-coverage
dotnet tool install -g dotnet-reportgenerator-globaltool
```

將這兩個套件安裝在全域環境。接著產生 XML 格式的文字報告：

```plaintext
dotnet-coverage collect -f xml -o coverage.xml dotnet test <solution/project>
```
然後可以產生網頁報告，assemblyfilters 一定要有 `+`  號或 `-`  號，以篩選涵蓋度報告的範圍：

```plaintext
reportgenerator -reports:coverage.xml -targetdir:.\report -assemblyfilters:+Service.dll
```

接著開啟 `report\index.html` 就能檢視報告。

(以上說明翻譯自 StackOverflow 的回答：[How to get Code Coverage from Unit Tests in Visual Studio 2022 Community Edition? - Stack Overflow](https://stackoverflow.com/questions/70321465/how-to-get-code-coverage-from-unit-tests-in-visual-studio-2022-community-edition )，並部分改寫)