---
title: "Prostředí PowerShell souboru bacpac příklad import souboru Azure SQL database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript pro import souboru bacpac dlaždice do databáze SQL"
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
ms.openlocfilehash: 20e1d4c195314d6e828e1dac6afae8be11805b42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-import-a-bacpac-file-into-an-azure-sql-database"></a><span data-ttu-id="31147-103">Importovat soubor souboru bacpac do Azure SQL database pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="31147-103">Use PowerShell to import a bacpac file into an Azure SQL database</span></span>

<span data-ttu-id="31147-104">Tento ukázkový skript prostředí PowerShell importuje databázi z **souboru bacpac** souboru do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="31147-104">This PowerShell script example imports a database from a **bacpac** file into an Azure SQL database.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="31147-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="31147-105">Sample script</span></span>

<span data-ttu-id="31147-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "vytvoření databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="31147-106">[!code-powershell[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="31147-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="31147-107">Clean up deployment</span></span>

<span data-ttu-id="31147-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="31147-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="31147-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="31147-109">Script explanation</span></span>

<span data-ttu-id="31147-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="31147-110">This script uses the following commands.</span></span> <span data-ttu-id="31147-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="31147-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="31147-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="31147-112">Command</span></span> | <span data-ttu-id="31147-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="31147-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="31147-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="31147-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="31147-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="31147-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="31147-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="31147-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="31147-117">Vytvoří logického serveru, který je hostitelem databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="31147-117">Creates a logical server that hosts the SQL Database.</span></span> |
| [<span data-ttu-id="31147-118">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="31147-118">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="31147-119">Vytvoří pravidlo brány firewall pro povolení přístupu pro všechny SQL databáze na serveru ze zadané rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="31147-119">Creates a firewall rule to allow access to all SQL Databases on the server from the entered IP address range.</span></span> |
| [<span data-ttu-id="31147-120">Nové AzureRmSqlDatabaseImport</span><span class="sxs-lookup"><span data-stu-id="31147-120">New-AzureRmSqlDatabaseImport</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | <span data-ttu-id="31147-121">Import a .bacpac souboru a vytvoření nové databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="31147-121">Imports a .bacpac file and create a new database on the server.</span></span> |
| [<span data-ttu-id="31147-122">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="31147-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="31147-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="31147-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="31147-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31147-124">Next steps</span></span>

<span data-ttu-id="31147-125">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="31147-125">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="31147-126">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="31147-126">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
