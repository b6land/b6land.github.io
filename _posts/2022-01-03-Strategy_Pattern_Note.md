---
layout: post
title: 策略模式 (Strategy Pattern)
date: 2022-01-03 12:00:00 +0800
categories:  [Design Pattern]
--- 

本篇以條列式的方式，簡單描述策略模式的特色，並在文末附上範例。

### 特色

- 藉由定義公用的 Interface，規範不同策略類別如何實作，並達到容易替換的效果。
- 產生多個類別，減少 if-else 的長度。
- 可搭配簡單工廠模式一同使用。

### 缺點

- 會產生很多類似的類別。因此可以搭配其它模式使用，如工廠模式。
- 適用在常常需要變動的模組，若內部不太再需要變動，則不需要使用策略模式增加複雜度。

### 使用的時機

- 單元測試注入假物件時。
- 同樣行為，但使用不同的程式碼實作，例如各種繪畫的行為：實心方形、空心方形、實心圓形...。

### 範例

假如有個加油站提供不同車種的里程預估，用策略模式實作如下。

```cs
public class Strategy
{
    ///<summary> 車輛介面 </summary>
    protected interface ICar
    {
        public void AddFuel(int vol);
        public void CalcDistance();
    }

    ///<summary> 跑車類別 </summary>   
    protected class SportsCar : ICar
    {
        int fuel = 0;
        public void AddFuel(int vol)
        {
            this.fuel += vol;
        }
        public void CalcDistance()
        {
            double distance = fuel * 0.1;
            Console.WriteLine("Sports Car distance: " + distance);
        }
    }

    ///<summary> 轎車類別 </summary>
    protected class Sedan : ICar
    {
        int fuel = 0;
        public void AddFuel(int vol)
        {
            this.fuel += vol;
        }
        public void CalcDistance()
        {
            double distance = fuel * 0.3;
            Console.WriteLine("Sedan distance: " + distance);
        }
    }

    public void Run(int car)
    {
        // 原本的寫法
        // if (car == 0)
        // {
        //     SportsCar s = new SportsCar();
        //     s.AddFuel(100);
        //     s.CalcDistance();
        // }
        // else if(car == 1)
        // {
        //     Sedan s = new Sedan();
        //     s.AddFuel(100);
        //     s.CalcDistance();
        // }

        // 根據參數產生不同類別
        ICar s = GetCarInstance(0);
        // 保留相同的部分
        s.AddFuel(100);
        s.CalcDistance();
    }

    ///<summary> 策略模式，取得不同類別 </summary>
    protected ICar GetCarInstance(int car)
    {
        if(car == 0)
        {
            return new SportsCar();
        }
        else
        {
            return new Sedan();
        }
    }
}
```

### 參考資料

[[ Day 3 ] 初探設計模式 - 策略模式 (Strategy Pattern)](https://ithelp.ithome.com.tw/m/articles/10202506)

[[30天快速上手TDD][Day 17]Refactoring - Strategy Pattern - In 91 - 點部落](https://dotblogs.com.tw/hatelove/2013/01/02/learning-tdd-in-30-days-day17-refactoring-with-strategy-pattern)