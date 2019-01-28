### 修改 PATH 環境變數

1. 使用 `setx /m path "%path%;C:\Folder1"`，不過會有字元長度不能大於 1024 的限制。

2. 直接編輯 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment` 內的 Path 變數。

> 因此可使用 cmd 取出既有的 Path 變數，修改以後再複寫原有的登錄值。如下面範例是附加一個測試目錄至 Path 變數底下：

> ```
> SET TestPath="%Path%;C:\Test"
> REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v Path /t REG_SZ /d %TestPath% /f
> ```

參考：

[Set PATH and other environment variables in Windows 10](https://www.opentechguides.com/how-to/article/windows-10/113/windows-10-set-path.html)

[Reg - Edit Registry - Windows CMD - SS64.com](https://ss64.com/nt/reg.html)

