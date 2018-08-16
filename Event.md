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

### 在 Static Class 實作 Event

- 和一般 Event 的宣告類似，額外加入 `static` 關鍵字即可。
- 在 invoke 時使用 null 代替 : `SomeEvent(null, EventArgs.Empty);`。
- 小心使用。除非物件取消訂閱事件 (Unsubscribe event handler from instance ) ，否則 Garbage Collect 機制不會回收物件，將其生命週期視為和 static class 相同。

### Event 特性

- Event delegates are multicast.

### 參考資料

- [How to: Implement Events in Your Class](https://msdn.microsoft.com/en-us/library/5z57dxz2(v=vs.85).aspx)
- [c# - How to raise custom event from a Static Class - Stack Overflow](https://stackoverflow.com/questions/289002/how-to-raise-custom-event-from-a-static-class)
- [Events and Delegates](https://msdn.microsoft.com/en-us/library/17sde2xt(v=vs.85).aspx)