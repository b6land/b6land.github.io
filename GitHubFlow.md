---
layout: post
title: GitHub Flow
date: 2020-07-27 12:00:00 +0800
categories: Git
--- 

GitHub Flow 的概念，是在 `master` 上建立新分支，直接開發新的功能與驗證，並合併至 `master`。發行時直接建立新 Tag。整體來說和 SVN 的開發流程類似。

GitHub Flow 可減少建立分支 / 合併的操作，避免開發人員在分支操作上的繁複操作，以及減低過多的分支操作導致出錯的機會。

### 使用的工作流程

GitHub Flow 的核心觀念是：`master` 分支是隨時處於可部署的狀態。

1. 建立分支，實作新功能、修 bug 等等...
2. 提交變更：當程式碼變更以後，提交並撰寫清晰的紀錄，讓他人能容易瞭解你的變更，或更快找到 bug。
3. Pull Request：要求合併，讓其他人可以檢視你的修改成果。
4. 討論和檢視你的程式碼。
5. 部署：程式碼現在已經被經過檢驗，可以用於部署上。
6. 合併：將經過檢驗的分支合併回 `master` 分支，整合變更的程式碼。

### 參考資料

- [Understanding the GitHub flow · GitHub Guides](https://guides.github.com/introduction/flow/)
- [讓我們來了解 GitHub Flow 吧！](https://medium.com/@trylovetom/-4144caf1f1bf)
- [GitHub Flow 及 Git Flow 流程使用時機](https://blog.wu-boy.com/2017/12/github-flow-vs-git-flow/)
- [Day12 什麼是 CICD - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10219083)