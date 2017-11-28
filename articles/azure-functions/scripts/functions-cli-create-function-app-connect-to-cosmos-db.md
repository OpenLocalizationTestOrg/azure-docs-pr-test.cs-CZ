---
title: "aaaCreate funkce Azure, která se připojuje tooan Azure Cosmos DB | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje tooan Azure Cosmos DB"
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
ms.openlocfilehash: 0fbc1ebec2dfd480e0cf3ca64f9febcec8af9a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-that-connects-tooan-azure-cosmos-db"></a><span data-ttu-id="6313e-103">Vytvoření funkce Azure, která se připojuje tooan Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6313e-103">Create an Azure Function that connects tooan Azure Cosmos DB</span></span>

<span data-ttu-id="6313e-104">Tento ukázkový skript vytvoří funkce aplikace Azure a připojí tooan Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="6313e-104">This sample script creates an Azure Function App and connects tooan Azure Cosmos DB database.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6313e-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6313e-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6313e-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="6313e-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="6313e-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6313e-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="6313e-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6313e-108">Sample script</span></span>

<span data-ttu-id="6313e-109">Tato ukázka vytvoří aplikaci funkce Azure a přidá nastavení pro klíče tooapp koncového bodu a přístup Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6313e-109">This sample creates an Azure Function app and adds a Cosmos DB endpoint and access key tooapp settings.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects tooan Azure Cosmos DB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="6313e-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="6313e-110">Clean up deployment</span></span>

<span data-ttu-id="6313e-111">Po spuštění ukázka skriptu hello hello postupujte podle příkaz lze použít tooremove skupiny prostředků hello a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6313e-111">After hello script sample has been run, hello follow command can be used tooremove hello resource group and all related resources.</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="6313e-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6313e-112">Script explanation</span></span>

<span data-ttu-id="6313e-113">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="6313e-113">This script uses hello following commands.</span></span> <span data-ttu-id="6313e-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="6313e-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6313e-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6313e-115">Command</span></span> | <span data-ttu-id="6313e-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6313e-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6313e-117">AZ přihlášení</span><span class="sxs-lookup"><span data-stu-id="6313e-117">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="6313e-118">TooAzure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6313e-118">Login tooAzure.</span></span> |
| [<span data-ttu-id="6313e-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="6313e-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6313e-120">Vytvořte skupinu prostředků s umístěním</span><span class="sxs-lookup"><span data-stu-id="6313e-120">Create a resource group with location</span></span> |
| [<span data-ttu-id="6313e-121">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="6313e-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="6313e-122">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6313e-122">Create a storage account</span></span> |
| [<span data-ttu-id="6313e-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="6313e-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="6313e-124">Vytvořit novou aplikaci funkce</span><span class="sxs-lookup"><span data-stu-id="6313e-124">Create a new function app</span></span> |
| [<span data-ttu-id="6313e-125">Vytvoření az documentdb</span><span class="sxs-lookup"><span data-stu-id="6313e-125">az documentdb create</span></span>](https://docs.microsoft.com/cli/azure/documentdb#create) | <span data-ttu-id="6313e-126">Vytvoření databáze documentdb</span><span class="sxs-lookup"><span data-stu-id="6313e-126">Create documentdb database</span></span> |
| [<span data-ttu-id="6313e-127">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="6313e-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="6313e-128">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="6313e-128">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6313e-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6313e-129">Next steps</span></span>

<span data-ttu-id="6313e-130">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6313e-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6313e-131">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v hello [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6313e-131">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>




