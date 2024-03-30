---
layout: post
title: C# Switch Case When
date: 2024-03-30 12:00:00 +0800
categories:  [C#]
--- 

在 C# 7.0 以後，Switch 敘述內，除了原本的 Case ，現在可以用 When 指定其它條件。

### 介紹

原本的 Switch Case 只能檢查一個變數的狀態，現在可以在每個 Case 的後方加入 When，接著判斷第二個變數的狀態。在官方文件中，稱之為 Case Guard。

可以混用多種不同的比對模式 (Pattern)，使 Switch Case When 能檢查各種不同的狀態。

### 範例

```cs
public class SwitchCaseWhenTest{

    /// <summary>
    /// 測試用動物類別
    /// </summary>
    public class Animal{
        public string? name { get; set; }
        public bool isFly {get; set; }
        public bool isWalk {get; set; }
    }

    /// <summary>
    /// 測試 Switch...Case...When 的使用方式
    /// </summary>
    public void Run(){

        Animal dog = new Animal {name = "狗", isFly = false, isWalk = true};
        ShowBySwitch(dog);
        Animal goose = new Animal {name = "鵝", isFly = true, isWalk = true};
        ShowBySwitch(goose);
        Animal bird = new Animal {name = "鳥", isFly = true, isWalk = false};
        ShowBySwitch(bird);
        Animal fish = new Animal {name = "魚", isFly = false, isWalk = false};
        ShowBySwitch(fish);
    }


    /// <summary>
    /// 用 switch 判斷是否可以飛和走路，並顯示在命令列
    /// </summary>
    private void ShowBySwitch(Animal a){
        switch(a.isWalk){
            case true when a.isFly == false:
                Console.WriteLine(a.name + "可以走路，但不能飛");
                break;
            case true when a.isFly == true:
                Console.WriteLine(a.name + "可以走路，也可以飛");
                break;
            case false when a.isFly == true:
                Console.WriteLine(a.name + "不能走，但可以飛");
                break;
            case false when a.isFly == false:
                Console.WriteLine(a.name + "不能走，也不能飛");
                break;
        }
    }
}
```
執行結果如下：

```plain
狗可以走路，但不能飛
鵝可以走路，也可以飛
鳥不能走，但可以飛
魚不能走，也不能飛
```

### 參考資料

- 官方說明: [if and switch statements - select a code path to execute - C# - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements)
- 詳細介紹 Switch Case When 語法，提供多種語法的範例: [Switch Case When In C# Statement And Expression - ochzhen.com](https://ochzhen.com/blog/switch-case-when-in-csharp)
- 各種 Switch Case 語法的範例: [switch 的模式比對 – Zoey的程式日常](https://www.zoeydc.com/zh/posts/2022-04-16-switch_pattern_matching/ )
- 另一個 Switch Case When 語法的範例: [利用C#的switch case when語法來忽略字串大小寫](https://slashview.com/archive2019/20190827.html)
