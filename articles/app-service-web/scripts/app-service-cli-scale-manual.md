---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - škálování webovou aplikaci ručně pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - škálování webovou aplikaci ručně pomocí Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 64464c8a44522fdc2c8f3d0192388302a1d12667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="2ca59-103">Ruční škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2ca59-103">Scale a web app manually</span></span>

<span data-ttu-id="2ca59-104">V tomto scénáři se dozvíte toocreate skupinu zdrojů, aplikace, služby, plán a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ca59-104">In this scenario you will learn toocreate a resource group, app service plan and web app.</span></span> <span data-ttu-id="2ca59-105">Pak bude škálovat hello plán služby App Service z instancí toomultiple jediné instance.</span><span class="sxs-lookup"><span data-stu-id="2ca59-105">You will then scale hello App Service Plan from a single instance toomultiple instances.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2ca59-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2ca59-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="2ca59-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="2ca59-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="2ca59-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2ca59-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="2ca59-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2ca59-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="2ca59-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="2ca59-110">Script explanation</span></span>

<span data-ttu-id="2ca59-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="2ca59-111">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="2ca59-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="2ca59-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="2ca59-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2ca59-113">Command</span></span> | <span data-ttu-id="2ca59-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="2ca59-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2ca59-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="2ca59-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2ca59-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="2ca59-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2ca59-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="2ca59-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="2ca59-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2ca59-118">Creates an App Service plan.</span></span> <span data-ttu-id="2ca59-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ca59-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="2ca59-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="2ca59-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="2ca59-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="2ca59-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="2ca59-122">Aktualizace plánu služby App Service az</span><span class="sxs-lookup"><span data-stu-id="2ca59-122">az appservice plan update</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#update) | <span data-ttu-id="2ca59-123">Aktualizace vlastnosti hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2ca59-123">Updates properties of hello App Service plan.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2ca59-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ca59-124">Next steps</span></span>

<span data-ttu-id="2ca59-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2ca59-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2ca59-126">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2ca59-126">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
