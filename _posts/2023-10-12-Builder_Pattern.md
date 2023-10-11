---
layout: post
title: 建造者模式 (Builder Pattern)
date: 2023-10-12 12:00:00 +0800
categories: [Design Pattern]
---

本文介紹設計模式中的建造者模式，並使用 C# 撰寫範例。

### 主要的類別

1. Builder Interface: 定義須實做的方法。
2. 實際的 Builder Class: 呼叫基礎的 Product 類別，並實作個別的差異性。
3. Product Class: 基礎類別。
4. Director Class: 組合 Builder 的建造步驟 (非必要)。

### 要解決的問題

1. 建構子太長的問題。
2. 不需要參數時，需填入 `null` 的情形。
3. 避免有很多類似的建構子。

### 注意

1. 基礎的方法相同時，比較適用此模式。
2. 可能會產生很多的 Builder 類別。

### 範例程式碼

**Builder.cs**

``` cs
public interface IHouseBuilder{
    IHouseBuilder SetBathroom();
    IHouseBuilder SetLivingRoom();
    IHouseBuilder SetBedroom(int n);
    IHouseBuilder SetKitchen();
    IHouseBuilder SetDiningRoom();
    IHouseBuilder SetBalcony();
    House Build();
}

public class House{
    private string title;
    private List<string> listDesc;

    public House(string title){
        this.title = title;
        listDesc = new List<string>();
    }
    
    public void Show(){
        Console.WriteLine($"**{title}**");
        foreach(string desc in listDesc){
            Console.WriteLine($"- {desc}");
        }
    }

    public void SetBathroom(){
        listDesc.Add("建造 1 個浴廁");
    }

    public void SetLivingRoom(){
        listDesc.Add("建造 1 個客廳");
    }

    public void SetBedroom(int n){
        listDesc.Add($"建造 {n} 個臥室");
    }

    public void SetKitchen(){
        listDesc.Add("建造 1 個廚房");
    }

    public void SetDiningRoom(){
        listDesc.Add("建造 1 個餐廳");
    }

    public void SetBalcony(){
        listDesc.Add("建造 1 個陽台");
    }
}

public class EuropeHouseBuilder: IHouseBuilder{
    private House _house;

    public EuropeHouseBuilder(){
        _house = new House("歐風洋房");
    }

    public IHouseBuilder SetBathroom(){
        _house.SetBathroom();
        return this;
    }

    public IHouseBuilder SetLivingRoom(){
        _house.SetLivingRoom();
        return this;
    }

    public IHouseBuilder SetBedroom(int n){
        _house.SetBedroom(n);
        return this;
    }

    public IHouseBuilder SetKitchen(){
        _house.SetKitchen();
        return this;
    }

    public IHouseBuilder SetDiningRoom(){
        _house.SetDiningRoom();
        return this;
    }

    public IHouseBuilder SetBalcony(){
        _house.SetBalcony();
        return this;
    }

    public House Build(){
        return _house;
    }
}

public class JapaneseHouseBuilder: IHouseBuilder{
    private House _house;

    public JapaneseHouseBuilder(){
        _house = new House("日式木屋");
    }

    public IHouseBuilder SetBathroom(){
        _house.SetBathroom();
        return this;
    }

    public IHouseBuilder SetLivingRoom(){
        _house.SetLivingRoom();
        return this;
    }

    public IHouseBuilder SetBedroom(int n){
        _house.SetBedroom(n);
        return this;
    }

    public IHouseBuilder SetKitchen(){
        _house.SetKitchen();
        return this;
    }

    public IHouseBuilder SetDiningRoom(){
        _house.SetDiningRoom();
        return this;
    }

    public IHouseBuilder SetBalcony(){
        _house.SetBalcony();
        return this;
    }

    public House Build(){
        return _house;
    }
}
```

**Director**

``` cs
public class Director{
    IHouseBuilder builder;

    public Director(IHouseBuilder builder){
        this.builder = builder;
    }

    public House BuildLuxuryHouse(){
        return builder.SetBalcony()
                    .SetBathroom()
                    .SetBedroom(4)
                    .SetDiningRoom()
                    .SetKitchen()
                    .SetLivingRoom()
                    .Build();
    }

    public House BuildBasicHouse(){
        return builder.SetBathroom()
                    .SetBedroom(2)
                    .SetDiningRoom()
                    .SetKitchen()
                    .Build();
    }
}
```

**Main.cs**

``` cs
House myEuropeHouse = new EuropeHouseBuilder()
                        .SetBalcony()
                        .SetBedroom(1)
                        .SetBathroom()
                        .SetLivingRoom()
                        .SetKitchen()
                        .Build();

House myJapaneseHouse = new JapaneseHouseBuilder()
                        .SetBalcony()
                        .SetBedroom(2)
                        .SetBathroom()
                        .SetDiningRoom()
                        .SetKitchen()
                        .Build();

Director europeHouseDirector = new Director(new EuropeHouseBuilder());
House luxuryEuropeHouse = europeHouseDirector.BuildLuxuryHouse();

Director japaneseHouseDirector = new Director(new JapaneseHouseBuilder());
House basicJapaneseHouse = japaneseHouseDirector.BuildBasicHouse();

myEuropeHouse.Show();
myJapaneseHouse.Show();
luxuryEuropeHouse.Show();
basicJapaneseHouse.Show();
```

### 參考資料

- [設計模式—建造者模式 (Builder Design Pattern) - by Wenchin - Wenchin Rolls Around - Medium](https://medium.com/wenchin-rolls-around/7c8eac7c9a7)
- [建造者模式-Builder Pattern - 老E隨手寫 - 點部落](https://dotblogs.com.tw/bda605/2020/01/20/165033)
- [[Design Pattern] Builder 建造者模式 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10217675)