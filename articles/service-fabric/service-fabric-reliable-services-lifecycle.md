---
title: "aaaOverview životního cyklu hello služby Azure Service Fabric spolehlivé | Microsoft Docs"
description: "Další informace o události různých životního cyklu hello v Service Fabric spolehlivé služby"
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 0d75ed5ee7cda85ac9af6a02e160727277804a2b
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

- Při spuštění
  - Služby se vytvářejí.
  - Nemají tooconstruct příležitostí a vrátit nula nebo více – moduly naslouchání
  - Jsou otevřené žádné vrácený naslouchací procesy, povolení komunikace se službou hello
  - Povolit hello RunAsync Hello služby, které metoda je volána, služba toodo dlouho spuštěný nebo práce na pozadí
- Při vypnutí
  - Hello zrušení token předaný tooRunAsync se zruší, a jsou uzavřeny hello – moduly naslouchání
  - Po který je dokončena, je destructed samotného objektu služby hello

Přesný řazení tyto události jsou podrobnosti kolem hello. Konkrétně hello pořadí událostí se může v závislosti na tom, jestli je hello spolehlivá služba Stateless nebo Stateful mírně změnit. Kromě toho pro stavové služby, máme toodeal se scénářem primárních prohození hello. Během tohoto pořadí hello role primární replikou přenášená tooanother (nebo zpátky) bez hello vypnutí služby. Nakonec máme toothink o podmínkách chyb nebo selhání.

## <a name="stateless-service-startup"></a>Spuštění bezstavové služby
životní cyklus Hello bezstavové služby je poměrně jednoduché. Tady je hello pořadí událostí:

1. Hello služby je vytvořený
2. Potom v paralelní dvě věci situace:
    - `StatelessService.CreateServiceInstanceListeners()`je voláno, a jsou otevřené žádné vrácený naslouchací procesy. `ICommunicationListener.OpenAsync()`je volána v každé naslouchací proces
    - Hello služby `StatelessService.RunAsync()` metoda je volána.
3. Pokud je k dispozici, hello služby `StatelessService.OnOpenAsync()` metoda je volána. Toto je neobvyklé přepsání, ale je k dispozici.

Je důležité toonote se žádné řazení mezi hello volání toocreate a otevřete hello naslouchací procesy a RunAsync. moduly pro naslouchání Hello může zobrazit před zahájením RunAsync. Podobně RunAsync může stát volána před naslouchací procesy komunikace hello jsou otevřené nebo i byla vytvořená. Pokud žádné synchronizace je vyžadována, je ponechán jako implementátora toohello cvičení. Běžná řešení:

  - Někdy naslouchací procesy nemůže fungovat, dokud některé další informace se vytvoří nebo fungovat Hotovo. Pro bezstavové služby, které pracovní obvykle stačí v jiných umístěních, jako například: 
    - v konstruktoru hello služby
    - během hello `CreateServiceInstanceListeners()` volání
    - jako součást hello konstrukce naslouchací proces hello sám sebe
  - Někdy hello kód v RunAsync nechce toostart dokud hello moduly pro naslouchání jsou otevřené. V takovém případě je nutné další spolupráce. Běžným řešením je některé příznak v rámci hello naslouchací procesy, která určuje, že dokončily. Tento příznak se pak změnami RunAsync před pokračováním tooactual práci.

## <a name="stateless-service-shutdown"></a>Vypnutí bezstavové služby
Při vypínání bezstavové služby, hello stejného vzoru je pak, právě obráceným:

1. Paralelní
    - Jsou zavřeny všechny otevřené naslouchací procesy. `ICommunicationListener.CloseAsync()`je volána v každé naslouchací proces.
    - token zrušení Hello předán příliš`RunAsync()` je zrušená. Kontrola, zda text hello token zrušení pro `IsCancellationRequested` vlastnost vrací hodnotu true a pokud volána hello token `ThrowIfCancellationRequested` metoda vrátí `OperationCanceledException`.
2. Jednou `CloseAsync()` dokončení na každý naslouchací proces a `RunAsync()` také dokončení hello služby `StatelessService.OnCloseAsync()` metoda je volána, pokud je k dispozici. Je neobvyklé toooverride `StatelessService.OnCloseAsync()`.
3. Po `StatelessService.OnCloseAsync()` dokončí, je destructed objekt služby hello

## <a name="stateful-service-startup"></a>Stavové služby spuštění
Stavové služby máte podobné služby toostateless vzor, s několik změn. Při spuštění stavové služby, hello pořadí událostí je následující:

1. Hello služby je vytvořený
2. `StatefulServiceBase.OnOpenAsync()`je volána. To je uncommonly přepsat ve službě hello.
3. Hello následující umět poradit paralelně
    - `StatefulServiceBase.CreateServiceReplicaListeners()`vyvolání 
      - Pokud je služba hello primární všechny vrácený naslouchací procesy jsou otevřené. `ICommunicationListener.OpenAsync()`je volána v každé naslouchací proces.
      - Pokud je služba hello sekundární, pouze tyto moduly pro naslouchání označen jako `ListenOnSecondary = true` jsou otevřené. Naslouchací procesy, které jsou otevřené na sekundárních hodnot je méně častých.
    - text Hello, pokud hello služba je aktuálně primární, služba hello `StatefulServiceBase.RunAsync()` metoda je volána.
4. Jakmile se všechny hello naslouchací proces repliky `OpenAsync()` dokončení volání a `RunAsync()` je volána, `StatefulServiceBase.OnChangeRoleAsync()` je volána. To je uncommonly přepsat ve službě hello.

Podobně toostateless služby, neexistuje žádný koordinovat hello pořadí, ve které hello naslouchací procesy vytvořil a spustil a kdy se nazývá RunAsync. Pokud potřebujete koordinaci, jsou mnohem hello řešení hello stejné. Je zde jeden další případ: Řekněme, že volání hello přicházejících u naslouchací procesy komunikace hello vyžadovat informace uložené v produktu některé [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md). Protože naslouchací procesy komunikace hello by bylo možné otevřít před hello spolehlivé kolekce jsou z něj číst nebo zapisovat, a než může začít RunAsync, je nutné některé další spolupráce. Hello nejjednodušší a nejběžnější řešení je pro tooreturn naslouchací procesy komunikace hello některé kód chyby, který hello klienta používá tooknow tooretry hello požadavku.

## <a name="stateful-service-shutdown"></a>Stavové služby vypnutí
Podobně služby tooStateless události životního cyklu hello během vypnutí jsou hello stejný jako při spuštění, ale vrátit zpět. Když se právě vypíná stavové služby, hello k následujícím událostem:

1. Paralelní
    - Jsou zavřeny všechny otevřené naslouchací procesy. `ICommunicationListener.CloseAsync()`je volána v každé naslouchací proces.
    - token zrušení Hello předán příliš`RunAsync()` je zrušená. Kontrola, zda text hello token zrušení pro `IsCancellationRequested` vlastnost vrací hodnotu true a pokud volána hello token `ThrowIfCancellationRequested` metoda vrátí `OperationCanceledException`.
2. Jednou `CloseAsync()` dokončení na každý naslouchací proces a `RunAsync()` také dokončení hello služby `StatefulServiceBase.OnChangeRoleAsync()` je volána. (To je uncommonly přepsat v hello service.)
    - Čekání na RunAsync toocomplete je nutné pouze pokud byl primární repliky této služby.
3. Jednou hello `StatefulServiceBase.OnChangeRoleAsync()` Metoda dokončení hello `StatefulServiceBase.OnCloseAsync()` metoda je volána. Toto je neobvyklé přepsání, ale je k dispozici.
3. Po `StatefulServiceBase.OnCloseAsync()` dokončí, je destructed objekt služby hello.

## <a name="stateful-service-primary-swaps"></a>Primární záměna stavové služby
Když je spuštěný stavové služby, hello primární repliky této stavové služby mít pouze jejich naslouchací procesy komunikace otevřít a jejich RunAsync metodu s názvem. Sekundární se vytvářejí ale zobrazit žádná další volání. Ale průběhu stavové služby, které replika je právě hello primární můžete změnit. Co to znamená v podmínkách hello životního cyklu události, které můžete zobrazit repliku? Hello chování hello stavová repliky vidí závisí na tom, jestli je hello repliky se degradaci nebo povýší během hello swap.

### <a name="for-hello-primary-being-demoted"></a>Primární hello se nesníží úroveň
Service Fabric musí tato replika toostop zpracování zpráv a ukončete všechny pozadí práce, kterou její probíhající činnosti. V důsledku toho se právě vypíná tento krok vypadá podobně jako toowhen hello služby. Jeden rozdílem je, že hello služby není destructed nebo uzavřeny vzhledem k tomu, že zůstane jako sekundární. Hello následující rozhraní API se označují jako:

1. Paralelní
    - Jsou zavřeny všechny otevřené naslouchací procesy. `ICommunicationListener.CloseAsync()`je volána v každé naslouchací proces.
    - token zrušení Hello předán příliš`RunAsync()` je zrušená. Kontrola, zda text hello token zrušení pro `IsCancellationRequested` vlastnost vrací hodnotu true a pokud volána hello token `ThrowIfCancellationRequested` metoda vrátí `OperationCanceledException`.
2. Jednou `CloseAsync()` dokončení na každý naslouchací proces a `RunAsync()` také dokončení hello služby `StatefulServiceBase.OnChangeRoleAsync()` je volána. To je uncommonly přepsat ve službě hello.

### <a name="for-hello-secondary-being-promoted"></a>Pro sekundární, jejíž úroveň zvyšujete hello
Podobně Service Fabric musí tato replika toostart naslouchání zpráv na hello přenosu a spusťte úlohy na pozadí, které ho záleží. V důsledku toho, tento proces vypadá podobně jako služba hello toowhen je vytvořena, s výjimkou repliky hello samotné již existuje. Hello následující rozhraní API se označují jako:

1. Paralelní
    - `StatefulServiceBase.CreateServiceReplicaListeners()`je voláno, a jsou otevřené žádné vrácený naslouchací procesy. `ICommunicationListener.OpenAsync()`je volána v každé naslouchací proces.
    - Hello služby `StatefulServiceBase.RunAsync()` metoda je volána.
2. Jakmile se všechny hello naslouchací proces repliky `OpenAsync()` dokončení volání a `RunAsync()` byla volána, `StatefulServiceBase.OnChangeRoleAsync()` je volána. To je uncommonly přepsat ve službě hello.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Běžné problémy při vypnutí stavové služby a primární snížení úrovně
Změny Service Fabric hello primární stavové služby z různých důvodů. jsou technologie Hello nejčastějším [clusteru vyrovnává](service-fabric-cluster-resource-manager-balancing.md) a [upgradu aplikace](service-fabric-application-upgrade.md). Během těchto operací (i během vypnutí normální služby, jako by se zobrazí, pokud byla odstraněna hello služby), je důležité, že hello služby ohledem hello `CancellationToken`. Služby, které zpracovávají zrušení řádně bude mít několik problémy. Konkrétně tyto operace bude pomalé, vzhledem k tomu, že Service Fabric hello služby toostop čeká řádně. Nakonec to může vést toofailed upgrady tohoto časového limitu a vrátit zpět. Token zrušení hello toohonor selhání může také způsobit imbalanced clustery, protože uzly získat aktivní, ale hello služby nemůže být znovu vyrovnána vzhledem k tomu, jak dlouho trvá příliš dlouho toomove je jinde. 

Vzhledem k tomu, že jsou stavové služby hello, je také pravděpodobné, že používají hello [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md). V Service Fabric při snížení úrovně primární jedním hello první věcí, které se stane, je zruší se oprávnění k zápisu toohello základní stavu. To vede tooa druhé sadě problémy, které můžou mít vliv hello služby životního cyklu. kolekce Hello návratový výjimky založené na hello načasování a jestli přesouvá hello repliky nebo vypnutí. Tyto výjimky by měl být správně zpracovat. Výjimky vyvolané Service Fabric spadají do trvalého [(`FabricException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) a přechodný [(`FabricTransientException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategorií. Trvalé výjimky by měl zaznamenána a vyvolána, zatímco hello přechodné výjimky může opakována na základě logiky některé opakování.

Zpracování výjimek hello, které pocházejí z použití hello `ReliableCollections` ve spojení s událostmi služby životního cyklu, je důležitou součástí testování a ověření spolehlivě. Hello doporučení je vždy toorun služby zatížení při provádění upgradu a [chaos testování](service-fabric-controlled-chaos.md) před nasazením někdy tooproduction. Tyto základní postup pomůže, zajistěte, aby služby je implementovaná správně a zpracovává události životního cyklu správně.


## <a name="notes-on-service-lifecycle"></a>Poznámky k služby životního cyklu
  - Obě hello `RunAsync()` metoda a hello `CreateServiceReplicaListeners/CreateServiceInstanceListeners` volání jsou volitelné. Služby může mít jeden z nich, obě nebo ani jeden z nich. Například pokud hello služba nemá svou práci v odpovědi toouser volání, není třeba pro něj tooimplement `RunAsync()`. Pouze naslouchací procesy komunikace hello a jejich přidružené kód jsou nezbytné. Podobně je nepovinný hello služby může mít pouze pozadí pracovní toodo vytváření a vrácení naslouchací procesy komunikace a proto stačí tooimplement`RunAsync()`
  - Je platná pro služby toocomplete `RunAsync()` úspěšně a zpět z něj. Dokončení není podmínce chyby. Dokončení `RunAsync()` označuje, že práce pozadí hello hello služby byla dokončena. Pro stavové služby spolehlivé `RunAsync()` nebude volán znovu, pokud došlo k degradaci z primární tooSecondary hello repliky a poté vyzval back tooPrimary.
  - Pokud služba ukončí z `RunAsync()` po vyvolání výjimky neočekávané výjimce, to se považuje za selhání. objekt služby Hello se vypne a oznámila Chyba stavu.
  - Když žádný časový limit na vrácení z těchto metod, okamžitě ztratit hello možnost toowrite tooReliable kolekcí a proto nelze dokončit všechna skutečná práce. Doporučuje se vrátíte co nejrychleji po přijetí hello žádost o zrušení. Pokud vaše služba neodpovídá volání toothese rozhraní API v přiměřené době Service Fabric může vynucené ukončení služby. Obvykle tato situace nastane pouze během upgradu aplikace nebo když se odstraňuje služby. Tento časový limit je 15 minut ve výchozím nastavení.
  - Selhání v hello `OnCloseAsync()` cesta povede `OnAbort()` volané, což je možnost best effort poslední chance pro hello služby tooclean nahoru a uvolnění prostředků, které budou požadovaly.

## <a name="next-steps"></a>Další kroky
- [Úvod tooReliable služby](service-fabric-reliable-services-introduction.md)
- [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
- [Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)
