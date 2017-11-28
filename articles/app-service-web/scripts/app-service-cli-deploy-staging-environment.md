---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí"
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
ms.openlocfilehash: cd07f5eda31041effd7b7334f5ecc04e6c1a0514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a><span data-ttu-id="ecd75-103">Vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="ecd75-103">Create a web app and deploy code tooa staging environment</span></span>

<span data-ttu-id="ecd75-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkové aplikace toohello "pracovní" slot.</span><span class="sxs-lookup"><span data-stu-id="ecd75-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app toohello "staging" slot.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ecd75-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ecd75-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ecd75-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="ecd75-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ecd75-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ecd75-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ecd75-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ecd75-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code tooa staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ecd75-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ecd75-109">Script explanation</span></span>

<span data-ttu-id="ecd75-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ecd75-110">This script uses hello following commands.</span></span> <span data-ttu-id="ecd75-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ecd75-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ecd75-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ecd75-112">Command</span></span> | <span data-ttu-id="ecd75-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ecd75-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ecd75-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ecd75-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ecd75-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ecd75-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ecd75-116">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="ecd75-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ecd75-117">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ecd75-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="ecd75-118">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="ecd75-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ecd75-119">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ecd75-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ecd75-120">Vytvoření az webapp nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="ecd75-120">az webapp deployment slot create</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | <span data-ttu-id="ecd75-121">Vytvořte nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="ecd75-121">Create a deployment slot.</span></span> |
| [<span data-ttu-id="ecd75-122">AZ webapp nasazení zdroj konfigurace</span><span class="sxs-lookup"><span data-stu-id="ecd75-122">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="ecd75-123">Přidruží úložiště Git nebo Mercurial webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ecd75-123">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="ecd75-124">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="ecd75-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="ecd75-125">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ecd75-125">Open an Azure web app in a browser.</span></span> |
| [<span data-ttu-id="ecd75-126">Prohození slotů nasazení az webapp</span><span class="sxs-lookup"><span data-stu-id="ecd75-126">az webapp deployment slot swap</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | <span data-ttu-id="ecd75-127">Prohodit slot zadané nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="ecd75-127">Swap a specified deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ecd75-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecd75-128">Next steps</span></span>

<span data-ttu-id="ecd75-129">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ecd75-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ecd75-130">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ecd75-130">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
