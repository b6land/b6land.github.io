---
layout: post
title: C# ToolTip 氣泡提示與 TreeView 樹狀列表
date: 2020-09-12 12:00:00 +0800
categories:  [C#]
--- 

本文介紹在 Winform 中 ToolTip 氣泡提示的彈出方式，與 TreeView 樹狀列表如何強制展開所有節點。

## ToolTip 氣泡提示

### 建立方式

以下的程式碼，可以建立氣泡提示，並設定各項參數，如標題和出現的時間等。最後一行則是設定要出現在哪個元件 (`control`) 上方，但只有元件為焦點時，如輸入中，才會彈出提示。

``` csharp
var toolTip1 = new System.Windows.Forms.ToolTip();
toolTip1.AutoPopDelay = 5000;
toolTip1.InitialDelay = 1000;
toolTip1.ReshowDelay = 500;
toolTip1.ShowAlways = true;
toolTip1.IsBalloon = true;
toolTip1.ToolTipIcon = System.Windows.Forms.ToolTipIcon.Info;
toolTip1.ToolTipTitle = "Title:";

toolTip1.SetToolTip(control, "My Info!");
```

希望在元件不為焦點時也彈出提示的話，可使用 `show()` 的方法。

``` csharp
toolTip1.Show("Tooltip text goes here", (Button)sender);
```

### 參考資料
- 如何建立氣泡提示: [c# - How can I show a Balloon Tip over a textbox? - Stack Overflow](https://stackoverflow.com/questions/7541767/how-can-i-show-a-balloon-tip-over-a-textbox)
- 氣泡提示的彈出方式 1 (當元件為焦點時，才會彈出): 
[ToolTip.SetToolTip(Control, String) Method (System.Windows.Forms)](https://docs.microsoft.com/zh-tw/dotnet/api/system.windows.forms.tooltip.settooltip?view=netcore-3.1)
- 氣泡提示的彈出方式 2 (無論元件是否為焦點，都會彈出): 
[c# - WinForms tooltips not showing up - Stack Overflow](https://stackoverflow.com/questions/27192532/winforms-tooltips-not-showing-up)

## TreeView 樹狀列表

### 建立方式

基礎教學可參考 Dot Net Perls 的介紹。

若要讓 TreeView 強制展開所有節點，可在 TreeView 的 `BeforeCollapse` 事件中使用 `ExpandAll()` 的語法。

### 參考資料
- [C# TreeView Tutorial - Dot Net Perls](https://www.dotnetperls.com/treeview)
- [TreeView.CheckBoxes 屬性 (System.Windows.Forms) - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/api/system.windows.forms.treeview.checkboxes?view=netframework-4.6)
- [c# - Is there a way to make a TreeView appear always fully expanded? - Stack Overflow](https://stackoverflow.com/questions/2813923/is-there-a-way-to-make-a-treeview-appear-always-fully-expanded)