---
title: aaaReliableConcurrentQueue v Azure Service Fabric
description: "ReliableConcurrentQueue je Vysoká propustnost fronty, který umožňuje paralelní enqueues a dequeues."
services: service-fabric
documentationcenter: .net
author: sangarg
manager: timlt
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: sangarg
ms.openlocfilehash: 78a9905996b9ab265c1288d2b49753638d7bc445
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooreliableconcurrentqueue-in-azure-service-fabric"></a>Úvod tooReliableConcurrentQueue v Azure Service Fabric
Spolehlivé souběžných fronty je frontu asynchronní, transakční a replikované které funkce vysoké souběžnosti pro zařazování a dequeue – operace. Je navrženou toodeliver vysoké prostupnosti a nízké latence podle uvolnit hello striktní FIFO řazení poskytované [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) a místo toho poskytuje best effort řazení.

## <a name="apis"></a>Rozhraní API

|Souběžné fronty                |Spolehlivé souběžných fronty                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T item)           | Úloha EnqueueAsync (ITransaction tx, položka T)                       |
| BOOL TryDequeue (out výsledek T)  | Úloha < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)  |
| int Count()                    | dlouhé Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Porovnání s [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Spolehlivé souběžných fronty je poskytován jako alternativu příliš[spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx). By být používána v případech, kde není vyžadována, striktní řazení FIFO jako zaručující FIFO vyžaduje kompromis s souběžnosti.  [Spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) používá zámky tooenforce FIFO řazení, s maximálně jednu transakci povolen tooenqueue a na většině jednu transakci toodequeue v jednu chvíli může. Porovnání spolehlivé souběžných fronty zmírní hello řazení omezení a umožňuje jejich zařadit všechny počet souběžných transakcí toointerleave a dequeue – operace. Řazení typu Best effort je zadáno, ale relativní řazení hello ze dvou hodnot v spolehlivé souběžných frontě možné nikdy zaručit.

Poskytuje spolehlivé souběžných fronty vyšší propustnost a nižší latenci než [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) vždy, když jsou více souběžných transakcí provádění enqueues nebo dequeues.

Ukázka případ použití hello ReliableConcurrentQueue je hello [fronta zpráv](https://en.wikipedia.org/wiki/Message_queue) scénář. V tomto scénáři jedním nebo více výrobci zprávu vytvořit a přidat položky toohello fronty, a jeden nebo více zpráv příjemci načítat zprávy z fronty hello a jejich zpracování. Více producenti a spotřebitelé může nezávisle pracovat, použití souběžných transakcí ve frontě hello tooprocess pořadí.

## <a name="usage-guidelines"></a>Pokyny týkající se používání
* fronty Hello očekává, že hello položek ve frontě hello mají doby uchování nízkou. To znamená hello položky by zůstat ve frontě hello po dlouhou dobu.
* fronty Hello nezaručuje striktní FIFO řazení.
* fronty Hello číst vlastní zápisy. Pokud je položka zařazených do fronty v rámci transakce, nebude viditelná tooa dequeuer v rámci hello stejné transakci.
* Dequeues nejsou od sebe navzájem oddělené. Pokud položka *A* je vyjmutou v transakci *txnA*, i když *txnA* položky není potvrzena, *A* nebudou viditelné tooa souběžných transakce *txnB*.  Pokud *txnA* zruší, *A* se bude zobrazovat příliš*txnB* okamžitě.
* *TryPeekAsync* chování může být implementováno pomocí *TryDequeueAsync* a pak ruší se transakce hello. Příklady najdete v hello části programování vzory.
* Počet je netransakční. Může být použité tooget představu o hello počet elementů ve frontě hello, ale představuje bodu v čase a nelze spoléhat.
* Nákladné zpracování na hello vyjmutou položky nesmí provést v průběhu hello transakcí je aktivní, tooavoid dlouhotrvajících transakcí, které by mohly mít dopad na výkon systému hello.

## <a name="code-snippets"></a>Fragmenty kódu
Dejte nám se podívejte na několik fragmenty kódu a jejich očekávané výstupy. Zpracovávání výjimek v jazyce je ignorován v této části.

### <a name="enqueueasync"></a>EnqueueAsync
Tady je několik fragmenty kódu pro použití EnqueueAsync následuje jejich očekávané výstupy.

- *Případ 1: Zařazování úloh jeden*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Předpokládejme, že hello úloha byla úspěšně dokončena, a který nebyly žádné souběžných transakcí úprava fronty hello. uživatel, Hello očekávat hello fronty toocontain hello položky v některém z hello následující příkazy:

> 10, 20

> 20, 10


- *Případ 2: Zařazování úloh paralelní*

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}

// Parallel Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken);

    await txn.CommitAsync();
}
```

Předpokládejme, že hello úlohy byl úspěšně dokončen, jestli hello úlohy spustila paralelně a že neexistují žádné souběžných transakcí úprava fronty hello. Žádné odvození můžete provedeny o hello pořadí položek ve frontě hello. Pro tento fragment kódu může se zobrazit hello položky v některém z hello 4! možné pořadí.  Hello fronty se pokusí tookeep hello položky v hello původní (zařazených do fronty) pořadí, ale může být vynucené tooreorder je z důvodu operace tooconcurrent nebo chyb.


### <a name="dequeueasync"></a>DequeueAsync
Tady je několik fragmenty kódu pro použití TryDequeueAsync následuje výstupy hello očekává. Předpokládejme, že této fronty hello je již naplněný hello následujících položek ve frontě hello:
> 10, 20, 30, 40, 50, 60

- *Případ 1: Jeden dequeue – úloha*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Předpokládejme, že hello úloha byla úspěšně dokončena, a který nebyly žádné souběžných transakcí úprava fronty hello. Vzhledem k tomu, že o hello pořadí hello položek ve frontě hello nelze realizovat žádná odvození, může být vyjmutou všechny tři položky hello v libovolném pořadí. Hello fronty se pokusí tookeep hello položky v hello původní (zařazených do fronty) pořadí, ale může být vynucené tooreorder je z důvodu operace tooconcurrent nebo chyb.  

- *Případ 2: Dequeue – paralelní úlohy*

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}
```

Předpokládejme, že hello úlohy byl úspěšně dokončen, jestli hello úlohy spustila paralelně a že neexistují žádné souběžných transakcí úprava fronty hello. Vzhledem k tomu, že o hello pořadí hello položek ve frontě hello nelze realizovat žádná odvození, hello seznamy *dequeue1* a *dequeue2* bude každý obsahovat jakékoli dvě položky v libovolném pořadí.

Hello se stejnou položku *není* se zobrazí v obou seznamy. Proto pokud má dequeue1 *10*, *30*, pak by měla mít dequeue2 *20*, *40*.

- *Případ 3: Dequeue – při řazení s přerušení transakcí*

Ruší se transakce s během letu dequeues PUT, které položky hello zpět na záhlaví hello hello fronty. Hello pořadí, ve kterém hello položky se vrátit zpět na záhlaví hello hello fronty není zaručena. Dejte nám podívejte se na hello následující kód:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort hello transaction
    await txn.AbortAsync();
}
```
Předpokládejme, že hello položky byly vyjmutou v hello následující pořadí:
> 10, 20

Když jsme abort hello transakce, hello položky by byl přidán back toohello head hello fronty v některém z hello následující příkazy:
> 10, 20

> 20, 10

Hello totéž platí pro všechny případy, kdy hello transakce nebyla úspěšně *potvrzení*.

## <a name="programming-patterns"></a>Vzory programování
V této části, dejte nám podívejte se na několik programování vzorů, které mohou být užitečné při použití ReliableConcurrentQueue.

### <a name="batch-dequeues"></a>Batch Dequeues
A doporučená programovací vzor je pro hello příjemce úloh toobatch jeho dequeues místo jedné dequeue – najednou. Hello uživatel může vybrat toothrottle zpoždění mezi každou dávka nebo hello velikost dávky. Hello následující fragment kódu ukazuje tento programovací model.  Všimněte si, že v tomto příkladu hello dokončení zpracování po hello transakce se potvrdí, takže pokud chybu byly toooccur při zpracování, hello nezpracované položky budou ztraceny bez zpracování.  Alternativně lze provést hello zpracování v rámci oboru hello transakce, ale to může mít negativní dopad na výkon a vyžaduje zpracování položek hello již zpracovány.

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add toohello buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break hello for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a>Zpracování oznámení na základě typu Best Effort
Jiné zajímavé programovací vzor používá hello počet rozhraní API. Zde můžeme implementovat zpracování oznámení na základě typu best effort pro frontu hello. fronty Hello počet může být použité toothrottle element zařazování nebo dequeue úloh.  Všimněte si, že jako v předchozím příkladu hello, vzhledem k tomu, že zpracování hello proběhne mimo transakci hello nezpracované položky může dojít ke ztrátě Pokud dojde k chybě během zpracování.

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If hello queue does not have hello threshold number of items, delay hello task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process hello queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add toohello buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a>Best Effort vyprazdňování
Z důvodu souběžných povaha hello datová struktura toohello nelze zaručit vyprazdňování hello fronty.  Je možné, že i když jsou neukládají žádné uživatele operace ve frontě hello, konkrétní volání tooTryDequeueAsync nemusí vracet položku, která byla dříve zařazených do fronty a potvrzené.  Hello zařazených do fronty položek záruku, že se příliš*nakonec* stát viditelné toodequeue, ale bez mechanismus komunikace out-of-band nemůže vědět nezávislé příjemce této fronty hello dosáhl stabilní i když všechny jsou povoleny žádné nové operace zařazování a producenti byly zastaveny. Operace vyprazdňování hello tedy best effort jak jsou implementované níže.

Hello uživatel by měl zastavit všechny další producent a úlohy příjemce a počkejte, všechny toocommit během letu transakce nebo přerušení, před pokusem o toodrain hello fronty.  Pokud uživatel hello nezná hello očekávaný počet položek ve frontě hello, můžete nastavit oznámení, která signalizuje, že máte byla vyjmutou všechny položky.

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if(ret.HasValue)
            {
                // Buffer hello dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a>Prohlížet
ReliableConcurrentQueue neposkytuje hello *TryPeekAsync* rozhraní api. Uživatelé dostanou funkce Náhled hello sémantického pomocí *TryDequeueAsync* a pak ruší se transakce hello. V tomto příkladu dequeues se zpracují pouze v případě, že je větší než hodnota položky hello *10*.

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process hello item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync();    
    }
    else
    {
        await txn.AbortAsync();
    }
}
```

## <a name="must-read"></a>Musí pro čtení
* [Spolehlivé služby rychlý Start](service-fabric-reliable-services-quick-start.md)
* [Práce s Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Spolehlivé služby oznámení](service-fabric-reliable-services-notifications.md)
* [Spolehlivé služby zálohování a obnovení (zotavení po havárii)](service-fabric-reliable-services-backup-restore.md)
* [Spolehlivé stavu Správce konfigurace](service-fabric-reliable-services-configuration.md)
* [Začínáme se službou Service Fabric webové rozhraní API služby](service-fabric-reliable-services-communication-webapi.md)
* [Rozšířené použití hello spolehlivé služby programovací Model](service-fabric-reliable-services-advanced-usage.md)
* [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
