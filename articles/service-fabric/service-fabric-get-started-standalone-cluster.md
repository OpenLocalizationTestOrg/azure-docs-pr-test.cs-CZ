---
title: "aaaSet clusteru Azure Service Fabric samostatné | Microsoft Docs"
description: "Vytvoření clusteru s podporou samostatné vývoj s tři uzly systémem hello stejný počítač. Po dokončení této instalace, bude připravená toocreate clusteru s více počítači."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: e4d0ea9fc3b8475160bd8ed19fd3716463791cc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a>Vytvoření vašeho prvního samostatného clusteru Service Fabric
Můžete vytvořit samostatný cluster Service Fabric na všechny virtuální počítače nebo počítače se systémem Windows Server 2012 R2 nebo Windows Server 2016, místní nebo v cloudu hello. Tento rychlý start vám pomůže toocreate samostatný cluster s podporou vývoj právě za několik minut.  Po provedení postupu budete mít cluster se třemi uzly, který běží v jednom počítači a do kterého můžete nasazovat aplikace.

## <a name="before-you-begin"></a>Než začnete
Service Fabric nabízí instalaci balíčku toocreate Service Fabric samostatné clustery.  [Stáhnout instalační balíček hello](http://go.microsoft.com/fwlink/?LinkId=730690).  Rozbalte hello instalace tooa složku balíčku na hello počítač nebo virtuální počítač, kde nastavujete hello vývojový cluster.  jsou podrobně popsány Hello obsah instalačního balíčku hello [zde](service-fabric-cluster-standalone-package-contents.md).

Správce clusteru Hello nasazení a konfigurace clusteru hello musí mít oprávnění správce na počítači hello. Service Fabric nelze nainstalovat na řadič domény.

## <a name="validate-hello-environment"></a>Ověření hello prostředí
Hello *TestConfiguration.ps1* skript v balíčku samostatné hello slouží jako nejlepší toovalidate analyzátor postupy, zda clusteru můžou být nasazené na dané prostředí. [Příprava nasazení](service-fabric-cluster-standalone-deployment-preparation.md) seznamy hello požadavky a požadavky na prostředí. Spusťte skript tooverify hello, pokud můžete vytvořit cluster vývoj hello:

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-hello-cluster"></a>Vytvoření clusteru hello
Několik ukázkových clusteru konfiguračních souborů jsou nainstalovány s hello instalační balíček. *ClusterConfig.Unsecure.DevCluster.json* je nejjednodušší konfiguraci clusteru hello: nezabezpečená, tři uzly clusteru se systémem na jednom počítači.  Další konfigurační soubory popisují clustery s jedním nebo více počítači zabezpečené pomocí certifikátů X.509 nebo s použitím zabezpečení systému Windows.  Nemáte potřebovat toomodify hello výchozí konfigurace nastavení pro tento kurz, ale projděte hello konfiguračního souboru a seznamte se s hello nastavení.  Hello **uzly** část popisuje hello tři uzly v clusteru hello: název, IP adresa, [typ uzlu, doména selhání a upgradu domény](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  Hello **vlastnosti** oddíl definuje hello [zabezpečení, úroveň spolehlivosti, diagnostiky kolekce a typy uzlů](service-fabric-cluster-manifest.md#cluster-properties) pro hello cluster.

Tento cluster není zabezpečený.  Každý se může anonymně připojit a provádět operace správy, proto by produkční clustery vždy měly být zabezpečené pomocí certifikátů X.509 nebo zabezpečení systému Windows.  Zabezpečení je nakonfigurovaná pouze při vytváření clusteru a není možné tooenable zabezpečení po vytvoření clusteru hello.  Čtení [zabezpečení clusteru](service-fabric-cluster-security.md) toolearn Další informace o zabezpečení clusteru Service Fabric.  

toocreate hello vývoj tři uzly clusteru, spusťte hello *CreateServiceFabricCluster.ps1* skriptu z relace prostředí PowerShell správce:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

balíček modulu runtime Service Fabric Hello je automaticky stáhnout a nainstalovat v době vytváření clusteru.

## <a name="connect-toohello-cluster"></a>Připojte toohello cluster
Vývojový cluster se třemi uzly je nyní spuštěn. Hello modulu ServiceFabric PowerShell se instaluje s hello runtime.  Můžete ověřit, že hello clusteru je spuštěn z hello stejný počítač nebo ze vzdáleného počítače pomocí modulu runtime Service Fabric hello.  Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutina vytvoří cluster toohello připojení.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
V tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md) pro další příklady připojování tooa clusteru. Po připojování toohello clusteru pomocí hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny toodisplay seznam uzlů v clusteru a stavové informace pro každý uzel hello. Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-hello-cluster-using-service-fabric-explorer"></a>Vizualizace hello clusteru pomocí Service Fabric Exploreru
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je nástroj vhodný pro vizualizaci clusteru a správu aplikací.  Service Fabric Explorer je služba, která běží v clusteru hello, kterým můžete přistupovat pomocí prohlížeče tak, že přejdete příliš[http://localhost: 19080/Explorer](http://localhost:19080/Explorer). 

řídicí panel clusteru Hello obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace. zobrazení uzlu Hello zobrazuje fyzické rozložení hello hello clusteru. Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-hello-cluster"></a>Odebrat hello cluster
tooremove clusteru, spusťte hello *RemoveServiceFabricCluster.ps1* skript prostředí PowerShell ze složky balíčku hello a předejte jí hello cesta toohello JSON konfigurační soubor. Volitelně můžete zadat umístění pro protokol hello hello odstranění.

```powershell
# Removes Service Fabric cluster nodes from each computer in hello configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

tooremove runtime Service Fabric hello z hello počítači, spusťte následující skript prostředí PowerShell ze složky balíčku hello hello.

```powershell
# Removes Service Fabric from hello current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a>Další kroky
Teď, když jste nastavili vývojový samostatný cluster, zkuste hello následující články:
* [Nastavení samostatného clusteru s více počítači](service-fabric-cluster-creation-for-windows-server.md) a povolení zabezpečení
* [Nasazení aplikací s použitím prostředí PowerShell](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
