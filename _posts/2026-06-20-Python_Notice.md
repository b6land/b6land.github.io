---
layout: post
title: 在 Windows 下排程執行 Python Script 發送錯誤信件 (使用 outlook)
date: 2026-06-20 17:00:00 +0800
categories: [Python]
--- 

本文會撰寫一個 Python Script 發送 API 請求，如果發送失敗或伺服器回覆錯誤的話，就透過 outlook 發送信件。此外透過 Windows 內建的工作排程器設定定時執行。

### Python Script 寫法

請參考以下 Script 的註解。

```python
import http.client
import ssl
import win32com.client as win32

# 透過 outlook 發送信件
def SendErrorNotice(error_message):
    outlook = win32.Dispatch('Outlook.Application')
    mail = outlook.CreateItem(0)
    mail.Subject = '你的信件標題'
    mail.Body = error_message
    mail.To = 'aaa@xxx.com' # 要寄給誰
    mail.Send()

# 設定連線請求
context = ssl._create_unverified_context() # 建立忽略 SSL 憑證驗證的 context (若有需要)
try: 
    conn = http.client.HTTPSConnection("xxx.com.tw:443", context=context)
    payload = "{}" # 請求的 Body
    headers = { # 要加入的 Header
        'Content-Type': "application/json",
        'Authorization': "xxxx"
    }

    # 連線並檢查回傳結果
    conn.request("GET", "/query/test", payload.encode("utf-8"), headers)
    res = conn.getresponse()

    if res.status != 200:
        error_msg = f'異常\n狀態碼：{res.status} {res.reason}'
        SendErrorNotice(error_msg)
    else:
        data = res.read()
        # print(data.decode("utf-8")) # 可以檢查結果

except Exception as e: # 連線失敗
    error_msg = f'連線失敗\n錯誤訊息：{str(e)}'
    SendErrorNotice(error_msg)

```

### 工作排程器設定


![開始功能表找工作排程器](/assets/imgs/2026-06-20/1.png)

從開始功能表搜尋「工作排程器」並執行。

![編輯觸發條件](/assets/imgs/2026-06-20/2.png)

設定每天重複執行，每隔 30 分鐘一次。

![編輯動作](/assets/imgs/2026-06-20/3.png)

在程式處貼上 pythonw.exe 的路徑，引數貼上 python script 的路徑。

python.exe 和 pythonw.exe 檔都會執行 Python 程式碼，差異是 pythonw.exe 不會顯示終端機，不會電腦用到一半突然彈出視窗干擾。

### 參考資料

- [使用Python自動發送Outlook郵件的教學](https://vocus.cc/article/65f80013fd89780001ac2e43)
- [IDLE Pythonw vs Python 執行檔 : r/learnpython](https://www.reddit.com/r/learnpython/comments/1c7cblv/idle_pythonw_vs_python_executable/?tl=zh-hant)
- ChatGPT

### 附註：PowerShell

可以參考以下網址，使用 Windows 排程執行 PowerShell Script。

- [c# - Could a windows scheduled task connect to a rest endpoint? - Stack Overflow](https://stackoverflow.com/questions/25435288/could-a-windows-scheduled-task-connect-to-a-rest-endpoint)
- [How to Schedule PowerShell Script Using Task Scheduler](https://o365reports.com/2019/08/02/schedule-powershell-script-task-scheduler/)