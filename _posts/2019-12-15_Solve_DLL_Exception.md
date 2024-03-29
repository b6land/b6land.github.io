---
layout: post
title: 解決 C# 專案整合第三方函式庫 DLL 錯誤
date: 2019-12-15 12:00:00 +0800
categories:  [C#, DLL]
---

### 前情提要

在開發 C# 的專案時，若要整合第三方函式庫 (Third-Party Library) 的 DLL，如果未正確設定引用的 DLL ，執行時會出現 Type Initialization Exception 錯誤。這個錯誤有可能是未找到對應的 DLL 而導致。

### 疑難排解

1. 使用 View Details 功能，檢查錯誤的詳細訊息和發生程式點，試圖找出可能導致問題發生的 DLL。
2. 檢查是不是有 DLL 檔案遺失：作為進入點的 DLL 有被正確參考，且其參考的其它有正確被放進對應的目錄 (通常與進入點 DLL 相同目錄)，這邊應該要閱讀函式庫的文件，檢查參考的設定方式。
3. 調整編譯設定：檢查專案中的編譯設定是否正確，通常需要檢查目標平台是否為 x86 或 x64， .Net Framework 的版本，以及編譯產出的目錄。
4. 檢查 Visual C++ Redistribution 版本：檢查函式庫的說明文件，並下載需要的 Visual C++ Redistribution ，並重新開機。
5. 使用正確版本的 DLL：確認第三方函式庫的版本是否為適用於現有作業平台的發行 (Release) 版本，若使用與現有平台不相容的除錯 (Debug) 或發行版本，容易因版本不相容而出現 SHException 和 File Not Found Exception。
6. 觀察 Sample 專案：若第三方函式庫有附帶 Sample 專案，應先測試專案所編譯的執行檔是否可正常運作。若可正常運作，則能證明 DLL 版本正確且無缺漏。透過觀察編譯所產生執行檔、編譯設定與編譯前置批次檔，找出所需之正確 DLL 檔案，並複製至原有專案之執行目錄底下；檢查專案原本參考的 DLL 檔案位置，將其更換為正確 DLL 檔案。
7. Listdlls 公用程式：可列出執行中程式所使用的 DLL，但未必會包含程式本身參考的 DLL。