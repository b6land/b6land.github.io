---
layout: post
title: TFS 標註功能 - 查看特定行變更
date: 2022-04-16 12:00:00 +0800
categories: [TFS]
---

在微軟的 TFS 版本控制系統內，想要查詢是誰編輯了哪一行，除了逐一查詢檔案的變更紀錄以外，也可以查詢特定行的變更。延伸閱讀內包含新手教學、TFS 與 Git 比較的連結。

### 使用方式

TFS 的標註功能，可以查看特定行最後是由誰變更的（不過沒有該行的完整歷史）。

從原始碼檔案總管找到要檢查的檔案，點擊右鍵，並選擇標註 (Annotation)，就可以看到哪一段最後由誰編輯簽入。

![TFS 右鍵功能表 - 標註功能](/assets/imgs/TFS_annotation.png)


### 參考資料

- [TFS - getting history for specific line of code in Visual Studio - Stack Overflow](https://stackoverflow.com/questions/15928072/tfs-getting-history-for-specific-line-of-code-in-visual-studio)

### 延伸閱讀

- [Team Foundation 版本控制 新手上路 - 買菜的保肝膠囊](https://fsmytsai.github.io/Team-Foundation-版本控制-新手上路/): TFS 的基礎操作教學。

- [TFS Git 筆記 - 該用 TFVC 還是 Git？-黑暗執行緒](https://blog.darkthread.net/blog/tfvc-vs-git/): TFS 和 Git 哪個好？這篇文章內有優缺點比較。

對我來說，無論是簡單或複雜的專案都適合用 Git 管理，但是有多人共同開發時，在開發流程上 Git 確實會花比 TFS 多的人工檢查成本。