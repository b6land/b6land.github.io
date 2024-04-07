---
layout: post
title: C# 使用命令列 (.NET CLI) 建立與執行專案
date: 2024-04-07 12:00:00 +0800
categories:  [C#]
--- 

在 .N⁠⁠ET SDK 內包含 .NET CLI，其用途是讓開發人員可以使用命令列建立與執行專案。以下列出常用的指令，更完整的參數和用法說明，可以參考命令列表：[https://learn.microsoft.com/zh-tw/dotnet/core/tools/](https://learn.microsoft.com/zh-tw/dotnet/core/tools/)。

### 建立專案

```powershell
dotnet new <TEMPLATE> --output <OUTPUT_DIRECTORY>
```

其中 `<TEMPLATE>` 可以指定不同專案類型的範本，`<OUTPUT_DIRECTORY>` 是輸出的資料夾名稱，例如

```powershell
dotnet new webapi --output MyProject/
dotnet new classlib --output Service/
```

[https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-new](https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-new)  

### 建立方案與加入專案

建立方案可以方便的管理複數個專案。

```powershell
dotnet new sln
dotnet sln add <PROJECT_PATH>
```

其中 `<PROJECT_PATH>` 是專案的路徑，例如

```powershell
dotnet new sln
dotnet sln add .\MyProject
dotnet sln add .\Service
```

[https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-sln](https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-sln)  

### 建置方案與執行

建置 (編譯) 整個方案，並執行特定的專案。

```powershell
dotnet build
dotnet run --project <PROJECT_PATH>
```

其中 `<PROJECT_PATH>` 是專案的路徑，例如

```powershell
dotnet build
dotnet run --project .\MyProject\
```

[https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-run](https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-run)  

### 專案內加入參考

如果專案之間需要參考的話，可以使用以下命令

```
dotnet add [<PROJECT>] reference <PROJECT_REFERENCES>
```

其中 `<PROJECT>` 是專案的名稱 `<PROJECT_REFERENCES>` 是被參考的專案，例如

```powershell
dotnet add .\MyProject reference .\Service
```

[https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-add-reference](https://learn.microsoft.com/zh-tw/dotnet/core/tools/dotnet-add-reference)  

### 安裝 NuGet 套件

```powershell
dotnet add <PROJECT_PATH> package <PACKAGE_NAME>
```

其中 `<PROJECT_PATH>` 是專案的路徑，`<PACKAGE_NAME>` 是 NuGet 套件的名稱，例如

```powershell
dotnet add .\Service package Refit
```

[https://learn.microsoft.com/zh-tw/nuget/consume-packages/install-use-packages-dotnet-cli](https://learn.microsoft.com/zh-tw/nuget/consume-packages/install-use-packages-dotnet-cli)