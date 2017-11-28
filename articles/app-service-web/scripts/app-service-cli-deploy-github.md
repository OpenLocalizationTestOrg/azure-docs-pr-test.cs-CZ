---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit webovou aplikaci s nasazením z Githubu | Microsoft Docs"
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
ms.openlocfilehash: eb7231aa5c6a7e23d76885107e733008382f7487
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-deployment-from-github"></a><span data-ttu-id="60bb8-103">Vytvoření webové aplikace s nasazením z Githubu</span><span class="sxs-lookup"><span data-stu-id="60bb8-103">Create a web app with deployment from GitHub</span></span>

<span data-ttu-id="60bb8-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace z veřejného úložiště GitHub (bez průběžné nasazování).</span><span class="sxs-lookup"><span data-stu-id="60bb8-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="60bb8-105">Nasazení Githubu se průběžné nasazování, najdete v tématu [vytvořit webovou aplikaci s průběžné nasazování z Githubu](app-service-cli-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="60bb8-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-cli-continuous-deployment-github.md).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="60bb8-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="60bb8-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="60bb8-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="60bb8-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="60bb8-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="60bb8-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="60bb8-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="60bb8-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Create a web app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="60bb8-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="60bb8-110">Script explanation</span></span> 

<span data-ttu-id="60bb8-111">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="60bb8-111">This script uses hello following commands.</span></span> <span data-ttu-id="60bb8-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="60bb8-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="60bb8-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="60bb8-113">Command</span></span> | <span data-ttu-id="60bb8-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="60bb8-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="60bb8-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="60bb8-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="60bb8-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="60bb8-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="60bb8-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="60bb8-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="60bb8-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="60bb8-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="60bb8-119">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="60bb8-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="60bb8-120">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="60bb8-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="60bb8-121">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="60bb8-121">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="60bb8-122">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="60bb8-122">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="60bb8-123">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="60bb8-123">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="60bb8-124">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="60bb8-124">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="60bb8-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60bb8-125">Next steps</span></span>

<span data-ttu-id="60bb8-126">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60bb8-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="60bb8-127">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="60bb8-127">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
