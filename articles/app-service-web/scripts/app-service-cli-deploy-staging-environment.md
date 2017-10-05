---
title: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu do pracovního prostředí | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu do pracovního prostředí"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d586b50258c32e44f55859aad0a89475e9e4d2eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a><span data-ttu-id="aeaaf-103">Vytvoření webové aplikace a nasazení kódu do pracovního prostředí</span><span class="sxs-lookup"><span data-stu-id="aeaaf-103">Create a web app and deploy code to a staging environment</span></span>

<span data-ttu-id="aeaaf-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkovou aplikaci na "pracovní" slot.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app to the "staging" slot.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="aeaaf-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="aeaaf-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="aeaaf-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="aeaaf-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="aeaaf-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="aeaaf-108">Sample script</span></span>

<span data-ttu-id="aeaaf-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "vytvoření webové aplikace a nasazení kódu do pracovního prostředí")]</span><span class="sxs-lookup"><span data-stu-id="aeaaf-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code to a staging environment")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="aeaaf-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="aeaaf-110">Script explanation</span></span>

<span data-ttu-id="aeaaf-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-111">This script uses the following commands.</span></span> <span data-ttu-id="aeaaf-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="aeaaf-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="aeaaf-113">Command</span></span> | <span data-ttu-id="aeaaf-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="aeaaf-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="aeaaf-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="aeaaf-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="aeaaf-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="aeaaf-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="aeaaf-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="aeaaf-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="aeaaf-119">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="aeaaf-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="aeaaf-120">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="aeaaf-121">Vytvoření az webapp nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-121">az webapp deployment slot create</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | <span data-ttu-id="aeaaf-122">Vytvořte nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-122">Create a deployment slot.</span></span> |
| [<span data-ttu-id="aeaaf-123">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="aeaaf-123">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="aeaaf-124">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-124">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="aeaaf-125">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="aeaaf-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="aeaaf-126">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-126">Open an Azure web app in a browser.</span></span> |
| [<span data-ttu-id="aeaaf-127">Prohození slotů nasazení az webapp</span><span class="sxs-lookup"><span data-stu-id="aeaaf-127">az webapp deployment slot swap</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | <span data-ttu-id="aeaaf-128">Prohodit slot zadané nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="aeaaf-128">Swap a specified deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="aeaaf-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aeaaf-129">Next steps</span></span>

<span data-ttu-id="aeaaf-130">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="aeaaf-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="aeaaf-131">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="aeaaf-131">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
