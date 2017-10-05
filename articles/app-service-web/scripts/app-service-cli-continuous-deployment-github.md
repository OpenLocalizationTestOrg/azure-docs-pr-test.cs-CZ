---
title: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s průběžné nasazování z Githubu | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s průběžné nasazování z Githubu"
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
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a12085a7a8146c22d6b079381542d4fe3a8e6e87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="3c9b9-103">Vytvoření webové aplikace s průběžné nasazování z Githubu</span><span class="sxs-lookup"><span data-stu-id="3c9b9-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="3c9b9-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="3c9b9-105">GitHub nasazení bez průběžné nasazování, najdete v tématu [vytvoření webové aplikace a nasazení kódu z Githubu](app-service-cli-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-cli-deploy-github.md).</span></span> <span data-ttu-id="3c9b9-106">V této ukázce budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="3c9b9-106">In this sample, you will need:</span></span>

* <span data-ttu-id="3c9b9-107">Úložiště GitHub s kódem aplikace, které mají oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-107">A GitHub repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="3c9b9-108">A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-108">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3c9b9-109">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-109">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3c9b9-110">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="3c9b9-111">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="3c9b9-112">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3c9b9-112">Sample script</span></span>

<span data-ttu-id="3c9b9-113">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "vytvořit webovou aplikaci s průběžné nasazování z Githubu")]</span><span class="sxs-lookup"><span data-stu-id="3c9b9-113">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="3c9b9-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="3c9b9-114">Script explanation</span></span>

<span data-ttu-id="3c9b9-115">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-115">This script uses the following commands.</span></span> <span data-ttu-id="3c9b9-116">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="3c9b9-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="3c9b9-117">Command</span></span> | <span data-ttu-id="3c9b9-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3c9b9-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3c9b9-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="3c9b9-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3c9b9-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3c9b9-121">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="3c9b9-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="3c9b9-122">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="3c9b9-123">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="3c9b9-123">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="3c9b9-124">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-124">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="3c9b9-125">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="3c9b9-125">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="3c9b9-126">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-126">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="3c9b9-127">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="3c9b9-127">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="3c9b9-128">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-128">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3c9b9-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c9b9-129">Next steps</span></span>

<span data-ttu-id="3c9b9-130">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3c9b9-131">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-131">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
