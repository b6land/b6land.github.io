## 為何不使用 Region 分隔程式

- 這篇文章是翻譯連結內的答案： [c# - Why are people so strongly opposed to #region tags in methods? - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/118818/why-are-people-so-strongly-opposed-to-region-tags-in-methods)
- 將程式分為獨立的方法可被測試，但是以 `region` 分段的話不行。
- Region 和原本的語法可能會互相衝突 (例如 `region` 插入在 `if` 前面，`endregion` 插入在 `if` 內。)。
- 遇到沒有程式碼折疊的編輯器時，只能看到一大串又臭又長的程式碼。因此在寫程式碼時，應時常改寫，將程式碼分隔開來。