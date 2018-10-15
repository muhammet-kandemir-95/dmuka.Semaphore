# dmuka.Semaphore

Demo : [Download Project](https://github.com/muhammet-kandemir-95/dmuka.Semaphore/archive/master.zip)

 Uygulama sunucusunda çok fazla sayıda işlemi belli bir sayıdaki "Thread" 'ler ile yönetmenizi sağlar. Böylelikle sunucuda aynı anda yürütülen işlemlerin sayısı her zaman belli olacaktır.

## Public Variables

### CoreCount
 Aynı anda yürütülen "Thread" sayısını belirler.
```csharp
public int CoreCount { get; }
```

### Disposed
 Aktif instance daha önceden "Dispose" oldumu.
```csharp
public bool Disposed { get; }
```

### Started
 Aktif instance daha önceden başlatıldımı.
```csharp
public bool Started { get; }
```

## Public Methods

### Start
 Aktif instance işlemini başlatır.
```csharp
public void Start()
```
 
### AddAction
 Aktif instance işlemindeki kuyruğa yeni bir action ekler. Daha önceden başlatılması veya başlatılmaması önemli değildir.
```csharp
public void AddAction(Action action)
```
 
### Dispose
 Aktif instance işlemini "Dispose" eder.
```csharp
public void Dispose()
```

## Örnek Kullanım

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
