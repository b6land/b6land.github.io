---
layout: post
title: 用 Codeium 自動完成程式碼
date: 2024-02-14 12:00:00 +0800
categories:  [Programming]
--- 

Codeium 是和 GitHub Copilot 類似的產品，透過大型語言模型 (LLM)，可以幫助開發人員產生程式片段，猜測將輸入的程式碼，以及協助除錯。

### Codeium 特色

1. 個人使用完全免費。
2. 支援多種不同的整合開發環境 (IDE)，以及不同的程式語言。 
3. 主打程式碼的自動完成、對話協助產生 / 除錯程式片段等功能。
4. 不會使用非 GPL 授權的程式碼進行訓練。

### 使用方式

以下使用 Visual Studio Code (下稱 VSCode) 安裝 Codeium 擴充套件，安裝時需要登入 / 註冊一個免費的帳號。

啟用 Codeium 後，輸入程式碼時，可以看到 Codeium 出現程式碼的提示，按下 Tab 鍵就可以套用。

![按下 Tab 自動完成](/assets/imgs/2024-02-14/Codeium_AutoComplete.png)  

可以按下左方的 Codeium 圖示，輸入問題獲得程式的回答，輸入中英文皆可。若答案為英文，也可要求輸出中文的答案。

![回答依賴注入的問題](/assets/imgs/2024-02-14/Codeium_Chat.png)  

選取程式碼要求 Codeium 解釋其邏輯。

![解釋選取程式碼的邏輯](/assets/imgs/2024-02-14/Codeium_Explain.png)  

發生錯誤時，也能在錯誤的提示中要求解釋

![解釋錯誤發生的原因與處理方式](/assets/imgs/2024-02-14/Codeium_Explain_Problem.png)  

### VSCode 特定專案停用 Codeium

雖然 Codeium 遵守嚴格的隱私權政策，不會利用上傳的程式碼訓練模型，但**程式碼仍會被送至遠端伺服器**，以提供自動完成的功能。

由於目前版本的 Codeium 不能設定要排除使用的目錄，若有不想使用 Codeium 產生程式碼的專案，可以使用 VSCode 內建功能，在專案目錄停用擴充套件 (包含 Codeium)，來避免上傳程式碼。

1\. 在 VSCode 中開啟需要設定的目錄。
2\. 從左方開啟 Extensions 頁籤，然後選擇 Codeium，再按下 Disable 右方的小箭頭，並選擇 Disable (Workspace)。

![選擇 Disable 停用 Codeium 套件](/assets/imgs/2024-02-14/Codeium_Disable.png)  

3\. 以後 VSCode 開啟該目錄時，就不會啟動 Codeium (可觀察右下方未載入 Codeium 伺服器)。

### 參考資料

- Codeium 官方網站: [Codeium · Free AI Code Completion & Chat](https://codeium.com/)
- 特定專案不使用擴充套件: [VS Code disable certain extensions for a specific subdirectory of a workspace - Stack Overflow](https://stackoverflow.com/questions/44797445/vs-code-disable-certain-extensions-for-a-specific-subdirectory-of-a-workspace)
- Reddit 一則原始碼被上傳至遠端伺服器的相關討論：[I accidentally leaked my company source code : r/cscareerquestionsEU](https://www.reddit.com/r/cscareerquestionsEU/comments/1bmnz6m/i_accidentally_leaked_my_company_source_code/)，使用前請留意 Codeium, GitHub Copliot 和其它 AI 助理的資料使用策略。
