---
title: "aaaPowerShell souboru bacpac příklad import souboru Azure SQL database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu tooimport souboru bacpac dlaždice do databáze SQL"
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
ms.openlocfilehash: b85fca1c7fd52037d74254980469f9f53906448e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooimport-a-bacpac-file-into-an-azure-sql-database"></a><span data-ttu-id="55aae-103">Pomocí prostředí PowerShell tooimport soubor souboru bacpac do Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="55aae-103">Use PowerShell tooimport a bacpac file into an Azure SQL database</span></span>

<span data-ttu-id="55aae-104">Tento ukázkový skript prostředí PowerShell importuje databázi z **souboru bacpac** souboru do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="55aae-104">This PowerShell script example imports a database from a **bacpac** file into an Azure SQL database.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="55aae-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="55aae-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="55aae-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="55aae-106">Clean up deployment</span></span>

<span data-ttu-id="55aae-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="55aae-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="55aae-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="55aae-108">Script explanation</span></span>

<span data-ttu-id="55aae-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="55aae-109">This script uses hello following commands.</span></span> <span data-ttu-id="55aae-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="55aae-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="55aae-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="55aae-111">Command</span></span> | <span data-ttu-id="55aae-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="55aae-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="55aae-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="55aae-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="55aae-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="55aae-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="55aae-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="55aae-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="55aae-116">Vytvoří logického serveru, hostitelé hello databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="55aae-116">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="55aae-117">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="55aae-117">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="55aae-118">Vytvoří brány firewall pravidla tooallow přístup tooall databází SQL na serveru hello z hello zadaný rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="55aae-118">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="55aae-119">Nové AzureRmSqlDatabaseImport</span><span class="sxs-lookup"><span data-stu-id="55aae-119">New-AzureRmSqlDatabaseImport</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | <span data-ttu-id="55aae-120">Import a .bacpac souboru a vytvoření nové databáze na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="55aae-120">Imports a .bacpac file and create a new database on hello server.</span></span> |
| [<span data-ttu-id="55aae-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="55aae-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="55aae-122">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="55aae-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="55aae-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55aae-123">Next steps</span></span>

<span data-ttu-id="55aae-124">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="55aae-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="55aae-125">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="55aae-125">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
