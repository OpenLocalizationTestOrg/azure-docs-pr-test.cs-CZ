---
title: "Ukázka skriptu Azure CLI - škálování webovou aplikaci ručně pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - škálování webovou aplikaci ručně pomocí Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: fe05661eb4e2d5c37aebdbfde002b34588db69e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="c8ac2-103">Ruční škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c8ac2-103">Scale a web app manually</span></span>

<span data-ttu-id="c8ac2-104">V tomto scénáři se dozvíte, pro vytvoření skupiny prostředků, aplikace, služby, plán a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-104">In this scenario you will learn to create a resource group, app service plan and web app.</span></span> <span data-ttu-id="c8ac2-105">Pak bude škálovat, plán služby App Service z jediné instance na více instancí.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-105">You will then scale the App Service Plan from a single instance to multiple instances.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c8ac2-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c8ac2-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="c8ac2-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c8ac2-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c8ac2-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c8ac2-109">Sample script</span></span>

<span data-ttu-id="c8ac2-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "ruční škálování")]</span><span class="sxs-lookup"><span data-stu-id="c8ac2-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c8ac2-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c8ac2-111">Script explanation</span></span>

<span data-ttu-id="c8ac2-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-112">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="c8ac2-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c8ac2-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c8ac2-114">Command</span></span> | <span data-ttu-id="c8ac2-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c8ac2-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c8ac2-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="c8ac2-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c8ac2-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c8ac2-118">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="c8ac2-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c8ac2-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-119">Creates an App Service plan.</span></span> <span data-ttu-id="c8ac2-120">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="c8ac2-121">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="c8ac2-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="c8ac2-122">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="c8ac2-123">Aktualizace plánu služby App Service az</span><span class="sxs-lookup"><span data-stu-id="c8ac2-123">az appservice plan update</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#update) | <span data-ttu-id="c8ac2-124">Aktualizace vlastnosti plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c8ac2-124">Updates properties of the App Service plan.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c8ac2-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8ac2-125">Next steps</span></span>

<span data-ttu-id="c8ac2-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c8ac2-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c8ac2-127">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c8ac2-127">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
