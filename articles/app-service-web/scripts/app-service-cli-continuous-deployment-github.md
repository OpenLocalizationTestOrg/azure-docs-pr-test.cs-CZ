---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit webovou aplikaci s průběžné nasazování z Githubu | Microsoft Docs"
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
ms.openlocfilehash: 6adb06a35ceea8ea64723c9887c25c50f046e280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="ff361-103">Vytvoření webové aplikace s průběžné nasazování z Githubu</span><span class="sxs-lookup"><span data-stu-id="ff361-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="ff361-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="ff361-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="ff361-105">GitHub nasazení bez průběžné nasazování, najdete v tématu [vytvoření webové aplikace a nasazení kódu z Githubu](app-service-cli-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="ff361-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-cli-deploy-github.md).</span></span> <span data-ttu-id="ff361-106">V této ukázce budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="ff361-106">In this sample, you will need:</span></span>

* <span data-ttu-id="ff361-107">Úložiště GitHub s kódem aplikace, které mají oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="ff361-107">A GitHub repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="ff361-108">A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff361-108">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ff361-109">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ff361-109">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ff361-110">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="ff361-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ff361-111">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ff361-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ff361-112">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ff361-112">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ff361-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ff361-113">Script explanation</span></span>

<span data-ttu-id="ff361-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ff361-114">This script uses hello following commands.</span></span> <span data-ttu-id="ff361-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ff361-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ff361-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ff361-116">Command</span></span> | <span data-ttu-id="ff361-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ff361-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ff361-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ff361-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ff361-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ff361-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ff361-120">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="ff361-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ff361-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ff361-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="ff361-122">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="ff361-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ff361-123">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ff361-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ff361-124">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="ff361-124">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="ff361-125">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ff361-125">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="ff361-126">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="ff361-126">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="ff361-127">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ff361-127">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ff361-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff361-128">Next steps</span></span>

<span data-ttu-id="ff361-129">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ff361-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ff361-130">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ff361-130">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
