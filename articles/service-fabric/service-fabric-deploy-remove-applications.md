---
title: "Nasazení aplikace Azure Service Fabric | Microsoft Docs"
description: "Postup nasazení a odebrat aplikace v Service Fabric pomocí prostředí PowerShell."
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
ms.openlocfilehash: edef23a8cdab7fd0bef54456f0caabb9db273bf9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a><span data-ttu-id="1d84b-103">Nasazení a odebírat aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d84b-103">Deploy and remove applications using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d84b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d84b-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="1d84b-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d84b-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="1d84b-106">Rozhraní API FabricClient</span><span class="sxs-lookup"><span data-stu-id="1d84b-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="1d84b-107">Jednou [typu aplikaci vytvořen balíček][10], je připraven k nasazení do clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1d84b-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="1d84b-108">Nasazení zahrnuje následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="1d84b-108">Deployment involves the following three steps:</span></span>

1. <span data-ttu-id="1d84b-109">Nahrání balíčku aplikace do úložiště bitové kopie</span><span class="sxs-lookup"><span data-stu-id="1d84b-109">Upload the application package to the image store</span></span>
2. <span data-ttu-id="1d84b-110">Registrace typu aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-110">Register the application type</span></span>
3. <span data-ttu-id="1d84b-111">Vytvoření instance aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-111">Create the application instance</span></span>

<span data-ttu-id="1d84b-112">Jakmile je aplikace nasazená a je spuštěna instance v clusteru, můžete odstranit instanci aplikace a její typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-112">After an application is deployed and an instance is running in the cluster, you can delete the application instance and its application type.</span></span> <span data-ttu-id="1d84b-113">Chcete-li úplně odebrat z clusteru zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d84b-113">To completely remove an application from the cluster involves the following steps:</span></span>

1. <span data-ttu-id="1d84b-114">Odebrat (nebo odstranit) spuštěné instance aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-114">Remove (or delete) the running application instance</span></span>
2. <span data-ttu-id="1d84b-115">Zrušení registrace typu aplikace, pokud již nepotřebujete</span><span class="sxs-lookup"><span data-stu-id="1d84b-115">Unregister the application type if you no longer need it</span></span>
3. <span data-ttu-id="1d84b-116">Odebrání balíčku aplikace z úložiště bitových kopií</span><span class="sxs-lookup"><span data-stu-id="1d84b-116">Remove the application package from the image store</span></span>

<span data-ttu-id="1d84b-117">Pokud používáte [Visual Studio pro nasazování a ladění aplikací](service-fabric-publish-app-remote-cluster.md) na vaše místní vývojový cluster, jsou automaticky zpracovává všechny předchozí kroky pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1d84b-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all the preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="1d84b-118">Tento skript se nachází v *skripty* složku projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-118">This script is found in the *Scripts* folder of the application project.</span></span> <span data-ttu-id="1d84b-119">Tento článek obsahuje pozadí na co skriptu je to proto, že můžete provádět stejné operace mimo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1d84b-119">This article provides background on what that script is doing so that you can perform the same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-to-the-cluster"></a><span data-ttu-id="1d84b-120">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="1d84b-120">Connect to the cluster</span></span>
<span data-ttu-id="1d84b-121">Než spustíte všechny příkazy prostředí PowerShell v tomto článku, vždy spustit pomocí [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) se připojit ke clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1d84b-121">Before you run any PowerShell commands in this article, always start by using [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) to connect to the Service Fabric cluster.</span></span> <span data-ttu-id="1d84b-122">Pro připojení k místní vývojový cluster, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="1d84b-122">To connect to the local development cluster, run the following:</span></span>

```powershell
PS C:\>Connect-ServiceFabricCluster
```

<span data-ttu-id="1d84b-123">Příklady připojující vzdáleného clusteru nebo clusteru zabezpečené pomocí Azure Active Directory, X509 certifikáty nebo služby Windows Active Directory najdete v tématu [připojení ke clusteru zabezpečené](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="1d84b-123">For examples of connecting to a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="upload-the-application-package"></a><span data-ttu-id="1d84b-124">Nahrání balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-124">Upload the application package</span></span>
<span data-ttu-id="1d84b-125">Nahrávání balíčku aplikace vloží ho do umístění, která je přístupná pomocí interní komponenty Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1d84b-125">Uploading the application package puts it in a location that's accessible by internal Service Fabric components.</span></span>
<span data-ttu-id="1d84b-126">Pokud chcete ověřit balíček aplikace místně, použijte [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1d84b-126">If you want to verify the application package locally, use the [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="1d84b-127">[Kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz odesílá balíček aplikace do úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d84b-127">The [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command uploads the application package to the cluster image store.</span></span>
<span data-ttu-id="1d84b-128">**Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell, se použije k získání připojovacího řetězce úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-128">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="1d84b-129">Chcete-li importovat modul SDK, spusťte:</span><span class="sxs-lookup"><span data-stu-id="1d84b-129">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="1d84b-130">Předpokládejme, že sestavení a balíček aplikace s názvem *Moje_aplikace* ve Visual Studiu 2015.</span><span class="sxs-lookup"><span data-stu-id="1d84b-130">Suppose you build and package an application named *MyApplication* in Visual Studio 2015.</span></span> <span data-ttu-id="1d84b-131">Ve výchozím nastavení je název typu aplikace uvedené v ApplicationManifest.xml "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="1d84b-131">By default, the application type name listed in the ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="1d84b-132">Balíček aplikace, která obsahuje manifest nezbytné aplikace, služby manifesty a balíčků kódu/config/data, se nachází v *C:\Users\<uživatelské jméno\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="1d84b-132">The application package, which contains the necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\<username\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span> 

<span data-ttu-id="1d84b-133">Následující příkaz vypíše obsah balíčku aplikace:</span><span class="sxs-lookup"><span data-stu-id="1d84b-133">The following command lists the contents of the application package:</span></span>

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

<span data-ttu-id="1d84b-134">Pokud balíček aplikace je velký nebo má mnoho souborů, můžete [je komprimovat](service-fabric-package-apps.md#compress-a-package).</span><span class="sxs-lookup"><span data-stu-id="1d84b-134">If the application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package).</span></span> <span data-ttu-id="1d84b-135">Komprese snižuje velikost a počet souborů.</span><span class="sxs-lookup"><span data-stu-id="1d84b-135">The compression reduces the size and the number of files.</span></span>
<span data-ttu-id="1d84b-136">Vedlejším účinkem je to registrace a zrušení registrace typu aplikace je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="1d84b-136">The side effect is that registering and un-registering the application type are faster.</span></span> <span data-ttu-id="1d84b-137">Čas odeslání může být pomalejší aktuálně, zvláště pokud zahrnete čas komprimovat balíčku.</span><span class="sxs-lookup"><span data-stu-id="1d84b-137">Upload time may be slower currently, especially if you include the time to compress the package.</span></span> 

<span data-ttu-id="1d84b-138">Pokud chcete komprimovat balíček, použijte stejný [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d84b-138">To compress a package, use the same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span> <span data-ttu-id="1d84b-139">Komprese stačí samostatné z nahrávání, pomocí `SkipCopy` příznak nebo společně s operace odesílání.</span><span class="sxs-lookup"><span data-stu-id="1d84b-139">Compression can be done separate from upload, by using the `SkipCopy` flag, or together with the upload operation.</span></span> <span data-ttu-id="1d84b-140">Použití komprese na zkomprimovaného balíčku je žádná operace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-140">Applying compression on a compressed package is no-op.</span></span>
<span data-ttu-id="1d84b-141">Chcete-li dekomprimovat zkomprimovaného balíčku, použijte stejný [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s `UncompressPackage` přepínače.</span><span class="sxs-lookup"><span data-stu-id="1d84b-141">To uncompress a compressed package, use the same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command with the `UncompressPackage` switch.</span></span>

<span data-ttu-id="1d84b-142">Následující rutiny komprimaci balíček bez kopírování do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-142">The following cmdlet compresses the package without copying it to the image store.</span></span> <span data-ttu-id="1d84b-143">Nyní zahrnuje komprimované soubory pro balíček `Code` a `Config` balíčky.</span><span class="sxs-lookup"><span data-stu-id="1d84b-143">The package now includes zipped files for the `Code` and `Config` packages.</span></span> <span data-ttu-id="1d84b-144">Aplikace a služby manifestech nejsou ZIP, protože jsou potřebné pro mnoho interní operace (jako je balíček sdílení, aplikace zadejte název a verze extrakce pro určité ověření).</span><span class="sxs-lookup"><span data-stu-id="1d84b-144">The application and the service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span> <span data-ttu-id="1d84b-145">Pomocí formátu ZIP manifesty by proveďte, tyto operace neefektivní.</span><span class="sxs-lookup"><span data-stu-id="1d84b-145">Zipping the manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="1d84b-146">Pro velkých balíčků aplikací komprese trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="1d84b-146">For large application packages, the compression takes time.</span></span> <span data-ttu-id="1d84b-147">Nejlepších výsledků dosáhnete pomocí rychlé jednotky SSD.</span><span class="sxs-lookup"><span data-stu-id="1d84b-147">For best results, use a fast SSD drive.</span></span> <span data-ttu-id="1d84b-148">Časy komprese a velikost zkomprimovaného balíčku se také liší podle obsah balíčku.</span><span class="sxs-lookup"><span data-stu-id="1d84b-148">The compression times and the size of the compressed package also differ based on the package content.</span></span>
<span data-ttu-id="1d84b-149">Zde je například komprese statistiku pro některé balíčky, které vykazují počáteční a velikost zkomprimovaného balíčku s časem komprese.</span><span class="sxs-lookup"><span data-stu-id="1d84b-149">For example, here is compression statistics for some packages, which show the initial and the compressed package size, with the compression time.</span></span>

|<span data-ttu-id="1d84b-150">Počáteční velikost (MB)</span><span class="sxs-lookup"><span data-stu-id="1d84b-150">Initial size (MB)</span></span>|<span data-ttu-id="1d84b-151">Počet souborů</span><span class="sxs-lookup"><span data-stu-id="1d84b-151">File count</span></span>|<span data-ttu-id="1d84b-152">Komprese čas</span><span class="sxs-lookup"><span data-stu-id="1d84b-152">Compression Time</span></span>|<span data-ttu-id="1d84b-153">Zkomprimovaného balíčku velikost (MB)</span><span class="sxs-lookup"><span data-stu-id="1d84b-153">Compressed package size (MB)</span></span>|
|----------------:|---------:|---------------:|---------------------------:|
|<span data-ttu-id="1d84b-154">100</span><span class="sxs-lookup"><span data-stu-id="1d84b-154">100</span></span>|<span data-ttu-id="1d84b-155">100</span><span class="sxs-lookup"><span data-stu-id="1d84b-155">100</span></span>|<span data-ttu-id="1d84b-156">00:00:03.3547592</span><span class="sxs-lookup"><span data-stu-id="1d84b-156">00:00:03.3547592</span></span>|<span data-ttu-id="1d84b-157">60</span><span class="sxs-lookup"><span data-stu-id="1d84b-157">60</span></span>|
|<span data-ttu-id="1d84b-158">512</span><span class="sxs-lookup"><span data-stu-id="1d84b-158">512</span></span>|<span data-ttu-id="1d84b-159">100</span><span class="sxs-lookup"><span data-stu-id="1d84b-159">100</span></span>|<span data-ttu-id="1d84b-160">00:00:16.3850303</span><span class="sxs-lookup"><span data-stu-id="1d84b-160">00:00:16.3850303</span></span>|<span data-ttu-id="1d84b-161">307</span><span class="sxs-lookup"><span data-stu-id="1d84b-161">307</span></span>|
|<span data-ttu-id="1d84b-162">1024</span><span class="sxs-lookup"><span data-stu-id="1d84b-162">1024</span></span>|<span data-ttu-id="1d84b-163">500</span><span class="sxs-lookup"><span data-stu-id="1d84b-163">500</span></span>|<span data-ttu-id="1d84b-164">00:00:32.5907950</span><span class="sxs-lookup"><span data-stu-id="1d84b-164">00:00:32.5907950</span></span>|<span data-ttu-id="1d84b-165">615</span><span class="sxs-lookup"><span data-stu-id="1d84b-165">615</span></span>|
|<span data-ttu-id="1d84b-166">2 048</span><span class="sxs-lookup"><span data-stu-id="1d84b-166">2048</span></span>|<span data-ttu-id="1d84b-167">1000</span><span class="sxs-lookup"><span data-stu-id="1d84b-167">1000</span></span>|<span data-ttu-id="1d84b-168">00:01:04.3775554</span><span class="sxs-lookup"><span data-stu-id="1d84b-168">00:01:04.3775554</span></span>|<span data-ttu-id="1d84b-169">1231</span><span class="sxs-lookup"><span data-stu-id="1d84b-169">1231</span></span>|
|<span data-ttu-id="1d84b-170">5012</span><span class="sxs-lookup"><span data-stu-id="1d84b-170">5012</span></span>|<span data-ttu-id="1d84b-171">100</span><span class="sxs-lookup"><span data-stu-id="1d84b-171">100</span></span>|<span data-ttu-id="1d84b-172">00:02:45.2951288</span><span class="sxs-lookup"><span data-stu-id="1d84b-172">00:02:45.2951288</span></span>|<span data-ttu-id="1d84b-173">3074</span><span class="sxs-lookup"><span data-stu-id="1d84b-173">3074</span></span>|

<span data-ttu-id="1d84b-174">Jakmile je komprimovaný balíček, může být nahrán do jeden nebo více clusterů Service Fabric, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1d84b-174">Once a package is compressed, it can be uploaded to one or multiple Service Fabric clusters as needed.</span></span> <span data-ttu-id="1d84b-175">Tento mechanismus nasazení je stejný pro komprimované a nekomprimované balíčky.</span><span class="sxs-lookup"><span data-stu-id="1d84b-175">The deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="1d84b-176">Pokud je komprimovaný balíček, je jako takový uložený v úložišti clusteru bitové kopie a je nekomprimovaným na uzlu před spuštěním aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-176">If the package is compressed, it is stored as such in the cluster image store and it's uncompressed on the node before the application is run.</span></span>


<span data-ttu-id="1d84b-177">Následujícím příkladu se uloží balíček do úložiště bitové kopie do složky s názvem "MyApplicationV1":</span><span class="sxs-lookup"><span data-stu-id="1d84b-177">The following example uploads the package to the image store, into a folder named "MyApplicationV1":</span></span>

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

<span data-ttu-id="1d84b-178">**Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell, se použije k získání připojovacího řetězce úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-178">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="1d84b-179">Chcete-li importovat modul SDK, spusťte:</span><span class="sxs-lookup"><span data-stu-id="1d84b-179">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="1d84b-180">Pokud nezadáte *- ApplicationPackagePathInImageStore* parametr, balíček aplikace se zkopíruje do složky "Debug" v úložišti bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-180">If you do not specify the *-ApplicationPackagePathInImageStore* parameter, the application package is copied into the "Debug" folder in the image store.</span></span>

<span data-ttu-id="1d84b-181">Čas potřebný k nahrání balíčku se liší v závislosti na několika faktorech.</span><span class="sxs-lookup"><span data-stu-id="1d84b-181">The time it takes to upload a package differs depending on multiple factors.</span></span> <span data-ttu-id="1d84b-182">Některé tyto faktory jsou počet souborů v balíčku, balíčku velikosti a velikosti souborů.</span><span class="sxs-lookup"><span data-stu-id="1d84b-182">Some of these factors are the number of files in the package, the package size, and the file sizes.</span></span> <span data-ttu-id="1d84b-183">Rychlost sítě mezi zdrojovým počítačem a cluster Service Fabric se zároveň ovlivňuje čas odeslání.</span><span class="sxs-lookup"><span data-stu-id="1d84b-183">The network speed between the source machine and the Service Fabric cluster also impacts the upload time.</span></span> <span data-ttu-id="1d84b-184">Výchozí hodnota časového limitu pro [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) je 30 minut.</span><span class="sxs-lookup"><span data-stu-id="1d84b-184">The default timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minutes.</span></span>
<span data-ttu-id="1d84b-185">V závislosti na popisuje faktory možná budete muset zvýšit časový limit.</span><span class="sxs-lookup"><span data-stu-id="1d84b-185">Depending on the described factors, you may have to increase the timeout.</span></span> <span data-ttu-id="1d84b-186">Pokud jsou komprese balíček ve volání kopie, musíte také vzít v úvahu dobu komprese.</span><span class="sxs-lookup"><span data-stu-id="1d84b-186">If you are compressing the package in the copy call, you need to also consider the compression time.</span></span>

<span data-ttu-id="1d84b-187">V tématu [pochopit připojovací řetězec úložiště bitové kopie](service-fabric-image-store-connection-string.md) doplňující informace o úložiště image store a bitové kopie připojovací řetězec uložit.</span><span class="sxs-lookup"><span data-stu-id="1d84b-187">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

## <a name="register-the-application-package"></a><span data-ttu-id="1d84b-188">Registrace balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-188">Register the application package</span></span>
<span data-ttu-id="1d84b-189">Typ aplikace a verze deklarované v manifestu aplikace k dispozici pro použití při registraci balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-189">The application type and version declared in the application manifest become available for use when the application package is registered.</span></span> <span data-ttu-id="1d84b-190">Systém přečte nahraný v předchozím kroku balíček, ověří balíčku, zpracuje obsah balíčku a zkopíruje zpracovaná balíček do umístění interní systému.</span><span class="sxs-lookup"><span data-stu-id="1d84b-190">The system reads the package uploaded in the previous step, verifies the package, processes the package contents, and copies the processed package to an internal system location.</span></span>  

<span data-ttu-id="1d84b-191">Spustit [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) rutiny registrace typu aplikace v clusteru a zpřístupní ji pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="1d84b-191">Run the [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet to register the application type in the cluster and make it available for deployment:</span></span>

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

<span data-ttu-id="1d84b-192">"MyApplicationV1" je složka, v úložišti bitové kopie, kde se nachází balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-192">"MyApplicationV1" is the folder in the image store where the application package is located.</span></span> <span data-ttu-id="1d84b-193">Typ aplikace s názvem "MyApplicationType" a verzí "1.0.0" (jak se nacházejí v manifestu aplikace) je nyní zaregistrovaná v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d84b-193">The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) is now registered in the cluster.</span></span>

<span data-ttu-id="1d84b-194">[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) příkaz vrátí jenom po systém úspěšně zaregistrovala balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-194">The [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) command returns only after the system has successfully registered the application package.</span></span> <span data-ttu-id="1d84b-195">Jak dlouho trvá registrace závisí na velikosti a obsah balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-195">How long registration takes depends on the size and contents of the application package.</span></span> <span data-ttu-id="1d84b-196">V případě potřeby **- TimeoutSec** parametr lze zadat delší časový limit (výchozí hodnota časového limitu je 60 sekund).</span><span class="sxs-lookup"><span data-stu-id="1d84b-196">If needed, the **-TimeoutSec** parameter can be used to supply a longer timeout (the default timeout is 60 seconds).</span></span>

<span data-ttu-id="1d84b-197">Pokud máte velké aplikací, balíčků nebo pokud dochází k vypršení časových limitů, použijte **- asynchronní** parametr.</span><span class="sxs-lookup"><span data-stu-id="1d84b-197">If you have a large application package or if you are experiencing timeouts, use the **-Async** parameter.</span></span> <span data-ttu-id="1d84b-198">Příkaz vrátí při clusteru přijme příkaz registrace a zpracování pokračuje podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1d84b-198">The command returns when the cluster accepts the register command, and the processing continues as needed.</span></span>
<span data-ttu-id="1d84b-199">[Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-199">The [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="1d84b-200">Tento příkaz slouží k určení, kdy se provádí registraci.</span><span class="sxs-lookup"><span data-stu-id="1d84b-200">You can use this command to determine when the registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-the-application"></a><span data-ttu-id="1d84b-201">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-201">Create the application</span></span>
<span data-ttu-id="1d84b-202">Můžete vytvořit instanci aplikace z libovolné verze typu aplikace, který byl úspěšně zaregistrován pomocí [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1d84b-202">You can instantiate an application from any application type version that has been registered successfully by using the [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="1d84b-203">Název každé aplikace musí začínat *"fabric:"* scheme a musí být jedinečný pro každou instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-203">The name of each application must start with the *"fabric:"* scheme and must be unique for each application instance.</span></span> <span data-ttu-id="1d84b-204">Žádné výchozí služby definované v manifestu aplikace cílového typu aplikace jsou také vytvářeny.</span><span class="sxs-lookup"><span data-stu-id="1d84b-204">Any default services defined in the application manifest of the target application type are also created.</span></span>

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
<span data-ttu-id="1d84b-205">Pro danou verzi typu zaregistrovanou aplikaci lze vytvořit více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-205">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="1d84b-206">Každá instance aplikace spouští v izolaci, s vlastní pracovní adresář a proces.</span><span class="sxs-lookup"><span data-stu-id="1d84b-206">Each application instance runs in isolation, with its own work directory and process.</span></span>

<span data-ttu-id="1d84b-207">Pokud chcete zobrazit, které s názvem aplikace a služby běží v clusteru, spusťte [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) a [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) rutiny:</span><span class="sxs-lookup"><span data-stu-id="1d84b-207">To see which named apps and services are running in the cluster, run the [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) and [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:</span></span>

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

## <a name="remove-an-application"></a><span data-ttu-id="1d84b-208">Odebrání aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-208">Remove an application</span></span>
<span data-ttu-id="1d84b-209">Instance aplikace už je potřeba, trvale odstraníte ho pomocí názvu [odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1d84b-209">When an application instance is no longer needed, you can permanently remove it by name using the [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="1d84b-210">[Odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automaticky odstraní všechny služby, které patří do aplikace také a trvale odebrat všechny služby stavu.</span><span class="sxs-lookup"><span data-stu-id="1d84b-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatically removes all services that belong to the application as well, permanently removing all service state.</span></span> 

> [!WARNING]
> <span data-ttu-id="1d84b-211">Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-211">This operation cannot be reversed, and application state cannot be recovered.</span></span>

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a><span data-ttu-id="1d84b-212">Typ aplikace se zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="1d84b-212">Unregister an application type</span></span>
<span data-ttu-id="1d84b-213">Pokud konkrétní verzi typ aplikace se už nepotřebuje, by měl zrušení registrace typu aplikace pomocí [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1d84b-213">When a particular version of an application type is no longer needed, you should unregister the application type using the [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="1d84b-214">Rušení registrace aplikace nepoužívá typy uvolní prostor úložiště používá úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="1d84b-214">Unregistering unused application types releases storage space used by the image store.</span></span> <span data-ttu-id="1d84b-215">Typ aplikace může být registrace, dokud u ní jsou vytvořeny žádné aplikace a žádné čekající na vyřízení aplikace upgrady na něj odkazují.</span><span class="sxs-lookup"><span data-stu-id="1d84b-215">An application type can be unregistered as long as no applications are instantiated against it and no pending application upgrades are referencing it.</span></span>

<span data-ttu-id="1d84b-216">Spustit [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) zobrazíte typy aplikací, které jsou aktuálně registrované v clusteru:</span><span class="sxs-lookup"><span data-stu-id="1d84b-216">Run [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) to see the application types currently registered in the cluster:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

<span data-ttu-id="1d84b-217">Spustit [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) se zrušit registraci typu konkrétní aplikaci:</span><span class="sxs-lookup"><span data-stu-id="1d84b-217">Run [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) to unregister a specific application type:</span></span>

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-the-image-store"></a><span data-ttu-id="1d84b-218">Balíček aplikace odebrat z úložiště bitových kopií</span><span class="sxs-lookup"><span data-stu-id="1d84b-218">Remove an application package from the image store</span></span>
<span data-ttu-id="1d84b-219">Pokud balíček aplikace je již nepotřebujete, odstraňte jej z úložiště bitových kopií uvolnit systémové prostředky.</span><span class="sxs-lookup"><span data-stu-id="1d84b-219">When an application package is no longer needed, you can delete it from the image store to free up system resources.</span></span>

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a><span data-ttu-id="1d84b-220">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="1d84b-220">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="1d84b-221">Kopírování ServiceFabricApplicationPackage požádá o parametr ImageStoreConnectionString</span><span class="sxs-lookup"><span data-stu-id="1d84b-221">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="1d84b-222">Prostředí Service Fabric SDK byste již měli mít správný výchozí hodnoty nastavení.</span><span class="sxs-lookup"><span data-stu-id="1d84b-222">The Service Fabric SDK environment should already have the correct defaults set up.</span></span> <span data-ttu-id="1d84b-223">Ale v případě potřeby parametr ImageStoreConnectionString pro všechny příkazy musí odpovídat hodnotě používající cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1d84b-223">But if needed, the ImageStoreConnectionString for all commands should match the value that the Service Fabric cluster is using.</span></span> <span data-ttu-id="1d84b-224">Parametr ImageStoreConnectionString můžete najít v manifestu clusteru pomocí [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) a příkazy Get-ImageStoreConnectionStringFromClusterManifest:</span><span class="sxs-lookup"><span data-stu-id="1d84b-224">You can find the ImageStoreConnectionString in the cluster manifest, retrieved using the [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="1d84b-225">**Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell, se použije k získání připojovacího řetězce úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-225">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="1d84b-226">Chcete-li importovat modul SDK, spusťte:</span><span class="sxs-lookup"><span data-stu-id="1d84b-226">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="1d84b-227">V manifestu clusteru se nachází parametr ImageStoreConnectionString:</span><span class="sxs-lookup"><span data-stu-id="1d84b-227">The ImageStoreConnectionString is found in the cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="1d84b-228">V tématu [pochopit připojovací řetězec úložiště bitové kopie](service-fabric-image-store-connection-string.md) doplňující informace o úložiště image store a bitové kopie připojovací řetězec uložit.</span><span class="sxs-lookup"><span data-stu-id="1d84b-228">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="1d84b-229">Nasazení balíčku velké aplikace</span><span class="sxs-lookup"><span data-stu-id="1d84b-229">Deploy large application package</span></span>
<span data-ttu-id="1d84b-230">Problém: [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) časového limitu pro balíček rozsáhlé aplikace (pořadí GB).</span><span class="sxs-lookup"><span data-stu-id="1d84b-230">Issue: [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) times out for a large application package (order of GB).</span></span>
<span data-ttu-id="1d84b-231">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="1d84b-231">Try:</span></span>
- <span data-ttu-id="1d84b-232">Zadat delší časový limit pro [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkazu s `TimeoutSec` parametr.</span><span class="sxs-lookup"><span data-stu-id="1d84b-232">Specify a larger timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command, with `TimeoutSec` parameter.</span></span> <span data-ttu-id="1d84b-233">Ve výchozím nastavení je časový limit 30 minut.</span><span class="sxs-lookup"><span data-stu-id="1d84b-233">By default, the timeout is 30 minutes.</span></span>
- <span data-ttu-id="1d84b-234">Zkontrolujte síťové připojení mezi zdrojovým počítačem a clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d84b-234">Check the network connection between your source machine and cluster.</span></span> <span data-ttu-id="1d84b-235">Pokud připojení je pomalý, zvažte použití na počítači s lepší síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="1d84b-235">If the connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="1d84b-236">Pokud klientský počítač je v jiné oblasti než clusteru, zvažte použití klientský počítač v oblasti blíže nebo stejné jako cluster.</span><span class="sxs-lookup"><span data-stu-id="1d84b-236">If the client machine is in another region than the cluster, consider using a client machine in a closer or same region as the cluster.</span></span>
- <span data-ttu-id="1d84b-237">Zkontrolujte, pokud jste nedosáhli externí omezení.</span><span class="sxs-lookup"><span data-stu-id="1d84b-237">Check if you are hitting external throttling.</span></span> <span data-ttu-id="1d84b-238">Například pokud je úložiště image store je nakonfigurovaná pro použití úložiště azure, nahrávání může být omezena.</span><span class="sxs-lookup"><span data-stu-id="1d84b-238">For example, when the image store is configured to use azure storage, upload may be throttled.</span></span>

<span data-ttu-id="1d84b-239">Problém: Nahrávání balíček úspěšně dokončit, ale [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="1d84b-239">Issue: Upload package completed successfully, but [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out.</span></span>
<span data-ttu-id="1d84b-240">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="1d84b-240">Try:</span></span>
- <span data-ttu-id="1d84b-241">[Komprimovat balíček](service-fabric-package-apps.md#compress-a-package) před kopírování do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-241">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span>
<span data-ttu-id="1d84b-242">Komprese snižuje velikost a počet souborů, který pak dále snižuje objem provozu a fungovat této Service Fabric musí provést.</span><span class="sxs-lookup"><span data-stu-id="1d84b-242">The compression reduces the size and the number of files, which in turn reduces the amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="1d84b-243">Operace odesílání může být pomalejší (obzvláště pokud zahrnete komprese čas), ale registrace a zrušení registrace typu aplikace je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="1d84b-243">The upload operation may be slower (especially if you include the compression time), but register and un-register the application type are faster.</span></span>
- <span data-ttu-id="1d84b-244">Zadat delší časový limit pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) s `TimeoutSec` parametr.</span><span class="sxs-lookup"><span data-stu-id="1d84b-244">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="1d84b-245">Zadejte `Async` přepínač pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="1d84b-245">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="1d84b-246">Příkaz vrátí, když clusteru přijme příkaz a registrace typu aplikace asynchronně.</span><span class="sxs-lookup"><span data-stu-id="1d84b-246">The command returns when the cluster accepts the command and the registration of the application type continues asynchronously.</span></span> <span data-ttu-id="1d84b-247">Z tohoto důvodu je nutné v tomto případě specifikovat vyšší vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="1d84b-247">For this reason, there is no need to specify a higher timeout in this case.</span></span> <span data-ttu-id="1d84b-248">[Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-248">The [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="1d84b-249">Tento příkaz slouží k určení, kdy se provádí registraci.</span><span class="sxs-lookup"><span data-stu-id="1d84b-249">You can use this command to determine when the registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="1d84b-250">Nasazení balíčku aplikace s mnoho souborů</span><span class="sxs-lookup"><span data-stu-id="1d84b-250">Deploy application package with many files</span></span>
<span data-ttu-id="1d84b-251">Problém: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) časového limitu pro balíček aplikace s mnoha souborech (pořadí tisíc).</span><span class="sxs-lookup"><span data-stu-id="1d84b-251">Issue: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="1d84b-252">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="1d84b-252">Try:</span></span>
- <span data-ttu-id="1d84b-253">[Komprimovat balíček](service-fabric-package-apps.md#compress-a-package) před kopírování do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1d84b-253">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span> <span data-ttu-id="1d84b-254">Komprese snižuje počet souborů.</span><span class="sxs-lookup"><span data-stu-id="1d84b-254">The compression reduces the number of files.</span></span>
- <span data-ttu-id="1d84b-255">Zadat delší časový limit pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) s `TimeoutSec` parametr.</span><span class="sxs-lookup"><span data-stu-id="1d84b-255">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="1d84b-256">Zadejte `Async` přepínač pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="1d84b-256">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="1d84b-257">Příkaz vrátí, když clusteru přijme příkaz a registrace typu aplikace asynchronně.</span><span class="sxs-lookup"><span data-stu-id="1d84b-257">The command returns when the cluster accepts the command and the registration of the application type continues asynchronously.</span></span>
<span data-ttu-id="1d84b-258">Z tohoto důvodu je nutné v tomto případě specifikovat vyšší vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="1d84b-258">For this reason, there is no need to specify a higher timeout in this case.</span></span> <span data-ttu-id="1d84b-259">[Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace.</span><span class="sxs-lookup"><span data-stu-id="1d84b-259">The [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="1d84b-260">Tento příkaz slouží k určení, kdy se provádí registraci.</span><span class="sxs-lookup"><span data-stu-id="1d84b-260">You can use this command to determine when the registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a><span data-ttu-id="1d84b-261">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d84b-261">Next steps</span></span>
[<span data-ttu-id="1d84b-262">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1d84b-262">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="1d84b-263">Úvod stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1d84b-263">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="1d84b-264">Diagnostika a řešení služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1d84b-264">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="1d84b-265">Model aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1d84b-265">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
