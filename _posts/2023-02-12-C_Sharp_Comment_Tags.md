---
layout: post
title: C# 常用文件註解標籤
date: 2023-02-12 12:00:00 +0800
categories:  [C#]
---

在 C# 裡面，使用 `///` 開頭的註解和標籤 (Tag)，會被編譯器編譯成 XML 格式文件。除了適合用來產生 API 文件，Visual Studio 的 IntelliSense 功能能預覽這些註解，讓其他人在開發時，也能快速的理解類別、類別成員的用途。

### 註解標籤的使用

在 Visual Studio 中，只要在類別名稱上一行、類別成員上一行輸入 `///`，就會自動產生 `<summary>` 標籤。可以輸入關於此類別、類別成員的描述。

此外，常見的註解標籤有幾個，列表如下：

- `<param name="name">`: 用來描述這個參數的作用。如果類別成員沒有這個參數，會提示錯誤。
- `<returns>`: 描述回傳的值。
- `<remarks>`: 常用來補充 `<summary>` 的額外說明。

此外還有更多的標籤可以使用，請參閱 [Documentation comments - document APIs using /// comments - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/xmldoc/recommended-tags)。

需要換行時，則有兩個選擇: 將每行文字分別用 `<para>` 段落標籤包圍，或在每行文字結尾輸入 `</br>` 換行。([How to add a line break in C# .NET documentation - Stack Overflow](https://stackoverflow.com/questions/7279108/how-to-add-a-line-break-in-c-sharp-net-documentation))

### 範例

```cs
/// <summary> 手錶類別 </summary>
/// <remarks> 示範用類別 </remarks>
protected class Watch
{
    protected int hour;
    protected int minute;
      
    /// <summary> 設定手錶的時間 </summary>
    /// <param name="h"> 小時 </param>
    /// <param name="m"> 分鐘 </param>
    public virtual void Setup(int h, int m)
    {
        this.hour = h;
        this.minute = m;
    }

    /// <summary> 顯示時間 </summary>
    public void Show()
    {
        Console.WriteLine(hour + ":" + minute);
    }
}
```

在 Visual Studio Code 中預覽結果如下：

![顯示摘要和備註](/assets/imgs/2023-02-12/comment_tags_1.png)

![顯示參數](/assets/imgs/2023-02-12/comment_tags_2.png)