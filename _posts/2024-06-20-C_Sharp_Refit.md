---
layout: post
title: C# 用 Refit 建立型別安全的 API 介面
date: 2024-06-20 21:00:00 +0800
categories:  [C#]
--- 

Refit 是 .Net 平台的函式庫，可以幫忙呼叫 API，並包含多個優點。以下將簡單介紹，並提供 C# 的使用範例。

### 介紹

透過 Refit 呼叫 API 的優點，是可以省略撰寫大部分的連線程式碼 (ex. HttpClient 的設定)，而且傳送和回應使用強型別，在資料操作上會比較安全。

### 步驟

1. 使用 NuGet 套件管理員安裝 Refit。
2. 建立回應和請求的資料類別。
3. 建立介面，將想要存取的 API 建立方法，使用屬性指定要存取的路徑和方式。
4. 使用 `RestService.For<介面>` 建立實作 API 介面的物件。
5. 呼叫 API 內的方法，並取得 API 的回應，回應會轉為資料物件。

### 範例

使用 [JSONPlaceholder - Free Fake REST API](https://jsonplaceholder.typicode.com) 網站的測試 API，這個網站可用 GET 傳回虛擬的內容，你也可以用 POST 傳送自己的內容出去。

1\. 建立 IRefitAPI.cs，裡面建立回應、請求的資料類別，以及 API 介面。

```csharp
using Refit;

public interface IRefitAPI{
    [Get("/posts")] // 表示要用 Get 呼叫 API 的路徑，並設定要取回的資料
    Task<List<Post>> GetPosts();

    [Post("/posts")] // 表示要用 Post 呼叫 API 的路徑，並傳入參數
    Task<Post> PostPosts(RequestBody req);
}

// 回應資料類別
public class Post
{
    public int Id { get; set; }
    public string? Title { get; set; }
    public string? Body { get; set; }
}

// 請求資料類別
public class RequestBody
{
    public string? title { get; set; }
    public string? body { get; set; }
}
```

2\. 建立 Refit.cs，新增類別呼叫 API。

```csharp
using Refit;

public class RefitAPI
{
    public async void Run(){
        var api = RestService.For<IRefitAPI>("https://jsonplaceholder.typicode.com"); // 輸入 API 網址

        try
        {
            var getPosts = await api.GetPosts(); // 呼叫 API 取得資料
            foreach (var post in getPosts)
            {
                Console.WriteLine($"Title: {post.Title}, Body: {post.Body}");
            }

            var Response = api.PostPosts(new RequestBody{title = "Anime", body = "Bocchi the Rock"}).Result; // 呼叫 API 傳送資料，並取得結果
            Console.WriteLine($"ID: {Response.Id}, Title: {Response.Title}, Body: {Response.Body}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

### 小秘訣

- 要判斷是否成功接收資料，可以用 `response.IsSuccessStatusCode` 。
- 使用 POST 發送字串資料時，如果希望傳送出去的內容被視為 JSON，可以加入 Content-Type 的 Header 屬性，範例如下：

```csharp
[Post("/PostSomeStuff")]
[Headers("Content-Type: application/json; charset=UTF-8")]
Task<Response> PostSomeStuff([Body] string body);
```

- 如果 API 請求需要特定標頭或認證，可以參考 [GitHub - reactiveui/refit: Setting request headers](https://github.com/reactiveui/refit?tab=readme-ov-file#setting-request-headers) 一節設定。

### 參考資料

- 官方網站：[GitHub - reactiveui/refit](https://github.com/reactiveui/refit )
- 使用 Refit 存取 GET 屬性 API: [Getting Started with Refit in .NET](https://www.c-sharpcorner.com/article/getting-started-with-refit-in-net/)
- 使用 Content-Type Header：[Content-Type header attribute on method doesn't apply correctly · Issue #136 · reactiveui/refit · GitHub](https://github.com/reactiveui/refit/issues/136)