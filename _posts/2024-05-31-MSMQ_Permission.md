---
layout: post
title: Microsoft Message Queue (MSMQ) 私有佇列的的權限設定
date: 2024-05-31 21:00:00 +0800
categories:  [Messaging]
---

這篇文章會跟你說怎麼設定 MSMQ 的私有佇列 (Private Queue) 的權限設定 (安全性)。

### 設定權限的方式

1. 進入「電腦管理」。
2. 選擇「服務與應用程式」 -> 「訊息佇列」 -> 「私用佇列」。
3. 在想設定的佇列上按右鍵，選擇「內容」。
4. 切到「安全性標籤」。
5. 選擇要設定的角色，調整權限，按下「確定」。

![設定安全性](/assets/imgs/2024-05-31/MSMQ_Setup_Permission.png)  
△ 設定安全性的視窗

### 無法查看和調整權限時怎麼辦

如果發生無法查看私有佇列 (Private Queue) 的權限問題，而且無法調整權限，可以照以下方法調整權限：

1. 停止 MSMQ 服務，從「電腦管理」>「服務」內找到「Message Queuing」(或中文的「訊息佇列」)，並按下停止。
2. 開啟 `C:\\WINDOWS\\system32\\msmq\\storage\\lqs` 資料夾。
3. 在這個資料夾，可以找到要處理的私有佇列，使用 Notepad 等文字編輯器開啟 lqs 檔案。
4. 使用 Notepad 等文字編輯器開啟其它 lqs 檔案，找到其它權限設定正確的私有佇列。如果沒有的話，就建立一個新的私有佇列。
5. 找到 `Security=...` 開始的文字列。
6. 將這一整行複製起來 (非常長，確保都有複製到)
7. 開啟剛剛找到要處理的私有佇列 lqs 檔案。
8. 貼上剛剛複製的文字列，覆寫 `Security=...` 這一列。
9. 儲存 lqs 檔案。
10. 啟動 MSMQ 服務。

![文字編輯器選取畫面](/assets/imgs/2024-05-31/MSMQ_Copy_Security.png)  
△ 用文字編輯器複製 lsq 檔案中的 `Security` 文字。
  
![服務視窗](/assets/imgs/2024-05-31/MSMQ_Service.png)  
△ 在服務中停止和啟動 MSMQ。

### 參考資料

- [wayne程式筆記(學習從模仿開始): C# MSMQ 基本設定](http://wayneprogramcity.blogspot.com/2014/04/c-msmq.html "http://wayneprogramcity.blogspot.com/2014/04/c-msmq.html")  
- [vb6 - No permission to access a private MSMQ - Stack Overflow](https://stackoverflow.com/questions/781154/no-permission-to-access-a-private-msmq "https://stackoverflow.com/questions/781154/no-permission-to-access-a-private-msmq")