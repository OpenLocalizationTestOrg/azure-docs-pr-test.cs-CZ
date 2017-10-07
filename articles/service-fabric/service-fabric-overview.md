---
title: aaaOverview Service Fabric v Azure | Microsoft Docs
description: "Přehled Service Fabric, kde se aplikace skládá z mnoha mikroslužeb tooprovide škálování a odolnost. Service Fabric je distribuovaných systémů platformy používá toobuild spolehlivé, škálovatelné a snadno spravovat aplikace pro hello cloud."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: mfussell
ms.openlocfilehash: 427fcedf97e6b2aae42d240c63e9f85daed8d962
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-service-fabric"></a>Přehled Azure Service Fabric
Azure Service Fabric je platforma distribuovaných systémů, která umožňuje snadno toopackage, nasazení a správě škálovatelného a spolehlivého mikroslužeb a kontejnerů. Service Fabric také řeší hello významné problémy ve vývoji a správě cloudu nativních aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné. Service Fabric představuje hello generace platforma pro vytváření a správě těchto podnikových, úrovně 1, cloudové škálování aplikace běžící v kontejnerech.

Následující krátké video představuje Service Fabric a mikroslužeb:<center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="applications-composed-of-microservices"></a>Aplikace se skládá z mikroslužeb 
Service Fabric vám umožní toobuild a spravovat škálovatelného a spolehlivého aplikace, které se skládají z mikroslužeb, běží na vysokou hustotou na sdílenému fondu počítače, který se označuje tooas cluster. Poskytuje sofistikované, lightweight runtime toobuild, distribuována, škálovatelná, bezstavové a stavové mikroslužeb spuštěna v kontejnerech. Poskytuje také tooprovision možnosti správy komplexní aplikace, nasazení, monitorování, upgrade nebo opravu a odstranění nasazené aplikace, včetně kontejnerizované služeb.

Service Fabric pohání řady služeb Microsoft services dnes, včetně databáze SQL Azure, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype pro firmy a mnoho základní služby Azure.

Service Fabric je cloudové šité na míru toocreate nativní se služby, které můžete spustit malé, podle potřeby a růst toomassive škálování se stovkami nebo tisíci počítačů.

Dnešní internetových služeb jsou vytvářeny ze mikroslužeb. Příklady mikroslužeb zahrnovat brány protokolu, uživatelských profilů, nákupních košíků, inventáře, zpracování, fronty a ukládá do mezipaměti. Service Fabric je mikroslužeb platforma, která poskytuje jedinečný název, který může být bezstavové nebo stavová každých mikroslužbu (nebo kontejneru).

Service Fabric nabízí komplexní runtime a životního cyklu tooapplications možnosti správy, který se skládá z těchto mikroslužeb. Počítač hostuje mikroslužeb uvnitř kontejnery, které jsou v clusteru Service Fabric hello aktivována a nasazena. Přesun z virtuálních počítačů toocontainers umožňuje pořadí velikosti zvýšení hustoty. Podobně bude možno jiné jednotkách o řád v hustotu, při přesunutí z toomicroservices kontejnery v těchto kontejnerech. Například jeden cluster pro Azure SQL Database zahrnuje stovky počítačů s desítkami tisíc kontejnery tohoto hostitele celkem stovky tisíc databáze. Každá databáze je stavová mikroslužbu Service Fabric. 

Další informace o hello mikroslužeb přístup pro čtení [proto mikroslužeb přístup toobuilding aplikace?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Nasazení kontejneru a Orchestrace
Service Fabric je společnosti Microsoft [kontejneru orchestrator](service-fabric-cluster-resource-manager-introduction.md) nasazení mikroslužeb mezi clustery počítačů. Mikroslužeb mohou být vytvořeny v mnoha směrech pomocí hello [Service Fabric programovací modely](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), toodeploying [žádný kód podle svého výběru](service-fabric-deploy-existing-app.md). Je důležité, je možné kombinovat jak služby v procesy a služby v kontejnerech v hello stejná aplikace. Pokud chcete příliš[nasazení a Správa kontejnerů](service-fabric-containers-overview.md), Service Fabric je ideální volbou jako kontejner orchestrator.

## <a name="any-os-any-cloud"></a>Všechny operační systém, všechny cloudu
Service Fabric se spustí everywhere. Můžete vytvořit clustery pro Service Fabric do mnoha prostředí, včetně Azure nebo místní, Windows Server nebo Linux. Clustery můžete vytvořit i na jiné veřejné cloudy. Kromě toho je hello vývojového prostředí v hello SDK **identické** toohello produkčním prostředí se žádné související se situací emulátorů. Jinými slovy co běží na místní vývojový cluster nasadí toohello clustery v jiných prostředích.

![Platforma Service Fabric][Image1]

Další informace o vytváření clusterů místní, přečtěte si [vytvoření clusteru v systému Windows Server nebo Linux](service-fabric-deploy-anywhere.md) nebo pro vytvoření clusteru s podporou Azure [prostřednictvím portálu Azure hello](service-fabric-cluster-creation-via-portal.md).

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Bezstavové a stavové mikroslužeb pro Service Fabric
Service Fabric umožňuje toobuild aplikace, které se skládají z mikroslužeb nebo kontejnerů. Bezstavové mikroslužeb (například protokol brány a webové proxy servery) neudržují měnitelný stav mimo požadavek a odpověď ze služby hello. Role pracovního procesu Azure Cloud Services jsou příkladem bezstavové služby. Stavová mikroslužeb (například uživatelské účty, databáze, zařízení, nákupní košíků a fronty) udržovat měnitelný, autoritativní stavu nad rámec hello požadavku a odpovědi. Dnešní internetových aplikací se skládají z kombinace bezstavové a stavové mikroslužeb. 

Klíče differentation s Service Fabric je silné zaměřuje na budování stavové služby, buď pomocí hello [předdefinované programovací modely ](service-fabric-choose-framework.md) nebo s kontejnerizované stavové služby. Hello [scénáře aplikací](service-fabric-application-scenarios.md) popisují hello scénáře, kdy se používá stavové služby.


## <a name="application-lifecycle-management"></a>Správa životního cyklu aplikací
Service Fabric poskytuje podporu pro CI/CD cloudových aplikací, včetně kontejnery a životního cyklu hello celou aplikaci. Zahrnuje tohoto životního cyklu vývoje po nasazení, každodenní správu a údržbu tooeventual vyřazení z provozu.

Možnosti správy životního cyklu aplikace Service Fabric povolení správci aplikací a IT operátory toouse jednoduchý, nízká touch pracovních tooprovision, nasazení, opravy a monitorování aplikací. Tyto pracovní postupy integrované výrazně omezit hello zátěž IT operátory tookeep aplikace průběžně k dispozici.

Většina aplikací se skládá z kombinace bezstavové a stavové mikroslužeb, kontejnery a další spustitelné soubory, které jsou nasazeny společně. Tak, že silné typy na hello aplikace, umožňuje Service Fabric hello nasazení více instancí aplikace. Každá instance je spravovat a upgradovat nezávisle. Service Fabric je nejdůležitější, můžete nasadit kontejnery nebo všech spustitelných souborů a jejich spolehlivost. Například Service Fabric můžete nasadit .NET, ASP.NET Core, node.js, Windows kontejnery, Linux kontejnery, Java virtuálních počítačů, skripty, úhlová nebo oznámena všechno, co tvoří vaší aplikace.

Service Fabric je integrována CI/CD Nástroje, jako [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [volaných](https://jenkins.io/index.html), a [Chobotnice nasazení](https://octopus.com/) a lze použít s jakýkoli jiný nástroj oblíbených položek konfigurace nebo CD.

Další informace o správě životního cyklu aplikací, přečtěte si [životního cyklu aplikace](service-fabric-application-lifecycle.md). Další informace o toodeploy žádný kód, najdete v části [nasazení spustitelný soubor hosta](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>Klíčové funkce
Pomocí Service Fabric, můžete:

* Nasaďte tooAzure nebo tooon místní datových center, která spustit Windows nebo Linux s nulové změnami kódu. Napsat jednou a pak nasadit kdekoli tooany cluster Service Fabric.
* Škálovatelné aplikace, které se skládají z mikroslužeb vyvíjet pomocí hello Service Fabric programovací modely, kontejnery nebo žádný kód.
* Vytvořte vysoce spolehlivé bezstavové a stavové mikroslužeb. Pomocí stavová mikroslužeb Zjednodušte hello návrh aplikace. 
* Hello nové Reliable Actors programovací model toocreate objekty cloudu pomocí samoobslužné obsažené kód a stavu.
* Nasazení a orchestraci kontejnerů, které zahrnují kontejnery Windows a Linux kontejnery. Service Fabric je vědět, stavová, kontejner orchestrator data.
* Nasazení aplikací v sekundách, se stovkami nebo tisíci aplikací nebo kontejnery na počítač s vysokou hustotou.
* Nasaďte různé verze hello stejná aplikace na straně stranou a upgradujte každou aplikaci nezávisle.
* Správa životního cyklu hello aplikací bez odstávky, včetně dopadem na dřívější kód a pevných upgrady.
* Horizontální navýšení kapacity nebo škálování hello počet uzlů v clusteru. Při změně měřítka uzly, vaše aplikace automaticky škálovat.
* Monitorování a Diagnostika hello stavu aplikací a nastavte zásady pro provedení automatické opravy.
* Podívejte se na vyrovnávání prostředků hello orchestraci hello redistribuci aplikací v clusteru hello. Service Fabric obnoví selhání a optimalizuje hello distribuce zatížení na základě dostupných prostředků.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
* Další informace:
  * [Proč mikroslužeb přístupu toobuilding aplikace?](service-fabric-overview-microservices.md)
  * [Přehled terminologie](service-fabric-technical-overview.md)
* Nastavení Service Fabric [vývojového prostředí](service-fabric-get-started.md)  
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
