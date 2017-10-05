---
title: "Prostředí PowerShell příklad obnovení zálohování Azure SQL database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript k obnovení databáze Azure SQL z geograficky redundantní zálohy"
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
ms.openlocfilehash: ae1d0c828ae1e7e1e7e07dcc7d6157187a3859d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-restore-an-azure-sql-database-from-backups"></a><span data-ttu-id="fd192-103">Obnovení ze zálohy Azure SQL database pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fd192-103">Use PowerShell to restore an Azure SQL database from backups</span></span>

<span data-ttu-id="fd192-104">Tento ukázkový skript prostředí PowerShell obnoví databázi Azure SQL z geograficky redundantní zálohy, obnovení odstraněné databáze Azure SQL v jeho nejnovější zálohu a obnoví Azure SQL database k určitému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="fd192-104">This PowerShell script example restores an Azure SQL database from a geo-redundant backup, restores a deleted Azure SQL database to its latest backup, and restores an Azure SQL database to a specific point in time.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="fd192-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="fd192-105">Sample script</span></span>

<span data-ttu-id="fd192-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "vytvoření databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="fd192-106">[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="fd192-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="fd192-107">Clean up deployment</span></span>

<span data-ttu-id="fd192-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="fd192-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="fd192-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="fd192-109">Script explanation</span></span>

<span data-ttu-id="fd192-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="fd192-110">This script uses the following commands.</span></span> <span data-ttu-id="fd192-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="fd192-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="fd192-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="fd192-112">Command</span></span> | <span data-ttu-id="fd192-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="fd192-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fd192-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fd192-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="fd192-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="fd192-115">Creates a resource group in which all resources are stored.</span></span> | [<span data-ttu-id="fd192-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="fd192-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="fd192-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="fd192-117">Creates a logical server that hosts a database or elastic pool.</span></span> | 
| [<span data-ttu-id="fd192-118">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="fd192-118">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="fd192-119">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="fd192-119">Creates a database in a logical server as a single or a pooled database.</span></span> |
[<span data-ttu-id="fd192-120">Get-AzureRmSqlDatabaseGeoBackup</span><span class="sxs-lookup"><span data-stu-id="fd192-120">Get-AzureRmSqlDatabaseGeoBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | <span data-ttu-id="fd192-121">Získá geograficky redundantní zálohy databáze.</span><span class="sxs-lookup"><span data-stu-id="fd192-121">Gets a geo-redundant backup of a database.</span></span> |
| [<span data-ttu-id="fd192-122">Obnovení AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="fd192-122">Restore-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | <span data-ttu-id="fd192-123">Obnoví databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="fd192-123">Restores a SQL database.</span></span> |
|[<span data-ttu-id="fd192-124">Remove-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="fd192-124">Remove-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | <span data-ttu-id="fd192-125">Odebere Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="fd192-125">Removes an Azure SQL database.</span></span> |
| [<span data-ttu-id="fd192-126">Get-AzureRmSqlDeletedDatabaseBackup</span><span class="sxs-lookup"><span data-stu-id="fd192-126">Get-AzureRmSqlDeletedDatabaseBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | <span data-ttu-id="fd192-127">Získá odstraněné databáze, kterou můžete obnovit.</span><span class="sxs-lookup"><span data-stu-id="fd192-127">Gets a deleted database that you can restore.</span></span> |
| [<span data-ttu-id="fd192-128">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fd192-128">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="fd192-129">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="fd192-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fd192-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd192-130">Next steps</span></span>

<span data-ttu-id="fd192-131">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fd192-131">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="fd192-132">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fd192-132">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
