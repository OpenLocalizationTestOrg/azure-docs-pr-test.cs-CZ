---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - škálování webové aplikace po celém světě s architekturou vysokou availabilty | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - škálování po celém světě s architekturou vysokou availabilty webové aplikace"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b72fbccd7f2aaab58e4b4721e14dca14146c7c72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="2f6e7-103">Škálování webové aplikace po celém světě s vysokou dostupností architektura</span><span class="sxs-lookup"><span data-stu-id="2f6e7-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="2f6e7-104">V tomto scénáři vytvoříte skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="2f6e7-105">Po dokončení hello cvičení budete mít s vysokou dostupností architektura, která umožňuje poskytuje globální dostupnost webové aplikace založené na nejnižší latenci sítě hello.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-105">Once hello exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on hello lowest network latency.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2f6e7-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="2f6e7-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="2f6e7-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2f6e7-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="2f6e7-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2f6e7-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="2f6e7-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="2f6e7-110">Script explanation</span></span>

<span data-ttu-id="2f6e7-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, profil správce provozu a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-111">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="2f6e7-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="2f6e7-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2f6e7-113">Command</span></span> | <span data-ttu-id="2f6e7-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="2f6e7-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2f6e7-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="2f6e7-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2f6e7-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2f6e7-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="2f6e7-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="2f6e7-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-118">Creates an App Service plan.</span></span> <span data-ttu-id="2f6e7-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="2f6e7-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="2f6e7-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="2f6e7-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="2f6e7-122">Vytvoření profilu Správce provozu sítě az</span><span class="sxs-lookup"><span data-stu-id="2f6e7-122">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="2f6e7-123">Vytvoří profilu Azure Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-123">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="2f6e7-124">vytvořit koncový bod správce provozu sítě az</span><span class="sxs-lookup"><span data-stu-id="2f6e7-124">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="2f6e7-125">Přidá tooan koncový bod profilu služby Traffic Manager Azure.</span><span class="sxs-lookup"><span data-stu-id="2f6e7-125">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2f6e7-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f6e7-126">Next steps</span></span>

<span data-ttu-id="2f6e7-127">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2f6e7-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2f6e7-128">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2f6e7-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
