---
title: "aaaOverview životního cyklu hello služby Azure Service Fabric spolehlivé | Microsoft Docs"
description: "Další informace o události různých životního cyklu hello v Service Fabric spolehlivé služby"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 6d48c217d12bc5248c2da57b544aac747cecd872
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Přehled životního cyklu spolehlivé služby
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java v Linuxu](service-fabric-reliable-services-lifecycle-java.md)
>
>

Pokud přemýšlíte o životních hello spolehlivé služeb, jsou hello základy životního cyklu hello hello nejdůležitější. Obecně platí:

* Při spuštění
  * Služby se vytvářejí.
  * Nemají tooconstruct příležitostí a vrátit nula nebo více – moduly naslouchání
  * Jsou otevřené žádné vrácený naslouchací procesy, povolení komunikace se službou hello
  * Služba Hello runAsync metoda je volána, povolit hello služby toodo dlouho spuštěný nebo práce na pozadí
* Při vypnutí
  * Hello zrušení token předaný toorunAsync se zruší, a jsou uzavřeny hello – moduly naslouchání
  * Po který je dokončena, je destructed samotného objektu služby hello

Přesný řazení tyto události jsou podrobnosti kolem hello. Konkrétně hello pořadí událostí se může v závislosti na tom, jestli je hello spolehlivá služba Stateless nebo Stateful mírně změnit. Kromě toho pro stavové služby, máme toodeal se scénářem primárních prohození hello. Během tohoto pořadí hello role primární replikou přenášená tooanother (nebo zpátky) bez hello vypnutí služby. Nakonec máme toothink o podmínkách chyb nebo selhání.

## <a name="stateless-service-startup"></a>Spuštění bezstavové služby
životní cyklus Hello bezstavové služby je poměrně jednoduché. Tady je hello pořadí událostí:

1. Hello služby je vytvořený
2. Potom v paralelní dvě věci situace:
    - `StatelessService.createServiceInstanceListeners()`je voláno, a jsou otevřené žádné vrácený naslouchací procesy (`CommunicationListener.openAsync()` se volá na každý naslouchací proces)
    - Hello metodě runAsync služby (`StatelessService.runAsync()`) se nazývá
3. Pokud je k dispozici, je volána metoda onOpenAsync služby vlastní hello (konkrétně `StatelessService.onOpenAsync()` je volána. Toto je neobvyklé přepsání ale je k dispozici).

Je důležité toonote se žádné řazení mezi hello volání toocreate a otevřete hello naslouchací procesy a runAsync. moduly pro naslouchání Hello může zobrazit před zahájením runAsync. Podobně runAsync může stát volána před naslouchací procesy komunikace hello jsou otevřené nebo i byla vytvořená. Pokud žádné synchronizace je vyžadována, je ponechán jako implementátora toohello cvičení. Běžná řešení:

* Někdy naslouchací procesy nemůže fungovat, dokud některé další informace se vytvoří nebo fungovat Hotovo. Pro bezstavové služby, které pracovní obvykle stačí v konstruktoru hello služby během hello `createServiceInstanceListeners()` volat, nebo jako součást hello konstrukce naslouchací proces hello sám sebe.
* Někdy hello kód v runAsync nechce toostart dokud hello moduly pro naslouchání jsou otevřené. V takovém případě je nutné další spolupráce. Běžným řešením je některé příznak v rámci hello naslouchací procesy, která udává, když nebude dokončena, který se změnami runAsync před pokračováním tooactual pracovní.

## <a name="stateless-service-shutdown"></a>Vypnutí bezstavové služby
Při vypínání bezstavové služby, hello stejného vzoru je pak, právě obráceným:

1. Paralelní
    - Zavřeny všechny otevřené naslouchací procesy (`CommunicationListener.closeAsync()` se volá na každý naslouchací proces)
    - token zrušení Hello předán příliš`runAsync()` zrušeny (Kontrola token zrušení hello `isCancelled` vlastnost vrací hodnotu true a pokud volána hello token `throwIfCancellationRequested` metoda vrátí `CancellationException`)
2. Jednou `closeAsync()` dokončení na každý naslouchací proces a `runAsync()` také dokončení hello služby `StatelessService.onCloseAsync()` metoda je volána, pokud je k dispozici (znovu jde neobvyklé přepsání).
3. Po `StatelessService.onCloseAsync()` dokončí, je destructed objekt služby hello

## <a name="notes-on-service-lifecycle"></a>Poznámky k služby životního cyklu
* Obě hello `runAsync()` metoda a hello `createServiceInstanceListeners` volání jsou volitelné. Služby může mít jeden z nich, obě nebo ani jeden z nich. Například pokud hello služba nemá svou práci v odpovědi toouser volání, není třeba pro něj tooimplement `runAsync()`. Pouze naslouchací procesy komunikace hello a jejich přidružené kód jsou nezbytné. Podobně je nepovinný hello služby může mít pouze pozadí pracovní toodo vytváření a vrácení naslouchací procesy komunikace a proto stačí tooimplement`runAsync()`
* Je platná pro služby toocomplete `runAsync()` úspěšně a zpět z něj. Toto není považováno za selhání podmínku a by představují práce pozadí hello od služby hello. Pro stavové služby spolehlivé `runAsync()` by volán znovu, pokud služba hello došlo k degradaci z primární a poté vyzval back tooprimary.
* Pokud služba ukončí z `runAsync()` po vyvolání výjimky neočekávané výjimce, jedná se o chybu a objekt služby hello se vypne a oznámila Chyba stavu.
* Když žádný časový limit na vrácení z těchto metod, okamžitě ztratit hello možnost toowrite a proto nelze dokončit všechna skutečná práce. Doporučuje se vrátíte co nejrychleji po přijetí hello žádost o zrušení. Pokud vaše služba neodpovídá volání toothese rozhraní API v přiměřené době Service Fabric může vynucené ukončení služby. Obvykle tato situace nastane pouze během upgradu aplikace nebo když se odstraňuje služby. Tento časový limit je 15 minut ve výchozím nastavení.
* Selhání v hello `onCloseAsync()` cesta povede `onAbort()` volané, což je možnost best effort poslední chance pro hello služby tooclean nahoru a uvolnění prostředků, které budou požadovaly.

> [!NOTE]
> Stavová spolehlivé služby ještě nepodporuje v jazyce java.
>
>

## <a name="next-steps"></a>Další kroky
* [Úvod tooReliable služby](service-fabric-reliable-services-introduction.md)
* [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
* [Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)
