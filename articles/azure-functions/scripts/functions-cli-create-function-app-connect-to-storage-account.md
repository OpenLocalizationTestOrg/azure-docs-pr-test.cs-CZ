---
title: "aaaCreate funkce Azure, která se připojuje tooan Azure Storage | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje tooan Azure Storage"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: a51a2c17149478eb2d3d0d4034400ed00cd8416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a><span data-ttu-id="edf6d-103">Integrace funkce aplikace do účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="edf6d-103">Integrate Function App into Azure Storage Account</span></span>

<span data-ttu-id="edf6d-104">Tento ukázkový skript vytvoří aplikaci funkce a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="edf6d-104">This sample script creates a Function App and Storage Account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="edf6d-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="edf6d-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="edf6d-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="edf6d-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="edf6d-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="edf6d-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="edf6d-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="edf6d-108">Sample script</span></span>

<span data-ttu-id="edf6d-109">Tato ukázka vytvoří aplikaci funkce Azure a přidá hello úložiště připojovací řetězec tooan nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="edf6d-109">This sample creates an Azure Function app and adds hello storage connection string tooan app setting.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a><span data-ttu-id="edf6d-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="edf6d-110">Clean up deployment</span></span>

<span data-ttu-id="edf6d-111">Po spuštění ukázka skriptu hello hello následující příkaz může být použité tooremove hello prostředků skupina, aplikační služby a všechny související prostředky:</span><span class="sxs-lookup"><span data-stu-id="edf6d-111">After hello script sample has been run, hello following command can be used tooremove hello resource group, App Service app, and all related resources:</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="edf6d-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="edf6d-112">Script explanation</span></span>

<span data-ttu-id="edf6d-113">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="edf6d-113">This script uses hello following commands.</span></span> <span data-ttu-id="edf6d-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="edf6d-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="edf6d-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="edf6d-115">Command</span></span> | <span data-ttu-id="edf6d-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="edf6d-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="edf6d-117">AZ přihlášení</span><span class="sxs-lookup"><span data-stu-id="edf6d-117">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="edf6d-118">TooAzure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="edf6d-118">Login tooAzure.</span></span> |
| [<span data-ttu-id="edf6d-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="edf6d-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="edf6d-120">Vytvořte skupinu prostředků s umístěním</span><span class="sxs-lookup"><span data-stu-id="edf6d-120">Create a resource group with location</span></span> |
| [<span data-ttu-id="edf6d-121">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="edf6d-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="edf6d-122">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="edf6d-122">Create a storage account</span></span> |
| [<span data-ttu-id="edf6d-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="edf6d-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="edf6d-124">Vytvořit novou aplikaci funkce</span><span class="sxs-lookup"><span data-stu-id="edf6d-124">Create a new function app</span></span> |
| [<span data-ttu-id="edf6d-125">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="edf6d-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="edf6d-126">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="edf6d-126">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="edf6d-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edf6d-127">Next steps</span></span>

<span data-ttu-id="edf6d-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="edf6d-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="edf6d-129">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="edf6d-129">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
