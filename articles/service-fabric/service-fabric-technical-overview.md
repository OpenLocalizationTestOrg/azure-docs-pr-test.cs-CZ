---
title: aaaLearn Azure Service Fabric terminologie | Microsoft Docs
description: "Přehled terminologie Service Fabric. Popisuje klíčové terminologie konceptů a termínů používaných v hello zbytek hello dokumentaci."
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
ms.openlocfilehash: 4781fc0527b8a58e534183249bc2759aded2730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-terminology-overview"></a>Přehled terminologie Service Fabric
Service Fabric je platforma distribuovaných systémů, která umožňuje snadno toopackage, nasadit a spravovat škálovatelného a spolehlivého mikroslužeb. Toto téma podrobnosti hello terminologie použitá pomocí Service Fabric toounderstand hello termínů používaných v dokumentaci k hello.

Hello koncepty, které jsou uvedené v této části jsou popsány i v hello následující videa Microsoft Virtual Academy: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">základní koncepty</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">koncepty návrhu</a>, a <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">Run-time koncepty</a>.

## <a name="infrastructure-concepts"></a>Koncepty infrastruktury
**Cluster**: sadu virtuální nebo fyzické počítače, do kterých jsou nasazené a spravovat vaše mikroslužeb připojené k síti.  Clustery můžete škálovat toothousands počítačů.

**Uzel**: počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzel. Každý uzel je přiřazen název uzlu (řetězec). Uzly mají charakteristiky jako vlastnosti umístění. Každý počítač nebo virtuální počítač má služby systému Windows automaticky spouštěná `FabricHost.exe`, který spuštění při spuštění a pak spustí dvě spustitelné soubory: `Fabric.exe` a `FabricGateway.exe`. Tyto dvě spustitelné soubory tvoří hello uzlu. Pro testování scénáře, může hostovat více uzlů na jeden počítač nebo virtuální počítač spuštěním několika instancí `Fabric.exe` a `FabricGateway.exe`.

## <a name="application-concepts"></a>Koncepty aplikace
**Typ aplikace**: hello název a verzi přiřazené kolekce tooa typy služeb. Definovaná v `ApplicationManifest.xml` souboru, vložených v adresáři balíčku aplikace, které se pak zkopíruje cluster Service Fabric toohello úložiště bitových kopií. Potom můžete vytvořit s názvem aplikace z tohoto typu aplikací v rámci clusteru hello.

Čtení hello [aplikačního modelu](service-fabric-application-model.md) Další informace najdete v článku.

**Balíček aplikace**: adresář disk obsahující typ aplikace hello `ApplicationManifest.xml` souboru. Odkazy na hello balíčky služeb pro každý typ služby, která vytváří typ aplikace hello. Hello soubory v adresáři balíčku aplikace hello jsou clusteru zkopírovaný tooService prostředků infrastruktury úložiště bitových kopií. Balíček aplikace pro typ e-mailové aplikace může například obsahovat odkazy tooa fronty služby balíček, front-endové služby balíčku a balíček služby databáze.

**Aplikace s názvem**: úložiště bitových kopií zkopírovaný toohello po balíček aplikace vytvořit instanci aplikace hello v rámci clusteru hello zadáním typ balíčku aplikace hello aplikace (pomocí jeho název a verzi). Každá instance typu aplikace je přiřazen název identifikátor URI, který vypadá takto: `"fabric:/MyNamedApp"`. V rámci clusteru můžete vytvořit více aplikací s názvem z typu jednu aplikaci. Můžete také vytvořit s názvem aplikace z typů jinou aplikaci. Každé pojmenované aplikace je spravovaná a verzí nezávisle.      

**Typ služby**: hello název a verzi přiřazené kód balíčky, data balíčky a balíčky konfigurace tooa služby. Definovaná v `ServiceManifest.xml` souboru, vložené do adresáře balíčku služby a adresář balíčku služby hello se pak odkazuje balíček aplikace `ApplicationManifest.xml` souboru. V rámci clusteru hello po vytvoření pojmenované aplikace, můžete vytvořit s názvem služby z jednoho z hello typů služeb typ aplikace. Typ služby Hello `ServiceManifest.xml` soubor popisuje hello služby.

Čtení hello [aplikačního modelu](service-fabric-application-model.md) Další informace najdete v článku.

Existují dva typy služeb:

* **Stateless:** pomocí bezstavové služby, když trvalý stav služby hello je uložené v službě externího úložiště, jako je například Azure Storage, Azure SQL Database nebo Azure Cosmos DB. Bezstavové služby použijte, když má služba hello vůbec žádné trvalé úložiště. Například kalkulačky služba, kde jsou hodnoty předány toohello služby, výpočet se provádí pomocí tyto hodnoty a výsledek je vrácen.
* **Stavové:** použijte stavové služby, pokud má vaše služba stavu přes jeho spolehlivé kolekce nebo Reliable Actors programovací modely toomanage Service Fabric. Zadejte, kolik oddíly, které chcete použít toospread vašemu stavu přes (pro škálovatelnost) při vytváření pojmenovaného služby. Také určete, jak často tooreplicate vašemu stavu mezi uzly (pro spolehlivosti). Každá s názvem služba má jednu primární repliku a více sekundárních replik. Stav s názvem služby upravíte napsáním toohello primární repliky. Service Fabric pak replikuje tento stav tooall hello sekundární repliky udržování vašemu stavu synchronizace. Service Fabric automaticky rozpozná primární repliky se nezdaří a zvýší úroveň existující primární repliku tooa sekundární repliky. Service Fabric pak vytvoří nové sekundární repliky.  

**Balíček služby**: adresář disk obsahující typ služby hello `ServiceManifest.xml` souboru. Tento soubor odkazuje hello kód, statických dat a balíčky konfigurace pro typ služby hello. Hello soubory v adresáři balíčku služby hello odkazuje typ aplikace hello `ApplicationManifest.xml` souboru. Balíček služby může například odkazovat toohello kód, statických dat a konfigurace balíčky, které tvoří databáze služby.

**S názvem služby**: Po vytvoření pojmenované aplikace, můžete vytvořit instanci jednoho z jeho typů služeb v rámci clusteru hello zadáním typu služby hello (pomocí jeho název a verzi). Každá instance typu služby je přiřazen název identifikátor URI oboru v identifikátoru URI s názvem aplikace. Například pokud vytvoříte "Databáze" s názvem služby v rámci "MyNamedApp" s názvem aplikace, hello URI bude vypadat takto: `"fabric:/MyNamedApp/MyDatabase"`. V rámci pojmenované aplikace můžete vytvořit několik pojmenované služeb. Počty instancí nebo repliky a každé pojmenované služby může mít svůj vlastní schéma oddílu.

**Balíček kódu**: adresář disk obsahující typ služby hello spustitelné soubory (obvykle soubory EXE nebo DLL). Hello soubory v adresáři balíčku kódu hello odkazuje typ služby hello `ServiceManifest.xml` souboru. Když je vytvořen s názvem služby, je balíček kódu hello zkopírovaný toohello uzlu nebo hello toorun vybrané uzly s názvem služby. Potom kód hello spuštění. Existují dva typy spustitelné soubory balíčku kódu:

* **Spustitelné soubory hosta**: spustitelné soubory, které spustit jako-je hello hostitelský operační systém (Windows nebo Linux). To znamená tyto spustitelné soubory nezadávejte odkaz tooor všechny soubory modulu runtime Service Fabric a proto nepoužívají žádné Service Fabric programovací modely. Tyto spustitelné soubory jsou nelze toouse, které se některé funkce Service Fabric, jako hello naming service pro koncový bod zjišťování. Spustitelné soubory hosta nemůžete hlásit instance služby konkrétní tooeach metriky zatížení.
* **Spustitelné soubory hostitele služby**: spustitelné soubory, které používají služby Fabric programovací modely pomocí propojení soubory modulu runtime tooService prostředků infrastruktury, povolení funkce Service Fabric. Například instanci s názvem služby můžete zaregistrovat koncové body služby DNS Service Fabric a můžete také sestavy metriky zatížení.      

**Data balíčku**: adresář disk obsahující typ služby hello statické, jen pro čtení datových souborů (obvykle fotografií, zvuk a video soubory). Hello soubory v adresáři balíčku data hello odkazuje typ služby hello `ServiceManifest.xml` souboru. Když je vytvořen s názvem služby, hello datového balíčku je zkopírovaný toohello uzlu nebo hello toorun vybrané uzly s názvem služby.  Kód Hello spuštění a teď mají přístup k hello datových souborů.

**Konfigurační balíček**: adresáře disku, který obsahuje statický typ služby hello, soubory jen pro čtení konfigurace (obvykle textových souborů). Hello soubory v adresáři balíčku konfigurace hello odkazuje typ služby hello `ServiceManifest.xml` souboru. Když je vytvořen s názvem služby, hello soubory v balíčku konfigurace hello jsou zkopírovaný toohello jednu nebo hello vybrané toorun další uzly s názvem služby. Potom kód hello spuštění a teď mají přístup k hello konfigurační soubory.

**Kontejnery**: ve výchozím nastavení, Service Fabric nasadí a aktivuje služby jako procesy. Service Fabric lze také nasadit services v kontejneru bitové kopie. Kontejnery jsou technologie virtualizace, která Virtualizuje hello příslušný operační systém z aplikací. Aplikace a její runtime, závislosti a systémové knihovny spusťte uvnitř kontejneru s úplnou, soukromý přístup toohello kontejneru vlastní izolované zobrazení konstrukce operačního systému. Service Fabric podporuje kontejnery Docker kontejnerům Linux a Windows Server.  Další informace najdete v tématu [Service Fabric a kontejnery](service-fabric-containers-overview.md).

**Schéma oddílu**: při vytváření služby s názvem, zadáte schéma oddílu. Služby s velkým množstvím stavu rozdělit hello data do oddílů, které šíří hello stavu mezi uzly clusteru hello. To umožňuje tooscale s názvem služby stavu. V rámci oddílu mají bezstavové služby s názvem instance, zatímco stavové služby s názvem mít repliky. Bezstavové služby s názvem mají obvykle, vždy jen jeden oddíl, protože mají žádný vnitřní stav. instance oddílu Hello přinášejí dostupnosti; Pokud se jedna instance nezdaří, ostatní instance normálně pokračovat toooperate a pak Service Fabric vytvoří novou instanci. Jejich stavu v rámci repliky udržovat stateful s názvem služby a každý oddíl má svou vlastní sadu stavem hello synchronizovány synchronizace replik. Má replika selhat, Service Fabric vytvoří novou repliku z existující repliky hello.

Čtení hello [Service Fabric oddíl spolehlivé služby](service-fabric-concepts-partitioning.md) Další informace najdete v článku.

## <a name="system-services"></a>Systémových služeb
Existují systémových služeb vytvořených v každém clusteru, které poskytují možnosti hello platformy Service Fabric.

**Zásady vytváření názvů služby**: pojmenování služby, která přeloží umístění tooa názvy služby v hello clusteru má každý Service Fabric cluster. Spravovat hello služby názvy a vlastnosti, podobně jako tooan internetové služby DNS (Domain Name) pro hello cluster. Klienti se bezpečně komunikovat s libovolného uzlu v clusteru hello pomocí tooresolve hello služby DNS název služby a jeho umístění.  Přesuňte aplikace v rámci clusteru hello například z důvodu toofailures, vyrovnávání prostředků nebo změnou velikosti hello hello clusteru. Můžete vyvíjet služeb a klientů, které řešení hello aktuálnímu umístění v síti. Klienti získat hello skutečné počítače IP adresu a port, kde je aktuálně spuštěna.

Čtení [komunikace službou](service-fabric-connect-and-communicate-with-services.md) Další informace o hello klientem a službou komunikace rozhraní API, které pracují s hello Naming service.

**Bitové kopie služby úložiště**: každý Service Fabric cluster má služby úložiště Image Store, kde jsou uchovány balíčky verzí nasazené aplikace. Zkopírujte balíček toohello aplikaci úložiště Image Store a pak registrace typu aplikace hello obsažené v tomto balíčku aplikace. Po zřízení typu aplikace hello vytvoříte aplikaci s názvem z něj. Po odstranění všechny pojmenované aplikace, můžete zrušit registraci typ aplikace z hello service úložiště Image Store.

Čtení [pochopit nastavení parametr ImageStoreConnectionString hello](service-fabric-image-store-connection-string.md) Další informace o hello service úložiště Image Store.

Čtení hello [nasazení aplikace](service-fabric-deploy-remove-applications.md) článek Další informace o nasazení aplikace toohello Image store služby.

## <a name="built-in-programming-models"></a>Předdefinované programovací modely
Nejsou k dispozici pro vás služby Service Fabric toobuild programovací modely rozhraní .NET Framework:

**Spolehlivé služby**: K rozhraní API toobuild bezzstavovými i stavovými službami. Stavové služby uložit jejich stavu spolehlivé kolekcí (například slovník nebo fronty). Můžete také získat tooplug v různých balíčcích komunikace, jako jsou webové rozhraní API a Windows Communication Foundation (WCF).

**Reliable Actors**: objekty bezstavové a stavové toobuild k rozhraní API prostřednictvím hello virtuální objektu Actor programovací model. Tento model může být užitečné, když máte velké množství nezávislé jednotky výpočtu nebo stavu. Tento model používá model vláken na základě zapnout, proto je nejlepší tooavoid kód, který volá na služby nebo tooother aktéři vzhledem k tomu, že jednotlivé objektu actor nemůže zpracovat další příchozí požadavky, dokud byly dokončeny všechny jeho odchozí požadavky.

Čtení hello [zvolte programovací Model pro vaši službu](service-fabric-choose-framework.md) Další informace najdete v článku.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
Další informace o Service Fabric toolearn:

* [Přehled Service Fabric](service-fabric-overview.md)
* [Proč mikroslužeb přístupu toobuilding aplikace?](service-fabric-overview-microservices.md)
* [Scénáře aplikací](service-fabric-application-scenarios.md)

