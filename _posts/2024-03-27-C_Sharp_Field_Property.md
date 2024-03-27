---
layout: post
title: C# 欄位與屬性
date: 2024-03-27 12:00:00 +0800
categories:  [C#]
--- 

在 C# 中，屬性和欄位都可以用來儲存資料，其中有哪些差異呢？它們使用的時機為何？

### 欄位 Field

用來儲存狀態或計算結果，可以根據 `public`, `protected`, `private` 等存取修飾詞設定欄位的可見範圍。

```csharp
public class Customer
{
    public string name;
    private int _money;
}
```

### 屬性 Property

一樣可以用來儲存狀態或計算結果，不過可以加上存取子，用以限制取得 (get) 或設定 (set) 的範圍；此外也可以存取子內加入設定或取得時須執行的邏輯。

```csharp
public class Customer
{
    private string? _name;
    private int _money;

    public string? Name // 可公開存取
    {
        get { return _name; }
        set { _name = value; }
    }

    public int Money
    {
        get { return _money; } 
        private set 
        { // 只能在 Customer 類別內設定，金額至少為 0 元
            if (value < 0) { _money = 0; }
            else { _money = value; }
        }
    }

    // 類別內的其它方法
}
```

### 什麼是自動實作屬性 ?

自動實作屬性 (Auto-Implemented Properties) 可以節省一部分的程式碼。

例如上述的 Customer 類別，就可以濃縮如下：

```csharp
public class Customer
{
    public string? Name { get; set; }
    public int Money { get; private set; } // 不含金額至少為 0 元的邏輯
}
```

但是沒辦法在內部實作存取時的邏輯，若需要撰寫，則需使用原本的寫法。

除了精簡程式碼以外，在使用 ORM 存取資料庫、WebAPI 傳遞資料時，通常類別內的資料也使用自動實作屬性。

### 參考資料

- 欄位、屬性的不同: [屬性Property與欄位Field，傻傻分不清楚](https://slashview.com/archive2014/20141008.html)
- 自動實作屬性的解說: [C# 3.0自動實作屬性的犀利之處-黑暗執行緒](https://blog.darkthread.net/blog/c-3-auto-imp-prop/)
- 關於欄位、屬性的使用時機: [mrkt 的程式學習筆記: 屬性(Property) 與 欄位(Field)](https://kevintsengtw.blogspot.com/2011/09/property-field.html )
- WebAPI 資料傳輸物件範例:  [DTO 建立資料傳輸物件 - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5)

