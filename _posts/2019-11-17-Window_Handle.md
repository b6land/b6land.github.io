---
layout: post
title: 使用 Window Handle 撰寫自動化程式
date: 2019-11-17 12:00:00 +0800
categories: C#
tags: [C#]
---

在 Windows 系統上，可以透過操作視窗元件的 Handle ，自動的執行一系列的步驟，且不會像錄製滑鼠 / 鍵盤動作的方式，會受到當時畫面的干擾。

### 關鍵字

- hWnd：常見的 Handle 變數名稱，h 代表 Handle，Wnd 表示 Window，意思為 Handle of Windows。

參考：[Definition of Window Handle](https://social.msdn.microsoft.com/Forums/windows/en-US/a25a604b-d92d-434a-a0f4-5da0180be41b/definition-of-window-handle?forum=winforms)

### 用 C# 操作 Handle

1. 使用 Spy++ 或其它工具，找到特定視窗和元件的 Class 和 Caption。

2. 使用 FindWindow API 找到視窗的 Handle，這些 Handle 其實為視窗元件在記憶體中的指標，因此會以 IntPtr 的方式呈現。FindWindow() 可以用於找視窗的 Handle，FindWindowEx 則可以根據找到的 Handle 尋找子元件的 Handle。

3. FindWindow / FindWindowEx 皆可用於找特定 Class 或(和)特定 Caption (Title) 的視窗。

4. 可使用 SendMessage() 輸入內容至編輯器內。如果要輸入按鍵訊號 (如鍵盤快捷鍵，Ctrl+C 等組合鍵) 的話，應使用 PostMessage()。

{% highlight csharp %}
[DllImport("user32.dll", SetLastError = true)]
static extern IntPtr FindWindow(string lpClassName, string lpWindowName);
 
[DllImport("user32.dll", SetLastError = true)]
public static extern IntPtr FindWindowEx(IntPtr hwndParent,IntPtr hwndChildAfter, string lpszClass, string lpszWindow);
 
[DllImport("user32.dll", SetLastError = true)]
static extern int SendMessage(IntPtr hWnd, int msg, int wParam, StringBuilder lParam);
 
const int WM_SETTEXT = 0x000C;
 
static void Main(string[] args)
{
IntPtr MainWnd = FindWindow("Notepad", null);
IntPtr ChildWnd = FindWindowEx(MainWnd, IntPtr.Zero, "Edit", "");
StringBuilder InsStr = new StringBuilder();
InsStr.Append("ABCDEFGHIJK565465465");
SendMessage(ChildWnd, WM_SETTEXT, 0, InsStr);
{% endhighlight %}

上方程式碼參考自：[C# Win32 API FindWindow/ FindWindowEx/SendMessage – Neil(After Work)](https://neilw.tw/2017/09/04/c-win32-api-findwindow-findwindowexsendmessage/)

5. 要送出按鍵訊號，可透過 PostMessage()，按鍵代碼可參考 Microsoft Docs 提供的特殊按鍵列表 。

{% highlight csharp %}
[DllImport("User32.Dll")]
public static extern Int32 PostMessage(IntPtr hWnd, int msg, int wParam, int lParam);

const int WM_KEYDOWN = 0x0100;
const int VK_TAB = 0x09;

PostMessage(process.MainWindowHandle, WM_KEYDOWN, VK_TAB, 1);
{% endhighlight %}

上方程式碼參考自：[handle - SendMessage Return Key and Text to WindowHandle in C# - Stack Overflow](https://stackoverflow.com/questions/23436339/sendmessage-return-key-and-text-to-windowhandle-in-c-sharp)

特殊按鍵列表：[Virtual-Key Codes - Win32 apps - Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes)

6. 若 Spy++ 的 Class 內容顯示如 "#32770 (Dialog)" 的編號內容，可以直接輸入"#32770"即可。

參考：[Findwindow does NOT find "#32770 (Dialog)" class window](https://social.msdn.microsoft.com/Forums/en-US/7a5ca858-461e-4a64-bf83-69cfcf2bc44e/findwindow-does-not-find-quot32770-dialogquot-class-window?forum=Vsexpressvb)

7. 同樣使用 PostMessage() 送出滑鼠按鈕的訊號，座標的位置使用位元運算設定：

{% highlight csharp %}
int x=58;
int y=32;

PostMessage(this.Handle, WM_LBUTTONDOWN, 0, (x & 0xFFFF) + (y & 0xFFFF) * 0x10000);
PostMessage(this.Handle, WM_LBUTTONUP, 0, (x & 0xFFFF) + (y & 0xFFFF) * 0x10000);

PostMessage(this.Handle, WM_LBUTTONDOWN, 0, x + (y<<16));
PostMessage(this.Handle, WM_LBUTTONUP, 0, x+(y<<16));
{% endhighlight %}

程式碼參考自：[c# 调用win32模拟点击的两种方法 - li-peng - 博客园](https://www.cnblogs.com/li-peng/p/3583771.html)



### 其它參考資料

簡易的 FindWindow 範例：[VS2005程式開發筆記-利用FindWindow( )與SendMessage( )關閉其他視窗程式 @ 楷策部落(Kyanite Blog)](https://blog.xuite.net/kyanite0909/Blog/61011014)

如何送出組合鍵：[PostMessage 向Windows窗口发送Alt组合键 - willen - 博客园](https://www.cnblogs.com/willen/archive/2008/10/22/1316523.html)

如何送出組合鍵 2：[How to Send key-combine to inactive window without active it](https://social.msdn.microsoft.com/Forums/vstudio/en-US/821a5bd3-665c-414e-91c6-c9415dd67bac/how-to-send-keycombine-to-inactive-window-without-active-it?forum=csharpgeneral)