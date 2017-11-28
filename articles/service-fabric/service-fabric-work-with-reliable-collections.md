---
title: "Práce s spolehlivé kolekce | Microsoft Docs"
description: "Zjistěte, osvědčené postupy pro práci s spolehlivé kolekce."
services: service-fabric
documentationcenter: .net
author: rajak
manager: timlt
editor: 
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rajak
ms.openlocfilehash: f53f13e4fb83b1cd370ec673e86e5311cd93055f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-reliable-collections"></a><span data-ttu-id="e15e0-103">Práce s spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="e15e0-103">Working with Reliable Collections</span></span>
<span data-ttu-id="e15e0-104">Service Fabric nabízí stavová programovací model k dispozici pro vývojáře .NET prostřednictvím spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="e15e0-104">Service Fabric offers a stateful programming model available to .NET developers via Reliable Collections.</span></span> <span data-ttu-id="e15e0-105">Konkrétně Service Fabric nabízí třídy spolehlivé fronty a spolehlivé slovníku.</span><span class="sxs-lookup"><span data-stu-id="e15e0-105">Specifically, Service Fabric provides reliable dictionary and reliable queue classes.</span></span> <span data-ttu-id="e15e0-106">Při použití těchto tříd vašemu stavu je rozdělena na oddíly (pro škálovatelnost), replikovat (pro dostupnosti) a transakční v rámci oddílu (pro ACID sémantiku).</span><span class="sxs-lookup"><span data-stu-id="e15e0-106">When you use these classes, your state is partitioned (for scalability), replicated (for availability), and transacted within a partition (for ACID semantics).</span></span> <span data-ttu-id="e15e0-107">Umožňuje podívejte se na Typickým použitím objektu slovník spolehlivé a jaké jeho ve skutečnosti stav.</span><span class="sxs-lookup"><span data-stu-id="e15e0-107">Let’s look at a typical usage of a reliable dictionary object and see what its actually doing.</span></span>

```csharp

///retry:

try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

<span data-ttu-id="e15e0-108">Všechny operace v spolehlivé slovník objekty (s výjimkou ClearAsync, což nevratná), vyžadují ITransaction objekt.</span><span class="sxs-lookup"><span data-stu-id="e15e0-108">All operations on reliable dictionary objects (except for ClearAsync which is not undoable), require an ITransaction object.</span></span> <span data-ttu-id="e15e0-109">Tento objekt má přidruženo žádné a všechny změny, které se pokoušíte provést jakékoli spolehlivé slovníku nebo spolehlivé fronty objektů v rámci jednoho oddílu.</span><span class="sxs-lookup"><span data-stu-id="e15e0-109">This object has associated with it any and all changes you’re attempting to make to any reliable dictionary and/or reliable queue objects within a single partition.</span></span> <span data-ttu-id="e15e0-110">Získat ITransaction objekt voláním oddílu je metoda CreateTransaction StateManager společnosti.</span><span class="sxs-lookup"><span data-stu-id="e15e0-110">You acquire an ITransaction object by calling the partition’s StateManager’s CreateTransaction method.</span></span>

<span data-ttu-id="e15e0-111">Ve výše uvedeném kódu je objekt ITransaction předaný metodě AddAsync spolehlivé slovník.</span><span class="sxs-lookup"><span data-stu-id="e15e0-111">In the code above, the ITransaction object is passed to a reliable dictionary’s AddAsync method.</span></span> <span data-ttu-id="e15e0-112">Slovník metody, které přijímá klíč interně trvat zámek pro čtečky/zápis přidružené ke klíči.</span><span class="sxs-lookup"><span data-stu-id="e15e0-112">Internally, dictionary methods that accepts a key take a reader/writer lock associated with the key.</span></span> <span data-ttu-id="e15e0-113">Pokud metoda mění hodnotu klíče, tato metoda přebírá uzamčení pro zápis na klíč, a pokud metoda četl pouze z hodnoty klíče, pak zámek pro čtení se provede na klíč.</span><span class="sxs-lookup"><span data-stu-id="e15e0-113">If the method modifies the key’s value, the method takes a write lock on the key and if the method only reads from the key’s value, then a read lock is taken on the key.</span></span> <span data-ttu-id="e15e0-114">Vzhledem k tomu, že AddAsync upraví hodnota klíče na nové, předané hodnotu, je převzat uzamčení pro zápis klíče.</span><span class="sxs-lookup"><span data-stu-id="e15e0-114">Since AddAsync modifies the key’s value to the new, passed-in value, the key’s write lock is taken.</span></span> <span data-ttu-id="e15e0-115">Ano Pokud vláken 2 (nebo více) došlo k pokusu o přidání hodnot se stejným klíčem ve stejnou dobu, jedno vlákno se získat uzamčení pro zápis a bude blokovat jiná vlákna.</span><span class="sxs-lookup"><span data-stu-id="e15e0-115">So, if 2 (or more) threads attempt to add values with the same key at the same time, one thread will acquire the write lock and the other threads will block.</span></span> <span data-ttu-id="e15e0-116">Ve výchozím nastavení metody blok pro až 4 sekundy získat zámek; metody po 4 a víc sekund, throw TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="e15e0-116">By default, methods block for up to 4 seconds to acquire the lock; after 4 seconds, the methods throw a TimeoutException.</span></span> <span data-ttu-id="e15e0-117">Přetížení metody existovat, což umožňuje předat hodnotu explicitní vypršení časového limitu, pokud si přejete.</span><span class="sxs-lookup"><span data-stu-id="e15e0-117">Method overloads exist allowing you to pass an explicit timeout value if you’d prefer.</span></span>

<span data-ttu-id="e15e0-118">Obvykle psaní kódu reagování na TimeoutException podle jeho zachytávání a opakovat celou operaci (jak je uvedeno ve výše uvedeném kódu).</span><span class="sxs-lookup"><span data-stu-id="e15e0-118">Usually, you write your code to react to a TimeoutException by catching it and retrying the entire operation (as shown in the code above).</span></span> <span data-ttu-id="e15e0-119">V jednoduchých kódu právě telefonické Task.Delay předávání pokaždé, když 100 milisekund.</span><span class="sxs-lookup"><span data-stu-id="e15e0-119">In my simple code, I’m just calling Task.Delay passing 100 milliseconds each time.</span></span> <span data-ttu-id="e15e0-120">Ale ve skutečnosti může být lepší místo toho použít nějaký druh exponenciální zpoždění back vypnout.</span><span class="sxs-lookup"><span data-stu-id="e15e0-120">But, in reality, you might be better off using some kind of exponential back-off delay instead.</span></span>

<span data-ttu-id="e15e0-121">Jakmile je získat zámek, AddAsync přidá klíč a hodnotu objektu odkazy na interní dočasné slovník, která je přidružená k objektu ITransaction.</span><span class="sxs-lookup"><span data-stu-id="e15e0-121">Once the lock is acquired, AddAsync adds the key and value object references to an internal temporary dictionary associated with the ITransaction object.</span></span> <span data-ttu-id="e15e0-122">Důvodem je, že bude potřeba poskytnout sémantiku číst vaše – vlastní – zápis.</span><span class="sxs-lookup"><span data-stu-id="e15e0-122">This is done to provide you with read-your-own-writes semantics.</span></span> <span data-ttu-id="e15e0-123">To znamená, že po zavolání metody AddAsync novější volání TryGetValueAsync (s použitím stejného objektu ITransaction) vrátí hodnotu i v případě, že ještě nebyly potvrzené transakce.</span><span class="sxs-lookup"><span data-stu-id="e15e0-123">That is, after you call AddAsync, a later call to TryGetValueAsync (using the same ITransaction object) will return the value even if you have not yet committed the transaction.</span></span> <span data-ttu-id="e15e0-124">V dalším kroku AddAsync serializuje klíč a hodnotu objektů na pole bajtů a připojí tyto bajtová pole do souboru protokolu v místním uzlu.</span><span class="sxs-lookup"><span data-stu-id="e15e0-124">Next, AddAsync serializes your key and value objects to byte arrays and appends these byte arrays to a log file on the local node.</span></span> <span data-ttu-id="e15e0-125">Nakonec AddAsync odešle bajtová pole na všech sekundárních replikách, mají stejné informace klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="e15e0-125">Finally, AddAsync sends the byte arrays to all the secondary replicas so they have the same key/value information.</span></span> <span data-ttu-id="e15e0-126">I když informace klíč/hodnota byl zapsán do souboru protokolu, informace není považovány za součást slovníku, dokud transakce, které jsou přidružené byla.</span><span class="sxs-lookup"><span data-stu-id="e15e0-126">Even though the key/value information has been written to a log file, the information is not considered part of the dictionary until the transaction that they are associated with has been committed.</span></span>

<span data-ttu-id="e15e0-127">Ve výše uvedeném kódu volání CommitAsync potvrdí všechny operace transakce.</span><span class="sxs-lookup"><span data-stu-id="e15e0-127">In the code above, the call to CommitAsync commits all of the transaction’s operations.</span></span> <span data-ttu-id="e15e0-128">Konkrétně připojí potvrzení informace do souboru protokolu v místním uzlu a také odesílá potvrzení záznam všech sekundárních replikách.</span><span class="sxs-lookup"><span data-stu-id="e15e0-128">Specifically, it appends commit information to the log file on the local node and also sends the commit record to all the secondary replicas.</span></span> <span data-ttu-id="e15e0-129">Jakmile má zodpovězených kvora (většinou) replik, všechny změny dat jsou považovány za trvalé a všechny zámky přidružené klíče, které byly manipulovat prostřednictvím objektu ITransaction vydávají tak dalších vláken transakcí můžete upravit stejnými klíči a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e15e0-129">Once a quorum (majority) of the replicas has replied, all data changes are considered permanent and any locks associated with keys that were manipulated via the ITransaction object are released so other threads/transactions can manipulate the same keys and their values.</span></span>

<span data-ttu-id="e15e0-130">Pokud CommitAsync není volána (obvykle kvůli výjimce hlášeny), získá objekt ITransaction zlikvidován.</span><span class="sxs-lookup"><span data-stu-id="e15e0-130">If CommitAsync is not called (usually due to an exception being thrown), then the ITransaction object gets disposed.</span></span> <span data-ttu-id="e15e0-131">Při zahazování objekt nepotvrzené ITransaction Service Fabric připojí přerušení informace do souboru protokolu je místní uzel a nic potřebuje k odeslání do všech sekundárních replikách.</span><span class="sxs-lookup"><span data-stu-id="e15e0-131">When disposing an uncommitted ITransaction object, Service Fabric appends abort information to the local node’s log file and nothing needs to be sent to any of the secondary replicas.</span></span> <span data-ttu-id="e15e0-132">A potom všechny zámky přidružené klíče, které byly pomocí transakce s nimi manipulovat neuvolní.</span><span class="sxs-lookup"><span data-stu-id="e15e0-132">And then, any locks associated with keys that were manipulated via the transaction are released.</span></span>

## <a name="common-pitfalls-and-how-to-avoid-them"></a><span data-ttu-id="e15e0-133">Běžné nástrahy a jak se vyhnout jejich</span><span class="sxs-lookup"><span data-stu-id="e15e0-133">Common pitfalls and how to avoid them</span></span>
<span data-ttu-id="e15e0-134">Teď, když budete rozumět tomu, jak fungují spolehlivé kolekce interně, Podívejme se na některé běžné nevhodnému použití z nich.</span><span class="sxs-lookup"><span data-stu-id="e15e0-134">Now that you understand how the reliable collections work internally, let’s take a look at some common misuses of them.</span></span> <span data-ttu-id="e15e0-135">Viz následující kód:</span><span class="sxs-lookup"><span data-stu-id="e15e0-135">See the code below:</span></span>

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

<span data-ttu-id="e15e0-136">Při práci s slovník regulární .NET, můžete přidat klíč/hodnota do slovníku a poté změňte hodnotu vlastnosti (například LastLogin).</span><span class="sxs-lookup"><span data-stu-id="e15e0-136">When working with a regular .NET dictionary, you can add a key/value to the dictionary and then change the value of a property (such as LastLogin).</span></span> <span data-ttu-id="e15e0-137">Tento kód však nebude fungovat správně s slovník spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="e15e0-137">However, this code will not work correctly with a reliable dictionary.</span></span> <span data-ttu-id="e15e0-138">Mějte na paměti z dřívějších diskusní volání AddAsync serializuje objekty klíč/hodnota, které se bajtová pole a pak uloží pole do místního souboru a také je odešle do sekundárních replikách.</span><span class="sxs-lookup"><span data-stu-id="e15e0-138">Remember from the earlier discussion, the call to AddAsync serializes the key/value objects to byte arrays and then saves the arrays to a local file and also sends them to the secondary replicas.</span></span> <span data-ttu-id="e15e0-139">Pokud později změníte vlastnost, tato operace změní hodnotu vlastnosti v paměti pouze; nemělo vliv místního souboru nebo data odeslaná do replik.</span><span class="sxs-lookup"><span data-stu-id="e15e0-139">If you later change a property, this changes the property’s value in memory only; it does not impact the local file or the data sent to the replicas.</span></span> <span data-ttu-id="e15e0-140">Pokud dojde k chybě procesu, co je v paměti je zahozeny.</span><span class="sxs-lookup"><span data-stu-id="e15e0-140">If the process crashes, what’s in memory is thrown away.</span></span> <span data-ttu-id="e15e0-141">Při spuštění nový proces nebo jiný replika stane primární, původní hodnota vlastnosti je co je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e15e0-141">When a new process starts or if another replica becomes primary, then the old property value is what is available.</span></span>

<span data-ttu-id="e15e0-142">Nelze I vystavila zátěži dostatečně jak je snadné aby druh chybu uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="e15e0-142">I cannot stress enough how easy it is to make the kind of mistake shown above.</span></span> <span data-ttu-id="e15e0-143">A získáte pouze informace o chybu případě, že proces přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="e15e0-143">And, you will only learn about the mistake if/when the process goes down.</span></span> <span data-ttu-id="e15e0-144">Správný způsob, jak napsat kód je jednoduše reverse dva řádky:</span><span class="sxs-lookup"><span data-stu-id="e15e0-144">The correct way to write the code is simply to reverse the two lines:</span></span>


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

<span data-ttu-id="e15e0-145">Zde je další příklad zobrazuje Obvyklou chybou:</span><span class="sxs-lookup"><span data-stu-id="e15e0-145">Here is another example showing a common mistake:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

<span data-ttu-id="e15e0-146">Znovu s regulární .NET slovník výše uvedený kód funguje bez problémů a je běžný vzor: vývojář použije klíč k vyhledání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e15e0-146">Again, with regular .NET dictionaries, the code above works fine and is a common pattern: the developer uses a key to look up a value.</span></span> <span data-ttu-id="e15e0-147">Pokud hodnota existuje, vývojáři změní vlastnosti na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e15e0-147">If the value exists, the developer changes a property’s value.</span></span> <span data-ttu-id="e15e0-148">Však s spolehlivé kolekcí, tento kód vykazuje stejný problém už popsané: **objekt nesmí změnit, po které jste zadali na kolekci spolehlivé.**</span><span class="sxs-lookup"><span data-stu-id="e15e0-148">However, with reliable collections, this code exhibits the same problem as already discussed: **you MUST not modify an object once you have given it to a reliable collection.**</span></span>

<span data-ttu-id="e15e0-149">Správný způsob aktualizujte hodnotu v spolehlivé kolekci, je-li stávající hodnotu a zvažte objektu, na kterou odkazuje tento odkaz neměnné.</span><span class="sxs-lookup"><span data-stu-id="e15e0-149">The correct way to update a value in a reliable collection, is to get a reference to the existing value and consider the object referred to by this reference immutable.</span></span> <span data-ttu-id="e15e0-150">Pak vytvořte nový objekt, který je přesnou kopii původní objekt.</span><span class="sxs-lookup"><span data-stu-id="e15e0-150">Then, create a new object which is an exact copy of the original object.</span></span> <span data-ttu-id="e15e0-151">Nyní můžete upravit stav tento nový objekt a zapsat nový objekt do kolekce tak, aby získá serializovat na pole bajtů, připojí k místního souboru a odeslat do replik.</span><span class="sxs-lookup"><span data-stu-id="e15e0-151">Now, you can modify the state of this new object and write the new object into the collection so that it gets serialized to byte arrays, appended to the local file and sent to the replicas.</span></span> <span data-ttu-id="e15e0-152">Po potvrzení změny, mají přesném stavu, ve stejné objekty v paměti, místního souboru a všechny repliky.</span><span class="sxs-lookup"><span data-stu-id="e15e0-152">After committing the change(s), the in-memory objects, the local file, and all the replicas have the same exact state.</span></span> <span data-ttu-id="e15e0-153">Všechny je dobré!</span><span class="sxs-lookup"><span data-stu-id="e15e0-153">All is good!</span></span>

<span data-ttu-id="e15e0-154">Následující kód ukazuje správný způsob aktualizujte hodnotu v spolehlivé kolekci:</span><span class="sxs-lookup"><span data-stu-id="e15e0-154">The code below shows the correct way to update a value in a reliable collection:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a><span data-ttu-id="e15e0-155">Definování typů neměnné dat, aby se zabránilo chybě programátory</span><span class="sxs-lookup"><span data-stu-id="e15e0-155">Define immutable data types to prevent programmer error</span></span>
<span data-ttu-id="e15e0-156">V ideálním případě by rádi bychom znali kompilátoru zprávy o chybách, když náhodně vytvořit kód, který mění stavu objektu, který se má vzít v úvahu neměnné.</span><span class="sxs-lookup"><span data-stu-id="e15e0-156">Ideally, we’d like the compiler to report errors when you accidentally produce code that mutates state of an object that you are supposed to consider immutable.</span></span> <span data-ttu-id="e15e0-157">Ale kompilátor jazyka C# nemá možnost to udělat.</span><span class="sxs-lookup"><span data-stu-id="e15e0-157">But, the C# compiler does not have the ability to do this.</span></span> <span data-ttu-id="e15e0-158">Tak, aby se zabránilo potenciální chyby programátory, důrazně doporučujeme, aby definování typů pomocí spolehlivé kolekce se nedá změnit typy.</span><span class="sxs-lookup"><span data-stu-id="e15e0-158">So, to avoid potential programmer bugs, we highly recommend that you define the types you use with reliable collections to be immutable types.</span></span> <span data-ttu-id="e15e0-159">Konkrétně to znamená, že přilepit na základní typy hodnot (například čísla [Int32, UInt64 atd.], DateTime, Guid, časový interval a podobně).</span><span class="sxs-lookup"><span data-stu-id="e15e0-159">Specifically, this means that you stick to core value types (such as numbers [Int32, UInt64, etc.], DateTime, Guid, TimeSpan, and the like).</span></span> <span data-ttu-id="e15e0-160">A samozřejmě můžete také použít řetězec.</span><span class="sxs-lookup"><span data-stu-id="e15e0-160">And, of course, you can also use String.</span></span> <span data-ttu-id="e15e0-161">Je vyhýbat se vlastnosti kolekce jako serializaci a deserializaci je můžete často může narušit výkonnost.</span><span class="sxs-lookup"><span data-stu-id="e15e0-161">It is best to avoid collection properties as serializing and deserializing them can frequently can hurt performance.</span></span> <span data-ttu-id="e15e0-162">Ale pokud budete chtít použít vlastnosti kolekce, důrazně doporučujeme použít. Knihovna neměnné kolekce na NET ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span><span class="sxs-lookup"><span data-stu-id="e15e0-162">However, if you want to use collection properties, we highly recommend the use of .NET’s immutable collections library ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span></span> <span data-ttu-id="e15e0-163">Tato knihovna je k dispozici ke stažení http://nuget.org.</span><span class="sxs-lookup"><span data-stu-id="e15e0-163">This library is available for download from http://nuget.org.</span></span> <span data-ttu-id="e15e0-164">Doporučujeme také zapečetění tříd a provádění pole jen pro čtení, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="e15e0-164">We also recommend sealing your classes and making fields read-only whenever possible.</span></span>

<span data-ttu-id="e15e0-165">Typ informací o uživateli níže ukazuje, jak definovat typ neměnné využívat výhod zmíněnými doporučení.</span><span class="sxs-lookup"><span data-stu-id="e15e0-165">The UserInfo type below demonstrates how to define an immutable type taking advantage of aforementioned recommendations.</span></span>

```csharp

[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

<span data-ttu-id="e15e0-166">Typ ItemId je také neměnné typu, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e15e0-166">The ItemId type is also an immutable type as shown here:</span></span>

```csharp

[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a><span data-ttu-id="e15e0-167">Verze schématu (upgrade)</span><span class="sxs-lookup"><span data-stu-id="e15e0-167">Schema versioning (upgrades)</span></span>
<span data-ttu-id="e15e0-168">Interně serializovat spolehlivé kolekce objektů s použitím. NET na DataContractSerializer.</span><span class="sxs-lookup"><span data-stu-id="e15e0-168">Internally, Reliable Collections serialize your objects using .NET’s DataContractSerializer.</span></span> <span data-ttu-id="e15e0-169">Serializovaných objektů jsou ukládaný na místním disku primární repliky a přenášejí na sekundárních replikách.</span><span class="sxs-lookup"><span data-stu-id="e15e0-169">The serialized objects are persisted to the primary replica’s local disk and are also transmitted to the secondary replicas.</span></span> <span data-ttu-id="e15e0-170">Během existence služby, je pravděpodobné, že budete chtít změnit druhu dat (schéma), vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="e15e0-170">As your service matures, it’s likely you’ll want to change the kind of data (schema) your service requires.</span></span> <span data-ttu-id="e15e0-171">Správa verzí vašich dat s velmi pečlivě musí postupovat.</span><span class="sxs-lookup"><span data-stu-id="e15e0-171">You must approach versioning of your data with great care.</span></span> <span data-ttu-id="e15e0-172">Především musíte být vždy možné deserializovat stará data.</span><span class="sxs-lookup"><span data-stu-id="e15e0-172">First and foremost, you must always be able to deserialize old data.</span></span> <span data-ttu-id="e15e0-173">Konkrétně to znamená deserializace kód musí být nekonečnou zpětně kompatibilní: 333 verze kódu služby musí být schopen pracovat umístěna v kolekci spolehlivé pomocí verze 1 kódu služby před 5 let.</span><span class="sxs-lookup"><span data-stu-id="e15e0-173">Specifically, this means your deserialization code must be infinitely backward compatible: Version 333 of your service code must be able to operate on data placed in a reliable collection by version 1 of your service code 5 years ago.</span></span>

<span data-ttu-id="e15e0-174">Kromě toho kódu služby je upgradovaná jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="e15e0-174">Furthermore, service code is upgraded one upgrade domain at a time.</span></span> <span data-ttu-id="e15e0-175">Ano při upgradu, máte dvě různé verze vaší služby kód spuštěný současně.</span><span class="sxs-lookup"><span data-stu-id="e15e0-175">So, during an upgrade, you have two different versions of your service code running simultaneously.</span></span> <span data-ttu-id="e15e0-176">Je nutné nemuseli novou verzi kódu služby používaly nové schéma jako staré verze kódu služby nemusí být schopna zpracovávat nové schéma.</span><span class="sxs-lookup"><span data-stu-id="e15e0-176">You must avoid having the new version of your service code use the new schema as old versions of your service code might not be able to handle the new schema.</span></span> <span data-ttu-id="e15e0-177">Pokud je to možné, měli byste navrhnout každou verzi služby jako dopředně kompatibilní verze 1.</span><span class="sxs-lookup"><span data-stu-id="e15e0-177">When possible, you should design each version of your service to be forward compatible by 1 version.</span></span> <span data-ttu-id="e15e0-178">Konkrétně to znamená, že V1 kódu služby byste měli mít ignorovat všechny prvky schématu, který nezpracovává explicitně.</span><span class="sxs-lookup"><span data-stu-id="e15e0-178">Specifically, this means that V1 of your service code should be able to simply ignore any schema elements it does not explicitly handle.</span></span> <span data-ttu-id="e15e0-179">Ale musí být možné uložit data, které není explicitně vědět o a jednoduše zápis zpátky se při aktualizaci slovníku klíče nebo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e15e0-179">However, it must be able to save any data it doesn’t explicitly know about and simply write it back out when updating a dictionary key or value.</span></span>

> [!WARNING]
> <span data-ttu-id="e15e0-180">Při změně schématu klíče, je nutné zajistit, že váš klíč hodnota hash a algoritmů rovná jsou stabilní.</span><span class="sxs-lookup"><span data-stu-id="e15e0-180">While you can modify the schema of a key, you must ensure that your key’s hash code and equals algorithms are stable.</span></span> <span data-ttu-id="e15e0-181">Pokud změníte způsob buď tyto algoritmy fungovat, nebudete moci vyhledávat klíč v rámci slovníku spolehlivé někdy znovu.</span><span class="sxs-lookup"><span data-stu-id="e15e0-181">If you change how either of these algorithms operate, you will not be able to look up the key within the reliable dictionary ever again.</span></span>
>
>

<span data-ttu-id="e15e0-182">Alternativně můžete provádět, co se obvykle označuje jako 2 fáze upgradu.</span><span class="sxs-lookup"><span data-stu-id="e15e0-182">Alternatively, you can perform what is typically referred to as a 2-phase upgrade.</span></span> <span data-ttu-id="e15e0-183">Fáze 2 inovaci, upgradu služby V1 na V2: V2 obsahuje kód, který umí jak nakládat s novou změnu schématu, ale tento kód není spuštěn.</span><span class="sxs-lookup"><span data-stu-id="e15e0-183">With a 2-phase upgrade, you upgrade your service from V1 to V2: V2 contains the code that knows how to deal with the new schema change but this code doesn’t execute.</span></span> <span data-ttu-id="e15e0-184">Pokud kód V2 čte V1 data, pracuje na něm a zapisuje V1 data.</span><span class="sxs-lookup"><span data-stu-id="e15e0-184">When the V2 code reads V1 data, it operates on it and writes V1 data.</span></span> <span data-ttu-id="e15e0-185">Potom po dokončení upgradu je napříč doménami všechny upgradu, můžete mohou nějakým způsobem signál spuštěné instance V2 dokončení upgradu je.</span><span class="sxs-lookup"><span data-stu-id="e15e0-185">Then, after the upgrade is complete across all upgrade domains, you can somehow signal to the running V2 instances that the upgrade is complete.</span></span> <span data-ttu-id="e15e0-186">(Jeden ze způsobů, jak signál jde zavádět konfigurace upgradu; to je to díky upgradu fáze 2.) Instance V2 můžete nyní, čtení dat V1, převést na V2 data, pracovat s nimi a vypsat jako V2 data.</span><span class="sxs-lookup"><span data-stu-id="e15e0-186">(One way to signal this is to roll out a configuration upgrade; this is what makes this a 2-phase upgrade.) Now, the V2 instances can read V1 data, convert it to V2 data, operate on it, and write it out as V2 data.</span></span> <span data-ttu-id="e15e0-187">Když ostatní instance načíst V2 data, není potřeba ji převést, se právě pracovat s nimi a zápis dat V2.</span><span class="sxs-lookup"><span data-stu-id="e15e0-187">When other instances read V2 data, they do not need to convert it, they just operate on it, and write out V2 data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e15e0-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e15e0-188">Next Steps</span></span>
<span data-ttu-id="e15e0-189">Další informace o vytváření dopředně kompatibilní datové kontrakty, najdete v části [kontrakty dat dopřednou](https://msdn.microsoft.com/library/ms731083.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15e0-189">To learn about creating forward compatible data contracts, see [Forward-Compatible Data Contracts](https://msdn.microsoft.com/library/ms731083.aspx).</span></span>

<span data-ttu-id="e15e0-190">Osvědčené postupy v Správa verzí kontraktů dat naleznete v tématu [Správa verzí kontraktů dat](https://msdn.microsoft.com/library/ms731138.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15e0-190">To learn best practices on versioning data contracts, see [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span></span>

<span data-ttu-id="e15e0-191">Zjistěte, jak implementovat kontrakty odolný vůči chybám dat verze, najdete v tématu [verze proti chybám zpětná volání serializace tolerantní](https://msdn.microsoft.com/library/ms733734.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15e0-191">To learn how to implement version tolerant data contracts, see [Version-Tolerant Serialization Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).</span></span>

<span data-ttu-id="e15e0-192">Zjistěte, jak zajistit datová struktura, která dokáže spolupracovat napříč více verzí, najdete v tématu [objekt IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="e15e0-192">To learn how to provide a data structure that can interoperate across multiple versions, see [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span></span>
