---
layout: post
title: .NET 6 實作 JWT (使用 Minimal API)
date: 2025-04-28 21:00:00 +0800
categories:  [ASP.NET]
---

這篇文章會介紹 JWT 的基本知識，以及用 .NET 6 的 Minimal API 打造一個簡易的程式範例。

### 基本知識

- 全名是 JSON Web Token，顧名思義是將安全資訊存為 JSON 物件。
- JWT 是由後端產生，用於驗證身分的 Token，產生後會交給前端存放。
- 分成 Header, Payload, Signature 三個部分，Header 存放通用資訊 (例如使用的演算法)、Payload 存放使用者資訊 (例如用戶帳號)、Signature 存放數位簽名。
- 透過 Base64 編碼 Header 和 Payload，所以建議存放非機密資訊，因為可以反向解碼。
- 可以由「對稱加密」或「非對稱加密」實作數位簽名功能。
    - 對稱加密：用同一把公鑰加密、解密資料。
    - 非對稱加密：用私鑰加密的資料，只能用公鑰來解密。

### 使用對稱加密的 .NET 6 範例

以下是一個最小可行的 C# 專案（使用 .NET 6/7 Minimal API）用來測試 JWT Token 的產生與驗證。這個範例會包含：

1. 產生 JWT Token 的 API (`/login`)
2. 一個需要驗證 JWT 的 API (`/secure`)
3. 使用內建 JWT 驗證中介軟體

建立新資料夾，接著開啟 PowerShell，輸入指令建立 ASP.NET Core 專案，並安裝必要的套件：

```bash
dotnet new web
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

接著建立 `Program.cs` ：

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;

var builder = WebApplication.CreateBuilder(args);

// JWT 設定
var jwtKey = "1234567890asfdghjklmnbvcxzqwertyuiop[]!@"; // 公鑰內文，請改成更安全的字串
var issuer = "MyApp";

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme) // 設定驗證合法有效的 Token
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = false,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = issuer,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwtKey)) // 建立對稱式的公鑰
        };
    });
builder.Services.AddAuthorization();

var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();

// 產生 Token 的 Login API
app.MapPost("/login", (UserLogin user) =>
{
    if (user.Username == "test" && user.Password == "1234") // 這裡是模擬登入成功
    {
        var claims = new[]
        {
            new Claim(ClaimTypes.Name, user.Username)
        };

        var token = new JwtSecurityToken( // 產生 JWT
            issuer: issuer,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(1), // 逾期時間，暫時設定為 1 分鐘
            signingCredentials: new SigningCredentials(
                new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwtKey)),
                SecurityAlgorithms.HmacSha256)
        );

        var tokenString = new JwtSecurityTokenHandler().WriteToken(token);

        return Results.Ok(new { token = tokenString });
    }

    return Results.Unauthorized();
});

// 受保護的 API
app.MapGet("/secure", (ClaimsPrincipal user) =>
{
    return $"Hello {user.Identity?.Name}, you accessed a protected endpoint!";
}).RequireAuthorization();

app.Run();

// 使用者登入資料紀錄 (可參考 https://www.huanlintalk.com/2022/03/csharp-9-record-explained.html)
record UserLogin(string Username, string Password);
```

接著在命令列輸入以下指令啟動程式：

```bash
dotnet run
```

然後可以在 Postman 或類似軟體發送 POST 請求到 Login API：

```json
{
  "username": "test",
  "password": "1234"
}
```

就可以得到回傳的 JWT。

![呼叫登入 API 成功](/assets/imgs/2025-04-28/jwt_login.png)<br>

接著再建立新的 GET 請求，驗證方式選擇 Bearer，並輸入剛剛的 JWT，發送後就能得到驗證成功的結果囉。 

![呼叫受保護的 API 成功](/assets/imgs/2025-04-28/jwt_secure_ok.png)<br>

如果輸入錯誤或逾時的話，會得到 401 未認證的回應：

![呼叫受保護的 API 逾時](/assets/imgs/2025-04-28/jwt_secure_unauthorized.png)

### Bonus: JWT Token 沒有立刻失效

如果把逾期時間設得很短的話，可能會發現 Token 沒有立刻失效。這是因為 Clock Skew (時鐘偏移) 的設定，有著 5 分鐘的容許值，以接受不同伺服器略有差異的時間。如果不需要容許值的話，可以調整 `TokenValidationParameters`  的 [ClockSkew](https://learn.microsoft.com/en-us/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters.clockskew?view=msal-web-dotnet-latest#microsoft-identitymodel-tokens-tokenvalidationparameters-clockskew) 為 `TimeSpan.Zero` 。

參考資料：[c# - JWT token expiration not working in Asp.Net Core API? - Stack Overflow](https://stackoverflow.com/questions/61909514/jwt-token-expiration-not-working-in-asp-net-core-api)  

### 參考資料：其它 ASP.NET API 加入 JWT 驗證

想用 MVC API 或其它 ASP.NET Core API 版本加入 JWT，可以參考以下連結：


- [\[Day24\] C# MVC API JWT Token驗證 (上) JWT基礎知識 - C#&AspNetCore - UM's Coding Note](https://yuhsiang237.github.io/2021/10/27/C-MVC-API-Bearer-Token%E9%A9%97%E8%AD%89/)
- [\[Day25\] C# MVC API JWT Token驗證 (下) JWT實作 - C#&AspNetCore - UM's Coding Note](https://yuhsiang237.github.io/2021/10/28/C-MVC-API-Bearer-Token%E5%AF%A6%E4%BD%9C/)
- [如何在 ASP.NET Core 6 使用 Token-based 身份認證與授權 (JWT) - The Will Will Web](https://blog.miniasp.com/post/2022/02/13/How-to-use-JWT-token-based-auth-in-aspnet-core-60)
- [在 Swagger UI 加上驗證按鈕，讓 Request Header 傳遞 Authorize Token - 伊果的沒人看筆記本](https://igouist.github.io/post/2021/10/swagger-enable-authorize/)


### 參考資料：古古的「JWT Token 原理」系列文

Note: 這系列文章的數位簽名是採用「非對稱加密」。

- [Session 和 JWT 的差別在哪裡？](https://kucw.io/blog/session-vs-jwt/)
- [密碼學中的 Encode、Encrypt、Hash 的差別在哪裡？](https://kucw.io/blog/encode-encrypt-hash-intro/ )
- [JWT 是什麼？一次搞懂 JWT 的組成和運作原理](https://kucw.io/blog/jwt/)
- [JWT 是如何實作「數位簽名」的？揭秘 signature 的面紗](https://kucw.io/blog/jwt-signature/ )