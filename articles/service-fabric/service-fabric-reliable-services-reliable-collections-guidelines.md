---
title: "aaaGuidelines & doporučení pro spolehlivé kolekce v Azure Service Fabric | Microsoft Docs"
description: "Pokyny a doporučení pro používání služby Fabric spolehlivé kolekce"
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
ms.date: 5/3/2017
ms.author: mcoskun
ms.openlocfilehash: bcdbc9d013bc044e06c43761e7f515c7e4bf340c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Pokyny a doporučení pro spolehlivé kolekce v Azure Service Fabric
Tato část obsahuje pokyny pro použití spolehlivé správce stavu a spolehlivé kolekce. Hello cílem je, že uživatelé toohelp vyhnout běžné nástrahy.

Hello pokyny jsou uspořádána jako jednoduchý doporučení předponu hello podmínky *provést*, *zvažte*, *nepoužívejte* a *nepodporují*.

* Nelze změnit objekt typu vlastní vrácené operací čtení (například `TryPeekAsync` nebo `TryGetValueAsync`). Spolehlivé kolekcí, stejně jako souběžných kolekcí, vrátí referenční toohello objekty a nelze zkopírovat.
* Provést hloubkové kopie hello vrátil objekt typu vlastní před změnou ho. Vzhledem k tomu, že struktury a vestavěné typy jsou průchodu hodnotu, není nutné toodo kopii hloubkové na ně.
* Nepoužívejte `TimeSpan.MaxValue` časových limitů. Vypršení časových limitů by měl být použité toodetect zablokování.
* Nepoužívejte transakce po bylo potvrzeno, byl zrušen nebo zlikvidován.
* Nepoužívejte výčet mimo obor hello transakce, který byl vytvořen v.
* Nevytvářejte transakce v rámci jiné transakci `using` příkaz vzhledem k tomu může dojít k zablokování.
* Ujistěte se, že vaše `IComparable<TKey>` implementace je správný. Hello systém označí závislost `IComparable<TKey>` ke sloučení kontrolních bodů a řádky.
* Použití aktualizační zámek při čtení položku se tooupdate záměr ho tooprevent třídu zablokování.
* Zvažte zachování položek (například TKey + TValue pro spolehlivé slovník) nižší než 80 kB: menší hello lepší. Tím se snižuje množství hello velkého objektu haldy využití a také diskových a síťových vstupně-VÝSTUPNÍMI požadavky. Často snižuje replikace duplicitních dat, je-li aktualizován pouze jeden malou část hodnoty hello. Běžné tooachieve způsob, jak to spolehlivé slovník je toobreak vaší řádků v toomultiple řádků.
* Zvažte použití zálohování a obnovení po havárii toohave funkce.
* Vyhněte se kombinování jedné entity operací a operací s více entit (např `GetCountAsync`, `CreateEnumerableAsync`) v hello stejné transakci kvůli toohello izolace různé úrovně.
* Zpracování výjimkou InvalidOperationException. Uživatelské transakce může být přerušil hello systému různých důvodů. Například když hello spolehlivé správce stavu mění jeho roli z primární nebo když dlouhotrvající transakci blokuje zkrácení protokolu transakcí hello. V takových případech uživatel obdržet InvalidOperationException, která určuje, že jejich transakce již byla ukončena. Za předpokladu, hello ukončení transakce hello nebyl požadován uživatelem hello, nejlepší způsob, jak toohandle tuto výjimku je toodispose hello transakce, zkontrolujte, jestli nesignalizuje token zrušení hello (nebo se změnily hello role repliky hello) a pokud ne Vytvořte novou transakci a zkuste to znovu.  

Zde jsou některé věci tookeep nezapomeňte:

* Výchozí časový limit Hello je čtyři sekund pro všechny hello spolehlivé kolekce rozhraní API. Většina uživatelů měli používat hello výchozí časový limit.
* je technologie Hello výchozí token zrušení `CancellationToken.None` v všechna rozhraní API spolehlivé kolekce.
* Typ klíče parametr Hello (*TKey*) pro slovník spolehlivé musí správně implementovat `GetHashCode()` a `Equals()`. Klíče musí být neměnitelný.
* tooachieve vysoká dostupnost pro hello spolehlivé kolekcí, každá služba musí mít alespoň cíl a minimální repliky nastavit velikost 3.
* Operace čtení na hello sekundární lze číst verze, které nejsou kvora potvrzené.
  To znamená, že může být verzi data, která je pro čtení z jedné sekundární false zvýšily.
  Čtení z primární jsou vždycky stabilní: můžete nikdy být false zvýšily.

### <a name="next-steps"></a>Další kroky
* [Práce s Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transakce a zámky.](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Správce spolehlivé stavu a interní informace o kolekci](service-fabric-reliable-services-reliable-collections-internals.md)
* Správa dat
  * [Zálohování a obnovení](service-fabric-reliable-services-backup-restore.md)
  * [Oznámení](service-fabric-reliable-services-notifications.md)
  * [Serializace a Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Configuration Manager spolehlivé stavu](service-fabric-reliable-services-configuration.md)
* Ostatní
  * [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
  * [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
