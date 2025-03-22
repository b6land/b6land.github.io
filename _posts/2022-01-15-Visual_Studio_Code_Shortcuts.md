---
layout: post
title: Visual Studio Code 用快捷鍵、搜尋與合併多行增進開發效率
date: 2022-01-15 12:00:00 +0800
categories:  [IDE]
--- 

本篇介紹我覺得 Visual Studio Code (VSCode) 中，便於開發的快捷鍵、搜尋與取代功能、把多行文字合併成一行。

### 快捷鍵介紹

| 快捷鍵 | 功能 |
| --- | --- |
| Ctrl + F | 搜尋功能 |
| Ctrl + H | 取代功能 |
| Ctrl + Shift + F | 檔案間的搜尋功能 |
| Ctrl + Shift + H | 檔案間的取代功能 |
| Ctrl + P | 開啟目錄或工作區內的程式檔案 |
| Ctrl + Shift + P 或 F1 | 執行 VSCode 的功能 |
| Ctrl + K -> Ctrl + F | 格式化選擇的範圍 |
| Ctrl + K -> Ctrl + C | 選擇的範圍轉為註解 |
| Ctrl + K -> Ctrl + U | 選擇的範圍從註解轉為程式 |

### 搜尋與取代功能

VSCode 具有彈性又親合的搜尋功能，在檔案內的搜尋方塊，可以用左方的箭頭切換搜尋 / 取代模式。

搜尋框右方分別是符合大小寫、符合整個搜尋詞、使用正規表達式。

![檔案內搜尋、取代方塊](/assets/imgs/2022-01-15/replace.png){:height="102px" width="636px"}

可以從左方面板叫出檔案間的搜尋功能，同樣可以在左上方箭頭切換搜尋 / 取代模式，搜尋框也能使用符合大小寫、整個搜尋詞、正規表達式等功能。

搜尋框下方可以設定要搜尋的檔案，該欄位右方的圖示可以指定僅搜尋開啟的編輯器；另外也能排除搜尋的檔案，還能進一步排除 .gitignore 內忽略的檔案。

![檔案間搜尋、取代面板](/assets/imgs/2022-01-15/replace_in_files.png){:height="688px" width="551px"}

如果需要查看搜尋結果的上下文，也能按下 Open in editor 功能另外顯示結果。

![查看搜尋結果的上下文](/assets/imgs/2022-01-15/open_in_editor.png){:height="298px" width="720px"}

*(寫到這裡，十分佩服 VSCode 的搜尋介面可以包含這麼多功能，但是又設計得十分簡潔。)*

也請參考拙作 [Visual Studio Code 尋找與取代新行](/Visual_Studio_Code_Newline/)。

### 把多行文字合併成一行

1\. 選取要合併的範圍。

2\. 按下 `F1` 或 `Ctrl + Shift + P` 鍵開啟 VSCode 的命令列，輸入 `Join Lines`。

![Join Lines](/assets/imgs/2022-01-15/join_lines.png){:height="394px" width="720px"}

3\. 即會合併成一行。

![合併為一行](/assets/imgs/2022-01-15/oneline.png){:height="62px" width="720px"}

### 延伸閱讀

- [Visual Studio Code Key Bindings](https://code.visualstudio.com/docs/getstarted/keybindings)
- [visual studio code - How to make vscode compact multiple lines to a single line? - Stack Overflow](https://stackoverflow.com/questions/45204617/how-to-make-vscode-compact-multiple-lines-to-a-single-line)
- [新一代的編輯器 — VSCode. 俗話說 「工欲善其事，必先利其器」… - by Larry Lu - Larry・Blog](https://larrylu.blog/vscode-1b6f24e082ba): 推坑文，這篇文章介紹了他喜歡 VSCode 的十個特色。