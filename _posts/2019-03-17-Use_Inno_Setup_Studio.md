---
layout: post
title: 使用 Inno Setup Studio 製作獨立安裝檔的技巧
date: 2019-03-17 12:00:00 +0800
categories: InnoSetup
tags: [Inno Setup]
---

想要將寫好的程式，製作成獨立安裝檔時，可以用 Inno Setup Studio 製作。

### 使用 Inno Setup Studio 製作獨立安裝檔的技巧

可以透過 Inno Setup Studio 編寫 Inno Setup 的腳本，以製作安裝程式。

- 在開啟 Inno Setup Studio 後，使用 **Use script wizard to create script** 即可一步一步製作安裝檔。
- 透過登錄檔新增路徑至 `%PATH%` 時，Value Type 選擇 `REG_EXPAND_SZ`，並加入 `{olddata}` 至 ValueData，以便在插入新路徑時保留原有的路徑。加入 `{olddata}` 並不會判斷路徑是否有被插入過，因此可加入 `Check: NeedsAddPath('路徑')` 方法檢查是否重複。
- 要安裝執行環境時 (如 Visual C++ 2010 Redistributable x86)，可將該檔案複製到 tmp 目錄下，並勾選 `deleteafterinstall` 旗標，在安裝完成後清除。
- Visual C++ Redistributable 的不同版本有不同的 Silent Install 指令，在 2010 版本是 `/q /norestart`。

參考：

[jrsoftware.org // Jordan Russell's Software](http://www.jrsoftware.org/)

[Inno Script Studio - Kymoto Solutions](https://www.kymoto.org/products/inno-script-studio)

[iInfo 資訊交流: Inno Setup 操作(2) --- 使用 InnoIDE 建立安裝包](http://white5168.blogspot.com/2018/03/inno-setup-2-innoide.html?m=1)

[Registry Value Types - Windows applications - Microsoft Docs](https://docs.microsoft.com/en-us/windows/desktop/sysinfo/registry-value-types)

[How do I modify the PATH environment variable when running an Inno Setup Installer?](https://stackoverflow.com/questions/3304463/how-do-i-modify-the-path-environment-variable-when-running-an-inno-setup-install)