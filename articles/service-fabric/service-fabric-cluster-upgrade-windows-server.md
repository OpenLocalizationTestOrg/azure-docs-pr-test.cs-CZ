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
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a><span data-ttu-id="fdc88-103">Upgradovat samostatné Azure Service Fabric v clusteru Windows Server</span><span class="sxs-lookup"><span data-stu-id="fdc88-103">Upgrade your standalone Azure Service Fabric on Windows Server cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fdc88-104">Azure clusteru</span><span class="sxs-lookup"><span data-stu-id="fdc88-104">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="fdc88-105">Samostatné clusteru</span><span class="sxs-lookup"><span data-stu-id="fdc88-105">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
>
>

<span data-ttu-id="fdc88-106">Pro všechny moderní systém je možnost tooupgrade hello dlouhodobé úspěšné klíče toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="fdc88-106">For any modern system, hello ability tooupgrade is a key toohello long-term success of your product.</span></span> <span data-ttu-id="fdc88-107">Cluster služby Azure Service Fabric je prostředek, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="fdc88-107">An Azure Service Fabric cluster is a resource that you own.</span></span> <span data-ttu-id="fdc88-108">Tento článek popisuje, jak zajistit, že tento cluster hello vždy běží podporované verze kódu Service Fabric a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fdc88-108">This article describes how you can make sure that hello cluster always runs supported versions of Service Fabric code and configurations.</span></span>

## <a name="control-hello-service-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="fdc88-109">Verze Service Fabric hello ovládacího prvku, který běží na clusteru</span><span class="sxs-lookup"><span data-stu-id="fdc88-109">Control hello Service Fabric version that runs on your cluster</span></span>
<span data-ttu-id="fdc88-110">tooset aktualizuje vaše toodownload clusteru Service Fabric, když společnost Microsoft vydá novou verzi sady hello **fabricClusterAutoupgradeEnabled** tootrue konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="fdc88-110">tooset your cluster toodownload updates of Service Fabric when Microsoft releases a new version, set hello **fabricClusterAutoupgradeEnabled** cluster configuration tootrue.</span></span> <span data-ttu-id="fdc88-111">tooselect podporovanou verzi Service Fabric, které chcete toobe vašeho clusteru na sadu hello **fabricClusterAutoupgradeEnabled** toofalse konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="fdc88-111">tooselect a supported version of Service Fabric that you want your cluster toobe on, set hello **fabricClusterAutoupgradeEnabled** cluster configuration toofalse.</span></span>

> [!NOTE]
> <span data-ttu-id="fdc88-112">Ujistěte se, že váš cluster vždy s podporovanou verzí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="fdc88-112">Make sure that your cluster always runs a supported Service Fabric version.</span></span> <span data-ttu-id="fdc88-113">Když Microsoft oznamuje hello vydání nové verze aplikace Service Fabric, hello předchozí verze budou označena k ukončení podpory po minimálně 60 dní od hello datum hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="fdc88-113">When Microsoft announces hello release of a new version of Service Fabric, hello previous version is marked for end of support after a minimum of 60 days from hello date of hello announcement.</span></span> <span data-ttu-id="fdc88-114">Nové verze není zmínka [na blogu týmu Service Fabric hello](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="fdc88-114">New releases are announced [on hello Service Fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="fdc88-115">v tomto okamžiku je k dispozici toochoose Hello nová verze.</span><span class="sxs-lookup"><span data-stu-id="fdc88-115">hello new release is available toochoose at that point.</span></span>
>
>

<span data-ttu-id="fdc88-116">Nová verze vašeho clusteru toohello můžete upgradovat pouze v případě, že používáte konfigurace uzlu produkční stylu, kde je každý uzel Service Fabric přidělena na samostatný fyzický nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fdc88-116">You can upgrade your cluster toohello new version only if you are using a production-style node configuration, where each Service Fabric node is allocated on a separate physical or virtual machine.</span></span> <span data-ttu-id="fdc88-117">Pokud máte cluster vývoj, které je více než jeden uzel Service Fabric na jeden fyzický nebo virtuální počítač, budete muset znovu vytvořit cluster hello s hello novou verzi.</span><span class="sxs-lookup"><span data-stu-id="fdc88-117">If you have a development cluster, where more than one Service Fabric node is on a single physical or virtual machine, you must re-create hello cluster with hello new version.</span></span>

<span data-ttu-id="fdc88-118">Dvě odlišné pracovních postupů můžete upgradovat vašeho clusteru toohello nejnovější verze nebo podporovanou verzi Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="fdc88-118">Two distinct workflows can upgrade your cluster toohello latest version or a supported Service Fabric version.</span></span> <span data-ttu-id="fdc88-119">Jeden pracovní postup je pro clustery, které mají automaticky připojení toodownload hello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="fdc88-119">One workflow is for clusters that have connectivity toodownload hello latest version automatically.</span></span> <span data-ttu-id="fdc88-120">Hello jiné pracovní postup je pro clustery, které nemají připojení toodownload hello nejnovější Service Fabric verzi.</span><span class="sxs-lookup"><span data-stu-id="fdc88-120">hello other workflow is for clusters that do not have connectivity toodownload hello latest Service Fabric version.</span></span>

### <a name="upgrade-clusters-that-have-connectivity-toodownload-hello-latest-code-and-configuration"></a><span data-ttu-id="fdc88-121">Upgrade clustery, které mají připojení toodownload hello nejnovější kódu a konfigurace</span><span class="sxs-lookup"><span data-stu-id="fdc88-121">Upgrade clusters that have connectivity toodownload hello latest code and configuration</span></span>
<span data-ttu-id="fdc88-122">Použijte tyto kroky tooupgrade tooa podporované verze vašeho clusteru, pokud uzly clusteru mít připojení k Internetu příliš[http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="fdc88-122">Use these steps tooupgrade your cluster tooa supported version if your cluster nodes have Internet connectivity too[http://download.microsoft.com](http://download.microsoft.com).</span></span>

<span data-ttu-id="fdc88-123">Pro clustery, které máte připojení k příliš[http://download.microsoft.com](http://download.microsoft.com), Microsoft pravidelně zjišťuje, zda text hello dostupnost nové verze Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="fdc88-123">For clusters that have connectivity too[http://download.microsoft.com](http://download.microsoft.com), Microsoft periodically checks for hello availability of new Service Fabric versions.</span></span>

<span data-ttu-id="fdc88-124">Pokud je dostupná nová verze Service Fabric, hello stažení balíčku místně toohello clusteru a zřízené pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="fdc88-124">When a new Service Fabric version is available, hello package is downloaded locally toohello cluster and provisioned for upgrade.</span></span> <span data-ttu-id="fdc88-125">Kromě toho tooinform hello zákazník tuto novou verzi, hello systému zobrazuje upozornění stavu explicitní clusteru, který je podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="fdc88-125">Additionally, tooinform hello customer of this new version, hello system shows an explicit cluster health warning that's similar toohello following:</span></span>

<span data-ttu-id="fdc88-126">"hello aktuální podpora clusteru verze [verze #] skončí [datum]".</span><span class="sxs-lookup"><span data-stu-id="fdc88-126">“hello current cluster version [version#] support ends [Date]."</span></span>

<span data-ttu-id="fdc88-127">Po hello clusteru je spuštěn hello nejnovější verzi, hello upozornění Vyčkat.</span><span class="sxs-lookup"><span data-stu-id="fdc88-127">After hello cluster is running hello latest version, hello warning goes away.</span></span>

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="fdc88-128">Pracovní postup upgradu clusteru</span><span class="sxs-lookup"><span data-stu-id="fdc88-128">Cluster upgrade workflow</span></span>
<span data-ttu-id="fdc88-129">Jakmile se zobrazí hello clusteru stavu upozornění, hello následující:</span><span class="sxs-lookup"><span data-stu-id="fdc88-129">After you see hello cluster health warning, do hello following:</span></span>

1. <span data-ttu-id="fdc88-130">Připojte toohello cluster z libovolného počítače, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-130">Connect toohello cluster from any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster.</span></span> <span data-ttu-id="fdc88-131">Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-131">hello machine that this script is run on does not have toobe part of hello cluster.</span></span>

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

2. <span data-ttu-id="fdc88-132">Získáte hello seznam verzí Service Fabric, které můžete upgradovat.</span><span class="sxs-lookup"><span data-stu-id="fdc88-132">Get hello list of Service Fabric versions that you can upgrade to.</span></span>

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    <span data-ttu-id="fdc88-133">Měli byste obdržet podobné toothis výstupu:</span><span class="sxs-lookup"><span data-stu-id="fdc88-133">You should get an output similar toothis:</span></span>

    ![získat fabric verze][getfabversions]
3. <span data-ttu-id="fdc88-135">Spusťte verzi k dispozici upgrade tooan clusteru pomocí [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) cmd. prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc88-135">Start a cluster upgrade tooan available version by using the [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="fdc88-136">Postup hello toomonitor hello upgradu, můžete použít Service Fabric Explorer nebo spuštění hello následující příkaz prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fdc88-136">toomonitor hello progress of hello upgrade, you can use Service Fabric Explorer or run hello following Windows PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="fdc88-137">Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="fdc88-137">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="fdc88-138">toospecify stavu vlastní zásady pro hello **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="fdc88-138">toospecify custom health policies for hello **Start-ServiceFabricClusterUpgrade** command, see documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="fdc88-139">Po opravě hello problémy, které výsledkem hello vrácení inicializujte následující hello stejný postup jako výše popsané hello upgrade znovu.</span><span class="sxs-lookup"><span data-stu-id="fdc88-139">After you fix hello issues that resulted in hello rollback, initiate hello upgrade again by following hello same steps as previously described.</span></span>

### <a name="upgrade-clusters-that-have-uno-connectivityu-toodownload-hello-latest-code-and-configuration"></a><span data-ttu-id="fdc88-140">Upgrade clustery, které mají <U>žádná připojení</u> toodownload hello nejnovější kódu a konfigurace</span><span class="sxs-lookup"><span data-stu-id="fdc88-140">Upgrade clusters that have <U>no connectivity</u> toodownload hello latest code and configuration</span></span>
<span data-ttu-id="fdc88-141">Použijte tyto kroky tooupgrade tooa podporované verze vašeho clusteru, pokud uzly clusteru nemají připojení k Internetu příliš[http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="fdc88-141">Use these steps tooupgrade your cluster tooa supported version if your cluster nodes do not have Internet connectivity too[http://download.microsoft.com](http://download.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="fdc88-142">Pokud používáte cluster, který není připojený toohello Internet, budete mít toomonitor hello Service Fabric team blog toolearn o novou verzi.</span><span class="sxs-lookup"><span data-stu-id="fdc88-142">If you are running a cluster that is not connected toohello Internet, you will have toomonitor hello Service Fabric team blog toolearn about a new release.</span></span> <span data-ttu-id="fdc88-143">Hello systému nezobrazuje tooalert upozornění stavu clusteru můžete novou verzi.</span><span class="sxs-lookup"><span data-stu-id="fdc88-143">hello system does not show a cluster health warning tooalert you of a new release.</span></span>  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a><span data-ttu-id="fdc88-144">Automatické zřizování vs ručního zřizování</span><span class="sxs-lookup"><span data-stu-id="fdc88-144">Auto Provisioning vs Manual Provisioning</span></span>
<span data-ttu-id="fdc88-145">tooenable automatické stahování a registrace pro hello nejnovější verze kódu, nastavení služby aktualizací prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="fdc88-145">tooenable automatic downloading and registration for hello latest code version, set up Service Fabric Update Service.</span></span> <span data-ttu-id="fdc88-146">Odkazovat toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt uvnitř hello [samostatný balíček](service-fabric-cluster-standalone-package-contents.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="fdc88-146">Refer toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inside hello [Standalone Package](service-fabric-cluster-standalone-package-contents.md) for instructions.</span></span>
<span data-ttu-id="fdc88-147">Proces ručního nastavení postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-147">For manual process, follow hello instructions below.</span></span>

<span data-ttu-id="fdc88-148">Upravte hello tooset konfigurace vašeho clusteru následující vlastnost toofalse před zahájením upgradu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fdc88-148">Modify your cluster configuration tooset hello following property toofalse before you start a configuration upgrade.</span></span>

        "fabricClusterAutoupgradeEnabled": false,

<span data-ttu-id="fdc88-149">Odkazovat příliš[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) podrobnosti o použití.</span><span class="sxs-lookup"><span data-stu-id="fdc88-149">Refer too[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) for usage details.</span></span> <span data-ttu-id="fdc88-150">Zkontrolujte zda tooupdate 'clusterConfigurationVersion' v vaše struktury JSON před zahájením upgradu konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-150">Make sure tooupdate 'clusterConfigurationVersion' in your JSON before you start hello configuration upgrade.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="fdc88-151">Pracovní postup upgradu clusteru</span><span class="sxs-lookup"><span data-stu-id="fdc88-151">Cluster upgrade workflow</span></span>

1. <span data-ttu-id="fdc88-152">Get-ServiceFabricClusterUpgrade spustit na jednom z hello uzlů v clusteru hello a poznamenejte si hello TargetCodeVersion.</span><span class="sxs-lookup"><span data-stu-id="fdc88-152">Run Get-ServiceFabricClusterUpgrade from one of hello nodes in hello cluster and note hello TargetCodeVersion.</span></span>
2. <span data-ttu-id="fdc88-153">Spuštění hello následující z toolist počítač připojený Internetu všechny upgrade verze kompatibilní s aktuální verzí hello a stáhněte si odpovídající balíček z odkazů přidružené ke stažení hello hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-153">Run hello following from an internet connected machine toolist all upgrade compatible versions with hello current version and download hello corresponding package from hello associated download links.</span></span>

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. <span data-ttu-id="fdc88-154">Připojte toohello cluster z libovolného počítače, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-154">Connect toohello cluster from any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster.</span></span> <span data-ttu-id="fdc88-155">Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello</span><span class="sxs-lookup"><span data-stu-id="fdc88-155">hello machine that this script is run on does not have toobe part of hello cluster</span></span>

    ```powershell

   ###### Get hello list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file including hello path tooit> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. <span data-ttu-id="fdc88-156">Zkopírujte balíček hello stáhli do úložiště bitových kopií clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-156">Copy hello downloaded package into hello cluster image store.</span></span>

5. <span data-ttu-id="fdc88-157">Zaregistrujte hello zkopírovat balíček.</span><span class="sxs-lookup"><span data-stu-id="fdc88-157">Register hello copied package.</span></span>

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. <span data-ttu-id="fdc88-158">Spusťte verzi k dispozici upgrade tooan clusteru.</span><span class="sxs-lookup"><span data-stu-id="fdc88-158">Start a cluster upgrade tooan available version.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="fdc88-159">Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru, nebo můžete spustit následující příkaz prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-159">You can monitor hello progress of hello upgrade on Service Fabric Explorer, or you can run hello following PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="fdc88-160">Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="fdc88-160">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="fdc88-161">toospecify stavu vlastní zásady pro hello **Start-ServiceFabricClusterUpgrade** příkaz, naleznete v dokumentaci k hello [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="fdc88-161">toospecify custom health policies for hello **Start-ServiceFabricClusterUpgrade** command, see hello documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="fdc88-162">Po opravě hello problémy, které výsledkem hello vrácení inicializujte následující hello stejný postup jako výše popsané hello upgrade znovu.</span><span class="sxs-lookup"><span data-stu-id="fdc88-162">After you fix hello issues that resulted in hello rollback, initiate hello upgrade again by following hello same steps as previously described.</span></span>


## <a name="upgrade-hello-cluster-configuration"></a><span data-ttu-id="fdc88-163">Upgradovat konfiguraci clusteru hello</span><span class="sxs-lookup"><span data-stu-id="fdc88-163">Upgrade hello cluster configuration</span></span>
<span data-ttu-id="fdc88-164">Před spuštěním upgradu hello konfigurace, můžete otestovat vaše nové konfigurace json clusteru spuštěním skriptu prostředí powershell hello v balíčku samostatné hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-164">Before you initiate hello configuration upgrade, you can test your new cluster configuration json by running hello powershell script in hello standalone package.</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File>

```
<span data-ttu-id="fdc88-165">nebo</span><span class="sxs-lookup"><span data-stu-id="fdc88-165">or</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File> -FabricRuntimePackagePath <Path toohello .cab file which you want tootest hello configuration against>

```

<span data-ttu-id="fdc88-166">Některé konfigurace nelze upgradovat, jako je například koncových bodů, název clusteru, uzel IP adresy, atd. Tato akce testování hello nového clusteru konfigurace json proti hello starý a throw chyby v okně Powershell text hello, pokud je jakýkoli problém.</span><span class="sxs-lookup"><span data-stu-id="fdc88-166">Some configurations can't be upgraded, such as endpoints, cluster name, node IP, etc. This will test hello new cluster configuration json against hello old one and throw errors in hello Powershell window if there is any issue.</span></span>

<span data-ttu-id="fdc88-167">tooupgrade hello clusteru konfigurace upgradu, spusťte **Start-ServiceFabricClusterConfigurationUpgrade**.</span><span class="sxs-lookup"><span data-stu-id="fdc88-167">tooupgrade hello cluster configuration upgrade, run **Start-ServiceFabricClusterConfigurationUpgrade**.</span></span> <span data-ttu-id="fdc88-168">zpracovaná upgradovací doméně podle upgradu domény je upgrade konfigurace Hello.</span><span class="sxs-lookup"><span data-stu-id="fdc88-168">hello configuration upgrade is processed upgrade domain by upgrade domain.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

### <a name="cluster-certificate-config-upgrade"></a><span data-ttu-id="fdc88-169">Aktualizace konfigurace certifikátu clusteru</span><span class="sxs-lookup"><span data-stu-id="fdc88-169">Cluster certificate config upgrade</span></span>  
<span data-ttu-id="fdc88-170">Cluster certifikát se používá k ověřování mezi uzly clusteru, tak hello certifikát mění, musejí být prováděny s další upozornění protože selhání bude blokovat hello komunikaci mezi uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="fdc88-170">Cluster certificate is used for authentication between cluster nodes, so hello certificate roll over should be performed with extra caution because failure will block hello communication among cluster nodes.</span></span>  
<span data-ttu-id="fdc88-171">Technicky jsou podporovány tři možnosti:</span><span class="sxs-lookup"><span data-stu-id="fdc88-171">Technically, three options are supported:</span></span>  

1. <span data-ttu-id="fdc88-172">Jeden certifikát upgrade: hello upgradu je ' certifikátu (primární) -> certifikát B (primární) -> C certifikátu (primární) ->...'.</span><span class="sxs-lookup"><span data-stu-id="fdc88-172">Single certificate upgrade: hello upgrade path is 'Certificate A (Primary) -> Certificate B (Primary) -> Certificate C (Primary) -> ...'.</span></span>   
2. <span data-ttu-id="fdc88-173">Double certifikátu upgrade: hello upgradu je ' certifikátu (primární) -> certifikátu (primární) a B (sekundární) -> certifikát B (primární) -> certifikát B (primární) a C (sekundární) -> C certifikátu (primární) ->...'.</span><span class="sxs-lookup"><span data-stu-id="fdc88-173">Double certificate upgrade: hello upgrade path is 'Certificate A (Primary) -> Certificate A (Primary) and B (Secondary) -> Certificate B (Primary) -> Certificate B (Primary) and C (Secondary) -> Certificate C (Primary) -> ...'.</span></span>
3. <span data-ttu-id="fdc88-174">Certifikát typ upgradu: Konfigurace certifikátu na základě CommonName configuration <> – na základě kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="fdc88-174">Certificate type upgrade: Thumbprint-based certificate configuration <-> CommonName-based certificate configuration.</span></span> <span data-ttu-id="fdc88-175">Například kryptografický otisk certifikátu (primární) a kryptografický otisk B (sekundární) -> certifikát CommonName C.</span><span class="sxs-lookup"><span data-stu-id="fdc88-175">For example, Certificate Thumbprint A (Primary) and Thumbprint B (Secondary) -> Certificate CommonName C.</span></span>


## <a name="next-steps"></a><span data-ttu-id="fdc88-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdc88-176">Next steps</span></span>
* <span data-ttu-id="fdc88-177">Zjistěte, jak toocustomize některé [nastavení clusteru Service Fabric](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="fdc88-177">Learn how toocustomize some [Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>
* <span data-ttu-id="fdc88-178">Zjistěte, jak příliš[škálování vašeho clusteru a odhlašování](service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="fdc88-178">Learn how too[scale your cluster in and out](service-fabric-cluster-scale-up-down.md).</span></span>
* <span data-ttu-id="fdc88-179">Další informace o [upgradů aplikací](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="fdc88-179">Learn about [application upgrades](service-fabric-application-upgrade.md).</span></span>

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
