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
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a><span data-ttu-id="836f0-103">Upgradovat samostatné Azure Service Fabric v clusteru Windows Server</span><span class="sxs-lookup"><span data-stu-id="836f0-103">Upgrade your standalone Azure Service Fabric on Windows Server cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="836f0-104">Azure clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-104">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="836f0-105">Samostatné clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-105">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
>
>

<span data-ttu-id="836f0-106">Možnosti upgradu pro libovolný systém moderní je klíč pro dlouhodobý úspěch vašeho produktu.</span><span class="sxs-lookup"><span data-stu-id="836f0-106">For any modern system, the ability to upgrade is a key to the long-term success of your product.</span></span> <span data-ttu-id="836f0-107">Cluster služby Azure Service Fabric je prostředek, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="836f0-107">An Azure Service Fabric cluster is a resource that you own.</span></span> <span data-ttu-id="836f0-108">Tento článek popisuje, jak můžete zkontrolovat, že cluster vždy běží podporované verze kódu Service Fabric a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="836f0-108">This article describes how you can make sure that the cluster always runs supported versions of Service Fabric code and configurations.</span></span>

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="836f0-109">Ovládací prvek verze Service Fabric, která běží na clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-109">Control the Service Fabric version that runs on your cluster</span></span>
<span data-ttu-id="836f0-110">Chcete-li nastavit cluster stahovat aktualizace Service Fabric, když společnost Microsoft vydá nové verze, nastavte **fabricClusterAutoupgradeEnabled** konfigurace clusteru na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="836f0-110">To set your cluster to download updates of Service Fabric when Microsoft releases a new version, set the **fabricClusterAutoupgradeEnabled** cluster configuration to true.</span></span> <span data-ttu-id="836f0-111">Chcete-li vybrat podporovanou verzi Service Fabric, které chcete cluster na, nastavte **fabricClusterAutoupgradeEnabled** konfigurace clusteru na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="836f0-111">To select a supported version of Service Fabric that you want your cluster to be on, set the **fabricClusterAutoupgradeEnabled** cluster configuration to false.</span></span>

> [!NOTE]
> <span data-ttu-id="836f0-112">Ujistěte se, že váš cluster vždy s podporovanou verzí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="836f0-112">Make sure that your cluster always runs a supported Service Fabric version.</span></span> <span data-ttu-id="836f0-113">Když Microsoft oznamuje vydání nové verze Service Fabric, předchozí verze je označena k ukončení podpory po minimálně 60 dní od data oznámení.</span><span class="sxs-lookup"><span data-stu-id="836f0-113">When Microsoft announces the release of a new version of Service Fabric, the previous version is marked for end of support after a minimum of 60 days from the date of the announcement.</span></span> <span data-ttu-id="836f0-114">Nové verze není zmínka [na blogu týmu Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="836f0-114">New releases are announced [on the Service Fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="836f0-115">Nové verze je možné zvolit v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="836f0-115">The new release is available to choose at that point.</span></span>
>
>

<span data-ttu-id="836f0-116">Clusteru můžete upgradovat na novou verzi, pouze pokud používáte produkční style konfigurace uzlu, kde je každý uzel Service Fabric přidělena na samostatný fyzický nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="836f0-116">You can upgrade your cluster to the new version only if you are using a production-style node configuration, where each Service Fabric node is allocated on a separate physical or virtual machine.</span></span> <span data-ttu-id="836f0-117">Pokud máte cluster vývoj, které je více než jeden uzel Service Fabric na jeden fyzický nebo virtuální počítač, budete muset znovu vytvořit cluster s novou verzí.</span><span class="sxs-lookup"><span data-stu-id="836f0-117">If you have a development cluster, where more than one Service Fabric node is on a single physical or virtual machine, you must re-create the cluster with the new version.</span></span>

<span data-ttu-id="836f0-118">Dvou různých pracovních postupů můžete upgradovat clusteru na nejnovější verzi nebo podporovanou verzi Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="836f0-118">Two distinct workflows can upgrade your cluster to the latest version or a supported Service Fabric version.</span></span> <span data-ttu-id="836f0-119">Jeden pracovní postup je pro clustery, které mají připojení k automaticky stáhnout nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="836f0-119">One workflow is for clusters that have connectivity to download the latest version automatically.</span></span> <span data-ttu-id="836f0-120">Jiné pracovní postup je pro clustery, které nemají připojení k stáhnout nejnovější verzi Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="836f0-120">The other workflow is for clusters that do not have connectivity to download the latest Service Fabric version.</span></span>

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a><span data-ttu-id="836f0-121">Upgrade clustery, které mají připojení k stáhnout nejnovější kódu a konfigurace</span><span class="sxs-lookup"><span data-stu-id="836f0-121">Upgrade clusters that have connectivity to download the latest code and configuration</span></span>
<span data-ttu-id="836f0-122">Tyto kroky použijte pro upgrade clusteru na podporovanou verzi, pokud uzly clusteru mít připojení k Internetu k [http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="836f0-122">Use these steps to upgrade your cluster to a supported version if your cluster nodes have Internet connectivity to [http://download.microsoft.com](http://download.microsoft.com).</span></span>

<span data-ttu-id="836f0-123">Pro clustery, které mají připojení k [http://download.microsoft.com](http://download.microsoft.com), Microsoft pravidelně kontroluje dostupnost nové verze Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="836f0-123">For clusters that have connectivity to [http://download.microsoft.com](http://download.microsoft.com), Microsoft periodically checks for the availability of new Service Fabric versions.</span></span>

<span data-ttu-id="836f0-124">Pokud je dostupná nová verze Service Fabric, balíček je stažen místně do clusteru a zřídit pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="836f0-124">When a new Service Fabric version is available, the package is downloaded locally to the cluster and provisioned for upgrade.</span></span> <span data-ttu-id="836f0-125">Navíc k informování zákazník tuto novou verzi, systém zobrazí upozornění stavu explicitní clusteru, který je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="836f0-125">Additionally, to inform the customer of this new version, the system shows an explicit cluster health warning that's similar to the following:</span></span>

<span data-ttu-id="836f0-126">"Aktuální verze [verze #] podpora clusteru končí [datum]".</span><span class="sxs-lookup"><span data-stu-id="836f0-126">“The current cluster version [version#] support ends [Date]."</span></span>

<span data-ttu-id="836f0-127">Po v clusteru je spuštěn na nejnovější verzi, upozornění Vyčkat.</span><span class="sxs-lookup"><span data-stu-id="836f0-127">After the cluster is running the latest version, the warning goes away.</span></span>

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="836f0-128">Pracovní postup upgradu clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-128">Cluster upgrade workflow</span></span>
<span data-ttu-id="836f0-129">Jakmile se zobrazí upozornění na stav clusteru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="836f0-129">After you see the cluster health warning, do the following:</span></span>

1. <span data-ttu-id="836f0-130">Připojte se ke clusteru z libovolného počítače, který má práva správce pro všechny počítače, které jsou označeny jako uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="836f0-130">Connect to the cluster from any machine that has administrator access to all the machines that are listed as nodes in the cluster.</span></span> <span data-ttu-id="836f0-131">Na počítač, který tento skript je spuštěn na nemá být součástí clusteru.</span><span class="sxs-lookup"><span data-stu-id="836f0-131">The machine that this script is run on does not have to be part of the cluster.</span></span>

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

2. <span data-ttu-id="836f0-132">Získáte seznam verzí Service Fabric, které můžete upgradovat.</span><span class="sxs-lookup"><span data-stu-id="836f0-132">Get the list of Service Fabric versions that you can upgrade to.</span></span>

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    <span data-ttu-id="836f0-133">Měli byste obdržet výstup podobná této:</span><span class="sxs-lookup"><span data-stu-id="836f0-133">You should get an output similar to this:</span></span>

    ![získat fabric verze][getfabversions]
3. <span data-ttu-id="836f0-135">Spusťte upgrade clusteru k dispozici verzi pomocí [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) prostředí PowerShell cmd.</span><span class="sxs-lookup"><span data-stu-id="836f0-135">Start a cluster upgrade to an available version by using the [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="836f0-136">Pokud chcete sledovat průběh upgradu, můžete použít Service Fabric Explorer nebo spuštěním následujícího příkazu prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="836f0-136">To monitor the progress of the upgrade, you can use Service Fabric Explorer or run the following Windows PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="836f0-137">Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="836f0-137">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="836f0-138">K určení stavu vlastní zásady pro **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="836f0-138">To specify custom health policies for the **Start-ServiceFabricClusterUpgrade** command, see documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="836f0-139">Po vyřešení problémů, jejichž výsledkem vrácení zpět zahájit upgrade znovu podle následujících kroků stejná jako bylo popsáno dříve.</span><span class="sxs-lookup"><span data-stu-id="836f0-139">After you fix the issues that resulted in the rollback, initiate the upgrade again by following the same steps as previously described.</span></span>

### <a name="upgrade-clusters-that-have-uno-connectivityu-to-download-the-latest-code-and-configuration"></a><span data-ttu-id="836f0-140">Upgrade clustery, které mají <U>žádná připojení</u> stáhnout nejnovější kódu a konfigurace</span><span class="sxs-lookup"><span data-stu-id="836f0-140">Upgrade clusters that have <U>no connectivity</u> to download the latest code and configuration</span></span>
<span data-ttu-id="836f0-141">Tyto kroky použijte pro upgrade clusteru na podporovanou verzi, pokud uzly clusteru nemají připojení k Internetu k [http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="836f0-141">Use these steps to upgrade your cluster to a supported version if your cluster nodes do not have Internet connectivity to [http://download.microsoft.com](http://download.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="836f0-142">Pokud používáte cluster, který není připojený k Internetu, je nutné sledovat blog týmu Service Fabric Další informace o novou verzi.</span><span class="sxs-lookup"><span data-stu-id="836f0-142">If you are running a cluster that is not connected to the Internet, you will have to monitor the Service Fabric team blog to learn about a new release.</span></span> <span data-ttu-id="836f0-143">V systému nezobrazuje upozornění stavu clusteru vás upozorní na novou verzi.</span><span class="sxs-lookup"><span data-stu-id="836f0-143">The system does not show a cluster health warning to alert you of a new release.</span></span>  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a><span data-ttu-id="836f0-144">Automatické zřizování vs ručního zřizování</span><span class="sxs-lookup"><span data-stu-id="836f0-144">Auto Provisioning vs Manual Provisioning</span></span>
<span data-ttu-id="836f0-145">Pokud chcete povolit automatické stahování a registrace na nejnovější verzi kódu, nastavení služby aktualizací prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="836f0-145">To enable automatic downloading and registration for the latest code version, set up Service Fabric Update Service.</span></span> <span data-ttu-id="836f0-146">Odkazovat na Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt uvnitř [samostatný balíček](service-fabric-cluster-standalone-package-contents.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="836f0-146">Refer to the Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inside the [Standalone Package](service-fabric-cluster-standalone-package-contents.md) for instructions.</span></span>
<span data-ttu-id="836f0-147">Proces ručního nastavení postupujte podle pokynů níže.</span><span class="sxs-lookup"><span data-stu-id="836f0-147">For manual process, follow the instructions below.</span></span>

<span data-ttu-id="836f0-148">Upravte konfiguraci clusteru a nastavte následující vlastnost na hodnotu false, před zahájením upgradu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="836f0-148">Modify your cluster configuration to set the following property to false before you start a configuration upgrade.</span></span>

        "fabricClusterAutoupgradeEnabled": false,

<span data-ttu-id="836f0-149">Odkazovat na [Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) podrobnosti o použití.</span><span class="sxs-lookup"><span data-stu-id="836f0-149">Refer to [Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) for usage details.</span></span> <span data-ttu-id="836f0-150">Ujistěte se, že jste před zahájením upgradu configuration aktualizujte 'clusterConfigurationVersion' ve vašem formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="836f0-150">Make sure to update 'clusterConfigurationVersion' in your JSON before you start the configuration upgrade.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="836f0-151">Pracovní postup upgradu clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-151">Cluster upgrade workflow</span></span>

1. <span data-ttu-id="836f0-152">Get-ServiceFabricClusterUpgrade spustit na jednom z uzlů v clusteru a poznamenejte si TargetCodeVersion.</span><span class="sxs-lookup"><span data-stu-id="836f0-152">Run Get-ServiceFabricClusterUpgrade from one of the nodes in the cluster and note the TargetCodeVersion.</span></span>
2. <span data-ttu-id="836f0-153">Spusťte následující z Internetu počítače připojené k seznamu všech verzí upgrade kompatibilní s aktuální verzí a stáhněte si odpovídající balíček z odkazů přidružené ke stažení.</span><span class="sxs-lookup"><span data-stu-id="836f0-153">Run the following from an internet connected machine to list all upgrade compatible versions with the current version and download the corresponding package from the associated download links.</span></span>

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. <span data-ttu-id="836f0-154">Připojte se ke clusteru z libovolného počítače, který má práva správce pro všechny počítače, které jsou označeny jako uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="836f0-154">Connect to the cluster from any machine that has administrator access to all the machines that are listed as nodes in the cluster.</span></span> <span data-ttu-id="836f0-155">Na počítač, který tento skript je spuštěn na nemusí být součástí clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-155">The machine that this script is run on does not have to be part of the cluster</span></span>

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. <span data-ttu-id="836f0-156">Zkopírujte stažený balíček do úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="836f0-156">Copy the downloaded package into the cluster image store.</span></span>

5. <span data-ttu-id="836f0-157">Zaregistrujte zkopírovaný balíčku.</span><span class="sxs-lookup"><span data-stu-id="836f0-157">Register the copied package.</span></span>

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. <span data-ttu-id="836f0-158">Spusťte upgrade clusteru dostupnou verzi.</span><span class="sxs-lookup"><span data-stu-id="836f0-158">Start a cluster upgrade to an available version.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="836f0-159">Můžete sledovat průběh upgradu v Service Fabric Exploreru, nebo můžete spustit následující příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="836f0-159">You can monitor the progress of the upgrade on Service Fabric Explorer, or you can run the following PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="836f0-160">Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="836f0-160">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="836f0-161">K určení stavu vlastní zásady pro **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="836f0-161">To specify custom health policies for the **Start-ServiceFabricClusterUpgrade** command, see the documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="836f0-162">Po vyřešení problémů, jejichž výsledkem vrácení zpět zahájit upgrade znovu podle následujících kroků stejná jako bylo popsáno dříve.</span><span class="sxs-lookup"><span data-stu-id="836f0-162">After you fix the issues that resulted in the rollback, initiate the upgrade again by following the same steps as previously described.</span></span>


## <a name="upgrade-the-cluster-configuration"></a><span data-ttu-id="836f0-163">Upgradovat konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-163">Upgrade the cluster configuration</span></span>
<span data-ttu-id="836f0-164">Než zahájíte upgrade konfigurace, můžete otestovat vaše nové konfigurace json clusteru spuštěním skriptu prostředí powershell v samostatné balíčku.</span><span class="sxs-lookup"><span data-stu-id="836f0-164">Before you initiate the configuration upgrade, you can test your new cluster configuration json by running the powershell script in the standalone package.</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
<span data-ttu-id="836f0-165">nebo</span><span class="sxs-lookup"><span data-stu-id="836f0-165">or</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

<span data-ttu-id="836f0-166">Některé konfigurace nelze upgradovat, jako je například koncových bodů, název clusteru, uzel IP adresy, atd. Tato akce testování nového json konfigurace clusteru proti starý a throw chyby v okně prostředí Powershell, pokud je jakýkoli problém.</span><span class="sxs-lookup"><span data-stu-id="836f0-166">Some configurations can't be upgraded, such as endpoints, cluster name, node IP, etc. This will test the new cluster configuration json against the old one and throw errors in the Powershell window if there is any issue.</span></span>

<span data-ttu-id="836f0-167">Chcete-li upgradovat upgrade konfiguraci clusteru, spusťte **Start-ServiceFabricClusterConfigurationUpgrade**.</span><span class="sxs-lookup"><span data-stu-id="836f0-167">To upgrade the cluster configuration upgrade, run **Start-ServiceFabricClusterConfigurationUpgrade**.</span></span> <span data-ttu-id="836f0-168">Konfigurace upgradu je zpracování doména upgradu podle upgradovací doméně.</span><span class="sxs-lookup"><span data-stu-id="836f0-168">The configuration upgrade is processed upgrade domain by upgrade domain.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a><span data-ttu-id="836f0-169">Aktualizace konfigurace certifikátu clusteru</span><span class="sxs-lookup"><span data-stu-id="836f0-169">Cluster certificate config upgrade</span></span>  
<span data-ttu-id="836f0-170">Cluster certifikát se používá pro ověřování mezi uzly clusteru, takže vrácení certifikát přes musí provést další opatrně, protože selhání bude blokovat komunikaci mezi uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="836f0-170">Cluster certificate is used for authentication between cluster nodes, so the certificate roll over should be performed with extra caution because failure will block the communication among cluster nodes.</span></span>  
<span data-ttu-id="836f0-171">Technicky jsou podporovány tři možnosti:</span><span class="sxs-lookup"><span data-stu-id="836f0-171">Technically, three options are supported:</span></span>  

1. <span data-ttu-id="836f0-172">Jeden certifikát upgrade: způsob upgradu je ' certifikátu (primární) -> certifikát B (primární) -> C certifikátu (primární) ->...'.</span><span class="sxs-lookup"><span data-stu-id="836f0-172">Single certificate upgrade: The upgrade path is 'Certificate A (Primary) -> Certificate B (Primary) -> Certificate C (Primary) -> ...'.</span></span>   
2. <span data-ttu-id="836f0-173">Double certifikátu upgrade: způsob upgradu je ' certifikátu (primární) -> certifikátu (primární) a B (sekundární) -> certifikát B (primární) -> certifikát B (primární) a C (sekundární) -> C certifikátu (primární) ->...'.</span><span class="sxs-lookup"><span data-stu-id="836f0-173">Double certificate upgrade: The upgrade path is 'Certificate A (Primary) -> Certificate A (Primary) and B (Secondary) -> Certificate B (Primary) -> Certificate B (Primary) and C (Secondary) -> Certificate C (Primary) -> ...'.</span></span>
3. <span data-ttu-id="836f0-174">Certifikát typ upgradu: Konfigurace certifikátu na základě CommonName configuration <> – na základě kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="836f0-174">Certificate type upgrade: Thumbprint-based certificate configuration <-> CommonName-based certificate configuration.</span></span> <span data-ttu-id="836f0-175">Například kryptografický otisk certifikátu (primární) a kryptografický otisk B (sekundární) -> certifikát CommonName C.</span><span class="sxs-lookup"><span data-stu-id="836f0-175">For example, Certificate Thumbprint A (Primary) and Thumbprint B (Secondary) -> Certificate CommonName C.</span></span>


## <a name="next-steps"></a><span data-ttu-id="836f0-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="836f0-176">Next steps</span></span>
* <span data-ttu-id="836f0-177">Zjistěte, jak přizpůsobit některé [nastavení clusteru Service Fabric](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="836f0-177">Learn how to customize some [Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>
* <span data-ttu-id="836f0-178">Zjistěte, jak [škálování vašeho clusteru a odhlašování](service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="836f0-178">Learn how to [scale your cluster in and out](service-fabric-cluster-scale-up-down.md).</span></span>
* <span data-ttu-id="836f0-179">Další informace o [upgradů aplikací](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="836f0-179">Learn about [application upgrades](service-fabric-application-upgrade.md).</span></span>

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
