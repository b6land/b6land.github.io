---
layout: post
title: 使用 JSFiddle 測試 JavaScript (含 jQueryUI)
date: 2024-01-14 12:00:00 +0800
categories:  [JavaScript]
--- 

[JSFiddle](https://jsfiddle.net/) 是一個可以測試網頁執行效果的網站，本篇記錄使用該網站的方式 (含挑選 jQueryUI 選項)。

### JSFiddle 的使用方式

![JSFiddle](/assets/imgs/2024-01-14/jsFiddle.png)

1\. 左上 HTML 文字框可以填入網頁中的 Body 標籤內容。

2\. 右上 CSS 文字框可以填入樣式 (通常是 Style 標籤內容)。

3\. 左下 JavaScript 文字框填入 JavaScript (通常是 Script 標籤內容)。

4\. 按下左上角的 Run 可以取得目前內容在右下角的預覽結果。

![想使用的 Framework 組合](/assets/imgs/2024-01-14/pick_framework.png)

5\. JavaScript 處可以選擇想要使用的 Framework 和擴充套件。如果有應用 jQueryUI 的語法，可選擇 jQuery 1.9.1 ，並開啟下方的 jQuery UI 1.9.2。(其它版本的 jQuery 不一定可使用 jQuery UI)

### 範例：踩地雷遊戲

以下利用 ChatGPT 產生的踩地雷遊戲作為示範：

1\. 詢問後，會分別得到 HTML 程式碼、CSS 樣式表和 JavaScript 程式碼。首先將以下程式碼填入左上的 HTML 文字框：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>踩地雷</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>踩地雷遊戲</h1>
    <div id="game-container"></div>
    <div id="message"></div>
    <script src="scripts.js"></script>
</body>
</html>

```

2\. 接著將 css 貼到右上文字框。

```css
body {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
}

h1 {
    margin-bottom: 20px;
}

#game-container {
    display: grid;
    grid-template-columns: repeat(10, 40px);
    grid-gap: 2px;
}

.cell {
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ddd;
    border: 1px solid #ccc;
    font-size: 18px;
    cursor: pointer;
}

.cell.revealed {
    background-color: #bbb;
    cursor: default;
}

.cell.bomb {
    background-color: red;
}

.cell.flag {
    background-color: yellow;
}

#message {
    margin-top: 20px;
    font-size: 24px;
    color: green;
}

```

3\. 然後再將 JavaScript 貼到左下方，因為是原生的 JavaScript，選擇「JavaScript + No-Library (pure JS)」 即可。

```javascript
document.addEventListener("DOMContentLoaded", () => {
    const gameContainer = document.getElementById("game-container");
    const messageElement = document.getElementById("message");
    const gridSize = 10;
    const numBombs = 20;
    const cells = [];
    const bombs = new Set();
    let revealedCount = 0;

    // Initialize game
    function init() {
        // Create cells
        for (let i = 0; i < gridSize * gridSize; i++) {
            const cell = document.createElement("div");
            cell.classList.add("cell");
            cell.dataset.index = i;
            cell.addEventListener("click", handleClick);
            cell.addEventListener("contextmenu", handleRightClick);
            cells.push(cell);
            gameContainer.appendChild(cell);
        }

        // Place bombs
        while (bombs.size < numBombs) {
            const bombIndex = Math.floor(Math.random() * gridSize * gridSize);
            bombs.add(bombIndex);
        }

        // Calculate numbers
        cells.forEach((cell, index) => {
            const isBomb = bombs.has(index);
            cell.dataset.isBomb = isBomb;
            if (!isBomb) {
                const bombCount = getAdjacentCells(index).filter(i => bombs.has(i)).length;
                cell.dataset.bombCount = bombCount;
            }
        });
    }

    // Handle cell click
    function handleClick(e) {
        const index = parseInt(e.target.dataset.index);
        if (bombs.has(index)) {
            revealBombs();
            messageElement.textContent = "Game Over!";
        } else {
            revealCell(index);
            checkWin();
        }
    }

    // Handle right click (flag)
    function handleRightClick(e) {
        e.preventDefault();
        const cell = e.target;
        if (!cell.classList.contains("revealed")) {
            cell.classList.toggle("flag");
        }
    }

    // Reveal a cell
    function revealCell(index) {
        const cell = cells[index];
        if (cell.classList.contains("revealed") || cell.classList.contains("flag")) {
            return;
        }
        cell.classList.add("revealed");
        revealedCount++;
        if (cell.dataset.bombCount > 0) {
            cell.textContent = cell.dataset.bombCount;
        } else {
            getAdjacentCells(index).forEach(revealCell);
        }
    }

    // Reveal all bombs
    function revealBombs() {
        bombs.forEach(index => {
            const cell = cells[index];
            cell.classList.add("revealed", "bomb");
        });
    }

    // Get adjacent cells
    function getAdjacentCells(index) {
        const adjacent = [];
        const row = Math.floor(index / gridSize);
        const col = index % gridSize;

        for (let i = -1; i <= 1; i++) {
            for (let j = -1; j <= 1; j++) {
                if (i === 0 && j === 0) continue;
                const newRow = row + i;
                const newCol = col + j;
                if (newRow >= 0 && newRow < gridSize && newCol >= 0 && newCol < gridSize) {
                    adjacent.push(newRow * gridSize + newCol);
                }
            }
        }

        return adjacent;
    }

    // Check if player has won
    function checkWin() {
        if (revealedCount === gridSize * gridSize - numBombs) {
            messageElement.textContent = "You Win!";
            gameContainer.removeEventListener("click", handleClick);
        }
    }

    init();
});

```

4. 按下左上角的「Run」按鈕，就可以在右下方玩踩地雷遊戲囉！

![JSFiddle 上的踩地雷遊戲](/assets/imgs/2024-01-14/jsFiddle_minesweeper.png)