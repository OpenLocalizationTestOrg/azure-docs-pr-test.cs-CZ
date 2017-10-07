---
title: aaaAzure Service Fabric Docker Compose Preview
description: "Azure Service Fabric přijímá Docker Compose toomake formát je snazší tooorchestrate stávajících kontejnerů pomocí Service Fabric. Tato podpora je aktuálně ve verzi preview."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: a60d1321fd6ef07b241a98c5ab2b8dfe5d441b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a><span data-ttu-id="5171c-104">Podpora aplikací docker Compose v Azure Service Fabric (Preview)</span><span class="sxs-lookup"><span data-stu-id="5171c-104">Docker Compose application support in Azure Service Fabric (Preview)</span></span>

<span data-ttu-id="5171c-105">Docker používá hello [docker-compose.yml](https://docs.docker.com/compose) souboru k definování více kontejneru aplikace.</span><span class="sxs-lookup"><span data-stu-id="5171c-105">Docker uses hello [docker-compose.yml](https://docs.docker.com/compose) file for defining multi-container applications.</span></span> <span data-ttu-id="5171c-106">toomake ho snadno zákazníky obeznámeni s Docker tooorchestrate existujících kontejneru aplikací v Azure Service Fabric, jsme zahrnuli preview podporu pro Docker Compose nativně hello platformy.</span><span class="sxs-lookup"><span data-stu-id="5171c-106">toomake it easy for customers familiar with Docker tooorchestrate existing container applications on Azure Service Fabric, we have included preview support for Docker Compose natively in hello platform.</span></span> <span data-ttu-id="5171c-107">Service Fabric může přijmout verze 3 nebo novější z `docker-compose.yml` soubory.</span><span class="sxs-lookup"><span data-stu-id="5171c-107">Service Fabric can accept version 3 and later of `docker-compose.yml` files.</span></span> 

<span data-ttu-id="5171c-108">Protože tato podpora je ve verzi preview, se podporuje jenom podmnožinu vytvářené direktivy.</span><span class="sxs-lookup"><span data-stu-id="5171c-108">Because this support is in preview, only a subset of Compose directives is supported.</span></span> <span data-ttu-id="5171c-109">Například upgrady aplikací nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="5171c-109">For example, application upgrades are not supported.</span></span> <span data-ttu-id="5171c-110">Můžete však vždy odebrat a nasadit aplikace místo jejich aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5171c-110">However, you can always remove and deploy applications instead of upgrading them.</span></span>

<span data-ttu-id="5171c-111">toouse to náhled, vytvořte cluster s verzi 5.7 nebo větší modulu runtime Service Fabric hello prostřednictvím portálu Azure společně s hello odpovídající SDK hello.</span><span class="sxs-lookup"><span data-stu-id="5171c-111">toouse this preview, create your cluster with version 5.7 or greater of hello Service Fabric runtime through hello Azure portal along with hello corresponding SDK.</span></span> 

> [!NOTE]
> <span data-ttu-id="5171c-112">Tato funkce je ve verzi preview a není podporována v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5171c-112">This feature is in preview and is not supported in production.</span></span>

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a><span data-ttu-id="5171c-113">Nasadit soubor Docker Compose v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5171c-113">Deploy a Docker Compose file on Service Fabric</span></span>

<span data-ttu-id="5171c-114">Hello následující příkazy vytvořit aplikace Service Fabric (s názvem `fabric:/TestContainerApp` v předchozím příkladu hello), které můžete sledovat a spravovat podobně jako všechny ostatní aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5171c-114">hello following commands create a Service Fabric application (named `fabric:/TestContainerApp` in hello preceding example), which you can monitor and manage like any other Service Fabric application.</span></span> <span data-ttu-id="5171c-115">Můžete použít název zadané aplikace hello pro dotazy na stav.</span><span class="sxs-lookup"><span data-stu-id="5171c-115">You can use hello specified application name for health queries.</span></span>

### <a name="use-powershell"></a><span data-ttu-id="5171c-116">Použití prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5171c-116">Use PowerShell</span></span>

<span data-ttu-id="5171c-117">Vytvoření aplikace Service Fabric tvoří z soubor docker-compose.yml tak, že spustíte následující příkaz v prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="5171c-117">Create a Service Fabric Compose application from a docker-compose.yml file by running hello following command in PowerShell:</span></span>

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

<span data-ttu-id="5171c-118">`RegistryUserName`a `RegistryPassword` odkazovat toohello kontejneru registru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5171c-118">`RegistryUserName` and `RegistryPassword` refer toohello container registry username and password.</span></span> <span data-ttu-id="5171c-119">Po dokončení aplikace hello, jeho stav můžete zkontrolovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5171c-119">After you've completed hello application, you can check its status by using hello following command:</span></span>

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

<span data-ttu-id="5171c-120">toodelete hello vytvářené aplikace pomocí prostředí PowerShell, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5171c-120">toodelete hello Compose application through PowerShell, use hello following command:</span></span>

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="5171c-121">Používání Azure Service Fabric rozhraní příkazového řádku (sfctl)</span><span class="sxs-lookup"><span data-stu-id="5171c-121">Use Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="5171c-122">Alternativně můžete použít hello následující služby infrastruktury rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="5171c-122">Alternatively, you can use hello following Service Fabric CLI command:</span></span>

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

<span data-ttu-id="5171c-123">Po vytvoření aplikace hello, jeho stav můžete zkontrolovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5171c-123">After you've created hello application, you can check its status by using hello following command:</span></span>

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

<span data-ttu-id="5171c-124">toodelete hello vytvářené aplikace, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5171c-124">toodelete hello Compose application, use hello following command:</span></span>

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a><span data-ttu-id="5171c-125">Podporované vytvářené direktivy</span><span class="sxs-lookup"><span data-stu-id="5171c-125">Supported Compose directives</span></span>

<span data-ttu-id="5171c-126">Tato verze preview podporuje podmnožinu možnosti konfigurace hello z formátu verze 3 vytvářené hello, včetně následujících primitiv hello:</span><span class="sxs-lookup"><span data-stu-id="5171c-126">This preview supports a subset of hello configuration options from hello Compose version 3 format, including hello following primitives:</span></span>

* <span data-ttu-id="5171c-127">Služby > nasazení > repliky</span><span class="sxs-lookup"><span data-stu-id="5171c-127">Services > Deploy > Replicas</span></span>
* <span data-ttu-id="5171c-128">Služby > nasazení > umístění > omezení</span><span class="sxs-lookup"><span data-stu-id="5171c-128">Services > Deploy > Placement > Constraints</span></span>
* <span data-ttu-id="5171c-129">Služby > nasazení > prostředky > omezení</span><span class="sxs-lookup"><span data-stu-id="5171c-129">Services > Deploy > Resources > Limits</span></span>
    * <span data-ttu-id="5171c-130">-procesoru-sdílené složky</span><span class="sxs-lookup"><span data-stu-id="5171c-130">-cpu-shares</span></span>
    * <span data-ttu-id="5171c-131">-paměti</span><span class="sxs-lookup"><span data-stu-id="5171c-131">-memory</span></span>
    * <span data-ttu-id="5171c-132">-paměti-swap</span><span class="sxs-lookup"><span data-stu-id="5171c-132">-memory-swap</span></span>
* <span data-ttu-id="5171c-133">Služby > příkazy</span><span class="sxs-lookup"><span data-stu-id="5171c-133">Services > Commands</span></span>
* <span data-ttu-id="5171c-134">Služby > prostředí</span><span class="sxs-lookup"><span data-stu-id="5171c-134">Services > Environment</span></span>
* <span data-ttu-id="5171c-135">Služby > porty</span><span class="sxs-lookup"><span data-stu-id="5171c-135">Services > Ports</span></span>
* <span data-ttu-id="5171c-136">Služby > bitové kopie</span><span class="sxs-lookup"><span data-stu-id="5171c-136">Services > Image</span></span>
* <span data-ttu-id="5171c-137">Služby > izolace (jenom pro Windows)</span><span class="sxs-lookup"><span data-stu-id="5171c-137">Services > Isolation (only for Windows)</span></span>
* <span data-ttu-id="5171c-138">Služby > protokolování > ovladačů</span><span class="sxs-lookup"><span data-stu-id="5171c-138">Services > Logging > Driver</span></span>
* <span data-ttu-id="5171c-139">Služby > protokolování > ovladače > Možnosti</span><span class="sxs-lookup"><span data-stu-id="5171c-139">Services > Logging > Driver > Options</span></span>
* <span data-ttu-id="5171c-140">Svazek & nasazení > svazku</span><span class="sxs-lookup"><span data-stu-id="5171c-140">Volume & Deploy > Volume</span></span>

<span data-ttu-id="5171c-141">Nastavení clusteru hello pro vynucení omezení prostředků, jak je popsáno v [Service Fabric prostředků zásad správného řízení](service-fabric-resource-governance.md).</span><span class="sxs-lookup"><span data-stu-id="5171c-141">Set up hello cluster for enforcing resource limits, as described in [Service Fabric resource governance](service-fabric-resource-governance.md).</span></span> <span data-ttu-id="5171c-142">Všechny ostatní Docker Compose direktivy nejsou podporovány pro tuto verzi preview.</span><span class="sxs-lookup"><span data-stu-id="5171c-142">All other Docker Compose directives are unsupported for this preview.</span></span>

## <a name="servicednsname-computation"></a><span data-ttu-id="5171c-143">Výpočet ServiceDnsName</span><span class="sxs-lookup"><span data-stu-id="5171c-143">ServiceDnsName computation</span></span>

<span data-ttu-id="5171c-144">Pokud je název služby hello, který určíte v souboru vytvářené platný plně kvalifikovaný název domény (to znamená, obsahuje tečku [.]), je název DNS hello registrovaných Service Fabric `<ServiceName>` (včetně hello tečku).</span><span class="sxs-lookup"><span data-stu-id="5171c-144">If hello service name that you specify in a Compose file is a fully qualified domain name (that is, it contains a dot [.]), hello DNS name registered by Service Fabric is `<ServiceName>` (including hello dot).</span></span> <span data-ttu-id="5171c-145">Pokud ne, každý segment cesty v názvu aplikace hello se změní na název domény v hello název DNS služby, se první segment cesty hello stal popisek hello doména nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="5171c-145">If not, each path segment in hello application name becomes a domain label in hello service DNS name, with hello first path segment becoming hello top-level domain label.</span></span>

<span data-ttu-id="5171c-146">Například pokud hello zadaný název aplikace je `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` by hello registrovaný název DNS.</span><span class="sxs-lookup"><span data-stu-id="5171c-146">For example, if hello specified application name is `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` would be hello registered DNS name.</span></span>

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a><span data-ttu-id="5171c-147">Rozdíly mezi vytvářené (instance definice) a modelu aplikace Service Fabric (definice typu)</span><span class="sxs-lookup"><span data-stu-id="5171c-147">Differences between Compose (instance definition) and Service Fabric application model (type definition)</span></span>

<span data-ttu-id="5171c-148">Soubor docker-compose.yml popisuje sadu nasadit kontejnery, včetně jejich vlastnosti a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5171c-148">A docker-compose.yml file describes a deployable set of containers, including their properties and configurations.</span></span>
<span data-ttu-id="5171c-149">Například hello soubor může obsahovat proměnné prostředí a porty.</span><span class="sxs-lookup"><span data-stu-id="5171c-149">For example, hello file can contain environment variables and ports.</span></span> <span data-ttu-id="5171c-150">Parametry nasazení, jako je například umístění omezení, omezení prostředků a názvy DNS, můžete zadat také v soubor docker-compose.yml hello.</span><span class="sxs-lookup"><span data-stu-id="5171c-150">You can also specify deployment parameters, such as placement constraints, resource limits, and DNS names, in hello docker-compose.yml file.</span></span>

<span data-ttu-id="5171c-151">Hello [model aplikace Service Fabric](service-fabric-application-model.md) používá služba typy a typy aplikací, kde může mít velký počet instancí aplikace hello stejného typu.</span><span class="sxs-lookup"><span data-stu-id="5171c-151">hello [Service Fabric application model](service-fabric-application-model.md) uses service types and application types, where you can have many application instances of hello same type.</span></span> <span data-ttu-id="5171c-152">Například můžete mít jedna instance aplikace na zákazníka.</span><span class="sxs-lookup"><span data-stu-id="5171c-152">For example, you can have one application instance per customer.</span></span> <span data-ttu-id="5171c-153">Tento model na základě typu podporuje více verzí hello stejného typu aplikace, která je zaregistrovaná u hello runtime.</span><span class="sxs-lookup"><span data-stu-id="5171c-153">This type-based model supports multiple versions of hello same application type that's registered with hello runtime.</span></span>

<span data-ttu-id="5171c-154">Například zákazník A může mít aplikaci vytvořit instanci typu 1.0 AppTypeA a zákazník B může mít jiné aplikace vytvořena s hello stejného typu a verzi.</span><span class="sxs-lookup"><span data-stu-id="5171c-154">For example, customer A can have an application instantiated with type 1.0 of AppTypeA, and customer B can have another application instantiated with hello same type and version.</span></span> <span data-ttu-id="5171c-155">Definování typů aplikací hello v hello manifestů aplikace a zadat parametry pro název a nasazení aplikace hello při vytvoření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5171c-155">You define hello application types in hello application manifests, and you specify hello application name and deployment parameters when you create hello application.</span></span>

<span data-ttu-id="5171c-156">I když tento model nabízí flexibilitu, jsme také plánování toosupport model jednodušší, založený na instancích nasazení, kde typy jsou implicitní ze souboru manifestu hello.</span><span class="sxs-lookup"><span data-stu-id="5171c-156">Although this model offers flexibility, we are also planning toosupport a simpler, instance-based deployment model where types are implicit from hello manifest file.</span></span> <span data-ttu-id="5171c-157">V tomto modelu každá aplikace získá svůj vlastní nezávislé manifestu.</span><span class="sxs-lookup"><span data-stu-id="5171c-157">In this model, each application gets its own independent manifest.</span></span> <span data-ttu-id="5171c-158">Přidáním podpory pro docker-compose.yml, který je ve formátu nasazení na základě instance jsme náhledu tohoto úsilí.</span><span class="sxs-lookup"><span data-stu-id="5171c-158">We are previewing this effort by adding support for docker-compose.yml, which is an instance-based deployment format.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5171c-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5171c-159">Next steps</span></span>

* <span data-ttu-id="5171c-160">Přečíst na hello [model aplikace Service Fabric](service-fabric-application-model.md)</span><span class="sxs-lookup"><span data-stu-id="5171c-160">Read up on hello [Service Fabric application model](service-fabric-application-model.md)</span></span>
* [<span data-ttu-id="5171c-161">Začínáme s rozhraním příkazového řádku Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5171c-161">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)
