---
title: "aaaAzure prostředí PowerShell vytvořit skript brány firewall pro Azure Cosmos DB | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření brány firewall pro Azure Cosmos DB"
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
ms.openlocfilehash: 00dd2dd847c7ed0e35f5555c2b87b90977f137f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-powershell"></a><span data-ttu-id="f3796-103">Azure Cosmos DB: Vytvoření brány firewall pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f3796-103">Azure Cosmos DB: Create a firewall using PowerShell</span></span>

<span data-ttu-id="f3796-104">Tento ukázkový skript prostředí PowerShell vytvoří brány firewall pro jakýkoli druh účtu Azure Cosmos DB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f3796-104">This sample PowerShell script creates a firewall for any kind of Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="f3796-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f3796-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-firewall/create-firewall.ps1?highlight=35-36,39-43 "Create a firewall for Azure Cosmos DB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f3796-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f3796-106">Clean up deployment</span></span>

<span data-ttu-id="f3796-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="f3796-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="f3796-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f3796-108">Script explanation</span></span>

<span data-ttu-id="f3796-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f3796-109">This script uses hello following commands.</span></span> <span data-ttu-id="f3796-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f3796-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f3796-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f3796-111">Command</span></span> | <span data-ttu-id="f3796-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f3796-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f3796-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f3796-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="f3796-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f3796-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f3796-115">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="f3796-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="f3796-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="f3796-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="f3796-117">Set-AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="f3796-117">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="f3796-118">Upravuje hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="f3796-118">Modifies hello database account.</span></span> |
| [<span data-ttu-id="f3796-119">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f3796-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="f3796-120">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="f3796-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="f3796-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3796-121">Next steps</span></span>

<span data-ttu-id="f3796-122">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="f3796-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="f3796-123">Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v hello [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f3796-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
