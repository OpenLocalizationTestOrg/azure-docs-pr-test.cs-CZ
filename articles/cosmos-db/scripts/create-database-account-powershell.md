---
title: "aaaAzure prostředí PowerShell vytvořit skript účet rozhraní API služby Azure Cosmos databáze DocumentDB | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření účtu Azure Cosmos databáze DocumentDB rozhraní API"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: f0751faeca3c1de5906b675e736c58f3d5beaea9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-documentdb-api-account-using-powershell"></a><span data-ttu-id="dfee0-103">Azure Cosmos DB: Vytvoření účtu DocumentDB rozhraní API pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="dfee0-103">Azure Cosmos DB: Create a DocumentDB API account using PowerShell</span></span>

<span data-ttu-id="dfee0-104">Tento ukázkový skript prostředí PowerShell vytvoří účet rozhraní API služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dfee0-104">This sample PowerShell script creates an Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="dfee0-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="dfee0-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="dfee0-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="dfee0-106">Clean up deployment</span></span>

<span data-ttu-id="dfee0-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="dfee0-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="dfee0-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="dfee0-108">Script explanation</span></span>

<span data-ttu-id="dfee0-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="dfee0-109">This script uses hello following commands.</span></span> <span data-ttu-id="dfee0-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="dfee0-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="dfee0-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="dfee0-111">Command</span></span> | <span data-ttu-id="dfee0-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="dfee0-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dfee0-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dfee0-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="dfee0-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="dfee0-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="dfee0-115">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="dfee0-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="dfee0-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="dfee0-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="dfee0-117">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dfee0-117">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="dfee0-118">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="dfee0-118">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="dfee0-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfee0-119">Next steps</span></span>

<span data-ttu-id="dfee0-120">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="dfee0-120">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="dfee0-121">Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v hello [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dfee0-121">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
