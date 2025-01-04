---
layout: post
title: 繁簡體中文辨識轉換簡介
date: 2025-01-04 19:00:00 +0800
categories: [Programming]
--- 

因為這個主題超大，本文章會簡單介紹一下中文的編碼與辨識、轉換問題，請透過內文的連結進一步理解編碼的方式，與辨識、轉換的作法。  

### 中文的編碼 (Encoding)

目前的中文大致上可分為繁體中文和簡體中文。

繁體中文常採用 Big5 編碼，其歷史以及衍生編碼、缺字問題，可以參考：[大五碼 - 維基百科](https://zh.wikipedia.org/zh-tw/%E5%A4%A7%E4%BA%94%E7%A2%BC)、[Big5 - 字嗨！](https://zi-hi.com/Big5)，其編碼空間圖解可參考：[認識中文字元碼](https://idv.sinica.edu.tw/bear/charcodes/Section09.htm)。

而簡體中文常採用 GB2312，其由來可以參考：[GB 2312 - 維基百科](https://zh.wikipedia.org/zh-tw/GB_2312)，並藉由 GB18030 或 GBK 編碼收錄數萬個文字，中間的差異請參考 [一图弄懂ASCII、GB2312、GBK、GB18030编码-腾讯云开发者社区](https://cloud.tencent.com/developer/article/1343240)。  
  
目前的主流編碼是 UTF-8，可同時顯示繁體、簡體中文，以及其它國家的文字。

相關的歷史，請參考 [簡體字 - 維基百科](https://zh.wikipedia.org/wiki/%E7%AE%80%E4%BD%93%E5%AD%97) 和 [繁體字 - 維基百科](https://zh.wikipedia.org/wiki/%E7%B9%81%E4%BD%93%E5%AD%97)。

### 簡繁體辨識

繁體中文的編碼 (如 Big5) 內，有繁體字和一部分簡化的字 (例如臺、台)；簡體中文的編碼 (如 GB2312) 內，也包含繁體字和一部分臺灣常用文字。由於 Big5 和 GB2312 等編碼有互相重疊的地方，因此文件沒有說明使用的編碼或語言十，要如何辨識編碼、顯示正確內容，也成為一大問題。

而主流的 UTF-8 並沒有依據簡、繁體分類編碼中文字，雖然能正確顯示中文字，但不易透過字碼判斷目前顯示或輸入的是簡體還是繁體中文。

相關的檢查議題如下，目前仍沒有合適的解決方案：

- [C#判断实现区别汉字简体中文还是繁体中文](http://www.china-tom.com/template.asp?articleid=541)
- [判断字符是否为简体字或繁体字 - Dvel's Blog](https://dvel.me/posts/judge-characters-simplified-or-traditional/)
- [\[C#\] 字串 Unicode 字碼辨識與繁簡字碼轉換心得筆記 - 記憶裂縫 - 點部落](https://dotblogs.azurewebsites.net/MemoryRecall/2021/09/13/234402)
- 裡面有很多 GB2312 和 BIG5 的知識，以及如何辨識的討論：[PowerShell 小工具 - 簡易檔案編碼識別-黑暗執行緒](https://blog.darkthread.net/blog/ps-enc-detector)

### 簡繁體轉換

一般來說，由於一個簡體字可以對應到多個繁體字，因此簡體轉繁體比較麻煩，而繁體轉簡體較為簡單，相關議題可參考：[正簡轉換 - 維基百科](https://zh.wikipedia.org/zh-tw/%E7%B9%81%E7%B0%A1%E8%BD%89%E6%8F%9B)。

想要將簡體中文轉換為繁體中文，最好可以先判斷來源文字是哪種中文，避免將繁體中文重複轉換，導致潛在的問題 (例如「斗六」誤翻成「鬥六」)。

- [使用 C# 繁體轉簡體 - HackMD](https://hackmd.io/@SuFrank/SyPonjQA5)

### 工具箱

1. 簡易的簡體、繁體轉換：[繁簡轉換 - 簡繁轉換 - 繁轉簡 - 簡轉繁](https://www.ifreesite.com/gbk-big5-gb2312-utf8.htm)
2. 檢查輸入的文字會被辨識為繁體還是簡體：[Chinese Tools - Simplified or traditional?](https://www.chineseconverter.com/en/convert/find-out-if-simplified-or-traditional-chinese )
3. 檢查 UTF-8 文字的編碼：[Utf-8 Converter, Utf-8 Encoding and Decoder - CheckSERP](https://checkserp.com/encode/utf8/ )
4. 檢查中文字在資料庫內的變體：[Unihan Database Lookup](http://www.unicode.org/charts/unihan.html) (變體的相關說明可以參考：[回應 Ptt Perl 版「Unihan 查詢」文章兩則](https://gugod.org/2015/09/unihan/))
