---
layout: post
title: 在 GitHub Pages 內使用 Jekyll 產生網誌，收到來自 dependabot 的 Bump ... from ... to ... 訊息
date: 2023-01-25 12:00:00 +0800
categories: [Jekyll, GitHub Pages]
---

在 GitHub Pages 內建立 Jekyll 產生出來的靜態網誌，可能偶爾會收到 dependabot 寄出，標題如下的訊息：Bump ... from ... to ... 的訊息，例如：Bump commonmarker from 0.23.6 to 0.23.7 (PR #6)。可透過這封郵件修正安全性錯誤。

通常收到此訊息，表示所使用的套件，已經有推出新版本，並修正需要修補的安全性錯誤。Jekyll 所使用的套件與版本，會寫在 `Gemfile.lock` 內，因此主要也是更新這個檔案。

![郵件內文](/assets/imgs/dependabot_mail.png)

收到郵件後，可以拉到信件的下方，點選 **You can view, comment on, or merge this pull request online at:** 下方的連結進入 Pull Request 頁面。

![從郵件進入 PR](/assets/imgs/dependabot_PR.png)

或從 GitHub 的 Reposistory 找到 Pull requests 頁面，再找到對應的 Pull request。

![PR 列表](/assets/imgs/dependabot_PR_list.png)

在 Pull request 的下方，按下 Merge pull request 的按鈕。

![準備合併](/assets/imgs/dependabot_merge.png)

需要填寫 commit 訊息，這裡也可以保持預設文字。完成後按 Confirm merge。

![填寫 commit 訊息](/assets/imgs/dependabot_merge_2.png)

合併完成。Pull request 會自動關閉。

![合併完成](/assets/imgs/dependabot_merge_3.png)

過一段時間 dependabot 會自己刪除自動建立的分支。

![刪除分支](/assets/imgs/dependabot_merge_4.png)

大功告成 !!