---
layout: post
title: 簡單工廠模式 (Simple Factory Pattern)
date: 2023-02-09 12:00:00 +0800
categories:  [Design Pattern]
---

本篇文章介紹如何用簡單工廠模式 (Simple Factory Pattern) 分離物件的建立與使用邏輯 ，並包含範例程式碼。

### 特色

- 運用在有多個子類別，且這些子類別都繼承自父類別的情形。因此都會有相同的方法，但實作的邏輯不同。
- 可以分離「建立」和「使用」類別物件，以達到單一職責原則 (SRP)。

### 範例程式碼

```cs
public class SimpleFactory
{
    /// <summary> 手錶類別 </summary>
    protected class Watch
    {
        protected int hour;
        protected int minute;

        public Watch()
        {

        }
        
        public virtual void Setup(int h, int m)
        {
            this.hour = h;
            this.minute = m;
        }

        public void Show()
        {
            Console.WriteLine(hour + ":" + minute);
        }
    }

    /// <summary> 數位手錶 </summary>
    protected class DigitalWatch: Watch
    {
        public override void Setup(int h, int m){
            this.hour = h;
            this.minute = m;
            Console.WriteLine("Setup by Button");
        }

        public void Beep()
        {
            Console.WriteLine("Beep");
        }
    }

    /// <summary> 類比手錶 </summary>
    protected class AnalogWatch: Watch
    {
        public override void Setup(int h, int m){
            this.hour = h;
            this.minute = m;
            Console.WriteLine("Setup by Crown");
        }
    }
    
    /// <summary> 工廠模式: 手錶工廠 </summary>
    protected class WatchFactory
    {
        /// <summary> 根據參數產生不同類型手錶 </summary>
        public static Watch GetWatch(int i)
        {
            if(i == 0)
            {
                return new DigitalWatch();
            }
            else if(i == 1)
            {
                return new AnalogWatch();
            }
            return null;
        }
    }

    public void Run(int i)
    {
        Watch w;
        // 修改前：每次調整 Watch 的種類，都要修改 Run() 方法
        // if(i == 0)
        // {
        //     w = new DigitalWatch();
        // }
        // else{
        //     w = new AnalogWatch();
        // }
        // 修改後：使用簡單工廠模式，可以不更動 Run() 方法
        w = WatchFactory.GetWatch(i);
        w.Setup(5, 0);
        w.Show();
    }
}
```

### 參考資料

- [[30天快速上手TDD][Day 18]Refactoring - Factory Pattern - In 91 - 點部落](https://dotblogs.com.tw/hatelove/2013/01/02/learning-tdd-in-30-days-day18-refactoring-with-factory-pattern)
- [[ Day 2 ] 初探設計模式 - 工廠方法模式 (Factory Method Pattern) - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10202075)
- [[Design Pattern] 簡單工廠模式 Simple Factory Pattern - Jesper程式學習筆記 - 點部落](https://dotblogs.com.tw/JesperLai/2018/04/14/225256)