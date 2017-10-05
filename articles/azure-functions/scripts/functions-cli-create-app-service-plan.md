---
title: "Azure CLI skriptu ukázkové – vytvoření aplikace pro funkce v rámci plánu služby App Service | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření aplikace pro funkce v rámci plánu služby App Service"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 40c3fa6fa6c07d59e4bf55531e116ba50aa92b91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a><span data-ttu-id="52e6e-103">Vytvoření aplikace pro funkce v rámci plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="52e6e-103">Create a Function App in an App Service plan</span></span>

<span data-ttu-id="52e6e-104">Tento ukázkový skript vytvoří aplikace funkce Azure, což je kontejner pro funkcí.</span><span class="sxs-lookup"><span data-stu-id="52e6e-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="52e6e-105">Funkce aplikace je vytvořený pomocí vyhrazené plán služby App Service, což znamená, že vaše prostředky serveru jsou vždy zapnuty.</span><span class="sxs-lookup"><span data-stu-id="52e6e-105">The Function App is created using a dedicated App Service plan, which means your server resources are always on.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="52e6e-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="52e6e-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="52e6e-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="52e6e-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="52e6e-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="52e6e-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="52e6e-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="52e6e-109">Sample script</span></span>

<span data-ttu-id="52e6e-110">Tento skript vytvoří aplikace funkce Azure pomocí vyhrazená [plán služby App Service](../functions-scale.md#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="52e6e-110">This script creates an Azure Function app using a dedicated [App Service plan](../functions-scale.md#app-service-plan).</span></span>

<span data-ttu-id="52e6e-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "vytvoření funkce Azure na plán služby App Service")]</span><span class="sxs-lookup"><span data-stu-id="52e6e-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="52e6e-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="52e6e-112">Script explanation</span></span>

<span data-ttu-id="52e6e-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="52e6e-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="52e6e-114">Tento skript používá následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="52e6e-114">This script uses the following commands:</span></span>

| <span data-ttu-id="52e6e-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="52e6e-115">Command</span></span> | <span data-ttu-id="52e6e-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="52e6e-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="52e6e-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="52e6e-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="52e6e-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="52e6e-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="52e6e-119">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="52e6e-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="52e6e-120">Vytvoří účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="52e6e-120">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="52e6e-121">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="52e6e-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appserviceplan#create) | <span data-ttu-id="52e6e-122">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="52e6e-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="52e6e-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="52e6e-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#delete) | <span data-ttu-id="52e6e-124">Vytvoří aplikaci Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="52e6e-124">Creates an Azure Function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="52e6e-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52e6e-125">Next steps</span></span>

<span data-ttu-id="52e6e-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="52e6e-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="52e6e-127">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="52e6e-127">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
