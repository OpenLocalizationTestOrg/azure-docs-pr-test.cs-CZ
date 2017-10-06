---
title: "Testovatelnosti: Komunikace služby | Microsoft Docs"
description: "Komunikace služby služby je důležité integrační bod aplikace Service Fabric. Tento článek popisuje aspekty návrhu a testování techniky."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 4a8f941c1e8e641384a9ee3a1149dabaaf9983cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric testovatelnosti scénáře: komunikace služby
Prostor orientované na služby architektury styly přirozeně v Azure Service Fabric a Mikroslužeb. V těchto typech distribuované architektury se komponentizované mikroslužbu aplikace obvykle skládají z více služeb, které je třeba tootalk tooeach jiné. V nejjednodušší případech i hello obecně mít alespoň bezstavové webové služby a služba úložiště stavová data, která potřebují toocommunicate.

Komunikace Service-to-service je důležité integrační bod aplikace, protože každá služba zpřístupní vzdálené tooother rozhraní API. Práce s sadu rozhraní API hranice, která zahrnuje vstupně-výstupních operací obvykle vyžaduje některé pozor, s dobrou množstvím testování a ověření.

Pokud tyto služby hranice představují kabelová dohromady v distribuovaného systému existují množství toomake aspekty:

* *Přenosový protokol*. Použijete pro maximální propustnost HTTP pro vyšší interoperabilitu nebo vlastní binární protokol?
* *Zpracování chyb*. Jak se budou trvalý a přechodné chyby zpracovávat? Co se stane, když služba přesune tooa jiný uzel?
* *Vypršení časových limitů a latence*. V aplikacích možných jak budou jednotlivé úrovně služby zpracovávat latence prostřednictvím hello zásobníku a toohello uživatele?

Ať už použijete jednu z poskytované Service Fabric součásti pro komunikaci hello integrovanou službu nebo můžete vytvořit vlastní, testování hello interakce mezi vaší služby je důležité tooensuring odolnosti ve vaší aplikaci.

## <a name="prepare-for-services-toomove"></a>Příprava pro toomove služby
Instance služby může pohybovat v čase. To platí hlavně když jsou nakonfigurované s vlastní přizpůsobit optimální prostředků Vyrovnávání zatížení metriky. Service Fabric přesune jejich dostupnost vaší toomaximize instancí služby i během upgradu, převzetí služeb při selhání, Škálováním na více systémů a jiné situace, ke kterým došlo v průběhu životnosti hello distribuovaného systému.

Jak služby pohyb v clusteru hello, klienty a další služby musí být připravené toohandle dva scénáře při jejich komunikovat tooa služby:

* Hello služby instance nebo oddíl repliky se přesunul od hello naposledy mluvili tooit. To je normální součást životního cyklu služby a očekávané toohappen musí být během doby života hello vaší aplikace.
* Hello služby instance nebo oddíl replika je v hello procesu přesunu. I když v Service Fabric, dojde k velmi rychle převzetí služeb při selhání služby z jednoho uzlu tooanother, může mít zpoždění v dostupnosti hello komunikace součástí služby je pomalé toostart.

Pohodlné zpracování těchto scénářů je důležité pro protokol smooth spuštění systému. toodo Ano, mějte na paměti:

* Všechny služby, která může být připojené toohas *adresu* která naslouchá na (například HTTP nebo objekty WebSockets). Pokud se přesune instance služby nebo oddíl, změní jeho adresa koncového bodu. (Ji přesune tooa jiného uzlu s jinou IP adresu.) Pokud používáte integrované komunikační součásti hello, že bude zpracovávat znovu řešení adresy služby pro vás.
* Může být dočasné zvýšení latence služby jako spuštění instance služby hello si jeho naslouchací proces znovu. To závisí na tom, jak rychle hello služby otevře naslouchací proces hello po přesunutí hello instance služby.
* Všech existujících připojení potřebovat toobe zavřít a znovu otevřít, když se otevře okno služby hello na nový uzel. Uzel řádné vypnutí nebo restartování umožňuje dobu pro existující připojení toobe korektně vypnout.

### <a name="test-it-move-service-instances"></a>Test se: přesunout instance služby
Pomocí nástroje Service Fabric testovatelnosti můžete vytvářet testovací scénáře tootest těchto situacích různými způsoby:

1. Přesuňte stavové služby primární repliky.
   
    primární repliky oddílu stavové služby Hello můžete přesunout pro libovolný počet důvodů. Pomocí této tootarget hello primární repliky konkrétní oddíl toosee, jak přesunout vaší služby reagují toohello velmi kontrolovaně.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Zastavení uzlu.
   
    Pokud uzel je zastavená, hello Service Fabric přesune všechny hello služby instance nebo oddíly, které byly na tento uzel tooone z jiných dostupných uzlů v clusteru hello. Pomocí této tootest situaci, kdy dojde ke ztrátě z clusteru s uzlem a mít toomove všechny instance služby hello a repliky na tomto uzlu.
   
    Uzel můžete zastavit pomocí prostředí PowerShell hello **Stop-ServiceFabricNode** rutiny:
   
    ```powershell
   
    PS > Restart-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Zachovat dostupnost služeb
Jako platformu Service Fabric je navrženou tooprovide vysokou dostupnost vašich služeb. Ale ve výjimečných případech může způsobit problémy s základní infrastrukturou stále nedostupnosti. Je důležité tootest pro tyto scénáře příliš.

Stavové služby používat tooreplicate stavu systému kvora pro vysokou dostupnost. To znamená, že kvorum repliky se musí operace zápisu toobe tooperform k dispozici. Ve výjimečných případech, například k poruše hardwaru rozšířeným nemusí být k dispozici kvorum repliky. V těchto případech nebudete moct tooperform operace zápisu, ale stále budete moct tooperform operace čtení.

### <a name="test-it-write-operation-unavailability"></a>Test se: nedostupnosti operace zápisu
Pomocí nástrojů testovatelnosti hello v Service Fabric, můžete vložit chybu, která indukuje ztrátě kvora jako testu. I když takové situaci je taková situace vzácná, je důležité, že služby, které jsou závislé na stavové služby a klienti jsou připraveny toohandle situacích, kde budou nelze provádět zápis tooit požadavky. Je také důležité hello stavové služby, samotné si je vědoma tuto možnost a můžete řádně sdělit toocallers.

Ztrátě kvora může být nutné pomocí prostředí PowerShell hello **Invoke-ServiceFabricPartitionQuorumLoss** rutiny:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

V tomto příkladu jsme nastavený `QuorumLossMode` příliš`QuorumReplicas` tooindicate, který chceme ztrátě kvora tooinduce bez nutnosti převádět dolů všechny repliky. Tímto způsobem operace čtení jsou stále možné. tootest scénář, kde celý oddíl není k dispozici, můžete nastavit tento přepínač také`AllReplicas`.

## <a name="next-steps"></a>Další kroky
[Další informace o akcích testovatelnosti](service-fabric-testability-actions.md)

[Další informace o scénářích testovatelnosti](service-fabric-testability-scenarios.md)

