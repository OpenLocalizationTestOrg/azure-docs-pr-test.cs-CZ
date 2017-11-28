---
title: "aaaAzure skript prostředí PowerShell-Multiregion replikace pro Azure Cosmos DB | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Multiregion replikace pro Azure Cosmos DB"
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
ms.openlocfilehash: 8e3e79f086f46fc037d52589eb8bcf4557214566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a><span data-ttu-id="abb6a-103">Replikovat účet Azure Cosmos DB databáze v několika oblastech a konfigurace priorit převzetí služeb při selhání pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="abb6a-103">Replicate an Azure Cosmos DB database account in multiple regions and configure failover priorities using PowerShell</span></span>

<span data-ttu-id="abb6a-104">Tato ukázka replikuje jakýkoli druh účtu Azure Cosmos DB databáze v několika oblastech a nakonfiguruje priorit převzetí služeb při selhání pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="abb6a-104">This sample replicates any kind of Azure Cosmos DB database account in multiple regions and configures failover priorities using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="abb6a-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="abb6a-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "Replicate an Azure Cosmos DB account across multiple regions")]

## <a name="clean-up-deployment"></a><span data-ttu-id="abb6a-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="abb6a-106">Clean up deployment</span></span>

<span data-ttu-id="abb6a-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="abb6a-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="abb6a-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="abb6a-108">Script explanation</span></span>

<span data-ttu-id="abb6a-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="abb6a-109">This script uses hello following commands.</span></span> <span data-ttu-id="abb6a-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="abb6a-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="abb6a-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="abb6a-111">Command</span></span> | <span data-ttu-id="abb6a-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="abb6a-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="abb6a-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="abb6a-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="abb6a-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="abb6a-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="abb6a-115">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="abb6a-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="abb6a-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="abb6a-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="abb6a-117">Set-AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="abb6a-117">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="abb6a-118">Upravuje hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="abb6a-118">Modifies hello database account.</span></span> |
| [<span data-ttu-id="abb6a-119">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="abb6a-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="abb6a-120">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="abb6a-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="abb6a-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abb6a-121">Next steps</span></span>

<span data-ttu-id="abb6a-122">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="abb6a-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="abb6a-123">Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v hello [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="abb6a-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
