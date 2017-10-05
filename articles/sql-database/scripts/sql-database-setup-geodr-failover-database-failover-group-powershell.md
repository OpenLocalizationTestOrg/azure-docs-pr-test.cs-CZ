---
title: "Prostředí PowerShell příklad geografická replikace převzetí služeb při selhání jedné skupiny Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript nastavit aktivní geografickou replikací pro jednu databázi Azure SQL"
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
ms.openlocfilehash: 8db8540d9c4caeb53ea8f34d45d9498496d2b8ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-configure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a><span data-ttu-id="9f430-103">Konfigurace skupiny aktivní geografickou replikaci převzetí služeb při selhání pro jednu databázi Azure SQL pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f430-103">Use PowerShell to configure an active geo-replication failover group for a single Azure SQL database</span></span>

<span data-ttu-id="9f430-104">Tento ukázkový skript prostředí PowerShell nakonfiguruje skupinu aktivní geografickou replikaci převzetí služeb při selhání pro jednu databázi Azure SQL a selhání na sekundární repliku databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="9f430-104">This PowerShell script example configures an active geo-replication failover group for a single Azure SQL database and fails it over to a secondary replica of the Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="9f430-105">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="9f430-105">Sample scripts</span></span>

<span data-ttu-id="9f430-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "nastavení převzetí služeb při selhání skupiny pro izolované databáze")]</span><span class="sxs-lookup"><span data-stu-id="9f430-106">[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="9f430-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="9f430-107">Clean up deployment</span></span>

<span data-ttu-id="9f430-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="9f430-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="9f430-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="9f430-109">Script explanation</span></span>

<span data-ttu-id="9f430-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="9f430-110">This script uses the following commands.</span></span> <span data-ttu-id="9f430-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="9f430-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9f430-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="9f430-112">Command</span></span> | <span data-ttu-id="9f430-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9f430-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9f430-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9f430-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="9f430-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="9f430-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9f430-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="9f430-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="9f430-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="9f430-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="9f430-118">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="9f430-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="9f430-119">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="9f430-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="9f430-120">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="9f430-120">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="9f430-121">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="9f430-121">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="9f430-122">Nové AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="9f430-122">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="9f430-123">Sekundární databáze pro databázi vytvoří a spustí replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="9f430-123">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="9f430-124">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="9f430-124">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="9f430-125">Získá jednu nebo více databází.</span><span class="sxs-lookup"><span data-stu-id="9f430-125">Gets one or more databases.</span></span> |
| [<span data-ttu-id="9f430-126">Set-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="9f430-126">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="9f430-127">Přepne sekundární databáze, která bude primární k zahájení převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9f430-127">Switches a secondary database to be primary to initiate failover.</span></span>|
| [<span data-ttu-id="9f430-128">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="9f430-128">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="9f430-129">Získá odkazy geografická replikace mezi Azure SQL Database a skupinu prostředků nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f430-129">Gets the geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="9f430-130">Odebrat AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="9f430-130">Remove-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | <span data-ttu-id="9f430-131">Ukončí replikaci dat mezi SQL Database a zadaný sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="9f430-131">Terminates data replication between a SQL Database and the specified secondary database.</span></span> |
| [<span data-ttu-id="9f430-132">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9f430-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="9f430-133">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="9f430-133">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="9f430-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f430-134">Next steps</span></span>

<span data-ttu-id="9f430-135">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9f430-135">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9f430-136">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9f430-136">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
