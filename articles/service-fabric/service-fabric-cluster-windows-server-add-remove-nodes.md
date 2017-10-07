---
title: "aaaAdd nebo odebrat uzly tooa samostatný cluster Service Fabric | Microsoft Docs"
description: "Zjistěte, jak tooadd nebo odebrat uzly tooan Azure Service Fabric clusteru na fyzický nebo virtuální počítač se systémem Windows Server, který může být místní nebo v jakékoli cloudu."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: dekapur
ms.openlocfilehash: 1da908ad9840faa052e0b4021bc2d4ce732b02bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-nodes-tooa-standalone-service-fabric-cluster-running-on-windows-server"></a>Přidat nebo odebrat uzly tooa samostatný cluster Service Fabric systémem Windows Server
Až budete mít [vytvořen samostatný cluster Service Fabric na počítačích systému Windows Server](service-fabric-cluster-creation-for-windows-server.md), může změnit obchodních potřeb a může být nutné tooadd nebo odebrat uzly tooyour clusteru. Tento článek obsahuje podrobné pokyny tooachieve to. Upozorňujeme, že přidat nebo odebrat uzel funkce není podporována v clusterech místní vývoj.

## <a name="add-nodes-tooyour-cluster"></a>Přidat uzly clusteru tooyour
1. Příprava hello virtuálního počítače nebo počítače chcete tooadd tooyour clusteru podle následujících kroků hello v hello [hello Příprava počítačů toomeet hello požadavky pro nasazení clusteru](service-fabric-cluster-creation-for-windows-server.md) části
2. Identifikovat, které domény selhání a upgradu domény jsou probíhající tooadd tohoto virtuálního počítače nebo počítače.
3. Vzdálené plochy (RDP) do virtuálního počítače nebo počítače, které chcete tooadd toohello cluster hello
4. Kopírování nebo [stáhnout hello samostatný balíček pro Service Fabric pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello virtuálního počítače nebo počítače a rozbalte balíček hello
5. Spusťte prostředí Powershell se zvýšenými oprávněními a přejděte toohello umístění rozbalené balíčku hello
6. Spustit hello *AddNode.ps1* skriptu s parametry hello popisující nový uzel tooadd hello. Následující příklad Hello přidá nový uzel s názvem VM5, s typem NodeType0 a IP adres 182.17.34.52, do UD1 a fd: / dc1/r0. Hello *ExistingClusterConnectionEndPoint* je koncový bod připojení pro uzel již v hello stávajícího clusteru, může být IP adresa hello *žádné* uzlu v clusteru hello.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    Po dokončení skriptu hello můžete zkontrolovat, zda byl přidán nový uzel hello spuštěním hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny.

7. tooensure konzistence mezi různými uzly v clusteru hello, je nutné inicializovat konfigurace upgradu. Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello nejnovější konfigurační soubor a přidejte hello příliš nově přidaná uzlu část "Uzlů". Je také vhodné tooalways mít hello nejnovější konfigurace clusteru k dispozici v případě hello, je nutné, aby tooredploy cluster s hello stejnou konfiguraci.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgradu.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru. Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-tooclusters-configured-with-windows-security-using-gmsa"></a>Přidat uzly tooclusters nakonfigurováni pomocí gMSA zabezpečení systému Windows
Pro clustery nakonfigurované skupiny spravované služby Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx) můžete přidat nový uzel pomocí konfigurace upgradu:
1. Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) na všech uzlech existujícího hello tooget hello nejnovější konfigurační soubor a přidat podrobnosti o novém uzlu hello chcete tooadd v části uzly"hello". Zajistěte, aby nový uzel hello je součástí systému hello stejné skupiny spravovaný účet. Tento účet by měl být správce na všech počítačích.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgradu.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>
    ```
    Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru. Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-tooyour-cluster"></a>Přidání uzlu typy tooyour clusteru
V pořadí tooadd nového typu uzlu, úpravě vaše konfigurace tooinclude hello nového typu uzlu v oddílu "NodeTypes" v části "Vlastnosti" a zahájit konfiguraci upgradovat pomocí [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Po dokončení upgradu hello můžete přidat nový cluster tooyour uzly s tímto typem uzlu.

## <a name="remove-nodes-from-your-cluster"></a>Odebrání uzlů z clusteru
Uzel může být odebrán z clusteru pomocí upgrade konfigurace v hello následujícím způsobem:

1. Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello nejnovější konfigurační soubor a *odebrat* hello uzlu z části "Uzlů".
Přidat hello "NodesToBeRemoved" parametr příliš "nastavení" tématu v části "FabricSettings". Hello "value" by měl být čárkami oddělený seznam názvů uzlu uzlů, které je třeba toobe odebrat.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgradu.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru. Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Odebrání uzlů může zahájit více upgradů. Některé uzly jsou označené jako `IsSeedNode=”true”` značky a lze identifikovat podle dotazování hello clusteru manifestu pomocí `Get-ServiceFabricClusterManifest`. Odebrání těchto uzlů může trvat déle než jiné, vzhledem k tomu, že uzly počáteční hodnoty hello bude mít toobe přesunout v takových scénářů. Hello clusteru musí zachovat minimálně 3 uzlů typu primárního uzlu.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Odebrat typy uzlů z clusteru
Před odebráním typ uzlu, prosím Překontrolujte, pokud jsou všechny uzly odkazující na typ uzlu hello. Odeberte tyto uzly před odebráním hello odpovídající typ uzlu. Po odebrání všech odpovídajících uzlů můžete odebrat hello NodeType z hello konfiguraci clusteru a zahájit konfiguraci upgradovat pomocí [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Nahraďte primární uzlů ve vašem clusteru
nahrazení Hello primární uzlů musí být provádí jeden uzel za druhým, místo odebráním a potom přidat v dávkách.


## <a name="next-steps"></a>Další kroky
* [Nastavení konfigurace pro samostatný cluster Windows](service-fabric-cluster-manifest.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty](service-fabric-windows-cluster-x509-security.md)
* [Vytvořit cluster Service Fabric samostatné s virtuálními počítači Azure s Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)

