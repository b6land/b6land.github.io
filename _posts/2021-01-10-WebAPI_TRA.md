---
layout: post
title: 建立 WebAPI 的筆記 (使用台鐵公開資料)
date: 2021-01-10 12:00:00 +0800
categories: C#
tags: [C#, ASP.NET MVC]
--- 

這篇文章描述在使用台鐵公開資料建立 Web API 時，所遇到的問題與解決方式。使用技術為 ASP.NET MVC、SQL 與 Entity Framework。

### 加入 WebAPI 至原有的 ASP.NET MVC 專案

以下節錄參考資料的內容並稍作修改。

1. 更新 ASP.NET MVC 至最新版本。
2. 安裝 ASP.NET Web API 相關套件

```
Install-Package Microsoft.AspNet.WebApi
Install-Package Microsoft.AspNet.WebApi.Client.zh-Hant
Install-Package Microsoft.AspNet.WebApi.Core.zh-Hant
Install-Package Microsoft.AspNet.WebApi.WebHost.zh-Hant
```

3. 新增 /App_Start/WebApiConfig.cs 檔案，並加入以下程式碼：

```
public static void Register(HttpConfiguration config)
{
    // Web API 設定和服務

    // Web API 路由
    config.MapHttpAttributeRoutes();

    config.Routes.MapHttpRoute(
        name: "DefaultApi",
        routeTemplate: "api/{controller}/{id}",
        defaults: new { id = RouteParameter.Optional }
    );
}
```

(可能須引用 System.Web.Http 命名空間)

4. 修改 Global.asax.cs 檔案，註冊 ASP.NET Web API 設定：在 Global.asax.cs 檔案最上方引用 `System.Web.Http` 命名空間，並在 `Application_Start()` 方法中的 `AreaRegistration.RegisterAllAreas();` 這行下方加入以下程式

`GlobalConfiguration.Configure(WebApiConfig.Register);`

- 參考資料：[如何在現有 ASP.NET MVC 5 專案上加入 ASP.NET Web API - The Will Will Web](https://blog.miniasp.com/post/2015/02/18/How-to-Add-Web-API-to-ASPNET-MVC)
- 參考資料：[如何讓 ASP.NET Web API 無論任何要求都回應 JSON 格式 - The Will Will Web](https://blog.miniasp.com/post/2013/11/05/Creating-ASPNET-Web-API-Help-Pages-for-ASPNET-MVC-4.aspx)
- 參考資料：[ASP.NET Web API 參數繫結 - Huan-Lin 學習筆記](https://www.huanlintalk.com/2013/01/aspnet-web-api-parameter-binding.html)

### 台鐵時刻表與資料來源

- **取得特定時間、由起站開往終站的當日列車班表**：可從參考資料取得今日預定 (非即時) 的台鐵列車時刻。可以在取回 JSON 資料以後進行剖析，檢查各列車的資料是否包含欲查詢的起點、終點站，並搭配停靠的順序取得結果。
- 參考資料：[【Day30】綜合練習：台鐵即時時刻表！ - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10252411)：使用 AJAX 查詢台鐵時刻表，可以從中瞭解擷取 API 和查詢台鐵資料的概念。
- 參考資料：[PTX OpenAPI Specification (TRA) ](https://ptx.transportdata.tw/MOTC?t=Rail&v=3#/)
- 參考資料：[今日列車時刻表 (JSON)](https://ptx.transportdata.tw/MOTC/v3/Rail/TRA/DailyTrainTimetable/Today?$format=JSON)

### 在 ASP.NET MVC 中取得文字方塊的輸入

- 在 Model 中加入 Input Model、Controller 中加入 Post 方法。當使用者按下按鈕後，送出 POST 訊息至 Controller，再回傳 Input Model 結果至 View。
- 參考資料：[c# - ASP.NET MVC get textbox input value - Stack Overflow](https://stackoverflow.com/questions/18873098/asp-net-mvc-get-textbox-input-value)

### 執行期間內儲存設定

- 為了紀錄是否已下載今日資料 (避免重新下載相同資料)，使用 Web.Config 紀錄並儲存設定，範例如下：

```
<appSettings>
    <add key="TraDate" value="2020/12/01" />
</appSettings>
```

可以使用 `System.Web.Configuration.WebConfigurationManager.AppSettings["TraDate"];` 來存取。
- 一旦停止執行，再重新啟動，先前的設定值就會清空。
- 參考資料：[Best Way to store configuration setting inside my asp.net mvc application - Stack Overflow](https://stackoverflow.com/questions/18044712/best-way-to-store-configuration-setting-inside-my-asp-net-mvc-application)

### 以 JSON 格式輸出查詢結果

- 如果要自訂輸出格式的話，可以透過 `HttpResponseMessage`，並透過在 `Get()` 語法或 _WebApiConfig.cs_ 內設定 `MediaTypeFormater` 自訂輸出格式。語法如下：

```
var appXmlType = config.Formatters.XmlFormatter.SupportedMediaTypes.FirstOrDefault(t => t.MediaType == "application/xml");
    config.Formatters.XmlFormatter.SupportedMediaTypes.Remove(appXmlType);
```

- 輸出時回傳 `Request.CreateResponse(HttpStatusCode.OK, result)` ，result 放入類別的物件或清單即可。

- 類別本身和內部成員都要宣告成 **public** 才能被序列化成 JSON 格式！

- 參考資料: [ASP.NET Web API 如何透過程式決定回應XML或JSON格式 - The Will Will Web](https://blog.miniasp.com/post/2012/10/14/ASPNET-Web-API-HttpResponseMessage-XmlMediaTypeFormatter-JsonMediaTypeFormatter)
- 參考資料: [c# - Remove reference $id from json object list in WebApi - Stack Overflow](https://stackoverflow.com/questions/54826797/remove-reference-id-from-json-object-list-in-webapi/54828472)
- 參考資料: [[C#][ASP.NET] Web API 開發心得 (3) - 統一回應 JSON 格式的資料 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10198146)
- 官方網站範例：[LINQ to JSON - Newtonsoft](https://www.newtonsoft.com/json/help/html/Samples.htm)

