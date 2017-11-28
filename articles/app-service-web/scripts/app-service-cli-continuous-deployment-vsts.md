---
title: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s průběžné nasazování z Visual Studio Team Services | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s průběžné nasazování z Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 2b983616757ca3c4226c12876f5fd4c285067318
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a><span data-ttu-id="fa211-103">Vytvoření webové aplikace s průběžné nasazování z Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="fa211-103">Create a web app with continuous deployment from Visual Studio Team Services</span></span>

<span data-ttu-id="fa211-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="fa211-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a Visual Studio Team Services repository.</span></span> <span data-ttu-id="fa211-105">Pro tuto ukázku budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="fa211-105">For this sample, you will need:</span></span>

* <span data-ttu-id="fa211-106">Visual Studio Team Services úložiště s kódem aplikace, které mají oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="fa211-106">A Visual Studio Team Services repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="fa211-107">A [Personal Access Token (Jan)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) pro váš účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="fa211-107">A [Personal Access Token (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) for your Visual Studio Team Services account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fa211-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fa211-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="fa211-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="fa211-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="fa211-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fa211-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="fa211-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="fa211-111">Sample script</span></span>

<span data-ttu-id="fa211-112">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "vytvořit webovou aplikaci s průběžné nasazování z Visual Studio Team Services")]</span><span class="sxs-lookup"><span data-stu-id="fa211-112">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="fa211-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="fa211-113">Script explanation</span></span>

<span data-ttu-id="fa211-114">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="fa211-114">This script uses the following commands.</span></span> <span data-ttu-id="fa211-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa211-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="fa211-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="fa211-116">Command</span></span> | <span data-ttu-id="fa211-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="fa211-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fa211-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="fa211-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="fa211-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="fa211-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fa211-120">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="fa211-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="fa211-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="fa211-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="fa211-122">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="fa211-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="fa211-123">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="fa211-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="fa211-124">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="fa211-124">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="fa211-125">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="fa211-125">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="fa211-126">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="fa211-126">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="fa211-127">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="fa211-127">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fa211-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa211-128">Next steps</span></span>

<span data-ttu-id="fa211-129">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fa211-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="fa211-130">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fa211-130">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
