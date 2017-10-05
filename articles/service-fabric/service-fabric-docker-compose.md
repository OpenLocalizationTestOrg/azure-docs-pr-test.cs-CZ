---
title: Azure Service Fabric Docker Compose Preview
description: "Azure Service Fabric přijme Docker Compose formátu, aby bylo snazší orchestraci existující kontejnery pomocí Service Fabric. Tato podpora je aktuálně ve verzi preview."
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
ms.openlocfilehash: e05d1a3d6111e3bbc34008226bcd1fdf35935450
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a><span data-ttu-id="8b883-104">Podpora aplikací docker Compose v Azure Service Fabric (Preview)</span><span class="sxs-lookup"><span data-stu-id="8b883-104">Docker Compose application support in Azure Service Fabric (Preview)</span></span>

<span data-ttu-id="8b883-105">Používá docker [docker-compose.yml](https://docs.docker.com/compose) souboru k definování více kontejneru aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b883-105">Docker uses the [docker-compose.yml](https://docs.docker.com/compose) file for defining multi-container applications.</span></span> <span data-ttu-id="8b883-106">Chcete-li snadno pro zákazníky obeznámeni s Docker orchestraci existující aplikace typu kontejner na Azure Service Fabric, jsme zahrnuli preview podporu pro Docker Compose nativně samotné platformy.</span><span class="sxs-lookup"><span data-stu-id="8b883-106">To make it easy for customers familiar with Docker to orchestrate existing container applications on Azure Service Fabric, we have included preview support for Docker Compose natively in the platform.</span></span> <span data-ttu-id="8b883-107">Service Fabric může přijmout verze 3 nebo novější z `docker-compose.yml` soubory.</span><span class="sxs-lookup"><span data-stu-id="8b883-107">Service Fabric can accept version 3 and later of `docker-compose.yml` files.</span></span> 

<span data-ttu-id="8b883-108">Protože tato podpora je ve verzi preview, se podporuje jenom podmnožinu vytvářené direktivy.</span><span class="sxs-lookup"><span data-stu-id="8b883-108">Because this support is in preview, only a subset of Compose directives is supported.</span></span> <span data-ttu-id="8b883-109">Například upgrady aplikací nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="8b883-109">For example, application upgrades are not supported.</span></span> <span data-ttu-id="8b883-110">Můžete však vždy odebrat a nasadit aplikace místo jejich aktualizace.</span><span class="sxs-lookup"><span data-stu-id="8b883-110">However, you can always remove and deploy applications instead of upgrading them.</span></span>

<span data-ttu-id="8b883-111">Pokud chcete použít tuto verzi preview, vytvořte cluster pomocí verzi 5.7 nebo větší modulu runtime Service Fabric prostřednictvím portálu Azure společně s odpovídající SDK.</span><span class="sxs-lookup"><span data-stu-id="8b883-111">To use this preview, create your cluster with version 5.7 or greater of the Service Fabric runtime through the Azure portal along with the corresponding SDK.</span></span> 

> [!NOTE]
> <span data-ttu-id="8b883-112">Tato funkce je ve verzi preview a není podporována v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b883-112">This feature is in preview and is not supported in production.</span></span>

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a><span data-ttu-id="8b883-113">Nasadit soubor Docker Compose v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8b883-113">Deploy a Docker Compose file on Service Fabric</span></span>

<span data-ttu-id="8b883-114">Následující příkaz třeba vytvoří aplikace Service Fabric (s názvem `fabric:/TestContainerApp` v předchozím příkladu), které můžete sledovat a spravovat podobně jako všechny ostatní aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8b883-114">The following commands create a Service Fabric application (named `fabric:/TestContainerApp` in the preceding example), which you can monitor and manage like any other Service Fabric application.</span></span> <span data-ttu-id="8b883-115">Můžete použít název zadané aplikace pro dotazy na stav.</span><span class="sxs-lookup"><span data-stu-id="8b883-115">You can use the specified application name for health queries.</span></span>

### <a name="use-powershell"></a><span data-ttu-id="8b883-116">Použití prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8b883-116">Use PowerShell</span></span>

<span data-ttu-id="8b883-117">Vytvoření aplikace Service Fabric tvoří z soubor docker-compose.yml spuštěním následujícího příkazu v prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8b883-117">Create a Service Fabric Compose application from a docker-compose.yml file by running the following command in PowerShell:</span></span>

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

<span data-ttu-id="8b883-118">`RegistryUserName`a `RegistryPassword` naleznete v kontejneru registru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8b883-118">`RegistryUserName` and `RegistryPassword` refer to the container registry username and password.</span></span> <span data-ttu-id="8b883-119">Po dokončení aplikace, můžete pomocí následujícího příkazu zkontrolovat její stav:</span><span class="sxs-lookup"><span data-stu-id="8b883-119">After you've completed the application, you can check its status by using the following command:</span></span>

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

<span data-ttu-id="8b883-120">Pokud chcete odstranit aplikaci vytvářené pomocí prostředí PowerShell, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b883-120">To delete the Compose application through PowerShell, use the following command:</span></span>

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="8b883-121">Používání Azure Service Fabric rozhraní příkazového řádku (sfctl)</span><span class="sxs-lookup"><span data-stu-id="8b883-121">Use Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="8b883-122">Alternativně můžete tyto příkazy Service Fabric rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8b883-122">Alternatively, you can use the following Service Fabric CLI command:</span></span>

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

<span data-ttu-id="8b883-123">Po vytvoření aplikace, můžete pomocí následujícího příkazu zkontrolovat její stav:</span><span class="sxs-lookup"><span data-stu-id="8b883-123">After you've created the application, you can check its status by using the following command:</span></span>

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

<span data-ttu-id="8b883-124">Pokud chcete odstranit vytvářené aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b883-124">To delete the Compose application, use the following command:</span></span>

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a><span data-ttu-id="8b883-125">Podporované vytvářené direktivy</span><span class="sxs-lookup"><span data-stu-id="8b883-125">Supported Compose directives</span></span>

<span data-ttu-id="8b883-126">Tato verze preview podporuje podmnožinu možnosti konfigurace z formátu verze 3 vytvářené, včetně následujících primitiv:</span><span class="sxs-lookup"><span data-stu-id="8b883-126">This preview supports a subset of the configuration options from the Compose version 3 format, including the following primitives:</span></span>

* <span data-ttu-id="8b883-127">Služby > nasazení > repliky</span><span class="sxs-lookup"><span data-stu-id="8b883-127">Services > Deploy > Replicas</span></span>
* <span data-ttu-id="8b883-128">Služby > nasazení > umístění > omezení</span><span class="sxs-lookup"><span data-stu-id="8b883-128">Services > Deploy > Placement > Constraints</span></span>
* <span data-ttu-id="8b883-129">Služby > nasazení > prostředky > omezení</span><span class="sxs-lookup"><span data-stu-id="8b883-129">Services > Deploy > Resources > Limits</span></span>
    * <span data-ttu-id="8b883-130">-procesoru-sdílené složky</span><span class="sxs-lookup"><span data-stu-id="8b883-130">-cpu-shares</span></span>
    * <span data-ttu-id="8b883-131">-paměti</span><span class="sxs-lookup"><span data-stu-id="8b883-131">-memory</span></span>
    * <span data-ttu-id="8b883-132">-paměti-swap</span><span class="sxs-lookup"><span data-stu-id="8b883-132">-memory-swap</span></span>
* <span data-ttu-id="8b883-133">Služby > příkazy</span><span class="sxs-lookup"><span data-stu-id="8b883-133">Services > Commands</span></span>
* <span data-ttu-id="8b883-134">Služby > prostředí</span><span class="sxs-lookup"><span data-stu-id="8b883-134">Services > Environment</span></span>
* <span data-ttu-id="8b883-135">Služby > porty</span><span class="sxs-lookup"><span data-stu-id="8b883-135">Services > Ports</span></span>
* <span data-ttu-id="8b883-136">Služby > bitové kopie</span><span class="sxs-lookup"><span data-stu-id="8b883-136">Services > Image</span></span>
* <span data-ttu-id="8b883-137">Služby > izolace (jenom pro Windows)</span><span class="sxs-lookup"><span data-stu-id="8b883-137">Services > Isolation (only for Windows)</span></span>
* <span data-ttu-id="8b883-138">Služby > protokolování > ovladačů</span><span class="sxs-lookup"><span data-stu-id="8b883-138">Services > Logging > Driver</span></span>
* <span data-ttu-id="8b883-139">Služby > protokolování > ovladače > Možnosti</span><span class="sxs-lookup"><span data-stu-id="8b883-139">Services > Logging > Driver > Options</span></span>
* <span data-ttu-id="8b883-140">Svazek & nasazení > svazku</span><span class="sxs-lookup"><span data-stu-id="8b883-140">Volume & Deploy > Volume</span></span>

<span data-ttu-id="8b883-141">Nastavení clusteru pro vynucení omezení prostředků, jak je popsáno v [Service Fabric prostředků zásad správného řízení](service-fabric-resource-governance.md).</span><span class="sxs-lookup"><span data-stu-id="8b883-141">Set up the cluster for enforcing resource limits, as described in [Service Fabric resource governance](service-fabric-resource-governance.md).</span></span> <span data-ttu-id="8b883-142">Všechny ostatní Docker Compose direktivy nejsou podporovány pro tuto verzi preview.</span><span class="sxs-lookup"><span data-stu-id="8b883-142">All other Docker Compose directives are unsupported for this preview.</span></span>

## <a name="servicednsname-computation"></a><span data-ttu-id="8b883-143">Výpočet ServiceDnsName</span><span class="sxs-lookup"><span data-stu-id="8b883-143">ServiceDnsName computation</span></span>

<span data-ttu-id="8b883-144">Pokud název služby, který určíte v souboru Compose je platný plně kvalifikovaný název domény (to znamená, obsahuje tečku [.]), je název DNS registrovaných Service Fabric `<ServiceName>` (včetně tečky).</span><span class="sxs-lookup"><span data-stu-id="8b883-144">If the service name that you specify in a Compose file is a fully qualified domain name (that is, it contains a dot [.]), the DNS name registered by Service Fabric is `<ServiceName>` (including the dot).</span></span> <span data-ttu-id="8b883-145">Pokud ne, každý segment cesty v názvu aplikace se změní na název domény v názvu služby DNS s první segment cesty stal popisek domény nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="8b883-145">If not, each path segment in the application name becomes a domain label in the service DNS name, with the first path segment becoming the top-level domain label.</span></span>

<span data-ttu-id="8b883-146">Například, pokud je zadaný název aplikace `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` by být registrovaný název DNS.</span><span class="sxs-lookup"><span data-stu-id="8b883-146">For example, if the specified application name is `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` would be the registered DNS name.</span></span>

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a><span data-ttu-id="8b883-147">Rozdíly mezi vytvářené (instance definice) a modelu aplikace Service Fabric (definice typu)</span><span class="sxs-lookup"><span data-stu-id="8b883-147">Differences between Compose (instance definition) and Service Fabric application model (type definition)</span></span>

<span data-ttu-id="8b883-148">Soubor docker-compose.yml popisuje sadu nasadit kontejnery, včetně jejich vlastnosti a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8b883-148">A docker-compose.yml file describes a deployable set of containers, including their properties and configurations.</span></span>
<span data-ttu-id="8b883-149">Například soubor může obsahovat proměnné prostředí a porty.</span><span class="sxs-lookup"><span data-stu-id="8b883-149">For example, the file can contain environment variables and ports.</span></span> <span data-ttu-id="8b883-150">Parametry nasazení, jako je například umístění omezení, omezení prostředků a názvy DNS, můžete zadat také v soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="8b883-150">You can also specify deployment parameters, such as placement constraints, resource limits, and DNS names, in the docker-compose.yml file.</span></span>

<span data-ttu-id="8b883-151">[Model aplikace Service Fabric](service-fabric-application-model.md) používá služby typy a typy aplikací, kde může mít velký počet instancí aplikace stejného typu.</span><span class="sxs-lookup"><span data-stu-id="8b883-151">The [Service Fabric application model](service-fabric-application-model.md) uses service types and application types, where you can have many application instances of the same type.</span></span> <span data-ttu-id="8b883-152">Například můžete mít jedna instance aplikace na zákazníka.</span><span class="sxs-lookup"><span data-stu-id="8b883-152">For example, you can have one application instance per customer.</span></span> <span data-ttu-id="8b883-153">Tento model na základě typu podporuje více verzí stejného typu aplikace, které je registrované s modulem runtime.</span><span class="sxs-lookup"><span data-stu-id="8b883-153">This type-based model supports multiple versions of the same application type that's registered with the runtime.</span></span>

<span data-ttu-id="8b883-154">Například zákazník A může mít aplikaci vytvořit instanci typu 1.0 AppTypeA a zákazník B může mít jiné aplikace vytvořena instance stejného typu a verzi.</span><span class="sxs-lookup"><span data-stu-id="8b883-154">For example, customer A can have an application instantiated with type 1.0 of AppTypeA, and customer B can have another application instantiated with the same type and version.</span></span> <span data-ttu-id="8b883-155">Definování typů aplikací v manifestů aplikace a můžete zadat parametry název a nasazení aplikace při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b883-155">You define the application types in the application manifests, and you specify the application name and deployment parameters when you create the application.</span></span>

<span data-ttu-id="8b883-156">I když tento model nabízí flexibilitu, jsme také plánování pro podporu nasazení jednodušší, založený na instancích modelu, kde typy jsou implicitní ze souboru manifestu.</span><span class="sxs-lookup"><span data-stu-id="8b883-156">Although this model offers flexibility, we are also planning to support a simpler, instance-based deployment model where types are implicit from the manifest file.</span></span> <span data-ttu-id="8b883-157">V tomto modelu každá aplikace získá svůj vlastní nezávislé manifestu.</span><span class="sxs-lookup"><span data-stu-id="8b883-157">In this model, each application gets its own independent manifest.</span></span> <span data-ttu-id="8b883-158">Přidáním podpory pro docker-compose.yml, který je ve formátu nasazení na základě instance jsme náhledu tohoto úsilí.</span><span class="sxs-lookup"><span data-stu-id="8b883-158">We are previewing this effort by adding support for docker-compose.yml, which is an instance-based deployment format.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b883-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b883-159">Next steps</span></span>

* <span data-ttu-id="8b883-160">Přečíst na [model aplikace Service Fabric](service-fabric-application-model.md)</span><span class="sxs-lookup"><span data-stu-id="8b883-160">Read up on the [Service Fabric application model](service-fabric-application-model.md)</span></span>
* [<span data-ttu-id="8b883-161">Začínáme s rozhraním příkazového řádku Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8b883-161">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)
