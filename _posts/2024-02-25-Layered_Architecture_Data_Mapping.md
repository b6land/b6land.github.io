---
layout: post
title: 分層模式 - 資料物件定義與操作
date: 2024-02-25 12:00:00 +0800
categories:  [Programming, C#]
--- 

本篇介紹分層模式中，DTO, Domain Object, Model 等資料物件的定義，以及要在哪一層操作資料。

### 關於 DTO, Domain Object 和 Model 的定義

關於資料物件的定義，不同開發者間有部分細微的差異，以下是較常見的定義：

- DTO 全名為 Data Transfer Object，可指在分層模式中在不同層間傳遞的資料物件。
- Domain Object 指的是 Domain-Driven Application (領域驅動應用程式) 內的物件，可能是概念、實體 (Entity) 或邏輯。
- Model 可以泛指 Domain Object, DTO 等物件類別。

可參考：

- [What is the difference between an MVC Model object, a domain object and a DTO - Stack Overflow](https://stackoverflow.com/questions/3853749/what-is-the-difference-between-an-mvc-model-object-a-domain-object-and-a-dto)
- [Understanding Domain Objects, Entities, DTOs, and Models in C# - by Abhinn Mishra - Medium](https://medium.com/@mishraabhinn/understanding-domain-objects-entities-dtos-and-models-in-c-207bb5c1d97c)

### 在 Persistence Layer 操作資料物件，而非 Service Layer

Persistence Layer (或稱 Repository Layer) 適合查詢資料庫或其它資料來源，將查詢結果初步操作、映射 (Mapping) 為 Data Model，再交給 Service Layer 處理商業邏輯，並轉為 Result Model。

這樣做的好處是減少耦合性，Service Layer 不必知道存取資料來源的任何細節。當資料來源有變動時，只需修改 Persistence Layer 。資料來源的類型不常變更 (例如 MySQL, MS-SQL ... 等)，但資料表和欄位等 Schema 可能常常變更，此時只需要調整 Persistence Layer 的存取方式。

請參閱下方的範例程式碼。

- 參考資料：[c# - Mapping in repository layer - Stack Overflow](https://stackoverflow.com/questions/22739845/mapping-in-repository-layer)

### 範例程式碼

由 ChatGPT 產生 C# 的範例程式碼：

```cs
using System;
using System.Collections.Generic;

// 定義 Data Model
public class DataModel
{
    public int Id { get; set; }
    public string Name { get; set; }
    // 其他屬性...
}

// 定義 Result Model
public class ResultModel
{
    public int Id { get; set; }
    public string Name { get; set; }
    // 其他屬性...
}

// Persistence Layer (Repository Layer)
public class PersistenceLayer
{
    // 模擬資料庫
    private List<DataModel> database = new List<DataModel>
    {
        new DataModel { Id = 1, Name = "Item 1" },
        new DataModel { Id = 2, Name = "Item 2" },
        // 其他資料...
    };

    // 查詢資料庫並映射為 Data Model
    public DataModel GetDataModelById(int id)
    {
        return database.Find(item => item.Id == id);
    }
}

// Service Layer
public class ServiceLayer
{
    private PersistenceLayer persistenceLayer = new PersistenceLayer();

    // 將 Data Model 轉換為 Result Model
    public ResultModel ProcessData(int id)
    {
        DataModel dataModel = persistenceLayer.GetDataModelById(id);
        // 在這裡進行商業邏輯處理
        // 可能包括操作 Data Model，驗證資料等等
        // 在這個簡單的例子中，我們只是將 Data Model 轉換為 Result Model
        return new ResultModel { Id = dataModel.Id, Name = dataModel.Name };
    }
}

class Program
{
    static void Main(string[] args)
    {
        ServiceLayer service = new ServiceLayer();
        int idToQuery = 1;
        ResultModel result = service.ProcessData(idToQuery);
        Console.WriteLine($"Result: Id={result.Id}, Name={result.Name}");
    }
}
```

### 其它參考資料

- 關於應用在 Persistence Layer 的 Repository Pattern 設計方式： [隨手 Design Pattern (4) - Repository 模式 (Repository Pattern) - Ray's Notes](https://wayne-blog.com/2023-02-24/webapi-3-tier-introduction/)
- 資料流向可以參考：[菜雞新訓記 (5): 使用 三層式架構 來切分服務的關注點和職責吧 - 伊果的沒人看筆記本](https://igouist.github.io/post/2021/10/newbie-5-3-layer-architecture/)
