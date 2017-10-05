---
title: "Prostředí PowerShell příklad. kopírování Azure SQL databáze – nový server | Microsoft Docs"
description: "Azure PowerShell ukázkový skript pro kopírování na nový server databáze SQL"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 005ea2e782f8e1cff29f743d9584eb2af2c77509
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-copy-a-sql-database-to-a-new-server"></a><span data-ttu-id="f656f-103">Kopírovat databázi SQL na nový server pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f656f-103">Use PowerShell to copy a SQL database to a new server</span></span>

<span data-ttu-id="f656f-104">Tento ukázkový skript prostředí PowerShell vytvoří kopii existující databázi v nový server.</span><span class="sxs-lookup"><span data-stu-id="f656f-104">This PowerShell script example creates a copy of an existing database in a new server.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-to-a-new-server"></a><span data-ttu-id="f656f-105">Kopírovat databázi na nový server</span><span class="sxs-lookup"><span data-stu-id="f656f-105">Copy a database to a new server</span></span>

<span data-ttu-id="f656f-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "kopie databáze na nový server")]</span><span class="sxs-lookup"><span data-stu-id="f656f-106">[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database to new server")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f656f-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f656f-107">Clean up deployment</span></span>

<span data-ttu-id="f656f-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="f656f-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="f656f-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f656f-109">Script explanation</span></span>

<span data-ttu-id="f656f-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f656f-110">This script uses the following commands.</span></span> <span data-ttu-id="f656f-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f656f-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f656f-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f656f-112">Command</span></span> | <span data-ttu-id="f656f-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f656f-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f656f-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f656f-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="f656f-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f656f-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f656f-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="f656f-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="f656f-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="f656f-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="f656f-118">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="f656f-118">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="f656f-119">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="f656f-119">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="f656f-120">Nové AzureRmSqlDatabaseCopy</span><span class="sxs-lookup"><span data-stu-id="f656f-120">New-AzureRmSqlDatabaseCopy</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | <span data-ttu-id="f656f-121">Vytvoří kopii databáze, která používá snímku v aktuálním čase.</span><span class="sxs-lookup"><span data-stu-id="f656f-121">Creates a copy of a database that uses the snapshot at the current time.</span></span> |
| [<span data-ttu-id="f656f-122">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f656f-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="f656f-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="f656f-123">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="f656f-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f656f-124">Next steps</span></span>

<span data-ttu-id="f656f-125">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f656f-125">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f656f-126">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f656f-126">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
