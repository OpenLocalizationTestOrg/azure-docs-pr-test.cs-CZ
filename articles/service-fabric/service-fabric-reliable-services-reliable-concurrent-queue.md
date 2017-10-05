---
title: ReliableConcurrentQueue v Azure Service Fabric
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
ms.openlocfilehash: 122cb48149477f295a65b8ee623c647b6db10a86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a><span data-ttu-id="b055a-103">Úvod do ReliableConcurrentQueue v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b055a-103">Introduction to ReliableConcurrentQueue in Azure Service Fabric</span></span>
<span data-ttu-id="b055a-104">Spolehlivé souběžných fronty je frontu asynchronní, transakční a replikované které funkce vysoké souběžnosti pro zařazování a dequeue – operace.</span><span class="sxs-lookup"><span data-stu-id="b055a-104">Reliable Concurrent Queue is an asynchronous, transactional, and replicated queue which features high concurrency for enqueue and dequeue operations.</span></span> <span data-ttu-id="b055a-105">Je navržen pro poskytování vysoké prostupnosti a nízké latence podle uvolnit striktní řazení FIFO poskytované [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) a místo toho poskytuje best effort řazení.</span><span class="sxs-lookup"><span data-stu-id="b055a-105">It is designed to deliver high throughput and low latency by relaxing the strict FIFO ordering provided by [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) and instead provides a best-effort ordering.</span></span>

## <a name="apis"></a><span data-ttu-id="b055a-106">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b055a-106">APIs</span></span>

|<span data-ttu-id="b055a-107">Souběžné fronty</span><span class="sxs-lookup"><span data-stu-id="b055a-107">Concurrent Queue</span></span>                |<span data-ttu-id="b055a-108">Spolehlivé souběžných fronty</span><span class="sxs-lookup"><span data-stu-id="b055a-108">Reliable Concurrent Queue</span></span>                                         |
|--------------------------------|------------------------------------------------------------------|
| <span data-ttu-id="b055a-109">void Enqueue(T item)</span><span class="sxs-lookup"><span data-stu-id="b055a-109">void Enqueue(T item)</span></span>           | <span data-ttu-id="b055a-110">Úloha EnqueueAsync (ITransaction tx, položka T)</span><span class="sxs-lookup"><span data-stu-id="b055a-110">Task EnqueueAsync(ITransaction tx, T item)</span></span>                       |
| <span data-ttu-id="b055a-111">BOOL TryDequeue (out výsledek T)</span><span class="sxs-lookup"><span data-stu-id="b055a-111">bool TryDequeue(out T result)</span></span>  | <span data-ttu-id="b055a-112">Úloha < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)</span><span class="sxs-lookup"><span data-stu-id="b055a-112">Task< ConditionalValue < T > > TryDequeueAsync(ITransaction tx)</span></span>  |
| <span data-ttu-id="b055a-113">int Count()</span><span class="sxs-lookup"><span data-stu-id="b055a-113">int Count()</span></span>                    | <span data-ttu-id="b055a-114">dlouhé Count()</span><span class="sxs-lookup"><span data-stu-id="b055a-114">long Count()</span></span>                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a><span data-ttu-id="b055a-115">Porovnání s [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span><span class="sxs-lookup"><span data-stu-id="b055a-115">Comparison with [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span></span>

<span data-ttu-id="b055a-116">Spolehlivé souběžných fronty je poskytován jako alternativu k [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span><span class="sxs-lookup"><span data-stu-id="b055a-116">Reliable Concurrent Queue is offered as an alternative to [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span></span> <span data-ttu-id="b055a-117">By být používána v případech, kde není vyžadována, striktní řazení FIFO jako zaručující FIFO vyžaduje kompromis s souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="b055a-117">It should be used in cases where strict FIFO ordering is not required, as guaranteeing FIFO requires a tradeoff with concurrency.</span></span>  <span data-ttu-id="b055a-118">[Spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) používá zámky vynutit FIFO řazení s maximálně jednu transakci povolen zařadit do fronty a maximálně jednu transakci dovoleno dequeue – najednou.</span><span class="sxs-lookup"><span data-stu-id="b055a-118">[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) uses locks to enforce FIFO ordering, with at most one transaction allowed to enqueue and at most one transaction allowed to dequeue at a time.</span></span> <span data-ttu-id="b055a-119">Porovnání spolehlivé souběžných fronty zmírní řazení omezení a umožňuje jakýchkoli počet souběžných transakcí interleave jejich zařazování a dequeue – operace.</span><span class="sxs-lookup"><span data-stu-id="b055a-119">In comparison, Reliable Concurrent Queue relaxes the ordering constraint and allows any number concurrent transactions to interleave their enqueue and dequeue operations.</span></span> <span data-ttu-id="b055a-120">Řazení typu Best effort je zadáno, ale relativní řazení ze dvou hodnot v spolehlivé souběžných frontě možné nikdy zaručit.</span><span class="sxs-lookup"><span data-stu-id="b055a-120">Best-effort ordering is provided, however the relative ordering of two values in a Reliable Concurrent Queue can never be guaranteed.</span></span>

<span data-ttu-id="b055a-121">Poskytuje spolehlivé souběžných fronty vyšší propustnost a nižší latenci než [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) vždy, když jsou více souběžných transakcí provádění enqueues nebo dequeues.</span><span class="sxs-lookup"><span data-stu-id="b055a-121">Reliable Concurrent Queue provides higher throughput and lower latency than [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) whenever there are multiple concurrent transactions performing enqueues and/or dequeues.</span></span>

<span data-ttu-id="b055a-122">Ukázka případ použití je ReliableConcurrentQueue [fronta zpráv](https://en.wikipedia.org/wiki/Message_queue) scénář.</span><span class="sxs-lookup"><span data-stu-id="b055a-122">A sample use case for the ReliableConcurrentQueue is the [Message Queue](https://en.wikipedia.org/wiki/Message_queue) scenario.</span></span> <span data-ttu-id="b055a-123">V tomto scénáři jeden nebo více zpráv producenti vytvoření a přidání položek do fronty, a jeden nebo více zpráv příjemci načítat zprávy z fronty a jejich zpracování.</span><span class="sxs-lookup"><span data-stu-id="b055a-123">In this scenario, one or more message producers create and add items to the queue, and one or more message consumers pull messages from the queue and process them.</span></span> <span data-ttu-id="b055a-124">Více producenti a spotřebitelé může nezávisle pracovat, aby bylo možné zpracovat fronty použití souběžných transakcí.</span><span class="sxs-lookup"><span data-stu-id="b055a-124">Multiple producers and consumers can work independently, using concurrent transactions in order to process the queue.</span></span>

## <a name="usage-guidelines"></a><span data-ttu-id="b055a-125">Pokyny týkající se používání</span><span class="sxs-lookup"><span data-stu-id="b055a-125">Usage Guidelines</span></span>
* <span data-ttu-id="b055a-126">Fronty očekává, že položky ve frontě mají doby uchování nízkou.</span><span class="sxs-lookup"><span data-stu-id="b055a-126">The queue expects that the items in the queue have a low retention period.</span></span> <span data-ttu-id="b055a-127">To znamená položky by zůstat ve frontě delší dobu.</span><span class="sxs-lookup"><span data-stu-id="b055a-127">That is, the items would not stay in the queue for a long time.</span></span>
* <span data-ttu-id="b055a-128">Fronty nezaručuje striktní FIFO řazení.</span><span class="sxs-lookup"><span data-stu-id="b055a-128">The queue does not guarantee strict FIFO ordering.</span></span>
* <span data-ttu-id="b055a-129">Fronta není přečíst vlastní zápisy.</span><span class="sxs-lookup"><span data-stu-id="b055a-129">The queue does not read its own writes.</span></span> <span data-ttu-id="b055a-130">Pokud je položka zařazených do fronty v rámci transakce, nebude viditelná pro dequeuer v rámci stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="b055a-130">If an item is enqueued within a transaction, it will not be visible to a dequeuer within the same transaction.</span></span>
* <span data-ttu-id="b055a-131">Dequeues nejsou od sebe navzájem oddělené.</span><span class="sxs-lookup"><span data-stu-id="b055a-131">Dequeues are not isolated from each other.</span></span> <span data-ttu-id="b055a-132">Pokud položka *A* je vyjmutou v transakci *txnA*, i když *txnA* položky není potvrzena, *A* nebude viditelná pro souběžné transakce *txnB*.</span><span class="sxs-lookup"><span data-stu-id="b055a-132">If item *A* is dequeued in transaction *txnA*, even though *txnA* is not committed, item *A* would not be visible to a concurrent transaction *txnB*.</span></span>  <span data-ttu-id="b055a-133">Pokud *txnA* zruší, *A* bude zobrazovat *txnB* okamžitě.</span><span class="sxs-lookup"><span data-stu-id="b055a-133">If *txnA* aborts, *A* will become visible to *txnB* immediately.</span></span>
* <span data-ttu-id="b055a-134">*TryPeekAsync* chování může být implementováno pomocí *TryDequeueAsync* a pak ruší se transakce.</span><span class="sxs-lookup"><span data-stu-id="b055a-134">*TryPeekAsync* behavior can be implemented by using a *TryDequeueAsync* and then aborting the transaction.</span></span> <span data-ttu-id="b055a-135">Příklady najdete v části programování vzory.</span><span class="sxs-lookup"><span data-stu-id="b055a-135">An example of this can be found in the Programming Patterns section.</span></span>
* <span data-ttu-id="b055a-136">Počet je netransakční.</span><span class="sxs-lookup"><span data-stu-id="b055a-136">Count is non-transactional.</span></span> <span data-ttu-id="b055a-137">Umožňuje získat představu o počet elementů ve frontě, ale představuje bodu v čase a nelze spoléhat.</span><span class="sxs-lookup"><span data-stu-id="b055a-137">It can be used to get an idea of the number of elements in the queue, but represents a point-in-time and cannot be relied upon.</span></span>
* <span data-ttu-id="b055a-138">Nákladné zpracování na dequeued položky nebude prováděna při transakci je aktivní, aby se zabránilo dlouhotrvajících transakcí, které by mohly mít dopad na výkon systému.</span><span class="sxs-lookup"><span data-stu-id="b055a-138">Expensive processing on the dequeued items should not be performed while the transaction is active, to avoid long-running transactions which may have a performance impact on the system.</span></span>

## <a name="code-snippets"></a><span data-ttu-id="b055a-139">Fragmenty kódu</span><span class="sxs-lookup"><span data-stu-id="b055a-139">Code Snippets</span></span>
<span data-ttu-id="b055a-140">Dejte nám se podívejte na několik fragmenty kódu a jejich očekávané výstupy.</span><span class="sxs-lookup"><span data-stu-id="b055a-140">Let us look at a few code snippets and their expected outputs.</span></span> <span data-ttu-id="b055a-141">Zpracovávání výjimek v jazyce je ignorován v této části.</span><span class="sxs-lookup"><span data-stu-id="b055a-141">Exception handling is ignored in this section.</span></span>

### <a name="enqueueasync"></a><span data-ttu-id="b055a-142">EnqueueAsync</span><span class="sxs-lookup"><span data-stu-id="b055a-142">EnqueueAsync</span></span>
<span data-ttu-id="b055a-143">Tady je několik fragmenty kódu pro použití EnqueueAsync následuje jejich očekávané výstupy.</span><span class="sxs-lookup"><span data-stu-id="b055a-143">Here are a few code snippets for using EnqueueAsync followed by their expected outputs.</span></span>

- <span data-ttu-id="b055a-144">*Případ 1: Zařazování úloh jeden*</span><span class="sxs-lookup"><span data-stu-id="b055a-144">*Case 1: Single Enqueue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="b055a-145">Předpokládejme, že úloha dokončena úspěšně a že v něm nebyly žádné souběžných transakcí úprava fronty.</span><span class="sxs-lookup"><span data-stu-id="b055a-145">Assume that the task completed successfully, and that there were no concurrent transactions modifying the queue.</span></span> <span data-ttu-id="b055a-146">Uživatele můžete očekávat fronty tak, aby obsahovala položky ve všech následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="b055a-146">The user can expect the queue to contain the items in any of the following orders:</span></span>

> <span data-ttu-id="b055a-147">10, 20</span><span class="sxs-lookup"><span data-stu-id="b055a-147">10, 20</span></span>

> <span data-ttu-id="b055a-148">20, 10</span><span class="sxs-lookup"><span data-stu-id="b055a-148">20, 10</span></span>


- <span data-ttu-id="b055a-149">*Případ 2: Zařazování úloh paralelní*</span><span class="sxs-lookup"><span data-stu-id="b055a-149">*Case 2: Parallel Enqueue Task*</span></span>

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

<span data-ttu-id="b055a-150">Předpokládejme, že úlohy byl úspěšně dokončen, že úkoly spustili paralelně a že neexistují žádné souběžných transakcí úprava fronty.</span><span class="sxs-lookup"><span data-stu-id="b055a-150">Assume that the tasks completed successfully, that the tasks ran in parallel, and that there were no other concurrent transactions modifying the queue.</span></span> <span data-ttu-id="b055a-151">Žádné odvození můžete provedeny o pořadí položek ve frontě.</span><span class="sxs-lookup"><span data-stu-id="b055a-151">No inference can be made about the order of items in the queue.</span></span> <span data-ttu-id="b055a-152">Pro tento fragment kódu může se zobrazit položky v některém ze 4!</span><span class="sxs-lookup"><span data-stu-id="b055a-152">For this code snippet, the items may appear in any of the 4!</span></span> <span data-ttu-id="b055a-153">možné pořadí.</span><span class="sxs-lookup"><span data-stu-id="b055a-153">possible orderings.</span></span>  <span data-ttu-id="b055a-154">Fronty se pokusí uchovat položky v původní pořadí (zařazených do fronty), ale muset změnit jejich pořadí z důvodu souběžných operací nebo chyb.</span><span class="sxs-lookup"><span data-stu-id="b055a-154">The queue will attempt to keep the items in the original (enqueued) order, but may be forced to reorder them due to concurrent operations or faults.</span></span>


### <a name="dequeueasync"></a><span data-ttu-id="b055a-155">DequeueAsync</span><span class="sxs-lookup"><span data-stu-id="b055a-155">DequeueAsync</span></span>
<span data-ttu-id="b055a-156">Tady je několik fragmenty kódu pro použití TryDequeueAsync následuje očekávané výstupy.</span><span class="sxs-lookup"><span data-stu-id="b055a-156">Here are a few code snippets for using TryDequeueAsync followed by the expected outputs.</span></span> <span data-ttu-id="b055a-157">Předpokládejme, že fronta je již naplněný následující položky ve frontě:</span><span class="sxs-lookup"><span data-stu-id="b055a-157">Assume that the queue is already populated with the following items in the queue:</span></span>
> <span data-ttu-id="b055a-158">10, 20, 30, 40, 50, 60</span><span class="sxs-lookup"><span data-stu-id="b055a-158">10, 20, 30, 40, 50, 60</span></span>

- <span data-ttu-id="b055a-159">*Případ 1: Jeden dequeue – úloha*</span><span class="sxs-lookup"><span data-stu-id="b055a-159">*Case 1: Single Dequeue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="b055a-160">Předpokládejme, že úloha dokončena úspěšně a že v něm nebyly žádné souběžných transakcí úprava fronty.</span><span class="sxs-lookup"><span data-stu-id="b055a-160">Assume that the task completed successfully, and that there were no concurrent transactions modifying the queue.</span></span> <span data-ttu-id="b055a-161">Vzhledem k tomu, že o pořadí položek ve frontě nelze realizovat žádná odvození, může být vyjmutou všechny tři položky v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="b055a-161">Since no inference can be made about the order of the items in the queue, any three of the items may be dequeued, in any order.</span></span> <span data-ttu-id="b055a-162">Fronty se pokusí uchovat položky v původní pořadí (zařazených do fronty), ale muset změnit jejich pořadí z důvodu souběžných operací nebo chyb.</span><span class="sxs-lookup"><span data-stu-id="b055a-162">The queue will attempt to keep the items in the original (enqueued) order, but may be forced to reorder them due to concurrent operations or faults.</span></span>  

- <span data-ttu-id="b055a-163">*Případ 2: Dequeue – paralelní úlohy*</span><span class="sxs-lookup"><span data-stu-id="b055a-163">*Case 2: Parallel Dequeue Task*</span></span>

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

<span data-ttu-id="b055a-164">Předpokládejme, že úlohy byl úspěšně dokončen, že úkoly spustili paralelně a že neexistují žádné souběžných transakcí úprava fronty.</span><span class="sxs-lookup"><span data-stu-id="b055a-164">Assume that the tasks completed successfully, that the tasks ran in parallel, and that there were no other concurrent transactions modifying the queue.</span></span> <span data-ttu-id="b055a-165">Vzhledem k tomu, že nelze realizovat žádná odvození o pořadí položek ve frontě, seznamy *dequeue1* a *dequeue2* bude každý obsahovat jakékoli dvě položky v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="b055a-165">Since no inference can be made about the order of the items in the queue, the lists *dequeue1* and *dequeue2* will each contain any two items, in any order.</span></span>

<span data-ttu-id="b055a-166">Stejná položka bude *není* se zobrazí v obou seznamy.</span><span class="sxs-lookup"><span data-stu-id="b055a-166">The same item will *not* appear in both lists.</span></span> <span data-ttu-id="b055a-167">Proto pokud má dequeue1 *10*, *30*, pak by měla mít dequeue2 *20*, *40*.</span><span class="sxs-lookup"><span data-stu-id="b055a-167">Hence, if dequeue1 has *10*, *30*, then dequeue2 would have *20*, *40*.</span></span>

- <span data-ttu-id="b055a-168">*Případ 3: Dequeue – při řazení s přerušení transakcí*</span><span class="sxs-lookup"><span data-stu-id="b055a-168">*Case 3: Dequeue Ordering With Transaction Abort*</span></span>

<span data-ttu-id="b055a-169">Ruší se transakce s během letu dequeues PUT, které položky zpět na záhlaví fronty.</span><span class="sxs-lookup"><span data-stu-id="b055a-169">Aborting a transaction with in-flight dequeues puts the items back on the head of the queue.</span></span> <span data-ttu-id="b055a-170">Pořadí, ve které položky se vrátit zpět na záhlaví fronty není zaručena.</span><span class="sxs-lookup"><span data-stu-id="b055a-170">The order in which the items are put back on the head of the queue is not guaranteed.</span></span> <span data-ttu-id="b055a-171">Dejte nám podívejte se na následující kód:</span><span class="sxs-lookup"><span data-stu-id="b055a-171">Let us look at the following code:</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
<span data-ttu-id="b055a-172">Předpokládejme, že položky byly vyjmutou v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="b055a-172">Assume that the items were dequeued in the following order:</span></span>
> <span data-ttu-id="b055a-173">10, 20</span><span class="sxs-lookup"><span data-stu-id="b055a-173">10, 20</span></span>

<span data-ttu-id="b055a-174">Když jsme ji zrušit, by byl přidán položky zpět do head fronty v některém z následujících objednávky:</span><span class="sxs-lookup"><span data-stu-id="b055a-174">When we abort the transaction, the items would be added back to the head of the queue in any of the following orders:</span></span>
> <span data-ttu-id="b055a-175">10, 20</span><span class="sxs-lookup"><span data-stu-id="b055a-175">10, 20</span></span>

> <span data-ttu-id="b055a-176">20, 10</span><span class="sxs-lookup"><span data-stu-id="b055a-176">20, 10</span></span>

<span data-ttu-id="b055a-177">Totéž platí pro všechny případy, pokud transakce nebyla úspěšně *potvrzení*.</span><span class="sxs-lookup"><span data-stu-id="b055a-177">The same is true for all cases where the transaction was not successfully *Committed*.</span></span>

## <a name="programming-patterns"></a><span data-ttu-id="b055a-178">Vzory programování</span><span class="sxs-lookup"><span data-stu-id="b055a-178">Programming Patterns</span></span>
<span data-ttu-id="b055a-179">V této části, dejte nám podívejte se na několik programování vzorů, které mohou být užitečné při použití ReliableConcurrentQueue.</span><span class="sxs-lookup"><span data-stu-id="b055a-179">In this section, let us look at a few programming patterns that might be helpful in using ReliableConcurrentQueue.</span></span>

### <a name="batch-dequeues"></a><span data-ttu-id="b055a-180">Batch Dequeues</span><span class="sxs-lookup"><span data-stu-id="b055a-180">Batch Dequeues</span></span>
<span data-ttu-id="b055a-181">A doporučená programovací vzor je pro úlohu příjemce dávku jeho dequeues místo jedné dequeue – najednou.</span><span class="sxs-lookup"><span data-stu-id="b055a-181">A recommended programming pattern is for the consumer task to batch its dequeues instead of performing one dequeue at a time.</span></span> <span data-ttu-id="b055a-182">Uživatel může vybrat k omezení zpoždění mezi každou dávku nebo velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="b055a-182">The user can choose to throttle delays between every batch or the batch size.</span></span> <span data-ttu-id="b055a-183">Následující fragment kódu ukazuje tento programovací model.</span><span class="sxs-lookup"><span data-stu-id="b055a-183">The following code snippet shows this programming model.</span></span>  <span data-ttu-id="b055a-184">Všimněte si, že se v tomto příkladu, se provádí zpracování po, že je transakce potvrzena, nebo, pokud k chybě dochází při zpracování, nezpracované položky budou ztraceny bez zpracování.</span><span class="sxs-lookup"><span data-stu-id="b055a-184">Note that in this example, the processing is done after the transaction is committed, so if a fault were to occur while processing, the unprocessed items will be lost without having been processed.</span></span>  <span data-ttu-id="b055a-185">Alternativně zpracování lze provést v rámci oboru transakce, ale to může mít negativní dopad na výkon a vyžaduje zpracování položek, které jsou již zpracována.</span><span class="sxs-lookup"><span data-stu-id="b055a-185">Alternatively, the processing can be done within the transaction's scope, however this may have a negative impact on performance and requires handling of the items already processed.</span></span>

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
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break the for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a><span data-ttu-id="b055a-186">Zpracování oznámení na základě typu Best Effort</span><span class="sxs-lookup"><span data-stu-id="b055a-186">Best-Effort Notification-Based Processing</span></span>
<span data-ttu-id="b055a-187">Jiné zajímavé programovací vzor používá rozhraní API počet.</span><span class="sxs-lookup"><span data-stu-id="b055a-187">Another interesting programming pattern uses the Count API.</span></span> <span data-ttu-id="b055a-188">Zde můžeme implementovat best effort oznámení na základě zpracování pro frontu.</span><span class="sxs-lookup"><span data-stu-id="b055a-188">Here, we can implement best-effort notification-based processing for the queue.</span></span> <span data-ttu-id="b055a-189">Fronty počet slouží k omezení dequeue úlohy nebo zařazování.</span><span class="sxs-lookup"><span data-stu-id="b055a-189">The queue Count can be used to throttle an enqueue or a dequeue task.</span></span>  <span data-ttu-id="b055a-190">Všimněte si, že jako v předchozím příkladu vzhledem k tomu, že mimo transakci, proběhne zpracování nezpracovaných položky může dojít ke ztrátě Pokud dojde k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="b055a-190">Note that as in the previous example, since the processing occurs outside the transaction, unprocessed items may be lost if a fault occurs during processing.</span></span>

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If the queue does not have the threshold number of items, delay the task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process the queue

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
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a><span data-ttu-id="b055a-191">Best Effort vyprazdňování</span><span class="sxs-lookup"><span data-stu-id="b055a-191">Best-Effort Drain</span></span>
<span data-ttu-id="b055a-192">Vyprazdňování fronty nemůže zaručit z důvodu souběžných povaha datovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="b055a-192">A drain of the queue cannot be guaranteed due to the concurrent nature of the data structure.</span></span>  <span data-ttu-id="b055a-193">Je možné, že i když žádné operace uživatele na fronty jsou během letu, konkrétní volání TryDequeueAsync nemusí vracet položku, která byla dříve zařazených do fronty a potvrzené.</span><span class="sxs-lookup"><span data-stu-id="b055a-193">It is possible that, even if no user operations on the queue are in-flight, a particular call to TryDequeueAsync may not return an item which was previously enqueued and committed.</span></span>  <span data-ttu-id="b055a-194">Položka zařazených do fronty záruku, že *nakonec* budou zobrazeny na vyřazení z fronty, ale bez mechanismus komunikace out-of-band nezávislé příjemce nemůže vědět, že fronty dosáhl stabilní i když všechny generátory byla zastavena a nové zařazování, které jsou povolené operace.</span><span class="sxs-lookup"><span data-stu-id="b055a-194">The enqueued item is guaranteed to *eventually* become visible to dequeue, however without an out-of-band communication mechanism, an independent consumer cannot know that the queue has reached a steady-state even if all producers have been stopped and no new enqueue operations are allowed.</span></span> <span data-ttu-id="b055a-195">Operace vyprazdňování tedy best effort jak jsou implementované níže.</span><span class="sxs-lookup"><span data-stu-id="b055a-195">Thus, the drain operation is best-effort as implemented below.</span></span>

<span data-ttu-id="b055a-196">Uživatel by měl zastavit všechny další producent a úlohy příjemce a počkat na jakékoli během letu transakce potvrzení nebo přerušení, před pokusem o vyprazdňování fronty.</span><span class="sxs-lookup"><span data-stu-id="b055a-196">The user should stop all further producer and consumer tasks, and wait for any in-flight transactions to commit or abort, before attempting to drain the queue.</span></span>  <span data-ttu-id="b055a-197">Pokud uživatel nezná očekávaný počet položek ve frontě, můžete nastavit oznámení, která signalizuje, že máte byla vyjmutou všechny položky.</span><span class="sxs-lookup"><span data-stu-id="b055a-197">If the user knows the expected number of items in the queue, they can set up a notification which signals that all items have been dequeued.</span></span>

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
                // Buffer the dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a><span data-ttu-id="b055a-198">Prohlížet</span><span class="sxs-lookup"><span data-stu-id="b055a-198">Peek</span></span>
<span data-ttu-id="b055a-199">ReliableConcurrentQueue neposkytuje *TryPeekAsync* rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="b055a-199">ReliableConcurrentQueue does not provide the *TryPeekAsync* api.</span></span> <span data-ttu-id="b055a-200">Uživatelé dostanou prohlížení sémantického pomocí *TryDequeueAsync* a pak ruší se transakce.</span><span class="sxs-lookup"><span data-stu-id="b055a-200">Users can get the peek semantic by using a *TryDequeueAsync* and then aborting the transaction.</span></span> <span data-ttu-id="b055a-201">V tomto příkladu dequeues se zpracují pouze v případě, že je větší než hodnota položky *10*.</span><span class="sxs-lookup"><span data-stu-id="b055a-201">In this example, dequeues are processed only if the item's value is greater than *10*.</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process the item
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

## <a name="must-read"></a><span data-ttu-id="b055a-202">Musí pro čtení</span><span class="sxs-lookup"><span data-stu-id="b055a-202">Must Read</span></span>
* [<span data-ttu-id="b055a-203">Spolehlivé služby rychlý Start</span><span class="sxs-lookup"><span data-stu-id="b055a-203">Reliable Services Quick Start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="b055a-204">Práce s Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="b055a-204">Working with Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="b055a-205">Spolehlivé služby oznámení</span><span class="sxs-lookup"><span data-stu-id="b055a-205">Reliable Services notifications</span></span>](service-fabric-reliable-services-notifications.md)
* [<span data-ttu-id="b055a-206">Spolehlivé služby zálohování a obnovení (zotavení po havárii)</span><span class="sxs-lookup"><span data-stu-id="b055a-206">Reliable Services Backup and Restore (Disaster Recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="b055a-207">Spolehlivé stavu Správce konfigurace</span><span class="sxs-lookup"><span data-stu-id="b055a-207">Reliable State Manager Configuration</span></span>](service-fabric-reliable-services-configuration.md)
* [<span data-ttu-id="b055a-208">Začínáme se službou Service Fabric webové rozhraní API služby</span><span class="sxs-lookup"><span data-stu-id="b055a-208">Getting Started with Service Fabric Web API Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="b055a-209">Rozšířené použití spolehlivé služby programovací Model</span><span class="sxs-lookup"><span data-stu-id="b055a-209">Advanced Usage of the Reliable Services Programming Model</span></span>](service-fabric-reliable-services-advanced-usage.md)
* [<span data-ttu-id="b055a-210">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="b055a-210">Developer Reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
