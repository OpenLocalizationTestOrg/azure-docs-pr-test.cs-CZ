---
title: "Kontejner Azure registru úložiště | Microsoft Docs"
description: "Jak používat Azure registru kontejner úložiště pro imagí Dockeru"
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
ms.openlocfilehash: dd4feff057269ed7106990bb63eed7fcffa2dbec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="945d8-103">Kontejner Azure registru úložiště</span><span class="sxs-lookup"><span data-stu-id="945d8-103">Azure container registry repositories</span></span>

<span data-ttu-id="945d8-104">Azure registrech kontejneru jsou kompatibilní s velkého množství služeb a orchestrators.</span><span class="sxs-lookup"><span data-stu-id="945d8-104">Azure Container Registries are compatible with a multitude of services and orchestrators.</span></span> <span data-ttu-id="945d8-105">Aby bylo snazší sledují zdroj služeb a agentů, ze kterých se používá ACR, jsme spustili pomocí pole hlavičky Docker v souboru Docker.config.</span><span class="sxs-lookup"><span data-stu-id="945d8-105">To make it easier to track the source services and agents from which ACR is used, we have started using the Docker header field in the Docker.config file.</span></span>



## <a name="viewing-repositories-in-the-portal"></a><span data-ttu-id="945d8-106">Zobrazení úložiště na portálu</span><span class="sxs-lookup"><span data-stu-id="945d8-106">Viewing repositories in the Portal</span></span>

<span data-ttu-id="945d8-107">Hlavičky ACR použijte formát:</span><span class="sxs-lookup"><span data-stu-id="945d8-107">The ACR headers follow the format:</span></span>
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* <span data-ttu-id="945d8-108">Cloudu: Azure, Azure zásobník, nebo jiných vládních nebo Azure cloudy jednotlivé země.</span><span class="sxs-lookup"><span data-stu-id="945d8-108">Cloud: Azure, Azure Stack, or other government or country-specific Azure clouds.</span></span> <span data-ttu-id="945d8-109">I když Azure zásobníku a government cloudy nejsou aktuálně podporovány, tento parametr umožňuje podpory v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="945d8-109">Although Azure Stack and government clouds are not currently supported, this parameter enables future support.</span></span>
* <span data-ttu-id="945d8-110">Služba: název služby.</span><span class="sxs-lookup"><span data-stu-id="945d8-110">Service: name of the service.</span></span>
* <span data-ttu-id="945d8-111">Optionalservicename: volitelný parametr pro služby s subservices nebo k zadání SKU (např: odpovídají webové aplikace s Azure nebo-aplikace služby App service/web).</span><span class="sxs-lookup"><span data-stu-id="945d8-111">Optionalservicename: optional parameter for services with subservices, or to specify a SKU (ex: web apps correspond with Azure/app-service/web-apps).</span></span>

<span data-ttu-id="945d8-112">Služby partnera a orchestrators doporučujeme použít hodnoty konkrétní hlavičky usnadní naše telemetrie.</span><span class="sxs-lookup"><span data-stu-id="945d8-112">Partner services and orchestrators are encouraged to use specific header values to help with our telemetry.</span></span> <span data-ttu-id="945d8-113">Uživatele můžete také upravit hodnota předaná hlavičku svého rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="945d8-113">Users can also modify the value passed to the header if they so desire.</span></span>

<span data-ttu-id="945d8-114">Hodnoty chceme ACR partnery sloužící k vyplnění pole "X-Meta-zdroje-Client" jsou následující:</span><span class="sxs-lookup"><span data-stu-id="945d8-114">The values we want ACR partners to use to populate the "X-Meta-Source-Client" field are below:</span></span>

| <span data-ttu-id="945d8-115">Název služby</span><span class="sxs-lookup"><span data-stu-id="945d8-115">Service Name</span></span>              | <span data-ttu-id="945d8-116">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="945d8-116">Header</span></span>                                |
| ------------------------- | ------------------------------------- |
| <span data-ttu-id="945d8-117">Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="945d8-117">Azure Container Service</span></span>   | <span data-ttu-id="945d8-118">Azure nebo výpočetní/azure kontejneru service</span><span class="sxs-lookup"><span data-stu-id="945d8-118">azure/compute/azure-container-service</span></span> |
| <span data-ttu-id="945d8-119">Aplikační služby - webové aplikace</span><span class="sxs-lookup"><span data-stu-id="945d8-119">App Service - Web Apps</span></span>    | <span data-ttu-id="945d8-120">Azure nebo-aplikace služby App service/web</span><span class="sxs-lookup"><span data-stu-id="945d8-120">azure/app-service/web-apps</span></span>            |
| <span data-ttu-id="945d8-121">Aplikační služby - Logic Apps</span><span class="sxs-lookup"><span data-stu-id="945d8-121">App Service - Logic Apps</span></span>  | <span data-ttu-id="945d8-122">Azure nebo-aplikace služby App service nebo logiku</span><span class="sxs-lookup"><span data-stu-id="945d8-122">azure/app-service/logic-apps</span></span>          |
| <span data-ttu-id="945d8-123">Batch</span><span class="sxs-lookup"><span data-stu-id="945d8-123">Batch</span></span>                     | <span data-ttu-id="945d8-124">Azure nebo výpočetní/batch</span><span class="sxs-lookup"><span data-stu-id="945d8-124">azure/compute/batch</span></span>                   |
| <span data-ttu-id="945d8-125">Cloud Console</span><span class="sxs-lookup"><span data-stu-id="945d8-125">Cloud Console</span></span>             | <span data-ttu-id="945d8-126">Azure nebo cloudu konzoly</span><span class="sxs-lookup"><span data-stu-id="945d8-126">azure/cloud-console</span></span>                   |
| <span data-ttu-id="945d8-127">Funkce</span><span class="sxs-lookup"><span data-stu-id="945d8-127">Functions</span></span>                 | <span data-ttu-id="945d8-128">Azure nebo výpočetní nebo funkce</span><span class="sxs-lookup"><span data-stu-id="945d8-128">azure/compute/functions</span></span>               |
| <span data-ttu-id="945d8-129">Internet věcí - Hub</span><span class="sxs-lookup"><span data-stu-id="945d8-129">Internet of Things - Hub</span></span>  | <span data-ttu-id="945d8-130">Azure nebo iot nebo Centrum</span><span class="sxs-lookup"><span data-stu-id="945d8-130">azure/iot/hub</span></span>                         |
| <span data-ttu-id="945d8-131">HDInsight</span><span class="sxs-lookup"><span data-stu-id="945d8-131">HDInsight</span></span>                 | <span data-ttu-id="945d8-132">Azure/data/hdinsight</span><span class="sxs-lookup"><span data-stu-id="945d8-132">azure/data/hdinsight</span></span>                  |
| <span data-ttu-id="945d8-133">Jenkins</span><span class="sxs-lookup"><span data-stu-id="945d8-133">Jenkins</span></span>                   | <span data-ttu-id="945d8-134">Azure nebo volaných</span><span class="sxs-lookup"><span data-stu-id="945d8-134">azure/jenkins</span></span>                         |
| <span data-ttu-id="945d8-135">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="945d8-135">Machine Learning</span></span>          | <span data-ttu-id="945d8-136">Azure/data/machile-learning</span><span class="sxs-lookup"><span data-stu-id="945d8-136">azure/data/machile-learning</span></span>           |
| <span data-ttu-id="945d8-137">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="945d8-137">Service Fabric</span></span>            | <span data-ttu-id="945d8-138">Azure nebo výpočetní/service-prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="945d8-138">azure/compute/service-fabric</span></span>          |
| <span data-ttu-id="945d8-139">SLUŽBY VSTS</span><span class="sxs-lookup"><span data-stu-id="945d8-139">VSTS</span></span>                      | <span data-ttu-id="945d8-140">Azure nebo služby vsts</span><span class="sxs-lookup"><span data-stu-id="945d8-140">azure/vsts</span></span>                            |


## <a name="next-steps"></a><span data-ttu-id="945d8-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="945d8-141">Next steps</span></span>
[<span data-ttu-id="945d8-142">Další informace o registrech a podporované služby a orchestrators</span><span class="sxs-lookup"><span data-stu-id="945d8-142">Learn more about registries and the supported services and orchestrators</span></span>](container-registry-intro.md)
