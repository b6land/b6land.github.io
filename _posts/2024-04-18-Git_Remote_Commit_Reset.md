---
layout: post
title: Git 推送遠端後的分支重命名、修改 Commit Message 和重設 Commit
date: 2024-04-18 12:00:00 +0800
categories: [Git]
---

Git Commit 完、推送上去後，才發現不小心手殘打錯字，要怎麼復原呢？如果分支目前還沒有被其他人修改過，可以使用以下的方法補救。

### 如何重新命名本地和遠端分支

包含 4 個步驟：

1. `git branch -m <old_name> <new_name>`  先為本地端的分支重新命名。
2. `git push <remote> --delete <old_name>`  刪除遠端的分支。
3. `git branch --unset-upstream <new_name>`  取消本地端分支原本追蹤的遠端分支。
4. `git push <remote> -u <new_name>`  重新推送目前分支至遠端，且追蹤該遠端分支。

參考資料：[repository - How do I rename both a Git local and remote branch name? - Stack Overflow](https://stackoverflow.com/questions/30590083/how-do-i-rename-both-a-git-local-and-remote-branch-name)

### 如何修改 Commit Message

1. `git commit --amend`  修改最近一次 Commit 的訊息內容。
2. `git push --force-with-lease <repository> <branch>`  指令強制推送分支到遠端。

使用 `--force-with-lease`  的理由是如果遠端分支已被 (其他人) 更新的話，會中斷操作，比較安全。

參考資料：[Changing git commit message after push (given that no one pulled from remote) - Stack Overflow](https://stackoverflow.com/questions/8981194/changing-git-commit-message-after-push-given-that-no-one-pulled-from-remote)  

### 如何重設 Commit

與修改 Commit Message 很類似，重設完後再強制推送即可。

1. `git reset --hard <commit-hash>`  將分支恢復到特定的 Commit 下 ( `--hard`  表示不保留期間修改的變更)。
2. `git push --force-with-lease <repository> <branch>`  指令強制推送分支到遠端。

參考資料：[git - Resetting remote to a certain commit - Stack Overflow](https://stackoverflow.com/questions/5816688/resetting-remote-to-a-certain-commit)