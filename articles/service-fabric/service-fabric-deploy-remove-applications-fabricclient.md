---
title: "Nasazení aplikace Azure Service Fabric | Microsoft Docs"
description: "Rozhraní API FabricClient použijte k nasazení a odebrat aplikace v Service Fabric."
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
ms.date: 07/07/2017
ms.author: ryanwi
ms.openlocfilehash: 2e4ca1069b4e8e473b26b790e81770b41e25ff50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a><span data-ttu-id="ca78d-103">Nasazení a odebírat aplikace pomocí FabricClient</span><span class="sxs-lookup"><span data-stu-id="ca78d-103">Deploy and remove applications using FabricClient</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca78d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca78d-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="ca78d-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca78d-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="ca78d-106">Rozhraní API FabricClient</span><span class="sxs-lookup"><span data-stu-id="ca78d-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="ca78d-107">Jednou [typu aplikaci vytvořen balíček][10], je připraven k nasazení do clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ca78d-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="ca78d-108">Nasazení zahrnuje následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="ca78d-108">Deployment involves the following three steps:</span></span>

1. <span data-ttu-id="ca78d-109">Nahrání balíčku aplikace do úložiště bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ca78d-109">Upload the application package to the image store</span></span>
2. <span data-ttu-id="ca78d-110">Registrace typu aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-110">Register the application type</span></span>
3. <span data-ttu-id="ca78d-111">Vytvoření instance aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-111">Create the application instance</span></span>

<span data-ttu-id="ca78d-112">Jakmile je aplikace nasazená a je spuštěna instance v clusteru, můžete odstranit instanci aplikace a její typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-112">After an application is deployed and an instance is running in the cluster, you can delete the application instance and its application type.</span></span> <span data-ttu-id="ca78d-113">Chcete-li úplně odebrat z clusteru zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca78d-113">To completely remove an application from the cluster involves the following steps:</span></span>

1. <span data-ttu-id="ca78d-114">Odebrat (nebo odstranit) spuštěné instance aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-114">Remove (or delete) the running application instance</span></span>
2. <span data-ttu-id="ca78d-115">Zrušení registrace typu aplikace, pokud již nepotřebujete</span><span class="sxs-lookup"><span data-stu-id="ca78d-115">Unregister the application type if you no longer need it</span></span>
3. <span data-ttu-id="ca78d-116">Odebrání balíčku aplikace z úložiště bitových kopií</span><span class="sxs-lookup"><span data-stu-id="ca78d-116">Remove the application package from the image store</span></span>

<span data-ttu-id="ca78d-117">Pokud používáte [Visual Studio pro nasazování a ladění aplikací](service-fabric-publish-app-remote-cluster.md) na vaše místní vývojový cluster, jsou automaticky zpracovává všechny předchozí kroky pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ca78d-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all the preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="ca78d-118">Tento skript se nachází v *skripty* složku projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-118">This script is found in the *Scripts* folder of the application project.</span></span> <span data-ttu-id="ca78d-119">Tento článek obsahuje pozadí na co skriptu je to proto, že můžete provádět stejné operace mimo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca78d-119">This article provides background on what that script is doing so that you can perform the same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-to-the-cluster"></a><span data-ttu-id="ca78d-120">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="ca78d-120">Connect to the cluster</span></span>
<span data-ttu-id="ca78d-121">Připojte se ke clusteru tak, že vytvoříte [FabricClient](/dotnet/api/system.fabric.fabricclient) instance předtím, než spustíte jakoukoli příklady kódu v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ca78d-121">Connect to the cluster by creating a [FabricClient](/dotnet/api/system.fabric.fabricclient) instance before you run any of the code examples in this article.</span></span> <span data-ttu-id="ca78d-122">Příklady připojení místního vývojového clusteru nebo vzdálený cluster nebo clusteru zabezpečené pomocí Azure Active Directory, X509 certifikáty nebo služby Windows Active Directory najdete v tématu [připojení ke clusteru zabezpečené](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span><span class="sxs-lookup"><span data-stu-id="ca78d-122">For examples of connecting to a local development cluster or a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span></span> <span data-ttu-id="ca78d-123">Pro připojení k místní vývojový cluster, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="ca78d-123">To connect to the local development cluster, run the following:</span></span>

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a><span data-ttu-id="ca78d-124">Nahrání balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-124">Upload the application package</span></span>
<span data-ttu-id="ca78d-125">Předpokládejme, že sestavení a balíček aplikace s názvem *Moje_aplikace* v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca78d-125">Suppose you build and package an application named *MyApplication* in Visual Studio.</span></span> <span data-ttu-id="ca78d-126">Ve výchozím nastavení je název typu aplikace uvedené v ApplicationManifest.xml "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="ca78d-126">By default, the application type name listed in the ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="ca78d-127">Balíček aplikace, která obsahuje manifest nezbytné aplikace, služby manifesty a balíčků kódu/config/data, se nachází v *C:\Users\&lt; uživatelské jméno&gt;\Documents\Visual Studio 2017\Projects\MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="ca78d-127">The application package, which contains the necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\&lt;username&gt;\Documents\Visual Studio 2017\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span>

<span data-ttu-id="ca78d-128">Nahrávání balíčku aplikace vloží ho do umístění, která je přístupná pomocí interní komponenty Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ca78d-128">Uploading the application package puts it in a location that's accessible by the internal Service Fabric components.</span></span> <span data-ttu-id="ca78d-129">Service Fabric ověřuje balíčku aplikace během registrace balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-129">Service Fabric verifies the application package during the registration of the application package.</span></span> <span data-ttu-id="ca78d-130">Pokud chcete ověřit balíček aplikace místně (tj. před nahráním), ale použít [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ca78d-130">However, if you want to verify the application package locally (i.e., before uploading), use the [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ca78d-131">[CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) rozhraní API odesílá balíček aplikace do úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="ca78d-131">The [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API uploads the application package to the cluster image store.</span></span> 

<span data-ttu-id="ca78d-132">Pokud balíček aplikace je velký nebo má mnoho souborů, můžete [je komprimovat](service-fabric-package-apps.md#compress-a-package) a zkopírujte jej do úložiště bitové kopie pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ca78d-132">If the application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package) and copy it to the image store using PowerShell.</span></span> <span data-ttu-id="ca78d-133">Komprese snižuje velikost a počet souborů.</span><span class="sxs-lookup"><span data-stu-id="ca78d-133">The compression reduces the size and the number of files.</span></span>

<span data-ttu-id="ca78d-134">V tématu [pochopit připojovací řetězec úložiště bitové kopie](service-fabric-image-store-connection-string.md) doplňující informace o úložiště image store a bitové kopie připojovací řetězec uložit.</span><span class="sxs-lookup"><span data-stu-id="ca78d-134">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

## <a name="register-the-application-package"></a><span data-ttu-id="ca78d-135">Registrace balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-135">Register the application package</span></span>
<span data-ttu-id="ca78d-136">Typ aplikace a verze deklarované v manifestu aplikace k dispozici pro použití při registraci balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-136">The application type and version declared in the application manifest become available for use when the application package is registered.</span></span> <span data-ttu-id="ca78d-137">Systém přečte nahraný v předchozím kroku balíček, ověří balíčku, zpracuje obsah balíčku a zkopíruje zpracovaná balíček do umístění interní systému.</span><span class="sxs-lookup"><span data-stu-id="ca78d-137">The system reads the package uploaded in the previous step, verifies the package, processes the package contents, and copies the processed package to an internal system location.</span></span>  

<span data-ttu-id="ca78d-138">[ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) registry rozhraní API aplikace, zadejte v clusteru a k dispozici pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="ca78d-138">The [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registers the application type in the cluster and make it available for deployment.</span></span>

<span data-ttu-id="ca78d-139">[GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) rozhraní API poskytuje informace o všech typech aplikací byl úspěšně zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="ca78d-139">The [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API provides information about all successfully registered application types.</span></span> <span data-ttu-id="ca78d-140">Toto rozhraní API můžete použít k určení, kdy se provádí registraci.</span><span class="sxs-lookup"><span data-stu-id="ca78d-140">You can use this API to determine when the registration is done.</span></span>

## <a name="create-an-application-instance"></a><span data-ttu-id="ca78d-141">Vytvoření instance aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-141">Create an application instance</span></span>
<span data-ttu-id="ca78d-142">Můžete vytvořit instanci aplikace z jakéhokoli typu aplikace, který byl úspěšně zaregistrován pomocí [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-142">You can instantiate an application from any application type that has been registered successfully by using the [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span></span> <span data-ttu-id="ca78d-143">Název každé aplikace musí začínat *"fabric:"* scheme a musí být jedinečný pro každou instanci aplikace (v rámci clusteru).</span><span class="sxs-lookup"><span data-stu-id="ca78d-143">The name of each application must start with the *"fabric:"* scheme and must be unique for each application instance (within a cluster).</span></span> <span data-ttu-id="ca78d-144">Žádné výchozí služby definované v manifestu aplikace cílového typu aplikace jsou také vytvářeny.</span><span class="sxs-lookup"><span data-stu-id="ca78d-144">Any default services defined in the application manifest of the target application type are also created.</span></span>

<span data-ttu-id="ca78d-145">Pro danou verzi typu zaregistrovanou aplikaci lze vytvořit více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-145">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="ca78d-146">Každá instance aplikace spouští v izolaci, s vlastní pracovní adresář a sadu procesy.</span><span class="sxs-lookup"><span data-stu-id="ca78d-146">Each application instance runs in isolation, with its own working directory and set of processes.</span></span>

<span data-ttu-id="ca78d-147">Pokud chcete zobrazit, které s názvem aplikace a služby běží v clusteru, spusťte [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) a [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-147">To see which named applications and services are running in the cluster, run the [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) and [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) APIs.</span></span>

## <a name="create-a-service-instance"></a><span data-ttu-id="ca78d-148">Vytvoření instance služby</span><span class="sxs-lookup"><span data-stu-id="ca78d-148">Create a service instance</span></span>
<span data-ttu-id="ca78d-149">Můžete vytvořit instanci služby z typu služby pomocí [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-149">You can instantiate a service from a service type using the [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span></span>  <span data-ttu-id="ca78d-150">Pokud služba je deklarován jako výchozí služba v manifestu aplikace, je vytvořena při vytváření instance aplikace instance služby.</span><span class="sxs-lookup"><span data-stu-id="ca78d-150">If the service is declared as a default service in the application manifest, the service is instantiated when the application is instantiated.</span></span>  <span data-ttu-id="ca78d-151">Volání [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) rozhraní API pro službu, která je již vytvořena instance vrátí k výjimce typu FabricException obsahující chybový kód s hodnotou FabricErrorCode.ServiceAlreadyExists.</span><span class="sxs-lookup"><span data-stu-id="ca78d-151">Calling the [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API for a service that is already instantiated will return an exception of type FabricException containing an error code with a value of FabricErrorCode.ServiceAlreadyExists.</span></span>

## <a name="remove-a-service-instance"></a><span data-ttu-id="ca78d-152">Odebrat instanci služby</span><span class="sxs-lookup"><span data-stu-id="ca78d-152">Remove a service instance</span></span>
<span data-ttu-id="ca78d-153">Pokud instance služby již nepotřebujete, můžete jej odebrat z spuštění instance aplikace voláním [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-153">When a service instance is no longer needed, you can remove it from the running application instance by calling the [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span></span>  

> [!WARNING]
> <span data-ttu-id="ca78d-154">Tato operace se nedá vrátit a nelze ji obnovit stav služby.</span><span class="sxs-lookup"><span data-stu-id="ca78d-154">This operation cannot be reversed, and service state cannot be recovered.</span></span>

## <a name="remove-an-application-instance"></a><span data-ttu-id="ca78d-155">Odebrání instance aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-155">Remove an application instance</span></span>
<span data-ttu-id="ca78d-156">Instance aplikace už je potřeba, trvale odstraníte ho pomocí názvu [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-156">When an application instance is no longer needed, you can permanently remove it by name using the [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span></span> <span data-ttu-id="ca78d-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automaticky odstraní všechny služby, které patří do aplikace také a trvale odebrat všechny služby stavu.</span><span class="sxs-lookup"><span data-stu-id="ca78d-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatically removes all services that belong to the application as well, permanently removing all service state.</span></span>

> [!WARNING]
> <span data-ttu-id="ca78d-158">Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-158">This operation cannot be reversed, and application state cannot be recovered.</span></span>

## <a name="unregister-an-application-type"></a><span data-ttu-id="ca78d-159">Typ aplikace se zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="ca78d-159">Unregister an application type</span></span>
<span data-ttu-id="ca78d-160">Pokud konkrétní verzi typ aplikace se už nepotřebuje, by měl zrušit registraci této konkrétní verze typu aplikace pomocí [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-160">When a particular version of an application type is no longer needed, you should unregister that particular version of the application type using the [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span></span> <span data-ttu-id="ca78d-161">Zrušení registrace nepoužívané verze typů aplikace uvolní prostor úložiště používá úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="ca78d-161">Unregistering unused versions of application types releases storage space used by the image store.</span></span> <span data-ttu-id="ca78d-162">Verze typu aplikace může být zrušit registraci, dokud instance žádné aplikace. u této verze typu aplikace a nebyl upgradován čekající aplikace odkazují na tuto verzi typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca78d-162">A version of an application type can be unregistered as long as no applications are instantiated against that version of the application type and no pending application upgrades are referencing that version of the application type.</span></span>

## <a name="remove-an-application-package-from-the-image-store"></a><span data-ttu-id="ca78d-163">Balíček aplikace odebrat z úložiště bitových kopií</span><span class="sxs-lookup"><span data-stu-id="ca78d-163">Remove an application package from the image store</span></span>
<span data-ttu-id="ca78d-164">Balíček aplikace už je potřeba, můžete jej odstranit z úložiště bitových kopií uvolnit systémové prostředky pomocí [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca78d-164">When an application package is no longer needed, you can delete it from the image store to free up system resources using the [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ca78d-165">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ca78d-165">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="ca78d-166">Kopírování ServiceFabricApplicationPackage požádá o parametr ImageStoreConnectionString</span><span class="sxs-lookup"><span data-stu-id="ca78d-166">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="ca78d-167">Prostředí Service Fabric SDK byste již měli mít správný výchozí hodnoty nastavení.</span><span class="sxs-lookup"><span data-stu-id="ca78d-167">The Service Fabric SDK environment should already have the correct defaults set up.</span></span> <span data-ttu-id="ca78d-168">Ale v případě potřeby parametr ImageStoreConnectionString pro všechny příkazy musí odpovídat hodnotě používající cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ca78d-168">But if needed, the ImageStoreConnectionString for all commands should match the value that the Service Fabric cluster is using.</span></span> <span data-ttu-id="ca78d-169">Parametr ImageStoreConnectionString můžete najít v manifestu clusteru pomocí [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) a příkazy Get-ImageStoreConnectionStringFromClusterManifest:</span><span class="sxs-lookup"><span data-stu-id="ca78d-169">You can find the ImageStoreConnectionString in the cluster manifest, retrieved using the [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="ca78d-170">**Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell, se použije k získání připojovacího řetězce úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ca78d-170">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="ca78d-171">Chcete-li importovat modul SDK, spusťte:</span><span class="sxs-lookup"><span data-stu-id="ca78d-171">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


<span data-ttu-id="ca78d-172">V manifestu clusteru se nachází parametr ImageStoreConnectionString:</span><span class="sxs-lookup"><span data-stu-id="ca78d-172">The ImageStoreConnectionString is found in the cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="ca78d-173">V tématu [pochopit připojovací řetězec úložiště bitové kopie](service-fabric-image-store-connection-string.md) doplňující informace o úložiště image store a bitové kopie připojovací řetězec uložit.</span><span class="sxs-lookup"><span data-stu-id="ca78d-173">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="ca78d-174">Nasazení balíčku velké aplikace</span><span class="sxs-lookup"><span data-stu-id="ca78d-174">Deploy large application package</span></span>
<span data-ttu-id="ca78d-175">Problém: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) rozhraní API časového limitu pro balíček rozsáhlé aplikace (pořadí GB).</span><span class="sxs-lookup"><span data-stu-id="ca78d-175">Issue: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API times out for a large application package (order of GB).</span></span>
<span data-ttu-id="ca78d-176">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="ca78d-176">Try:</span></span>
- <span data-ttu-id="ca78d-177">Zadat delší časový limit pro [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) metody s `timeout` parametr.</span><span class="sxs-lookup"><span data-stu-id="ca78d-177">Specify a larger timeout for [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) method, with `timeout` parameter.</span></span> <span data-ttu-id="ca78d-178">Ve výchozím nastavení je časový limit 30 minut.</span><span class="sxs-lookup"><span data-stu-id="ca78d-178">By default, the timeout is 30 minutes.</span></span>
- <span data-ttu-id="ca78d-179">Zkontrolujte síťové připojení mezi zdrojovým počítačem a clusteru.</span><span class="sxs-lookup"><span data-stu-id="ca78d-179">Check the network connection between your source machine and cluster.</span></span> <span data-ttu-id="ca78d-180">Pokud připojení je pomalý, zvažte použití na počítači s lepší síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="ca78d-180">If the connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="ca78d-181">Pokud klientský počítač je v jiné oblasti než clusteru, zvažte použití klientský počítač v oblasti blíže nebo stejné jako cluster.</span><span class="sxs-lookup"><span data-stu-id="ca78d-181">If the client machine is in another region than the cluster, consider using a client machine in a closer or same region as the cluster.</span></span>
- <span data-ttu-id="ca78d-182">Zkontrolujte, pokud jste nedosáhli externí omezení.</span><span class="sxs-lookup"><span data-stu-id="ca78d-182">Check if you are hitting external throttling.</span></span> <span data-ttu-id="ca78d-183">Například pokud je úložiště image store je nakonfigurovaná pro použití úložiště azure, nahrávání může být omezena.</span><span class="sxs-lookup"><span data-stu-id="ca78d-183">For example, when the image store is configured to use azure storage, upload may be throttled.</span></span>

<span data-ttu-id="ca78d-184">Problém: Nahrávání balíček úspěšně dokončit, ale [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API časového limitu.</span><span class="sxs-lookup"><span data-stu-id="ca78d-184">Issue: Upload package completed successfully, but [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API times out.</span></span>
<span data-ttu-id="ca78d-185">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="ca78d-185">Try:</span></span>
- <span data-ttu-id="ca78d-186">[Komprimovat balíček](service-fabric-package-apps.md#compress-a-package) před kopírování do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ca78d-186">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span>
<span data-ttu-id="ca78d-187">Komprese snižuje velikost a počet souborů, který pak dále snižuje objem provozu a fungovat této Service Fabric musí provést.</span><span class="sxs-lookup"><span data-stu-id="ca78d-187">The compression reduces the size and the number of files, which in turn reduces the amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="ca78d-188">Operace odesílání může být pomalejší (obzvláště pokud zahrnete komprese čas), ale registrace a zrušení registrace typu aplikace je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="ca78d-188">The upload operation may be slower (especially if you include the compression time), but register and un-register the application type are faster.</span></span>
- <span data-ttu-id="ca78d-189">Zadat delší časový limit pro [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API pomocí `timeout` parametr.</span><span class="sxs-lookup"><span data-stu-id="ca78d-189">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API with `timeout` parameter.</span></span>

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="ca78d-190">Nasazení balíčku aplikace s mnoho souborů</span><span class="sxs-lookup"><span data-stu-id="ca78d-190">Deploy application package with many files</span></span>
<span data-ttu-id="ca78d-191">Problém: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) časového limitu pro balíček aplikace s mnoha souborech (pořadí tisíc).</span><span class="sxs-lookup"><span data-stu-id="ca78d-191">Issue: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="ca78d-192">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="ca78d-192">Try:</span></span>
- <span data-ttu-id="ca78d-193">[Komprimovat balíček](service-fabric-package-apps.md#compress-a-package) před kopírování do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ca78d-193">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span> <span data-ttu-id="ca78d-194">Komprese snižuje počet souborů.</span><span class="sxs-lookup"><span data-stu-id="ca78d-194">The compression reduces the number of files.</span></span>
- <span data-ttu-id="ca78d-195">Zadat delší časový limit pro [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) s `timeout` parametr.</span><span class="sxs-lookup"><span data-stu-id="ca78d-195">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) with `timeout` parameter.</span></span>

## <a name="code-example"></a><span data-ttu-id="ca78d-196">Příklad kódu</span><span class="sxs-lookup"><span data-stu-id="ca78d-196">Code example</span></span>
<span data-ttu-id="ca78d-197">Následující příklad zkopíruje do úložiště bitové kopie, balíček aplikace zřizuje typ aplikace, vytvoří instanci aplikace, vytvoří instanci služby, odebere instance aplikace na zrušení zřizuje typu aplikace a odstranění balíčku aplikace z úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="ca78d-197">The following example copies an application package to the image store, provisions the application type, creates an application instance, creates a service instance, removes the application instance, un-provisions the application type, and deletes the application package from the image store.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect to the cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy the application package to a location in the image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied to {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy to Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision the application.  "MyApplicationV1" is the folder in the image store where the application package is located. 
    // The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) 
    // is now registered in the cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create the application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create the stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create the service instance.  If the service is declared as a default service in the ApplicationManifest.xml,
    // the service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from the application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision the application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete the application package from a location in the image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a><span data-ttu-id="ca78d-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ca78d-198">Next steps</span></span>
[<span data-ttu-id="ca78d-199">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ca78d-199">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="ca78d-200">Úvod stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ca78d-200">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="ca78d-201">Diagnostika a řešení služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ca78d-201">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="ca78d-202">Model aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ca78d-202">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
