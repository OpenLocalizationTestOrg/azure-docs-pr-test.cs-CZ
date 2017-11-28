---
title: "aaaAzure kontejneru registru úložiště | Microsoft Docs"
description: "Jak toouse registru Azure kontejner úložiště pro Docker bitové kopie"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 06172a63465838a78a607f268da116d8158789ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="4124e-103">Kontejner Azure registru úložiště</span><span class="sxs-lookup"><span data-stu-id="4124e-103">Azure container registry repositories</span></span>

<span data-ttu-id="4124e-104">Azure registrech kontejneru jsou kompatibilní s velkého množství služeb a orchestrators.</span><span class="sxs-lookup"><span data-stu-id="4124e-104">Azure Container Registries are compatible with a multitude of services and orchestrators.</span></span> <span data-ttu-id="4124e-105">toomake je snazší tootrack hello zdroj služeb a agentů, ze kterých se používá ACR, jsme spustili pomocí pole hlavičky hello Docker v souboru Docker.config hello.</span><span class="sxs-lookup"><span data-stu-id="4124e-105">toomake it easier tootrack hello source services and agents from which ACR is used, we have started using hello Docker header field in hello Docker.config file.</span></span>



## <a name="viewing-repositories-in-hello-portal"></a><span data-ttu-id="4124e-106">Zobrazení úložiště v hello portálu</span><span class="sxs-lookup"><span data-stu-id="4124e-106">Viewing repositories in hello Portal</span></span>

<span data-ttu-id="4124e-107">Hello ACR záhlaví podle hello formátu:</span><span class="sxs-lookup"><span data-stu-id="4124e-107">hello ACR headers follow hello format:</span></span>
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* <span data-ttu-id="4124e-108">Cloudu: Azure, Azure zásobník, nebo jiných vládních nebo Azure cloudy jednotlivé země.</span><span class="sxs-lookup"><span data-stu-id="4124e-108">Cloud: Azure, Azure Stack, or other government or country-specific Azure clouds.</span></span> <span data-ttu-id="4124e-109">I když Azure zásobníku a government cloudy nejsou aktuálně podporovány, tento parametr umožňuje podpory v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="4124e-109">Although Azure Stack and government clouds are not currently supported, this parameter enables future support.</span></span>
* <span data-ttu-id="4124e-110">Služby: název služby hello.</span><span class="sxs-lookup"><span data-stu-id="4124e-110">Service: name of hello service.</span></span>
* <span data-ttu-id="4124e-111">Optionalservicename: volitelný parametr pro služby s subservices nebo toospecify SKU (např: odpovídají webové aplikace s Azure nebo-aplikace služby App service/web).</span><span class="sxs-lookup"><span data-stu-id="4124e-111">Optionalservicename: optional parameter for services with subservices, or toospecify a SKU (ex: web apps correspond with Azure/app-service/web-apps).</span></span>

<span data-ttu-id="4124e-112">Služby partnera a orchestrators jsou podporovali toouse konkrétní hlavičky hodnoty toohelp s naše telemetrií.</span><span class="sxs-lookup"><span data-stu-id="4124e-112">Partner services and orchestrators are encouraged toouse specific header values toohelp with our telemetry.</span></span> <span data-ttu-id="4124e-113">Uživatele můžete také upravit hello předaná hodnota hlavičky toohello svého rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="4124e-113">Users can also modify hello value passed toohello header if they so desire.</span></span>

<span data-ttu-id="4124e-114">Hello hodnoty chceme ACR partnery toouse toopopulate hello "X-Meta-zdroje-Client" pole jsou níže:</span><span class="sxs-lookup"><span data-stu-id="4124e-114">hello values we want ACR partners toouse toopopulate hello "X-Meta-Source-Client" field are below:</span></span>

| <span data-ttu-id="4124e-115">Název služby</span><span class="sxs-lookup"><span data-stu-id="4124e-115">Service Name</span></span>              | <span data-ttu-id="4124e-116">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="4124e-116">Header</span></span>                                |
| ------------------------- | ------------------------------------- |
| <span data-ttu-id="4124e-117">Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="4124e-117">Azure Container Service</span></span>   | <span data-ttu-id="4124e-118">Azure nebo výpočetní/azure kontejneru service</span><span class="sxs-lookup"><span data-stu-id="4124e-118">azure/compute/azure-container-service</span></span> |
| <span data-ttu-id="4124e-119">Aplikační služby - webové aplikace</span><span class="sxs-lookup"><span data-stu-id="4124e-119">App Service - Web Apps</span></span>    | <span data-ttu-id="4124e-120">Azure nebo-aplikace služby App service/web</span><span class="sxs-lookup"><span data-stu-id="4124e-120">azure/app-service/web-apps</span></span>            |
| <span data-ttu-id="4124e-121">Aplikační služby - Logic Apps</span><span class="sxs-lookup"><span data-stu-id="4124e-121">App Service - Logic Apps</span></span>  | <span data-ttu-id="4124e-122">Azure nebo-aplikace služby App service nebo logiku</span><span class="sxs-lookup"><span data-stu-id="4124e-122">azure/app-service/logic-apps</span></span>          |
| <span data-ttu-id="4124e-123">Batch</span><span class="sxs-lookup"><span data-stu-id="4124e-123">Batch</span></span>                     | <span data-ttu-id="4124e-124">Azure nebo výpočetní/batch</span><span class="sxs-lookup"><span data-stu-id="4124e-124">azure/compute/batch</span></span>                   |
| <span data-ttu-id="4124e-125">Cloud Console</span><span class="sxs-lookup"><span data-stu-id="4124e-125">Cloud Console</span></span>             | <span data-ttu-id="4124e-126">Azure nebo cloudu konzoly</span><span class="sxs-lookup"><span data-stu-id="4124e-126">azure/cloud-console</span></span>                   |
| <span data-ttu-id="4124e-127">Funkce</span><span class="sxs-lookup"><span data-stu-id="4124e-127">Functions</span></span>                 | <span data-ttu-id="4124e-128">Azure nebo výpočetní nebo funkce</span><span class="sxs-lookup"><span data-stu-id="4124e-128">azure/compute/functions</span></span>               |
| <span data-ttu-id="4124e-129">Internet věcí - Hub</span><span class="sxs-lookup"><span data-stu-id="4124e-129">Internet of Things - Hub</span></span>  | <span data-ttu-id="4124e-130">Azure nebo iot nebo Centrum</span><span class="sxs-lookup"><span data-stu-id="4124e-130">azure/iot/hub</span></span>                         |
| <span data-ttu-id="4124e-131">HDInsight</span><span class="sxs-lookup"><span data-stu-id="4124e-131">HDInsight</span></span>                 | <span data-ttu-id="4124e-132">Azure/data/hdinsight</span><span class="sxs-lookup"><span data-stu-id="4124e-132">azure/data/hdinsight</span></span>                  |
| <span data-ttu-id="4124e-133">Jenkins</span><span class="sxs-lookup"><span data-stu-id="4124e-133">Jenkins</span></span>                   | <span data-ttu-id="4124e-134">Azure nebo volaných</span><span class="sxs-lookup"><span data-stu-id="4124e-134">azure/jenkins</span></span>                         |
| <span data-ttu-id="4124e-135">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4124e-135">Machine Learning</span></span>          | <span data-ttu-id="4124e-136">Azure/data/machile-learning</span><span class="sxs-lookup"><span data-stu-id="4124e-136">azure/data/machile-learning</span></span>           |
| <span data-ttu-id="4124e-137">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4124e-137">Service Fabric</span></span>            | <span data-ttu-id="4124e-138">Azure nebo výpočetní/service-prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="4124e-138">azure/compute/service-fabric</span></span>          |
| <span data-ttu-id="4124e-139">SLUŽBY VSTS</span><span class="sxs-lookup"><span data-stu-id="4124e-139">VSTS</span></span>                      | <span data-ttu-id="4124e-140">Azure nebo služby vsts</span><span class="sxs-lookup"><span data-stu-id="4124e-140">azure/vsts</span></span>                            |


## <a name="next-steps"></a><span data-ttu-id="4124e-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4124e-141">Next steps</span></span>
[<span data-ttu-id="4124e-142">Další informace o registrech a hello podporované služby a orchestrators</span><span class="sxs-lookup"><span data-stu-id="4124e-142">Learn more about registries and hello supported services and orchestrators</span></span>](container-registry-intro.md)
