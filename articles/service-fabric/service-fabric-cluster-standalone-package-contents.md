---
title: "aaaAzure samostatný balíček prostředků infrastruktury služby pro systém Windows Server | Microsoft Docs"
description: "Popis a obsah hello Azure Service Fabric samostatný balíček pro systém Windows Server."
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik
ms.openlocfilehash: e4c6cb9128d659144e559fcad06bb78c9969dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Obsah balíčku Fabric samostatné služby pro systém Windows Server
V hello [Stáhnout](http://go.microsoft.com/fwlink/?LinkId=730690) balíček Service Fabric Standalone zjistíte hello následující soubory:

| **Název souboru** | **Krátký popis** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |Skript prostředí PowerShell, který vytváří cluster hello pomocí hello nastavení v souboru ClusterConfig.json. |
| RemoveServiceFabricCluster.ps1 |Skript prostředí PowerShell, které odstraní cluster pomocí hello nastavení v souboru ClusterConfig.json. |
| AddNode.ps1 |Skript prostředí PowerShell pro přidání uzlu tooan existující nasazení clusteru hello aktuálního počítače. |
| RemoveNode.ps1 |Skript prostředí PowerShell pro odebrání uzlu ze stávajícího nasazení clusteru z aktuálního počítače hello. |
| CleanFabric.ps1 |Skript prostředí PowerShell pro čištění samostatnou instalaci Service Fabric vypnout hello aktuálního počítače. Pomocí vlastních přidružené uninstallers, byste měli odebrat předchozí instalace Instalační služby MSI. |
| TestConfiguration.ps1 |Skript prostředí PowerShell pro analýzu infrastruktury hello zadané v hello Cluster.json. |
| DownloadServiceFabricRuntimePackage.ps1 |Skript prostředí PowerShell pro stahování hello nejnovější modul runtime balíček vzdálené správy, používá pro scénáře, kdy hello nasazení počítače není připojené toohello Internetu. |
| DeploymentComponentsAutoextractor.exe |Samorozbalovací archivu obsahující součásti nasazení používá hello samostatný balíček skripty. |
| EULA_ENU.txt |Hello licenční podmínky pro hello použít Microsoft Azure Service Fabric samostatné Windows serveru balíčku. Můžete [stáhnout kopii hello smlouvy EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nyní. |
| Readme.txt |Odkaz toohello poznámky k verzi a pokyny k základní instalaci. Je podmnožinou hello pokyny v tomto dokumentu. |
| ThirdPartyNotice.rtf |Oznámení o software třetích stran, který je v balíčku hello. |
| Tools\Microsoft.Azure.ServiceFabric.windowsserver.SupportPackage.zip |StandaloneLogCollector.exe který běží na vyžádání toocollect a nahrání trasování zaznamená tooMicrosoft za účelem podpory. |
| Tools\ServiceFabricUpdateService.zip |Nástroj používaný tooenable automaticky kód upgradu pro clustery, které nemají přístup k Internetu. Další podrobnosti najdete [sem](service-fabric-cluster-upgrade-windows-server.md)|

**Šablony** 
| **Název souboru** | **Krátký popis** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Soubor ukázkové konfiguraci clusteru, který obsahuje hello nastavení pro zabezpečená, tři uzly, jeden počítač (nebo virtuální počítač) vývoj clusteru služby, včetně hello informace pro každý uzel v clusteru hello. |
| ClusterConfig.Unsecure.MultiMachine.json |Soubor ukázkové konfiguraci clusteru, který obsahuje hello nastavení pro cluster služby zabezpečená, více počítače (nebo virtuálního počítače), včetně hello informace pro každý počítač v clusteru hello. |
| ClusterConfig.Windows.DevCluster.json |Soubor ukázkové konfiguraci clusteru, který obsahuje všechny hello nastavení zabezpečení, tři uzly, jednoho počítače (nebo virtuální počítač) vývojového clusteru, včetně hello informace pro každý uzel, který je v clusteru hello. Hello clusteru je zabezpečená pomocí [Windows identity](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |Soubor ukázkové konfiguraci clusteru, který obsahuje všechna nastavení hello zabezpečený a více počítače (nebo virtuálního počítače) clusteru pomocí zabezpečení systému Windows, včetně hello informace pro každý počítač, který je v clusteru zabezpečené hello. Hello clusteru je zabezpečená pomocí [Windows identity](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |Soubor ukázkové konfiguraci clusteru, který obsahuje všechny hello nastavení pro zabezpečení, tři uzly, jeden počítač (nebo virtuální počítač) vývoj cluster, včetně hello informace pro každý uzel v clusteru hello. Hello clusteru zabezpečené pomocí x509 certifikáty. |
| ClusterConfig.x509.MultiMachine.json |Ukázkový soubor konfigurace clusteru, která obsahuje všechna nastavení hello hello zabezpečení clusteru více počítače (nebo virtuálního počítače), včetně hello informace pro každý uzel v clusteru zabezpečené hello. Hello clusteru zabezpečené pomocí x509 certifikáty. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Ukázkový soubor konfigurace clusteru, která obsahuje všechna nastavení hello hello zabezpečení clusteru více počítače (nebo virtuálního počítače), včetně hello informace pro každý uzel v clusteru zabezpečené hello. Hello clusteru zabezpečené pomocí [skupinové účty spravované služby](https://technet.microsoft.com/en-us/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Ukázky konfigurace clusteru
Nejnovější verze šablon konfigurace clusteru najdete na GitHub stránce hello: [ukázky konfigurace clusteru samostatné](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Nezávislé balíček modulu Runtime
Hello nejnovější modul runtime balíček se nestahuje automaticky při nasazení clusteru z [stáhnout odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>Související
* [Vytvoření clusteru s podporou samostatné Azure Service Fabric](service-fabric-cluster-creation-for-windows-server.md)
* [Scénáře zabezpečení clusteru Service Fabric](service-fabric-windows-cluster-windows-security.md)
