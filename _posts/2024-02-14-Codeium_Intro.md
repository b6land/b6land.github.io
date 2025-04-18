---
layout: post
title: 用 Windsurf (Codeium) 自動完成程式碼 (含下載與安裝於 VSCode)
date: 2024-02-14 12:00:00 +0800
categories:  [Programming]
--- 

Windsurf (以前稱為 Codeium) 是和 GitHub Copilot 類似的產品，透過大型語言模型 (LLM)，可以幫助開發人員產生程式片段，猜測將輸入的程式碼，以及協助除錯。以下將介紹重要功能、下載與安裝方式 (以 VSCode 為例)。

### 目錄

- [Codeium 特色](#codeium-特色)
- [Codeium 重要功能](#codeium-重要功能)
- [進階：設定 Context](#進階設定-context)
- [VSCode 特定專案停用 Codeium](#vscode-特定專案停用-codeium)
- [Codeium 下載與安裝 (以 VSCode 為例)](#codeium-下載與安裝-以-vscode-為例)
- [參考資料](#參考資料)

### Codeium 特色

1. 個人使用完全免費 (需要登入 / 註冊一個免費的帳號)。
2. 支援多種不同的整合開發環境 (IDE)，以及不同的程式語言。 
3. 主打程式碼的自動完成、對話協助產生 / 除錯程式片段等功能。
4. 不會使用非 GPL 授權的程式碼進行訓練。

### Codeium 重要功能

以下使用 Visual Studio Code (下稱 VSCode) 示範重要功能。

輸入程式碼時，可以看到 Codeium 出現程式碼的提示，按下 Tab 鍵就可以套用。

![按下 Tab 自動完成](/assets/imgs/2024-02-14/Codeium_AutoComplete.png){:height="121px" width="640px"}  

可以按下左方的 Codeium 圖示，輸入問題獲得程式的回答，輸入中英文皆可。若答案為英文，也可要求輸出中文的答案。此外，可以用 @ 符號指定特定的類別或函數。

![回答依賴注入的問題](/assets/imgs/2024-02-14/Codeium_Chat.png){:height="312px" width="720px"}  

選取程式碼要求 Codeium 解釋其邏輯。

![解釋選取程式碼的邏輯](/assets/imgs/2024-02-14/Codeium_Explain.png){:height="342px" width="720px"}  

發生錯誤時，也能在錯誤的提示中要求解釋。

![解釋錯誤發生的原因與處理方式](/assets/imgs/2024-02-14/Codeium_Explain_Problem.png){:height="360px" width="720px"}  

在程式碼的任一處按下 Ctrl + Shift + I 組合鍵，會出現 Codeium Command 方塊，可以在裡面輸入指令。

![Codeium Command 方塊](/assets/imgs/2024-02-14/Codeium_Command.png){:height="261px" width="720px"}

如果 Codeium 有變更內容，可以選擇要接受或拒絕變更。

![Diff 內容](/assets/imgs/2024-02-14/Codeium_Diff.png){:height="286px" width="720px"}

### 進階：設定 Context

可以在 Codeium 的窗格中，在下方找到 Context 區域。在這裡可以設定：

1. 設定產生提示的來源：可以是本機端的相關資料夾、已開啟的檔案等等。
1. 按下 Advance 可以設定自訂的提示字。輸入後，執行的自動完成 / 對談皆會套用自訂提示。例如：輸入「use C# 6.0 syntax」指定使用特定版本的語法。*(經測試，可能會有沒套用提示效果的情形。)*
3. 在按下 Advance 後，可以在 Local Indexes 看到本機引用的來源。

![自訂提示與提示來源](/assets/imgs/2024-02-14/Codeium_Context_202412.png){:height="285px" width="452px"}
△目前版本畫面 (2024/12)

![自訂提示與提示來源 (2024-02)](/assets/imgs/2024-02-14/Codeium_Context.png){:height="598px" width="478px"}
△舊版畫面，Context 位於上方分頁

### VSCode 特定專案停用 Codeium

雖然 Codeium 遵守嚴格的隱私權政策，不會利用上傳的程式碼訓練模型，但**程式碼仍會被送至遠端伺服器**，以提供自動完成的功能。

由於目前版本的 Codeium 不能設定要排除使用的目錄，若有不想使用 Codeium 產生程式碼的專案，可以使用 VSCode 內建功能，在專案目錄停用擴充套件 (包含 Codeium)，來避免上傳程式碼。

1\. 在 VSCode 中開啟需要設定的目錄。

2\. 從左方開啟 Extensions 頁籤，然後選擇 Codeium，再按下 Disable 右方的小箭頭，並選擇 Disable (Workspace)。

![選擇 Disable 停用 Codeium 套件](/assets/imgs/2024-02-14/Codeium_Disable.png){:height="226px" width="720px"}  

3\. 以後 VSCode 開啟該目錄時，就不會啟動 Codeium (可觀察右下方未載入 Codeium 伺服器)。

### Codeium 下載與安裝 (以 VSCode 為例)

1\. 進入[官網](https://codeium.com/)，按下右邊的下載。
![按下右邊的下載](/assets/imgs/2024-02-14/Download.png){:height="67px" width="720px"}

2\. 選擇右方要安裝 Codeium 的 IDE，這裡選 Visual Sutdio Code。
![右方選 VSCode](/assets/imgs/2024-02-14/Download_2.png){:height="332px" width="720px"}

3\. 註冊帳號或登入。
![註冊帳號或登入](/assets/imgs/2024-02-14/Download_3.png){:height="581px" width="663px"}

4\. 按下 Quick Install 按鈕，如果瀏覽器有詢問是否同意開啟連結，選擇同意。此頁面下方也有手動安裝的說明。
![按下 Quick Install 按鈕](/assets/imgs/2024-02-14/Download_4.png){:height="461px" width="720px"}

5\. 進入 VSCode 的擴充頁面頁面，按下 Install 按鈕。
![按下 Install 按鈕](/assets/imgs/2024-02-14/Download_5.png){:height="296px" width="720px"}

6\. 詢問是否允許認證，按下 Allow。
![允許認證](/assets/imgs/2024-02-14/Download_6.png){:height="166px" width="697px"}

7\. 按下 Open 開啟 Codeium 網站。
![按下 Open](/assets/imgs/2024-02-14/Download_7.png){:height="182px" width="577px"}

8\. 因為剛剛已經登入帳號，接著按下瀏覽器的開啟網站 (連結)。
![按下開啟網站](/assets/imgs/2024-02-14/Download_8.png){:height="193px" width="720px"}

9\. 最後再按下 Open，允許 Codeium 開啟此連結，就完成安裝。
![按下 Open](/assets/imgs/2024-02-14/Download_9.png){:height="226px" width="538px"}

### 參考資料

- Codeium 官方網站: [Codeium · Free AI Code Completion & Chat](https://codeium.com/)
- 特定專案不使用擴充套件: [VS Code disable certain extensions for a specific subdirectory of a workspace - Stack Overflow](https://stackoverflow.com/questions/44797445/vs-code-disable-certain-extensions-for-a-specific-subdirectory-of-a-workspace)
- Reddit 一則原始碼被上傳至遠端伺服器的相關討論：[I accidentally leaked my company source code : r/cscareerquestionsEU](https://www.reddit.com/r/cscareerquestionsEU/comments/1bmnz6m/i_accidentally_leaked_my_company_source_code/)，使用前請留意 Codeium, GitHub Copliot 和其它 AI 助理的資料使用策略。
