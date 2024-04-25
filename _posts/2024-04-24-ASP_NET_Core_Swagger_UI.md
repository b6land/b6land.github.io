---
layout: post
title: ASP.NET Core Swagger UI 的設定
date: 2024-04-24 12:00:00 +0800
categories:  [ASP.NET]
---

Swagger UI 可以產生互動式的 API 的說明文件，列出 API 的說明、使用的模型，以及發送請求。這篇文章專注於 .Net 6 的 Swagger UI 設定，包含「專案文件設定」、「自訂 Request Header」兩個部份。

### 專案文件設定

如果方案底下有兩個或以上的專案，要把所有的專案的說明加入至 Swagger UI 內，需要增加以下兩行至每個專案的 csproj 檔案，放置於 `PropertyGroup` 標籤內：

```xml
<PropertyGroup>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <NoWarn>$(NoWarn);1591</NoWarn> <!-- 如果有其它需壓制的警告，也一起加進去 -->
</PropertyGroup>
```

這樣專案在建置時會產生 {專案名稱}.xml 的檔案，需要在 `Program.cs` 內的 `AddSwaggerGen` 方法加入 XML 檔案：

```csharp
builder.Services.AddSwaggerGen(c =>
{
    c.IncludeXmlComments(Path.Combine(AppContext.BaseDirectory, "Project.xml")); // 需替換成專案名稱.xml
    // 其它 Swagger UI 設定
}
```

### 自訂 Request Header

如果今天 API 的設計包含額外的 Header，例如請求來源、語系、Token 等，想要在 Swagger 內加入這些自訂的 Header 的話，可以用以下的方式新增：

1\. 建立一個實作 `IOperationFilter` 的類別，包含 `Apply` 方法。

2\. `Apply` 方法內填入以下的程式碼：

```csharp
public void Apply(OpenApiOperation operation, OperationFilterContext context)
{
    if(operation.Parameters == null) {
        operation.Parameters = new List<OpenApiParameter>();
    }

    operation.Parameters.Add(new OpenApiParameter
    {
        Name = "Source", // 參數名稱
        In = ParameterLocation.Header, // 除了標頭以外，也能放在 Path, QueryString ... 等
        Schema = new OpenApiSchema { Type = "string" }, // 參數資料型態
        Description = "請求來源", // 參數說明
        Required = true // 必須輸入
    }) ;
}
```

3\. 在 `AddSwaggerGen`  方法內加入 `c.OperationFilter<HeaderFilter>()` ：

```csharp
builder.Services.AddSwaggerGen(c =>
{
    c.OperationFilter<HeaderFilter>()
    // 其它 Swagger UI 設定
}
```

在 Swagger UI 上就會出現輸入額外 Header 的欄位 ~

### 參考資料

- Step By Step 建立 Swagger UI：[\[.net 6\] swagger範例 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10298616)
- .net 5 以前如何自訂 Request Header：[自訂Swagger Request Header (昕力)](https://www.tpisoftware.com/tpu/articleDetails/2325)
- .net 5 以後，需要改用 OpenAPI 類別自訂 Request Header：[c# - Web Api How to add a Header parameter for all API in Swagger - Stack Overflow](https://stackoverflow.com/questions/41493130/web-api-how-to-add-a-header-parameter-for-all-api-in-swagger)
- 官方說明：[開始使用 Swashbuckle 及 ASP.NET Core - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-8.0&tabs=visual-studio)
- 如果要加入 Authorize Token 可以看這篇：[在 Swagger UI 加上驗證按鈕，讓 Request Header 傳遞 Authorize Token - 伊果的沒人看筆記本](https://igouist.github.io/post/2021/10/swagger-enable-authorize/)
- 保哥的完整「觀念到實作」文章 (適用 .Net Core 3 ~ .Net 5)：[如何在 ASP․NET Core 3 完整設定 NSwag 與 OpenAPI v3 文件 - The Will Will Web](https://blog.miniasp.com/post/2019/12/21/ASP%E2%80%A4NET-Core-3-NSwag-OpenAPI-v3)