---
layout: post
title: C# 使用 SqlParameter 加入查詢條件
date: 2023-12-25 12:00:00 +0800
categories: [C#]
---

C# 中有 SqlCommand 類別可以放入 Raw SQL，查詢時可以使用 SqlParamter 類別加入查詢的條件，並避免 SQL Injection 導致的資安風險。

### 使用方式

SQL 查詢語法中，為要加入的查詢條件設定名稱，並在前方加上 `@` 符號。

用 `SqlCommand` 類別設定查詢語法，並在 `Parameters.Add` 方法傳入 `SqlParameter`。第一個參數為查詢條件的名稱，第二個參數為查詢的變數。

若傳入的變數內容包含 SQL Injection 的控制符號時，會拋出例外。

以下是範例：

``` cs
SqlCommand command = new SqlCommand("SELECT * FROM Dogs1 WHERE Name LIKE @Name", connection);
command.Parameters.Add(new SqlParameter("Name", dogName));
```

另外，因為 SqlParameter 實作 DbParameter 抽象類別，當方法需要傳入 DbParameter 時，可以直接建立 SqlParameter。從以下範例中，可看到建立 SqlParameter 時直接指定屬性名稱、值。

```csharp
var parameters = new[]{
            new SqlParameter(){ ParameterName="foo", Value="hello" },
            new SqlParameter(){ ParameterName="bar", Value="World" }
        };
x.ExecuteScalar<int>(commandText, commandType, parameters);
```

### 如何在 IN 條件加入參數

如果今天要查詢的 SQL 語法，裡面的條件包含用 `IN` 查詢多個條件的話，可以自己組多個參數和查詢語法。

StackOverflow 上的解法如下：

```cs
string[] values = new [] {"value1", "value2", "value3", "value4"};
  StringBuilder query = new StringBuilder("Select * From Table Where Column in (");
  SqlCommand cmd = new SqlCommand();
  cmd.Connection = new SqlConnection("Your connection string");
  for(int i = 0; i < columns.Length; i++)
  {
      string arg = string.Format("@arg{0}", i);
      cmd.Parameters.AddwithValue(arg, SanatizeSqlString(columns[i]));
      sb.AppendFormat("{0}, ", arg);
  }
  sb = sb.Remove(sb.Length -2, 2);
  sb.Append(")");
  cmd.CommandText = sb.ToString();
```

最後的查詢語法呈現如下：

```sql
Select * From Table Where Column in (@arg0, @arg1, @arg2, @arg3)
```

### 參考資料

- 更詳細的用法：[C# SqlParameter Example - Dot Net Perls](https://www.dotnetperls.com/sqlparameter)
- DbParameter 的使用：[c# - How to create array DbParameter\[\] - Stack Overflow](https://stackoverflow.com/questions/7097624/how-to-create-array-dbparameter )
- IN 的使用：[c# 2.0 - Using an 'IN' operator with a SQL Command Object and C# 2.0 - Stack Overflow](https://stackoverflow.com/questions/400819/using-an-in-operator-with-a-sql-command-object-and-c-sharp-2-0)