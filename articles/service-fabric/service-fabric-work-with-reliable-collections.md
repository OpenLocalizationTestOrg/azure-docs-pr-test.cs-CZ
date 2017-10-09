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
# <a name="working-with-reliable-collections"></a>Práce s spolehlivé kolekce
Service Fabric nabízí stateful, programovací model vývojáře k dispozici too.NET prostřednictvím spolehlivé kolekce. Konkrétně Service Fabric nabízí třídy spolehlivé fronty a spolehlivé slovníku. Při použití těchto tříd vašemu stavu je rozdělena na oddíly (pro škálovatelnost), replikovat (pro dostupnosti) a transakční v rámci oddílu (pro ACID sémantiku). Umožňuje podívejte se na Typickým použitím objektu slovník spolehlivé a jaké jeho ve skutečnosti stav.

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

Všechny operace v spolehlivé slovník objekty (s výjimkou ClearAsync, což nevratná), vyžadují ITransaction objekt. Tento objekt se s ním spojená veškeré změny, které se pokoušíte toomake tooany spolehlivé slovník nebo spolehlivé fronty objektů v rámci jednoho oddílu. Získat ITransaction objekt voláním hello oddílu je metoda CreateTransaction StateManager společnosti.

Ve výše hello kódu je hello ITransaction objekt předaný metoda AddAsync tooa spolehlivé slovníku. Slovník metody, které přijímá klíč interně trvat zámek pro čtečky/zápis přidružené hello klíči. Pokud metoda hello upraví hello klíč hodnota, hello metoda přebírá uzamčení pro zápis na hello klíč a pokud metoda hello četl pouze z hodnoty hello klíč, pak zámek pro čtení se provede na klíč hello. Vzhledem k tomu, že AddAsync upraví toohello hodnotu klíče hello nové, je předané hodnoty uzamčení pro zápis hello klíč zabraný. Ano, pokud 2 (nebo více) vláken pokus tooadd hodnoty hello stejné klíče v hello stejný čas, jedno vlákno se získat uzamčení pro zápis hello a hello jiná vlákna zablokuje. Ve výchozím nastavení metody blok pro až too4 sekund tooacquire hello zámek; Po 4 a víc sekund throw hello metody TimeoutException. Přetížení metody existují povolení jste toopass hodnotou explicitní vypršení časového limitu, pokud si přejete.

Obvykle zápisu váš kód tooreact tooa TimeoutException podle jeho zachytávání a opakovat celou operaci hello (jak je znázorněno v výše uvedený kód hello). V jednoduchých kódu právě telefonické Task.Delay předávání pokaždé, když 100 milisekund. Ale ve skutečnosti může být lepší místo toho použít nějaký druh exponenciální zpoždění back vypnout.

Po uzamčení hello je získali, AddAsync přidá hello klíč a hodnotu objekt odkazuje na interní dočasné slovník tooan přidružené k objektu ITransaction hello. Děje se tak tooprovide vám sémantiku číst vaše – vlastní – zápis. To znamená, že po zavolání metody AddAsync, novější tooTryGetValueAsync volání (pomocí hello stejný objekt ITransaction) vrátí hodnotu hello i v případě, že ještě nebyly potvrzeny hello transakce. V dalším kroku AddAsync serializuje klíč a hodnotu objekty toobyte pole a připojí tyto bajtové pole tooa souboru protokolu na místní uzel hello. Nakonec AddAsync odešle klíč/hodnota hello bajtové pole tooall hello sekundární repliky tak budou mít hello stejné informace. I když byl zapsán hello klíč/hodnota informace souboru protokolu tooa, hello informace není považovány za součást hello slovník, dokud hello transakce, které jsou přidružené byla.

Ve výše hello kódu hello volání tooCommitAsync potvrdí všechny operace hello transakce. Konkrétně připojí potvrzení informace toohello protokolový soubor na místní uzel hello a navíc odesílá hello potvrzení záznamů tooall hello sekundární repliky. Jakmile má zodpovězených kvora (většinou) hello replik, změny jsou považovány za trvalé a všechny zámky přidružené klíče, které byly manipulovat prostřednictvím objektu ITransaction hello vydání, můžete upravit dalších vláken transakcí všechna data hello stejnými klíči a jejich hodnot.

Pokud CommitAsync není volána (obvykle kvůli tooan výjimky), získá objekt ITransaction hello zlikvidován. Při uvolnění objekt nepotvrzené ITransaction Service Fabric připojí přerušení informace toohello místního uzlu souboru protokolu a nic musí toobe odeslána tooany hello sekundární repliky. A potom všechny zámky přidružené klíče, které byly pomocí transakce hello s nimi manipulovat neuvolní.

## <a name="common-pitfalls-and-how-tooavoid-them"></a>Běžné nástrahy a jak tooavoid je
Teď, když budete rozumět tomu, jak fungují spolehlivé kolekce hello interně, Podívejme se na některé běžné nevhodnému použití z nich. Zobrazit hello kód níže:

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

Při práci s slovník regulární .NET, můžete přidat slovník toohello klíč/hodnota a poté změňte hello hodnota vlastnosti (například LastLogin). Tento kód však nebude fungovat správně s slovník spolehlivé. Mějte na paměti, z hello objekty starší výkladu hello volání tooAddAsync serializuje hello klíč/hodnota toobyte pole a pak uloží hello polí tooa místního souboru a také je odešle toohello sekundární repliky. Pokud později změníte vlastnost, tato operace změní hodnoty vlastností hello v paměť jenom; nemělo vliv hello místního souboru nebo hello data odeslaná toohello repliky. Pokud dojde k chybě hello procesu, co je v paměti je zahozeny. Při spuštění nový proces, nebo pokud jiná replika stane primární, pak hello původní hodnota vlastnosti je co je k dispozici.

Nelze I vystavila zátěži dostatečně jak je snadné toomake hello druh chybu uvedené výše. A získáte pouze informace o chybu hello případě, že proces hello ocitne mimo provoz. Hello správný způsob toowrite hello kód je jednoduše tooreverse hello dva řádky:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Zde je další příklad zobrazuje Obvyklou chybou:

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

Znovu s regulární .NET slovník výše uvedený kód hello funguje bez problémů a je běžný vzor: hello developer používá klíče toolook hodnotu. Pokud hodnota hello existuje, hello vývojáře změny vlastnosti na hodnotu. Však s spolehlivé kolekcí, tento kód vykazuje stejný problém už popsané hello: **objekt nesmí změnit, jakmile ji jste přidělili tooa spolehlivé kolekce.**

Hello správný způsob tooupdate hodnota v kolekci spolehlivé je tooget odkaz stávající hodnotu toohello a vezměte v úvahu, že objekt hello označuje tooby tento odkaz neměnné. Pak vytvořte nový objekt, který je přesnou kopii původní objekt hello. Nyní můžete upravit hello stav tohoto objektu nový a zapsat nový objekt hello do kolekce hello tak, aby získá serializovat toobyte polí, připojením toohello místního souboru a odeslat toohello repliky. Po potvrzení hello change(s) hello objektům v paměti, hello místního souboru a mají všechny repliky hello hello stejné přesnou stavu. Všechny je dobré!

Následující kód Hello ukazuje hello správný způsob tooupdate hodnotu v spolehlivé kolekci:

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

## <a name="define-immutable-data-types-tooprevent-programmer-error"></a>Definování chyba programátory tooprevent typy neměnné dat
V ideálním případě by rádi bychom znali hello kompilátoru tooreport chyby při náhodně vytvořit kód, který mění stav objektu, kterou máte tooconsider neměnné. Ale kompilátor jazyka C# hello nemá hello možnost toodo to. Ano, tooavoid potenciální programátory chyb, důrazně doporučujeme definovat hello typy, které používáte pro typy neměnné toobe spolehlivé kolekce. Konkrétně to znamená, že jste přilepit toocore typy hodnot (například čísla [Int32, UInt64 atd.], DateTime, Guid, TimeSpan a hello jako). A samozřejmě můžete také použít řetězec. Je nejvhodnější vlastnosti kolekce tooavoid jako serializaci a deserializaci je můžete často může narušit výkonnost. Ale pokud chcete vlastnosti kolekce toouse, důrazně doporučujeme použít hello. Knihovna neměnné kolekce na NET ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Tato knihovna je k dispozici ke stažení http://nuget.org. Doporučujeme také zapečetění tříd a provádění pole jen pro čtení, kdykoli je to možné.

Hello typ informací o uživateli níže ukazuje, jak toodefine na neměnné zadejte využívat výhod zmíněnými doporučení.

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

Hello ItemId typ je také neměnné typu, jak je vidět tady:

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

## <a name="schema-versioning-upgrades"></a>Verze schématu (upgrade)
Interně serializovat spolehlivé kolekce objektů s použitím. NET na DataContractSerializer. Hello serializovat objekty jsou trvalé toohello primární repliky místního disku a také přenášená toohello sekundární repliky. Během existence služby, je pravděpodobné, že budete muset toochange hello druh dat (schéma), vaše služba vyžaduje. Správa verzí vašich dat s velmi pečlivě musí postupovat. Především musí být vždy možné toodeserialize stará data. Konkrétně to znamená deserializace kód musí být nekonečnou zpětně kompatibilní: 333 verze kódu služby musí být schopný toooperate na umístěna v kolekci spolehlivé pomocí verze 1 kódu služby před 5 let.

Kromě toho kódu služby je upgradovaná jednu upgradovací doménu najednou. Ano při upgradu, máte dvě různé verze vaší služby kód spuštěný současně. Je nutné nemuseli hello je nová verze kódu služby použít nové schéma hello jako staré verze kódu služby nemusí být možné toohandle hello nové schéma. Pokud je to možné, měli byste navrhnout každou verzi vaší služby toobe dopředně kompatibilní verze 1. Konkrétně to znamená, že byste měli mít V1 kódu služby toosimply ignorovat všechny prvky schématu nezpracovává explicitně. Ale musí být schopný toosave zpět žádná data, není explicitně vědět o a jednoduše zapisuje se při aktualizaci slovníku klíče nebo hodnoty.

> [!WARNING]
> Při změně schématu hello klíče, je nutné zajistit, že váš klíč hodnota hash a algoritmů rovná jsou stabilní. Pokud změníte způsob buď tyto algoritmy fungovat, nebudete moct toolook hello klíč v rámci slovníku spolehlivé hello někdy znovu.
>
>

Alternativně můžete provádět, co se obvykle označují tooas 2fáze upgradu. Fáze 2 inovaci, upgradujte službu z V1 tooV2: V2 obsahuje hello kód, který zná, jak neprovede toodeal s hello novou změnu schématu, ale tento kód. Když hello V2 kód čte V1 data, pracuje na něm a zapisuje V1 data. Poté po dokončení upgradu hello je všechny upgradu domény, vám může nějakým způsobem signál toohello spuštěné instance V2, že dokončení tohoto upgradu hello je. (Jedním ze způsobů toosignal Toto je tooroll se konfigurace upgradu; to je to díky upgradu fáze 2.) Instance V2 hello můžete nyní, čtení dat V1, ji převést tooV2 data, pracovat s nimi a vypsat jako V2 data. Když ostatní instance načíst V2 data, nepotřebují tooconvert, se právě pracovat s nimi a zápis dat V2.

## <a name="next-steps"></a>Další kroky
toolearn o vytváření dopředně kompatibilní datové kontrakty, najdete v části [kontrakty dat dopřednou](https://msdn.microsoft.com/library/ms731083.aspx).

osvědčené postupy toolearn na Správa verzí kontraktů dat, najdete v části [Správa verzí kontraktů dat](https://msdn.microsoft.com/library/ms731138.aspx).

toolearn způsobu měnící data odolný vůči chybám tooimplement verzi, najdete v [verze proti chybám zpětná volání serializace tolerantní](https://msdn.microsoft.com/library/ms733734.aspx).

toolearn jak zjistit, tooprovide datová struktura, která dokáže spolupracovat napříč více verzí [objekt IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
