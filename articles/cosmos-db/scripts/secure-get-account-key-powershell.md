---
title: "aaaAzure skript prostředí PowerShell-Get účet klíče pro cosmosdb | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Get klíče účtu pro cosmosdb"
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
ms.openlocfilehash: 9ccee3085dc4fa6507d43e4a220dd5fc32889a9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-account-keys-for-azure-cosmos-db-using-powershell"></a><span data-ttu-id="2d59f-103">Získání klíče účtu pro Azure DB Cosmos pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2d59f-103">Get account keys for Azure Cosmos DB using PowerShell</span></span>

<span data-ttu-id="2d59f-104">Tato ukázka získá klíče účtu pro jakýkoli druh účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2d59f-104">This sample gets account keys for any kind of Azure Cosmos DB account.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="2d59f-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2d59f-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/get-account-keys/get-account-keys.ps1?highlight=36-40 "Get hello keys for an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="2d59f-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="2d59f-106">Clean up deployment</span></span>

<span data-ttu-id="2d59f-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="2d59f-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="2d59f-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="2d59f-108">Script explanation</span></span>

<span data-ttu-id="2d59f-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="2d59f-109">This script uses hello following commands.</span></span> <span data-ttu-id="2d59f-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="2d59f-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="2d59f-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2d59f-111">Command</span></span> | <span data-ttu-id="2d59f-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="2d59f-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2d59f-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2d59f-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="2d59f-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="2d59f-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2d59f-115">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="2d59f-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="2d59f-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="2d59f-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="2d59f-117">Vyvolání AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="2d59f-117">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="2d59f-118">Vyvolá akce na hello Azure CosmosDB účtu.</span><span class="sxs-lookup"><span data-stu-id="2d59f-118">Invokes an action on hello Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="2d59f-119">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2d59f-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="2d59f-120">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="2d59f-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="2d59f-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d59f-121">Next steps</span></span>

<span data-ttu-id="2d59f-122">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="2d59f-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="2d59f-123">Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v hello [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2d59f-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
