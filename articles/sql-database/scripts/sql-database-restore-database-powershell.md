---
title: "aaaPowerShell příklad obnovení zálohování Azure SQL database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu toorestore Azure SQL database z geograficky redundantní zálohy"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 68becb89e8a8680aa2efc3de8ad947e674c5fc35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toorestore-an-azure-sql-database-from-backups"></a><span data-ttu-id="4fe95-103">Pomocí prostředí PowerShell toorestore ze zálohy databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="4fe95-103">Use PowerShell toorestore an Azure SQL database from backups</span></span>

<span data-ttu-id="4fe95-104">Tento ukázkový skript prostředí PowerShell obnoví databázi Azure SQL z geograficky redundantní zálohy, obnoví odstraněné Azure SQL database tooits nejnovější zálohování a obnoví tooa databáze Azure SQL pro určitý bod v čase.</span><span class="sxs-lookup"><span data-stu-id="4fe95-104">This PowerShell script example restores an Azure SQL database from a geo-redundant backup, restores a deleted Azure SQL database tooits latest backup, and restores an Azure SQL database tooa specific point in time.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="4fe95-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4fe95-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4fe95-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="4fe95-106">Clean up deployment</span></span>

<span data-ttu-id="4fe95-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="4fe95-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="4fe95-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4fe95-108">Script explanation</span></span>

<span data-ttu-id="4fe95-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="4fe95-109">This script uses hello following commands.</span></span> <span data-ttu-id="4fe95-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4fe95-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4fe95-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4fe95-111">Command</span></span> | <span data-ttu-id="4fe95-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4fe95-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4fe95-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4fe95-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="4fe95-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="4fe95-114">Creates a resource group in which all resources are stored.</span></span> | [<span data-ttu-id="4fe95-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="4fe95-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="4fe95-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="4fe95-116">Creates a logical server that hosts a database or elastic pool.</span></span> | 
| [<span data-ttu-id="4fe95-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="4fe95-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="4fe95-118">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="4fe95-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
[<span data-ttu-id="4fe95-119">Get-AzureRmSqlDatabaseGeoBackup</span><span class="sxs-lookup"><span data-stu-id="4fe95-119">Get-AzureRmSqlDatabaseGeoBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | <span data-ttu-id="4fe95-120">Získá geograficky redundantní zálohy databáze.</span><span class="sxs-lookup"><span data-stu-id="4fe95-120">Gets a geo-redundant backup of a database.</span></span> |
| [<span data-ttu-id="4fe95-121">Obnovení AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="4fe95-121">Restore-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | <span data-ttu-id="4fe95-122">Obnoví databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="4fe95-122">Restores a SQL database.</span></span> |
|[<span data-ttu-id="4fe95-123">Remove-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="4fe95-123">Remove-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | <span data-ttu-id="4fe95-124">Odebere Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="4fe95-124">Removes an Azure SQL database.</span></span> |
| [<span data-ttu-id="4fe95-125">Get-AzureRmSqlDeletedDatabaseBackup</span><span class="sxs-lookup"><span data-stu-id="4fe95-125">Get-AzureRmSqlDeletedDatabaseBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | <span data-ttu-id="4fe95-126">Získá odstraněné databáze, kterou můžete obnovit.</span><span class="sxs-lookup"><span data-stu-id="4fe95-126">Gets a deleted database that you can restore.</span></span> |
| [<span data-ttu-id="4fe95-127">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4fe95-127">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="4fe95-128">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="4fe95-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4fe95-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4fe95-129">Next steps</span></span>

<span data-ttu-id="4fe95-130">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4fe95-130">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="4fe95-131">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4fe95-131">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
