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
# <a name="reliable-services-notifications"></a>Spolehlivé služby oznámení
Oznámení klientům tootrack hello změny, které jsou určeny jako tooan objekt, který se vás zajímá. Oznámení podporují dva typy objektů: *spolehlivé správce stavu* a *spolehlivé slovník*.

Běžnými důvody používání oznámení jsou:

* Vytváření materializována zobrazení, jako je například sekundární indexy nebo agregovat filtrovaných zobrazení stavu repliky hello. Příkladem je seřazená index všechny klíče ve slovníku spolehlivé.
* Odesílání monitorování data, jako je například hello počet uživatelů přidaných v hello poslední hodinu.

Oznámení při vyvolání jako součást použije operace. Z tohoto důvodu měla by ji zpracovat oznámení tak rychle, jak je možné a synchronní události nesmí obsahovat žádné náročná operace.

## <a name="reliable-state-manager-notifications"></a>Stav spolehlivé Správce oznámení
Správce spolehlivé stavu poskytuje oznámení pro hello následující události:

* Transakce
  * Potvrzení
* Správce stavu
  * Opětovné sestavení
  * Přidání spolehlivé stavu
  * Odebrání spolehlivé stavu

Správce spolehlivé stavu sleduje aktuální transakce aktivních pořadových hello. Hello pouze změny ve stavu transakce, který způsobuje, že oznámení toobe aktivováno je právě potvrzené transakce.

Správce spolehlivé stavu udržuje kolekci stavy spolehlivá jako slovník spolehlivé a spolehlivé fronty. Správce spolehlivé stavu aktivuje oznámení při změně této kolekce: spolehlivé stavu je přidat nebo odebrat nebo celou kolekci hello je znovu sestavit.
Hello spolehlivé správce stavu kolekce je znovu sestavit ve třech případech:

* Obnovení: Když se spustí repliku, obnoví předchozí stav z disku hello. Na konci hello obnovení, používá **NotifyStateManagerChangedEventArgs** toofire událost, která obsahuje sadu hello obnovené spolehlivé stavy.
* Úplné kopie: před repliku můžete připojit hello konfigurační sady, má toobe sestaven. V některých případech to vyžaduje úplnou kopii spolehlivé správce stavu stavu z hello primární repliky toobe toohello použité v nečinnosti sekundární repliky. Správce spolehlivé stavu na sekundární repliku, která používá hello **NotifyStateManagerChangedEventArgs** toofire událost, která obsahuje sadu hello spolehlivé stavy, které jej získali z primární repliky hello.
* Obnovení: Ve scénářích zotavení po havárii, stav hello repliky může být obnovena ze zálohy, prostřednictvím **RestoreAsync**. V takových případech spolehlivé správce stavu na primární replice hello používá **NotifyStateManagerChangedEventArgs** toofire událost, která obsahuje sadu hello spolehlivé stavy, které ji obnovit ze zálohy hello.

tooregister pro transakce oznámení a oznámení správce stavu, je nutné tooregister s hello **TransactionChanged** nebo **StateManagerChanged** událostí na spolehlivé správce stavu. Běžné místní tooregister s tyto obslužné rutiny událostí je hello konstruktoru stavové služby. Při registraci na hello konstruktor nebude neproběhly všechna oznámení, která je způsobena změnou během hello životnost **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Hello **TransactionChanged** používá obslužné rutiny události **NotifyTransactionChangedEventArgs** tooprovide podrobnosti o události hello. Obsahuje vlastnost action hello (například **NotifyTransactionChangedAction.Commit**) určující typ hello změny. Také obsahuje vlastnost transaction hello, která poskytuje transakce toohello odkaz, který změnil.

> [!NOTE]
> V současné době **TransactionChanged** události jsou vyvolány pouze v případě, že je hello transakce potvrzena. Hello akce je pak rovna příliš**NotifyTransactionChangedAction.Commit**. Ale v budoucnu hello, může být vyvolána události pro jiné typy změny stavu transakce. Doporučujeme, abyste kontrolu hello akce a zpracování události hello, pouze pokud je ten, který jste očekávali.
> 
> 

Tady je příklad **TransactionChanged** obslužné rutiny události.

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

Hello **StateManagerChanged** používá obslužné rutiny události **NotifyStateManagerChangedEventArgs** tooprovide podrobnosti o události hello.
**NotifyStateManagerChangedEventArgs** má dva podtřídy: **NotifyStateManagerRebuildEventArgs** a **NotifyStateManagerSingleEntityChangedEventArgs**.
Pomocí vlastnosti akce hello v **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello správné podtřídami:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** a **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Tady je příklad **StateManagerChanged** obslužná rutina oznámení.

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

## <a name="reliable-dictionary-notifications"></a>Spolehlivé slovník oznámení
Spolehlivé slovník obsahuje oznámení o hello následující události:

* Opětovné sestavení: Voláno, když **ReliableDictionary** její stav se zotavil z obnovené nebo zkopírovaný místní stavu nebo zálohování.
* Clear: Voláno, když hello stav **ReliableDictionary** byl vymazán prostřednictvím hello **ClearAsync** metoda.
* Přidejte: Volána, když se příliš přidat položku**ReliableDictionary**.
* Aktualizace: Volána, když se položky v **IReliableDictionary** byla aktualizována.
* Odebrat: Volána, když se položky v **IReliableDictionary** byla odstraněna.

tooget spolehlivé slovník oznámení, musíte tooregister s hello **DictionaryChanged** obslužné rutiny události na **IReliableDictionary**. Běžné místní tooregister s tyto obslužné rutiny událostí je v hello **ReliableStateManager.StateManagerChanged** přidat oznámení.
Při registraci **IReliableDictionary** je přidána příliš**IReliableStateManager** zajistí, že nebude neproběhly žádné oznámení.

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
> **ProcessStateManagerSingleEntityNotification** je metoda ukázky hello této hello předchozí **OnStateManagerChangedHandler** příklad volání.
> 
> 

Hello předchozí kód nastaví hello **IReliableNotificationAsyncCallback** rozhraní, spolu s **DictionaryChanged**. Protože **NotifyDictionaryRebuildEventArgs** obsahuje **IAsyncEnumerable** rozhraní – které je výčet asynchronně – toobe opětovné sestavení oznámení při vyvolání prostřednictvím  **RebuildNotificationAsyncCallback** místo **OnDictionaryChangedHandler**.

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
> V předchozích kódu, jako součást zpracování hello opětovné sestavení oznámení, hello první hello udržovat, že agregovaný stav je vymazán. Protože kolekce spolehlivé hello se znovu sestaven pomocí nového stavu, jsou všechny předchozí oznámení důležité.
> 
> 

Hello **DictionaryChanged** používá obslužné rutiny události **NotifyDictionaryChangedEventArgs** tooprovide podrobnosti o události hello.
**NotifyDictionaryChangedEventArgs** má pět podtřídy. Použijte vlastnost hello akce v **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello správné podtřídami:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add** a **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

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

## <a name="recommendations"></a>Doporučení
* *Proveďte* co nejrychleji provést oznámení události.
* *Nechcete* spouštět žádné náročná operace (například vstupně-výstupních operací) jako součást synchronní události.
* *Proveďte* zkontrolujte typ akce hello před zpracovat hello událost. Nové typy akcí pravděpodobně přidána v budoucí hello.

Zde jsou některé věci tookeep nezapomeňte:

* Oznámení při vyvolání jako součást hello provedení operace. Například oznámení o obnovení, je vyvolána jako poslední krok hello operaci obnovení. Obnovení nebude dokončen, dokud zpracovává událost upozornění hello.
* Protože oznámení při vyvolání jako součást hello použije operace, klienti zobrazit pouze oznámení pro místně potvrzení operace. A protože operace jsou zaručit pouze toobe místně potvrzené (jinými slovy, přihlášení), se může nebo nemusí být v budoucnu hello odvolat.
* V cestě opakování hello je pro každou operaci použité aktivována jednoho oznámení. To znamená, že pokud transakce T1 obsahuje Create(X), Delete(X) a Create(X), získáte jedno oznámení pro vytvoření hello X, jeden pro odstranění hello a jeden pro vytvoření hello znovu, v tomto pořadí.
* Pro transakce, které obsahují více operací operace se použijí v hello pořadí, ve kterém bylo přijato na primární replice hello od uživatele hello.
* Jako součást zpracování false průběh může být některé operace vrátit zpět. Oznámení jsou vyvolány pro tyto operace vrácení zpět, vracení stavu hello hello repliky back tooa stabilní bodu. Jeden důležitý rozdíl oznámení vrácení zpět je, že jsou agregovaný události, které mají duplicitní klíče. Například pokud se vrací zpět transakci T1, uvidíte tooDelete(X) jednoho oznámení.

## <a name="next-steps"></a>Další kroky
* [Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
* [Spolehlivé služby zálohování a obnovení (zotavení po havárii)](service-fabric-reliable-services-backup-restore.md)
* [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

