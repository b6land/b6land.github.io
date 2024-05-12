---
layout: post
title: C# 多工處理 - Thread 
date: 2024-05-12 12:00:00 +0800
categories:  [C#]
--- 

這篇文章是講解基礎的執行緒優點、建立方式和需要留意的地方，來自於個人兩年前寫的鐵人賽文章 [Day 14: C# 多工處理: Thread - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10298104)。

### 為什麼需要執行緒

1. 平行計算，以減少執行時間。
2. 在執行持續性的動作時，仍能執行其它動作，例如：從網路上下載檔案時，程式的介面仍然可以操作或反映目前下載進度。

### 建立執行緒的方式

1. 建立一個方法，裡面寫需要執行的邏輯，通常包含 `while` 或 `for` 迴圈，或是其它不斷執行的作業。
2. 建立 `Thread` 類別物件，並傳入剛剛建立的方法作為參數。
3. 呼叫物件的 `Start` 方法，即開始啟動執行緒。
4. 如果需要等待執行緒結束才執行下一個步驟，如觀察結果或是確保資料計算結束，可以呼叫 `Join` 方法。

```cs
int n;

private void AddN(){
    for(int i = 0;i < 50;i++){
        n = n + 1;
    }
}

public void Run(){
    Thread threadAdd = new Thread(AddN); // 傳入要執行的方法
    threadAdd.Start();
    //threadAdd.Join(); // 等待執行緒結束，以觀察結果
    //Console.WriteLine(n.ToString());
}
```

### 寫執行緒要注意什麼 ?

1\. 有多個執行緒同時操作同一份資料時，應該要注意資料的正確性。例如上方的範例程式碼，如果加入第二個執行緒加 N，並把數字調成 500000，並不會如預期 500000 x 2 = 1000000：

```cs
Thread threadAdd = new Thread(AddN);
threadAdd.Start();
Thread threadAdd2 = new Thread(AddN);
threadAdd2.Start();
threadAdd.Join();
threadAdd2.Join();
Console.WriteLine(n.ToString());
```

[範例程式碼](https://github.com/b6land/ithelp_2022_example/blob/8315e2fc5f9492bcd562ff7874dc8cb7278bcce9/Day14_Thread.cs)

這是因為同時對相同資料讀取和寫入，導致有時讀取到非最新的資料，產生的錯誤。遇到同時讀寫相同資料的需求時，寫入時應限定其他執行緒不能讀寫。

2\. 執行緒的暫停或結束，通常需要依靠執行緒本身控制。如果沒控制好，就會導致程式不斷執行，無法正常關閉，或其它工作的處理發生問題。

### 參考資料

[Thread 類別 (System.Threading) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.threading.thread?view=net-6.0)