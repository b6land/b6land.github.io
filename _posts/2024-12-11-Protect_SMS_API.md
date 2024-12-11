---
layout: post
title: 保護簡訊 API，避免被 BOT 攻擊的作法
date: 2024-12-11 21:00:00 +0800
categories: [Other]
--- 

如果設計了發送手機簡訊的 API，我們會希望這個 API 不會被 BOT 攻擊或隨意使用，以免產生大量發送簡訊的費用，或是造成接收簡訊顧客的困擾。以下有幾種做法，可以避免或減緩 API 被攻擊所帶來的影響。

### 作法  

1. 實作兩階段驗證。
2. 使用 CAPTCHA 驗證方式 (可能會降低使用者體驗)。
3. 設定發送的頻率限制。
4. 允許或阻擋特定來源的使用者 (ex. 來自國外的 IP)。
5. 監控你的 API 使用行為。
6. 使用人工智慧和機器學習技術辨識 BOT 攻擊。
7. 限制能存取簡訊 API 金鑰的使用者。
8. 持續更新 API 主機的安全漏洞。
9. 訓練員工，避免他們濫用 API 或發生資安意外。
10. 定期的安全稽核和滲透性測試。

### 參考資料

- [Tips for Protecting Your SMS API from Bot Attacks - MXI Coders](https://www.mxicoders.com/tips-for-protecting-your-sms-api-from-bot-attacks/)