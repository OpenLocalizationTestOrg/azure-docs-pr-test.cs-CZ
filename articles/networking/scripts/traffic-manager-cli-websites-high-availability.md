---
title: "Ukázka skriptu Azure CLI - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - směrovat provoz pro vysokou dostupnost aplikací"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: tysonn
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 0593d063a4935d02aae124d83b62b11e37aa3c33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="f90f5-103">Směrovat provoz pro vysokou dostupnost aplikací</span><span class="sxs-lookup"><span data-stu-id="f90f5-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="f90f5-104">Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="f90f5-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="f90f5-105">Správce provozu přesměruje přenosy na aplikaci v jedné oblasti jako primární oblasti a sekundární oblast, když aplikace v primární oblasti není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f90f5-105">Traffic Manager directs traffic to the application in one region as the primary region, and to the secondary region when the application in the primary region is unavailable.</span></span> <span data-ttu-id="f90f5-106">Před spuštěním skriptu, musíte změnit hodnoty MyWebApp, MyWebAppL1 a MyWebAppL2 jedinečné hodnoty mezi Azure.</span><span class="sxs-lookup"><span data-stu-id="f90f5-106">Before executing the script, you must change the MyWebApp, MyWebAppL1 and MyWebAppL2 values to unique values across Azure.</span></span> <span data-ttu-id="f90f5-107">Po spuštění skriptu, můžete přístup k aplikaci v primární oblasti s mywebapp.trafficmanager.net adresy URL.</span><span class="sxs-lookup"><span data-stu-id="f90f5-107">After running the script, you can access the app in the primary region with the URL mywebapp.trafficmanager.net.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f90f5-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f90f5-108">Sample script</span></span>

<span data-ttu-id="f90f5-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "směrování provozu pro zajištění vysoké dostupnosti")]</span><span class="sxs-lookup"><span data-stu-id="f90f5-109">[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]</span></span>


## <a name="clean-up-deployment"></a><span data-ttu-id="f90f5-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f90f5-110">Clean up deployment</span></span> 

<span data-ttu-id="f90f5-111">Po spuštění ukázka skriptu, použijte příkaz lze použít k odebrání skupiny prostředků, aplikační služby a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f90f5-111">After the script sample has been run, the follow command can be used to remove the resource group, App Service app, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a><span data-ttu-id="f90f5-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f90f5-112">Script explanation</span></span>

<span data-ttu-id="f90f5-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, profil správce provozu a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f90f5-113">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="f90f5-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f90f5-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f90f5-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f90f5-115">Command</span></span> | <span data-ttu-id="f90f5-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f90f5-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f90f5-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="f90f5-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f90f5-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f90f5-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f90f5-119">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="f90f5-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="f90f5-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f90f5-120">Creates an App Service plan.</span></span> <span data-ttu-id="f90f5-121">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f90f5-121">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="f90f5-122">vytvoření služby App Service web az</span><span class="sxs-lookup"><span data-stu-id="f90f5-122">az appservice web create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#create) | <span data-ttu-id="f90f5-123">Vytvoří webové aplikace Azure v rámci plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f90f5-123">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="f90f5-124">Vytvoření profilu Správce provozu sítě az</span><span class="sxs-lookup"><span data-stu-id="f90f5-124">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="f90f5-125">Vytvoří profilu Azure Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="f90f5-125">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="f90f5-126">vytvořit koncový bod správce provozu sítě az</span><span class="sxs-lookup"><span data-stu-id="f90f5-126">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="f90f5-127">Koncový bod se přidá do profilu Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="f90f5-127">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f90f5-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f90f5-128">Next steps</span></span>

<span data-ttu-id="f90f5-129">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f90f5-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f90f5-130">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [sítí Azure dokumentaci](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f90f5-130">Additional App Service CLI script samples can be found in the [Azure Networking documentation](../cli-samples.md).</span></span>
