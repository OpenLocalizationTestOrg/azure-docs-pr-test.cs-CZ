---
title: "oznámení služby aaaReliable | Microsoft Docs"
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
ms.openlocfilehash: 8c43190d31dbe82d1dc7fa1c228128bdcc3684f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-notifications"></a><span data-ttu-id="241e8-103">Spolehlivé služby oznámení</span><span class="sxs-lookup"><span data-stu-id="241e8-103">Reliable Services notifications</span></span>
<span data-ttu-id="241e8-104">Oznámení klientům tootrack hello změny, které jsou určeny jako tooan objekt, který se vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="241e8-104">Notifications allow clients tootrack hello changes that are being made tooan object that they're interested in.</span></span> <span data-ttu-id="241e8-105">Oznámení podporují dva typy objektů: *spolehlivé správce stavu* a *spolehlivé slovník*.</span><span class="sxs-lookup"><span data-stu-id="241e8-105">Two types of objects support notifications: *Reliable State Manager* and *Reliable Dictionary*.</span></span>

<span data-ttu-id="241e8-106">Běžnými důvody používání oznámení jsou:</span><span class="sxs-lookup"><span data-stu-id="241e8-106">Common reasons for using notifications are:</span></span>

* <span data-ttu-id="241e8-107">Vytváření materializována zobrazení, jako je například sekundární indexy nebo agregovat filtrovaných zobrazení stavu repliky hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-107">Building materialized views, such as secondary indexes or aggregated filtered views of hello replica's state.</span></span> <span data-ttu-id="241e8-108">Příkladem je seřazená index všechny klíče ve slovníku spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="241e8-108">An example is a sorted index of all keys in Reliable Dictionary.</span></span>
* <span data-ttu-id="241e8-109">Odesílání monitorování data, jako je například hello počet uživatelů přidaných v hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="241e8-109">Sending monitoring data, such as hello number of users added in hello last hour.</span></span>

<span data-ttu-id="241e8-110">Oznámení při vyvolání jako součást použije operace.</span><span class="sxs-lookup"><span data-stu-id="241e8-110">Notifications are fired as part of applying operations.</span></span> <span data-ttu-id="241e8-111">Z tohoto důvodu měla by ji zpracovat oznámení tak rychle, jak je možné a synchronní události nesmí obsahovat žádné náročná operace.</span><span class="sxs-lookup"><span data-stu-id="241e8-111">Because of that, notifications should be handled as fast as possible, and synchronous events shouldn't include any expensive operations.</span></span>

## <a name="reliable-state-manager-notifications"></a><span data-ttu-id="241e8-112">Stav spolehlivé Správce oznámení</span><span class="sxs-lookup"><span data-stu-id="241e8-112">Reliable State Manager notifications</span></span>
<span data-ttu-id="241e8-113">Správce spolehlivé stavu poskytuje oznámení pro hello následující události:</span><span class="sxs-lookup"><span data-stu-id="241e8-113">Reliable State Manager provides notifications for hello following events:</span></span>

* <span data-ttu-id="241e8-114">Transakce</span><span class="sxs-lookup"><span data-stu-id="241e8-114">Transaction</span></span>
  * <span data-ttu-id="241e8-115">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="241e8-115">Commit</span></span>
* <span data-ttu-id="241e8-116">Správce stavu</span><span class="sxs-lookup"><span data-stu-id="241e8-116">State manager</span></span>
  * <span data-ttu-id="241e8-117">Opětovné sestavení</span><span class="sxs-lookup"><span data-stu-id="241e8-117">Rebuild</span></span>
  * <span data-ttu-id="241e8-118">Přidání spolehlivé stavu</span><span class="sxs-lookup"><span data-stu-id="241e8-118">Addition of a reliable state</span></span>
  * <span data-ttu-id="241e8-119">Odebrání spolehlivé stavu</span><span class="sxs-lookup"><span data-stu-id="241e8-119">Removal of a reliable state</span></span>

<span data-ttu-id="241e8-120">Správce spolehlivé stavu sleduje aktuální transakce aktivních pořadových hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-120">Reliable State Manager tracks hello current inflight transactions.</span></span> <span data-ttu-id="241e8-121">Hello pouze změny ve stavu transakce, který způsobuje, že oznámení toobe aktivováno je právě potvrzené transakce.</span><span class="sxs-lookup"><span data-stu-id="241e8-121">hello only change in transaction state that causes a notification toobe fired is a transaction being committed.</span></span>

<span data-ttu-id="241e8-122">Správce spolehlivé stavu udržuje kolekci stavy spolehlivá jako slovník spolehlivé a spolehlivé fronty.</span><span class="sxs-lookup"><span data-stu-id="241e8-122">Reliable State Manager maintains a collection of reliable states like Reliable Dictionary and Reliable Queue.</span></span> <span data-ttu-id="241e8-123">Správce spolehlivé stavu aktivuje oznámení při změně této kolekce: spolehlivé stavu je přidat nebo odebrat nebo celou kolekci hello je znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="241e8-123">Reliable State Manager fires notifications when this collection changes: a reliable state is added or removed, or hello entire collection is rebuilt.</span></span>
<span data-ttu-id="241e8-124">Hello spolehlivé správce stavu kolekce je znovu sestavit ve třech případech:</span><span class="sxs-lookup"><span data-stu-id="241e8-124">hello Reliable State Manager collection is rebuilt in three cases:</span></span>

* <span data-ttu-id="241e8-125">Obnovení: Když se spustí repliku, obnoví předchozí stav z disku hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-125">Recovery: When a replica starts, it recovers its previous state from hello disk.</span></span> <span data-ttu-id="241e8-126">Na konci hello obnovení, používá **NotifyStateManagerChangedEventArgs** toofire událost, která obsahuje sadu hello obnovené spolehlivé stavy.</span><span class="sxs-lookup"><span data-stu-id="241e8-126">At hello end of recovery, it uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of recovered reliable states.</span></span>
* <span data-ttu-id="241e8-127">Úplné kopie: před repliku můžete připojit hello konfigurační sady, má toobe sestaven.</span><span class="sxs-lookup"><span data-stu-id="241e8-127">Full copy: Before a replica can join hello configuration set, it has toobe built.</span></span> <span data-ttu-id="241e8-128">V některých případech to vyžaduje úplnou kopii spolehlivé správce stavu stavu z hello primární repliky toobe toohello použité v nečinnosti sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="241e8-128">Sometimes, this requires a full copy of Reliable State Manager's state from hello primary replica toobe applied toohello idle secondary replica.</span></span> <span data-ttu-id="241e8-129">Správce spolehlivé stavu na sekundární repliku, která používá hello **NotifyStateManagerChangedEventArgs** toofire událost, která obsahuje sadu hello spolehlivé stavy, které jej získali z primární repliky hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-129">Reliable State Manager on hello secondary replica uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of reliable states that it acquired from hello primary replica.</span></span>
* <span data-ttu-id="241e8-130">Obnovení: Ve scénářích zotavení po havárii, stav hello repliky může být obnovena ze zálohy, prostřednictvím **RestoreAsync**.</span><span class="sxs-lookup"><span data-stu-id="241e8-130">Restore: In disaster recovery scenarios, hello replica's state can be restored from a backup via **RestoreAsync**.</span></span> <span data-ttu-id="241e8-131">V takových případech spolehlivé správce stavu na primární replice hello používá **NotifyStateManagerChangedEventArgs** toofire událost, která obsahuje sadu hello spolehlivé stavy, které ji obnovit ze zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-131">In such cases, Reliable State Manager on hello primary replica uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of reliable states that it restored from hello backup.</span></span>

<span data-ttu-id="241e8-132">tooregister pro transakce oznámení a oznámení správce stavu, je nutné tooregister s hello **TransactionChanged** nebo **StateManagerChanged** událostí na spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="241e8-132">tooregister for transaction notifications and/or state manager notifications, you need tooregister with hello **TransactionChanged** or **StateManagerChanged** events on Reliable State Manager.</span></span> <span data-ttu-id="241e8-133">Běžné místní tooregister s tyto obslužné rutiny událostí je hello konstruktoru stavové služby.</span><span class="sxs-lookup"><span data-stu-id="241e8-133">A common place tooregister with these event handlers is hello constructor of your stateful service.</span></span> <span data-ttu-id="241e8-134">Při registraci na hello konstruktor nebude neproběhly všechna oznámení, která je způsobena změnou během hello životnost **IReliableStateManager**.</span><span class="sxs-lookup"><span data-stu-id="241e8-134">When you register on hello constructor, you won't miss any notification that's caused by a change during hello lifetime of **IReliableStateManager**.</span></span>

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

<span data-ttu-id="241e8-135">Hello **TransactionChanged** používá obslužné rutiny události **NotifyTransactionChangedEventArgs** tooprovide podrobnosti o události hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-135">hello **TransactionChanged** event handler uses **NotifyTransactionChangedEventArgs** tooprovide details about hello event.</span></span> <span data-ttu-id="241e8-136">Obsahuje vlastnost action hello (například **NotifyTransactionChangedAction.Commit**) určující typ hello změny.</span><span class="sxs-lookup"><span data-stu-id="241e8-136">It contains hello action property (for example, **NotifyTransactionChangedAction.Commit**) that specifies hello type of change.</span></span> <span data-ttu-id="241e8-137">Také obsahuje vlastnost transaction hello, která poskytuje transakce toohello odkaz, který změnil.</span><span class="sxs-lookup"><span data-stu-id="241e8-137">It also contains hello transaction property that provides a reference toohello transaction that changed.</span></span>

> [!NOTE]
> <span data-ttu-id="241e8-138">V současné době **TransactionChanged** události jsou vyvolány pouze v případě, že je hello transakce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="241e8-138">Today, **TransactionChanged** events are raised only if hello transaction is committed.</span></span> <span data-ttu-id="241e8-139">Hello akce je pak rovna příliš**NotifyTransactionChangedAction.Commit**.</span><span class="sxs-lookup"><span data-stu-id="241e8-139">hello action is then equal too**NotifyTransactionChangedAction.Commit**.</span></span> <span data-ttu-id="241e8-140">Ale v budoucnu hello, může být vyvolána události pro jiné typy změny stavu transakce.</span><span class="sxs-lookup"><span data-stu-id="241e8-140">But in hello future, events might be raised for other types of transaction state changes.</span></span> <span data-ttu-id="241e8-141">Doporučujeme, abyste kontrolu hello akce a zpracování události hello, pouze pokud je ten, který jste očekávali.</span><span class="sxs-lookup"><span data-stu-id="241e8-141">We recommend checking hello action and processing hello event only if it's one that you expect.</span></span>
> 
> 

<span data-ttu-id="241e8-142">Tady je příklad **TransactionChanged** obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="241e8-142">Following is an example **TransactionChanged** event handler.</span></span>

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

<span data-ttu-id="241e8-143">Hello **StateManagerChanged** používá obslužné rutiny události **NotifyStateManagerChangedEventArgs** tooprovide podrobnosti o události hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-143">hello **StateManagerChanged** event handler uses **NotifyStateManagerChangedEventArgs** tooprovide details about hello event.</span></span>
<span data-ttu-id="241e8-144">**NotifyStateManagerChangedEventArgs** má dva podtřídy: **NotifyStateManagerRebuildEventArgs** a **NotifyStateManagerSingleEntityChangedEventArgs**.</span><span class="sxs-lookup"><span data-stu-id="241e8-144">**NotifyStateManagerChangedEventArgs** has two subclasses: **NotifyStateManagerRebuildEventArgs** and **NotifyStateManagerSingleEntityChangedEventArgs**.</span></span>
<span data-ttu-id="241e8-145">Pomocí vlastnosti akce hello v **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello správné podtřídami:</span><span class="sxs-lookup"><span data-stu-id="241e8-145">You use hello action property in **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello correct subclass:</span></span>

* <span data-ttu-id="241e8-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span></span>
* <span data-ttu-id="241e8-147">**NotifyStateManagerChangedAction.Add** a **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-147">**NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span></span>

<span data-ttu-id="241e8-148">Tady je příklad **StateManagerChanged** obslužná rutina oznámení.</span><span class="sxs-lookup"><span data-stu-id="241e8-148">Following is an example **StateManagerChanged** notification handler.</span></span>

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

## <a name="reliable-dictionary-notifications"></a><span data-ttu-id="241e8-149">Spolehlivé slovník oznámení</span><span class="sxs-lookup"><span data-stu-id="241e8-149">Reliable Dictionary notifications</span></span>
<span data-ttu-id="241e8-150">Spolehlivé slovník obsahuje oznámení o hello následující události:</span><span class="sxs-lookup"><span data-stu-id="241e8-150">Reliable Dictionary provides notifications for hello following events:</span></span>

* <span data-ttu-id="241e8-151">Opětovné sestavení: Voláno, když **ReliableDictionary** její stav se zotavil z obnovené nebo zkopírovaný místní stavu nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="241e8-151">Rebuild: Called when **ReliableDictionary** has recovered its state from a recovered or copied local state or backup.</span></span>
* <span data-ttu-id="241e8-152">Clear: Voláno, když hello stav **ReliableDictionary** byl vymazán prostřednictvím hello **ClearAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="241e8-152">Clear: Called when hello state of **ReliableDictionary** has been cleared through hello **ClearAsync** method.</span></span>
* <span data-ttu-id="241e8-153">Přidejte: Volána, když se příliš přidat položku**ReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="241e8-153">Add: Called when an item has been added too**ReliableDictionary**.</span></span>
* <span data-ttu-id="241e8-154">Aktualizace: Volána, když se položky v **IReliableDictionary** byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="241e8-154">Update: Called when an item in **IReliableDictionary** has been updated.</span></span>
* <span data-ttu-id="241e8-155">Odebrat: Volána, když se položky v **IReliableDictionary** byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="241e8-155">Remove: Called when an item in **IReliableDictionary** has been deleted.</span></span>

<span data-ttu-id="241e8-156">tooget spolehlivé slovník oznámení, musíte tooregister s hello **DictionaryChanged** obslužné rutiny události na **IReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="241e8-156">tooget Reliable Dictionary notifications, you need tooregister with hello **DictionaryChanged** event handler on **IReliableDictionary**.</span></span> <span data-ttu-id="241e8-157">Běžné místní tooregister s tyto obslužné rutiny událostí je v hello **ReliableStateManager.StateManagerChanged** přidat oznámení.</span><span class="sxs-lookup"><span data-stu-id="241e8-157">A common place tooregister with these event handlers is in hello **ReliableStateManager.StateManagerChanged** add notification.</span></span>
<span data-ttu-id="241e8-158">Při registraci **IReliableDictionary** je přidána příliš**IReliableStateManager** zajistí, že nebude neproběhly žádné oznámení.</span><span class="sxs-lookup"><span data-stu-id="241e8-158">Registering when **IReliableDictionary** is added too**IReliableStateManager** ensures that you won't miss any notifications.</span></span>

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
> <span data-ttu-id="241e8-159">**ProcessStateManagerSingleEntityNotification** je metoda ukázky hello této hello předchozí **OnStateManagerChangedHandler** příklad volání.</span><span class="sxs-lookup"><span data-stu-id="241e8-159">**ProcessStateManagerSingleEntityNotification** is hello sample method that hello preceding **OnStateManagerChangedHandler** example calls.</span></span>
> 
> 

<span data-ttu-id="241e8-160">Hello předchozí kód nastaví hello **IReliableNotificationAsyncCallback** rozhraní, spolu s **DictionaryChanged**.</span><span class="sxs-lookup"><span data-stu-id="241e8-160">hello preceding code sets hello **IReliableNotificationAsyncCallback** interface, along with **DictionaryChanged**.</span></span> <span data-ttu-id="241e8-161">Protože **NotifyDictionaryRebuildEventArgs** obsahuje **IAsyncEnumerable** rozhraní – které je výčet asynchronně – toobe opětovné sestavení oznámení při vyvolání prostřednictvím  **RebuildNotificationAsyncCallback** místo **OnDictionaryChangedHandler**.</span><span class="sxs-lookup"><span data-stu-id="241e8-161">Because **NotifyDictionaryRebuildEventArgs** contains an **IAsyncEnumerable** interface--which needs toobe enumerated asynchronously--rebuild notifications are fired through **RebuildNotificationAsyncCallback** instead of **OnDictionaryChangedHandler**.</span></span>

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
> <span data-ttu-id="241e8-162">V předchozích kódu, jako součást zpracování hello opětovné sestavení oznámení, hello první hello udržovat, že agregovaný stav je vymazán.</span><span class="sxs-lookup"><span data-stu-id="241e8-162">In hello preceding code, as part of processing hello rebuild notification, first hello maintained aggregated state is cleared.</span></span> <span data-ttu-id="241e8-163">Protože kolekce spolehlivé hello se znovu sestaven pomocí nového stavu, jsou všechny předchozí oznámení důležité.</span><span class="sxs-lookup"><span data-stu-id="241e8-163">Because hello reliable collection is being rebuilt with a new state, all previous notifications are irrelevant.</span></span>
> 
> 

<span data-ttu-id="241e8-164">Hello **DictionaryChanged** používá obslužné rutiny události **NotifyDictionaryChangedEventArgs** tooprovide podrobnosti o události hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-164">hello **DictionaryChanged** event handler uses **NotifyDictionaryChangedEventArgs** tooprovide details about hello event.</span></span>
<span data-ttu-id="241e8-165">**NotifyDictionaryChangedEventArgs** má pět podtřídy.</span><span class="sxs-lookup"><span data-stu-id="241e8-165">**NotifyDictionaryChangedEventArgs** has five subclasses.</span></span> <span data-ttu-id="241e8-166">Použijte vlastnost hello akce v **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello správné podtřídami:</span><span class="sxs-lookup"><span data-stu-id="241e8-166">Use hello action property in **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello correct subclass:</span></span>

* <span data-ttu-id="241e8-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span></span>
* <span data-ttu-id="241e8-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span></span>
* <span data-ttu-id="241e8-169">**NotifyDictionaryChangedAction.Add** a **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-169">**NotifyDictionaryChangedAction.Add** and **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span></span>
* <span data-ttu-id="241e8-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span></span>
* <span data-ttu-id="241e8-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="241e8-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span></span>

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

## <a name="recommendations"></a><span data-ttu-id="241e8-172">Doporučení</span><span class="sxs-lookup"><span data-stu-id="241e8-172">Recommendations</span></span>
* <span data-ttu-id="241e8-173">*Proveďte* co nejrychleji provést oznámení události.</span><span class="sxs-lookup"><span data-stu-id="241e8-173">*Do* complete notification events as fast as possible.</span></span>
* <span data-ttu-id="241e8-174">*Nechcete* spouštět žádné náročná operace (například vstupně-výstupních operací) jako součást synchronní události.</span><span class="sxs-lookup"><span data-stu-id="241e8-174">*Do not* execute any expensive operations (for example, I/O operations) as part of synchronous events.</span></span>
* <span data-ttu-id="241e8-175">*Proveďte* zkontrolujte typ akce hello před zpracovat hello událost.</span><span class="sxs-lookup"><span data-stu-id="241e8-175">*Do* check hello action type before you process hello event.</span></span> <span data-ttu-id="241e8-176">Nové typy akcí pravděpodobně přidána v budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-176">New action types might be added in hello future.</span></span>

<span data-ttu-id="241e8-177">Zde jsou některé věci tookeep nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="241e8-177">Here are some things tookeep in mind:</span></span>

* <span data-ttu-id="241e8-178">Oznámení při vyvolání jako součást hello provedení operace.</span><span class="sxs-lookup"><span data-stu-id="241e8-178">Notifications are fired as part of hello execution of an operation.</span></span> <span data-ttu-id="241e8-179">Například oznámení o obnovení, je vyvolána jako poslední krok hello operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="241e8-179">For example, a restore notification is fired as hello last step of a restore operation.</span></span> <span data-ttu-id="241e8-180">Obnovení nebude dokončen, dokud zpracovává událost upozornění hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-180">A restore will not finish until hello notification event is processed.</span></span>
* <span data-ttu-id="241e8-181">Protože oznámení při vyvolání jako součást hello použije operace, klienti zobrazit pouze oznámení pro místně potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="241e8-181">Because notifications are fired as part of hello applying operations, clients see only notifications for locally committed operations.</span></span> <span data-ttu-id="241e8-182">A protože operace jsou zaručit pouze toobe místně potvrzené (jinými slovy, přihlášení), se může nebo nemusí být v budoucnu hello odvolat.</span><span class="sxs-lookup"><span data-stu-id="241e8-182">And because operations are guaranteed only toobe locally committed (in other words, logged), they might or might not be undone in hello future.</span></span>
* <span data-ttu-id="241e8-183">V cestě opakování hello je pro každou operaci použité aktivována jednoho oznámení.</span><span class="sxs-lookup"><span data-stu-id="241e8-183">On hello redo path, a single notification is fired for each applied operation.</span></span> <span data-ttu-id="241e8-184">To znamená, že pokud transakce T1 obsahuje Create(X), Delete(X) a Create(X), získáte jedno oznámení pro vytvoření hello X, jeden pro odstranění hello a jeden pro vytvoření hello znovu, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="241e8-184">This means that if transaction T1 includes Create(X), Delete(X), and Create(X), you'll get one notification for hello creation of X, one for hello deletion, and one for hello creation again, in that order.</span></span>
* <span data-ttu-id="241e8-185">Pro transakce, které obsahují více operací operace se použijí v hello pořadí, ve kterém bylo přijato na primární replice hello od uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="241e8-185">For transactions that contain multiple operations, operations are applied in hello order in which they were received on hello primary replica from hello user.</span></span>
* <span data-ttu-id="241e8-186">Jako součást zpracování false průběh může být některé operace vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="241e8-186">As part of processing false progress, some operations might be undone.</span></span> <span data-ttu-id="241e8-187">Oznámení jsou vyvolány pro tyto operace vrácení zpět, vracení stavu hello hello repliky back tooa stabilní bodu.</span><span class="sxs-lookup"><span data-stu-id="241e8-187">Notifications are raised for such undo operations, rolling hello state of hello replica back tooa stable point.</span></span> <span data-ttu-id="241e8-188">Jeden důležitý rozdíl oznámení vrácení zpět je, že jsou agregovaný události, které mají duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="241e8-188">One important difference of undo notifications is that events that have duplicate keys are aggregated.</span></span> <span data-ttu-id="241e8-189">Například pokud se vrací zpět transakci T1, uvidíte tooDelete(X) jednoho oznámení.</span><span class="sxs-lookup"><span data-stu-id="241e8-189">For example, if transaction T1 is being undone, you'll see a single notification tooDelete(X).</span></span>

## <a name="next-steps"></a><span data-ttu-id="241e8-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="241e8-190">Next steps</span></span>
* [<span data-ttu-id="241e8-191">Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="241e8-191">Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="241e8-192">Spolehlivé služby rychlý start</span><span class="sxs-lookup"><span data-stu-id="241e8-192">Reliable Services quick start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="241e8-193">Spolehlivé služby zálohování a obnovení (zotavení po havárii)</span><span class="sxs-lookup"><span data-stu-id="241e8-193">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="241e8-194">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="241e8-194">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

