---
title: "aaaAzure přípravy nasazení služby prostředků infrastruktury samostatné clusteru | Microsoft Docs"
description: "Dokumentace související s toopreparing hello prostředí a vytváření hello konfiguraci clusteru, toobe považována za předchozí toodeploying určená pro zpracování produkční zatížení clusteru."
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
ms.date: 1/17/2017
ms.author: maburlik;chackdan
ms.openlocfilehash: e503c61a64b408af3f22bd75ab02f1c34ac9f380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
<a id="preparemachines"></a>

## <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Plánování a příprava nasazení samostatný cluster Service Fabric
Proveďte následující kroky předtím, než vytvoříte cluster hello.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Krok 1: Plánování infrastruktury clusteru
Chystáte se toocreate cluster Service Fabric na počítačích, které vlastníte, abyste se mohli rozhodnout, jaké druhy chyb, které chcete hello toosurvive clusteru. Například musíte samostatné power řádky nebo připojení k Internetu zadaný toothese počítače? Kromě toho zvažte hello fyzického zabezpečení těchto počítačů. Umístění hello počítače, a který potřebuje přístup toothem? Když provedete tato rozhodnutí, můžete počítače toohello hello logicky namapovat různých domén selhání (viz krok 4). plánování pro produkční clustery infrastruktury Hello je složitější než pro testovacích clusterů.

### <a name="step-2-prepare-hello-machines-toomeet-hello-prerequisites"></a>Krok 2: Příprava počítače toomeet hello hello požadavky
Předpoklady pro každý počítač, které chcete tooadd toohello clusteru:

* Doporučuje se minimálně 16 GB paměti RAM.
* Doporučuje se minimálně 40 GB volného místa na disku.
* Doporučuje se 4 jádra nebo větší využití procesoru.
* Zabezpečené sítě tooa připojení nebo sítě pro všechny počítače.
* Windows Server 2012 R2 nebo Windows Server 2016. 
* [Rozhraní .NET framework 4.5.1 nebo novější](https://www.microsoft.com/download/details.aspx?id=40773), úplné instalace.
* [Prostředí Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
* Hello [RemoteRegistry služby](https://technet.microsoft.com/library/cc754820) by však být spuštěna ve všech počítačích hello.

Hello Správce clusteru, nasazení a konfigurace clusteru hello musí mít [oprávnění správce](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) na všech počítačích hello. Service Fabric nelze nainstalovat na řadič domény.

### <a name="step-3-determine-hello-initial-cluster-size"></a>Krok 3: Určení velikosti počáteční clusteru hello
Každý uzel v clusteru Service Fabric samostatné má hello Service Fabric runtime nasadit a je členem clusteru hello. V typické provozní nasazení je jeden uzel jednu instanci OS (fyzické nebo virtuální). velikost clusteru Hello je určen podle obchodních potřeb. Ale musí mít velikost minimální clusteru tři uzly (počítačů nebo virtuálních počítačů).
Pro účely vývoje může mít více než jeden uzel v daném počítači. V produkčním prostředí Service Fabric podporuje jenom jeden uzel na fyzický nebo virtuální počítač.

### <a name="step-4-determine-hello-number-of-fault-domains-and-upgrade-domains"></a>Krok 4: Určení hello počet domén selhání a upgradu domény
A *doména selhání* (FD) je fyzická jednotka selhání a je přímo související toohello fyzické infrastruktury v datacentrech hello. Domény selhání se skládá z hardwarové součásti (počítače, přepínače, sítí a více), které sdílejí jediný bod selhání. I když neexistuje mapování 1:1 mezi domén selhání a stojany, volně platí, že každý rack lze považovat za domény selhání. Při určování hello uzlů v clusteru, důrazně doporučujeme, aby uzly hello distribuovat mezi aspoň tři domény selhání.

Když zadáte FDs v souboru ClusterConfig.json, můžete název každé FD hello. Service Fabric podporuje hierarchické FDs, takže můžete zrcadlit topologie infrastruktury v nich.  Platné jsou například hello FDs následující:

* "faultDomain": "fd: / Room1/Rack1/Počítač1"
* "faultDomain": "fd: / FD1"
* "faultDomain": "fd: / Room1/Rack1/PDU1/M1"

*Upgradovací doméně* (UD) je logickou jednotku uzlů. Během Service Fabric orchestrovat upgradů (upgrade aplikace nebo upgrade clusteru) jsou všechny uzly v UD ukončena tooperform hello upgrade dobu, uzly v jiných UDs jsou k dispozici tooserve požadavky. Hello upgrady firmwaru provést na vašich počítačích nerespektují UDs, takže je nutné je provést některou počítače najednou.

Hello nejjednodušší způsob, jak toothink o těchto postupech je tooconsider FDs jako jednotku hello neplánovaném selhání a UDs jako hello jednotku plánované údržby.

Když zadáte UDs v souboru ClusterConfig.json, můžete název každé UD hello. Platné jsou například hello následující názvy:

* "upgradeDomain": "UD0"
* "upgradeDomain": "UD1A"
* "upgradeDomain": "DomainRed"
* "upgradeDomain": "Blue"

Podrobné informace o upgradovacích domén a domén selhání, najdete v části [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-hello-service-fabric-standalone-package-for-windows-server"></a>Krok 5: Stáhněte si hello Service Fabric samostatný balíček pro systém Windows Server
[Stáhněte si odkaz – samostatný balíček prostředků infrastruktury služby - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) a rozbalte balíček hello, buď tooa nasazení počítače tedy není součástí clusteru hello nebo tooone hello počítačů, které budou součástí clusteru.

### <a name="step-6-modify-cluster-configuration"></a>Krok 6: Úprava konfigurace clusteru
toocreate samostatný cluster s podporou máte toocreate samostatné clusteru konfigurace souboru ClusterConfig.json soubor, který popisuje hello specifikace hello clusteru. Můžete základní hello konfigurační soubor pro šablony hello nalezený na hello následujícím odkazu. <br>
[Konfigurace clusteru samostatné](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Podrobnosti o hello části v tomto souboru, najdete v části [nastavení konfigurace pro samostatný cluster Windows](service-fabric-cluster-manifest.md).

Otevřete ho hello souboru ClusterConfig.json soubory z balíčku hello, kterou jste stáhli a upravte hello následující nastavení:
| **Nastavení konfigurace** | **Popis** |
| --- | --- |
| **NodeTypes** |Typy uzlů umožňují tooseparate uzly clusteru do různých skupin. Cluster musí mít alespoň jeden typ uzlu. Všechny uzly ve skupině mají hello následující běžné vlastnosti: <br> **Název** -Toto je název typu uzlu hello. <br>**Porty koncových bodů** – tyto mají různé název koncového bodu (porty), které jsou spojeny s tímto typem uzlu. Můžete použít libovolné číslo portu, který chcete, tak dlouho, dokud nedošlo ke konfliktu s nic jiného v této manifest a nejsou již používá jiná aplikace, systémem hello počítače nebo virtuálního počítače. <br> **Vlastnosti umístění** -tyto popisují vlastnosti pro tento typ uzlu, který používáte jako omezení umístění pro služby systému hello nebo vaše služby. Tyto vlastnosti jsou páry klíč – hodnota definovaný uživatelem, které poskytují další metadata pro daný uzel. Příklady vlastnosti uzlu by, jestli má uzel hello pevný disk nebo grafickou kartu, hello počet diskových jednotek v jeho pevný disk, jader a další fyzické vlastnosti. <br> **Kapacity** -uzlu kapacity definovat název hello a množství určitý prostředek, má k dispozici pro využívání konkrétní uzel. Uzlem může můžete například definovat, že má kapacitu pro metriku názvem "MemoryInMb" a že má 2 048 MB k dispozici ve výchozím nastavení. V modulu runtime tooensure, služby, které vyžadují určité množství prostředků jsou umístěny na uzlech hello, že mají tyto prostředky, které jsou k dispozici v hello požadovaná objemy používá tyto kapacity.<br>**IsPrimary** – Pokud máte více než jeden typ uzlu definována zkontrolujte, že pouze jedna je nastavená tooprimary s hodnotou hello *true*, což je, kde systém hello služeb spustit. Všechny ostatní typy uzlu musí být nastavená hodnota toohello *false* |
| **Uzly** |Toto jsou hello podrobnosti pro jednotlivé hello uzlů, které jsou součástí clusteru hello (typ uzlu, název uzlu, IP adresa, doména selhání a upgradu domény uzlu hello). Hello počítače chcete toobe hello clusteru vytvořen na toobe nutné uvedené spolu s jejich IP adresy. <br> Pokud používáte hello je vytvořena stejnou IP adresu pro všechny uzly hello a potom políčko jeden cluster, který můžete použít pro účely testování. Nepoužívejte jeden pole clustery pro nasazení v produkčním prostředí. |

Po konfiguraci clusteru hello zaznamenala prostředí toohello všechna nastavení, může být porovnávaný prostředí clusteru hello (krok 7).

<a id="environmentsetup"></a>

### <a name="step-7-environment-setup"></a>Krok 7. Nastavení prostředí

Pokud správce clusteru konfiguruje samostatný cluster Service Fabric, je hello prostředí vyžaduje toobe nastavit hello následující kritéria: <br>
1. zabezpečení na úrovni správce oprávnění tooall počítače, které jsou označeny jako uzly v souboru konfigurace clusteru hello by měl mít Hello uživatel vytvoření clusteru s podporou hello.
2. Počítač, ze které hello je vytvořen cluster, jakož i každého počítače uzlu clusteru musí být:
* Odinstalovali Service Fabric SDK
* Mít odinstalaci modulu runtime Service Fabric 
* Hello služby brány Windows Firewall (mpssvc) povolili
* Povolili hello služba Remote Registry Service (remoteregistry)
* Soubor povoleno sdílení (SMB)
* Máte nezbytné porty otevřít, založené na porty konfigurace clusteru
* Mít nezbytné porty otevřené pro službu Windows SMB a Remote Registry: 135, 137, 138, 139 a 445
* Mít síťové připojení tooone jiný
3. Žádné počítače uzlu clusteru hello by měl být řadič domény.
4. Pokud toobe clusteru hello nasazení clusteru s podporou zabezpečení, ověřte hello potřeby zabezpečení, které jsou požadavky umístěte a jsou správně nakonfigurované proti hello konfigurace.
5. Pokud počítače clusteru hello nejsou přístupné z Internetu, následující sadu hello hello konfigurace clusteru:
* Zakázat telemetrická data: v části *vlastnosti* nastavit *"enableTelemetry": false*
* Zakázat automatické stahování verze Fabric & oznámení, že hello clusteru verzí se blíží konec podpory: v části *vlastnosti* nastavit *"fabricClusterAutoupgradeEnabled": false*
* Případně pokud přístup k síti internet je omezená uvedené toowhite domén, hello domén níže jsou požadovány pro automatický upgrade: go.microsoft.com download.microsoft.com

6. Nastavte příslušné Service Fabric antivirový vyloučení:

| **Antivirový Vyloučené adresáře** |
| --- |
| Program Files\Microsoft Service Fabric |
| Proměnná FabricDataRoot (z konfigurace clusteru) |
| Adresáři FabricLogRoot (z konfigurace clusteru) |

| **Antivirový vyloučené procesy** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |

### <a name="step-8-validate-environment-using-testconfiguration-script"></a>Krok 8. Ověření pomocí skriptu TestConfiguration prostředí
Hello TestConfiguration.ps1 skriptu najdete v hello samostatný balíček. Je použít jako Analyzátor osvědčených postupů toovalidate některé z výše uvedená kritéria hello a má být použit jako toovalidate správností zkontrolujte, zda clusteru můžou být nasazené na dané prostředí. Pokud je jakákoli chyba, najdete v seznamu toohello v části [nastavení prostředí](service-fabric-cluster-standalone-deployment-preparation.md) pro řešení potíží. 

Tento skript můžete spustit na jakýkoli počítač, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v souboru konfigurace clusteru hello. Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
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

Tento modul testování konfigurace aktuálně neověřuje hello konfigurace zabezpečení, tak to má toobe provádí nezávisle.  

> [!NOTE]
> Průběžně vydáváme zlepšení toomake tento modul robustnější, takže pokud je vadný nebo chybějící písma a to budete mít dojem, není aktuálně zachytila TestConfiguration, dejte nám vědět prostřednictvím našich [podporují kanály](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).   
> 
> 

## <a name="next-steps"></a>Další kroky
* [Vytvoření clusteru s podporou samostatné systémem Windows Server](service-fabric-cluster-creation-for-windows-server.md)
