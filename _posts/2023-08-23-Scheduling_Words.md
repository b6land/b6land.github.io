---
layout: post
title: 工業排程系統介紹與相關縮寫
date: 2023-08-23 12:00:00 +0800
categories: [Other]
---

很久以前紀錄的排程系統介紹，以及工業自動化領域的縮寫與全名。

### 縮寫

先來認識以下的縮寫：

- SCM: Supply Chain Management, 供應鏈管理系統
- APS: Advanced Planning and Scheduling, 先進規劃與排程系統
- MES: Manufacturing Execution System, 製造執行系統
- QTS: Quality Tracking System, 品質追蹤系統
- ERP: Enterprise Resource Planning, 企業資源規劃

### 關於排程系統

在工廠製作產品時，各種工單會有著不同的製造時間，有一或多條產線的狀態下，需要設計排程，安排各工單生產的機器、指定各工單的順序。

工業領域排程系統的目標如下：

1. 盡可能提高生產的效率。
2. 減少機器閒置的時間。

目前大部分的排程系統，皆使用人工安排各產線的工單。部分公司可能應用上述提到的 APS，採用自訂的程式邏輯，或使用基因演算法 (Genetic Algorithm) 找出最佳的安排組合。

由於排程系統掌控工單，而工單內容與物料相關，因此常和 MES、ERP 系統一同使用。