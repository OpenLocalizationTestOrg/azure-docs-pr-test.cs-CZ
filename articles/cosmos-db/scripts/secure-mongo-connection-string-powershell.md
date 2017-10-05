---
title: "Azure PowerShell Get skriptu Azure Cosmos DB připojovací řetězec pro MongoDB aplikace | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Get Azure Cosmos DB připojovací řetězec pro MongoDB aplikace"
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
ms.openlocfilehash: e44e35cc7d11db48cd82e470ce8226b3c8cc220a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-powershell"></a><span data-ttu-id="9d790-103">Získat připojovací řetězec databázi Azure Cosmos pro MongoDB aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9d790-103">Get an Azure Cosmos DB connection string for MongoDB apps using PowerShell</span></span>

<span data-ttu-id="9d790-104">Tato ukázka získá připojovací řetězec databázi Cosmos Azure pro MongoDB aplikací pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9d790-104">This sample gets an Azure Cosmos DB connection string for MongoDB apps using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="9d790-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="9d790-105">Sample script</span></span>

<span data-ttu-id="9d790-106">[!code-powershell[hlavní](../../../powershell_scripts/cosmosdb/get-mongodb-connection-string/get-mongodb-connection-string.ps1?highlight=37-41 "získat připojovací řetězec MongoDB z účtu Azure Cosmos DB")]</span><span class="sxs-lookup"><span data-stu-id="9d790-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/get-mongodb-connection-string/get-mongodb-connection-string.ps1?highlight=37-41 "Get the MongoDB connection string from an Azure Cosmos DB account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="9d790-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="9d790-107">Clean up deployment</span></span>

<span data-ttu-id="9d790-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="9d790-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="9d790-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9d790-109">Script explanation</span></span>

<span data-ttu-id="9d790-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="9d790-110">This script uses the following commands.</span></span> <span data-ttu-id="9d790-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="9d790-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9d790-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9d790-112">Command</span></span> | <span data-ttu-id="9d790-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9d790-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9d790-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9d790-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="9d790-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9d790-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9d790-116">Nové AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="9d790-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="9d790-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="9d790-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="9d790-118">Vyvolání AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="9d790-118">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="9d790-119">Vyvolá akce na účet Azure CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="9d790-119">Invokes an action on the Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="9d790-120">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9d790-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="9d790-121">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="9d790-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="9d790-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d790-122">Next steps</span></span>

<span data-ttu-id="9d790-123">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="9d790-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="9d790-124">Další ukázky skriptu Azure Cosmos DB PowerShell lze nalézt v [skriptů prostředí PowerShell DB Cosmos Azure](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9d790-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>