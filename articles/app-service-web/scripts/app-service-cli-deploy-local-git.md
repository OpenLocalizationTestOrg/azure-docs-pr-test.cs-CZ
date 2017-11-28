---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření webové aplikace a nasazení kódu z místního úložiště Git | Microsoft Docs"
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
ms.openlocfilehash: 5ad75394c40025d8941282eabeaf34c19c72ee1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="ece41-103">Vytvoření webové aplikace a nasazení kódu z místního úložiště Git</span><span class="sxs-lookup"><span data-stu-id="ece41-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="ece41-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace v místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="ece41-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ece41-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ece41-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ece41-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="ece41-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ece41-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ece41-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ece41-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ece41-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create a web app and deploy code from a local Git repository")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ece41-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ece41-109">Script explanation</span></span>

<span data-ttu-id="ece41-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ece41-110">This script uses hello following commands.</span></span> <span data-ttu-id="ece41-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ece41-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ece41-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ece41-112">Command</span></span> | <span data-ttu-id="ece41-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ece41-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ece41-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ece41-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ece41-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ece41-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ece41-116">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="ece41-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ece41-117">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ece41-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="ece41-118">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="ece41-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ece41-119">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ece41-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ece41-120">není nastavený az webapp nasazení uživatele</span><span class="sxs-lookup"><span data-stu-id="ece41-120">az webapp deployment user set</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/user#set) | <span data-ttu-id="ece41-121">Nastaví přihlašovací údaje hello nasazení úrovni účtu pro službu App Service.</span><span class="sxs-lookup"><span data-stu-id="ece41-121">Sets hello account-level deployment credentials for App Service.</span></span> |
| [<span data-ttu-id="ece41-122">AZ webapp nasazení zdroj konfigurace místní git</span><span class="sxs-lookup"><span data-stu-id="ece41-122">az webapp deployment source config-local-git</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | <span data-ttu-id="ece41-123">Vytvoří konfiguraci ovládacího prvku zdroje pro místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="ece41-123">Creates a source control configuration for a local Git repository.</span></span> |
| [<span data-ttu-id="ece41-124">Procházet az webapp</span><span class="sxs-lookup"><span data-stu-id="ece41-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="ece41-125">Webové aplikace Azure, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ece41-125">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ece41-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ece41-126">Next steps</span></span>

<span data-ttu-id="ece41-127">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ece41-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ece41-128">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ece41-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
