---
layout: post
title: JavaScript 變數宣告
date: 2025-01-31 22:00:00 +0800
categories: [JavaScript]
---  

以下是 JavaScript 宣告變數的規則與例子。

### var 宣告變數

- `var` 具有 **Hoisting（提升）** 行為，變數在編譯階段即被提升，相當於變數被宣告在程式的最開頭，因此可以在宣告前使用，但值為 `undefined`。
- `var` **可以重複宣告**，新的宣告若有指定值，會覆蓋舊的值。
- `var` 宣告的變數屬於 **函式範圍（Function Scope）**，若在函式內宣告，則變數僅在該函式內有效，否則會變成全域變數。
- 變數提升的例子：

```javascript
console.log(a); // undefined
var a = 10;
console.log(a); // 10
```

### 函式宣告（Function Declaration）

- **函式宣告（function declaration）** 也會發生 **Hoisting**，與 `var` 不同的是，函式不僅提升宣告，也會提升其完整的定義，故可在宣告前呼叫。
- 例子：

```javascript
console.log(foo()); // "Hello"
function foo() {
    return "Hello";
}
```

### let 與 const 宣告變數

- `let` 和 `const` **不會被提升到程式的最開頭**，但仍然會被 Hoisting，只是它們會進入「**暫時性死區（Temporal Dead Zone, TDZ）**」，在變數宣告之前無法存取，否則會拋出 `ReferenceError`。
- `let` 允許變數重新賦值，但 **不能重複宣告**。
- `const` 代表 **常數（constant）**，必須在宣告時初始化，且無法重新賦值。
- `let` 和 `const` **屬於區塊範圍（Block Scope）**，即 `{}` 內的變數不會影響區塊外的變數。
- 例子：

```javascript
console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 20;
console.log(b); // 20

const PI = 3.14159;
PI = 3.14; // TypeError: Assignment to constant variable.
```

### 參考資料

- [\[教學\] JavaScript Hoisting 是什麼？ let, const, var 的差異是什麼？](https://shubo.io/javascript-hoisting/)
- ChatGPT