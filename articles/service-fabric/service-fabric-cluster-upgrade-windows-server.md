---
title: "Upgradovat samostatné clusteru Azure Service Fabric na Windows serveru | Microsoft Docs"
description: "Upgrade Azure Service Fabric kód nebo konfigurace, který spouští samostatný cluster Service Fabric, včetně nastavení režimu aktualizace clusteru."
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
ms.openlocfilehash: ac40775ca62362a32184207857a0b965a798e135
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a>Upgradovat samostatné Azure Service Fabric v clusteru Windows Server
> [!div class="op_single_selector"]
> * [Azure clusteru](service-fabric-cluster-upgrade.md)
> * [Samostatné clusteru](service-fabric-cluster-upgrade-windows-server.md)
>
>

Možnosti upgradu pro libovolný systém moderní je klíč pro dlouhodobý úspěch vašeho produktu. Cluster služby Azure Service Fabric je prostředek, který vlastníte. Tento článek popisuje, jak můžete zkontrolovat, že cluster vždy běží podporované verze kódu Service Fabric a konfigurace.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>Ovládací prvek verze Service Fabric, která běží na clusteru
Chcete-li nastavit cluster stahovat aktualizace Service Fabric, když společnost Microsoft vydá nové verze, nastavte **fabricClusterAutoupgradeEnabled** konfigurace clusteru na hodnotu true. Chcete-li vybrat podporovanou verzi Service Fabric, které chcete cluster na, nastavte **fabricClusterAutoupgradeEnabled** konfigurace clusteru na hodnotu false.

> [!NOTE]
> Ujistěte se, že váš cluster vždy s podporovanou verzí Service Fabric. Když Microsoft oznamuje vydání nové verze Service Fabric, předchozí verze je označena k ukončení podpory po minimálně 60 dní od data oznámení. Nové verze není zmínka [na blogu týmu Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/). Nové verze je možné zvolit v daném okamžiku.
>
>

Clusteru můžete upgradovat na novou verzi, pouze pokud používáte produkční style konfigurace uzlu, kde je každý uzel Service Fabric přidělena na samostatný fyzický nebo virtuální počítač. Pokud máte cluster vývoj, které je více než jeden uzel Service Fabric na jeden fyzický nebo virtuální počítač, budete muset znovu vytvořit cluster s novou verzí.

Dvou různých pracovních postupů můžete upgradovat clusteru na nejnovější verzi nebo podporovanou verzi Service Fabric. Jeden pracovní postup je pro clustery, které mají připojení k automaticky stáhnout nejnovější verzi. Jiné pracovní postup je pro clustery, které nemají připojení k stáhnout nejnovější verzi Service Fabric.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Upgrade clustery, které mají připojení k stáhnout nejnovější kódu a konfigurace
Tyto kroky použijte pro upgrade clusteru na podporovanou verzi, pokud uzly clusteru mít připojení k Internetu k [http://download.microsoft.com](http://download.microsoft.com).

Pro clustery, které mají připojení k [http://download.microsoft.com](http://download.microsoft.com), Microsoft pravidelně kontroluje dostupnost nové verze Service Fabric.

Pokud je dostupná nová verze Service Fabric, balíček je stažen místně do clusteru a zřídit pro upgrade. Navíc k informování zákazník tuto novou verzi, systém zobrazí upozornění stavu explicitní clusteru, který je podobný následujícímu:

"Aktuální verze [verze #] podpora clusteru končí [datum]".

Po v clusteru je spuštěn na nejnovější verzi, upozornění Vyčkat.

#### <a name="cluster-upgrade-workflow"></a>Pracovní postup upgradu clusteru
Jakmile se zobrazí upozornění na stav clusteru, postupujte takto:

1. Připojte se ke clusteru z libovolného počítače, který má práva správce pro všechny počítače, které jsou označeny jako uzly v clusteru. Na počítač, který tento skript je spuštěn na nemá být součástí clusteru.

    ```powershell

    ###### connect to the secure cluster using certs
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

2. Získáte seznam verzí Service Fabric, které můžete upgradovat.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Měli byste obdržet výstup podobná této:

    ![získat fabric verze][getfabversions]
3. Spusťte upgrade clusteru k dispozici verzi pomocí [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) prostředí PowerShell cmd.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Pokud chcete sledovat průběh upgradu, můžete použít Service Fabric Explorer nebo spuštěním následujícího příkazu prostředí Windows PowerShell.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět. K určení stavu vlastní zásady pro **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Po vyřešení problémů, jejichž výsledkem vrácení zpět zahájit upgrade znovu podle následujících kroků stejná jako bylo popsáno dříve.

### <a name="upgrade-clusters-that-have-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Upgrade clustery, které mají <U>žádná připojení</u> stáhnout nejnovější kódu a konfigurace
Tyto kroky použijte pro upgrade clusteru na podporovanou verzi, pokud uzly clusteru nemají připojení k Internetu k [http://download.microsoft.com](http://download.microsoft.com).

> [!NOTE]
> Pokud používáte cluster, který není připojený k Internetu, je nutné sledovat blog týmu Service Fabric Další informace o novou verzi. V systému nezobrazuje upozornění stavu clusteru vás upozorní na novou verzi.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatické zřizování vs ručního zřizování
Pokud chcete povolit automatické stahování a registrace na nejnovější verzi kódu, nastavení služby aktualizací prostředků infrastruktury. Odkazovat na Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt uvnitř [samostatný balíček](service-fabric-cluster-standalone-package-contents.md) pokyny.
Proces ručního nastavení postupujte podle pokynů níže.

Upravte konfiguraci clusteru a nastavte následující vlastnost na hodnotu false, před zahájením upgradu konfigurace.

        "fabricClusterAutoupgradeEnabled": false,

Odkazovat na [Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) podrobnosti o použití. Ujistěte se, že jste před zahájením upgradu configuration aktualizujte 'clusterConfigurationVersion' ve vašem formátu JSON.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Pracovní postup upgradu clusteru

1. Get-ServiceFabricClusterUpgrade spustit na jednom z uzlů v clusteru a poznamenejte si TargetCodeVersion.
2. Spusťte následující z Internetu počítače připojené k seznamu všech verzí upgrade kompatibilní s aktuální verzí a stáhněte si odpovídající balíček z odkazů přidružené ke stažení.

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Připojte se ke clusteru z libovolného počítače, který má práva správce pro všechny počítače, které jsou označeny jako uzly v clusteru. Na počítač, který tento skript je spuštěn na nemusí být součástí clusteru

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Zkopírujte stažený balíček do úložiště bitových kopií clusteru.

5. Zaregistrujte zkopírovaný balíčku.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Spusťte upgrade clusteru dostupnou verzi.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Můžete sledovat průběh upgradu v Service Fabric Exploreru, nebo můžete spustit následující příkaz prostředí PowerShell.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět. K určení stavu vlastní zásady pro **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Po vyřešení problémů, jejichž výsledkem vrácení zpět zahájit upgrade znovu podle následujících kroků stejná jako bylo popsáno dříve.


## <a name="upgrade-the-cluster-configuration"></a>Upgradovat konfiguraci clusteru
Než zahájíte upgrade konfigurace, můžete otestovat vaše nové konfigurace json clusteru spuštěním skriptu prostředí powershell v samostatné balíčku.

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
nebo

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Některé konfigurace nelze upgradovat, jako je například koncových bodů, název clusteru, uzel IP adresy, atd. Tato akce testování nového json konfigurace clusteru proti starý a throw chyby v okně prostředí Powershell, pokud je jakýkoli problém.

Chcete-li upgradovat upgrade konfiguraci clusteru, spusťte **Start-ServiceFabricClusterConfigurationUpgrade**. Konfigurace upgradu je zpracování doména upgradu podle upgradovací doméně.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Aktualizace konfigurace certifikátu clusteru  
Cluster certifikát se používá pro ověřování mezi uzly clusteru, takže vrácení certifikát přes musí provést další opatrně, protože selhání bude blokovat komunikaci mezi uzly clusteru.  
Technicky jsou podporovány tři možnosti:  

1. Jeden certifikát upgrade: způsob upgradu je ' certifikátu (primární) -> certifikát B (primární) -> C certifikátu (primární) ->...'.   
2. Double certifikátu upgrade: způsob upgradu je ' certifikátu (primární) -> certifikátu (primární) a B (sekundární) -> certifikát B (primární) -> certifikát B (primární) a C (sekundární) -> C certifikátu (primární) ->...'.
3. Certifikát typ upgradu: Konfigurace certifikátu na základě CommonName configuration <> – na základě kryptografický otisk certifikátu. Například kryptografický otisk certifikátu (primární) a kryptografický otisk B (sekundární) -> certifikát CommonName C.


## <a name="next-steps"></a>Další kroky
* Zjistěte, jak přizpůsobit některé [nastavení clusteru Service Fabric](service-fabric-cluster-fabric-settings.md).
* Zjistěte, jak [škálování vašeho clusteru a odhlašování](service-fabric-cluster-scale-up-down.md).
* Další informace o [upgradů aplikací](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
