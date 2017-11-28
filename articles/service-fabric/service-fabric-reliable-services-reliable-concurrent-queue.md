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
# <a name="introduction-tooreliableconcurrentqueue-in-azure-service-fabric"></a><span data-ttu-id="04ab2-103">Úvod tooReliableConcurrentQueue v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="04ab2-103">Introduction tooReliableConcurrentQueue in Azure Service Fabric</span></span>
<span data-ttu-id="04ab2-104">Spolehlivé souběžných fronty je frontu asynchronní, transakční a replikované které funkce vysoké souběžnosti pro zařazování a dequeue – operace.</span><span class="sxs-lookup"><span data-stu-id="04ab2-104">Reliable Concurrent Queue is an asynchronous, transactional, and replicated queue which features high concurrency for enqueue and dequeue operations.</span></span> <span data-ttu-id="04ab2-105">Je navrženou toodeliver vysoké prostupnosti a nízké latence podle uvolnit hello striktní FIFO řazení poskytované [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) a místo toho poskytuje best effort řazení.</span><span class="sxs-lookup"><span data-stu-id="04ab2-105">It is designed toodeliver high throughput and low latency by relaxing hello strict FIFO ordering provided by [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) and instead provides a best-effort ordering.</span></span>

## <a name="apis"></a><span data-ttu-id="04ab2-106">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="04ab2-106">APIs</span></span>

|<span data-ttu-id="04ab2-107">Souběžné fronty</span><span class="sxs-lookup"><span data-stu-id="04ab2-107">Concurrent Queue</span></span>                |<span data-ttu-id="04ab2-108">Spolehlivé souběžných fronty</span><span class="sxs-lookup"><span data-stu-id="04ab2-108">Reliable Concurrent Queue</span></span>                                         |
|--------------------------------|------------------------------------------------------------------|
| <span data-ttu-id="04ab2-109">void Enqueue(T item)</span><span class="sxs-lookup"><span data-stu-id="04ab2-109">void Enqueue(T item)</span></span>           | <span data-ttu-id="04ab2-110">Úloha EnqueueAsync (ITransaction tx, položka T)</span><span class="sxs-lookup"><span data-stu-id="04ab2-110">Task EnqueueAsync(ITransaction tx, T item)</span></span>                       |
| <span data-ttu-id="04ab2-111">BOOL TryDequeue (out výsledek T)</span><span class="sxs-lookup"><span data-stu-id="04ab2-111">bool TryDequeue(out T result)</span></span>  | <span data-ttu-id="04ab2-112">Úloha < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)</span><span class="sxs-lookup"><span data-stu-id="04ab2-112">Task< ConditionalValue < T > > TryDequeueAsync(ITransaction tx)</span></span>  |
| <span data-ttu-id="04ab2-113">int Count()</span><span class="sxs-lookup"><span data-stu-id="04ab2-113">int Count()</span></span>                    | <span data-ttu-id="04ab2-114">dlouhé Count()</span><span class="sxs-lookup"><span data-stu-id="04ab2-114">long Count()</span></span>                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a><span data-ttu-id="04ab2-115">Porovnání s [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span><span class="sxs-lookup"><span data-stu-id="04ab2-115">Comparison with [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span></span>

<span data-ttu-id="04ab2-116">Spolehlivé souběžných fronty je poskytován jako alternativu příliš[spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span><span class="sxs-lookup"><span data-stu-id="04ab2-116">Reliable Concurrent Queue is offered as an alternative too[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span></span> <span data-ttu-id="04ab2-117">By být používána v případech, kde není vyžadována, striktní řazení FIFO jako zaručující FIFO vyžaduje kompromis s souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="04ab2-117">It should be used in cases where strict FIFO ordering is not required, as guaranteeing FIFO requires a tradeoff with concurrency.</span></span>  <span data-ttu-id="04ab2-118">[Spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) používá zámky tooenforce FIFO řazení, s maximálně jednu transakci povolen tooenqueue a na většině jednu transakci toodequeue v jednu chvíli může.</span><span class="sxs-lookup"><span data-stu-id="04ab2-118">[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) uses locks tooenforce FIFO ordering, with at most one transaction allowed tooenqueue and at most one transaction allowed toodequeue at a time.</span></span> <span data-ttu-id="04ab2-119">Porovnání spolehlivé souběžných fronty zmírní hello řazení omezení a umožňuje jejich zařadit všechny počet souběžných transakcí toointerleave a dequeue – operace.</span><span class="sxs-lookup"><span data-stu-id="04ab2-119">In comparison, Reliable Concurrent Queue relaxes hello ordering constraint and allows any number concurrent transactions toointerleave their enqueue and dequeue operations.</span></span> <span data-ttu-id="04ab2-120">Řazení typu Best effort je zadáno, ale relativní řazení hello ze dvou hodnot v spolehlivé souběžných frontě možné nikdy zaručit.</span><span class="sxs-lookup"><span data-stu-id="04ab2-120">Best-effort ordering is provided, however hello relative ordering of two values in a Reliable Concurrent Queue can never be guaranteed.</span></span>

<span data-ttu-id="04ab2-121">Poskytuje spolehlivé souběžných fronty vyšší propustnost a nižší latenci než [spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx) vždy, když jsou více souběžných transakcí provádění enqueues nebo dequeues.</span><span class="sxs-lookup"><span data-stu-id="04ab2-121">Reliable Concurrent Queue provides higher throughput and lower latency than [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) whenever there are multiple concurrent transactions performing enqueues and/or dequeues.</span></span>

<span data-ttu-id="04ab2-122">Ukázka případ použití hello ReliableConcurrentQueue je hello [fronta zpráv](https://en.wikipedia.org/wiki/Message_queue) scénář.</span><span class="sxs-lookup"><span data-stu-id="04ab2-122">A sample use case for hello ReliableConcurrentQueue is hello [Message Queue](https://en.wikipedia.org/wiki/Message_queue) scenario.</span></span> <span data-ttu-id="04ab2-123">V tomto scénáři jedním nebo více výrobci zprávu vytvořit a přidat položky toohello fronty, a jeden nebo více zpráv příjemci načítat zprávy z fronty hello a jejich zpracování.</span><span class="sxs-lookup"><span data-stu-id="04ab2-123">In this scenario, one or more message producers create and add items toohello queue, and one or more message consumers pull messages from hello queue and process them.</span></span> <span data-ttu-id="04ab2-124">Více producenti a spotřebitelé může nezávisle pracovat, použití souběžných transakcí ve frontě hello tooprocess pořadí.</span><span class="sxs-lookup"><span data-stu-id="04ab2-124">Multiple producers and consumers can work independently, using concurrent transactions in order tooprocess hello queue.</span></span>

## <a name="usage-guidelines"></a><span data-ttu-id="04ab2-125">Pokyny týkající se používání</span><span class="sxs-lookup"><span data-stu-id="04ab2-125">Usage Guidelines</span></span>
* <span data-ttu-id="04ab2-126">fronty Hello očekává, že hello položek ve frontě hello mají doby uchování nízkou.</span><span class="sxs-lookup"><span data-stu-id="04ab2-126">hello queue expects that hello items in hello queue have a low retention period.</span></span> <span data-ttu-id="04ab2-127">To znamená hello položky by zůstat ve frontě hello po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="04ab2-127">That is, hello items would not stay in hello queue for a long time.</span></span>
* <span data-ttu-id="04ab2-128">fronty Hello nezaručuje striktní FIFO řazení.</span><span class="sxs-lookup"><span data-stu-id="04ab2-128">hello queue does not guarantee strict FIFO ordering.</span></span>
* <span data-ttu-id="04ab2-129">fronty Hello číst vlastní zápisy.</span><span class="sxs-lookup"><span data-stu-id="04ab2-129">hello queue does not read its own writes.</span></span> <span data-ttu-id="04ab2-130">Pokud je položka zařazených do fronty v rámci transakce, nebude viditelná tooa dequeuer v rámci hello stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="04ab2-130">If an item is enqueued within a transaction, it will not be visible tooa dequeuer within hello same transaction.</span></span>
* <span data-ttu-id="04ab2-131">Dequeues nejsou od sebe navzájem oddělené.</span><span class="sxs-lookup"><span data-stu-id="04ab2-131">Dequeues are not isolated from each other.</span></span> <span data-ttu-id="04ab2-132">Pokud položka *A* je vyjmutou v transakci *txnA*, i když *txnA* položky není potvrzena, *A* nebudou viditelné tooa souběžných transakce *txnB*.</span><span class="sxs-lookup"><span data-stu-id="04ab2-132">If item *A* is dequeued in transaction *txnA*, even though *txnA* is not committed, item *A* would not be visible tooa concurrent transaction *txnB*.</span></span>  <span data-ttu-id="04ab2-133">Pokud *txnA* zruší, *A* se bude zobrazovat příliš*txnB* okamžitě.</span><span class="sxs-lookup"><span data-stu-id="04ab2-133">If *txnA* aborts, *A* will become visible too*txnB* immediately.</span></span>
* <span data-ttu-id="04ab2-134">*TryPeekAsync* chování může být implementováno pomocí *TryDequeueAsync* a pak ruší se transakce hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-134">*TryPeekAsync* behavior can be implemented by using a *TryDequeueAsync* and then aborting hello transaction.</span></span> <span data-ttu-id="04ab2-135">Příklady najdete v hello části programování vzory.</span><span class="sxs-lookup"><span data-stu-id="04ab2-135">An example of this can be found in hello Programming Patterns section.</span></span>
* <span data-ttu-id="04ab2-136">Počet je netransakční.</span><span class="sxs-lookup"><span data-stu-id="04ab2-136">Count is non-transactional.</span></span> <span data-ttu-id="04ab2-137">Může být použité tooget představu o hello počet elementů ve frontě hello, ale představuje bodu v čase a nelze spoléhat.</span><span class="sxs-lookup"><span data-stu-id="04ab2-137">It can be used tooget an idea of hello number of elements in hello queue, but represents a point-in-time and cannot be relied upon.</span></span>
* <span data-ttu-id="04ab2-138">Nákladné zpracování na hello vyjmutou položky nesmí provést v průběhu hello transakcí je aktivní, tooavoid dlouhotrvajících transakcí, které by mohly mít dopad na výkon systému hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-138">Expensive processing on hello dequeued items should not be performed while hello transaction is active, tooavoid long-running transactions which may have a performance impact on hello system.</span></span>

## <a name="code-snippets"></a><span data-ttu-id="04ab2-139">Fragmenty kódu</span><span class="sxs-lookup"><span data-stu-id="04ab2-139">Code Snippets</span></span>
<span data-ttu-id="04ab2-140">Dejte nám se podívejte na několik fragmenty kódu a jejich očekávané výstupy.</span><span class="sxs-lookup"><span data-stu-id="04ab2-140">Let us look at a few code snippets and their expected outputs.</span></span> <span data-ttu-id="04ab2-141">Zpracovávání výjimek v jazyce je ignorován v této části.</span><span class="sxs-lookup"><span data-stu-id="04ab2-141">Exception handling is ignored in this section.</span></span>

### <a name="enqueueasync"></a><span data-ttu-id="04ab2-142">EnqueueAsync</span><span class="sxs-lookup"><span data-stu-id="04ab2-142">EnqueueAsync</span></span>
<span data-ttu-id="04ab2-143">Tady je několik fragmenty kódu pro použití EnqueueAsync následuje jejich očekávané výstupy.</span><span class="sxs-lookup"><span data-stu-id="04ab2-143">Here are a few code snippets for using EnqueueAsync followed by their expected outputs.</span></span>

- <span data-ttu-id="04ab2-144">*Případ 1: Zařazování úloh jeden*</span><span class="sxs-lookup"><span data-stu-id="04ab2-144">*Case 1: Single Enqueue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="04ab2-145">Předpokládejme, že hello úloha byla úspěšně dokončena, a který nebyly žádné souběžných transakcí úprava fronty hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-145">Assume that hello task completed successfully, and that there were no concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="04ab2-146">uživatel, Hello očekávat hello fronty toocontain hello položky v některém z hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="04ab2-146">hello user can expect hello queue toocontain hello items in any of hello following orders:</span></span>

> <span data-ttu-id="04ab2-147">10, 20</span><span class="sxs-lookup"><span data-stu-id="04ab2-147">10, 20</span></span>

> <span data-ttu-id="04ab2-148">20, 10</span><span class="sxs-lookup"><span data-stu-id="04ab2-148">20, 10</span></span>


- <span data-ttu-id="04ab2-149">*Případ 2: Zařazování úloh paralelní*</span><span class="sxs-lookup"><span data-stu-id="04ab2-149">*Case 2: Parallel Enqueue Task*</span></span>

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

<span data-ttu-id="04ab2-150">Předpokládejme, že hello úlohy byl úspěšně dokončen, jestli hello úlohy spustila paralelně a že neexistují žádné souběžných transakcí úprava fronty hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-150">Assume that hello tasks completed successfully, that hello tasks ran in parallel, and that there were no other concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="04ab2-151">Žádné odvození můžete provedeny o hello pořadí položek ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-151">No inference can be made about hello order of items in hello queue.</span></span> <span data-ttu-id="04ab2-152">Pro tento fragment kódu může se zobrazit hello položky v některém z hello 4!</span><span class="sxs-lookup"><span data-stu-id="04ab2-152">For this code snippet, hello items may appear in any of hello 4!</span></span> <span data-ttu-id="04ab2-153">možné pořadí.</span><span class="sxs-lookup"><span data-stu-id="04ab2-153">possible orderings.</span></span>  <span data-ttu-id="04ab2-154">Hello fronty se pokusí tookeep hello položky v hello původní (zařazených do fronty) pořadí, ale může být vynucené tooreorder je z důvodu operace tooconcurrent nebo chyb.</span><span class="sxs-lookup"><span data-stu-id="04ab2-154">hello queue will attempt tookeep hello items in hello original (enqueued) order, but may be forced tooreorder them due tooconcurrent operations or faults.</span></span>


### <a name="dequeueasync"></a><span data-ttu-id="04ab2-155">DequeueAsync</span><span class="sxs-lookup"><span data-stu-id="04ab2-155">DequeueAsync</span></span>
<span data-ttu-id="04ab2-156">Tady je několik fragmenty kódu pro použití TryDequeueAsync následuje výstupy hello očekává.</span><span class="sxs-lookup"><span data-stu-id="04ab2-156">Here are a few code snippets for using TryDequeueAsync followed by hello expected outputs.</span></span> <span data-ttu-id="04ab2-157">Předpokládejme, že této fronty hello je již naplněný hello následujících položek ve frontě hello:</span><span class="sxs-lookup"><span data-stu-id="04ab2-157">Assume that hello queue is already populated with hello following items in hello queue:</span></span>
> <span data-ttu-id="04ab2-158">10, 20, 30, 40, 50, 60</span><span class="sxs-lookup"><span data-stu-id="04ab2-158">10, 20, 30, 40, 50, 60</span></span>

- <span data-ttu-id="04ab2-159">*Případ 1: Jeden dequeue – úloha*</span><span class="sxs-lookup"><span data-stu-id="04ab2-159">*Case 1: Single Dequeue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="04ab2-160">Předpokládejme, že hello úloha byla úspěšně dokončena, a který nebyly žádné souběžných transakcí úprava fronty hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-160">Assume that hello task completed successfully, and that there were no concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="04ab2-161">Vzhledem k tomu, že o hello pořadí hello položek ve frontě hello nelze realizovat žádná odvození, může být vyjmutou všechny tři položky hello v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="04ab2-161">Since no inference can be made about hello order of hello items in hello queue, any three of hello items may be dequeued, in any order.</span></span> <span data-ttu-id="04ab2-162">Hello fronty se pokusí tookeep hello položky v hello původní (zařazených do fronty) pořadí, ale může být vynucené tooreorder je z důvodu operace tooconcurrent nebo chyb.</span><span class="sxs-lookup"><span data-stu-id="04ab2-162">hello queue will attempt tookeep hello items in hello original (enqueued) order, but may be forced tooreorder them due tooconcurrent operations or faults.</span></span>  

- <span data-ttu-id="04ab2-163">*Případ 2: Dequeue – paralelní úlohy*</span><span class="sxs-lookup"><span data-stu-id="04ab2-163">*Case 2: Parallel Dequeue Task*</span></span>

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

<span data-ttu-id="04ab2-164">Předpokládejme, že hello úlohy byl úspěšně dokončen, jestli hello úlohy spustila paralelně a že neexistují žádné souběžných transakcí úprava fronty hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-164">Assume that hello tasks completed successfully, that hello tasks ran in parallel, and that there were no other concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="04ab2-165">Vzhledem k tomu, že o hello pořadí hello položek ve frontě hello nelze realizovat žádná odvození, hello seznamy *dequeue1* a *dequeue2* bude každý obsahovat jakékoli dvě položky v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="04ab2-165">Since no inference can be made about hello order of hello items in hello queue, hello lists *dequeue1* and *dequeue2* will each contain any two items, in any order.</span></span>

<span data-ttu-id="04ab2-166">Hello se stejnou položku *není* se zobrazí v obou seznamy.</span><span class="sxs-lookup"><span data-stu-id="04ab2-166">hello same item will *not* appear in both lists.</span></span> <span data-ttu-id="04ab2-167">Proto pokud má dequeue1 *10*, *30*, pak by měla mít dequeue2 *20*, *40*.</span><span class="sxs-lookup"><span data-stu-id="04ab2-167">Hence, if dequeue1 has *10*, *30*, then dequeue2 would have *20*, *40*.</span></span>

- <span data-ttu-id="04ab2-168">*Případ 3: Dequeue – při řazení s přerušení transakcí*</span><span class="sxs-lookup"><span data-stu-id="04ab2-168">*Case 3: Dequeue Ordering With Transaction Abort*</span></span>

<span data-ttu-id="04ab2-169">Ruší se transakce s během letu dequeues PUT, které položky hello zpět na záhlaví hello hello fronty.</span><span class="sxs-lookup"><span data-stu-id="04ab2-169">Aborting a transaction with in-flight dequeues puts hello items back on hello head of hello queue.</span></span> <span data-ttu-id="04ab2-170">Hello pořadí, ve kterém hello položky se vrátit zpět na záhlaví hello hello fronty není zaručena.</span><span class="sxs-lookup"><span data-stu-id="04ab2-170">hello order in which hello items are put back on hello head of hello queue is not guaranteed.</span></span> <span data-ttu-id="04ab2-171">Dejte nám podívejte se na hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="04ab2-171">Let us look at hello following code:</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort hello transaction
    await txn.AbortAsync();
}
```
<span data-ttu-id="04ab2-172">Předpokládejme, že hello položky byly vyjmutou v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="04ab2-172">Assume that hello items were dequeued in hello following order:</span></span>
> <span data-ttu-id="04ab2-173">10, 20</span><span class="sxs-lookup"><span data-stu-id="04ab2-173">10, 20</span></span>

<span data-ttu-id="04ab2-174">Když jsme abort hello transakce, hello položky by byl přidán back toohello head hello fronty v některém z hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="04ab2-174">When we abort hello transaction, hello items would be added back toohello head of hello queue in any of hello following orders:</span></span>
> <span data-ttu-id="04ab2-175">10, 20</span><span class="sxs-lookup"><span data-stu-id="04ab2-175">10, 20</span></span>

> <span data-ttu-id="04ab2-176">20, 10</span><span class="sxs-lookup"><span data-stu-id="04ab2-176">20, 10</span></span>

<span data-ttu-id="04ab2-177">Hello totéž platí pro všechny případy, kdy hello transakce nebyla úspěšně *potvrzení*.</span><span class="sxs-lookup"><span data-stu-id="04ab2-177">hello same is true for all cases where hello transaction was not successfully *Committed*.</span></span>

## <a name="programming-patterns"></a><span data-ttu-id="04ab2-178">Vzory programování</span><span class="sxs-lookup"><span data-stu-id="04ab2-178">Programming Patterns</span></span>
<span data-ttu-id="04ab2-179">V této části, dejte nám podívejte se na několik programování vzorů, které mohou být užitečné při použití ReliableConcurrentQueue.</span><span class="sxs-lookup"><span data-stu-id="04ab2-179">In this section, let us look at a few programming patterns that might be helpful in using ReliableConcurrentQueue.</span></span>

### <a name="batch-dequeues"></a><span data-ttu-id="04ab2-180">Batch Dequeues</span><span class="sxs-lookup"><span data-stu-id="04ab2-180">Batch Dequeues</span></span>
<span data-ttu-id="04ab2-181">A doporučená programovací vzor je pro hello příjemce úloh toobatch jeho dequeues místo jedné dequeue – najednou.</span><span class="sxs-lookup"><span data-stu-id="04ab2-181">A recommended programming pattern is for hello consumer task toobatch its dequeues instead of performing one dequeue at a time.</span></span> <span data-ttu-id="04ab2-182">Hello uživatel může vybrat toothrottle zpoždění mezi každou dávka nebo hello velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="04ab2-182">hello user can choose toothrottle delays between every batch or hello batch size.</span></span> <span data-ttu-id="04ab2-183">Hello následující fragment kódu ukazuje tento programovací model.</span><span class="sxs-lookup"><span data-stu-id="04ab2-183">hello following code snippet shows this programming model.</span></span>  <span data-ttu-id="04ab2-184">Všimněte si, že v tomto příkladu hello dokončení zpracování po hello transakce se potvrdí, takže pokud chybu byly toooccur při zpracování, hello nezpracované položky budou ztraceny bez zpracování.</span><span class="sxs-lookup"><span data-stu-id="04ab2-184">Note that in this example, hello processing is done after hello transaction is committed, so if a fault were toooccur while processing, hello unprocessed items will be lost without having been processed.</span></span>  <span data-ttu-id="04ab2-185">Alternativně lze provést hello zpracování v rámci oboru hello transakce, ale to může mít negativní dopad na výkon a vyžaduje zpracování položek hello již zpracovány.</span><span class="sxs-lookup"><span data-stu-id="04ab2-185">Alternatively, hello processing can be done within hello transaction's scope, however this may have a negative impact on performance and requires handling of hello items already processed.</span></span>

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

### <a name="best-effort-notification-based-processing"></a><span data-ttu-id="04ab2-186">Zpracování oznámení na základě typu Best Effort</span><span class="sxs-lookup"><span data-stu-id="04ab2-186">Best-Effort Notification-Based Processing</span></span>
<span data-ttu-id="04ab2-187">Jiné zajímavé programovací vzor používá hello počet rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="04ab2-187">Another interesting programming pattern uses hello Count API.</span></span> <span data-ttu-id="04ab2-188">Zde můžeme implementovat zpracování oznámení na základě typu best effort pro frontu hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-188">Here, we can implement best-effort notification-based processing for hello queue.</span></span> <span data-ttu-id="04ab2-189">fronty Hello počet může být použité toothrottle element zařazování nebo dequeue úloh.</span><span class="sxs-lookup"><span data-stu-id="04ab2-189">hello queue Count can be used toothrottle an enqueue or a dequeue task.</span></span>  <span data-ttu-id="04ab2-190">Všimněte si, že jako v předchozím příkladu hello, vzhledem k tomu, že zpracování hello proběhne mimo transakci hello nezpracované položky může dojít ke ztrátě Pokud dojde k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="04ab2-190">Note that as in hello previous example, since hello processing occurs outside hello transaction, unprocessed items may be lost if a fault occurs during processing.</span></span>

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

### <a name="best-effort-drain"></a><span data-ttu-id="04ab2-191">Best Effort vyprazdňování</span><span class="sxs-lookup"><span data-stu-id="04ab2-191">Best-Effort Drain</span></span>
<span data-ttu-id="04ab2-192">Z důvodu souběžných povaha hello datová struktura toohello nelze zaručit vyprazdňování hello fronty.</span><span class="sxs-lookup"><span data-stu-id="04ab2-192">A drain of hello queue cannot be guaranteed due toohello concurrent nature of hello data structure.</span></span>  <span data-ttu-id="04ab2-193">Je možné, že i když jsou neukládají žádné uživatele operace ve frontě hello, konkrétní volání tooTryDequeueAsync nemusí vracet položku, která byla dříve zařazených do fronty a potvrzené.</span><span class="sxs-lookup"><span data-stu-id="04ab2-193">It is possible that, even if no user operations on hello queue are in-flight, a particular call tooTryDequeueAsync may not return an item which was previously enqueued and committed.</span></span>  <span data-ttu-id="04ab2-194">Hello zařazených do fronty položek záruku, že se příliš*nakonec* stát viditelné toodequeue, ale bez mechanismus komunikace out-of-band nemůže vědět nezávislé příjemce této fronty hello dosáhl stabilní i když všechny jsou povoleny žádné nové operace zařazování a producenti byly zastaveny.</span><span class="sxs-lookup"><span data-stu-id="04ab2-194">hello enqueued item is guaranteed too*eventually* become visible toodequeue, however without an out-of-band communication mechanism, an independent consumer cannot know that hello queue has reached a steady-state even if all producers have been stopped and no new enqueue operations are allowed.</span></span> <span data-ttu-id="04ab2-195">Operace vyprazdňování hello tedy best effort jak jsou implementované níže.</span><span class="sxs-lookup"><span data-stu-id="04ab2-195">Thus, hello drain operation is best-effort as implemented below.</span></span>

<span data-ttu-id="04ab2-196">Hello uživatel by měl zastavit všechny další producent a úlohy příjemce a počkejte, všechny toocommit během letu transakce nebo přerušení, před pokusem o toodrain hello fronty.</span><span class="sxs-lookup"><span data-stu-id="04ab2-196">hello user should stop all further producer and consumer tasks, and wait for any in-flight transactions toocommit or abort, before attempting toodrain hello queue.</span></span>  <span data-ttu-id="04ab2-197">Pokud uživatel hello nezná hello očekávaný počet položek ve frontě hello, můžete nastavit oznámení, která signalizuje, že máte byla vyjmutou všechny položky.</span><span class="sxs-lookup"><span data-stu-id="04ab2-197">If hello user knows hello expected number of items in hello queue, they can set up a notification which signals that all items have been dequeued.</span></span>

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

### <a name="peek"></a><span data-ttu-id="04ab2-198">Prohlížet</span><span class="sxs-lookup"><span data-stu-id="04ab2-198">Peek</span></span>
<span data-ttu-id="04ab2-199">ReliableConcurrentQueue neposkytuje hello *TryPeekAsync* rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="04ab2-199">ReliableConcurrentQueue does not provide hello *TryPeekAsync* api.</span></span> <span data-ttu-id="04ab2-200">Uživatelé dostanou funkce Náhled hello sémantického pomocí *TryDequeueAsync* a pak ruší se transakce hello.</span><span class="sxs-lookup"><span data-stu-id="04ab2-200">Users can get hello peek semantic by using a *TryDequeueAsync* and then aborting hello transaction.</span></span> <span data-ttu-id="04ab2-201">V tomto příkladu dequeues se zpracují pouze v případě, že je větší než hodnota položky hello *10*.</span><span class="sxs-lookup"><span data-stu-id="04ab2-201">In this example, dequeues are processed only if hello item's value is greater than *10*.</span></span>

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

## <a name="must-read"></a><span data-ttu-id="04ab2-202">Musí pro čtení</span><span class="sxs-lookup"><span data-stu-id="04ab2-202">Must Read</span></span>
* [<span data-ttu-id="04ab2-203">Spolehlivé služby rychlý Start</span><span class="sxs-lookup"><span data-stu-id="04ab2-203">Reliable Services Quick Start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="04ab2-204">Práce s Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="04ab2-204">Working with Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="04ab2-205">Spolehlivé služby oznámení</span><span class="sxs-lookup"><span data-stu-id="04ab2-205">Reliable Services notifications</span></span>](service-fabric-reliable-services-notifications.md)
* [<span data-ttu-id="04ab2-206">Spolehlivé služby zálohování a obnovení (zotavení po havárii)</span><span class="sxs-lookup"><span data-stu-id="04ab2-206">Reliable Services Backup and Restore (Disaster Recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="04ab2-207">Spolehlivé stavu Správce konfigurace</span><span class="sxs-lookup"><span data-stu-id="04ab2-207">Reliable State Manager Configuration</span></span>](service-fabric-reliable-services-configuration.md)
* [<span data-ttu-id="04ab2-208">Začínáme se službou Service Fabric webové rozhraní API služby</span><span class="sxs-lookup"><span data-stu-id="04ab2-208">Getting Started with Service Fabric Web API Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="04ab2-209">Rozšířené použití hello spolehlivé služby programovací Model</span><span class="sxs-lookup"><span data-stu-id="04ab2-209">Advanced Usage of hello Reliable Services Programming Model</span></span>](service-fabric-reliable-services-advanced-usage.md)
* [<span data-ttu-id="04ab2-210">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="04ab2-210">Developer Reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
