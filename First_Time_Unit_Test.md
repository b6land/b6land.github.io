## 單元測試入門

### 為什麼要寫單元測試

當專案越寫越大的時候，很容易遇到修改了 A 功能，卻導致 B 功能出錯的情形。這個狀況通常是未考慮到其它方法 (或這些方法的依賴性) 所導致的，多人合作時容易發生這樣的狀況。

如果建立單元測試的話，就能檢查在測試條件下，程式碼是否會出錯，這會帶來以下的好處：

1. 可以避免修改 A 程式後導致 B 程式出錯的情形。
2. 可以即時的驗證想法是否正確，不用等到上線測試。
3. 單元測試相當於該方法的規格、使用說明書。

### 實作單元測試

1. 在想要測試的方法上點右鍵，選擇「建立單元測試」。在 Visual Studio 2012 及以後版本，不能對非 `public` 方法建立單元測試。
2. 在建立單元測試的視窗中，選擇是否要建立新專案測試或以既有專案測試，以及調整單元測試的格式等。
3. 在建立的測試方法中，預設會包含需傳入的參數和傳出的結果，以及要測試的目標方法。
4. 輸入要測試的參數，呼叫方法，並驗證結果是否與預期相同。
5. 範例的單元測試程式碼如下：

```
[TestMethod()]
public void CheckEducationValidTest_NotPass1()
{
    Education edu = new Education();
    edu.IsFinish = true;
    edu.FinishYear = null;
    edu.Department = "資訊管理系";
    edu.School = "國立中山大學";

    Form1 form1 = new Form1();
    Assert.AreEqual(form1.CheckEducationValid(edu), false);
}
```

在上方的方法中，建立了一個 `Education` 物件，並設定其 `IsFinish` 為 `true`，`FinishYear`為 null。由於聲明已經畢業，卻沒有輸入畢業年份，預期為回傳 False。其呼叫的原始方法如下，會檢查 `School`, `Department` 屬性是否為空白，以及畢業年份是否有正確輸入：

```
public bool CheckEducationValid(Education edu)
{
    if(edu.Department.Equals("") || edu.School.Equals(""))
    {
        return false;
    }
    if((edu.IsFinish && edu.FinishYear == null) || (edu.IsFinish == false && edu.FinishYear != null))
    {
        return false;
    }

    return true;
}
```

接著就可以在單元測試的程式碼上點選右鍵，選擇「執行測試」。測試的結果會放在「測試結果」視窗，可以檢查是否有通過測試，若沒有通過測試，可以再檢查是否原有程式邏輯有誤。

### 參考連結

- [[Day 3]動手寫Unit Test - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10102643)
- [【Unit Test】Day 2 - 如何寫一個好的單元測試 ~ 程式隨筆](https://toyo0103.blogspot.com/2017/04/unit-testday-2.html)
- [【Unit Test】Day 3 - 專注於邏輯，隔離與外部的關聯 ~ 程式隨筆](https://toyo0103.blogspot.com/2017/04/unit-testday-3.html)
- [先寫單元測試的12個好處！ - 轉個彎日誌](https://blog.turn.tw/?p=2821)