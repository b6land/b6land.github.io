---
layout: post
title: PowerShell Send-MailMessage 寄信
date: 2026-01-25 15:30:00 +0800
categories: [Programming]
--- 

如果想要測試信件寄送的設定，手邊又沒有合適的工具，在 Windows 上可以透過 PowerShell 內建的 `Send-MailMessage`  命令，發送測試的信件。

直接來看怎麼使用！

### 語法範例

```powershell
Send-MailMessage `
-from xxx@yyy.com.tw `
-to lazy@qqq.com.tw `
-subject "this is subject" `
-body "this is mail body" `
-smtpserver in-smtp.yyy.com.tw `
-port 25 `
-credential xxx
```

註：`` ` `` 可以將命令分為很多行。

### 參數說明

常用的參數如下：

| 參數  | 說明  |
| --- | --- |
| from | 寄件人 |
| to  | 收件人，有多位時可以用逗號分隔 |
| subject<br> | 信件主旨 |
| body | 信件內文 |
| smtpserver | 要用來寄信的 SMTP 伺服器 |
| port | 指定 SMTP 伺服器的 Port，預設是 25 |
| credential<br> | 可以用來寄信的使用者帳號 |

輸入指令後，會接著詢問使用者密碼 (有可能彈出視窗詢問)。

另外還有其它參數，擷取部分說明如下：

| 參數  | 說明  |
| --- | --- |
| useSsl | 是否啟用 SSL |
| cc  | 副本收件人，有多位時可以用逗號分隔 |
| bcc | 密件副本收件人，有多位時可以用逗號分隔<br> |
| attachments <br> | 附件  |

### 參考資料

- [使用 PowerShell 的 Send-MailMessage 發送郵件 – 蛙蛙醬筆記本](https://blog.wawajohn.net/12458.html ) 
- [Send-MailMessage (Microsoft.PowerShell.Utility) - PowerShell - Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/send-mailmessage?view=powershell-7.4 )
