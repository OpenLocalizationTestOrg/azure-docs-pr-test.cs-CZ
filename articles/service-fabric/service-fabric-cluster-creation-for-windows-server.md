---
title: "aaaCreate cluster Azure Service Fabric samostatné | Microsoft Docs"
description: "Vytvoření clusteru služby Azure Service Fabric z jakéhokoli počítače (fyzické nebo virtuální) s Windows serverem, ať už místní nebo v jakékoli cloudu."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 444970816290a0448d88a8b2082c75eb7a64cb44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Vytvoření clusteru s podporou samostatné systémem Windows Server
Můžete vytvořit clustery infrastruktury služby Azure Service Fabric toocreate na všechny virtuální počítače nebo počítače se systémem Windows Server. To znamená, můžete nasazovat a spouštět aplikace Service Fabric v jakémkoli prostředí, které obsahuje sadu vzájemně propojena počítačů Windows serveru, je-li jej v místním nebo se všechny poskytovatele cloudových služeb. Service Fabric poskytuje toocreate balíček Instalační program, který clusterů Service Fabric názvem balíček hello samostatné verze Windows serveru.

Tento článek vás provede hello kroky pro vytvoření samostatné clusteru Service Fabric.

> [!NOTE]
> Tento balíček samostatné systému Windows Server je dostupný a mohou být použity pro nasazení v produkčním prostředí. Tento balíček může obsahovat nové funkce Service Fabric, které jsou v "Náhled". Posuňte se dolů příliš"[zobrazit náhled funkce obsažené v tomto balíčku](#previewfeatures_anchor)." část hello seznam hello funkce verze preview. Můžete [stáhnout kopii hello smlouvy EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nyní.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-hello-service-fabric-for-windows-server-package"></a>Získat podporu pro balíček prostředků infrastruktury služby pro systém Windows Server hello
* Požádejte o samostatný balíček Service Fabric se hello hello komunity pro systém Windows Server v hello [fórum Azure Service Fabric](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* Otevřít lístek pro [Professional podporu pro Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).  Další informace o Professional podporu společnosti Microsoft [zde](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Můžete také získat podporu pro tento balíček jako součást [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).
* Další podrobnosti najdete v tématu [možnosti podpory Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).
* protokoly toocollect pro účely podpory, spusťte hello [kolektor protokolů samostatné Service Fabric](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-hello-service-fabric-for-windows-server-package"></a>Stáhněte si balíček prostředků infrastruktury služby pro systém Windows Server hello
toocreate hello clusteru, použijte balíček prostředků infrastruktury služby pro systém Windows Server hello (Windows Server 2012 R2 a novější) nachází tady: <br>
[Stáhněte si odkaz – samostatný balíček prostředků infrastruktury služby - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)

Najde podrobnosti o na obsah balíčku hello [zde](service-fabric-cluster-standalone-package-contents.md).

v době vytváření clusteru automaticky stažení balíčku modulu runtime Service Fabric Hello. Pokud nasazení z počítače není připojený toohello Internetu, stáhněte balíček modulu runtime hello vzdálené správy z tohoto umístění: <br>
[Stáhněte si odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)

Najděte ukázky samostatné konfigurace clusteru na: <br>
[Konfigurace clusteru samostatné – ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-hello-cluster"></a>Vytvoření clusteru hello
Service Fabric může být nasazený tooa jeden počítač vývojového clusteru pomocí hello *ClusterConfig.Unsecure.DevCluster.json* zahrnutý v [ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Rozbalte hello samostatný balíček tooyour počítače, kopie hello ukázka konfigurační soubor toohello místního počítače a pak spusťte hello *CreateServiceFabricCluster.ps1* skriptu prostřednictvím relaci prostředí PowerShell správce ze samostatné hello složky balíčku:
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>Krok 1A: vytvoření zabezpečená místního vývojového clusteru
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

V tématu hello část nastavení prostředí v [plánování a příprava nasazení clusteru](service-fabric-cluster-standalone-deployment-preparation.md) pro řešení potíží s podrobnosti.

Pokud dokončíte spuštěné vývojové scénáře, můžete odebrat cluster Service Fabric hello z hello počítače tím, že odkazuje toosteps v části "[odebrat cluster](#removecluster_anchor)". 

### <a name="step-1b-create-a-multi-machine-cluster"></a>Krok 1B: vytvoření clusteru s více počítači
Poté, co jste prošli hello plánování a přípravné kroky popsané v hello následujícím odkazu, jste připraveni toocreate produkční clusteru pomocí konfiguračního souboru clusteru. <br>
[Plánování a příprava nasazení clusteru](service-fabric-cluster-standalone-deployment-preparation.md)

1. Ověření hello konfigurační soubor jste napsali spuštěním hello *TestConfiguration.ps1* skriptu ze složky balíčku samostatné hello:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Zobrazený výstup by měl jako níže. Pokud pole dolní hello "Úspěšně dokončeno" se vrátí jako "PRAVDA", mít zkontrolovány správností a hello clusteru vypadá toobe nasadit na základě konfigurace vstupní hello.

    ```
    Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. Vytvoření clusteru hello: Spusťte hello *CreateServiceFabricCluster.ps1* skriptu toodeploy hello cluster Service Fabric v každém počítači v konfiguraci hello. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Nasazení trasování jsou zapsány toohello virtuálního počítače nebo počítače, na kterém jste spustili skript prostředí PowerShell CreateServiceFabricCluster.ps1 hello. Ty lze najít v podsložce hello DeploymentTraces, založené na hello adresáře, ze které hello se skript spouštěl. tooa počítače správně nasazena toosee, pokud byl Service Fabric, vyhledejte hello nainstalované soubory v adresáři FabricDataRoot directory hello, jak je podrobně uvedeno v konfiguračním souboru na clusteru hello FabricSettings část (ve výchozím nastavení c:\ProgramData\SF). FabricHost.exe a Fabric.exe procesy je možné taky zobrazit spuštěná ve Správci úloh.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>Krok 1C: vytvoření clusteru do režimu offline (internet odpojení)
Při vytváření clusteru automaticky stažení balíčku modulu runtime Service Fabric Hello. Pokud není nasazení clusteru toomachines připojené toohello Internetu, budete potřebovat toodownload hello Service Fabric runtime balíček samostatně a zadejte cestu tooit hello při vytváření clusteru.
Hello runtime balíčku stahování lze provádět samostatně, z jiného počítače připojené toohello Internetu, v [stáhnout odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). Zkopírujte hello runtime balíček toowhere nasazujete hello clusteru do režimu offline a vytvoření clusteru hello spuštěním `CreateServiceFabricCluster.ps1` s hello `-FabricRuntimePackagePath` parametr zahrnuty, jak je uvedeno níže: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
kde `.\ClusterConfig.json` a `.\MicrosoftAzureServiceFabric.cab` jsou konfigurace clusteru toohello hello cesty a souboru .cab hello runtime v uvedeném pořadí.


### <a name="step-2-connect-toohello-cluster"></a>Krok 2: Připojení toohello clusteru
tooconnect tooa zabezpečení clusteru, najdete v části [Service fabric připojit toosecure cluster](service-fabric-connect-to-secure-cluster.md).

tooconnect tooan nezabezpečená clusteru, spusťte následující příkaz prostředí PowerShell hello:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
Příklad:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a>Krok 3: Zprovoznit Service Fabric Exploreru
Teď můžete připojit toohello clusteru pomocí Service Fabric Exploreru buď přímo z jednoho z počítačů hello http://localhost:19080/Explorer/index.html nebo vzdáleně s http://<*IPAddressofaMachine*>: 19080 / Explorer/index.HTML.

## <a name="add-and-remove-nodes"></a>Přidání a odebrání uzlů
Můžete přidat nebo odebrat cluster Service Fabric samostatné tooyour uzly, protože vaše obchodní potřeby změnit. V tématu [přidat nebo odebrat uzly tooa Service Fabric samostatný cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) podrobné pokyny.

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Odebrat cluster
tooremove clusteru, spusťte hello *RemoveServiceFabricCluster.ps1* skript prostředí PowerShell ze složky balíčku hello a předejte jí hello cesta toohello JSON konfigurační soubor. Volitelně můžete zadat umístění pro protokol hello hello odstranění.

Tento skript můžete spustit na jakýkoli počítač, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v souboru konfigurace clusteru hello. Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello.

```
# Removes Service Fabric from each machine in hello configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from hello current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-tooopt-out-of-it"></a>Mezi shromažďovaná telemetrická data a jak tooopt mimo ho
Ve výchozím hello produktu shromažďuje telemetrická data na hello Service Fabric využití tooimprove hello produktu. Hello Analyzátor osvědčených postupů, které běží jako součást instalace hello zkontroluje připojení příliš[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Pokud není dostupný, hello instalace se nezdaří, pokud vyjádření výslovného nesouhlasu telemetrie.

1. Hello telemetrie kanálu pokusí tooupload hello následující data příliš[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) jednou denně. Je načtení typu best effort a nemá žádný vliv na fungování clusteru hello. telemetrie Hello je odeslán, pouze z uzlu hello, spustí hello primární Správce převzetí služeb při selhání. Žádné další uzly k odeslání telemetrie.
2. telemetrie Hello se skládá z následujících hello:

* Počet služeb
* Počet ServiceTypes
* Počet aplikací
* Počet ApplicationUpgrades
* Počet FailoverUnits
* Počet InBuildFailoverUnits
* Počet UnhealthyFailoverUnits
* Počet replik
* Počet InBuildReplicas
* Počet StandByReplicas
* Počet OfflineReplicas
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Počet uzlů
* IsContextComplete: True nebo False
* ClusterId: Toto je identifikátor GUID náhodně vygenerované pro každý cluster
* ServiceFabricVersion
* IP adresa hello virtuálního počítače nebo počítače, ze které hello je načtený telemetrie

toodisable telemetrie, přidejte následující hello příliš*vlastnosti* v souboru config clusteru: *enableTelemetry: false*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Funkce Preview obsažené v tomto balíčku
Žádné.


> [!NOTE]
> Počínaje hello nové [verzí GA hello samostatné clusteru pro systém Windows Server (verze 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), toofuture verze vašeho clusteru, můžete upgradovat ručně nebo automaticky. Odkazovat příliš[upgradovat na verzi clusteru Service Fabric samostatné](service-fabric-cluster-upgrade-windows-server.md) v dokumentu.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Nasazení a odebírat aplikace pomocí prostředí PowerShell](service-fabric-deploy-remove-applications.md)
* [Nastavení konfigurace pro samostatný cluster Windows](service-fabric-cluster-manifest.md)
* [Přidat nebo odebrat cluster Service Fabric samostatné tooa uzly](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Upgrade na verzi clusteru Service Fabric samostatné](service-fabric-cluster-upgrade-windows-server.md)
* [Vytvořit cluster Service Fabric samostatné s virtuálními počítači Azure s Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
* [Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
