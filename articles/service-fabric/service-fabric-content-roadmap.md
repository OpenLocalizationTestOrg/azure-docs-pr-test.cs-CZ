---
title: "Další informace o Azure Service Fabric aaaLearn | Microsoft Docs"
description: "Další informace o hello základní koncepty a hlavními oblastmi Azure Service Fabric. Obsahuje přehled rozšířené Service Fabric a jak toocreate mikroslužeb."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/14/2017
ms.author: ryanwi
ms.openlocfilehash: 7fe8de777755be11635912613bb5b970e3fe3ea3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="so-you-want-toolearn-about-service-fabric"></a>Proto má toolearn o Service Fabric?
Azure Service Fabric je platforma distribuovaných systémů, která umožňuje snadno toopackage, nasadit a spravovat škálovatelného a spolehlivého mikroslužeb.  Service Fabric má rozlehlých, ale a je mnohem toolearn.  Tento článek obsahuje souhrn Service Fabric a popisuje hello základní koncepty programování modely, životního cyklu aplikací, testování, clustery a sledování stavu. Čtení hello [přehled](service-fabric-overview.md) a [co jsou mikroslužeb?](service-fabric-overview-microservices.md) úvod a jak Service Fabric lze použít toocreate mikroslužeb. Tento článek neobsahuje kompletní seznam obsahu, ale toooverview a získávání Začínáme články pro každou oblast Service Fabric propojení. 

## <a name="core-concepts"></a>Základní koncepty
[Terminologie Service Fabric](service-fabric-technical-overview.md), [aplikačního modelu](service-fabric-application-model.md), a [podporované programovací modely](service-fabric-choose-framework.md) poskytují další koncepty a popisy, ale zde jsou základní informace o hello.

<table><tr><th>Základní koncepty</th><th>V době návrhu</th><th>Za běhu</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Návrh čas: typ aplikace, typ služby, balíček aplikace a manifest, balíček služby a manifestu
Typ aplikace je hello název a verzi přiřazené kolekce tooa typy služeb. To je definována v *ApplicationManifest.xml* souboru, která je integrována v adresáři balíčku aplikace. Hello balíčku aplikace se pak zkopíruje cluster Service Fabric toohello úložiště bitových kopií. Potom můžete vytvořit s názvem aplikace z tohoto typu aplikace, které pak spustí v rámci clusteru hello. 

Typ služby je, že hello název/verze přiřadili kód balíčky, data balíčky a balíčky konfigurace tooa služby. To je definována v souboru ServiceManifest.xml, která je integrována v adresáři balíčku služby. Hello adresář balíčku služby se pak odkazuje balíček aplikace *ApplicationManifest.xml* souboru. V rámci clusteru hello po vytvoření pojmenované aplikace, můžete vytvořit s názvem služby z jednoho z hello typů služeb typ aplikace. Typ služby je popsán v jeho *ServiceManifest.xml* souboru. Typ služby Hello se skládá z nastavení konfigurace služby spustitelného kódu, které se načtou za běhu, a statických dat, která se využívá služba hello.

![Typy aplikací Service Fabric a typů služeb][cluster-imagestore-apptypes]

Hello balíčku aplikace je disk adresář obsahující typ aplikace hello *ApplicationManifest.xml* souboru, který odkazuje na balíčky hello služeb pro každý typ služby, která vytváří typ aplikace hello. Balíček aplikace pro typ e-mailové aplikace může například obsahovat odkazy tooa fronty služby balíček, front-endové služby balíčku a balíček služby databáze. Hello soubory v adresáři balíčku aplikace hello se cluster Service Fabric zkopírovaný toohello úložiště bitových kopií. 

Balíček služby je adresář disk obsahující typ služby hello *ServiceManifest.xml* souboru, který odkazuje na kód hello, statických dat a balíčky konfigurace pro typ služby hello. Hello soubory v adresáři balíčku služby hello odkazuje typ aplikace hello *ApplicationManifest.xml* souboru. Balíček služby může například odkazovat toohello kód, statických dat a konfigurace balíčky, které tvoří databáze služby.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Čas spuštění: clustery a uzly, s názvem aplikace, s názvem služby, oddíly a repliky
[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby. Clustery můžete škálovat toothousands počítačů.

Počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzel. Každý uzel je přiřazen název uzlu (řetězec). Uzly mají charakteristiky jako vlastnosti umístění. Každý počítač nebo virtuální počítač má služby systému Windows automaticky spouštěná `FabricHost.exe`, který spuštění při spuštění a pak spustí dvě spustitelné soubory: `Fabric.exe` a `FabricGateway.exe`. Tyto dvě spustitelné soubory tvoří hello uzlu. Pro vývoj nebo testování scénáře, může hostovat více uzlů na jeden počítač nebo virtuální počítač spuštěním několika instancí `Fabric.exe` a `FabricGateway.exe`.

Pojmenované aplikace je kolekce s názvem služby, které provádí některé funkce nebo funkce. Služba provede kompletní a samostatné funkce (ho můžete spustit a spustit nezávisle na jiných služeb) a se skládá z kódu, konfigurace a data. Úložiště bitových kopií zkopírovaný toohello po balíček aplikace vytvoříte instanci hello aplikací v rámci clusteru hello zadáním typ balíčku aplikace hello aplikace (pomocí jeho název a verzi). Každá instance typu aplikace je přiřazen název identifikátor URI, který vypadá jako *fabric: / MyNamedApp*. V rámci clusteru můžete vytvořit více aplikací s názvem z typu jednu aplikaci. Můžete také vytvořit s názvem aplikace z typů jinou aplikaci. Každé pojmenované aplikace je spravovaná a verzí nezávisle.

Po vytvoření pojmenované aplikace, můžete vytvořit instanci jednoho z jeho typů služeb (s názvem service) v rámci clusteru hello zadáním typu služby hello (pomocí jeho název a verzi). Každá instance typu služby je přiřazen název identifikátor URI oboru v identifikátoru URI s názvem aplikace. Například pokud vytvoříte "Databáze" s názvem služby v rámci "MyNamedApp" s názvem aplikace, hello URI bude vypadat takto: *fabric: / MyNamedApp/databáze*. V rámci pojmenované aplikace můžete vytvořit jednu nebo více služeb s názvem. Počty instancí nebo repliky a každé pojmenované služby může mít svůj vlastní schéma oddílu. 

Existují dva typy služeb: bezstavové a stavové. Bezstavové služby můžete uložit trvalý stav služby externího úložiště, jako je například Azure Storage, Azure SQL Database nebo Azure Cosmos DB. Bezstavové služby použijte, když má služba hello vůbec žádné trvalé úložiště. Stavové služby používá Service Fabric toomanage stav vaší služby prostřednictvím jeho spolehlivé kolekce nebo Reliable Actors programovací modely. 

Při vytváření pojmenovaného služby, zadejte schéma oddílu. Služby s velkým množstvím stavu rozdělit hello data do oddílů. Každý oddíl je zodpovědná za část stavu dokončení hello hello služby, který je rozdělena mezi uzly clusteru hello. V rámci oddílu mají bezstavové služby s názvem instance, zatímco stavové služby s názvem mít repliky. Bezstavové služby s názvem mají obvykle, vždy jen jeden oddíl, protože mají žádný vnitřní stav. Stateful s názvem služby udržovat, že jejich stavu v rámci repliky a každý oddíl má svou vlastní sady replik. Operace čtení a zápisu jsou prováděny v jedné replice (nazývané hello primární). Změny toostate z možnosti zápis, které operace jsou replikovány toomultiple další repliky (označovaný jako aktivní sekundární databáze). 

Hello následující diagram znázorňuje hello vztah mezi aplikací a instance služby, oddíly a repliky.

![Oddíly a repliky v rámci služby][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Rozdělení do oddílů, škálování a dostupnosti
[Vytváření oddílů](service-fabric-concepts-partitioning.md) není jedinečný tooService prostředků infrastruktury. Dobře známé formulář oddíly je vytvoření oddílů dat nebo horizontálního dělení. Stavové služby s velkým množstvím stavu rozdělit hello data do oddílů. Každý oddíl je zodpovědná za část hello dokončení stavu služby hello. 

Hello repliky každý oddíl se rozdělují mezi uzly clusteru hello, což umožňuje stavu s názvem služby příliš[škálování](service-fabric-concepts-scalability.md). Jako hello data potřebuje růst, oddíly růst a Service Fabric oddíly znovu vytvoří rovnováhu mezi uzly toomake efektivní využití hardwarové prostředky. Pokud chcete přidat nový cluster toohello uzly, Service Fabric znovu vyvážit repliky oddílu hello napříč hello zvýšit počet uzlů. Celkové zlepšuje výkon aplikace a snižuje kolize pro toomemory přístup. Pokud hello uzly v clusteru hello nejsou používány efektivně, může snížit hello počet uzlů v clusteru hello. Service Fabric opakujte znovu vytvoří, rovnováhu repliky oddílu hello napříč hello zmenšit počet uzlů toomake lepší využití hello hardwaru na každém uzlu.

V rámci oddílu mají bezstavové služby s názvem instance, zatímco stavové služby s názvem mít repliky. Bezstavové služby s názvem mají obvykle, vždy jen jeden oddíl, protože mají žádný vnitřní stav. instance oddílu Hello přinášejí [dostupnosti](service-fabric-availability-services.md). Pokud se jedna instance nezdaří, ostatní instance normálně pokračovat toooperate a Service Fabric vytvoří novou instanci. Stateful s názvem služby udržovat, že jejich stavu v rámci repliky a každý oddíl má svou vlastní sady replik. Operace čtení a zápisu jsou prováděny v jedné replice (nazývané hello primární). Změny toostate z možnosti zápis, které operace jsou replikovány toomultiple další repliky (označovaný jako aktivní sekundární databáze). Má replika selhat, Service Fabric vytvoří novou repliku z existující repliky hello.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Bezstavové a stavové mikroslužeb pro Service Fabric
Service Fabric umožňuje toobuild aplikace, které se skládají z mikroslužeb nebo kontejnerů. Bezstavové mikroslužeb (například protokol brány a webové proxy servery) neudržují měnitelný stav mimo požadavek a odpověď ze služby hello. Role pracovního procesu Azure Cloud Services jsou příkladem bezstavové služby. Stavová mikroslužeb (například uživatelské účty, databáze, zařízení, nákupní košíků a fronty) udržovat měnitelný, autoritativní stavu nad rámec hello požadavku a odpovědi. Dnešní internetových aplikací se skládají z kombinace bezstavové a stavové mikroslužeb. 

Klíče differentation s Service Fabric je silné zaměřuje na budování stavové služby, buď pomocí hello [integrovaný programovací modely ](service-fabric-choose-framework.md) nebo s kontejnerizované stavové služby. Hello [scénáře aplikací](service-fabric-application-scenarios.md) popisují hello scénáře, kdy se používá stavové služby.

Proč máte stavová mikroslužeb společně s bezstavové ty? Hello dva hlavní důvody jsou:

* Můžete vytvořit vysoce propustnost, nízkou latencí, proti chybám online zpracování transakcí (OLTP) služby kódem udržuje a hello dat zavřít na stejném počítači. Některé příklady jsou interaktivní obchodní poutače, hledání, systémy Internet věcí (IoT), obchodní systémy, platební karty zpracování a podvod detekce systémů a správu osobní záznamu.
* Návrh aplikace můžete zjednodušit. Stavová mikroslužeb odebrat hello potřebu další fronty a mezipaměti, které jsou tradičně nezbytné požadavky dostupnosti a latence hello tooaddress čistě bezstavové aplikace. Stavové služby jsou přirozeně vysokou dostupnost a nízkou latencí, což snižuje počet hello přesunutí toomanage částí v aplikaci jako celek.

## <a name="supported-programming-models"></a>Podporované programovací modely
Service Fabric nabízí několik způsobů toowrite a spravovat vaše služby. Služby můžou používat hello rozhraní API pro Service Fabric tootake všech výhod funkcí hello platformy a architektur aplikací. Služby mohou být také všechny kompilované spustitelný program napsané v libovolném jazyce a hostovaná v clusteru Service Fabric. Další informace najdete v tématu [podporované programovací modely](service-fabric-choose-framework.md).

### <a name="containers"></a>Kontejnery
Ve výchozím nastavení Service Fabric nasadí a aktivuje služby jako procesy. Service Fabric lze také nasadit služby v [kontejnery](service-fabric-containers-overview.md). Je důležité, je možné kombinovat služby v procesy a služby v kontejnerech v hello stejná aplikace. Service Fabric podporuje nasazení kontejnerů Linux kontejnery Windows na Windows Server 2016. Můžete nasadit existující aplikace, bezstavové služby nebo stavové služby v kontejnerech. 

### <a name="reliable-services"></a>Reliable Services
[Spolehlivé služby](service-fabric-reliable-services-introduction.md) je jednoduché rozhraní pro zápis služby, které integrovat platformy Service Fabric hello a využívat hello úplnou sadu funkcí platformy. Spolehlivé služby může být bezstavové (podobně jako toomost služby platformy, jako jsou webové servery nebo rolí pracovního procesu ve službě Azure Cloud Services), kde je uložen stav externí řešení, například Azure DB nebo úložiště tabulek Azure. Spolehlivé služby může být také stateful, kde je uložen stav přímo v samotné pomocí spolehlivé kolekcí služby hello. Stav se provádí [vysokou dostupností](service-fabric-availability-services.md) prostřednictvím replikace a distribuované prostřednictvím [dělení](service-fabric-concepts-partitioning.md), všechny spravované automaticky pomocí Service Fabric.

### <a name="reliable-actors"></a>Reliable Actors
Spolehlivé služby nástavbou, hello [spolehlivé objektu Actor](service-fabric-reliable-actors-introduction.md) framework je aplikační rozhraní, který implementuje vzor virtuální objektu Actor hello, na základě vzoru návrhu objektu actor hello. framework spolehlivé objektu Actor Hello používá nezávislé jednotky výpočetních operací a stavu s jedním podprocesem provádění názvem aktéři. Hello spolehlivé objektu Actor framework poskytuje součástí komunikace pro aktéři trvalost předem nastavené stavu a konfigurace Škálováním na více systémů.

### <a name="aspnet-core"></a>Jádro ASP.NET
Service Fabric se integruje s [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) jako první třídy programovací model pro vytváření webových a aplikací API

### <a name="guest-executables"></a>Spustitelné soubory hosta
A [spustitelný soubor hosta](service-fabric-deploy-existing-app.md) je existující, libovolného spustitelného souboru (napsané v libovolném jazyce) hostovaná v clusteru Service Fabric souběžně s jinými službami. Spustitelné soubory hosta není integrovat přímo s rozhraními API služby prostředků infrastruktury. Ale budou přesto využívat výhod funkce hello nabízí platformy, jako je například vlastní stavu a spouštění sestav a volání rozhraní REST API služby možnosti rozpoznání. Mají také celou aplikaci životní cyklus podpory. 

## <a name="application-lifecycle"></a>Životní cyklus aplikace
Jako s jinými platformami, v Service Fabric aplikace obvykle projde hello následující fáze: návrh, vývoj, testování, nasazení, upgrade, údržbu a odebírání. Service Fabric nabízí prvotřídní podporu pro životní cyklus úplné aplikace hello cloudových aplikací, od vývoje přes nasazení, každodenní správu a údržbu tooeventual vyřazení z provozu. model služby Hello umožňuje několika tooparticipate různé role nezávisle v životním cyklu aplikací hello. [Životní cyklus aplikace Service Fabric](service-fabric-application-lifecycle.md) poskytuje přehled hello rozhraní API a jak se používají v hello různé role v rámci hello fáze životního cyklu aplikace Service Fabric hello. 

životní cyklus Hello celé aplikace můžete spravovat pomocí [rutiny prostředí PowerShell](/powershell/module/ServiceFabric/), [rozhraní API jazyka C#](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [rozhraní API Java](/java/api/system.fabric._application_management_client), a [rozhraní REST API](/rest/api/servicefabric/). Můžete také nastavit nepřetržité integrace/průběžné kanály nasazení pomocí nástrojů, jako [Visual Studio Team Services](service-fabric-set-up-continuous-integration.md) nebo [volaných](service-fabric-cicd-your-linux-java-application-with-jenkins.md).

Popisuje, Hello následující video Microsoft Virtual Academy jak toomanage životním cyklu aplikací:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-applications-and-services"></a>Testování aplikací a služeb
toocreate skutečně cloudové služby, je důležité tooverify vaší aplikace a služby se může odolat selhání reálného. Hello selhání Analysis Service je určená pro testování služby, které jsou postaveny na Service Fabric. S hello [selhání Analysis Service](service-fabric-testability-overview.md), můžete vyvolat smysluplný chyb a spouštění scénáře dokončení testování vaší aplikace. Tyto scénáře a chyb postupu a ověření hello množství stavy a přechody, které služba bude na základě zkušeností uživatelů v průběhu své životnosti, všechny řízené, bezpečné a konzistentním způsobem.

[Akce](service-fabric-testability-actions.md) cíle služby pro testování pomocí jednotlivých chyb. Vývojář služby můžete použít jako stavební bloky scénáře toowrite komplikovanější. Příklady simulované chyb:

* Restartujte uzel toosimulate libovolný počet situace, kdy počítač nebo virtuální počítač restartovat.
* Přesuňte repliku vaší stavové služby toosimulate Vyrovnávání zatížení a převzetí služeb při selhání nebo upgradu aplikace.
* Vyvolání ztrátě kvora na stavové služby toocreate situaci, kdy operace zápisu nemůže pokračovat, protože nejsou k dispozici dostatek repliky "zálohování" nebo "sekundární" tooaccept nová data.
* Vyvolání ztrátě dat na stavové služby toocreate situaci, kde všechny stavy v paměti je zcela k vymazání.

[Scénáře](service-fabric-testability-scenarios.md) komplexních operací se skládají z jedné nebo více akcí. Hello selhání Analysis Service obsahuje dva předdefinované kompletní scénáře:

* [Scénář Chaos](service-fabric-controlled-chaos.md)-simuluje průběžné, prokládaná chyb (řádně i vynuceném) v rámci clusteru hello přes dlouhou dobu.
* [Scénář převzetí služeb při selhání](service-fabric-testability-scenarios.md#failover-test)-verzi hello chaos testovací scénář, který cílí oddíl konkrétní službu a nechat jiných služeb neovlivní.

## <a name="clusters"></a>Clustery
[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby. Clustery můžete škálovat toothousands počítačů. Počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzlem clusteru. Každý uzel je přiřazen název uzlu (řetězec). Uzly mají charakteristiky jako vlastnosti umístění. Každý počítač nebo virtuální počítač se automaticky spouštěná služba, `FabricHost.exe`, který spuštění při spuštění a pak spustí dvě spustitelné soubory: Fabric.exe a FabricGateway.exe. Tyto dvě spustitelné soubory tvoří hello uzlu. Pro testování scénáře, může hostovat více uzlů na jeden počítač nebo virtuální počítač spuštěním několika instancí `Fabric.exe` a `FabricGateway.exe`.

Clusterů Service Fabric se dají vytvořit na virtuálních nebo fyzických počítačích se systémem Windows Server nebo Linux. Je možné toodeploy a spouštět aplikace Service Fabric v jakémkoli prostředí, kde máte sadu Windows Server nebo Linux počítače, které jsou vzájemně provázány: místně na Microsoft Azure nebo na všechny poskytovatele cloudových služeb.

Hello následující video Microsoft Virtual Academy popisuje clusterů Service Fabric:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Clustery v Azure
Clusterů Service Fabric systémem Azure poskytuje integraci se službou další funkce Azure a služby, takže je operace a Správa clusteru hello jednodušší a spolehlivější. Cluster je prostředek Azure Resource Manager, abyste mohli clustery podobně jako všechny ostatní prostředky v Azure. Resource Manager také poskytuje snadnou správu všechny prostředky používané hello clusteru jako na jednu jednotku. Clustery v Azure jsou integrované s Azure diagnostiku a analýzy protokolů. Typy uzlů clusteru jsou [sady škálování virtuálního počítače](/azure/virtual-machine-scale-sets/index), takže je součástí funkce automatického škálování.

Můžete vytvořit cluster v Azure prostřednictvím hello [portál Azure](service-fabric-cluster-creation-via-portal.md), z [šablony](service-fabric-cluster-creation-via-arm.md), nebo z [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

verze preview Hello Service Fabric v systému Linux vám umožní toobuild, nasadit a spravovat vysoce dostupné a vysoce škálovatelné aplikace v systému Linux, stejně jako v systému Windows. jsou k dispozici v jazyce Java v systému Linux, v přidání tooC # (.NET Core) Hello Service Fabric rozhraní (Reliable Services a Reliable Actors). Můžete také vytvořit [spustitelný soubor služby hosta](service-fabric-deploy-existing-app.md) s libovolný jazyk nebo rozhraní. Kromě toho hello preview podporuje také Orchestrace Docker kontejnery. Kontejnery docker lze spouštět spustitelné soubory hosta nebo nativní služby, Service Fabric, které používají hello Service Fabric rozhraní. Další informace najdete v tématu [Service Fabric v systému Linux](service-fabric-linux-overview.md).

Protože Service Fabric v Linuxu je ve verzi Preview, existují určité funkce, které jsou ve Windows podporované, ale v Linuxu ne. toolearn víc, přečtěte si [rozdíly mezi Service Fabric v systému Linux a Windows](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Samostatné clustery
Service Fabric nabízí instalační balíček pro jste toocreate samostatné Service Fabric clustery místně nebo na všechny poskytovatele cloudových služeb. Samostatné clustery udělení hello volnost toohost cluster požadovaná místa. Pokud vaše data jsou toocompliance předmětu nebo regulačních omezení, nebo chcete tookeep vaše místní data, můžete hostovat vlastní cluster a aplikace. Aplikace Service Fabric můžete spustit v prostředí s více hostitelských beze změn, vaše znalosti o vytváření aplikací se přenesou z jednoho tooanother hostování prostředí. 

[Vytvoření vaší první samostatný cluster Service Fabric](service-fabric-get-started-standalone-cluster.md)

Linux samostatné clustery se ještě nepodporují.

### <a name="cluster-security"></a>Zabezpečení clusteru
Clustery musí být zabezpečené tooprevent neoprávněného uživatele z připojení clusteru tooyour, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné toocreate zabezpečená clusteru, tak umožňuje anonymní uživatelé tooconnect tooit Pokud koncové body správy jsou zveřejněné toohello veřejného Internetu. Není možné toolater povolit zabezpečení na zabezpečená clusteru: při vytváření clusteru je povoleno zabezpečení clusteru.

scénáře zabezpečení clusteru Hello jsou:
* Zabezpečení – uzly
* Uzel Klient zabezpečení
* Řízení přístupu na základě role (RBAC)

Další informace najdete v tématu [zabezpečení clusteru](service-fabric-cluster-security.md).

### <a name="scaling"></a>Škálování
Pokud chcete přidat nový cluster toohello uzly, Service Fabric znovu vytvoří rovnováhu repliky oddílu hello a instance napříč hello zvýšit počet uzlů. Celkové zlepšuje výkon aplikace a snižuje kolize pro toomemory přístup. Pokud hello uzly v clusteru hello nejsou používány efektivně, může snížit hello počet uzlů v clusteru hello. Service Fabric znovu vytvoří znovu rovnováhu replik hello oddílů a lepší využití instance napříč hello zmenšit počet uzlů toomake hello hardwaru na každém uzlu. Clustery v Azure je možné škálovat buď [ručně](service-fabric-cluster-scale-up-down.md) nebo [prostřednictvím kódu programu](service-fabric-cluster-programmatic-scaling.md). Je možné rozšířit samostatnou clustery [ručně](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Upgrade clusteru
Pravidelně jsou vydávány nové verze modulu runtime Service Fabric hello. Provést upgrady modulu runtime, nebo z prostředků infrastruktury, v clusteru tak, aby vždy běží [podporovaná verze](service-fabric-support.md). Kromě toho toofabric upgrady, můžete také aktualizovat konfiguraci clusteru, jako je například certifikáty nebo porty aplikace.

Cluster Service Fabric je na prostředek, který vlastníte, ale je částečně spravováno společností Microsoft. Microsoft je zodpovědná za oprav hello základní operační systém a provádění upgrady prostředků infrastruktury v clusteru. Můžete nastavit architektury clusteru tooreceive automatické upgrady, když společnost Microsoft vydá nové verze, nebo zvolte tooselect verze podporovaných fabric, který chcete. Upgrade infrastruktury a konfigurace lze nastavit prostřednictvím hello portál Azure nebo prostřednictvím Resource Manager. Další informace najdete v tématu [Upgrade clusteru Service Fabric](service-fabric-cluster-upgrade.md). 

Cluster s podporou samostatné je prostředek které je zcela vlastní. Jste zodpovědní za oprav hello základní operační systém a inicializaci upgrady prostředků infrastruktury. Pokud váš cluster můžete připojit příliš[https://www.microsoft.com/download](https://www.microsoft.com/download), můžete nastavit stahování tooautomatically clusteru a zřídit hello nové Service Fabric balíček modulu runtime. By potom zahájit hello upgrade. Pokud váš cluster nemůže získat přístup k [https://www.microsoft.com/download](https://www.microsoft.com/download), můžete ručně stáhnout nový balíček modulu runtime hello z počítače připojené Internetu a potom zahájit hello upgrade. Další informace najdete v tématu [Upgrade clusteru Service Fabric samostatné](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Monitorování stavu
Zavádí Service Fabric [stavu modelu](service-fabric-health-introduction.md) určené tooflag není v pořádku, cluster a aplikace podmínek na konkrétní entity (například uzly clusteru a repliky služby). model stavu Hello používá stavu reporters (součásti systému a watchdogs). cílem Hello je rychlé a snadné diagnostiky a opravy. Služba zapisovače potřebovat toothink předem o stavu a jak příliš[návrhu, vytváření sestav stavu](service-fabric-report-health.md#design-health-reporting). Všechny podmínku, která může mít vliv na stav by měl být zaznamenány na, zejména v případě, že může pomoci zavřete toohello kořenové problémy příznak. informace o stavu Hello můžete ušetřit čas a úsilí na ladění a šetření, jakmile je služba hello a běží ve velkém měřítku v produkčním prostředí.

monitorování reporters Service Fabric Hello identifikovat podmínky, které vás zajímají. Sestavy budou tyto podmínky založené na jejich místní zobrazení. Hello [úložiště stavu](service-fabric-health-introduction.md#health-store) agreguje data o stavu odeslaných všechny toodetermine reporters jestli entity jsou globální v pořádku. Hello model je určený toobe bohaté, flexibilní a snadno toouse. Hello kvalitu sestav stavu hello Určuje přesnost hello zobrazení stavu hello hello clusteru. Falešně pozitivních zjištění, které se nesprávně zobrazí není v pořádku problémy může mít negativní vliv na upgrady nebo jiné služby, které používají data o stavu. Příkladem takové služby jsou opravy služeb a mechanismy pro výstrahy. Proto některé myšlenku je potřebné tooprovide sestavy, které zaznamenat podmínky zájem o hello co nejlepší možný způsob.

Vytváření sestav se dá udělat z:
* Hello sledovat Service Fabric service replik nebo instancí.
* Interní watchdogs nasazené jako služba Service Fabric (například Service Fabric bezstavové služba, která monitoruje podmínky a vydá sestavy). Hello watchdogs lze nasadit na všech uzlech, nebo může být spřaženém toohello monitorované služby.
* Interní watchdogs, které spouštět na uzlech hello Service Fabric, ale nejsou implementované jako služby Service Fabric.
* Externí watchdogs prostředku hello testu z mimo cluster Service Fabric hello (například monitorování služby jako Gomez).

Předinstalované hello komponenty Service Fabric sestavy stavu všech entit v clusteru hello. [Sestav o stavu systému](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) poskytují přehled o clusteru a aplikaci funkce a příznak problémy prostřednictvím stavu. Pro aplikace a služby ověřte sestav o stavu systému entity jsou implementované a chovají správně z hlediska hello modulu runtime Service Fabric hello. sestavy Hello nezadávejte zadejte jakékoli sledování stavu hello obchodní logiky hello služby nebo zjistit "zamrzlých" procesů. tooadd informace o konkrétní tooyour služby health logiku [implementovat vlastní stavu reporting](service-fabric-report-health.md) v službě.

Service Fabric nabízí několik způsobů příliš[zobrazit sestavy stavu](service-fabric-view-entities-aggregated-health.md) agregován v hello health store:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) nebo jiných nástrojů vizualizace.
* Dotazy na stav (prostřednictvím [prostředí PowerShell](/powershell/module/ServiceFabric/), hello [FabricClient rozhraní API jazyka C#](/api/system.fabric.fabricclient.healthclient) a [rozhraní API Java FabricClient](/java/api/system.fabric._health_client), nebo [rozhraní REST API](/rest/api/servicefabric)).
* Obecné dotazy, které vrací seznam entit, které mají stav jako jedna z vlastností hello (pomocí prostředí PowerShell, rozhraní API hello nebo REST).

Hello následující Microsoft Virtual Academy video popisuje hello Service Fabric stavu modelu a jeho použití:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak toocreate [clusteru v Azure](service-fabric-cluster-creation-via-portal.md) nebo [samostatné clusteru v systému Windows](service-fabric-cluster-creation-for-windows-server.md).
* Zkuste vytvořit službu pomocí hello [spolehlivé služby](service-fabric-reliable-services-quick-start.md) nebo [Reliable Actors](service-fabric-reliable-actors-get-started.md) programovací modely.
* Zjistěte, jak příliš[migrovat z cloudové služby](service-fabric-cloud-services-migration-differences.md).
* Další informace příliš[monitorování a Diagnostika služby](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Další informace příliš[testování vaší aplikace a služby](service-fabric-testability-overview.md).
* Další informace příliš[spravovat a orchestraci prostředky clusteru](service-fabric-cluster-resource-manager-introduction.md).
* Projděte hello [Service Fabric ukázky](http://aka.ms/servicefabricsamples).
* Další informace o [možnosti podpory Service Fabric](service-fabric-support.md).
* Čtení hello [blogu týmu](https://blogs.msdn.microsoft.com/azureservicefabric/) pro články a oznámení.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
