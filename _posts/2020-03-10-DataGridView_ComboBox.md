---
layout: post
title: 在 DataGridView 中加入 ComboBox
date: 2020-03-10 12:00:00 +0800
categories: C# 
tags: [C#]
---

本文介紹 C# Winform 中的 DataGridView 如何增加 ComboBox 欄位。

### 在 DataGridView 中加入 ComboBox

- 從程式碼動態加入 DataGridViewComboBoxColumn 至 DataGridView 中。這是一種可以用於 DataGridView 的 ComboBox 元件。
- DataGridViewComboBoxColumn 的事件偵測，需先設定 DataGridView 本身的 EditingControlShowing 事件，並在事件內部再設定 ComboBox 的事件，如 SelectedIndexChanged。

### 程式碼

以下取自參考資料中的 Microsoft Docs。

```
private DataGridView dataGridView1 = new DataGridView();

private void AddColorColumn()
{
    DataGridViewComboBoxColumn comboBoxColumn =
        new DataGridViewComboBoxColumn();
    comboBoxColumn.Items.AddRange(
        Color.Red, Color.Yellow, Color.Green, Color.Blue);
    comboBoxColumn.ValueType = typeof(Color);
    dataGridView1.Columns.Add(comboBoxColumn);
    dataGridView1.EditingControlShowing +=
        new DataGridViewEditingControlShowingEventHandler(
        dataGridView1_EditingControlShowing);
}

private void dataGridView1_EditingControlShowing(object sender,
    DataGridViewEditingControlShowingEventArgs e)
{
    ComboBox combo = e.Control as ComboBox;
    if (combo != null)
    {
        // Remove an existing event-handler, if present, to avoid 
        // adding multiple handlers when the editing control is reused.
        combo.SelectedIndexChanged -=
            new EventHandler(ComboBox_SelectedIndexChanged);

        // Add the event handler. 
        combo.SelectedIndexChanged +=
            new EventHandler(ComboBox_SelectedIndexChanged);
    }
}

private void ComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    ((ComboBox)sender).BackColor = (Color)((ComboBox)sender).SelectedItem;
}
```

### 參考資料
[DataGridViewComboBoxEditingControl 類別 (System.Windows.Forms) - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/api/system.windows.forms.datagridviewcomboboxeditingcontrol?view=netframework-4.8)

(另一種解決方式) [Is there a DataGridView event when a combobox ... [SOLVED] - DaniWeb](https://www.daniweb.com/programming/software-development/threads/452140/is-there-a-datagridview-event-when-a-combobox-value-changes)