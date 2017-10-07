---
title: "Přehled služby Analysis Service aaaFault | Microsoft Docs"
description: "Tento článek popisuje hello selhání službu Analysis Services v Service Fabric vyvolat chyb a spuštěným scénáře testování pro vaše služby."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: deac16ec830aa10d4e488e60691faa9ef2b6cd33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-fault-analysis-service"></a>Úvod toohello selhání Analysis Service
Hello selhání Analysis Service je určená pro testování služby, které jsou postaveny na Microsoft Azure Service Fabric. S hello selhání Analysis Service můžete vyvolat smysluplný chyb a spouštění scénáře dokončení testování vaší aplikace. Tyto scénáře a chyb postupu a ověření hello množství stavy a přechody, které služba bude na základě zkušeností uživatelů v průběhu své životnosti, všechny řízené, bezpečné a konzistentním způsobem.

Akce jsou jednotlivé chyb hello cílení na služby pro testování ho. Vývojář služby můžete použít jako stavební bloky scénáře toowrite komplikovanější. Například:

* Restartujte uzel toosimulate libovolný počet situace, kdy počítač nebo virtuální počítač restartovat.
* Přesuňte repliku vaší stavové služby toosimulate Vyrovnávání zatížení a převzetí služeb při selhání nebo upgradu aplikace.
* Vyvolání ztrátě kvora na stavové služby toocreate situaci, kdy operace zápisu nemůže pokračovat, protože nejsou k dispozici dostatek repliky "zálohování" nebo "sekundární" tooaccept nová data.
* Vyvolání ztrátě dat na stavové služby toocreate situaci, kde všechny stavy v paměti je zcela k vymazání.

Scénáře jsou komplexní operace skládá z jedné nebo více akcí. Hello selhání Analysis Service obsahuje dva předdefinované kompletní scénáře:

* Scénář Chaos
* Scénář převzetí služeb při selhání

## <a name="testing-as-a-service"></a>Testování jako služby
Hello selhání Analysis Service je systémová služba Service Fabric, která se automaticky spustí s cluster Service Fabric. Tato služba slouží jako hostitel hello pravděpodobnost vkládání, spuštění testu scénáři a analýz stavu. 

![Služba analýza selhání][0]

Při selhání akce nebo testovací scénář se zahájí, je odeslán příkaz toohello selhání Analysis Service toorun hello selhání akce nebo testovací scénář. Hello selhání Analysis Service je stavová, aby mohli spolehlivě spuštění chyb a scénáře a ověření výsledky. Například scénář testovací dlouho běžící může spolehlivě provádět hello selhání Analysis Service. A protože testy se spouštějí v clusteru hello, hello služby můžete zkontrolovat stav hello hello clusteru a vaší služby tooprovide podrobnější informace o selhání.

## <a name="testing-distributed-systems"></a>Testování distribuovaných systémů
Service Fabric díky hello úloha zápis a správu distribuovaných aplikací škálovatelný výrazně jednodušší. Hello selhání Analysis Service tomu bude testování podobně jednodušší distribuované aplikace. Existují tři hlavní problémy, které je třeba toobe vyřešeny při testování:

1. Simulaci nebo generování chyb, které můžou nastat ve scénářích reálného světa: jeden z hello důležité aspekty Service Fabric je, že umožňuje toorecover distribuované aplikace při různých selháních. Ale tootest, který hello aplikace je možné toorecover z těchto chyb, potřebujeme mechanismus toosimulate nebo generování reálného selhání v řízené testovacím prostředí.
2. možnost toogenerate Hello korelační selhání: základní selhání v systému hello, například selhání sítě a selhání počítače jsou snadno tooproduce jednotlivě. Generování velký počet scénáře, které může dojít v reálném světě hello v důsledku hello interakce jednotlivých selhání je netriviální.
3. Jednotné rozhraní mezi různé úrovně vývoj a nasazení: existuje mnoho selhání vkládání systémy, které můžete provést různých typů chyb. Hello prostředí ve všech těchto je však nízký, při přesouvání z jedné pole Vývojářské scénáře, hello toorunning stejné testy v prostředích s velkými testovacími toousing je testuje v produkčním prostředí.

Existuje mnoho toosolve mechanismy těchto problémů, systém, který stejné hello s požadované záruky – všechny způsob hello z prostředí vývojáře jeden pole tootest v produkčních clusterů – chybí. Hello selhání Analysis Service pomáhá vývojářům aplikací hello soustředit se jenom na své obchodní logiky testování. Hello selhání Analysis Service poskytuje, že všechny funkce hello tootest hello interakce služby hello základní distribuované systémem hello.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Simulaci generování scénářích reálného selhání
robustnost hello tootest distribuovaného systému proti selhání, potřebujeme mechanismus toogenerate selhání. Zatímco teoreticky generování selhání jako vypnutý uzel zdá se, že snadno, že začne stiskne hello stejná sada konzistence problémy, Service Fabric se pokouší toosolve. Například pokud chceme tooshut dolů uzlu, hello požadovaný pracovního postupu je hello následující:

1. Z klienta hello vydejte žádost o vypnutí uzlu.
2. Odešlete hello požadavek toohello správný uzel.
   
    a. Pokud není nalezen hello uzlu, má selhat.
   
    b. Pokud je nalezen uzel hello, má být vrácen pouze pokud je vypnutý uzel hello.

selhání hello tooverify z perspektivy testu, hello test musí tooknow, když je toto selhání vyvolané, selhání hello ve skutečnosti se stane. Hello záruka, která poskytuje služby infrastruktury je, že buď uzel hello bude přejděte nebo již byla dolů po dosažení hello příkaz hello uzlu. V obou případu hello test by měl být schopný toocorrectly důvod o stavu hello a úspěch nebo neúspěch správně v jeho ověření. Systém implementována mimo hello toodo Service Fabric stejná sada selhání může časový mnoho sítě, hardwaru a softwaru problémy, které by jinak znemožňovaly v poskytování hello předchozí záruky. V hello přítomnost hello problémy uvádí před Service Fabric překonfigurovat hello clusteru stavu toowork hello problémů a proto hello selhání Analysis Service bude stále možné toogive hello správné nastavit záruk.

### <a name="generating-required-events-and-scenarios"></a>Generování požadované události a scénáře
Při simulaci selhání reálného konzistentně robustním toostart s, je i tougher hello možnost toogenerate korelační selhání. Například ke ztrátě dat probíhá stavové služby trvalou při dojít hello následující věci:

1. Pouze zápisu kvora hello repliky jsou zachycena na replikaci. Všechny sekundární repliky hello funkce lag za hello primární.
2. Hello zápisu kvora ocitne mimo provoz z důvodu hello repliky směrem dolů (z důvodu balíček kódu tooa nebo uzel směrem dolů).
3. Hello zápisu kvora nelze vyvolat, zpět protože hello data pro repliky hello dojde ke ztrátě (z důvodu toodisk poškození nebo obnovování počítače).

Korelační selhání dojít hello reálném světě, ale ne tak často jako jednotlivé selhání. Hello možnost tootest pro tyto scénáře před dějí v produkčním prostředí je velmi důležité. I důležitější je hello možnost toosimulate tyto scénáře s produkčním prostředí v řízené případech (uprostřed hello hello den s všechny techniky podlaží). To je mnohem lepší, než s jeho nastat hello poprvé v produkčním prostředí ve 2:00

### <a name="unified-experience-across-different-environments"></a>Jednotném rozhraní různých prostředích
Postup Hello tradičně byl toocreate tři různé sady prostředí, jeden pro hello vývojového prostředí, jednu pro testy a jeden pro produkční prostředí. Hello model byl:

1. Ve vývojovém prostředí hello vytvořit přechodů mezi stavy, které umožňují testování částí jednotlivých metod.
2. V testovacím prostředí hello vytvořit selhání tooallow začátku do konce testy, které vykonává různé scénáře selhání.
3. Zachovat hello produkční prostředí pristine tooprevent případné selhání bez přirozený a tooensure, že je velmi rychlý lidského odpovědi toofailure.

V Service Fabric, prostřednictvím hello selhání Analysis Service, jsme navrhované tooturn tento řádek a použití hello stejné metodika z prostředí tooproduction developer. Existují dva způsoby tooachieve toto:

1. selhání tooinduce řídí, použijte hello selhání Analysis Service API z prostředí jeden pole všechny tooproduction způsob hello clustery.
2. toogive hello clusteru prasat, který způsobuje, že automatické indukční chyb, použijte hello selhání Analysis Service toogenerate automatické selhání. Řízení hello počet selhání prostřednictvím konfigurace umožňuje hello stejné služby toobe testováno jinak v různých prostředích.

Pomocí Service Fabric i když hello škále selhání se liší v různých prostředích hello, skutečný mechanismy hello by identické. To umožňuje mnohem rychlejší nasazení kódu kanálu hello možnost tootest hello služby a v části reálného zatížením.

## <a name="using-hello-fault-analysis-service"></a>Pomocí hello selhání Analysis Service
**C#**

Funkce Analysis Service odolnosti jsou v oboru názvů System.Fabric hello v hello balíček Microsoft.ServiceFabric NuGet. Funkce Analysis Service selhání hello toouse, zahrnují balíček nuget hello jako odkaz ve vašem projektu.

**PowerShell**

toouse prostředí PowerShell, je nutné nainstalovat hello Service Fabric SDK. Po hello SDK je nainstalována, hello ServiceFabric PowerShell modul je načten pro toouse jste auto.

## <a name="next-steps"></a>Další kroky
toocreate skutečně cloudové služby, je důležité tooensure před a po nasazení, že služby můžete odolat selhání skutečném světě. V služby Vítáme dnes hello tooinnovate možnost rychle a přesunutí kódu tooproduction rychle je velmi důležité. Hello selhání Analysis Service pomáhá vývojářům toodo služby přesněji který.

Zahájit testování vaší aplikace a služby pomocí integrovaných hello [scénáře otestovat](service-fabric-testability-scenarios.md), nebo vytvořit vlastní scénáře testování pomocí hello [poruch akce](service-fabric-testability-actions.md) poskytované hello selhání Analysis Service.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
