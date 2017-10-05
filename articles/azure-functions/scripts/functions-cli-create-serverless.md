---
title: "Azure CLI skriptu ukázkové – vytvoření funkce aplikace bez serveru provedení | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce aplikace bez serveru provedení"
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
ms.openlocfilehash: 37046967bd5ab0d797d1c66690db7200fb4805e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a><span data-ttu-id="ceb59-103">Vytvoření funkce aplikace bez serveru provedení</span><span class="sxs-lookup"><span data-stu-id="ceb59-103">Create a Function App for serverless execution</span></span>

<span data-ttu-id="ceb59-104">Tento ukázkový skript vytvoří aplikace funkce Azure, což je kontejner pro funkcí.</span><span class="sxs-lookup"><span data-stu-id="ceb59-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="ceb59-105">Funkce aplikace je vytvořený pomocí [plánu spotřeby](../functions-scale.md#consumption-plan), což je ideální pro úlohy, bez serveru založeného na událostech.</span><span class="sxs-lookup"><span data-stu-id="ceb59-105">The Function App is created using the [consumption plan](../functions-scale.md#consumption-plan), which is ideal for event-driven serverless workloads.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ceb59-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ceb59-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ceb59-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ceb59-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="ceb59-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ceb59-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ceb59-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ceb59-109">Sample script</span></span>

<span data-ttu-id="ceb59-110">Tento skript vytvoří aplikace funkce Azure pomocí [plánu spotřeby](../functions-scale.md#consumption-plan).</span><span class="sxs-lookup"><span data-stu-id="ceb59-110">This script creates an Azure Function app using the [consumption plan](../functions-scale.md#consumption-plan).</span></span>

<span data-ttu-id="ceb59-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "vytvoření funkce Azure v plánu spotřeby")]</span><span class="sxs-lookup"><span data-stu-id="ceb59-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ceb59-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ceb59-112">Script explanation</span></span>

<span data-ttu-id="ceb59-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="ceb59-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="ceb59-114">Tento skript používá následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ceb59-114">This script uses the following commands:</span></span>

| <span data-ttu-id="ceb59-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ceb59-115">Command</span></span> | <span data-ttu-id="ceb59-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ceb59-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ceb59-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ceb59-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ceb59-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ceb59-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ceb59-119">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="ceb59-119">az storage account create</span></span>](/cli/azure/storage/account#create) | <span data-ttu-id="ceb59-120">Vytvoří účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb59-120">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="ceb59-121">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="ceb59-121">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="ceb59-122">Vytvoří Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="ceb59-122">Creates an Azure Function.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ceb59-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ceb59-123">Next steps</span></span>

<span data-ttu-id="ceb59-124">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ceb59-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ceb59-125">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ceb59-125">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
