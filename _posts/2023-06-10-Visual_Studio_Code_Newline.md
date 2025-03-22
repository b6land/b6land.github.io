---
layout: post
title: Visual Studio Code 尋找與取代新行
date: 2023-06-11 12:00:00 +0800
categories:  [IDE]
--- 

本篇介紹 Visual Studio Code (VSCode) 中，怎麼尋找與取代新行。尋找與取代的介紹，請參考拙作 [Visual Studio Code 用快捷鍵、搜尋與合併多行增進開發效率](/Visual_Studio_Code_Shortcuts/)。

### 尋找與取代新行

![在搜尋時按下 Ctrl + Enter 尋找換行](/assets/imgs/2023-06-10/find_newline.png){:height="170px" width="720px"}
1\. 按下 Ctrl + F，在搜尋欄位中，按下 Ctrl + Enter，就可以找到換行符號。

![啟用 Regex，在取代欄內輸入 \n](/assets/imgs/2023-06-10/replace_newline.png){:height="102px" width="720px"}
2\. 如果想要把特定文字用換行符號取代，按下 Ctrl + H，在搜尋欄內輸入想查詢的文字 (例如：換行) 右方按下 Regex 的按鈕，取代欄輸入 `\n` 即可。

![取代結果](/assets/imgs/2023-06-10/replace_newline_2.png){:height="97px" width="720px"}
3\. 按下取代後，可以看到換行的結果。

![在左方取代面板按下 Ctrl + Enter 尋找換行](/assets/imgs/2023-06-10/find_newline_panel.png){:height="522px" width="558px"}
4\. 以上的步驟，也適用於左邊的檔案間查詢 / 取代面板 (Ctrl + Shift + F)。

### 參考資料

- [Find and replace with a newline in Visual Studio Code - Stack Overflow](https://stackoverflow.com/questions/30351529/find-and-replace-with-a-newline-in-visual-studio-code)