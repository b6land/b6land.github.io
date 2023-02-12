---
layout: post
title: 表象模式 (Facade Pattern)
date: 2023-02-04 12:00:00 +0800
categories:  [Design Pattern]
---

表象模式 (Facade Pattern) ，是把好幾種類別物件的方法，封裝在一個表象類別的方法內，只需要執行表象類別的方法，就等於執行了封裝在內部多個物件的方法。

### 特性

- 用於整合複雜的類別，減少使用上的難度。
- 使用者在使用表象類別時，不須理解背後的邏輯。
- 注意：表象類別封裝多種類別時，也增加了兩者間的耦合性。
- 適合用來簡化與整理複雜的程式碼。

### 範例

一個上班族可能有以下動作：

1. 上班: 分別要開啟辦公室軟體寫報告、音樂播放器放音樂。
2. 中午休息: 時要存檔、暫停音樂。
3. 下班: 存檔和關閉辦公室軟體，並停止播放音樂。

可以透過表象模式類別封裝，只要執行上班、休息、下班的動作就好。

```cs
public class Facade{
    
    ///<summary> 辦公室軟體 </summary>
    protected class Office
    {
        public void Open()
        {
            Console.WriteLine("Office: New");
        }

        public void Save()
        {
            Console.WriteLine("Office: Save");
        }

        public void Close()
        {
            Console.WriteLine("Office: Close");
        }
    }

    ///<summary> 音樂播放器 </summary>
    protected class AudioPlayer
    {
        public void Play()
        {
            Console.WriteLine("Player: Play");
        }

        public void Pause()
        {
            Console.WriteLine("Player: Pause");
        }

        public void Stop()
        {
            Console.WriteLine("Player: Stop");
        }
    } 

    ///<summary> 表象模式: 工作組合 </summary>
    protected class WorkSuiteFacade
    {
        protected Office office;
        protected AudioPlayer player;

        public WorkSuiteFacade()
        {
            office = new Office();
            player = new AudioPlayer();
        }

        public void StartWork()
        {
            office.Open();
            player.Play();
        }

        public void Break()
        {
            office.Save();
            player.Pause();
        }

        public void StopWork()
        {
            office.Save();
            office.Close();
            player.Stop();
        }
    }

    public void Run(){
        WorkSuiteFacade workFacade = new WorkSuiteFacade();
        
        Console.WriteLine("### Morning");
        workFacade.StartWork();
        workFacade.Break();

        Console.WriteLine("### Afternoon");
        workFacade.StartWork();
        workFacade.StopWork();
    }
}
```

執行結果:

```
### Morning
Office: New
Player: Play
Office: Save
Player: Pause
### Afternoon
Office: New
Player: Play
Office: Save
Office: Close
Player: Stop
```

### 參考資料

- [[Design Pattern] 外觀模式 Facade Pattern - Jesper程式學習筆記 - 點部落](https://dotblogs.com.tw/jesperlai/2018/04/15/153646)
- [[Design Pattern] Facade 門面模式 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10227186)
- [[ Day 15 ] 整理出漂亮的介面 - 外觀模式 ( Facade Pattern ) - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10206318)