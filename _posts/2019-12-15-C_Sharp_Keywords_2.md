---
layout: post
title: C# 關鍵字 2 - readonly & const
date: 2019-12-15 12:00:00 +0800
categories:  [C#]
---

我們都常在程式碼中看到 `readonly` 和 `const`，從字面上來看，兩個關鍵字都帶有「無法修改」的意味，但實際上它們分別有不同的特色。

### `const` 的特色

- 只能在宣告時指定數值。
- 宣告以後不能再修改數值。
- 必須為基本型別 (如 `int`) 或字串。

在編譯時就會直接取代原本使用到 `const` 常數的地方。適合用在需要效率，或是數值完全不更動的情況。

### `readonly` 的特色

- 只能在宣告時，或建構子內指定數值。
- 建構子執行完畢後，無法再修改內容。 (但是在建構子內是可以修改的) 
- 跟一般變數一樣可以是任意型別。 (物件也可以)

由於在編譯時仍然是變數，在初始化時可以再重新指定數值，因此較具備使用上的彈性。

### 牛刀小試

```cs
public class Program
{
    public class book{
        public int pages;
        
        public book(int p){
            this.pages = p;    
        }
    }
    
    public static readonly book a = new book(150); // 可以在初始化時、建構子內指定數值
    
    public static void Main()
    {
        // a = new book(200); // 不能修改
        Console.WriteLine(a.pages);
    }
}
```

*(上述部分內容為參考資料的文字，並重新改寫)*
*(2024/05 更新為鐵人賽文章的內容)*

### 參考資料

- [C# - const vs static readonly - John Wu's Blog](https://blog.johnwu.cc/article/c-sharp-const-vs-static-readonly.html)