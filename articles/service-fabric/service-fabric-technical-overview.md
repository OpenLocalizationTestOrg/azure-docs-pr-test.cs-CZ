---
title: "Další Azure Service Fabric terminologie | Microsoft Docs"
description: "Přehled terminologie Service Fabric. Popisuje klíčové terminologie konceptů a termínů používaných ve zbývající části v dokumentaci."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.openlocfilehash: 42e5e651d5a3c08e829b48bb47e14458a5c65900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="service-fabric-terminology-overview"></a>Přehled terminologie Service Fabric
Service Fabric je platforma distribuovaných systémů, která usnadňuje balíčku, nasazovat a spravovat škálovatelného a spolehlivého mikroslužeb. Toto téma podrobné informace o technologiím použitým pomocí Service Fabric pochopit termínů používaných v dokumentaci.

Koncepty uvedené v této části jsou popsány i v následující videa Microsoft Virtual Academy: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">základní koncepty</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">koncepty návrhu</a>, a <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">Run-time koncepty</a>.

## <a name="infrastructure-concepts"></a>Koncepty infrastruktury
**Cluster**: sadu virtuální nebo fyzické počítače, do kterých jsou nasazené a spravovat vaše mikroslužeb připojené k síti.  Clustery můžete škálovat tisíce počítačů.

**Uzel**: počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzel. Každý uzel je přiřazen název uzlu (řetězec). Uzly mají charakteristiky jako vlastnosti umístění. Každý počítač nebo virtuální počítač má služby systému Windows automaticky spouštěná `FabricHost.exe`, který spuštění při spuštění a pak spustí dvě spustitelné soubory: `Fabric.exe` a `FabricGateway.exe`. Tyto dvě spustitelné soubory tvoří uzlu. Pro testování scénáře, může hostovat více uzlů na jeden počítač nebo virtuální počítač spuštěním několika instancí `Fabric.exe` a `FabricGateway.exe`.

## <a name="application-concepts"></a>Koncepty aplikace
**Typ aplikace**: název a verzi, která je přiřazena ke kolekci typů služeb. Definovaná v `ApplicationManifest.xml` souboru vložených v adresáři balíčku aplikace, které se pak zkopíruje do úložiště bitové kopie cluster Service Fabric. Potom můžete vytvořit s názvem aplikace z tohoto typu aplikací v rámci clusteru.

Pro čtení [aplikačního modelu](service-fabric-application-model.md) Další informace najdete v článku.

**Balíček aplikace**: adresář disk obsahující typ aplikace `ApplicationManifest.xml` souboru. Odkazuje na balíčky služeb pro každý typ služby, která vytváří typ aplikace. Soubory v adresáři balíčku aplikace se zkopírují do úložiště bitové kopie cluster Service Fabric. Balíček aplikace pro typ e-mailové aplikace může například obsahovat odkazy na balíček služby fronty, front-endové služby balíčku a balíček služby databáze.

**Aplikace s názvem**: po balíček aplikace se zkopíruje do úložiště bitové kopie, vytvoření instance aplikace v rámci clusteru zadáním typu aplikace balíček aplikace (pomocí jeho název a verzi). Každá instance typu aplikace je přiřazen název identifikátor URI, který vypadá takto: `"fabric:/MyNamedApp"`. V rámci clusteru můžete vytvořit více aplikací s názvem z typu jednu aplikaci. Můžete také vytvořit s názvem aplikace z typů jinou aplikaci. Každé pojmenované aplikace je spravovaná a verzí nezávisle.      

**Typ služby**: název a verzi, která je přiřazená kód balíčky, data balíčky a balíčky konfigurace služby. Definovaná v `ServiceManifest.xml` souboru, které jsou součástí služby adresář balíčku a balíček adresáře služby se pak odkazuje balíček aplikace `ApplicationManifest.xml` souboru. V rámci clusteru po vytvoření pojmenované aplikace, můžete vytvořit s názvem služby z jednoho z typů služeb typ aplikace. Typ služby `ServiceManifest.xml` soubor popisuje službu.

Pro čtení [aplikačního modelu](service-fabric-application-model.md) Další informace najdete v článku.

Existují dva typy služeb:

* **Stateless:** použít bezstavové služby při trvalý stav služby je uložen v službě externího úložiště, jako je například Azure Storage, Azure SQL Database nebo Azure Cosmos DB. Bezstavové služby použijte, pokud služba používá žádné trvalé úložiště vůbec. Například kalkulačky služba, kde jsou hodnoty předány do služby, výpočet se provádí pomocí tyto hodnoty a výsledek je vrácen.
* **Stavové:** použít stavové služby, pokud chcete Service Fabric pro správu služby stavu přes jeho spolehlivé kolekce nebo Reliable Actors programovací modely. Zadejte počet oddílů, které chcete šíří vašemu stavu přes (pro škálovatelnost) při vytváření pojmenovaného služby. Také určete, jak často k vašemu stavu replikaci mezi uzly (spolehlivost). Každá s názvem služba má jednu primární repliku a více sekundárních replik. Stav s názvem služby upravíte zápisem na primární repliku. Tento stav Service Fabric poté replikuje do všech sekundárních replikách udržování vašemu stavu synchronizace. Service Fabric automaticky rozpozná primární repliky se nezdaří a podporuje stávající sekundární repliky na primární repliku. Service Fabric pak vytvoří nové sekundární repliky.  

**Balíček služby**: adresář disk obsahující typ služby `ServiceManifest.xml` souboru. Tento soubor odkazuje na kód, statických dat a balíčky konfigurace pro typ služby. Soubory v adresáři balíčku služby odkazuje typ aplikace `ApplicationManifest.xml` souboru. Například může naleznete v kódu, statických dat a konfigurace balíčky, které tvoří služba databáze balíčku služby.

**S názvem služby**: Po vytvoření pojmenované aplikace, můžete vytvořit instanci jednoho z jeho typů služeb v rámci clusteru zadáním typu služby (pomocí jeho název a verzi). Každá instance typu služby je přiřazen název identifikátor URI oboru v identifikátoru URI s názvem aplikace. Například pokud vytvoříte "Databáze" s názvem služby v rámci "MyNamedApp" s názvem aplikace, identifikátor URI bude vypadat takto: `"fabric:/MyNamedApp/MyDatabase"`. V rámci pojmenované aplikace můžete vytvořit několik pojmenované služeb. Počty instancí nebo repliky a každé pojmenované služby může mít svůj vlastní schéma oddílu.

**Balíček kódu**: adresář disk obsahující typ služby spustitelné soubory (obvykle soubory EXE nebo DLL). Soubory v adresáři balíčku kódu se odkazuje typ služby `ServiceManifest.xml` souboru. Když je vytvořen s názvem služby, balíček kódu zkopírován do uzlu nebo vybrané spouštět službu s názvem uzly. Potom kód spuštěn. Existují dva typy spustitelné soubory balíčku kódu:

* **Spustitelné soubory hosta**: spustitelné soubory, které spustit jako-je na hostitelský operační systém (Windows nebo Linux). To znamená, tyto spustitelné soubory nezadávejte propojit nebo odkazovat všechny soubory modulu runtime Service Fabric a proto se nepoužívá žádné Service Fabric programovací modely. Tyto spustitelné soubory nelze použít některé funkce Service Fabric, jako je například služba pojmenování pro koncový bod zjišťování. Spustitelné soubory hosta nelze zprávu metriky zatížení konkrétní každá instance služby.
* **Spustitelné soubory hostitele služby**: spustitelné soubory, které používají služby Fabric programovací modely propojením se soubory modulu runtime Service Fabric, povolení funkce Service Fabric. Například instanci s názvem služby můžete zaregistrovat koncové body služby DNS Service Fabric a můžete také sestavy metriky zatížení.      

**Data balíčku**: adresář disk obsahující typ služby statické, jen pro čtení datových souborů (obvykle fotografií, zvuk a video soubory). Soubory v adresáři data balíčku se odkazuje typ služby `ServiceManifest.xml` souboru. Když je vytvořen s názvem služby, datový balíček zkopírován do uzlu nebo vybrané spouštět službu s názvem uzly.  Kód spuštění a teď mají přístup k datové soubory.

**Konfigurační balíček**: adresář disk obsahující typ služby statické, jen pro čtení konfigurační soubory (obvykle textových souborů). Soubory v adresáři balíčku konfigurace odkazuje typ služby `ServiceManifest.xml` souboru. Když je vytvořen s názvem služby, soubory v konfigurační balíček se zkopírují do jednoho či více uzlů, které chcete spustit službu s názvem. Kód spuštění a teď mají přístup k konfigurační soubory.

**Kontejnery**: ve výchozím nastavení, Service Fabric nasadí a aktivuje služby jako procesy. Service Fabric lze také nasadit services v kontejneru bitové kopie. Kontejnery jsou technologie virtualizace, která Virtualizuje příslušný operační systém z aplikací. Aplikace a její runtime, závislosti a systémové knihovny spusťte uvnitř kontejneru s úplnou, soukromý přístup do kontejneru vlastní izolované zobrazení konstrukce operačního systému. Service Fabric podporuje kontejnery Docker kontejnerům Linux a Windows Server.  Další informace najdete v tématu [Service Fabric a kontejnery](service-fabric-containers-overview.md).

**Schéma oddílu**: při vytváření služby s názvem, zadáte schéma oddílu. Služby s velkým množstvím stavu rozdělit data do oddílů, které šíří stav mezi uzly clusteru. To umožňuje stavu služby s názvem škálování. V rámci oddílu mají bezstavové služby s názvem instance, zatímco stavové služby s názvem mít repliky. Bezstavové služby s názvem mají obvykle, vždy jen jeden oddíl, protože mají žádný vnitřní stav. Zadejte instance oddílu pro dostupnost; Pokud se jedna instance nezdaří, ostatní instance i nadále fungovat normálně a pak Service Fabric vytvoří novou instanci. Stateful s názvem služby udržovat, že jejich stavu v rámci repliky a každý oddíl má svou vlastní sadu s daným stavem synchronizovány synchronizované replik. Má replika selhat, Service Fabric vytvoří novou repliku z existujících replik.

Pro čtení [Service Fabric oddíl spolehlivé služby](service-fabric-concepts-partitioning.md) Další informace najdete v článku.

## <a name="system-services"></a>Systémových služeb
Existují systémových služeb vytvořených v každém clusteru, které poskytují možnosti platformy Service Fabric.

**Zásady vytváření názvů služby**: pojmenování služby, která přeloží názvy služby do umístění v clusteru má každý Service Fabric cluster. Správa služby názvy a vlastnosti, podobně jako Internetu služby DNS (Domain Name) pro cluster. Klienti bezpečné komunikaci s libovolného uzlu v clusteru pomocí služby DNS přeložit název služby a jeho umístění.  Přesuňte aplikace v rámci clusteru, například kvůli chybám, vyrovnávání prostředků nebo změna velikosti clusteru. Můžete vyvíjet služeb a klientů, které vyřešit aktuální umístění v síti. Klienti získat skutečný počítače IP adresu a port, kde je aktuálně spuštěna.

Čtení [komunikace službou](service-fabric-connect-and-communicate-with-services.md) Další informace o komunikaci rozhraní API, které pracují se službou pojmenování klienta a služby.

**Bitové kopie služby úložiště**: každý Service Fabric cluster má služby úložiště Image Store, kde jsou uchovány balíčky verzí nasazené aplikace. Zkopírujte balíček aplikace do úložiště bitové kopie a poté registrace typu aplikace, které jsou obsažené v tomto balíčku aplikace. Po zřízení typu aplikace vytvoříte aplikaci s názvem z něj. Po odstranění všechny pojmenované aplikace, můžete zrušit registraci typ aplikace ze služby úložiště bitových kopií.

Čtení [pochopit nastavení parametr ImageStoreConnectionString](service-fabric-image-store-connection-string.md) Další informace o službě úložiště bitových kopií.

Pro čtení [nasazení aplikace](service-fabric-deploy-remove-applications.md) článku pro další informace o nasazení aplikace do služby úložiště bitové kopie.

## <a name="built-in-programming-models"></a>Předdefinované programovací modely
Jsou k dispozici pro můžete vytvářet služby, Service Fabric programovací modely rozhraní .NET Framework:

**Spolehlivé služby**: K rozhraní API k sestavení bezzstavovými i stavovými službami. Stavové služby uložit jejich stavu spolehlivé kolekcí (například slovník nebo fronty). Získáte také zařaďte různé zásobníky komunikace například webového rozhraní API a Windows Communication Foundation (WCF).

**Reliable Actors**: K rozhraní API k sestavení bezstavové a stavové objekty prostřednictvím virtuální objektu Actor programovací model. Tento model může být užitečné, když máte velké množství nezávislé jednotky výpočtu nebo stavu. Protože tento model používá model vláken na základě zapnout, je vyhýbat se kód, který volá do jiných aktéři nebo služeb, protože jednotlivé objektu actor nemůže zpracovat další příchozí požadavky, dokud byly dokončeny všechny jeho odchozí požadavky.

Pro čtení [zvolte programovací Model pro vaši službu](service-fabric-choose-framework.md) Další informace najdete v článku.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
Další informace o Service Fabric:

* [Přehled Service Fabric](service-fabric-overview.md)
* [Proč mikroslužeb přístupu k sestavení aplikací?](service-fabric-overview-microservices.md)
* [Scénáře aplikací](service-fabric-application-scenarios.md)

