---
layout: post
title: C# 單元測試 (2) 用 NUnit 和 Moq 寫單元測試
date: 2025-06-20 07:00:00 +0800
categories: [C#]
--- 

這篇文章要來介紹 C# 單元測試的主流套件 NUnit、Moq 的語法。

### 什麼樣的專案適合做單元測試

- 專案架構有實作依賴注入 (Dependency Injection, DI)。
- 可以用模擬物件 (mock）。

單元測試應避免受到其他外部模組、資料庫、API 的影響。

### 軟體測試的 3A Pattern

測試方法通常遵循以下 3A 原則：

1. Arrange: 安排要測試的資料。
2. Act: 執行要被測試的方法，並取得結果。
3. Assert: 判斷測試結果是否符合預期。

[ASP.NET MVC 單元測試系列 (1)：新手上路 / 開始撰寫！ - The Will Will Web](https://blog.miniasp.com/post/2010/09/14/ASPNET-MVC-Unit-Testing-Part-01-Kick-off )  

### 安裝套件

可以透過 NuGet 管理員安裝 NUnit 和 Moq，或輸入以下指令：

```bash
dotnet add package NUnit
dotnet add package Moq
dotnet add package Microsoft.NET.Test.Sdk
dotnet add package NUnit3TestAdapter
```

### NUnit 的介紹

NUnit 是一種測試框架，常用的語法如下：

- `Assert`  類別: 判斷測試結果是否符合預期，底下有 `AreEqual` , `IsTrue`  等語法。
- `[Test]`  屬性: 標記是一個測試方法。
- `[TestFixture]`  屬性: 標記是一個測試類別。
- `[TestCase]` 屬性: 可以帶入不同的數值至變數。
- `[Setup]`  屬性: 在執行每個測試前，都會先呼叫標記 Setup 的方法。
- `[Teardown]`  屬性: 每個測試結束後，都會呼叫標記 Teardown 的方法。

#### 參考資料

- 從頭開始建立 NUnit 的單元測試：[使用 NUnit 和 .NET Core 對 C# 進行單元測試 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/core/testing/unit-testing-csharp-with-nunit)
- NUnit 的各種用法：[單元測試的藝術第二章 - 第一個單元測試 - HackMD](https://hackmd.io/@CityChen/ryLjBY-lP)

### Moq 的模擬物件語法

可以用來建立模仿的物件 (假物件的一種)。這個物件裡面所有的方法都被實作，但是沒有實際的功能。

Moq 的常見語法如下：

- `Setup` : 設定如何執行裡面的功能，例如要回傳的值 (Returns)，或是驗證是否被執行過 (Verifiable)。用於 Arrange 階段。
- `Verify` : 可以用來判斷方法是否被執行過。有些方法的結果不是輸出數值，而是執行外部方法，此時這個語法就會很有用。用於 Assert 階段。可以用 `It.IsAny<string>` 讓參數接受任意字串 (`It.IsAny<object>` 設定任意物件)。
- `VerifySet`  : 判斷物件是否被設定特定數值，用於 Assert 階段。

#### 範例語法

```csharp
var mock = new Mock<IMyService>();

// Arrange 階段：設定方法回傳值
mock.Setup(s => s.GetData()).Returns("Hello");

// Arrange 階段：使用 It.IsAny<T>() 接受任意參數
mock.Setup(s => s.Save(It.IsAny<string>())).Returns(true);

// Assert 階段：驗證是否被呼叫
mock.Verify(s => s.SendData(), Times.Once);
```

#### 參考資料

[ASP.NET MVC 單元測試系列 (3)：瞭解 Mock 假物件 ( moq ) - The Will Will Web](https://blog.miniasp.com/post/2010/09/16/ASPNET-MVC-Unit-Testing-Part-03-Using-Mock-moq)  

[c# - Verify a method call using Moq - Stack Overflow](https://stackoverflow.com/questions/9136674/verify-a-method-call-using-moq)  

[c# - Using Moq to set any by any key and value - Stack Overflow](https://stackoverflow.com/questions/6294234/using-moq-to-set-any-by-any-key-and-value)

### 小技巧

1. 對條件式 (如 if/else) 的回傳結果，建立不同的單元測試。
2. 測試專案命名：`MyApp.Tests` 。
3. 測試類別應與被測類別對應，如：`OrderServiceTests` 對應 `OrderService` 。
4. 單元測試的方法名稱，可以遵守以下格式：`方法名稱_測試條件_預期結果` 。用中文撰寫，可以更快確認測試的作用。

```csharp
public bool Login_InvalidPassword_ReturnsFalse() {}
public bool Login_密碼錯誤_回傳False() {}
```

### 單元測試中手動注入私有欄位

如果變數是在建構式裡建立，不是被注入的，可以這樣做：

```csharp
typeof(XXXService) 
// 取得 XXXService 類型的 Type 物件，讓我們可以進一步操作它的欄位。
.GetField("_api", System.Reflection.BindingFlags.NonPublic | System.Reflection.BindingFlags.Instance) 
// 從 XXXService 類別中取出名稱為 _api 的 非公開（private）實體欄位。
//  BindingFlags.NonPublic: 包含 private 欄位。
//  BindingFlags.Instance: 只找實例欄位（不找 static）。
?.SetValue(_service, _mockApi.Object);
// 如果 _api 欄位有找到，就將它設為 _mockApi.Object（你模擬出來的物件）。
```

用反射強行修改 `_api`，把它換成 `Moq` 建立的假物件。

不過這是「破壞封裝性」的做法，建議可以：

- 把 `_api` 改成透過 DI 用 constructor 傳入。
- 或者提供一個 internal setter 屬性專門給測試用。

### 想要測試 Private 方法

1. 使用 `delegate`  或 `Func<int, (bool, string)>`  注入方式替代。
2. 改為 `internal`  + `InternalsVisibleTo`  給測試專案，並在專案檔 (.csproj) 加上這行：

```csharp
[assembly: InternalsVisibleTo("MyApp.Tests")]
```