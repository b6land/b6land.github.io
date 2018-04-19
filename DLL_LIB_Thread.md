**DLL 和 LIB 的差異性**

呼叫他人所寫的函式時，編譯器會經由連結這個動作，找尋 Library 內所需要的程式碼。

Windows 的 Library 分為 2 類：

- DLL - Win32 Dynamic-Link Library，當程式執行時，在需要的時候動態載入程式碼。
- LIB - Win32 Static Library，在開發程式時，即將程式碼包入執行檔中。

參考資料 : 

[.lib 和 .dll 的不同處在哪裡呢？](http://www.programmer-club.com.tw/ShowSameTitleN/vc/6921.html)

**在 Visual Studio 載入 LIB 的方式**

1. 利用 `#pragma comment(lib, "../../xxx.lib")`，優點是不用擔心忘記加入 Library 至 (不同的 Build) 專案設定內，缺點是有些情形下會難以除錯連結器 (Linker) 相關的問題。
2. 使用 Visual Studio 的專案設定。
3. 將使用的 LIB 專案加入至*依賴專案*內。

參考資料 : 

[Library importing: #pragma comment VS Visual studio project input](https://stackoverflow.com/questions/6287338/library-importing-pragma-comment-vs-visual-studio-project-input)

**Thread Safe**

包在 DLL 內部的程式碼，如果變數會被多個執行緒存取，那麼它就不符合 Thread Safe 的性質。在 DLL 內部使用多執行緒，或在外部使用多執行緒呼叫 DLL 的程式碼，都可能會導致資料出錯。因此應該在使用前檢查 Library 的說明文件。

如果使用了 Global 變數，在同一個 Process 內，所有的執行緒都會共用這個變數，進而導致資料錯誤存取。

參考資料 : 

[How to make DLL routines thread safe?](https://software.intel.com/en-us/forums/intel-visual-fortran-compiler-for-windows/topic/279905)

[When I call C++ code from C# code, is it thread-safe?](https://stackoverflow.com/questions/20705116/when-i-call-c-code-from-c-sharp-code-is-it-thread-safe)

[[問題] Load dll問題請教](https://www.ptt.cc/bbs/C_and_CPP/M.1446654673.A.042.html)

[How to create Multithreaded Dll](https://stackoverflow.com/questions/16896436/how-to-create-multithreaded-dll)