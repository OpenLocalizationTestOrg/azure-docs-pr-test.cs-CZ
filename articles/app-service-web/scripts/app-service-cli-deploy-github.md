---
title: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s nasazením z Githubu | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s nasazením z Githubu"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: sample
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 61e9d65319cecf3ea4e9152ebdf1035566aad74c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-with-deployment-from-github"></a><span data-ttu-id="6c5f4-103">Vytvoření webové aplikace s nasazením z Githubu</span><span class="sxs-lookup"><span data-stu-id="6c5f4-103">Create a web app with deployment from GitHub</span></span>

<span data-ttu-id="6c5f4-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace z veřejného úložiště GitHub (bez průběžné nasazování).</span><span class="sxs-lookup"><span data-stu-id="6c5f4-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="6c5f4-105">Nasazení Githubu se průběžné nasazování, najdete v tématu [vytvořit webovou aplikaci s průběžné nasazování z Githubu](app-service-cli-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="6c5f4-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-cli-continuous-deployment-github.md).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6c5f4-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6c5f4-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="6c5f4-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6c5f4-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="6c5f4-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6c5f4-109">Sample script</span></span>

<span data-ttu-id="6c5f4-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "vytvořit webovou aplikaci s nasazením z Githubu")]</span><span class="sxs-lookup"><span data-stu-id="6c5f4-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Create a web app with deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="6c5f4-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6c5f4-111">Script explanation</span></span> 

<span data-ttu-id="6c5f4-112">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-112">This script uses the following commands.</span></span> <span data-ttu-id="6c5f4-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="6c5f4-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6c5f4-114">Command</span></span> | <span data-ttu-id="6c5f4-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6c5f4-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6c5f4-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="6c5f4-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6c5f4-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6c5f4-118">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="6c5f4-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="6c5f4-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="6c5f4-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="6c5f4-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="6c5f4-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="6c5f4-122">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="6c5f4-122">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="6c5f4-123">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-123">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="6c5f4-124">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="6c5f4-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="6c5f4-125">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6c5f4-125">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6c5f4-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c5f4-126">Next steps</span></span>

<span data-ttu-id="6c5f4-127">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6c5f4-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6c5f4-128">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6c5f4-128">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
