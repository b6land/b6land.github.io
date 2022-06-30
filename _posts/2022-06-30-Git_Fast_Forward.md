---
layout: post
title: Git 的 Fast Forward 合併
date: 2022-06-06 12:00:00 +0800
categories: [Git]
---

本文介紹 Git 的 Fast Forward 合併是什麼意思。

### 什麼是 Fast Forward 合併

- Git 在可以使用 Fast Forward 合併分支的情況下，就會使用 Fast Forward。
- 假設今天從 `master` 建立了一個新的 `feature` 分支，然後只在 `feature` 分支 commit 的話，這時就可以用 Fast Forward 的方式，將 `feature` 分支移動到 `master` 合併。
- 其實就是將指向 `master` 的 commit 節點移動至 `feature` 的最新 commit 節點。
- 如果 `master` 已經被修改過的話，就不能使用 Fast Forward 合併，需要建立一個新的合併節點。
- 如果使用 `git merge --no-ff [branch_name]` ，強制不使用  Fast Forward 合併的話，則建立新的合併節點。
- 參考資料: [Git Fast-forward Merge – FunMing 基地](https://fmbase.tw/blog/2015/04/05/git-fast-forward-merge/)
- 參考資料: [合併分支【分支】 - 連猴子都能懂的Git入門指南 - 貝格樂（Backlog）](https://backlog.com/git-tutorial/tw/stepup/stepup1_4.html)

