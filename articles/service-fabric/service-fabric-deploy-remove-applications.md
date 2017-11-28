---
title: "aaaAzure nasazení aplikace Service Fabric | Microsoft Docs"
description: "Jak toodeploy a odebrat aplikace v Service Fabric pomocí prostředí PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 3de9c6a937ee7b29bf9ec86d6e9e631487797507
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a><span data-ttu-id="3b9be-103">Nasazení a odebírat aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3b9be-103">Deploy and remove applications using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3b9be-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3b9be-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="3b9be-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b9be-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="3b9be-106">Rozhraní API FabricClient</span><span class="sxs-lookup"><span data-stu-id="3b9be-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="3b9be-107">Jednou [typu aplikaci vytvořen balíček][10], je připraven k nasazení do clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3b9be-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="3b9be-108">Nasazení zahrnuje hello následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="3b9be-108">Deployment involves hello following three steps:</span></span>

1. <span data-ttu-id="3b9be-109">Nahrát úložiště bitových kopií toohello balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-109">Upload hello application package toohello image store</span></span>
2. <span data-ttu-id="3b9be-110">Registrace typu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-110">Register hello application type</span></span>
3. <span data-ttu-id="3b9be-111">Vytvoření instance aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-111">Create hello application instance</span></span>

<span data-ttu-id="3b9be-112">Jakmile je aplikace nasazená a instance běží v clusteru hello, můžete odstranit hello instanci aplikace a její typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-112">After an application is deployed and an instance is running in hello cluster, you can delete hello application instance and its application type.</span></span> <span data-ttu-id="3b9be-113">toocompletely odebrat aplikaci z clusteru hello zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b9be-113">toocompletely remove an application from hello cluster involves hello following steps:</span></span>

1. <span data-ttu-id="3b9be-114">Odebrat (nebo odstranit) hello spuštěna instance aplikace</span><span class="sxs-lookup"><span data-stu-id="3b9be-114">Remove (or delete) hello running application instance</span></span>
2. <span data-ttu-id="3b9be-115">Zrušení registrace typu aplikace hello, pokud již nepotřebujete</span><span class="sxs-lookup"><span data-stu-id="3b9be-115">Unregister hello application type if you no longer need it</span></span>
3. <span data-ttu-id="3b9be-116">Odebrání balíčku aplikace hello z úložiště bitových kopií hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-116">Remove hello application package from hello image store</span></span>

<span data-ttu-id="3b9be-117">Pokud používáte [Visual Studio pro nasazování a ladění aplikací](service-fabric-publish-app-remote-cluster.md) na vaše místní vývojový cluster všechny předchozí kroky hello se zpracovávají automaticky pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b9be-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all hello preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="3b9be-118">Tento skript se nachází v hello *skripty* složku projekt aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-118">This script is found in hello *Scripts* folder of hello application project.</span></span> <span data-ttu-id="3b9be-119">Tento článek obsahuje pozadí na co skriptu je to, aby mohli provést hello stejné operace mimo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b9be-119">This article provides background on what that script is doing so that you can perform hello same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-toohello-cluster"></a><span data-ttu-id="3b9be-120">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="3b9be-120">Connect toohello cluster</span></span>
<span data-ttu-id="3b9be-121">Než spustíte všechny příkazy prostředí PowerShell v tomto článku, vždy spustit pomocí [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cluster Service Fabric toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="3b9be-121">Before you run any PowerShell commands in this article, always start by using [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric cluster.</span></span> <span data-ttu-id="3b9be-122">tooconnect toohello místního vývojového clusteru, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="3b9be-122">tooconnect toohello local development cluster, run hello following:</span></span>

```powershell
PS C:\>Connect-ServiceFabricCluster
```

<span data-ttu-id="3b9be-123">Příklady připojující vzdáleného clusteru tooa nebo clusteru zabezpečené pomocí Azure Active Directory, X509 certifikáty nebo služby Windows Active Directory najdete v tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3b9be-123">For examples of connecting tooa remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="upload-hello-application-package"></a><span data-ttu-id="3b9be-124">Nahrání balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-124">Upload hello application package</span></span>
<span data-ttu-id="3b9be-125">Odesílání balíčku aplikace hello vloží ho do umístění, která je přístupná pomocí interní komponenty Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3b9be-125">Uploading hello application package puts it in a location that's accessible by internal Service Fabric components.</span></span>
<span data-ttu-id="3b9be-126">Pokud chcete tooverify hello balíčku aplikace místně, použijte hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3b9be-126">If you want tooverify hello application package locally, use hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="3b9be-127">Hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz nahrávání hello úložiště bitových kopií clusteru toohello balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-127">hello [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command uploads hello application package toohello cluster image store.</span></span>
<span data-ttu-id="3b9be-128">Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b9be-128">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="3b9be-129">tooimport hello SDK modul, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3b9be-129">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="3b9be-130">Předpokládejme, že sestavení a balíček aplikace s názvem *Moje_aplikace* ve Visual Studiu 2015.</span><span class="sxs-lookup"><span data-stu-id="3b9be-130">Suppose you build and package an application named *MyApplication* in Visual Studio 2015.</span></span> <span data-ttu-id="3b9be-131">Ve výchozím nastavení je název typu aplikace hello uvedené v hello ApplicationManifest.xml "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="3b9be-131">By default, hello application type name listed in hello ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="3b9be-132">Hello balíček aplikace, která obsahuje manifest hello nezbytné aplikace, služby manifesty a balíčků kódu/config/data, se nachází v *C:\Users\<uživatelské jméno\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="3b9be-132">hello application package, which contains hello necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\<username\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span> 

<span data-ttu-id="3b9be-133">Hello následující příkaz vypíše hello obsah balíčku aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="3b9be-133">hello following command lists hello contents of hello application package:</span></span>

```powershell
PS C:\> $path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

<span data-ttu-id="3b9be-134">Pokud balíček aplikace hello je velký nebo má mnoho souborů, můžete [je komprimovat](service-fabric-package-apps.md#compress-a-package).</span><span class="sxs-lookup"><span data-stu-id="3b9be-134">If hello application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package).</span></span> <span data-ttu-id="3b9be-135">Hello komprese snižuje velikost hello a hello počet souborů.</span><span class="sxs-lookup"><span data-stu-id="3b9be-135">hello compression reduces hello size and hello number of files.</span></span>
<span data-ttu-id="3b9be-136">Hello vedlejším účinkem je hello této registrace a zrušení registrace typu aplikace je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="3b9be-136">hello side effect is that registering and un-registering hello application type are faster.</span></span> <span data-ttu-id="3b9be-137">Čas odeslání může být pomalejší aktuálně, zvláště pokud zahrnete hello čas toocompress hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="3b9be-137">Upload time may be slower currently, especially if you include hello time toocompress hello package.</span></span> 

<span data-ttu-id="3b9be-138">hello toocompress balíček, použijte stejný [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3b9be-138">toocompress a package, use hello same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span> <span data-ttu-id="3b9be-139">Komprese lze provést samostatné z nahrávání, pomocí hello `SkipCopy` příznak nebo společně s hello operace odesílání.</span><span class="sxs-lookup"><span data-stu-id="3b9be-139">Compression can be done separate from upload, by using hello `SkipCopy` flag, or together with hello upload operation.</span></span> <span data-ttu-id="3b9be-140">Použití komprese na zkomprimovaného balíčku je žádná operace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-140">Applying compression on a compressed package is no-op.</span></span>
<span data-ttu-id="3b9be-141">hello toouncompress zkomprimovaného balíčku, použijte stejný [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s hello `UncompressPackage` přepínače.</span><span class="sxs-lookup"><span data-stu-id="3b9be-141">toouncompress a compressed package, use hello same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command with hello `UncompressPackage` switch.</span></span>

<span data-ttu-id="3b9be-142">Hello následující rutiny komprimaci hello balíček bez kopírování toohello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="3b9be-142">hello following cmdlet compresses hello package without copying it toohello image store.</span></span> <span data-ttu-id="3b9be-143">balíček Hello teď obsahuje komprimované soubory pro hello `Code` a `Config` balíčky.</span><span class="sxs-lookup"><span data-stu-id="3b9be-143">hello package now includes zipped files for hello `Code` and `Config` packages.</span></span> <span data-ttu-id="3b9be-144">hello služby manifestů aplikace Hello a nejsou ZIP, protože jsou potřebné pro mnoho interních operací (např. balíček sdílení extrakci název a verze typu aplikace pro určité ověření).</span><span class="sxs-lookup"><span data-stu-id="3b9be-144">hello application and hello service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span> <span data-ttu-id="3b9be-145">Pomocí formátu ZIP manifesty hello by proveďte tyto operace neefektivní.</span><span class="sxs-lookup"><span data-stu-id="3b9be-145">Zipping hello manifests would make these operations inefficient.</span></span>

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

<span data-ttu-id="3b9be-146">Pro velkých balíčků aplikací komprese hello trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="3b9be-146">For large application packages, hello compression takes time.</span></span> <span data-ttu-id="3b9be-147">Nejlepších výsledků dosáhnete pomocí rychlé jednotky SSD.</span><span class="sxs-lookup"><span data-stu-id="3b9be-147">For best results, use a fast SSD drive.</span></span> <span data-ttu-id="3b9be-148">časy Hello komprese a velikost hello hello zkomprimovaného balíčku se také liší podle hello obsah balíčku.</span><span class="sxs-lookup"><span data-stu-id="3b9be-148">hello compression times and hello size of hello compressed package also differ based on hello package content.</span></span>
<span data-ttu-id="3b9be-149">Zde je například komprese statistiku pro některé balíčky, které ukazují hello počáteční a hello velikost zkomprimovaného balíčku s časem komprese hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-149">For example, here is compression statistics for some packages, which show hello initial and hello compressed package size, with hello compression time.</span></span>

|<span data-ttu-id="3b9be-150">Počáteční velikost (MB)</span><span class="sxs-lookup"><span data-stu-id="3b9be-150">Initial size (MB)</span></span>|<span data-ttu-id="3b9be-151">Počet souborů</span><span class="sxs-lookup"><span data-stu-id="3b9be-151">File count</span></span>|<span data-ttu-id="3b9be-152">Komprese čas</span><span class="sxs-lookup"><span data-stu-id="3b9be-152">Compression Time</span></span>|<span data-ttu-id="3b9be-153">Zkomprimovaného balíčku velikost (MB)</span><span class="sxs-lookup"><span data-stu-id="3b9be-153">Compressed package size (MB)</span></span>|
|----------------:|---------:|---------------:|---------------------------:|
|<span data-ttu-id="3b9be-154">100</span><span class="sxs-lookup"><span data-stu-id="3b9be-154">100</span></span>|<span data-ttu-id="3b9be-155">100</span><span class="sxs-lookup"><span data-stu-id="3b9be-155">100</span></span>|<span data-ttu-id="3b9be-156">00:00:03.3547592</span><span class="sxs-lookup"><span data-stu-id="3b9be-156">00:00:03.3547592</span></span>|<span data-ttu-id="3b9be-157">60</span><span class="sxs-lookup"><span data-stu-id="3b9be-157">60</span></span>|
|<span data-ttu-id="3b9be-158">512</span><span class="sxs-lookup"><span data-stu-id="3b9be-158">512</span></span>|<span data-ttu-id="3b9be-159">100</span><span class="sxs-lookup"><span data-stu-id="3b9be-159">100</span></span>|<span data-ttu-id="3b9be-160">00:00:16.3850303</span><span class="sxs-lookup"><span data-stu-id="3b9be-160">00:00:16.3850303</span></span>|<span data-ttu-id="3b9be-161">307</span><span class="sxs-lookup"><span data-stu-id="3b9be-161">307</span></span>|
|<span data-ttu-id="3b9be-162">1024</span><span class="sxs-lookup"><span data-stu-id="3b9be-162">1024</span></span>|<span data-ttu-id="3b9be-163">500</span><span class="sxs-lookup"><span data-stu-id="3b9be-163">500</span></span>|<span data-ttu-id="3b9be-164">00:00:32.5907950</span><span class="sxs-lookup"><span data-stu-id="3b9be-164">00:00:32.5907950</span></span>|<span data-ttu-id="3b9be-165">615</span><span class="sxs-lookup"><span data-stu-id="3b9be-165">615</span></span>|
|<span data-ttu-id="3b9be-166">2 048</span><span class="sxs-lookup"><span data-stu-id="3b9be-166">2048</span></span>|<span data-ttu-id="3b9be-167">1000</span><span class="sxs-lookup"><span data-stu-id="3b9be-167">1000</span></span>|<span data-ttu-id="3b9be-168">00:01:04.3775554</span><span class="sxs-lookup"><span data-stu-id="3b9be-168">00:01:04.3775554</span></span>|<span data-ttu-id="3b9be-169">1231</span><span class="sxs-lookup"><span data-stu-id="3b9be-169">1231</span></span>|
|<span data-ttu-id="3b9be-170">5012</span><span class="sxs-lookup"><span data-stu-id="3b9be-170">5012</span></span>|<span data-ttu-id="3b9be-171">100</span><span class="sxs-lookup"><span data-stu-id="3b9be-171">100</span></span>|<span data-ttu-id="3b9be-172">00:02:45.2951288</span><span class="sxs-lookup"><span data-stu-id="3b9be-172">00:02:45.2951288</span></span>|<span data-ttu-id="3b9be-173">3074</span><span class="sxs-lookup"><span data-stu-id="3b9be-173">3074</span></span>|

<span data-ttu-id="3b9be-174">Jakmile je komprimovaný balíček, může být nahrané tooone nebo více Service Fabric clusterů podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b9be-174">Once a package is compressed, it can be uploaded tooone or multiple Service Fabric clusters as needed.</span></span> <span data-ttu-id="3b9be-175">mechanismus pro nasazení Hello je stejný pro komprimované a nekomprimované balíčky.</span><span class="sxs-lookup"><span data-stu-id="3b9be-175">hello deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="3b9be-176">Pokud balíček hello je komprimován, uloží se jako takový do úložiště bitových kopií clusteru hello a je nekomprimovaným na hello uzlu před spuštěním aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-176">If hello package is compressed, it is stored as such in hello cluster image store and it's uncompressed on hello node before hello application is run.</span></span>


<span data-ttu-id="3b9be-177">Hello následujícím příkladu se uloží úložiště hello balíček toohello bitové kopie, do složky s názvem "MyApplicationV1":</span><span class="sxs-lookup"><span data-stu-id="3b9be-177">hello following example uploads hello package toohello image store, into a folder named "MyApplicationV1":</span></span>

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

<span data-ttu-id="3b9be-178">Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b9be-178">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="3b9be-179">tooimport hello SDK modul, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3b9be-179">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="3b9be-180">Pokud nezadáte hello *- ApplicationPackagePathInImageStore* parametr, balíček aplikace hello zkopírován do složky "Ladění" hello v úložišti bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-180">If you do not specify hello *-ApplicationPackagePathInImageStore* parameter, hello application package is copied into hello "Debug" folder in hello image store.</span></span>

<span data-ttu-id="3b9be-181">Hello doby potřebné tooupload balíčku se liší v závislosti na několika faktorech.</span><span class="sxs-lookup"><span data-stu-id="3b9be-181">hello time it takes tooupload a package differs depending on multiple factors.</span></span> <span data-ttu-id="3b9be-182">Některé tyto faktory jsou hello počet souborů v balíčku hello, velikost balíčku hello a velikosti souborů hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-182">Some of these factors are hello number of files in hello package, hello package size, and hello file sizes.</span></span> <span data-ttu-id="3b9be-183">rychlost sítě Hello mezi hello zdrojového počítače a cluster Service Fabric hello se zároveň ovlivňuje doba nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-183">hello network speed between hello source machine and hello Service Fabric cluster also impacts hello upload time.</span></span> <span data-ttu-id="3b9be-184">Výchozí hodnota časového limitu pro Hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) je 30 minut.</span><span class="sxs-lookup"><span data-stu-id="3b9be-184">hello default timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minutes.</span></span>
<span data-ttu-id="3b9be-185">V závislosti na hello popisuje faktory, může mít tooincrease hello vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="3b9be-185">Depending on hello described factors, you may have tooincrease hello timeout.</span></span> <span data-ttu-id="3b9be-186">Pokud jsou komprese hello balíček ve volání hello kopie, musíte tooalso zvažte hello komprese čas.</span><span class="sxs-lookup"><span data-stu-id="3b9be-186">If you are compressing hello package in hello copy call, you need tooalso consider hello compression time.</span></span>

<span data-ttu-id="3b9be-187">V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b9be-187">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

## <a name="register-hello-application-package"></a><span data-ttu-id="3b9be-188">Registrace balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-188">Register hello application package</span></span>
<span data-ttu-id="3b9be-189">v manifestu aplikace hello k dispozici pro použití při registraci balíčku aplikace hello deklarována Hello typ a verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-189">hello application type and version declared in hello application manifest become available for use when hello application package is registered.</span></span> <span data-ttu-id="3b9be-190">Hello systému přečte nahraný v předchozím kroku hello hello balíček, ověří hello balíčku, zpracuje hello obsah balíčku a zkopíruje hello zpracovat balíček tooan systému interní umístění.</span><span class="sxs-lookup"><span data-stu-id="3b9be-190">hello system reads hello package uploaded in hello previous step, verifies hello package, processes hello package contents, and copies hello processed package tooan internal system location.</span></span>  

<span data-ttu-id="3b9be-191">Spustit hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) tooregister rutiny hello typu aplikace v clusteru hello a zpřístupní ji pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="3b9be-191">Run hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet tooregister hello application type in hello cluster and make it available for deployment:</span></span>

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

<span data-ttu-id="3b9be-192">"MyApplicationV1" je složka hello v úložišti hello bitové kopie, kde se nachází balíček aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-192">"MyApplicationV1" is hello folder in hello image store where hello application package is located.</span></span> <span data-ttu-id="3b9be-193">Typ aplikace Hello s názvem "MyApplicationType" a verzí "1.0.0" (jak se nacházejí v manifestu aplikace hello) je nyní zaregistrovaná v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-193">hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) is now registered in hello cluster.</span></span>

<span data-ttu-id="3b9be-194">Hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) příkaz vrátí jenom po hello systém obsahuje balíček aplikace hello byl úspěšně zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="3b9be-194">hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) command returns only after hello system has successfully registered hello application package.</span></span> <span data-ttu-id="3b9be-195">Jak dlouho trvá registrace závisí na velikosti hello a obsah balíčku aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-195">How long registration takes depends on hello size and contents of hello application package.</span></span> <span data-ttu-id="3b9be-196">V případě potřeby hello **- TimeoutSec** parametr může být použité toosupply delší časový limit (výchozí hodnota časového limitu hello je 60 sekund).</span><span class="sxs-lookup"><span data-stu-id="3b9be-196">If needed, hello **-TimeoutSec** parameter can be used toosupply a longer timeout (hello default timeout is 60 seconds).</span></span>

<span data-ttu-id="3b9be-197">Pokud máte velké aplikací, balíčků nebo pokud dochází k vypršení časových limitů, použijte hello **- asynchronní** parametr.</span><span class="sxs-lookup"><span data-stu-id="3b9be-197">If you have a large application package or if you are experiencing timeouts, use hello **-Async** parameter.</span></span> <span data-ttu-id="3b9be-198">příkaz Hello vrátí při hello clusteru přijme příkaz hello registrace a zpracování hello pokračuje podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b9be-198">hello command returns when hello cluster accepts hello register command, and hello processing continues as needed.</span></span>
<span data-ttu-id="3b9be-199">Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-199">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="3b9be-200">Tento příkaz toodetermine můžete použít, pokud se provádí registraci hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-200">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-hello-application"></a><span data-ttu-id="3b9be-201">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-201">Create hello application</span></span>
<span data-ttu-id="3b9be-202">Můžete vytvořit instanci aplikace z libovolné verze typu aplikace, který byl úspěšně zaregistrován pomocí hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3b9be-202">You can instantiate an application from any application type version that has been registered successfully by using hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="3b9be-203">Hello názvu každé aplikace musí začínat hello *"fabric:"* scheme a musí být jedinečný pro každou instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-203">hello name of each application must start with hello *"fabric:"* scheme and must be unique for each application instance.</span></span> <span data-ttu-id="3b9be-204">Všechny výchozí služby definované v manifestu aplikace hello typu hello cílové aplikace jsou také vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3b9be-204">Any default services defined in hello application manifest of hello target application type are also created.</span></span>

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
<span data-ttu-id="3b9be-205">Pro danou verzi typu zaregistrovanou aplikaci lze vytvořit více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-205">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="3b9be-206">Každá instance aplikace spouští v izolaci, s vlastní pracovní adresář a proces.</span><span class="sxs-lookup"><span data-stu-id="3b9be-206">Each application instance runs in isolation, with its own work directory and process.</span></span>

<span data-ttu-id="3b9be-207">toosee, které s názvem aplikace a služby běží v clusteru hello, spusťte hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) a [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) rutiny:</span><span class="sxs-lookup"><span data-stu-id="3b9be-207">toosee which named apps and services are running in hello cluster, run hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) and [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a><span data-ttu-id="3b9be-208">Odebrání aplikace</span><span class="sxs-lookup"><span data-stu-id="3b9be-208">Remove an application</span></span>
<span data-ttu-id="3b9be-209">Instance aplikace už je potřeba, trvale odstraníte ho podle názvu pomocí hello [odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3b9be-209">When an application instance is no longer needed, you can permanently remove it by name using hello [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="3b9be-210">[Odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automaticky odstraní všechny služby, které patří toohello aplikace také a trvale odebrat všechny služby stavu.</span><span class="sxs-lookup"><span data-stu-id="3b9be-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatically removes all services that belong toohello application as well, permanently removing all service state.</span></span> 

> [!WARNING]
> <span data-ttu-id="3b9be-211">Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-211">This operation cannot be reversed, and application state cannot be recovered.</span></span>

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a><span data-ttu-id="3b9be-212">Typ aplikace se zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="3b9be-212">Unregister an application type</span></span>
<span data-ttu-id="3b9be-213">Pokud konkrétní verzi typ aplikace se už nepotřebuje, by měl zrušit registraci typu aplikace hello pomocí hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3b9be-213">When a particular version of an application type is no longer needed, you should unregister hello application type using hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="3b9be-214">Zrušení registrace nepoužívané aplikace typy verzích prostor úložiště používá úložiště bitových kopií hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-214">Unregistering unused application types releases storage space used by hello image store.</span></span> <span data-ttu-id="3b9be-215">Typ aplikace může být registrace, dokud u ní jsou vytvořeny žádné aplikace a žádné čekající na vyřízení aplikace upgrady na něj odkazují.</span><span class="sxs-lookup"><span data-stu-id="3b9be-215">An application type can be unregistered as long as no applications are instantiated against it and no pending application upgrades are referencing it.</span></span>

<span data-ttu-id="3b9be-216">Spustit [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) typy aplikací hello toosee aktuálně registrované v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="3b9be-216">Run [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello application types currently registered in hello cluster:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

<span data-ttu-id="3b9be-217">Spustit [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister konkrétní typ aplikace:</span><span class="sxs-lookup"><span data-stu-id="3b9be-217">Run [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister a specific application type:</span></span>

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-hello-image-store"></a><span data-ttu-id="3b9be-218">Balíček aplikace odebrat z úložiště bitových kopií hello</span><span class="sxs-lookup"><span data-stu-id="3b9be-218">Remove an application package from hello image store</span></span>
<span data-ttu-id="3b9be-219">Balíček aplikace už je potřeba, odstraňte jej z hello image store toofree až systémové prostředky.</span><span class="sxs-lookup"><span data-stu-id="3b9be-219">When an application package is no longer needed, you can delete it from hello image store toofree up system resources.</span></span>

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a><span data-ttu-id="3b9be-220">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="3b9be-220">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="3b9be-221">Kopírování ServiceFabricApplicationPackage požádá o parametr ImageStoreConnectionString</span><span class="sxs-lookup"><span data-stu-id="3b9be-221">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="3b9be-222">prostředí Service Fabric SDK Hello byste již měli mít hello správný, nastavit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3b9be-222">hello Service Fabric SDK environment should already have hello correct defaults set up.</span></span> <span data-ttu-id="3b9be-223">Ale v případě potřeby pro všechny příkazy hello parametr ImageStoreConnectionString musí odpovídat hello hodnotu této hello používá cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3b9be-223">But if needed, hello ImageStoreConnectionString for all commands should match hello value that hello Service Fabric cluster is using.</span></span> <span data-ttu-id="3b9be-224">Hello parametr ImageStoreConnectionString můžete najít v manifestu clusteru hello načten pomocí hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) a Get-ImageStoreConnectionStringFromClusterManifest příkazy:</span><span class="sxs-lookup"><span data-stu-id="3b9be-224">You can find hello ImageStoreConnectionString in hello cluster manifest, retrieved using hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="3b9be-225">Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b9be-225">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="3b9be-226">tooimport hello SDK modul, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3b9be-226">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="3b9be-227">Parametr ImageStoreConnectionString Hello se nachází v manifestu clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="3b9be-227">hello ImageStoreConnectionString is found in hello cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="3b9be-228">V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b9be-228">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="3b9be-229">Nasazení balíčku velké aplikace</span><span class="sxs-lookup"><span data-stu-id="3b9be-229">Deploy large application package</span></span>
<span data-ttu-id="3b9be-230">Problém: [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) časového limitu pro balíček rozsáhlé aplikace (pořadí GB).</span><span class="sxs-lookup"><span data-stu-id="3b9be-230">Issue: [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) times out for a large application package (order of GB).</span></span>
<span data-ttu-id="3b9be-231">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="3b9be-231">Try:</span></span>
- <span data-ttu-id="3b9be-232">Zadat delší časový limit pro [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkazu s `TimeoutSec` parametr.</span><span class="sxs-lookup"><span data-stu-id="3b9be-232">Specify a larger timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command, with `TimeoutSec` parameter.</span></span> <span data-ttu-id="3b9be-233">Ve výchozím nastavení je hello časový limit 30 minut.</span><span class="sxs-lookup"><span data-stu-id="3b9be-233">By default, hello timeout is 30 minutes.</span></span>
- <span data-ttu-id="3b9be-234">Zkontrolujte hello síťové připojení mezi zdrojovým počítačem a clusteru.</span><span class="sxs-lookup"><span data-stu-id="3b9be-234">Check hello network connection between your source machine and cluster.</span></span> <span data-ttu-id="3b9be-235">Pokud jde o pomalé připojení hello, zvažte použití na počítači s lepší síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="3b9be-235">If hello connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="3b9be-236">Pokud hello klientský počítač je v jiné oblasti než hello clusteru, zvažte použití klientský počítač v oblasti blíže nebo stejné jako hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3b9be-236">If hello client machine is in another region than hello cluster, consider using a client machine in a closer or same region as hello cluster.</span></span>
- <span data-ttu-id="3b9be-237">Zkontrolujte, pokud jste nedosáhli externí omezení.</span><span class="sxs-lookup"><span data-stu-id="3b9be-237">Check if you are hitting external throttling.</span></span> <span data-ttu-id="3b9be-238">Například v případě je úložiště image hello nakonfigurované toouse azure storage, může nahrávání omezeny.</span><span class="sxs-lookup"><span data-stu-id="3b9be-238">For example, when hello image store is configured toouse azure storage, upload may be throttled.</span></span>

<span data-ttu-id="3b9be-239">Problém: Nahrávání balíček úspěšně dokončit, ale [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) vyprší časový limit. Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="3b9be-239">Issue: Upload package completed successfully, but [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out. Try:</span></span>
- <span data-ttu-id="3b9be-240">[Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="3b9be-240">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span>
<span data-ttu-id="3b9be-241">Hello komprese snižuje velikost hello a musíte provést hello počet souborů, která zase snižuje hello objem provozu a že Service Fabric fungovat.</span><span class="sxs-lookup"><span data-stu-id="3b9be-241">hello compression reduces hello size and hello number of files, which in turn reduces hello amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="3b9be-242">operace nahrávání Hello může být pomalejší (obzvláště pokud zahrnete hello komprese čas), ale registrace a zrušení registrace typu aplikace hello je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="3b9be-242">hello upload operation may be slower (especially if you include hello compression time), but register and un-register hello application type are faster.</span></span>
- <span data-ttu-id="3b9be-243">Zadat delší časový limit pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) s `TimeoutSec` parametr.</span><span class="sxs-lookup"><span data-stu-id="3b9be-243">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="3b9be-244">Zadejte `Async` přepínač pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="3b9be-244">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="3b9be-245">příkaz Hello vrátí, když hello clusteru přijme příkaz hello a hello registrace typu aplikace hello asynchronně.</span><span class="sxs-lookup"><span data-stu-id="3b9be-245">hello command returns when hello cluster accepts hello command and hello registration of hello application type continues asynchronously.</span></span> <span data-ttu-id="3b9be-246">Z tohoto důvodu není bez nutnosti toospecify vyšší vypršení časového limitu v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="3b9be-246">For this reason, there is no need toospecify a higher timeout in this case.</span></span> <span data-ttu-id="3b9be-247">Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-247">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="3b9be-248">Tento příkaz toodetermine můžete použít, pokud se provádí registraci hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-248">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="3b9be-249">Nasazení balíčku aplikace s mnoho souborů</span><span class="sxs-lookup"><span data-stu-id="3b9be-249">Deploy application package with many files</span></span>
<span data-ttu-id="3b9be-250">Problém: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) časového limitu pro balíček aplikace s mnoha souborech (pořadí tisíc).</span><span class="sxs-lookup"><span data-stu-id="3b9be-250">Issue: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="3b9be-251">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="3b9be-251">Try:</span></span>
- <span data-ttu-id="3b9be-252">[Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="3b9be-252">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span> <span data-ttu-id="3b9be-253">Hello komprese snižuje hello počet souborů.</span><span class="sxs-lookup"><span data-stu-id="3b9be-253">hello compression reduces hello number of files.</span></span>
- <span data-ttu-id="3b9be-254">Zadat delší časový limit pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) s `TimeoutSec` parametr.</span><span class="sxs-lookup"><span data-stu-id="3b9be-254">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="3b9be-255">Zadejte `Async` přepínač pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="3b9be-255">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="3b9be-256">příkaz Hello vrátí, když hello clusteru přijme příkaz hello a hello registrace typu aplikace hello asynchronně.</span><span class="sxs-lookup"><span data-stu-id="3b9be-256">hello command returns when hello cluster accepts hello command and hello registration of hello application type continues asynchronously.</span></span>
<span data-ttu-id="3b9be-257">Z tohoto důvodu není bez nutnosti toospecify vyšší vypršení časového limitu v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="3b9be-257">For this reason, there is no need toospecify a higher timeout in this case.</span></span> <span data-ttu-id="3b9be-258">Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace.</span><span class="sxs-lookup"><span data-stu-id="3b9be-258">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="3b9be-259">Tento příkaz toodetermine můžete použít, pokud se provádí registraci hello.</span><span class="sxs-lookup"><span data-stu-id="3b9be-259">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a><span data-ttu-id="3b9be-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b9be-260">Next steps</span></span>
[<span data-ttu-id="3b9be-261">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3b9be-261">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="3b9be-262">Úvod stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3b9be-262">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="3b9be-263">Diagnostika a řešení služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3b9be-263">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="3b9be-264">Model aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3b9be-264">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
