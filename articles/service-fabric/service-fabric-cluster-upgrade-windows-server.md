---
title: aaaUpgrade samostatnou Azure Service Fabric clusteru na Windows serveru | Microsoft Docs
description: "Upgrade hello Azure Service Fabric kód nebo konfigurace, který spouští samostatný cluster Service Fabric, včetně nastavení režimu aktualizace clusteru hello."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 5132795e544b6f0185accedbf5092dcaafd66df0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a>Upgradovat samostatné Azure Service Fabric v clusteru Windows Server
> [!div class="op_single_selector"]
> * [Azure clusteru](service-fabric-cluster-upgrade.md)
> * [Samostatné clusteru](service-fabric-cluster-upgrade-windows-server.md)
>
>

Pro všechny moderní systém je možnost tooupgrade hello dlouhodobé úspěšné klíče toohello produktu. Cluster služby Azure Service Fabric je prostředek, který vlastníte. Tento článek popisuje, jak zajistit, že tento cluster hello vždy běží podporované verze kódu Service Fabric a konfigurace.

## <a name="control-hello-service-fabric-version-that-runs-on-your-cluster"></a>Verze Service Fabric hello ovládacího prvku, který běží na clusteru
tooset aktualizuje vaše toodownload clusteru Service Fabric, když společnost Microsoft vydá novou verzi sady hello **fabricClusterAutoupgradeEnabled** tootrue konfigurace clusteru. tooselect podporovanou verzi Service Fabric, které chcete toobe vašeho clusteru na sadu hello **fabricClusterAutoupgradeEnabled** toofalse konfigurace clusteru.

> [!NOTE]
> Ujistěte se, že váš cluster vždy s podporovanou verzí Service Fabric. Když Microsoft oznamuje hello vydání nové verze aplikace Service Fabric, hello předchozí verze budou označena k ukončení podpory po minimálně 60 dní od hello datum hello oznámení. Nové verze není zmínka [na blogu týmu Service Fabric hello](https://blogs.msdn.microsoft.com/azureservicefabric/). v tomto okamžiku je k dispozici toochoose Hello nová verze.
>
>

Nová verze vašeho clusteru toohello můžete upgradovat pouze v případě, že používáte konfigurace uzlu produkční stylu, kde je každý uzel Service Fabric přidělena na samostatný fyzický nebo virtuální počítač. Pokud máte cluster vývoj, které je více než jeden uzel Service Fabric na jeden fyzický nebo virtuální počítač, budete muset znovu vytvořit cluster hello s hello novou verzi.

Dvě odlišné pracovních postupů můžete upgradovat vašeho clusteru toohello nejnovější verze nebo podporovanou verzi Service Fabric. Jeden pracovní postup je pro clustery, které mají automaticky připojení toodownload hello nejnovější verzi. Hello jiné pracovní postup je pro clustery, které nemají připojení toodownload hello nejnovější Service Fabric verzi.

### <a name="upgrade-clusters-that-have-connectivity-toodownload-hello-latest-code-and-configuration"></a>Upgrade clustery, které mají připojení toodownload hello nejnovější kódu a konfigurace
Použijte tyto kroky tooupgrade tooa podporované verze vašeho clusteru, pokud uzly clusteru mít připojení k Internetu příliš[http://download.microsoft.com](http://download.microsoft.com).

Pro clustery, které máte připojení k příliš[http://download.microsoft.com](http://download.microsoft.com), Microsoft pravidelně zjišťuje, zda text hello dostupnost nové verze Service Fabric.

Pokud je dostupná nová verze Service Fabric, hello stažení balíčku místně toohello clusteru a zřízené pro upgrade. Kromě toho tooinform hello zákazník tuto novou verzi, hello systému zobrazuje upozornění stavu explicitní clusteru, který je podobný toohello následující:

"hello aktuální podpora clusteru verze [verze #] skončí [datum]".

Po hello clusteru je spuštěn hello nejnovější verzi, hello upozornění Vyčkat.

#### <a name="cluster-upgrade-workflow"></a>Pracovní postup upgradu clusteru
Jakmile se zobrazí hello clusteru stavu upozornění, hello následující:

1. Připojte toohello cluster z libovolného počítače, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v clusteru hello. Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello.

    ```powershell

    ###### connect toohello secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Získáte hello seznam verzí Service Fabric, které můžete upgradovat.

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Měli byste obdržet podobné toothis výstupu:

    ![získat fabric verze][getfabversions]
3. Spusťte verzi k dispozici upgrade tooan clusteru pomocí [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) cmd. prostředí PowerShell

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Postup hello toomonitor hello upgradu, můžete použít Service Fabric Explorer nebo spuštění hello následující příkaz prostředí Windows PowerShell.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. toospecify stavu vlastní zásady pro hello **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Po opravě hello problémy, které výsledkem hello vrácení inicializujte následující hello stejný postup jako výše popsané hello upgrade znovu.

### <a name="upgrade-clusters-that-have-uno-connectivityu-toodownload-hello-latest-code-and-configuration"></a>Upgrade clustery, které mají <U>žádná připojení</u> toodownload hello nejnovější kódu a konfigurace
Použijte tyto kroky tooupgrade tooa podporované verze vašeho clusteru, pokud uzly clusteru nemají připojení k Internetu příliš[http://download.microsoft.com](http://download.microsoft.com).

> [!NOTE]
> Pokud používáte cluster, který není připojený toohello Internet, budete mít toomonitor hello Service Fabric team blog toolearn o novou verzi. Hello systému nezobrazuje tooalert upozornění stavu clusteru můžete novou verzi.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatické zřizování vs ručního zřizování
tooenable automatické stahování a registrace pro hello nejnovější verze kódu, nastavení služby aktualizací prostředků infrastruktury. Odkazovat toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt uvnitř hello [samostatný balíček](service-fabric-cluster-standalone-package-contents.md) pokyny.
Proces ručního nastavení postupujte podle pokynů hello.

Upravte hello tooset konfigurace vašeho clusteru následující vlastnost toofalse před zahájením upgradu konfigurace.

        "fabricClusterAutoupgradeEnabled": false,

Odkazovat příliš[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) podrobnosti o použití. Zkontrolujte zda tooupdate 'clusterConfigurationVersion' v vaše struktury JSON před zahájením upgradu konfigurace hello.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

#### <a name="cluster-upgrade-workflow"></a>Pracovní postup upgradu clusteru

1. Get-ServiceFabricClusterUpgrade spustit na jednom z hello uzlů v clusteru hello a poznamenejte si hello TargetCodeVersion.
2. Spuštění hello následující z toolist počítač připojený Internetu všechny upgrade verze kompatibilní s aktuální verzí hello a stáhněte si odpovídající balíček z odkazů přidružené ke stažení hello hello.

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Připojte toohello cluster z libovolného počítače, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v clusteru hello. Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello

    ```powershell

   ###### Get hello list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file including hello path tooit> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Zkopírujte balíček hello stáhli do úložiště bitových kopií clusteru hello.

5. Zaregistrujte hello zkopírovat balíček.

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Spusťte verzi k dispozici upgrade tooan clusteru.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru, nebo můžete spustit následující příkaz prostředí PowerShell hello.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. toospecify stavu vlastní zásady pro hello **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k hello [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Po opravě hello problémy, které výsledkem hello vrácení inicializujte následující hello stejný postup jako výše popsané hello upgrade znovu.


## <a name="upgrade-hello-cluster-configuration"></a>Upgradovat konfiguraci clusteru hello
Před spuštěním upgradu hello konfigurace, můžete otestovat vaše nové konfigurace json clusteru spuštěním skriptu prostředí powershell hello v balíčku samostatné hello.

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File>

```
nebo

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File> -FabricRuntimePackagePath <Path toohello .cab file which you want tootest hello configuration against>

```

Některé konfigurace nelze upgradovat, jako je například koncových bodů, název clusteru, uzel IP adresy, atd. Tato akce testování hello nového clusteru konfigurace json proti hello starý a throw chyby v okně Powershell text hello, pokud je jakýkoli problém.

tooupgrade hello clusteru konfigurace upgradu, spusťte **Start-ServiceFabricClusterConfigurationUpgrade**. zpracovaná upgradovací doméně podle upgradu domény je upgrade konfigurace Hello.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Aktualizace konfigurace certifikátu clusteru  
Cluster certifikát se používá k ověřování mezi uzly clusteru, tak hello certifikát mění, musejí být prováděny s další upozornění protože selhání bude blokovat hello komunikaci mezi uzly clusteru.  
Technicky jsou podporovány tři možnosti:  

1. Jeden certifikát upgrade: hello upgradu je ' certifikátu (primární) -> certifikát B (primární) -> C certifikátu (primární) ->...'.   
2. Double certifikátu upgrade: hello upgradu je ' certifikátu (primární) -> certifikátu (primární) a B (sekundární) -> certifikát B (primární) -> certifikát B (primární) a C (sekundární) -> C certifikátu (primární) ->...'.
3. Certifikát typ upgradu: Konfigurace certifikátu na základě CommonName configuration <> – na základě kryptografický otisk certifikátu. Například kryptografický otisk certifikátu (primární) a kryptografický otisk B (sekundární) -> certifikát CommonName C.


## <a name="next-steps"></a>Další kroky
* Zjistěte, jak toocustomize některé [nastavení clusteru Service Fabric](service-fabric-cluster-fabric-settings.md).
* Zjistěte, jak příliš[škálování vašeho clusteru a odhlašování](service-fabric-cluster-scale-up-down.md).
* Další informace o [upgradů aplikací](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
