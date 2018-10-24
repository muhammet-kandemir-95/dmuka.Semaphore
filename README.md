# dmuka.Semaphore

Demo : [Download Project](https://github.com/muhammet-kandemir-95/dmuka.Semaphore/archive/master.zip)

 You can manage multiple "Thread" on application server using this library. So the "Thread Count" on server is always fix.

## Public Variables

### CoreCount
 "Thread Count" for actions on server.
```csharp
public int CoreCount { get; }
```

### Disposed
 Is the current instance dispose?
```csharp
public bool Disposed { get; }
```

### Started
 Was the current instance start?.
```csharp
public bool Started { get; }
```

## Public Methods

### Start
 This is for start to current instance
```csharp
public void Start()
```
 
### AddAction
 The current instance add to queue. 
```csharp
public void AddAction(Action action)
```
 
### Dispose
 Is the current instance dispose?
```csharp
public void Dispose()
```

## Example Usage

```csharp
using System;
using System.Threading;

namespace dmuka.Semaphore.TestsConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("===== Muhammet Kandemir ======");
            Console.WriteLine("Welcome dmuka.Semaphore Tests!");

            // Create instance new semphore
            // This instance will have 4 core(Thread)
            ActionQueue semaphore = new ActionQueue(coreCount: 4);
            // Start semaphore as async
            semaphore.Start();

            var rowIndex = 0;
            // This thread is for add 10 action per 5 second
            // And each action wait 1 second after complate
            new Thread(() =>
            {
                while (true)
                {
                    for (int i = 0; i < 10; i++)
                    {
                        semaphore.AddAction(() =>
                        {
                            Console.WriteLine("Row Index = {0}", ++rowIndex);
                            Thread.Sleep(1000);
                        });
                    }

                    Thread.Sleep(5000);
                }
            }).Start();

            while (true)
            {
                Thread.Sleep(1);
            }
        }
    }
}
```
