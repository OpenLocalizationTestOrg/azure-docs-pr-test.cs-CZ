---
title: "Vytvoření funkce Azure, která se připojuje k databázi Azure Cosmos | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje k databázi Cosmos Azure"
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
ms.openlocfilehash: ba7e934f71824493f29b001cea6dd1c567ef3414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a><span data-ttu-id="dbb8b-103">Vytvoření funkce Azure, která se připojuje k databázi Cosmos Azure</span><span class="sxs-lookup"><span data-stu-id="dbb8b-103">Create an Azure Function that connects to an Azure Cosmos DB</span></span>

<span data-ttu-id="dbb8b-104">Tento ukázkový skript vytvoří aplikace funkce Azure, připojí se k databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-104">This sample script creates an Azure Function App and connects to an Azure Cosmos DB database.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="dbb8b-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="dbb8b-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="dbb8b-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="dbb8b-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="dbb8b-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="dbb8b-108">Sample script</span></span>

<span data-ttu-id="dbb8b-109">Tato ukázka vytvoří aplikaci funkce Azure a přidá Cosmos DB koncový bod a přístupový klíč nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-109">This sample creates an Azure Function app and adds a Cosmos DB endpoint and access key to app settings.</span></span>

<span data-ttu-id="dbb8b-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "vytvoření funkce Azure, která se připojuje k databázi Cosmos Azure")]</span><span class="sxs-lookup"><span data-stu-id="dbb8b-110">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="dbb8b-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="dbb8b-111">Clean up deployment</span></span>

<span data-ttu-id="dbb8b-112">Po spuštění ukázka skriptu, použijte příkaz lze použít k odebrání skupiny prostředků a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-112">After the script sample has been run, the follow command can be used to remove the resource group and all related resources.</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="dbb8b-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="dbb8b-113">Script explanation</span></span>

<span data-ttu-id="dbb8b-114">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-114">This script uses the following commands.</span></span> <span data-ttu-id="dbb8b-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="dbb8b-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="dbb8b-116">Command</span></span> | <span data-ttu-id="dbb8b-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="dbb8b-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dbb8b-118">AZ přihlášení</span><span class="sxs-lookup"><span data-stu-id="dbb8b-118">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="dbb8b-119">Přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb8b-119">Login to Azure.</span></span> |
| [<span data-ttu-id="dbb8b-120">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="dbb8b-120">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="dbb8b-121">Vytvořte skupinu prostředků s umístěním</span><span class="sxs-lookup"><span data-stu-id="dbb8b-121">Create a resource group with location</span></span> |
| [<span data-ttu-id="dbb8b-122">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="dbb8b-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="dbb8b-123">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="dbb8b-123">Create a storage account</span></span> |
| [<span data-ttu-id="dbb8b-124">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="dbb8b-124">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="dbb8b-125">Vytvořit novou aplikaci funkce</span><span class="sxs-lookup"><span data-stu-id="dbb8b-125">Create a new function app</span></span> |
| [<span data-ttu-id="dbb8b-126">Vytvoření az documentdb</span><span class="sxs-lookup"><span data-stu-id="dbb8b-126">az documentdb create</span></span>](https://docs.microsoft.com/cli/azure/documentdb#create) | <span data-ttu-id="dbb8b-127">Vytvoření databáze documentdb</span><span class="sxs-lookup"><span data-stu-id="dbb8b-127">Create documentdb database</span></span> |
| [<span data-ttu-id="dbb8b-128">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="dbb8b-128">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="dbb8b-129">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="dbb8b-129">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dbb8b-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dbb8b-130">Next steps</span></span>

<span data-ttu-id="dbb8b-131">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dbb8b-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="dbb8b-132">Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dbb8b-132">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>




