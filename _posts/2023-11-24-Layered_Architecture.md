---
layout: post
title: .NET 8 分層模式簡介與實作範例
date: 2023-11-24 12:00:00 +0800
categories:  [ASP.NET, Interview]
--- 

分層模式表示軟體結構分成多層，不過沒有規定一定要分成幾層，以及定義每一層所扮演的角色。以下將介紹常見的分層模式，並附上實作範例。

## 分層模式介紹

常見的分層模式將程式分成：

1. 展示層 (Presentation Layer): 使用者介面、檢視。 (可能也包含路由的控制)
2. 業務層 (Business Layer): 負責將從持久層取得資料，執行商業邏輯，將其輸出至展示層。
3. 持久層 (Persistence Layer): 存取資料庫或檔案的永久性的儲存媒介，進行更新、刪除、讀取、建立。
4. 資料存取層 (Data Access Layer): 資料庫、第三方 API 等外部資料來源，通常獨立於應用程式。

### 優點

1. 便於單獨對各層的元件測試。
2. 易於理解與開發，資料流主要由上往下，便於提出需求與修改。
3. 各元件容易分隔出來修改。

### 缺點

1. 如果每一層都只有處理很少的邏輯，只是單純讓資料從儲存媒介流至使用者介面，使用分層模式只會增加複雜度 (常見於小型的專案)。
2. 當專案越來越大時，會成為一個巨大的單體應用 (Monolithic application)，各層之間仍然有一定的耦合度。

## 實作範例

接下來會建立一個不含使用者介面、省略資料存取層的 WebAPI ，來示範分層模式的架構。

在這個範例裡面，只有使用資料夾分層。當專案規模較大時，可考慮將每一層建立為一個專案 (Project)，並使用 Visual Studio 的方案 (Solution) 來管理，以增加管理和分工的彈性。

以下的程式用 .net 8.0 作為範例。

### 撰寫檔案

#### `Controller.cs` - 展示層

```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;

namespace ExampleApp.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class ProductsController : ControllerBase
    {
        private readonly IProductService _productService;

        public ProductsController(IProductService productService)
        {
            _productService = productService;
        }

        [HttpGet]
        public ActionResult<IEnumerable<Product>> GetAllProducts()
        {
            var products = _productService.GetAllProducts();
            return Ok(products);
        }
    }
}
```

#### `IService.cs` - 業務層介面

```cs
using System.Collections.Generic;

namespace ExampleApp
{
    public interface IProductService
    {
        IEnumerable<Product> GetAllProducts();
    }
}

```

#### `Service.cs` - 業務層實作

```cs
using System.Collections.Generic;

namespace ExampleApp
{
    public class ProductService : IProductService
    {
        private readonly IProductRepository _productRepository;

        public ProductService(IProductRepository productRepository)
        {
            _productRepository = productRepository;
        }

        public IEnumerable<Product> GetAllProducts()
        {
            // 如果有其它商業邏輯，可以寫在這裡
            return _productRepository.GetAllProducts();
        }
    }
}

```

#### `IRepository.cs` - 持久層介面

```cs
using System.Collections.Generic;

namespace ExampleApp
{
    public interface IProductRepository
    {
        IEnumerable<Product> GetAllProducts();
    }
}

```

#### `Repository.cs` - 持久層實作

```cs
using System.Collections.Generic;

namespace ExampleApp
{
    public class ProductRepository : IProductRepository
    {
        // 一般來說會在這裡查詢資料庫或其它資料來源，在此用 List 模擬資料表取回的結果
        private readonly List<Product> _products = new List<Product>
        {
            new Product { Id = 1, Name = "Product 1", Price = 10 },
            new Product { Id = 2, Name = "Product 2", Price = 20 }
        };

        public IEnumerable<Product> GetAllProducts()
        {
            return _products;
        }
    }
}

```

#### Product Model - 資料物件

```cs
namespace ExampleApp
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```

### 建立專案

1. 安裝 .NET SDK

如果尚未安裝，請先從 [Microsoft .NET](https://dotnet.microsoft.com/zh-tw/download) 官網下載並安裝 .NET SDK。

2. 建立新的 ASP.NET Core Web API 專案

打開終端機（命令提示字元或 PowerShell），執行 `dotnet new webapi -n ExampleApp` 指令，會在資料夾下建立一個名為 ExampleApp 的 .NET 8 Web API 專案。

3. 進入專案目錄

4. 建立分層式結構

分別建立下列資料夾與檔案，並將剛剛的程式碼填入對應的檔案：

- Controllers 資料夾: 放置展示層的 `ProductsController.cs`。
- Services 資料夾: 放置業務層的 `IProductService.cs` 和 `ProductService.cs`。
- Repositories 資料夾: 放置持久層的 `IProductRepository.cs` 和 `ProductRepository.cs`。
- Models 資料夾: 放置資料模型 `Product.cs`。

5. 新增、註冊分層架構

在專案主資料夾找到 `Program.cs`，並加入剛剛的 Controller，再註冊 Service 和 Repository。

```cs
using ExampleApp;

var builder = WebApplication.CreateBuilder(args);

// 其它的程式碼

// 在 builder.Build() 前，先新增、註冊分層架構
builder.Services.AddControllers();
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddScoped<IProductRepository, ProductRepository>();

var app = builder.Build();

// 其它的程式碼

// 在 app.Run() 之前，先建立剛剛新增 Controller 的 API 路由
app.MapControllers();
app.Run();
```
6. 建置和執行專案

呼叫 `dotnet run`，預設會幫你建置專案並直接啟動。

執行後會出現類似訊息：

```
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5105
```

可以透過 API 工具 (例如從 Postman 建立 Get 請求) 或瀏覽器輸入網址，檢查 `http://localhost:5105/api/products`，應該能看到一個包含產品的 JSON 列表。

```json
[
  {
    "id": 1,
    "name": "Product 1",
    "price": 10
  },
  {
    "id": 2,
    "name": "Product 2",
    "price": 20
  }
]
```

## 參考資料

- [軟體架構：分層架構模式 ( Layered Architecture ) - 程式愛好者 - Medium](https://medium.com/程式愛好者/軟體架構-分層架構模式-layered-architecture-a959da09d1c6)
- [軟體架構-關於分層 - 老E隨手寫 - 點部落](https://dotblogs.com.tw/bda605/2021/01/19/103355)
- 另一種分層的方式: [[Day 16] 剖析分層架構 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10291824)
- 三層的分層模式介紹: [菜雞新訓記 (5): 使用 三層式架構 來切分服務的關注點和職責吧 - 伊果的沒人看筆記本](https://igouist.github.io/post/2021/10/newbie-5-3-layer-architecture/)
- 部分程式碼由 [ChatGPT](https://chatgpt.com/) 產生
- 進一步閱讀：[1. Layered Architecture - Software Architecture Patterns [Book]](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html)