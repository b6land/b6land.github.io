---
layout: post
title: CefSharp 內嵌瀏覽器的介紹和幾個重要改版
date: 2024-12-03 22:00:00 +0800
categories: [C#]
--- 

CefSharp 可以在 WinForm / WPF 內，嵌入 Chromium 瀏覽器，顯示需要的網頁。

### CEF 與其架構

CEF 是 Chromium Embedded Framework 的縮寫，可用於嵌入 Chromium 瀏覽器至其它應用程式。原生 CEF 僅支援 C/C++，而 CefSharp 則是外部開發者提供的 C# 版本 Framework。

CEF 使用多重程序 (Process)，主要應用程序稱為「瀏覽器」，子程序會被建立，用於 Renderers、Plugins, GPU 等。

在 Windows 和 Linux 中，主程序和子程序使用相同的執行檔，在 OS X 中需要為子程序建立分離的執行檔和 App Bundle。

CEF 的絕大部分程序是多執行緒。CEF 提供函式與介面，在這些不同的執行緒中發布工作 (posting tasks)。

一些 Callback 和函式可能只用在特定程序或執行緒，在使用新的 Callback 或函式前，建議先閱讀 API 標頭的原始碼註解。

(此段落大致上翻譯自 CEF 官方 Wiki 與 Readme ：[chromiumembedded / cef / wiki / Tutorial — Bitbucket](https://bitbucket.org/chromiumembedded/cef/wiki/Tutorial))

### CefSharp 的教學

請直接參閱官方網站和軟體主廚的介紹：

- 官方 GitHub：[GitHub - cefsharp/CefSharp: .NET (WPF and Windows Forms) bindings for the Chromium Embedded Framework](https://github.com/cefsharp/CefSharp/)
- 網誌介紹：[\[食譜好菜\] CefSharp 把 Google Chrome 的核心 Chromium 變成了 WinForms/WPF 的控制項 - 軟體主廚的程式料理廚房 - 點部落](https://dotblogs.com.tw/supershowwei/2022/06/06/cefsharp-turns-google-chrome_s-core-chromium-into-a-winforms-wpf-control)
- CefSharp 和 C# 應用程式間溝通：[\[食譜好菜\] CefSharp 在 JavaScript 與 .NET 之間互相呼叫方法並且傳遞參數及回傳值 - 軟體主廚的程式料理廚房 - 點部落](https://www.dotblogs.com.tw/supershowwei/2022/06/13/call-methods-and-pass-parameters-and-return-values-between-javascript-and-dot-net-in-cefsharp) 
- 如何除錯：[\[小菜一碟\] 介紹兩種在 CefSharp 開啟偵錯工具（DevTools）的方式 - 軟體主廚的程式料理廚房 - 點部落](https://www.dotblogs.com.tw/supershowwei/2022/08/22/two-ways-to-open-devtools-in-cefsharp)

### 升級 CefSharp

基本上 CefSharp 的更新，會跟隨著 Chromium 的腳步。

由於較新的版本需要搭配較新的開發環境，因此可在官方 GitHub 查詢執行 CefSharp 需要的 .Net 版本和 VC++ 執行環境 (Visual C++ Redistribution) 版本。

各版本的 Release 說明可以參考：[Releases · cefsharp/CefSharp](https://github.com/cefsharp/CefSharp/releases)。

以下列出幾個 (個人認為) 重要的版本議題，若有從舊版本升級 CefSharp 的需求，可以參考看看：

| 版本  | 說明  | 相關連結 |
| --- | --- | --- |
| 至 v109<br> | 最後一個支援 Windows 7  / 8 / 8.1 的版本。需要使用 VC++ 2019 執行環境<br> | [Release v109.1.110 · cefsharp/CefSharp · GitHub](https://github.com/cefsharp/CefSharp/releases/tag/v109.1.110)<br> |
| v108 開始<br> | 可能會有複寫應用程式 DPI 的問題<br> | [winforms - Cefsharp UI issue after 108.4.130 version update in Windows application - Stack Overflow](https://stackoverflow.com/questions/75029258/cefsharp-ui-issue-after-108-4-130-version-update-in-windows-application)<br> |
| v92 開始<br> | 需要使用 `CefSharpSettings.LegacyJavascriptBindingEnabled = true;` <br> | [Remove CefSharpSettings.LegacyJavascriptBindingEnabled · Issue #3531 · cefsharp/CefSharp · GitHub](https://github.com/cefsharp/CefSharp/issues/3531)<br> |
| v83 開始<br> | `RegisterAsyncJsObject`  方法不再被使用<br> | [Remove CefSharp.WebBrowserExtensions.RegisterJsObject/RegisterAsyncJsObject · Issue #2990 · cefsharp/CefSharp · GitHub](https://github.com/cefsharp/CefSharp/issues/2990)<br> |
| v73 開始<br> | 不再使用 [IsBrowserInitializedChangedEventArgs Class](https://cefsharp.github.io/api/73.1.x/html/T_CefSharp_IsBrowserInitializedChangedEventArgs.htm) 類別<br> | <br> |
| v63 開始<br> | 可以使用新的 JavaScript Binding<br> | [Javascript Binding v2 · Issue #2246 · cefsharp/CefSharp · GitHub](https://github.com/cefsharp/CefSharp/issues/2246)<br> |

### 應用程式內嵌入網頁的其它方法

1. 使用 .NET 內建的 WebBrowser 元件。(可參考 [\[C#\]\[JavaScript\]WinForm與WebPage的JavaScript互通(一) - Level Up - 點部落](https://dotblogs.com.tw/larrynung/2012/03/12/70677))
2. 使用 Electron: [Build cross-platform desktop apps with JavaScript, HTML, and CSS - Electron](https://www.electronjs.org/)