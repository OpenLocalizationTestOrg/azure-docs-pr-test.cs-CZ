---
title: "aaaAzure nasazení aplikace Service Fabric | Microsoft Docs"
description: "Použijte hello toodeploy FabricClient rozhraní API a odebrat aplikace v Service Fabric."
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
ms.openlocfilehash: b2986b71c461f3e785ba16ec1b827fe47ad852fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a><span data-ttu-id="a2441-103">Nasazení a odebírat aplikace pomocí FabricClient</span><span class="sxs-lookup"><span data-stu-id="a2441-103">Deploy and remove applications using FabricClient</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a2441-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a2441-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="a2441-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2441-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="a2441-106">Rozhraní API FabricClient</span><span class="sxs-lookup"><span data-stu-id="a2441-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="a2441-107">Jednou [typu aplikaci vytvořen balíček][10], je připraven k nasazení do clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a2441-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="a2441-108">Nasazení zahrnuje hello následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="a2441-108">Deployment involves hello following three steps:</span></span>

1. <span data-ttu-id="a2441-109">Nahrát úložiště bitových kopií toohello balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a2441-109">Upload hello application package toohello image store</span></span>
2. <span data-ttu-id="a2441-110">Registrace typu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a2441-110">Register hello application type</span></span>
3. <span data-ttu-id="a2441-111">Vytvoření instance aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a2441-111">Create hello application instance</span></span>

<span data-ttu-id="a2441-112">Jakmile je aplikace nasazená a instance běží v clusteru hello, můžete odstranit hello instanci aplikace a její typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2441-112">After an application is deployed and an instance is running in hello cluster, you can delete hello application instance and its application type.</span></span> <span data-ttu-id="a2441-113">toocompletely odebrat aplikaci z clusteru hello zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a2441-113">toocompletely remove an application from hello cluster involves hello following steps:</span></span>

1. <span data-ttu-id="a2441-114">Odebrat (nebo odstranit) hello spuštěna instance aplikace</span><span class="sxs-lookup"><span data-stu-id="a2441-114">Remove (or delete) hello running application instance</span></span>
2. <span data-ttu-id="a2441-115">Zrušení registrace typu aplikace hello, pokud již nepotřebujete</span><span class="sxs-lookup"><span data-stu-id="a2441-115">Unregister hello application type if you no longer need it</span></span>
3. <span data-ttu-id="a2441-116">Odebrání balíčku aplikace hello z úložiště bitových kopií hello</span><span class="sxs-lookup"><span data-stu-id="a2441-116">Remove hello application package from hello image store</span></span>

<span data-ttu-id="a2441-117">Pokud používáte [Visual Studio pro nasazování a ladění aplikací](service-fabric-publish-app-remote-cluster.md) na vaše místní vývojový cluster všechny předchozí kroky hello se zpracovávají automaticky pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a2441-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all hello preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="a2441-118">Tento skript se nachází v hello *skripty* složku projekt aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a2441-118">This script is found in hello *Scripts* folder of hello application project.</span></span> <span data-ttu-id="a2441-119">Tento článek obsahuje pozadí na co skriptu je to, aby mohli provést hello stejné operace mimo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2441-119">This article provides background on what that script is doing so that you can perform hello same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-toohello-cluster"></a><span data-ttu-id="a2441-120">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="a2441-120">Connect toohello cluster</span></span>
<span data-ttu-id="a2441-121">Připojte toohello cluster tak, že vytvoříte [FabricClient](/dotnet/api/system.fabric.fabricclient) instance předtím, než spustíte jakoukoli hello příklady kódu v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a2441-121">Connect toohello cluster by creating a [FabricClient](/dotnet/api/system.fabric.fabricclient) instance before you run any of hello code examples in this article.</span></span> <span data-ttu-id="a2441-122">Příklady připojování tooa místního vývojového clusteru nebo vzdálený cluster nebo clusteru zabezpečené pomocí Azure Active Directory, X509 certifikáty nebo služby Windows Active Directory najdete v tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span><span class="sxs-lookup"><span data-stu-id="a2441-122">For examples of connecting tooa local development cluster or a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span></span> <span data-ttu-id="a2441-123">tooconnect toohello místního vývojového clusteru, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="a2441-123">tooconnect toohello local development cluster, run hello following:</span></span>

```csharp
// Connect toohello local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-hello-application-package"></a><span data-ttu-id="a2441-124">Nahrání balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a2441-124">Upload hello application package</span></span>
<span data-ttu-id="a2441-125">Předpokládejme, že sestavení a balíček aplikace s názvem *Moje_aplikace* v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2441-125">Suppose you build and package an application named *MyApplication* in Visual Studio.</span></span> <span data-ttu-id="a2441-126">Ve výchozím nastavení je název typu aplikace hello uvedené v hello ApplicationManifest.xml "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="a2441-126">By default, hello application type name listed in hello ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="a2441-127">Hello balíček aplikace, která obsahuje manifest hello nezbytné aplikace, služby manifesty a balíčků kódu/config/data, se nachází v *C:\Users\&lt; uživatelské jméno&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="a2441-127">hello application package, which contains hello necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\&lt;username&gt;\Documents\Visual Studio 2017\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span>

<span data-ttu-id="a2441-128">Odesílání balíčku aplikace hello vloží ho do umístění, která je přístupná pomocí hello interní komponenty Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a2441-128">Uploading hello application package puts it in a location that's accessible by hello internal Service Fabric components.</span></span> <span data-ttu-id="a2441-129">Service Fabric ověřuje balíčku aplikace hello během registrace hello balíčku aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a2441-129">Service Fabric verifies hello application package during hello registration of hello application package.</span></span> <span data-ttu-id="a2441-130">Pokud však chcete tooverify hello balíčku aplikace místně (tj. před nahráním), použít hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="a2441-130">However, if you want tooverify hello application package locally (i.e., before uploading), use hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="a2441-131">Hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) rozhraní API ukládání balíčku aplikace hello, toohello úložiště bitových kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="a2441-131">hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API uploads hello application package toohello cluster image store.</span></span> 

<span data-ttu-id="a2441-132">Pokud balíček aplikace hello je velký nebo má mnoho souborů, můžete [je komprimovat](service-fabric-package-apps.md#compress-a-package) a zkopírujte ho toohello image store pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a2441-132">If hello application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package) and copy it toohello image store using PowerShell.</span></span> <span data-ttu-id="a2441-133">Hello komprese snižuje velikost hello a hello počet souborů.</span><span class="sxs-lookup"><span data-stu-id="a2441-133">hello compression reduces hello size and hello number of files.</span></span>

<span data-ttu-id="a2441-134">V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="a2441-134">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

## <a name="register-hello-application-package"></a><span data-ttu-id="a2441-135">Registrace balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a2441-135">Register hello application package</span></span>
<span data-ttu-id="a2441-136">v manifestu aplikace hello k dispozici pro použití při registraci balíčku aplikace hello deklarována Hello typ a verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2441-136">hello application type and version declared in hello application manifest become available for use when hello application package is registered.</span></span> <span data-ttu-id="a2441-137">Hello systému přečte nahraný v předchozím kroku hello hello balíček, ověří hello balíčku, zpracuje hello obsah balíčku a zkopíruje hello zpracovat balíček tooan systému interní umístění.</span><span class="sxs-lookup"><span data-stu-id="a2441-137">hello system reads hello package uploaded in hello previous step, verifies hello package, processes hello package contents, and copies hello processed package tooan internal system location.</span></span>  

<span data-ttu-id="a2441-138">Hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API registry hello typu aplikace v clusteru hello a zpřístupní ji pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="a2441-138">hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registers hello application type in hello cluster and make it available for deployment.</span></span>

<span data-ttu-id="a2441-139">Hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) rozhraní API poskytuje informace o všech typech aplikací byl úspěšně zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="a2441-139">hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API provides information about all successfully registered application types.</span></span> <span data-ttu-id="a2441-140">Toodetermine toto rozhraní API můžete použít, pokud se provádí registraci hello.</span><span class="sxs-lookup"><span data-stu-id="a2441-140">You can use this API toodetermine when hello registration is done.</span></span>

## <a name="create-an-application-instance"></a><span data-ttu-id="a2441-141">Vytvoření instance aplikace</span><span class="sxs-lookup"><span data-stu-id="a2441-141">Create an application instance</span></span>
<span data-ttu-id="a2441-142">Můžete vytvořit instanci aplikace z jakéhokoli typu aplikace, který byl úspěšně zaregistrován pomocí hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-142">You can instantiate an application from any application type that has been registered successfully by using hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span></span> <span data-ttu-id="a2441-143">Hello názvu každé aplikace musí začínat hello *"fabric:"* scheme a musí být jedinečný pro každou instanci aplikace (v rámci clusteru).</span><span class="sxs-lookup"><span data-stu-id="a2441-143">hello name of each application must start with hello *"fabric:"* scheme and must be unique for each application instance (within a cluster).</span></span> <span data-ttu-id="a2441-144">Všechny výchozí služby definované v manifestu aplikace hello typu hello cílové aplikace jsou také vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a2441-144">Any default services defined in hello application manifest of hello target application type are also created.</span></span>

<span data-ttu-id="a2441-145">Pro danou verzi typu zaregistrovanou aplikaci lze vytvořit více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2441-145">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="a2441-146">Každá instance aplikace spouští v izolaci, s vlastní pracovní adresář a sadu procesy.</span><span class="sxs-lookup"><span data-stu-id="a2441-146">Each application instance runs in isolation, with its own working directory and set of processes.</span></span>

<span data-ttu-id="a2441-147">toosee, které s názvem služby a aplikace běží v clusteru hello, spusťte hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) a [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-147">toosee which named applications and services are running in hello cluster, run hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) and [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) APIs.</span></span>

## <a name="create-a-service-instance"></a><span data-ttu-id="a2441-148">Vytvoření instance služby</span><span class="sxs-lookup"><span data-stu-id="a2441-148">Create a service instance</span></span>
<span data-ttu-id="a2441-149">Můžete vytvořit instanci služby z typu služby pomocí hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-149">You can instantiate a service from a service type using hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span></span>  <span data-ttu-id="a2441-150">Pokud služba hello je deklarován jako výchozí služba v manifestu aplikace hello, je vytvořena při vytváření instance aplikace hello instance hello služby.</span><span class="sxs-lookup"><span data-stu-id="a2441-150">If hello service is declared as a default service in hello application manifest, hello service is instantiated when hello application is instantiated.</span></span>  <span data-ttu-id="a2441-151">Volání hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) rozhraní API pro službu, která je již vytvořena instance vrátí k výjimce typu FabricException obsahující chybový kód s hodnotou FabricErrorCode.ServiceAlreadyExists.</span><span class="sxs-lookup"><span data-stu-id="a2441-151">Calling hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API for a service that is already instantiated will return an exception of type FabricException containing an error code with a value of FabricErrorCode.ServiceAlreadyExists.</span></span>

## <a name="remove-a-service-instance"></a><span data-ttu-id="a2441-152">Odebrat instanci služby</span><span class="sxs-lookup"><span data-stu-id="a2441-152">Remove a service instance</span></span>
<span data-ttu-id="a2441-153">Pokud instance služby již nepotřebujete, můžete jej odebrat z hello spuštěna instance aplikace pomocí volání hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-153">When a service instance is no longer needed, you can remove it from hello running application instance by calling hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span></span>  

> [!WARNING]
> <span data-ttu-id="a2441-154">Tato operace se nedá vrátit a nelze ji obnovit stav služby.</span><span class="sxs-lookup"><span data-stu-id="a2441-154">This operation cannot be reversed, and service state cannot be recovered.</span></span>

## <a name="remove-an-application-instance"></a><span data-ttu-id="a2441-155">Odebrání instance aplikace</span><span class="sxs-lookup"><span data-stu-id="a2441-155">Remove an application instance</span></span>
<span data-ttu-id="a2441-156">Instance aplikace už je potřeba, trvale odstraníte ho podle názvu pomocí hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-156">When an application instance is no longer needed, you can permanently remove it by name using hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span></span> <span data-ttu-id="a2441-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automaticky odstraní všechny služby, které patří toohello aplikace také a trvale odebrat všechny služby stavu.</span><span class="sxs-lookup"><span data-stu-id="a2441-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatically removes all services that belong toohello application as well, permanently removing all service state.</span></span>

> [!WARNING]
> <span data-ttu-id="a2441-158">Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2441-158">This operation cannot be reversed, and application state cannot be recovered.</span></span>

## <a name="unregister-an-application-type"></a><span data-ttu-id="a2441-159">Typ aplikace se zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="a2441-159">Unregister an application type</span></span>
<span data-ttu-id="a2441-160">Pokud konkrétní verzi typ aplikace se už nepotřebuje, by měl zrušit registraci této konkrétní verze typu aplikace hello pomocí hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-160">When a particular version of an application type is no longer needed, you should unregister that particular version of hello application type using hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span></span> <span data-ttu-id="a2441-161">Zrušení registrace nepoužívané verze aplikace typy verzích prostoru úložiště používá úložiště bitových kopií hello.</span><span class="sxs-lookup"><span data-stu-id="a2441-161">Unregistering unused versions of application types releases storage space used by hello image store.</span></span> <span data-ttu-id="a2441-162">Verze typu aplikace může být zrušit registraci, dokud instance žádné aplikace. u této verze typu aplikace hello a nebyl upgradován čekající aplikace odkazují na tuto verzi typ aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a2441-162">A version of an application type can be unregistered as long as no applications are instantiated against that version of hello application type and no pending application upgrades are referencing that version of hello application type.</span></span>

## <a name="remove-an-application-package-from-hello-image-store"></a><span data-ttu-id="a2441-163">Balíček aplikace odebrat z úložiště bitových kopií hello</span><span class="sxs-lookup"><span data-stu-id="a2441-163">Remove an application package from hello image store</span></span>
<span data-ttu-id="a2441-164">Balíček aplikace už je potřeba, musíte jej odstranit ze hello image store toofree až systémové prostředky pomocí hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2441-164">When an application package is no longer needed, you can delete it from hello image store toofree up system resources using hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a2441-165">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a2441-165">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="a2441-166">Kopírování ServiceFabricApplicationPackage požádá o parametr ImageStoreConnectionString</span><span class="sxs-lookup"><span data-stu-id="a2441-166">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="a2441-167">prostředí Service Fabric SDK Hello byste již měli mít hello správný, nastavit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a2441-167">hello Service Fabric SDK environment should already have hello correct defaults set up.</span></span> <span data-ttu-id="a2441-168">Ale v případě potřeby pro všechny příkazy hello parametr ImageStoreConnectionString musí odpovídat hello hodnotu této hello používá cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a2441-168">But if needed, hello ImageStoreConnectionString for all commands should match hello value that hello Service Fabric cluster is using.</span></span> <span data-ttu-id="a2441-169">Hello parametr ImageStoreConnectionString můžete najít v manifestu clusteru hello načten pomocí hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) a Get-ImageStoreConnectionStringFromClusterManifest příkazy:</span><span class="sxs-lookup"><span data-stu-id="a2441-169">You can find hello ImageStoreConnectionString in hello cluster manifest, retrieved using hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="a2441-170">Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="a2441-170">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="a2441-171">tooimport hello SDK modul, spusťte:</span><span class="sxs-lookup"><span data-stu-id="a2441-171">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


<span data-ttu-id="a2441-172">Parametr ImageStoreConnectionString Hello se nachází v manifestu clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="a2441-172">hello ImageStoreConnectionString is found in hello cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="a2441-173">V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="a2441-173">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="a2441-174">Nasazení balíčku velké aplikace</span><span class="sxs-lookup"><span data-stu-id="a2441-174">Deploy large application package</span></span>
<span data-ttu-id="a2441-175">Problém: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) rozhraní API časového limitu pro balíček rozsáhlé aplikace (pořadí GB).</span><span class="sxs-lookup"><span data-stu-id="a2441-175">Issue: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API times out for a large application package (order of GB).</span></span>
<span data-ttu-id="a2441-176">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="a2441-176">Try:</span></span>
- <span data-ttu-id="a2441-177">Zadat delší časový limit pro [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) metody s `timeout` parametr.</span><span class="sxs-lookup"><span data-stu-id="a2441-177">Specify a larger timeout for [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) method, with `timeout` parameter.</span></span> <span data-ttu-id="a2441-178">Ve výchozím nastavení je hello časový limit 30 minut.</span><span class="sxs-lookup"><span data-stu-id="a2441-178">By default, hello timeout is 30 minutes.</span></span>
- <span data-ttu-id="a2441-179">Zkontrolujte hello síťové připojení mezi zdrojovým počítačem a clusteru.</span><span class="sxs-lookup"><span data-stu-id="a2441-179">Check hello network connection between your source machine and cluster.</span></span> <span data-ttu-id="a2441-180">Pokud jde o pomalé připojení hello, zvažte použití na počítači s lepší síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="a2441-180">If hello connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="a2441-181">Pokud hello klientský počítač je v jiné oblasti než hello clusteru, zvažte použití klientský počítač v oblasti blíže nebo stejné jako hello cluster.</span><span class="sxs-lookup"><span data-stu-id="a2441-181">If hello client machine is in another region than hello cluster, consider using a client machine in a closer or same region as hello cluster.</span></span>
- <span data-ttu-id="a2441-182">Zkontrolujte, pokud jste nedosáhli externí omezení.</span><span class="sxs-lookup"><span data-stu-id="a2441-182">Check if you are hitting external throttling.</span></span> <span data-ttu-id="a2441-183">Například v případě je úložiště image hello nakonfigurované toouse azure storage, může nahrávání omezeny.</span><span class="sxs-lookup"><span data-stu-id="a2441-183">For example, when hello image store is configured toouse azure storage, upload may be throttled.</span></span>

<span data-ttu-id="a2441-184">Problém: Nahrávání balíček úspěšně dokončit, ale [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API časového limitu. Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="a2441-184">Issue: Upload package completed successfully, but [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API times out. Try:</span></span>
- <span data-ttu-id="a2441-185">[Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="a2441-185">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span>
<span data-ttu-id="a2441-186">Hello komprese snižuje velikost hello a musíte provést hello počet souborů, která zase snižuje hello objem provozu a že Service Fabric fungovat.</span><span class="sxs-lookup"><span data-stu-id="a2441-186">hello compression reduces hello size and hello number of files, which in turn reduces hello amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="a2441-187">operace nahrávání Hello může být pomalejší (obzvláště pokud zahrnete hello komprese čas), ale registrace a zrušení registrace typu aplikace hello je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="a2441-187">hello upload operation may be slower (especially if you include hello compression time), but register and un-register hello application type are faster.</span></span>
- <span data-ttu-id="a2441-188">Zadat delší časový limit pro [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API pomocí `timeout` parametr.</span><span class="sxs-lookup"><span data-stu-id="a2441-188">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API with `timeout` parameter.</span></span>

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="a2441-189">Nasazení balíčku aplikace s mnoho souborů</span><span class="sxs-lookup"><span data-stu-id="a2441-189">Deploy application package with many files</span></span>
<span data-ttu-id="a2441-190">Problém: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) časového limitu pro balíček aplikace s mnoha souborech (pořadí tisíc).</span><span class="sxs-lookup"><span data-stu-id="a2441-190">Issue: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="a2441-191">Vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="a2441-191">Try:</span></span>
- <span data-ttu-id="a2441-192">[Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="a2441-192">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span> <span data-ttu-id="a2441-193">Hello komprese snižuje hello počet souborů.</span><span class="sxs-lookup"><span data-stu-id="a2441-193">hello compression reduces hello number of files.</span></span>
- <span data-ttu-id="a2441-194">Zadat delší časový limit pro [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) s `timeout` parametr.</span><span class="sxs-lookup"><span data-stu-id="a2441-194">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) with `timeout` parameter.</span></span>

## <a name="code-example"></a><span data-ttu-id="a2441-195">Příklad kódu</span><span class="sxs-lookup"><span data-stu-id="a2441-195">Code example</span></span>
<span data-ttu-id="a2441-196">Hello následující příklad zkopíruje úložišti aplikace toohello balíček bitové kopie, zřídí hello typu aplikace, vytvoří instanci aplikace, vytvoří instanci služby, odebere instanci aplikace hello, zrušení zřizuje hello typ aplikace, a balíček aplikace hello odstraní z úložiště bitových kopií hello.</span><span class="sxs-lookup"><span data-stu-id="a2441-196">hello following example copies an application package toohello image store, provisions hello application type, creates an application instance, creates a service instance, removes hello application instance, un-provisions hello application type, and deletes hello application package from hello image store.</span></span>

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

    // Connect toohello cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy hello application package tooa location in hello image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied too{0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy tooImage Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision hello application.  "MyApplicationV1" is hello folder in hello image store where hello application package is located. 
    // hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) 
    // is now registered in hello cluster.            
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

    //  Create hello application instance.
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

    // Create hello stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create hello service instance.  If hello service is declared as a default service in hello ApplicationManifest.xml,
    // hello service instance is already running and this call will fail.
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

    // Delete an application instance from hello application type.
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

    // Un-provision hello application type.
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

    // Delete hello application package from a location in hello image store.
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

## <a name="next-steps"></a><span data-ttu-id="a2441-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2441-197">Next steps</span></span>
[<span data-ttu-id="a2441-198">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a2441-198">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="a2441-199">Úvod stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a2441-199">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="a2441-200">Diagnostika a řešení služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a2441-200">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="a2441-201">Model aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a2441-201">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
