---
layout: post
title: 觀察者模式簡介
date: 2021-03-18 12:00:00 +0800
categories:  [Design Pattern]
--- 

本篇文章將簡單介紹觀察者模式 (Observer Pattern) 的概念。

### 摘要

- 觀察者模式將資料的接收拆解，分為**發佈者**和**訂閱者**兩個角色。可以有多個訂閱者向由一個發佈者訂閱訊息，發佈者可以發送事件通知訂閱者，訂閱者可以隨時訂閱或取消訂閱事件通知。
- 透過撰寫兩個 Interface 建立觀察者模式，分別是**發佈者** Interface ，裡面可以包含訂閱者的清單 (List) ，另一個是**訂閱者** Interface ，發佈者裡面要有以下方法：

1. 訂閱的方法 (Subscribe)
2. 取消訂閱的方法 (Unsubscribe)
3. 通知的方法 (Notify)

以讓訂閱者進行訂閱。

- C# 的 Delegate、Event Handler 機制可以實踐觀察者模式。

### 範例

以下是觀察者模式的基本實作，包含訂閱、取消訂閱、通知等方法。

``` csharp
public class ObserverPattern
{
    ///<summary> 發佈者 (主題) 類別 </summary>
    public class Subject{
    
        List<Observer> observers = new List<Observer>();

        public void Subscribe(Observer ob)
        {
            observers.Add(ob);
        }

        public void Unsubscribe(Observer ob)
        {
            observers.Remove(ob);
        }

        public void Notify()
        {
            foreach(Observer ob in observers){
                ob.GotNotification();
            }
        }
    }

    ///<summary> 訂閱者 (觀察) 類別 </summary>
    public class Observer{
        int id;

        public Observer(int i)
        {
            id = i;
        }

        public void GotNotification()
        {
            Console.WriteLine(id + " got notification from subject.");
        }
    }

    ///<summary> 主程式 </summary>
    public void Run()
    {
        Observer a = new Observer(1);
        Observer b = new Observer(2);
        Observer c = new Observer(3);
        Subject s = new Subject();
        s.Subscribe(a);
        s.Subscribe(b);
        s.Subscribe(c);

        s.Notify();

        s.Unsubscribe(b);

        s.Notify();
    }
}

```

但是沒有實作 Interface 的話，假如同時有多種主題要發佈，也有不同的觀眾行為要處理，還是要加入 Interface，以減少類別間的耦合。

``` csharp
public class ObserverWithInterface
{
    ///<summary> 發佈者 (主題) 介面 </summary>
    public interface Subject{
        public void Subscribe(Observer ob);
        public void Unsubscribe(Observer ob);
        public void Notify();
    }

    ///<summary> 訂閱者 (觀察) 介面 </summary>
    public interface Observer{
        public void GotNotification(string message);
    }

    ///<summary> 音樂發佈者 </summary>
    public class MusicSubject: Subject{
    
        List<Observer> observers = new List<Observer>();

        public void Subscribe(Observer ob)
        {
            observers.Add(ob);
        }

        public void Unsubscribe(Observer ob)
        {
            observers.Remove(ob);
        }

        public void Notify()
        {
            foreach(Observer ob in observers){
                ob.GotNotification("Pop Music started!");
            }
        }
    }

    ///<summary> 卡通發佈者 </summary>
    public class CartoonSubject: Subject{
    
        List<Observer> observers = new List<Observer>();

        public void Subscribe(Observer ob)
        {
            observers.Add(ob);
        }

        public void Unsubscribe(Observer ob)
        {
            observers.Remove(ob);
        }

        public void Notify()
        {
            foreach(Observer ob in observers){
                ob.GotNotification("Now Playing: Doraemon");
            }
        }
    }

    ///<summary> 兒童訂閱者 </summary>
    public class KidsObserver:Observer{
        int id;
        
        public KidsObserver(int i)
        {
            id = i;
        }

        public void GotNotification(string message)
        {
            Console.WriteLine("Kid: " + id + " got notification \"" + message + "\" from subject.");
        }
    }

    ///<summary> 青年訂閱者 </summary>
    public class YoungObserver:Observer{
        int id;
        
        public YoungObserver(int i)
        {
            id = i;
        }

        public void GotNotification(string message)
        {
            Console.WriteLine("Young " + id + " got notification \"" + message + "\" from subject.");
        }
    }

    ///<summary> 主程式 </summary>
    public void Run()
    {
        Observer kids1 = new KidsObserver(1);
        Observer young2 = new YoungObserver(2);
        Observer kids3 = new KidsObserver(3);
        Subject cartoon = new CartoonSubject();
        Subject music = new MusicSubject();
        cartoon.Subscribe(kids1);
        cartoon.Subscribe(kids3);

        music.Subscribe(young2);
        music.Subscribe(kids3);

        cartoon.Notify();
        music.Notify();
    }
}

```


### 參考資料

- [Design Pattern - 只要你想知道，我就告訴你 - 觀察者模式（ Observer Pattern ） feat. TypeScript - by 神Q超人 - Enjoy life enjoy coding - Medium](https://medium.com/enjoy-life-enjoy-coding/8c15dcb21622)
- [觀念 設計模式–Observer Pattern - 我，傑夫。開發人](https://jeffprogrammer.wordpress.com/2015/07/23/淺談設計模式-observer-pattern/)
- [[C#]Observer Pattern到Delegate和Event - 全端開發人員天梯 - 點部落](https://dotblogs.com.tw/wellwind/2016/05/22/csharp-observer-pattern-delegate-event)
- [[Design Pattern] 觀察者模式 Observer Pattern - Jesper程式學習筆記 - 點部落](https://dotblogs.com.tw/JesperLai/2018/04/16/225616): 如果既有類別也想實作觀察者模式，可以使用 Delegate + EventHandler 發送通知。