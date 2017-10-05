---
title: Azure PowerShell. skript-Multiregion replikace pro Azure Cosmos DB | Microsoft Docs
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
ms.openlocfilehash: 3a469ba43e6c601f5eb0e13d588cd0bd4a0f8683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a><span data-ttu-id="29001-103">Replikovat účet Azure Cosmos DB databáze v několika oblastech a konfigurace priorit převzetí služeb při selhání pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="29001-103">Replicate an Azure Cosmos DB database account in multiple regions and configure failover priorities using PowerShell</span></span>

<span data-ttu-id="29001-104">Tato ukázka replikuje jakýkoli druh účtu Azure Cosmos DB databáze v několika oblastech a nakonfiguruje priorit převzetí služeb při selhání pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="29001-104">This sample replicates any kind of Azure Cosmos DB database account in multiple regions and configures failover priorities using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="29001-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="29001-105">Sample script</span></span>

<span data-ttu-id="29001-106">[!code-powershell[hlavní](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "replikovat účet Azure Cosmos DB nad několika oblastmi")]</span><span class="sxs-lookup"><span data-stu-id="29001-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "Replicate an Azure Cosmos DB account across multiple regions")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="29001-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="29001-107">Clean up deployment</span></span>

<span data-ttu-id="29001-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="29001-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="29001-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="29001-109">Script explanation</span></span>

<span data-ttu-id="29001-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="29001-110">This script uses the following commands.</span></span> <span data-ttu-id="29001-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="29001-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="29001-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="29001-112">Command</span></span> | <span data-ttu-id="29001-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="29001-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="29001-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="29001-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="29001-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="29001-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="29001-116">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="29001-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="29001-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="29001-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="29001-118">Set-AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="29001-118">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="29001-119">Upravuje databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="29001-119">Modifies the database account.</span></span> |
| [<span data-ttu-id="29001-120">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="29001-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="29001-121">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="29001-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="29001-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29001-122">Next steps</span></span>

<span data-ttu-id="29001-123">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="29001-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="29001-124">Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="29001-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>