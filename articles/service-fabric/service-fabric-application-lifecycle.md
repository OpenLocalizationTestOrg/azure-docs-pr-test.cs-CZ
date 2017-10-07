---
title: "životní cyklus aaaApplication v Service Fabric | Microsoft Docs"
description: "Popisuje vývoj, nasazení, testování, upgrade, údržbu a odebírání aplikací Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 08837cca-5aa7-40da-b087-2b657224a097
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/14/2017
ms.author: ryanwi
ms.openlocfilehash: 36cd6081010e83cb8226c8f85d1e912ac9eebd00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-lifecycle"></a>Životní cyklus aplikace Service Fabric
Jako s jinými platformami, aplikace na Azure Service Fabric obvykle projde hello následující fáze: návrh, vývoj, testování, nasazení, upgrade, údržbu a odebírání. Service Fabric nabízí prvotřídní podporu pro životní cyklus úplné aplikace hello cloudových aplikací, od vývoje přes nasazení, každodenní správu a údržbu tooeventual vyřazení z provozu. model služby Hello umožňuje několika tooparticipate různé role nezávisle v životním cyklu aplikací hello. Tento článek obsahuje přehled hello rozhraní API a jak se používají v hello různé role v rámci hello fáze životního cyklu aplikace Service Fabric hello.

Popisuje, Hello následující video Microsoft Virtual Academy jak toomanage životním cyklu aplikací:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-application-lifecycle/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="service-model-roles"></a>Role služby modelu
Hello služby modelu role jsou:

* **Služba vývojáře**: sama vyvinula modulární a obecné služby, které můžete jinému účelu a použít v několika aplikací hello stejný typ nebo různé typy. Například fronty služby slouží k vytváření lístků aplikace (technická podpora) nebo aplikace pro elektronické obchodování (nákupní košík).
* **Vývojář aplikace**: vytvoří aplikace integrací kolekce služeb toosatisfy některé specifické požadavky nebo scénářů. Webem elektronického obchodování například může integrovat "JSON bezstavové Front-End služby," "Aukce Stateful služba" a "Fronty Stateful služba" toobuild auctioning řešení.
* **Správce aplikací**: rozhoduje na konfiguraci aplikace hello (vyplnění parametry šablony konfigurace hello), nasazení (mapování tooavailable prostředky) a kvalitu služeb. Například Správce aplikací rozhodne hello národní prostředí (angličtina pro hello USA nebo v japonštině pro Japonsko, např.) aplikace hello. Jiné nasazené aplikace mohou mít různá nastavení.
* **Operátor**: nasazení aplikace na základě konfigurace aplikace hello a požadavky na zadaný správcem aplikace hello. Například operátor zřídí a nasadí aplikaci hello a zajistí, že je spuštěna v Azure. Operátory informace o stavu a výkonu aplikaci infrastrukturu monitorovat a spravovat hello fyzické podle potřeby.

## <a name="develop"></a>Vývoj
1. A *služby vývojáře* sama vyvinula různých typů služeb pomocí hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) nebo [spolehlivé služby](service-fabric-reliable-services-introduction.md) programovací model.
2. A *služby vývojáře* deklarativně popisuje typy služby hello vyvinuté v soubor manifestu služby, který se skládá z jedné nebo více balíčků kódu, konfigurace a data.
3. *Vývojář aplikace* pak sestavení aplikace pomocí typy jinou službu.
4. *Vývojář aplikace* deklarativně popisuje typ aplikace hello v manifestu aplikace odkazující na manifesty služby hello hello základních služeb a odpovídajícím způsobem přepsání a Parametrizace jiné konfigurace a nasazení nastavení hello základních služeb.

V tématu [začít pracovat s Reliable Actors](service-fabric-reliable-actors-get-started.md) a [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md) příklady.

## <a name="deploy"></a>Nasazení
1. *Správce aplikací* přizpůsobuje jim hello aplikace typu tooa konkrétní aplikaci toobe nasazené tooa cluster Service Fabric zadáním hello příslušné parametry hello **ApplicationType**element v manifestu aplikace hello.
2. *Operátor* nahrávání hello úložiště bitových kopií clusteru toohello balíčku aplikace pomocí hello [ **CopyApplicationPackage** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) nebo hello [  **Kopírování ServiceFabricApplicationPackage** rutiny](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). balíček aplikace Hello obsahuje manifest aplikace hello a hello kolekce balíčky služeb. Service Fabric nasadí aplikace z balíčku aplikace hello je uložený v úložišti bitové kopie hello, což může být na úložiště objektů blob v Azure nebo hello služba system Service Fabric.
3. Hello *operátor* pak zřídí typ aplikace hello v hello cílový cluster z balíčku nahrané aplikace hello pomocí hello [ **ProvisionApplicationAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Register-ServiceFabricApplicationType** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), nebo hello [ **zřídit aplikaci** operace REST](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
4. Po zřízení aplikace hello *operátor* spustí hello aplikace s parametry hello poskytl hello *Správce aplikací* pomocí hello [  **CreateApplicationAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CreateApplicationAsync_System_Fabric_Description_ApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [ **New-ServiceFabricApplication** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricapplication), nebo hello [ **vytvořit Aplikace** operace REST](https://docs.microsoft.com/rest/api/servicefabric/create-an-application).
5. Po nasazení aplikace hello *operátor* hello používá [ **CreateServiceAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_CreateServiceAsync_System_Fabric_Description_ServiceDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [  **Nové ServiceFabricService** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice), nebo hello [ **vytvořit službu** operace REST](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) na základě toocreate nové instance služby pro aplikaci hello typy dostupných služeb.
6. Hello aplikace je nyní spuštěna v clusteru Service Fabric hello.

V tématu [nasazení aplikace](service-fabric-deploy-remove-applications.md) příklady.

## <a name="test"></a>Test
1. Po nasazení toohello místního vývojového clusteru nebo na zkušební cluster *služby vývojáře* spustí hello scénáře integrované převzetí služeb při selhání testu s použitím hello [ **FailoverTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters#System_Fabric_Testability_Scenario_FailoverTestScenarioParameters) a [ **FailoverTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario#System_Fabric_Testability_Scenario_FailoverTestScenario) třídy nebo hello [ **Invoke-ServiceFabricFailoverTestScenario** rutiny ](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps). scénáře testovacího převzetí služeb při selhání Hello spustí určitou službu prostřednictvím důležité tooensure přechody a převzetí služeb při selhání, že je stále dostupné a funkční.
2. Hello *služby vývojáře* pak spustí hello scénáře integrované chaos testu pomocí hello [ **ChaosTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters#System_Fabric_Testability_Scenario_ChaosTestScenarioParameters) a [  **ChaosTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario#System_Fabric_Testability_Scenario_ChaosTestScenario) třídy nebo hello [ **Invoke-ServiceFabricChaosTestScenario** rutiny](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps). scénáře testu chaos Hello náhodně indukuje více uzel, balíčku kódu a repliky chyb do clusteru hello.
3. Hello *služby vývojáře* [testy service-to-service komunikace](service-fabric-testability-scenarios-service-communication.md) vytvářením testovací scénáře, které přesunout primární repliky kolem hello clusteru.

V tématu [Úvod toohello selhání Analysis Service](service-fabric-testability-overview.md) Další informace.

## <a name="upgrade"></a>Upgrade
1. A *služby vývojáře* aktualizace hello základní služby hello instanci aplikace a opravy chyb a obsahuje novou verzi hello service manifest.
2. *Vývojář aplikace* přepsání a parameterizes hello nastavení konfigurace a nasazení služeb hello konzistentní a obsahuje novou verzi souboru manifestu aplikace hello. Vývojář aplikace Hello pak zahrnuje hello nové verze hello služby manifestů do aplikace hello a poskytuje nové verze typu aplikace hello v balíčku aktualizované aplikace.
3. *Správce aplikací* hello nová verze typu aplikace hello zapadá do cílové aplikace hello aktualizací hello příslušné parametry.
4. *Operátor* nahrávání hello aktualizovanou aplikaci balíčku toohello clusteru úložiště image store pomocí hello [ **CopyApplicationPackage** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) nebo hello [ **Kopie ServiceFabricApplicationPackage** rutiny](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). balíček aplikace Hello obsahuje manifest aplikace hello a hello kolekce balíčky služeb.
5. *Operátor* zřizuje hello nové verze aplikace hello v hello cílový cluster s použitím hello [ **ProvisionApplicationAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Register-ServiceFabricApplicationType** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), nebo hello [ **zřídit aplikaci** operace REST](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
6. *Operátor* upgrady hello cíl aplikací toohello novou verzi pomocí hello [ **UpgradeApplicationAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpgradeApplicationAsync_System_Fabric_Description_ApplicationUpgradeDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [  **Spuštění ServiceFabricApplicationUpgrade** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationupgrade), nebo hello [ **Upgrade aplikace** operace REST](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application).
7. *Operátor* kontroly hello průběh upgradu pomocí hello [ **GetApplicationUpgradeProgressAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_GetApplicationUpgradeProgressAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), hello [  **Get-ServiceFabricApplicationUpgrade** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricapplicationupgrade), nebo hello [ **získat průběh upgradu aplikace** operace REST](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1).
8. V případě potřeby hello *operátor* upraví a znovu použije hello parametry aktuální upgradu aplikace hello pomocí hello [ **UpdateApplicationUpgradeAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpdateApplicationUpgradeAsync_System_Fabric_Description_ApplicationUpgradeUpdateDescription_System_TimeSpan_System_Threading_CancellationToken_), Hello [ **aktualizace ServiceFabricApplicationUpgrade** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/update-servicefabricapplicationupgrade), nebo hello [ **aktualizace aplikace upgradu** operace REST](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade).
9. V případě potřeby hello *operátor* vrátí zpět aktuální upgradu aplikace hello pomocí hello [ **RollbackApplicationUpgradeAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RollbackApplicationUpgradeAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Start-ServiceFabricApplicationRollback** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationrollback), nebo hello [ **vrácení upgradu aplikace** operace REST](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade).
10. Service Fabric upgraduje hello cílové aplikace běžící v clusteru hello bez ztráty hello dostupnost žádné z jejích základních služeb.

V tématu hello [upgradu kurz aplikace](service-fabric-application-upgrade-tutorial.md) příklady.

## <a name="maintain"></a>Udržovat
1. Pro operační systém upgraduje a opravy Service Fabric rozhraní s hello infrastrukturu Azure tooguarantee dostupnost všechny hello aplikace běžící v clusteru hello.
2. Pro upgrade a opravy toohello platformy Service Fabric upgradů Service Fabric samotné bez ztráty dostupnosti všech hello aplikací běžících na clusteru hello.
3. *Správce aplikací* schválí hello přidání nebo odebrání uzlů z clusteru se po analýze kapacity historická data o využití a předpokládané budoucí poptávky.
4. *Operátor* přidá a odebere uzly určeného hello *Správce aplikací*.
5. Při přidání nových uzlů z clusteru hello odstranit existující uzly tooor, Service Fabric automaticky zatížení – zůstatky hello spouštění aplikací pro všechny uzly v hello clusteru tooachieve optimální výkon.

## <a name="remove"></a>Odebrat
1. *Operátor* můžete odstranit konkrétní instanci služby běžící v clusteru hello bez odebrání hello celá aplikace pomocí hello [ **DeleteServiceAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_DeleteServiceAsync_System_Fabric_Description_DeleteServiceDescription_System_TimeSpan_System_Threading_CancellationToken_) , hello [ **odebrat ServiceFabricService** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricservice), nebo hello [ **odstranění služby** operace REST](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service).  
2. *Operátor* můžete také odstranit instanci aplikace a všechny její služby pomocí hello [ **DeleteApplicationAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_DeleteApplicationAsync_System_Fabric_Description_DeleteApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), hello [ **Odebrat ServiceFabricApplication** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricapplication), nebo hello [ **odstranit aplikaci** operace REST](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application).
3. Jakmile přestaly hello aplikací a služeb, hello *operátor* můžete zrušit zřízení typu aplikace hello pomocí hello [ **UnprovisionApplicationAsync** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UnprovisionApplicationAsync_System_String_System_String_System_TimeSpan_System_Threading_CancellationToken_), Hello [ **Unregister-ServiceFabricApplicationType** rutiny](https://docs.microsoft.com/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype), nebo hello [ **zrušit zřízení aplikace** operace REST](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Rušení zřizování typ aplikace hello neodebere balíčku aplikace hello z hello úložiště bitových kopií. Balíček aplikace hello musíte ručně odebrat.
4. *Operátor* balíčku aplikace hello odebere z hello úložiště bitových kopií pomocí hello [ **RemoveApplicationPackage** metoda](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RemoveApplicationPackage_System_String_System_String_) nebo hello [ **Odebrat ServiceFabricApplicationPackage** rutiny](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps).

V tématu [nasazení aplikace](service-fabric-deploy-remove-applications.md) příklady.

## <a name="next-steps"></a>Další kroky
Další informace o vývoji testování a Správa aplikací Service Fabric a služeb, najdete v části:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Nasazení aplikace](service-fabric-deploy-remove-applications.md)
* [Upgrade aplikace](service-fabric-application-upgrade.md)
* [Přehled testovatelnosti](service-fabric-testability-overview.md)
