---
title: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu z místního úložiště Git | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu z místního úložiště Git"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 50d69ac48438920ce59808ee79809235d8330b14
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="90563-103">Vytvoření webové aplikace a nasazení kódu z místního úložiště Git</span><span class="sxs-lookup"><span data-stu-id="90563-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="90563-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace v místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="90563-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="90563-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="90563-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="90563-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="90563-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="90563-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="90563-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="90563-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="90563-108">Sample script</span></span>

<span data-ttu-id="90563-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "vytvoření webové aplikace a nasazení kódu z místního úložiště Git")]</span><span class="sxs-lookup"><span data-stu-id="90563-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create a web app and deploy code from a local Git repository")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="90563-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="90563-110">Script explanation</span></span>

<span data-ttu-id="90563-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="90563-111">This script uses the following commands.</span></span> <span data-ttu-id="90563-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="90563-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="90563-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="90563-113">Command</span></span> | <span data-ttu-id="90563-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="90563-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="90563-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="90563-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="90563-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="90563-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="90563-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="90563-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="90563-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="90563-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="90563-119">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="90563-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="90563-120">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="90563-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="90563-121">není nastavený az webapp nasazení uživatele</span><span class="sxs-lookup"><span data-stu-id="90563-121">az webapp deployment user set</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/user#set) | <span data-ttu-id="90563-122">Nastaví pověření účtu úrovni nasazení pro službu App Service.</span><span class="sxs-lookup"><span data-stu-id="90563-122">Sets the account-level deployment credentials for App Service.</span></span> |
| [<span data-ttu-id="90563-123">AZ webapp nasazení zdroj konfigurace místní git</span><span class="sxs-lookup"><span data-stu-id="90563-123">az webapp deployment source config-local-git</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | <span data-ttu-id="90563-124">Vytvoří konfiguraci ovládacího prvku zdroje pro místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="90563-124">Creates a source control configuration for a local Git repository.</span></span> |
| [<span data-ttu-id="90563-125">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="90563-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="90563-126">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="90563-126">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="90563-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90563-127">Next steps</span></span>

<span data-ttu-id="90563-128">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="90563-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="90563-129">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="90563-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
