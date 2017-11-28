---
title: "Spolehlivé služby oznámení | Microsoft Docs"
description: "Rámcová dokumentace pro oznámení služby Fabric spolehlivé služby"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: c6a53d851510ed5e6eec1f3ac0f636ad034a6d4c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-services-notifications"></a><span data-ttu-id="d09a4-103">Spolehlivé služby oznámení</span><span class="sxs-lookup"><span data-stu-id="d09a4-103">Reliable Services notifications</span></span>
<span data-ttu-id="d09a4-104">Oznámení klientům sledovat změny provedené na objekt, který se vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="d09a4-104">Notifications allow clients to track the changes that are being made to an object that they're interested in.</span></span> <span data-ttu-id="d09a4-105">Oznámení podporují dva typy objektů: *spolehlivé správce stavu* a *spolehlivé slovník*.</span><span class="sxs-lookup"><span data-stu-id="d09a4-105">Two types of objects support notifications: *Reliable State Manager* and *Reliable Dictionary*.</span></span>

<span data-ttu-id="d09a4-106">Běžnými důvody používání oznámení jsou:</span><span class="sxs-lookup"><span data-stu-id="d09a4-106">Common reasons for using notifications are:</span></span>

* <span data-ttu-id="d09a4-107">Vytváření materializována zobrazení, jako je například sekundární indexy nebo agregovat filtrovaných zobrazení stavu repliky.</span><span class="sxs-lookup"><span data-stu-id="d09a4-107">Building materialized views, such as secondary indexes or aggregated filtered views of the replica's state.</span></span> <span data-ttu-id="d09a4-108">Příkladem je seřazená index všechny klíče ve slovníku spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="d09a4-108">An example is a sorted index of all keys in Reliable Dictionary.</span></span>
* <span data-ttu-id="d09a4-109">Odesílání monitorování dat, například počet uživatelů, které jsou přidány za poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="d09a4-109">Sending monitoring data, such as the number of users added in the last hour.</span></span>

<span data-ttu-id="d09a4-110">Oznámení při vyvolání jako součást použije operace.</span><span class="sxs-lookup"><span data-stu-id="d09a4-110">Notifications are fired as part of applying operations.</span></span> <span data-ttu-id="d09a4-111">Z tohoto důvodu měla by ji zpracovat oznámení tak rychle, jak je možné a synchronní události nesmí obsahovat žádné náročná operace.</span><span class="sxs-lookup"><span data-stu-id="d09a4-111">Because of that, notifications should be handled as fast as possible, and synchronous events shouldn't include any expensive operations.</span></span>

## <a name="reliable-state-manager-notifications"></a><span data-ttu-id="d09a4-112">Stav spolehlivé Správce oznámení</span><span class="sxs-lookup"><span data-stu-id="d09a4-112">Reliable State Manager notifications</span></span>
<span data-ttu-id="d09a4-113">Správce spolehlivé stavu poskytuje oznámení pro následující události:</span><span class="sxs-lookup"><span data-stu-id="d09a4-113">Reliable State Manager provides notifications for the following events:</span></span>

* <span data-ttu-id="d09a4-114">Transakce</span><span class="sxs-lookup"><span data-stu-id="d09a4-114">Transaction</span></span>
  * <span data-ttu-id="d09a4-115">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="d09a4-115">Commit</span></span>
* <span data-ttu-id="d09a4-116">Správce stavu</span><span class="sxs-lookup"><span data-stu-id="d09a4-116">State manager</span></span>
  * <span data-ttu-id="d09a4-117">Opětovné sestavení</span><span class="sxs-lookup"><span data-stu-id="d09a4-117">Rebuild</span></span>
  * <span data-ttu-id="d09a4-118">Přidání spolehlivé stavu</span><span class="sxs-lookup"><span data-stu-id="d09a4-118">Addition of a reliable state</span></span>
  * <span data-ttu-id="d09a4-119">Odebrání spolehlivé stavu</span><span class="sxs-lookup"><span data-stu-id="d09a4-119">Removal of a reliable state</span></span>

<span data-ttu-id="d09a4-120">Správce spolehlivé stavu sleduje aktuální aktivních pořadových transakce.</span><span class="sxs-lookup"><span data-stu-id="d09a4-120">Reliable State Manager tracks the current inflight transactions.</span></span> <span data-ttu-id="d09a4-121">Pouze změny ve stavu transakce, která způsobuje být aktivováno oznámení je právě potvrzené transakce.</span><span class="sxs-lookup"><span data-stu-id="d09a4-121">The only change in transaction state that causes a notification to be fired is a transaction being committed.</span></span>

<span data-ttu-id="d09a4-122">Správce spolehlivé stavu udržuje kolekci stavy spolehlivá jako slovník spolehlivé a spolehlivé fronty.</span><span class="sxs-lookup"><span data-stu-id="d09a4-122">Reliable State Manager maintains a collection of reliable states like Reliable Dictionary and Reliable Queue.</span></span> <span data-ttu-id="d09a4-123">Správce spolehlivé stavu aktivuje oznámení při změně této kolekce: spolehlivé stavu je přidat nebo odebrat nebo celou kolekci znovu sestaven.</span><span class="sxs-lookup"><span data-stu-id="d09a4-123">Reliable State Manager fires notifications when this collection changes: a reliable state is added or removed, or the entire collection is rebuilt.</span></span>
<span data-ttu-id="d09a4-124">Správce spolehlivé stavu kolekce je znovu sestavit ve třech případech:</span><span class="sxs-lookup"><span data-stu-id="d09a4-124">The Reliable State Manager collection is rebuilt in three cases:</span></span>

* <span data-ttu-id="d09a4-125">Obnovení: Když se spustí repliku, obnoví předchozí stav z disku.</span><span class="sxs-lookup"><span data-stu-id="d09a4-125">Recovery: When a replica starts, it recovers its previous state from the disk.</span></span> <span data-ttu-id="d09a4-126">Na konci obnovení používá **NotifyStateManagerChangedEventArgs** má provést událost, která obsahuje sadu obnovené spolehlivé stavy.</span><span class="sxs-lookup"><span data-stu-id="d09a4-126">At the end of recovery, it uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of recovered reliable states.</span></span>
* <span data-ttu-id="d09a4-127">Úplné kopie: před repliku se můžete zapojit do konfigurační sady, má má být sestaven.</span><span class="sxs-lookup"><span data-stu-id="d09a4-127">Full copy: Before a replica can join the configuration set, it has to be built.</span></span> <span data-ttu-id="d09a4-128">V některých případech to vyžaduje úplnou kopii spolehlivé správce stavu stavu z primární repliky má být použita pro nečinnosti sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="d09a4-128">Sometimes, this requires a full copy of Reliable State Manager's state from the primary replica to be applied to the idle secondary replica.</span></span> <span data-ttu-id="d09a4-129">Správce spolehlivé stavu v sekundární replice používá **NotifyStateManagerChangedEventArgs** má provést událost, která obsahuje sadu spolehlivé stavy, které jej získali z primární repliky.</span><span class="sxs-lookup"><span data-stu-id="d09a4-129">Reliable State Manager on the secondary replica uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of reliable states that it acquired from the primary replica.</span></span>
* <span data-ttu-id="d09a4-130">Obnovení: Ve scénářích zotavení po havárii, stav repliky může být obnovena ze zálohy, prostřednictvím **RestoreAsync**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-130">Restore: In disaster recovery scenarios, the replica's state can be restored from a backup via **RestoreAsync**.</span></span> <span data-ttu-id="d09a4-131">V takových případech spolehlivé správce stavu na primární replice používá **NotifyStateManagerChangedEventArgs** má provést událost, která obsahuje sadu spolehlivé stavy, které ji obnovit ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="d09a4-131">In such cases, Reliable State Manager on the primary replica uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of reliable states that it restored from the backup.</span></span>

<span data-ttu-id="d09a4-132">Registrace pro oznámení transakce nebo oznámení správce stavu, budete muset zaregistrovat **TransactionChanged** nebo **StateManagerChanged** událostí na spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="d09a4-132">To register for transaction notifications and/or state manager notifications, you need to register with the **TransactionChanged** or **StateManagerChanged** events on Reliable State Manager.</span></span> <span data-ttu-id="d09a4-133">Je běžné registraci obslužné rutiny těchto událostí se v konstruktoru stavové služby.</span><span class="sxs-lookup"><span data-stu-id="d09a4-133">A common place to register with these event handlers is the constructor of your stateful service.</span></span> <span data-ttu-id="d09a4-134">Při registraci v konstruktoru, nebude neproběhly všechna oznámení, která je způsobena změnou po dobu životnosti **IReliableStateManager**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-134">When you register on the constructor, you won't miss any notification that's caused by a change during the lifetime of **IReliableStateManager**.</span></span>

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

<span data-ttu-id="d09a4-135">**TransactionChanged** používá obslužné rutiny události **NotifyTransactionChangedEventArgs** k zajištění podrobných informací o události.</span><span class="sxs-lookup"><span data-stu-id="d09a4-135">The **TransactionChanged** event handler uses **NotifyTransactionChangedEventArgs** to provide details about the event.</span></span> <span data-ttu-id="d09a4-136">Obsahuje vlastnost action (například **NotifyTransactionChangedAction.Commit**) určující typ změny.</span><span class="sxs-lookup"><span data-stu-id="d09a4-136">It contains the action property (for example, **NotifyTransactionChangedAction.Commit**) that specifies the type of change.</span></span> <span data-ttu-id="d09a4-137">Také obsahuje vlastnost transakce, který poskytuje odkaz na transakce, který změnil.</span><span class="sxs-lookup"><span data-stu-id="d09a4-137">It also contains the transaction property that provides a reference to the transaction that changed.</span></span>

> [!NOTE]
> <span data-ttu-id="d09a4-138">V současné době **TransactionChanged** události jsou vyvolány pouze v případě, že je transakce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="d09a4-138">Today, **TransactionChanged** events are raised only if the transaction is committed.</span></span> <span data-ttu-id="d09a4-139">Akce bude rovna **NotifyTransactionChangedAction.Commit**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-139">The action is then equal to **NotifyTransactionChangedAction.Commit**.</span></span> <span data-ttu-id="d09a4-140">Ale v budoucnu, může být vyvolána události pro jiné typy změny stavu transakce.</span><span class="sxs-lookup"><span data-stu-id="d09a4-140">But in the future, events might be raised for other types of transaction state changes.</span></span> <span data-ttu-id="d09a4-141">Doporučujeme, abyste kontrolu akce a zpracování události, pouze pokud je ten, který jste očekávali.</span><span class="sxs-lookup"><span data-stu-id="d09a4-141">We recommend checking the action and processing the event only if it's one that you expect.</span></span>
> 
> 

<span data-ttu-id="d09a4-142">Tady je příklad **TransactionChanged** obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="d09a4-142">Following is an example **TransactionChanged** event handler.</span></span>

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

<span data-ttu-id="d09a4-143">**StateManagerChanged** používá obslužné rutiny události **NotifyStateManagerChangedEventArgs** k zajištění podrobných informací o události.</span><span class="sxs-lookup"><span data-stu-id="d09a4-143">The **StateManagerChanged** event handler uses **NotifyStateManagerChangedEventArgs** to provide details about the event.</span></span>
<span data-ttu-id="d09a4-144">**NotifyStateManagerChangedEventArgs** má dva podtřídy: **NotifyStateManagerRebuildEventArgs** a **NotifyStateManagerSingleEntityChangedEventArgs**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-144">**NotifyStateManagerChangedEventArgs** has two subclasses: **NotifyStateManagerRebuildEventArgs** and **NotifyStateManagerSingleEntityChangedEventArgs**.</span></span>
<span data-ttu-id="d09a4-145">Pomocí vlastnosti akce v **NotifyStateManagerChangedEventArgs** přetypovat **NotifyStateManagerChangedEventArgs** pro správné podtřídu:</span><span class="sxs-lookup"><span data-stu-id="d09a4-145">You use the action property in **NotifyStateManagerChangedEventArgs** to cast **NotifyStateManagerChangedEventArgs** to the correct subclass:</span></span>

* <span data-ttu-id="d09a4-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span></span>
* <span data-ttu-id="d09a4-147">**NotifyStateManagerChangedAction.Add** a **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-147">**NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span></span>

<span data-ttu-id="d09a4-148">Tady je příklad **StateManagerChanged** obslužná rutina oznámení.</span><span class="sxs-lookup"><span data-stu-id="d09a4-148">Following is an example **StateManagerChanged** notification handler.</span></span>

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a><span data-ttu-id="d09a4-149">Spolehlivé slovník oznámení</span><span class="sxs-lookup"><span data-stu-id="d09a4-149">Reliable Dictionary notifications</span></span>
<span data-ttu-id="d09a4-150">Spolehlivé slovník poskytuje oznámení pro následující události:</span><span class="sxs-lookup"><span data-stu-id="d09a4-150">Reliable Dictionary provides notifications for the following events:</span></span>

* <span data-ttu-id="d09a4-151">Opětovné sestavení: Voláno, když **ReliableDictionary** její stav se zotavil z obnovené nebo zkopírovaný místní stavu nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="d09a4-151">Rebuild: Called when **ReliableDictionary** has recovered its state from a recovered or copied local state or backup.</span></span>
* <span data-ttu-id="d09a4-152">Clear: Voláno, když stav **ReliableDictionary** byl vymazán prostřednictvím **ClearAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="d09a4-152">Clear: Called when the state of **ReliableDictionary** has been cleared through the **ClearAsync** method.</span></span>
* <span data-ttu-id="d09a4-153">Přidejte: Volána, když byla přidat položku do **ReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-153">Add: Called when an item has been added to **ReliableDictionary**.</span></span>
* <span data-ttu-id="d09a4-154">Aktualizace: Volána, když se položky v **IReliableDictionary** byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="d09a4-154">Update: Called when an item in **IReliableDictionary** has been updated.</span></span>
* <span data-ttu-id="d09a4-155">Odebrat: Volána, když se položky v **IReliableDictionary** byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="d09a4-155">Remove: Called when an item in **IReliableDictionary** has been deleted.</span></span>

<span data-ttu-id="d09a4-156">Pokud chcete dostávat oznámení spolehlivé slovník, je potřeba zaregistrovat **DictionaryChanged** obslužné rutiny události na **IReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-156">To get Reliable Dictionary notifications, you need to register with the **DictionaryChanged** event handler on **IReliableDictionary**.</span></span> <span data-ttu-id="d09a4-157">Je běžné registraci obslužné rutiny těchto událostí se v **ReliableStateManager.StateManagerChanged** přidat oznámení.</span><span class="sxs-lookup"><span data-stu-id="d09a4-157">A common place to register with these event handlers is in the **ReliableStateManager.StateManagerChanged** add notification.</span></span>
<span data-ttu-id="d09a4-158">Při registraci **IReliableDictionary** se přidá do **IReliableStateManager** zajistí, že nebude neproběhly žádné oznámení.</span><span class="sxs-lookup"><span data-stu-id="d09a4-158">Registering when **IReliableDictionary** is added to **IReliableStateManager** ensures that you won't miss any notifications.</span></span>

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="d09a4-159">**ProcessStateManagerSingleEntityNotification** je metoda ukázky, předchozím **OnStateManagerChangedHandler** příklad volání.</span><span class="sxs-lookup"><span data-stu-id="d09a4-159">**ProcessStateManagerSingleEntityNotification** is the sample method that the preceding **OnStateManagerChangedHandler** example calls.</span></span>
> 
> 

<span data-ttu-id="d09a4-160">Předchozí sady kódu **IReliableNotificationAsyncCallback** rozhraní, spolu s **DictionaryChanged**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-160">The preceding code sets the **IReliableNotificationAsyncCallback** interface, along with **DictionaryChanged**.</span></span> <span data-ttu-id="d09a4-161">Protože **NotifyDictionaryRebuildEventArgs** obsahuje **IAsyncEnumerable** rozhraní – které je možné provést výčet asynchronně – opětovné sestavení oznámení při vyvolání prostřednictvím  **RebuildNotificationAsyncCallback** místo **OnDictionaryChangedHandler**.</span><span class="sxs-lookup"><span data-stu-id="d09a4-161">Because **NotifyDictionaryRebuildEventArgs** contains an **IAsyncEnumerable** interface--which needs to be enumerated asynchronously--rebuild notifications are fired through **RebuildNotificationAsyncCallback** instead of **OnDictionaryChangedHandler**.</span></span>

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> <span data-ttu-id="d09a4-162">V předchozím kódu v rámci zpracování oznámení o opětovné sestavení nejprve zachována agregovaný stav je vymazán.</span><span class="sxs-lookup"><span data-stu-id="d09a4-162">In the preceding code, as part of processing the rebuild notification, first the maintained aggregated state is cleared.</span></span> <span data-ttu-id="d09a4-163">Protože spolehlivé kolekce se znovu sestaven pomocí nového stavu, jsou všechny předchozí oznámení důležité.</span><span class="sxs-lookup"><span data-stu-id="d09a4-163">Because the reliable collection is being rebuilt with a new state, all previous notifications are irrelevant.</span></span>
> 
> 

<span data-ttu-id="d09a4-164">**DictionaryChanged** používá obslužné rutiny události **NotifyDictionaryChangedEventArgs** k zajištění podrobných informací o události.</span><span class="sxs-lookup"><span data-stu-id="d09a4-164">The **DictionaryChanged** event handler uses **NotifyDictionaryChangedEventArgs** to provide details about the event.</span></span>
<span data-ttu-id="d09a4-165">**NotifyDictionaryChangedEventArgs** má pět podtřídy.</span><span class="sxs-lookup"><span data-stu-id="d09a4-165">**NotifyDictionaryChangedEventArgs** has five subclasses.</span></span> <span data-ttu-id="d09a4-166">Použijte vlastnost akce v **NotifyDictionaryChangedEventArgs** přetypovat **NotifyDictionaryChangedEventArgs** pro správné podtřídu:</span><span class="sxs-lookup"><span data-stu-id="d09a4-166">Use the action property in **NotifyDictionaryChangedEventArgs** to cast **NotifyDictionaryChangedEventArgs** to the correct subclass:</span></span>

* <span data-ttu-id="d09a4-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span></span>
* <span data-ttu-id="d09a4-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span></span>
* <span data-ttu-id="d09a4-169">**NotifyDictionaryChangedAction.Add** a **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-169">**NotifyDictionaryChangedAction.Add** and **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span></span>
* <span data-ttu-id="d09a4-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span></span>
* <span data-ttu-id="d09a4-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="d09a4-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span></span>

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a><span data-ttu-id="d09a4-172">Doporučení</span><span class="sxs-lookup"><span data-stu-id="d09a4-172">Recommendations</span></span>
* <span data-ttu-id="d09a4-173">*Proveďte* co nejrychleji provést oznámení události.</span><span class="sxs-lookup"><span data-stu-id="d09a4-173">*Do* complete notification events as fast as possible.</span></span>
* <span data-ttu-id="d09a4-174">*Nechcete* spouštět žádné náročná operace (například vstupně-výstupních operací) jako součást synchronní události.</span><span class="sxs-lookup"><span data-stu-id="d09a4-174">*Do not* execute any expensive operations (for example, I/O operations) as part of synchronous events.</span></span>
* <span data-ttu-id="d09a4-175">*Proveďte* zkontrolujte typ akce před zpracovat událost.</span><span class="sxs-lookup"><span data-stu-id="d09a4-175">*Do* check the action type before you process the event.</span></span> <span data-ttu-id="d09a4-176">Nové typy akce může v budoucnu přidat.</span><span class="sxs-lookup"><span data-stu-id="d09a4-176">New action types might be added in the future.</span></span>

<span data-ttu-id="d09a4-177">Zde jsou některé věci, třeba vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="d09a4-177">Here are some things to keep in mind:</span></span>

* <span data-ttu-id="d09a4-178">Oznámení při vyvolání jako součást provádění operace.</span><span class="sxs-lookup"><span data-stu-id="d09a4-178">Notifications are fired as part of the execution of an operation.</span></span> <span data-ttu-id="d09a4-179">Například oznámení o obnovení, je vyvolána jako poslední krok operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="d09a4-179">For example, a restore notification is fired as the last step of a restore operation.</span></span> <span data-ttu-id="d09a4-180">Obnovení nebude dokončen, dokud zpracovává událost upozornění.</span><span class="sxs-lookup"><span data-stu-id="d09a4-180">A restore will not finish until the notification event is processed.</span></span>
* <span data-ttu-id="d09a4-181">Protože oznámení při vyvolání jako součást provádění operací, klienti zobrazit pouze oznámení pro místně potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="d09a4-181">Because notifications are fired as part of the applying operations, clients see only notifications for locally committed operations.</span></span> <span data-ttu-id="d09a4-182">A protože je zaručeno, že operace pouze místně potvrzené (jinými slovy, přihlášení), se může nebo nemusí být v budoucnu vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="d09a4-182">And because operations are guaranteed only to be locally committed (in other words, logged), they might or might not be undone in the future.</span></span>
* <span data-ttu-id="d09a4-183">V cestě opakování je aktivována, jednoho oznámení pro každou operaci použité.</span><span class="sxs-lookup"><span data-stu-id="d09a4-183">On the redo path, a single notification is fired for each applied operation.</span></span> <span data-ttu-id="d09a4-184">To znamená, že obsahuje-li transakce T1 Create(X), Delete(X) a Create(X), získáte jedno oznámení pro vytvoření X, jeden pro odstranění a jeden pro vytvoření znovu, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="d09a4-184">This means that if transaction T1 includes Create(X), Delete(X), and Create(X), you'll get one notification for the creation of X, one for the deletion, and one for the creation again, in that order.</span></span>
* <span data-ttu-id="d09a4-185">Pro transakce, které obsahují více operací operace se použijí v pořadí, ve kterém byly načteny na primární replice od uživatele.</span><span class="sxs-lookup"><span data-stu-id="d09a4-185">For transactions that contain multiple operations, operations are applied in the order in which they were received on the primary replica from the user.</span></span>
* <span data-ttu-id="d09a4-186">Jako součást zpracování false průběh může být některé operace vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="d09a4-186">As part of processing false progress, some operations might be undone.</span></span> <span data-ttu-id="d09a4-187">Oznámení jsou vyvolány pro tyto operace vrácení zpět, vracení stavu repliky na bod stabilní.</span><span class="sxs-lookup"><span data-stu-id="d09a4-187">Notifications are raised for such undo operations, rolling the state of the replica back to a stable point.</span></span> <span data-ttu-id="d09a4-188">Jeden důležitý rozdíl oznámení vrácení zpět je, že jsou agregovaný události, které mají duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="d09a4-188">One important difference of undo notifications is that events that have duplicate keys are aggregated.</span></span> <span data-ttu-id="d09a4-189">Například pokud se vrací zpět transakci T1, se zobrazí jeden oznámení Delete(X).</span><span class="sxs-lookup"><span data-stu-id="d09a4-189">For example, if transaction T1 is being undone, you'll see a single notification to Delete(X).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d09a4-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d09a4-190">Next steps</span></span>
* [<span data-ttu-id="d09a4-191">Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="d09a4-191">Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="d09a4-192">Spolehlivé služby rychlý start</span><span class="sxs-lookup"><span data-stu-id="d09a4-192">Reliable Services quick start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="d09a4-193">Spolehlivé služby zálohování a obnovení (zotavení po havárii)</span><span class="sxs-lookup"><span data-stu-id="d09a4-193">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="d09a4-194">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="d09a4-194">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

