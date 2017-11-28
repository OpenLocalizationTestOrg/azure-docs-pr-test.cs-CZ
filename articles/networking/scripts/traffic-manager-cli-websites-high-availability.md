---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
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
ms.openlocfilehash: 2142c8bbec1dffc2f12b5500df142a429393a145
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="b3e02-103">Směrovat provoz pro vysokou dostupnost aplikací</span><span class="sxs-lookup"><span data-stu-id="b3e02-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="b3e02-104">Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="b3e02-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="b3e02-105">Traffic Manager směrovat provoz toohello aplikaci v jedné oblasti jako primární oblasti hello a sekundární oblasti toohello když aplikace hello v primární oblasti hello není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b3e02-105">Traffic Manager directs traffic toohello application in one region as hello primary region, and toohello secondary region when hello application in hello primary region is unavailable.</span></span> <span data-ttu-id="b3e02-106">Před provedením hello skriptu, je nutné změnit hello MyWebApp, MyWebAppL1 a MyWebAppL2 hodnoty toounique hodnoty v Azure.</span><span class="sxs-lookup"><span data-stu-id="b3e02-106">Before executing hello script, you must change hello MyWebApp, MyWebAppL1 and MyWebAppL2 values toounique values across Azure.</span></span> <span data-ttu-id="b3e02-107">Po spuštění skriptu hello, budete mít přístup aplikace hello v hello primární oblasti s mywebapp.trafficmanager.net hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b3e02-107">After running hello script, you can access hello app in hello primary region with hello URL mywebapp.trafficmanager.net.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b3e02-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b3e02-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a><span data-ttu-id="b3e02-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="b3e02-109">Clean up deployment</span></span> 

<span data-ttu-id="b3e02-110">Po spuštění ukázka skriptu hello hello postupujte podle příkaz může být použité tooremove hello prostředků skupina, aplikační služby a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="b3e02-110">After hello script sample has been run, hello follow command can be used tooremove hello resource group, App Service app, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a><span data-ttu-id="b3e02-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b3e02-111">Script explanation</span></span>

<span data-ttu-id="b3e02-112">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, profil správce provozu a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="b3e02-112">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="b3e02-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="b3e02-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b3e02-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b3e02-114">Command</span></span> | <span data-ttu-id="b3e02-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b3e02-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b3e02-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="b3e02-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b3e02-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="b3e02-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b3e02-118">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="b3e02-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="b3e02-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b3e02-119">Creates an App Service plan.</span></span> <span data-ttu-id="b3e02-120">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3e02-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="b3e02-121">vytvoření služby App Service web az</span><span class="sxs-lookup"><span data-stu-id="b3e02-121">az appservice web create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#create) | <span data-ttu-id="b3e02-122">Vytvoří webové aplikace Azure v rámci hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b3e02-122">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="b3e02-123">Vytvoření profilu Správce provozu sítě az</span><span class="sxs-lookup"><span data-stu-id="b3e02-123">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="b3e02-124">Vytvoří profilu Azure Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="b3e02-124">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="b3e02-125">vytvořit koncový bod správce provozu sítě az</span><span class="sxs-lookup"><span data-stu-id="b3e02-125">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="b3e02-126">Přidá tooan koncový bod profilu služby Traffic Manager Azure.</span><span class="sxs-lookup"><span data-stu-id="b3e02-126">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b3e02-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3e02-127">Next steps</span></span>

<span data-ttu-id="b3e02-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b3e02-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b3e02-129">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [sítí Azure dokumentaci](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b3e02-129">Additional App Service CLI script samples can be found in hello [Azure Networking documentation](../cli-samples.md).</span></span>
