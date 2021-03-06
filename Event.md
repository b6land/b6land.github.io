## Event 與 Event Handler 的介紹與使用

本篇主要介紹 C# 中 Event 和 Event Handler 的特性與使用方式。

### 為什麼要實作 Event

- 因為可以降低程式模組間的耦合性。
- 是一種觀察者模式的實作：觀察者模式中包含發佈者/訂閱者，發佈者要「註冊」和「通知」眾多的訂閱者。訂閱者要提供發佈者一個方法發送通知，發佈者則實作通知的方法。
- 參考資料：[Unity 事件機制淺談 (C# events, unity events)](https://dev.twsiyuan.com/2017/03/c-sharp-event-in-unity.html)
- 參考資料：[觀念 C# — EventHandler - 我，傑夫。開發人](https://jeffprogrammer.wordpress.com/2015/07/29/觀念-c-eventhandler/)

### 實作 Event (不含特定 Event 資料)

1\. 宣告一個 public event handler。

`public event EventHandler CountdownCompleted;  `

2\. 宣告一個 event 發生的函式，函式名稱通常以 **On** 開頭。

```
protected virtual void OnCountdownCompleted(EventArgs e)
{
    if (CountdownCompleted != null)
        CountdownCompleted(this, e);
}
```

3\. 在需要發生事件的地方進行呼叫。

`OnCountdownCompleted(new EventArgs());`

- 參考資料：[How to: Implement Events in Your Class](https://msdn.microsoft.com/en-us/library/5z57dxz2(v=vs.85).aspx)

### 在 Static Class 實作 Event

- 和一般 Event 的宣告類似，額外加入 `static` 關鍵字即可。
- 在 invoke 時使用 null 代替 : `SomeEvent(null, EventArgs.Empty);`。
- 小心使用。除非物件取消訂閱事件 (Unsubscribe event handler from instance ) ，否則 Garbage Collect 機制不會回收物件，將其生命週期視為和 static class 相同。
- 參考資料：[c# - How to raise custom event from a Static Class - Stack Overflow](https://stackoverflow.com/questions/289002/how-to-raise-custom-event-from-a-static-class)

### Event 特性

- Event delegates are multicast. 當事件發生時，會觸發所有委派的方法。
- 參考資料：[Events and Delegates](https://msdn.microsoft.com/en-us/library/17sde2xt(v=vs.85).aspx)