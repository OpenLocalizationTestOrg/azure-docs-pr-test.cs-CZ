---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit webovou aplikaci s průběžné nasazování z Visual Studio Team Services | Microsoft Docs"
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
ms.openlocfilehash: f8d0c2645ec5311296ca9b2df20f97e77bab2283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a><span data-ttu-id="c678f-103">Vytvoření webové aplikace s průběžné nasazování z Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="c678f-103">Create a web app with continuous deployment from Visual Studio Team Services</span></span>

<span data-ttu-id="c678f-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="c678f-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a Visual Studio Team Services repository.</span></span> <span data-ttu-id="c678f-105">Pro tuto ukázku budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c678f-105">For this sample, you will need:</span></span>

* <span data-ttu-id="c678f-106">Visual Studio Team Services úložiště s kódem aplikace, které mají oprávnění pro správu.</span><span class="sxs-lookup"><span data-stu-id="c678f-106">A Visual Studio Team Services repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="c678f-107">A [Personal Access Token (Jan)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) pro váš účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="c678f-107">A [Personal Access Token (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) for your Visual Studio Team Services account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c678f-108">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c678f-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c678f-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="c678f-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c678f-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c678f-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c678f-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c678f-111">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c678f-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c678f-112">Script explanation</span></span>

<span data-ttu-id="c678f-113">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="c678f-113">This script uses hello following commands.</span></span> <span data-ttu-id="c678f-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c678f-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c678f-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c678f-115">Command</span></span> | <span data-ttu-id="c678f-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c678f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c678f-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="c678f-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c678f-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="c678f-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c678f-119">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="c678f-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c678f-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c678f-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="c678f-121">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="c678f-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="c678f-122">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c678f-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="c678f-123">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="c678f-123">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="c678f-124">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c678f-124">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="c678f-125">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="c678f-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="c678f-126">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c678f-126">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c678f-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c678f-127">Next steps</span></span>

<span data-ttu-id="c678f-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c678f-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c678f-129">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c678f-129">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
