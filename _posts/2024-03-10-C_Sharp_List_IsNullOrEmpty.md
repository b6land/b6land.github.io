---
layout: post
title: C# List / IEnumerable 實作 IsNullOrEmpty
date: 2024-03-10 12:00:00 +0800
categories:  [C#]
--- 

在 C# 中，string 包含 [IsNullOrEmpty](https://learn.microsoft.com/zh-tw/dotnet/api/system.string.isnullorempty?view=net-8.0) 方法，可以檢查兩種型態的空字串。其實也可以為 List (列表) / IEnumerable (可列舉介面) 建立類似的方法。

### 內文

在 StackOverFlow 上的 [Does C# have IsNullOrEmpty for List/IEnumerable? - Stack Overflow](https://stackoverflow.com/questions/8582344/does-c-sharp-have-isnullorempty-for-list-ienumerable) 問題內，提到可以自行建立擴充方法。

參考 [IEnumerable IsNullOrEmpty](http://danielvaughan.org/posts/.net/collections/2010/04/18/IEnumerable-IsNullOrEmpty/)，節錄程式碼並翻譯成中文，如下：

```cs
public static class Extensions
{
    /// <summary>
    /// 檢查集合是否為 null 或未包含元素
    /// </summary>
    /// <typeparam name="T"> 列舉型別 </typeparam>
    /// <param name="enumerable"> 列舉物件，可能是 null 或空白 </param>
    /// <returns>
    ///     <c>true</c> 當 IEnumerable 是 null 或空白; 否則, <c>false</c>.
    /// </returns>
    public static bool IsNullOrEmpty<T>(this IEnumerable<T> enumerable)
    {
        if (enumerable == null)
        {
            return true;
        }
        /* 如果是 list，使用 Count 屬性
         * Count 屬性計算量為 O(1) ，而 IEnumerable.Count() 為 O(N). */
        var collection = enumerable as ICollection<T>;
        if (collection != null)
        {
            return collection.Count < 1;
        }
        return !enumerable.Any();
    }

    /// <summary>
    /// 檢查集合是否為 null 或未包含元素
    /// </summary>
    /// <typeparam name="T"> 列舉型別 </typeparam>
    /// <param name="collection"> 集合物件，可能是 null 或空白 </param>
    /// <returns>
    ///     <c>true</c> 當 ICollection 是 null 或空白; 否則, <c>false</c>.
    /// </returns>
    public static bool IsNullOrEmpty<T>(this ICollection<T> collection)
    {
        if (collection == null)
        {
            return true;
        }
        return collection.Count < 1;
    }
}
```

重點：在參數內加入 this 關鍵字，建立擴充方法；針對 ICollection 和 IEnumerable 分開處理，使用最有效率的計算方法。
