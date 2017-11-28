---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření funkce aplikace bez serveru provedení | Microsoft Docs"
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
ms.openlocfilehash: 7ec872b642e50827896a73a9e43bcc87233e15c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a><span data-ttu-id="874d8-103">Vytvoření funkce aplikace bez serveru provedení</span><span class="sxs-lookup"><span data-stu-id="874d8-103">Create a Function App for serverless execution</span></span>

<span data-ttu-id="874d8-104">Tento ukázkový skript vytvoří aplikace funkce Azure, což je kontejner pro funkcí.</span><span class="sxs-lookup"><span data-stu-id="874d8-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="874d8-105">Hello funkce aplikace je vytvořený pomocí hello [plánu spotřeby](../functions-scale.md#consumption-plan), což je ideální pro úlohy, bez serveru založeného na událostech.</span><span class="sxs-lookup"><span data-stu-id="874d8-105">hello Function App is created using hello [consumption plan](../functions-scale.md#consumption-plan), which is ideal for event-driven serverless workloads.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="874d8-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="874d8-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="874d8-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="874d8-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="874d8-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="874d8-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="874d8-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="874d8-109">Sample script</span></span>

<span data-ttu-id="874d8-110">Tento skript vytvoří aplikace funkce Azure pomocí hello [plánu spotřeby](../functions-scale.md#consumption-plan).</span><span class="sxs-lookup"><span data-stu-id="874d8-110">This script creates an Azure Function app using hello [consumption plan](../functions-scale.md#consumption-plan).</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="874d8-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="874d8-111">Script explanation</span></span>

<span data-ttu-id="874d8-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="874d8-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="874d8-113">Tento skript používá hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="874d8-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="874d8-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="874d8-114">Command</span></span> | <span data-ttu-id="874d8-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="874d8-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="874d8-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="874d8-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="874d8-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="874d8-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="874d8-118">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="874d8-118">az storage account create</span></span>](/cli/azure/storage/account#create) | <span data-ttu-id="874d8-119">Vytvoří účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="874d8-119">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="874d8-120">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="874d8-120">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="874d8-121">Vytvoří Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="874d8-121">Creates an Azure Function.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="874d8-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="874d8-122">Next steps</span></span>

<span data-ttu-id="874d8-123">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="874d8-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="874d8-124">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="874d8-124">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
