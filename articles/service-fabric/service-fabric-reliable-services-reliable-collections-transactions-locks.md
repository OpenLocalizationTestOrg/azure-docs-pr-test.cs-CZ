---
title: "Spolehlivé kolekce prostředků infrastruktury služby transakce a uzamčení režimy v Azure | Microsoft Docs"
description: "Azure Service Fabric spolehlivé stavu Manager a spolehlivé kolekce transakce a uzamyká."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 3452473f5b2f86d29e46339c997193bc6403736a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a><span data-ttu-id="44770-103">Transakce a uzamčení režimy v Azure Service Fabric spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="44770-103">Transactions and lock modes in Azure Service Fabric Reliable Collections</span></span>

## <a name="transaction"></a><span data-ttu-id="44770-104">Transakce</span><span class="sxs-lookup"><span data-stu-id="44770-104">Transaction</span></span>
<span data-ttu-id="44770-105">Transakce je posloupnost operací provést jako jednu logickou jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="44770-105">A transaction is a sequence of operations performed as a single logical unit of work.</span></span>
<span data-ttu-id="44770-106">Transakce musí mít květy ACID následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="44770-106">A transaction must exhibit the following ACID properties.</span></span> <span data-ttu-id="44770-107">(viz: https://technet.microsoft.com/en-us/library/ms190612)</span><span class="sxs-lookup"><span data-stu-id="44770-107">(see: https://technet.microsoft.com/en-us/library/ms190612)</span></span>
* <span data-ttu-id="44770-108">**Nedělitelnost**: transakce musí být atomické jednotky práce.</span><span class="sxs-lookup"><span data-stu-id="44770-108">**Atomicity**: A transaction must be an atomic unit of work.</span></span> <span data-ttu-id="44770-109">Jinými slovy se provádí jeho změny dat, nebo žádná z nich probíhá.</span><span class="sxs-lookup"><span data-stu-id="44770-109">In other words, either all its data modifications are performed, or none of them is performed.</span></span>
* <span data-ttu-id="44770-110">**Konzistence**: Po dokončení transakce musí zůstat všechna data v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="44770-110">**Consistency**: When completed, a transaction must leave all data in a consistent state.</span></span> <span data-ttu-id="44770-111">Všechny interních datových strukturách musí být správné na konci transakce.</span><span class="sxs-lookup"><span data-stu-id="44770-111">All internal data structures must be correct at the end of the transaction.</span></span>
* <span data-ttu-id="44770-112">**Izolace**: úpravy provedené souběžných transakcí musí být izolované od změny provedené při dalších souběžných transakcí.</span><span class="sxs-lookup"><span data-stu-id="44770-112">**Isolation**: Modifications made by concurrent transactions must be isolated from the modifications made by any other concurrent transactions.</span></span> <span data-ttu-id="44770-113">Úroveň izolace, používat pro operace v rámci ITransaction je určen podle IReliableState provádění této operace.</span><span class="sxs-lookup"><span data-stu-id="44770-113">The isolation level used for an operation within an ITransaction is determined by the IReliableState performing the operation.</span></span>
* <span data-ttu-id="44770-114">**Odolnost**: Po dokončení transakce jeho účinky jsou trvale zavedené v systému.</span><span class="sxs-lookup"><span data-stu-id="44770-114">**Durability**: After a transaction has completed, its effects are permanently in place in the system.</span></span> <span data-ttu-id="44770-115">Změny zachovat i v případě selhání systému.</span><span class="sxs-lookup"><span data-stu-id="44770-115">The modifications persist even in the event of a system failure.</span></span>

### <a name="isolation-levels"></a><span data-ttu-id="44770-116">Úrovně izolace</span><span class="sxs-lookup"><span data-stu-id="44770-116">Isolation levels</span></span>
<span data-ttu-id="44770-117">Úroveň izolace definuje úroveň, do které musí být transakce izolované od změny provedené při dalších transakcí.</span><span class="sxs-lookup"><span data-stu-id="44770-117">Isolation level defines the degree to which the transaction must be isolated from modifications made by other transactions.</span></span>
<span data-ttu-id="44770-118">Existují dvě úrovně izolace, které jsou podporovány v spolehlivé kolekce:</span><span class="sxs-lookup"><span data-stu-id="44770-118">There are two isolation levels that are supported in Reliable Collections:</span></span>

* <span data-ttu-id="44770-119">**Opakovatelných čtení**: Určuje, že příkazy nelze číst data, která byla upravena, ale ještě nebyly potvrzeny podle dalších transakcí a že žádné další transakce můžete upravit data, která byla přečtena aktuální transakce, dokud nebude dokončeno aktuální transakci.</span><span class="sxs-lookup"><span data-stu-id="44770-119">**Repeatable Read**: Specifies that statements cannot read data that has been modified but not yet committed by other transactions and that no other transactions can modify data that has been read by the current transaction until the current transaction finishes.</span></span> <span data-ttu-id="44770-120">Další podrobnosti najdete v tématu [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).</span><span class="sxs-lookup"><span data-stu-id="44770-120">For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).</span></span>
* <span data-ttu-id="44770-121">**Snímek**: Určuje, že data načtená žádné příkazem v transakci je stavu transakční konzistence verzi data, která byla na začátku transakce.</span><span class="sxs-lookup"><span data-stu-id="44770-121">**Snapshot**: Specifies that data read by any statement in a transaction is the transactionally consistent version of the data that existed at the start of the transaction.</span></span>
  <span data-ttu-id="44770-122">Transakce poznáte pouze změny dat, které nebyly potvrzeny před zahájením transakce.</span><span class="sxs-lookup"><span data-stu-id="44770-122">The transaction can recognize only data modifications that were committed before the start of the transaction.</span></span>
  <span data-ttu-id="44770-123">Změny dat provedené dalších transakcí po začátku aktuální transakce nejsou viditelné pro příkazy v aktuální transakci.</span><span class="sxs-lookup"><span data-stu-id="44770-123">Data modifications made by other transactions after the start of the current transaction are not visible to statements executing in the current transaction.</span></span>
  <span data-ttu-id="44770-124">Výsledný efekt působí, jako kdyby příkazy v transakci získat snímek potvrdit dat tak, jak byly na začátku transakce.</span><span class="sxs-lookup"><span data-stu-id="44770-124">The effect is as if the statements in a transaction get a snapshot of the committed data as it existed at the start of the transaction.</span></span>
  <span data-ttu-id="44770-125">Snímky jsou konzistentní v rámci spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="44770-125">Snapshots are consistent across Reliable Collections.</span></span>
  <span data-ttu-id="44770-126">Další podrobnosti najdete v tématu [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).</span><span class="sxs-lookup"><span data-stu-id="44770-126">For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).</span></span>

<span data-ttu-id="44770-127">Spolehlivé kolekce automaticky zvolte úroveň izolace použitou pro danou operaci čtení v závislosti na operaci a role repliky v době vytvoření transakce.</span><span class="sxs-lookup"><span data-stu-id="44770-127">Reliable Collections automatically choose the isolation level to use for a given read operation depending on the operation and the role of the replica at the time of transaction's creation.</span></span>
<span data-ttu-id="44770-128">Následuje tabulka, která znázorňuje úrovni výchozí nastavení izolace pro operace spolehlivé slovník a fronty.</span><span class="sxs-lookup"><span data-stu-id="44770-128">Following is the table that depicts isolation level defaults for Reliable Dictionary and Queue operations.</span></span>

| <span data-ttu-id="44770-129">Operace \ Role</span><span class="sxs-lookup"><span data-stu-id="44770-129">Operation \ Role</span></span> | <span data-ttu-id="44770-130">Primární</span><span class="sxs-lookup"><span data-stu-id="44770-130">Primary</span></span> | <span data-ttu-id="44770-131">Sekundární</span><span class="sxs-lookup"><span data-stu-id="44770-131">Secondary</span></span> |
| --- |:--- |:--- |
| <span data-ttu-id="44770-132">Čtení jedné Entity</span><span class="sxs-lookup"><span data-stu-id="44770-132">Single Entity Read</span></span> |<span data-ttu-id="44770-133">Opakovatelných pro čtení</span><span class="sxs-lookup"><span data-stu-id="44770-133">Repeatable Read</span></span> |<span data-ttu-id="44770-134">Snímek</span><span class="sxs-lookup"><span data-stu-id="44770-134">Snapshot</span></span> |
| <span data-ttu-id="44770-135">Výčet, počet</span><span class="sxs-lookup"><span data-stu-id="44770-135">Enumeration, Count</span></span> |<span data-ttu-id="44770-136">Snímek</span><span class="sxs-lookup"><span data-stu-id="44770-136">Snapshot</span></span> |<span data-ttu-id="44770-137">Snímek</span><span class="sxs-lookup"><span data-stu-id="44770-137">Snapshot</span></span> |

> [!NOTE]
> <span data-ttu-id="44770-138">Běžných příkladů pro jednu entitu operace jsou `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.</span><span class="sxs-lookup"><span data-stu-id="44770-138">Common examples for Single Entity Operations are `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.</span></span>
> 

<span data-ttu-id="44770-139">Slovníku spolehlivé a spolehlivé fronty podporovat vaše zapíše pro čtení.</span><span class="sxs-lookup"><span data-stu-id="44770-139">Both the Reliable Dictionary and the Reliable Queue support Read Your Writes.</span></span>
<span data-ttu-id="44770-140">Jinými slovy všechny zápisu v rámci transakce budou viditelné pro následující pro čtení, který patří do stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="44770-140">In other words, any write within a transaction will be visible to a following read that belongs to the same transaction.</span></span>

## <a name="locks"></a><span data-ttu-id="44770-141">Zámky</span><span class="sxs-lookup"><span data-stu-id="44770-141">Locks</span></span>
<span data-ttu-id="44770-142">V kolekcích spolehlivé, všechny transakce implementace přísných dvě fáze uzamčení: transakce neuvolní zámky získala dokud neskončí transakce s přerušení nebo potvrzení.</span><span class="sxs-lookup"><span data-stu-id="44770-142">In Reliable Collections, all transactions implement rigorous two phase locking: a transaction does not release the locks it has acquired until the transaction terminates with either an abort or a commit.</span></span>

<span data-ttu-id="44770-143">Spolehlivé slovník používá uzamčení pro všechny operace jedné entity na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="44770-143">Reliable Dictionary uses row level locking for all single entity operations.</span></span>
<span data-ttu-id="44770-144">Spolehlivé fronty ztrátách souběžnosti pro striktní transakční FIFO vlastnost.</span><span class="sxs-lookup"><span data-stu-id="44770-144">Reliable Queue trades off concurrency for strict transactional FIFO property.</span></span>
<span data-ttu-id="44770-145">Spolehlivé fronty používá úrovně zámky operace povolení jednu transakci s `TryPeekAsync` nebo `TryDequeueAsync` a jednu transakci s `EnqueueAsync` najednou.</span><span class="sxs-lookup"><span data-stu-id="44770-145">Reliable Queue uses operation level locks allowing one transaction with `TryPeekAsync` and/or `TryDequeueAsync` and one transaction with `EnqueueAsync` at a time.</span></span>
<span data-ttu-id="44770-146">Všimněte si, že chcete zachovat FIFO, pokud `TryPeekAsync` nebo `TryDequeueAsync` někdy zjistí, že spolehlivé fronty je prázdný, budou také zamknout `EnqueueAsync`.</span><span class="sxs-lookup"><span data-stu-id="44770-146">Note that to preserve FIFO, if a `TryPeekAsync` or `TryDequeueAsync` ever observes that the Reliable Queue is empty, they will also lock `EnqueueAsync`.</span></span>

<span data-ttu-id="44770-147">Zápisu operace vždy provést výhradní zámky.</span><span class="sxs-lookup"><span data-stu-id="44770-147">Write operations always take Exclusive locks.</span></span>
<span data-ttu-id="44770-148">Pro operace čtení blokovací závisí na několika faktorech.</span><span class="sxs-lookup"><span data-stu-id="44770-148">For read operations, the locking depends on a couple of factors.</span></span>
<span data-ttu-id="44770-149">Všechny operace čtení provádí pomocí izolace snímku je zamykání.</span><span class="sxs-lookup"><span data-stu-id="44770-149">Any read operation done using Snapshot isolation is lock free.</span></span>
<span data-ttu-id="44770-150">Všechny operace čtení opakovatelných ve výchozím nastavení trvá sdílené zámky.</span><span class="sxs-lookup"><span data-stu-id="44770-150">Any Repeatable Read operation by default takes Shared locks.</span></span>
<span data-ttu-id="44770-151">Pro všechny operace čtení, podporující opakovatelných pro čtení, však může uživatel požádat o aktualizační zámek místo Sdílený zámek.</span><span class="sxs-lookup"><span data-stu-id="44770-151">However, for any read operation that supports Repeatable Read, the user can ask for an Update lock instead of the Shared lock.</span></span>
<span data-ttu-id="44770-152">Aktualizační zámek je zámek asymetrické používá při prevenci běžnou zablokování, který nastane, když více transakcí zamknutí prostředků pro potenciální aktualizace později.</span><span class="sxs-lookup"><span data-stu-id="44770-152">An Update lock is an asymmetric lock used to prevent a common form of deadlock that occurs when multiple transactions lock resources for potential updates at a later time.</span></span>

<span data-ttu-id="44770-153">Matice kompatibility zámku naleznete v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="44770-153">The lock compatibility matrix can be found in the following table:</span></span>

| <span data-ttu-id="44770-154">Žádosti o \ udělena</span><span class="sxs-lookup"><span data-stu-id="44770-154">Request \ Granted</span></span> | <span data-ttu-id="44770-155">Žádný</span><span class="sxs-lookup"><span data-stu-id="44770-155">None</span></span> | <span data-ttu-id="44770-156">Shared</span><span class="sxs-lookup"><span data-stu-id="44770-156">Shared</span></span> | <span data-ttu-id="44770-157">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="44770-157">Update</span></span> | <span data-ttu-id="44770-158">Exkluzivní</span><span class="sxs-lookup"><span data-stu-id="44770-158">Exclusive</span></span> |
| --- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="44770-159">Shared</span><span class="sxs-lookup"><span data-stu-id="44770-159">Shared</span></span> |<span data-ttu-id="44770-160">Ke konfliktu</span><span class="sxs-lookup"><span data-stu-id="44770-160">No conflict</span></span> |<span data-ttu-id="44770-161">Ke konfliktu</span><span class="sxs-lookup"><span data-stu-id="44770-161">No conflict</span></span> |<span data-ttu-id="44770-162">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-162">Conflict</span></span> |<span data-ttu-id="44770-163">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-163">Conflict</span></span> |
| <span data-ttu-id="44770-164">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="44770-164">Update</span></span> |<span data-ttu-id="44770-165">Ke konfliktu</span><span class="sxs-lookup"><span data-stu-id="44770-165">No conflict</span></span> |<span data-ttu-id="44770-166">Ke konfliktu</span><span class="sxs-lookup"><span data-stu-id="44770-166">No conflict</span></span> |<span data-ttu-id="44770-167">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-167">Conflict</span></span> |<span data-ttu-id="44770-168">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-168">Conflict</span></span> |
| <span data-ttu-id="44770-169">Exkluzivní</span><span class="sxs-lookup"><span data-stu-id="44770-169">Exclusive</span></span> |<span data-ttu-id="44770-170">Ke konfliktu</span><span class="sxs-lookup"><span data-stu-id="44770-170">No conflict</span></span> |<span data-ttu-id="44770-171">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-171">Conflict</span></span> |<span data-ttu-id="44770-172">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-172">Conflict</span></span> |<span data-ttu-id="44770-173">Konflikt</span><span class="sxs-lookup"><span data-stu-id="44770-173">Conflict</span></span> |

<span data-ttu-id="44770-174">Časový limit argument spolehlivé rozhraní API kolekce se používá pro zjišťování zablokování.</span><span class="sxs-lookup"><span data-stu-id="44770-174">Time-out argument in the Reliable Collections APIs is used for deadlock detection.</span></span>
<span data-ttu-id="44770-175">Dvě transakce (T1 a T2) se například pokoušíte číst a aktualizovat K1.</span><span class="sxs-lookup"><span data-stu-id="44770-175">For example, two transactions (T1 and T2) are trying to read and update K1.</span></span>
<span data-ttu-id="44770-176">Je možné pro ně k zablokování, protože obě ukončení tím, že mají Sdílený zámek.</span><span class="sxs-lookup"><span data-stu-id="44770-176">It is possible for them to deadlock, because they both end up having the Shared lock.</span></span>
<span data-ttu-id="44770-177">Jedna nebo obě operací v tomto případě bude časový limit.</span><span class="sxs-lookup"><span data-stu-id="44770-177">In this case, one or both of the operations will time out.</span></span>

<span data-ttu-id="44770-178">Tento scénář vzájemného zablokování je skvělým příkladem jak aktualizační zámek může zabránit zablokování.</span><span class="sxs-lookup"><span data-stu-id="44770-178">This deadlock scenario is a great example of how an Update lock can prevent deadlocks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44770-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44770-179">Next steps</span></span>
* [<span data-ttu-id="44770-180">Práce s Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="44770-180">Working with Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="44770-181">Spolehlivé služby oznámení</span><span class="sxs-lookup"><span data-stu-id="44770-181">Reliable Services notifications</span></span>](service-fabric-reliable-services-notifications.md)
* [<span data-ttu-id="44770-182">Spolehlivé služby zálohování a obnovení (zotavení po havárii)</span><span class="sxs-lookup"><span data-stu-id="44770-182">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="44770-183">Configuration Manager spolehlivé stavu</span><span class="sxs-lookup"><span data-stu-id="44770-183">Reliable State Manager configuration</span></span>](service-fabric-reliable-services-configuration.md)
* [<span data-ttu-id="44770-184">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="44770-184">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
