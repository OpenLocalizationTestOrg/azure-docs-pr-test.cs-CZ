---
title: "aaaIntroduction tooReliable kolekce v stavové služby Azure Service Fabric | Microsoft Docs"
description: "Service Fabric stavové služby poskytují spolehlivé kolekce, které umožňují toowrite vysoce dostupných, škálovatelných a nízkou latencí cloudové aplikace."
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
ms.openlocfilehash: 9f67c48f13e8b91b84977e127e2545cbb9d9a158
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooreliable-collections-in-azure-service-fabric-stateful-services"></a>Úvod tooReliable kolekce v stavové služby Azure Service Fabric
Spolehlivé kolekce povolit toowrite vysoce dostupných, škálovatelných a nízkou latencí cloudové aplikace, jako by byly psaní aplikací jeden počítač. Hello třídy v hello **Microsoft.ServiceFabric.Data.Collections** obor názvů poskytují sadu kolekcí, které automaticky nastavit vašemu stavu jako vysoce dostupný. Vývojáři potřebovat pouze toohello tooprogram spolehlivé kolekce rozhraní API a nechat spolehlivé kolekce správě hello replikovat a místní stavu.

Hello klíče rozdíl mezi spolehlivé kolekce a další technologie vysokou dostupnost (například Redis, služby Azure Table a fronty Azure service) je, že hello stavu bude místně nacházet v instanci služby hello při také prováděné vysoce dostupný. To znamená, že:

* Všechny operace čtení jsou místní, výsledkem s nízkou latencí a vysokou propustností čte.
* Všech zápisů zpoplatněná hello minimální počet IOs sítě, což vede k s nízkou latencí a vysokou propustností zapisuje.

![Obrázek vývoj kolekcí.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Spolehlivé kolekce si lze představit jako hello přirozené vývoj hello **System.Collections** třídy: novou sadu kolekcí, které jsou určené pro cloudové a více počítači aplikace hello bez zvyšuje složitost pro Hello developer. Jako takový spolehlivé kolekce jsou:

* Replikované: Se pro zajištění vysoké dostupnosti replikují změny stavu.
* Trvalé: Data jsou trvalé toodisk pro odolnost proti výpadkům ve velkém rozsahu (například výpadku napájení datacenter).
* Asynchronního: Rozhraní API jsou asynchronní tooensure že vláken nejsou blokované při by docházelo k vstupně-výstupní operace.
* Transakční: Rozhraní API využívat hello abstrakce transakce, můžete snadno spravovat více spolehlivé kolekcí v rámci služby.

Spolehlivé kolekce poskytují silnou konzistenci zaručuje mimo toomake pole hello důvody o stavu aplikace jednodušší.
Silnou konzistenci je dosaženo tím, že zajistí transakce, které potvrzení dokončit až poté, co byl zaprotokolován hello celá transakce na většinu kvora repliky, včetně primární hello.
tooachieve slabší konzistence, aplikace můžete vědomí back toohello klienta nebo žadatel, před vrátí hello asynchronního potvrzování.

Hello spolehlivé kolekce rozhraní API jsou vývojem souběžných kolekcí rozhraní API (v hello najít **System.Collections.Concurrent** oboru názvů):

* Asynchronní: Vrátí úlohu, protože na rozdíl od souběžných kolekcí hello operace jsou replikována a trvalé.
* Žádná výstupní parametry: používá `ConditionalValue<T>` tooreturn bool a hodnota místo výstupní parametry. `ConditionalValue<T>`je třeba `Nullable<T>` ale nevyžaduje T toobe struktury.
* Transakce: Používá transakční objekt tooenable hello uživatele toogroup akce na více kolekcí spolehlivé v transakci.

V současné době **Microsoft.ServiceFabric.Data.Collections** obsahuje tři kolekce:

* [Spolehlivé slovník](https://msdn.microsoft.com/library/azure/dn971511.aspx): představuje replikovaných transakcí a asynchronní kolekci dvojic klíč/hodnota. Podobně jako příliš**ConcurrentDictionary**, obě hello klíč a hodnotu hello můžou být jakéhokoli typu.
* [Spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx): představuje replikovaných transakcí a asynchronní striktní first-in použity fronty. Podobně jako příliš**ConcurrentQueue**, hello hodnota může být jakéhokoli typu.
* [Spolehlivé souběžných fronty](service-fabric-reliable-services-reliable-concurrent-queue.md): představuje replikovaných transakcí a asynchronní úsilí nejlepší řazení fronty pro vysoké propustnosti. Podobně jako toohello **ConcurrentQueue**, hello hodnota může být jakéhokoli typu.

## <a name="next-steps"></a>Další kroky
* [Spolehlivé kolekce pokyny a doporučení](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Práce s Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transakce a zámky.](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Správce spolehlivé stavu a interní informace o kolekci](service-fabric-reliable-services-reliable-collections-internals.md)
* Správa dat
  * [Zálohování a obnovení](service-fabric-reliable-services-backup-restore.md)
  * [Oznámení](service-fabric-reliable-services-notifications.md)
  * [Spolehlivá serializace kolekcí](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serializace a Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Configuration Manager spolehlivé stavu](service-fabric-reliable-services-configuration.md)
* Ostatní
  * [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
  * [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
