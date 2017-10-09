---
title: "aaaWorking s spolehlivé kolekce | Microsoft Docs"
description: "Přečtěte si hello osvědčené postupy pro práci s spolehlivé kolekce."
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
ms.openlocfilehash: 41ba0b257da8493c1fc2e99ad7565593dc7cbcce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-reliable-collections"></a><span data-ttu-id="e242a-103">Práce s spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="e242a-103">Working with Reliable Collections</span></span>
<span data-ttu-id="e242a-104">Service Fabric nabízí stateful, programovací model vývojáře k dispozici too.NET prostřednictvím spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="e242a-104">Service Fabric offers a stateful programming model available too.NET developers via Reliable Collections.</span></span> <span data-ttu-id="e242a-105">Konkrétně Service Fabric nabízí třídy spolehlivé fronty a spolehlivé slovníku.</span><span class="sxs-lookup"><span data-stu-id="e242a-105">Specifically, Service Fabric provides reliable dictionary and reliable queue classes.</span></span> <span data-ttu-id="e242a-106">Při použití těchto tříd vašemu stavu je rozdělena na oddíly (pro škálovatelnost), replikovat (pro dostupnosti) a transakční v rámci oddílu (pro ACID sémantiku).</span><span class="sxs-lookup"><span data-stu-id="e242a-106">When you use these classes, your state is partitioned (for scalability), replicated (for availability), and transacted within a partition (for ACID semantics).</span></span> <span data-ttu-id="e242a-107">Umožňuje podívejte se na Typickým použitím objektu slovník spolehlivé a jaké jeho ve skutečnosti stav.</span><span class="sxs-lookup"><span data-stu-id="e242a-107">Let’s look at a typical usage of a reliable dictionary object and see what its actually doing.</span></span>

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

      // CommitAsync sends Commit record toolog & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record toolog & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

<span data-ttu-id="e242a-108">Všechny operace v spolehlivé slovník objekty (s výjimkou ClearAsync, což nevratná), vyžadují ITransaction objekt.</span><span class="sxs-lookup"><span data-stu-id="e242a-108">All operations on reliable dictionary objects (except for ClearAsync which is not undoable), require an ITransaction object.</span></span> <span data-ttu-id="e242a-109">Tento objekt se s ním spojená veškeré změny, které se pokoušíte toomake tooany spolehlivé slovník nebo spolehlivé fronty objektů v rámci jednoho oddílu.</span><span class="sxs-lookup"><span data-stu-id="e242a-109">This object has associated with it any and all changes you’re attempting toomake tooany reliable dictionary and/or reliable queue objects within a single partition.</span></span> <span data-ttu-id="e242a-110">Získat ITransaction objekt voláním hello oddílu je metoda CreateTransaction StateManager společnosti.</span><span class="sxs-lookup"><span data-stu-id="e242a-110">You acquire an ITransaction object by calling hello partition’s StateManager’s CreateTransaction method.</span></span>

<span data-ttu-id="e242a-111">Ve výše hello kódu je hello ITransaction objekt předaný metoda AddAsync tooa spolehlivé slovníku.</span><span class="sxs-lookup"><span data-stu-id="e242a-111">In hello code above, hello ITransaction object is passed tooa reliable dictionary’s AddAsync method.</span></span> <span data-ttu-id="e242a-112">Slovník metody, které přijímá klíč interně trvat zámek pro čtečky/zápis přidružené hello klíči.</span><span class="sxs-lookup"><span data-stu-id="e242a-112">Internally, dictionary methods that accepts a key take a reader/writer lock associated with hello key.</span></span> <span data-ttu-id="e242a-113">Pokud metoda hello upraví hello klíč hodnota, hello metoda přebírá uzamčení pro zápis na hello klíč a pokud metoda hello četl pouze z hodnoty hello klíč, pak zámek pro čtení se provede na klíč hello.</span><span class="sxs-lookup"><span data-stu-id="e242a-113">If hello method modifies hello key’s value, hello method takes a write lock on hello key and if hello method only reads from hello key’s value, then a read lock is taken on hello key.</span></span> <span data-ttu-id="e242a-114">Vzhledem k tomu, že AddAsync upraví toohello hodnotu klíče hello nové, je předané hodnoty uzamčení pro zápis hello klíč zabraný.</span><span class="sxs-lookup"><span data-stu-id="e242a-114">Since AddAsync modifies hello key’s value toohello new, passed-in value, hello key’s write lock is taken.</span></span> <span data-ttu-id="e242a-115">Ano, pokud 2 (nebo více) vláken pokus tooadd hodnoty hello stejné klíče v hello stejný čas, jedno vlákno se získat uzamčení pro zápis hello a hello jiná vlákna zablokuje.</span><span class="sxs-lookup"><span data-stu-id="e242a-115">So, if 2 (or more) threads attempt tooadd values with hello same key at hello same time, one thread will acquire hello write lock and hello other threads will block.</span></span> <span data-ttu-id="e242a-116">Ve výchozím nastavení metody blok pro až too4 sekund tooacquire hello zámek; Po 4 a víc sekund throw hello metody TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="e242a-116">By default, methods block for up too4 seconds tooacquire hello lock; after 4 seconds, hello methods throw a TimeoutException.</span></span> <span data-ttu-id="e242a-117">Přetížení metody existují povolení jste toopass hodnotou explicitní vypršení časového limitu, pokud si přejete.</span><span class="sxs-lookup"><span data-stu-id="e242a-117">Method overloads exist allowing you toopass an explicit timeout value if you’d prefer.</span></span>

<span data-ttu-id="e242a-118">Obvykle zápisu váš kód tooreact tooa TimeoutException podle jeho zachytávání a opakovat celou operaci hello (jak je znázorněno v výše uvedený kód hello).</span><span class="sxs-lookup"><span data-stu-id="e242a-118">Usually, you write your code tooreact tooa TimeoutException by catching it and retrying hello entire operation (as shown in hello code above).</span></span> <span data-ttu-id="e242a-119">V jednoduchých kódu právě telefonické Task.Delay předávání pokaždé, když 100 milisekund.</span><span class="sxs-lookup"><span data-stu-id="e242a-119">In my simple code, I’m just calling Task.Delay passing 100 milliseconds each time.</span></span> <span data-ttu-id="e242a-120">Ale ve skutečnosti může být lepší místo toho použít nějaký druh exponenciální zpoždění back vypnout.</span><span class="sxs-lookup"><span data-stu-id="e242a-120">But, in reality, you might be better off using some kind of exponential back-off delay instead.</span></span>

<span data-ttu-id="e242a-121">Po uzamčení hello je získali, AddAsync přidá hello klíč a hodnotu objekt odkazuje na interní dočasné slovník tooan přidružené k objektu ITransaction hello.</span><span class="sxs-lookup"><span data-stu-id="e242a-121">Once hello lock is acquired, AddAsync adds hello key and value object references tooan internal temporary dictionary associated with hello ITransaction object.</span></span> <span data-ttu-id="e242a-122">Děje se tak tooprovide vám sémantiku číst vaše – vlastní – zápis.</span><span class="sxs-lookup"><span data-stu-id="e242a-122">This is done tooprovide you with read-your-own-writes semantics.</span></span> <span data-ttu-id="e242a-123">To znamená, že po zavolání metody AddAsync, novější tooTryGetValueAsync volání (pomocí hello stejný objekt ITransaction) vrátí hodnotu hello i v případě, že ještě nebyly potvrzeny hello transakce.</span><span class="sxs-lookup"><span data-stu-id="e242a-123">That is, after you call AddAsync, a later call tooTryGetValueAsync (using hello same ITransaction object) will return hello value even if you have not yet committed hello transaction.</span></span> <span data-ttu-id="e242a-124">V dalším kroku AddAsync serializuje klíč a hodnotu objekty toobyte pole a připojí tyto bajtové pole tooa souboru protokolu na místní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="e242a-124">Next, AddAsync serializes your key and value objects toobyte arrays and appends these byte arrays tooa log file on hello local node.</span></span> <span data-ttu-id="e242a-125">Nakonec AddAsync odešle klíč/hodnota hello bajtové pole tooall hello sekundární repliky tak budou mít hello stejné informace.</span><span class="sxs-lookup"><span data-stu-id="e242a-125">Finally, AddAsync sends hello byte arrays tooall hello secondary replicas so they have hello same key/value information.</span></span> <span data-ttu-id="e242a-126">I když byl zapsán hello klíč/hodnota informace souboru protokolu tooa, hello informace není považovány za součást hello slovník, dokud hello transakce, které jsou přidružené byla.</span><span class="sxs-lookup"><span data-stu-id="e242a-126">Even though hello key/value information has been written tooa log file, hello information is not considered part of hello dictionary until hello transaction that they are associated with has been committed.</span></span>

<span data-ttu-id="e242a-127">Ve výše hello kódu hello volání tooCommitAsync potvrdí všechny operace hello transakce.</span><span class="sxs-lookup"><span data-stu-id="e242a-127">In hello code above, hello call tooCommitAsync commits all of hello transaction’s operations.</span></span> <span data-ttu-id="e242a-128">Konkrétně připojí potvrzení informace toohello protokolový soubor na místní uzel hello a navíc odesílá hello potvrzení záznamů tooall hello sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="e242a-128">Specifically, it appends commit information toohello log file on hello local node and also sends hello commit record tooall hello secondary replicas.</span></span> <span data-ttu-id="e242a-129">Jakmile má zodpovězených kvora (většinou) hello replik, změny jsou považovány za trvalé a všechny zámky přidružené klíče, které byly manipulovat prostřednictvím objektu ITransaction hello vydání, můžete upravit dalších vláken transakcí všechna data hello stejnými klíči a jejich hodnot.</span><span class="sxs-lookup"><span data-stu-id="e242a-129">Once a quorum (majority) of hello replicas has replied, all data changes are considered permanent and any locks associated with keys that were manipulated via hello ITransaction object are released so other threads/transactions can manipulate hello same keys and their values.</span></span>

<span data-ttu-id="e242a-130">Pokud CommitAsync není volána (obvykle kvůli tooan výjimky), získá objekt ITransaction hello zlikvidován.</span><span class="sxs-lookup"><span data-stu-id="e242a-130">If CommitAsync is not called (usually due tooan exception being thrown), then hello ITransaction object gets disposed.</span></span> <span data-ttu-id="e242a-131">Při uvolnění objekt nepotvrzené ITransaction Service Fabric připojí přerušení informace toohello místního uzlu souboru protokolu a nic musí toobe odeslána tooany hello sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="e242a-131">When disposing an uncommitted ITransaction object, Service Fabric appends abort information toohello local node’s log file and nothing needs toobe sent tooany of hello secondary replicas.</span></span> <span data-ttu-id="e242a-132">A potom všechny zámky přidružené klíče, které byly pomocí transakce hello s nimi manipulovat neuvolní.</span><span class="sxs-lookup"><span data-stu-id="e242a-132">And then, any locks associated with keys that were manipulated via hello transaction are released.</span></span>

## <a name="common-pitfalls-and-how-tooavoid-them"></a><span data-ttu-id="e242a-133">Běžné nástrahy a jak tooavoid je</span><span class="sxs-lookup"><span data-stu-id="e242a-133">Common pitfalls and how tooavoid them</span></span>
<span data-ttu-id="e242a-134">Teď, když budete rozumět tomu, jak fungují spolehlivé kolekce hello interně, Podívejme se na některé běžné nevhodnému použití z nich.</span><span class="sxs-lookup"><span data-stu-id="e242a-134">Now that you understand how hello reliable collections work internally, let’s take a look at some common misuses of them.</span></span> <span data-ttu-id="e242a-135">Zobrazit hello kód níže:</span><span class="sxs-lookup"><span data-stu-id="e242a-135">See hello code below:</span></span>

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes hello name/user, logs hello bytes,
   // & sends hello bytes toohello secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // hello line below updates hello property’s value in memory only; the
   // new value is NOT serialized, logged, & sent toosecondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

<span data-ttu-id="e242a-136">Při práci s slovník regulární .NET, můžete přidat slovník toohello klíč/hodnota a poté změňte hello hodnota vlastnosti (například LastLogin).</span><span class="sxs-lookup"><span data-stu-id="e242a-136">When working with a regular .NET dictionary, you can add a key/value toohello dictionary and then change hello value of a property (such as LastLogin).</span></span> <span data-ttu-id="e242a-137">Tento kód však nebude fungovat správně s slovník spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="e242a-137">However, this code will not work correctly with a reliable dictionary.</span></span> <span data-ttu-id="e242a-138">Mějte na paměti, z hello objekty starší výkladu hello volání tooAddAsync serializuje hello klíč/hodnota toobyte pole a pak uloží hello polí tooa místního souboru a také je odešle toohello sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="e242a-138">Remember from hello earlier discussion, hello call tooAddAsync serializes hello key/value objects toobyte arrays and then saves hello arrays tooa local file and also sends them toohello secondary replicas.</span></span> <span data-ttu-id="e242a-139">Pokud později změníte vlastnost, tato operace změní hodnoty vlastností hello v paměť jenom; nemělo vliv hello místního souboru nebo hello data odeslaná toohello repliky.</span><span class="sxs-lookup"><span data-stu-id="e242a-139">If you later change a property, this changes hello property’s value in memory only; it does not impact hello local file or hello data sent toohello replicas.</span></span> <span data-ttu-id="e242a-140">Pokud dojde k chybě hello procesu, co je v paměti je zahozeny.</span><span class="sxs-lookup"><span data-stu-id="e242a-140">If hello process crashes, what’s in memory is thrown away.</span></span> <span data-ttu-id="e242a-141">Při spuštění nový proces, nebo pokud jiná replika stane primární, pak hello původní hodnota vlastnosti je co je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e242a-141">When a new process starts or if another replica becomes primary, then hello old property value is what is available.</span></span>

<span data-ttu-id="e242a-142">Nelze I vystavila zátěži dostatečně jak je snadné toomake hello druh chybu uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="e242a-142">I cannot stress enough how easy it is toomake hello kind of mistake shown above.</span></span> <span data-ttu-id="e242a-143">A získáte pouze informace o chybu hello případě, že proces hello ocitne mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="e242a-143">And, you will only learn about hello mistake if/when hello process goes down.</span></span> <span data-ttu-id="e242a-144">Hello správný způsob toowrite hello kód je jednoduše tooreverse hello dva řádky:</span><span class="sxs-lookup"><span data-stu-id="e242a-144">hello correct way toowrite hello code is simply tooreverse hello two lines:</span></span>


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

<span data-ttu-id="e242a-145">Zde je další příklad zobrazuje Obvyklou chybou:</span><span class="sxs-lookup"><span data-stu-id="e242a-145">Here is another example showing a common mistake:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use hello user’s name toolook up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // hello user exists in hello dictionary, update one of their properties.
   if (user.HasValue) {
      // hello line below updates hello property’s value in memory only; the
      // new value is NOT serialized, logged, & sent toosecondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

<span data-ttu-id="e242a-146">Znovu s regulární .NET slovník výše uvedený kód hello funguje bez problémů a je běžný vzor: hello developer používá klíče toolook hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e242a-146">Again, with regular .NET dictionaries, hello code above works fine and is a common pattern: hello developer uses a key toolook up a value.</span></span> <span data-ttu-id="e242a-147">Pokud hodnota hello existuje, hello vývojáře změny vlastnosti na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e242a-147">If hello value exists, hello developer changes a property’s value.</span></span> <span data-ttu-id="e242a-148">Však s spolehlivé kolekcí, tento kód vykazuje stejný problém už popsané hello: **objekt nesmí změnit, jakmile ji jste přidělili tooa spolehlivé kolekce.**</span><span class="sxs-lookup"><span data-stu-id="e242a-148">However, with reliable collections, this code exhibits hello same problem as already discussed: **you MUST not modify an object once you have given it tooa reliable collection.**</span></span>

<span data-ttu-id="e242a-149">Hello správný způsob tooupdate hodnota v kolekci spolehlivé je tooget odkaz stávající hodnotu toohello a vezměte v úvahu, že objekt hello označuje tooby tento odkaz neměnné.</span><span class="sxs-lookup"><span data-stu-id="e242a-149">hello correct way tooupdate a value in a reliable collection, is tooget a reference toohello existing value and consider hello object referred tooby this reference immutable.</span></span> <span data-ttu-id="e242a-150">Pak vytvořte nový objekt, který je přesnou kopii původní objekt hello.</span><span class="sxs-lookup"><span data-stu-id="e242a-150">Then, create a new object which is an exact copy of hello original object.</span></span> <span data-ttu-id="e242a-151">Nyní můžete upravit hello stav tohoto objektu nový a zapsat nový objekt hello do kolekce hello tak, aby získá serializovat toobyte polí, připojením toohello místního souboru a odeslat toohello repliky.</span><span class="sxs-lookup"><span data-stu-id="e242a-151">Now, you can modify hello state of this new object and write hello new object into hello collection so that it gets serialized toobyte arrays, appended toohello local file and sent toohello replicas.</span></span> <span data-ttu-id="e242a-152">Po potvrzení hello change(s) hello objektům v paměti, hello místního souboru a mají všechny repliky hello hello stejné přesnou stavu.</span><span class="sxs-lookup"><span data-stu-id="e242a-152">After committing hello change(s), hello in-memory objects, hello local file, and all hello replicas have hello same exact state.</span></span> <span data-ttu-id="e242a-153">Všechny je dobré!</span><span class="sxs-lookup"><span data-stu-id="e242a-153">All is good!</span></span>

<span data-ttu-id="e242a-154">Následující kód Hello ukazuje hello správný způsob tooupdate hodnotu v spolehlivé kolekci:</span><span class="sxs-lookup"><span data-stu-id="e242a-154">hello code below shows hello correct way tooupdate a value in a reliable collection:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use hello user’s name toolook up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // hello user exists in hello dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with hello same state as hello current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In hello new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update hello key’s value toohello updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-tooprevent-programmer-error"></a><span data-ttu-id="e242a-155">Definování chyba programátory tooprevent typy neměnné dat</span><span class="sxs-lookup"><span data-stu-id="e242a-155">Define immutable data types tooprevent programmer error</span></span>
<span data-ttu-id="e242a-156">V ideálním případě by rádi bychom znali hello kompilátoru tooreport chyby při náhodně vytvořit kód, který mění stav objektu, kterou máte tooconsider neměnné.</span><span class="sxs-lookup"><span data-stu-id="e242a-156">Ideally, we’d like hello compiler tooreport errors when you accidentally produce code that mutates state of an object that you are supposed tooconsider immutable.</span></span> <span data-ttu-id="e242a-157">Ale kompilátor jazyka C# hello nemá hello možnost toodo to.</span><span class="sxs-lookup"><span data-stu-id="e242a-157">But, hello C# compiler does not have hello ability toodo this.</span></span> <span data-ttu-id="e242a-158">Ano, tooavoid potenciální programátory chyb, důrazně doporučujeme definovat hello typy, které používáte pro typy neměnné toobe spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="e242a-158">So, tooavoid potential programmer bugs, we highly recommend that you define hello types you use with reliable collections toobe immutable types.</span></span> <span data-ttu-id="e242a-159">Konkrétně to znamená, že jste přilepit toocore typy hodnot (například čísla [Int32, UInt64 atd.], DateTime, Guid, TimeSpan a hello jako).</span><span class="sxs-lookup"><span data-stu-id="e242a-159">Specifically, this means that you stick toocore value types (such as numbers [Int32, UInt64, etc.], DateTime, Guid, TimeSpan, and hello like).</span></span> <span data-ttu-id="e242a-160">A samozřejmě můžete také použít řetězec.</span><span class="sxs-lookup"><span data-stu-id="e242a-160">And, of course, you can also use String.</span></span> <span data-ttu-id="e242a-161">Je nejvhodnější vlastnosti kolekce tooavoid jako serializaci a deserializaci je můžete často může narušit výkonnost.</span><span class="sxs-lookup"><span data-stu-id="e242a-161">It is best tooavoid collection properties as serializing and deserializing them can frequently can hurt performance.</span></span> <span data-ttu-id="e242a-162">Ale pokud chcete vlastnosti kolekce toouse, důrazně doporučujeme použít hello. Knihovna neměnné kolekce na NET ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span><span class="sxs-lookup"><span data-stu-id="e242a-162">However, if you want toouse collection properties, we highly recommend hello use of .NET’s immutable collections library ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span></span> <span data-ttu-id="e242a-163">Tato knihovna je k dispozici ke stažení http://nuget.org. Doporučujeme také zapečetění tříd a provádění pole jen pro čtení, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="e242a-163">This library is available for download from http://nuget.org. We also recommend sealing your classes and making fields read-only whenever possible.</span></span>

<span data-ttu-id="e242a-164">Hello typ informací o uživateli níže ukazuje, jak toodefine na neměnné zadejte využívat výhod zmíněnými doporučení.</span><span class="sxs-lookup"><span data-stu-id="e242a-164">hello UserInfo type below demonstrates how toodefine an immutable type taking advantage of aforementioned recommendations.</span></span>

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
      // Convert hello deserialized collection tooan immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has tooset it. So instead, hello getter is public and hello setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId toohello ItemsBidding
   // collection by creating a new immutable UserInfo object with hello added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

<span data-ttu-id="e242a-165">Hello ItemId typ je také neměnné typu, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e242a-165">hello ItemId type is also an immutable type as shown here:</span></span>

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

## <a name="schema-versioning-upgrades"></a><span data-ttu-id="e242a-166">Verze schématu (upgrade)</span><span class="sxs-lookup"><span data-stu-id="e242a-166">Schema versioning (upgrades)</span></span>
<span data-ttu-id="e242a-167">Interně serializovat spolehlivé kolekce objektů s použitím. NET na DataContractSerializer.</span><span class="sxs-lookup"><span data-stu-id="e242a-167">Internally, Reliable Collections serialize your objects using .NET’s DataContractSerializer.</span></span> <span data-ttu-id="e242a-168">Hello serializovat objekty jsou trvalé toohello primární repliky místního disku a také přenášená toohello sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="e242a-168">hello serialized objects are persisted toohello primary replica’s local disk and are also transmitted toohello secondary replicas.</span></span> <span data-ttu-id="e242a-169">Během existence služby, je pravděpodobné, že budete muset toochange hello druh dat (schéma), vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="e242a-169">As your service matures, it’s likely you’ll want toochange hello kind of data (schema) your service requires.</span></span> <span data-ttu-id="e242a-170">Správa verzí vašich dat s velmi pečlivě musí postupovat.</span><span class="sxs-lookup"><span data-stu-id="e242a-170">You must approach versioning of your data with great care.</span></span> <span data-ttu-id="e242a-171">Především musí být vždy možné toodeserialize stará data.</span><span class="sxs-lookup"><span data-stu-id="e242a-171">First and foremost, you must always be able toodeserialize old data.</span></span> <span data-ttu-id="e242a-172">Konkrétně to znamená deserializace kód musí být nekonečnou zpětně kompatibilní: 333 verze kódu služby musí být schopný toooperate na umístěna v kolekci spolehlivé pomocí verze 1 kódu služby před 5 let.</span><span class="sxs-lookup"><span data-stu-id="e242a-172">Specifically, this means your deserialization code must be infinitely backward compatible: Version 333 of your service code must be able toooperate on data placed in a reliable collection by version 1 of your service code 5 years ago.</span></span>

<span data-ttu-id="e242a-173">Kromě toho kódu služby je upgradovaná jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="e242a-173">Furthermore, service code is upgraded one upgrade domain at a time.</span></span> <span data-ttu-id="e242a-174">Ano při upgradu, máte dvě různé verze vaší služby kód spuštěný současně.</span><span class="sxs-lookup"><span data-stu-id="e242a-174">So, during an upgrade, you have two different versions of your service code running simultaneously.</span></span> <span data-ttu-id="e242a-175">Je nutné nemuseli hello je nová verze kódu služby použít nové schéma hello jako staré verze kódu služby nemusí být možné toohandle hello nové schéma.</span><span class="sxs-lookup"><span data-stu-id="e242a-175">You must avoid having hello new version of your service code use hello new schema as old versions of your service code might not be able toohandle hello new schema.</span></span> <span data-ttu-id="e242a-176">Pokud je to možné, měli byste navrhnout každou verzi vaší služby toobe dopředně kompatibilní verze 1.</span><span class="sxs-lookup"><span data-stu-id="e242a-176">When possible, you should design each version of your service toobe forward compatible by 1 version.</span></span> <span data-ttu-id="e242a-177">Konkrétně to znamená, že byste měli mít V1 kódu služby toosimply ignorovat všechny prvky schématu nezpracovává explicitně.</span><span class="sxs-lookup"><span data-stu-id="e242a-177">Specifically, this means that V1 of your service code should be able toosimply ignore any schema elements it does not explicitly handle.</span></span> <span data-ttu-id="e242a-178">Ale musí být schopný toosave zpět žádná data, není explicitně vědět o a jednoduše zapisuje se při aktualizaci slovníku klíče nebo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e242a-178">However, it must be able toosave any data it doesn’t explicitly know about and simply write it back out when updating a dictionary key or value.</span></span>

> [!WARNING]
> <span data-ttu-id="e242a-179">Při změně schématu hello klíče, je nutné zajistit, že váš klíč hodnota hash a algoritmů rovná jsou stabilní.</span><span class="sxs-lookup"><span data-stu-id="e242a-179">While you can modify hello schema of a key, you must ensure that your key’s hash code and equals algorithms are stable.</span></span> <span data-ttu-id="e242a-180">Pokud změníte způsob buď tyto algoritmy fungovat, nebudete moct toolook hello klíč v rámci slovníku spolehlivé hello někdy znovu.</span><span class="sxs-lookup"><span data-stu-id="e242a-180">If you change how either of these algorithms operate, you will not be able toolook up hello key within hello reliable dictionary ever again.</span></span>
>
>

<span data-ttu-id="e242a-181">Alternativně můžete provádět, co se obvykle označují tooas 2fáze upgradu.</span><span class="sxs-lookup"><span data-stu-id="e242a-181">Alternatively, you can perform what is typically referred tooas a 2-phase upgrade.</span></span> <span data-ttu-id="e242a-182">Fáze 2 inovaci, upgradujte službu z V1 tooV2: V2 obsahuje hello kód, který zná, jak neprovede toodeal s hello novou změnu schématu, ale tento kód.</span><span class="sxs-lookup"><span data-stu-id="e242a-182">With a 2-phase upgrade, you upgrade your service from V1 tooV2: V2 contains hello code that knows how toodeal with hello new schema change but this code doesn’t execute.</span></span> <span data-ttu-id="e242a-183">Když hello V2 kód čte V1 data, pracuje na něm a zapisuje V1 data.</span><span class="sxs-lookup"><span data-stu-id="e242a-183">When hello V2 code reads V1 data, it operates on it and writes V1 data.</span></span> <span data-ttu-id="e242a-184">Poté po dokončení upgradu hello je všechny upgradu domény, vám může nějakým způsobem signál toohello spuštěné instance V2, že dokončení tohoto upgradu hello je.</span><span class="sxs-lookup"><span data-stu-id="e242a-184">Then, after hello upgrade is complete across all upgrade domains, you can somehow signal toohello running V2 instances that hello upgrade is complete.</span></span> <span data-ttu-id="e242a-185">(Jedním ze způsobů toosignal Toto je tooroll se konfigurace upgradu; to je to díky upgradu fáze 2.) Instance V2 hello můžete nyní, čtení dat V1, ji převést tooV2 data, pracovat s nimi a vypsat jako V2 data.</span><span class="sxs-lookup"><span data-stu-id="e242a-185">(One way toosignal this is tooroll out a configuration upgrade; this is what makes this a 2-phase upgrade.) Now, hello V2 instances can read V1 data, convert it tooV2 data, operate on it, and write it out as V2 data.</span></span> <span data-ttu-id="e242a-186">Když ostatní instance načíst V2 data, nepotřebují tooconvert, se právě pracovat s nimi a zápis dat V2.</span><span class="sxs-lookup"><span data-stu-id="e242a-186">When other instances read V2 data, they do not need tooconvert it, they just operate on it, and write out V2 data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e242a-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e242a-187">Next Steps</span></span>
<span data-ttu-id="e242a-188">toolearn o vytváření dopředně kompatibilní datové kontrakty, najdete v části [kontrakty dat dopřednou](https://msdn.microsoft.com/library/ms731083.aspx).</span><span class="sxs-lookup"><span data-stu-id="e242a-188">toolearn about creating forward compatible data contracts, see [Forward-Compatible Data Contracts](https://msdn.microsoft.com/library/ms731083.aspx).</span></span>

<span data-ttu-id="e242a-189">osvědčené postupy toolearn na Správa verzí kontraktů dat, najdete v části [Správa verzí kontraktů dat](https://msdn.microsoft.com/library/ms731138.aspx).</span><span class="sxs-lookup"><span data-stu-id="e242a-189">toolearn best practices on versioning data contracts, see [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span></span>

<span data-ttu-id="e242a-190">toolearn způsobu měnící data odolný vůči chybám tooimplement verzi, najdete v [verze proti chybám zpětná volání serializace tolerantní](https://msdn.microsoft.com/library/ms733734.aspx).</span><span class="sxs-lookup"><span data-stu-id="e242a-190">toolearn how tooimplement version tolerant data contracts, see [Version-Tolerant Serialization Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).</span></span>

<span data-ttu-id="e242a-191">toolearn jak zjistit, tooprovide datová struktura, která dokáže spolupracovat napříč více verzí [objekt IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="e242a-191">toolearn how tooprovide a data structure that can interoperate across multiple versions, see [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span></span>
