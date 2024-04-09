---
layout: post
title: Git Commit Message 怎麼寫
date: 2024-04-09 12:00:00 +0800
categories: [Git]
---

Git 的 Commit Message (簽入訊息)，雖然可以隨意輸入，但是可以按照特定的格式撰寫，會讓大家在查閱歷史 Commit 時，更加地有效率。

### 類型

以下是常見的 Git Commit 類型：

| 類型  | 描述  |
| --- | --- |
| feat | 新增或修改功能 |
| fix | 修正錯誤 |
| docs | 新增或修改文件 |
| style | 調整程式碼風格，例如格式、縮排、空白 |
| refactor | 重構程式碼 |
| perf | 改善效能 |
| test | 新增或修改測試案例 |
| chore | 與程式碼不直接相關的變動，例如調整建構程序、版號調整、加入依賴套件等 |
| revert | 復原之前的 Commit |

### 撰寫的重點

使用 `Type: Commit Message`  的格式，更容易在檢視歷史紀錄時快速辨識 Commit 內容，例如：

```plaintext
feat: 新增網頁瀏覽的功能
fix: 修正計算金額結果未顯示小數點的問題
```

此外，在輸入 Commit Message 時，應該盡可能精確地描述變更的內容，不精確的描述，會導致檢視時需要更多時間理解 (那就失去寫 Commit Message 的意義了)。

以下是幾個不好的範例：

```plaintext
feat: 更新了 5 個功能
fix
  (什麼訊息都沒有留下)
```

### 參考資料

- [Git Commit Message 這樣寫會更好，替專案引入規範與範例 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10228738)  
- [Mastering Git Commit Types: Proper Usage and Best Practices](https://www.linkedin.com/pulse/mastering-git-commit-types-guide-proper-usage-best-practices-sharma)  