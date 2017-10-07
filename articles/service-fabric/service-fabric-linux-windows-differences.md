---
title: "aaaAzure Service Fabric rozdíly mezi systémy Linux a Windows | Microsoft Docs"
description: "Rozdíly mezi hello Azure Service Fabric Preview v systému Linux a Azure Service Fabric v systému Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7a16a440dfc8d9006e274f46951be1562e6f10d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Rozdíly mezi Service Fabric v Linuxu (verze Preview) a ve Windows (obecně dostupná verze)

Protože Service Fabric v Linuxu je ve verzi Preview, existují určité funkce, které jsou ve Windows podporované, ale v Linuxu zatím ne. Nakonec sady funkcí hello budou v parita, když je obecně dostupná Service Fabric v systému Linux. S budoucími verzemi se bude tato mezera zmenšovat. Hello následující rozdíly mezi existovat nejnovější dostupné verze hello (tedy mezi verzemi 5.6 v systému Windows a verzi 5.5 v systému Linux): 

* Reliable Collections (a Reliable Stateful Services) 
* ReverseProxy 
* Samostatný instalační program 
* Ověření schématu XML pro soubory manifestu 
* Přesměrování konzoly 
* Hello selhání Analysis Service (DM)
* Ovladače protokolování a svazku a Docker Compose pro kontejnery 
* Zásady správného řízení prostředků pro kontejnery a služby 
* Služba DNS
* Podpora Azure Active Directory
* Ekvivalenty příkazů rozhraní příkazového řádku pro určité příkazy PowerShellu 
* Oproti Linux clusteru (jak rozšířit v další části hello) lze spustit pouze podmnožinu příkazy prostředí Powershell.

>[!NOTE]
>Přesměrování konzoly se nepodporuje v produkčních clusterech, dokonce ani ve Windows.

Nástroje pro vývoj ve Windows a v Linuxu se také liší. Ve Windows se používá sada Visual Studio, PowerShell, VSTS a Trasování událostí pro Windows, zatímco v Linuxu se používá Yeoman, Eclipse, Jenkins a LTTng.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>Rutiny PowerShellu, které nefungují proti linuxovému clusteru Service Fabric

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Další kroky
* [Příprava vývojového prostředí v Linuxu](service-fabric-get-started-linux.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření první aplikace v CSharp v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Pomocí aplikace hello toomanage Service Fabric rozhraní příkazového řádku](service-fabric-application-lifecycle-sfctl.md)
