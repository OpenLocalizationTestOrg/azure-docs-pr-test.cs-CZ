---
title: "Prostředí PowerShell příklad aktivní geografické replikace jedním Azure SQL Database | Microsoft Docs"
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
ms.openlocfilehash: b25bc02acda7905cd4c08bbafee1d9b29907d332
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-single-azure-sql-database"></a><span data-ttu-id="cefd6-103">Pomocí prostředí PowerShell ke konfiguraci aktivní geografickou replikací pro jednu databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="cefd6-103">Use PowerShell to configure active geo-replication for a single Azure SQL database</span></span>

<span data-ttu-id="cefd6-104">Tento ukázkový skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro jednu databázi Azure SQL a selhání na sekundární repliku databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="cefd6-104">This PowerShell script example configures active geo-replication for a single Azure SQL database and fails it over to a secondary replica of the Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="cefd6-105">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="cefd6-105">Sample scripts</span></span>

<span data-ttu-id="cefd6-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database.ps1?highlight=17-20 "nastavit aktivní geografickou replikací pro izolované databáze")]</span><span class="sxs-lookup"><span data-stu-id="cefd6-106">[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database.ps1?highlight=17-20 "Set up active geo-replication for single database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="cefd6-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="cefd6-107">Clean up deployment</span></span>

<span data-ttu-id="cefd6-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="cefd6-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="cefd6-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="cefd6-109">Script explanation</span></span>

<span data-ttu-id="cefd6-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="cefd6-110">This script uses the following commands.</span></span> <span data-ttu-id="cefd6-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="cefd6-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cefd6-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="cefd6-112">Command</span></span> | <span data-ttu-id="cefd6-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cefd6-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cefd6-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cefd6-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="cefd6-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="cefd6-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cefd6-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="cefd6-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="cefd6-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="cefd6-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="cefd6-118">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="cefd6-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="cefd6-119">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="cefd6-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="cefd6-120">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="cefd6-120">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="cefd6-121">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="cefd6-121">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="cefd6-122">Nové AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="cefd6-122">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="cefd6-123">Sekundární databáze pro databázi vytvoří a spustí replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="cefd6-123">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="cefd6-124">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="cefd6-124">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="cefd6-125">Získá jednu nebo více databází.</span><span class="sxs-lookup"><span data-stu-id="cefd6-125">Gets one or more databases.</span></span> |
| [<span data-ttu-id="cefd6-126">Set-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="cefd6-126">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="cefd6-127">Přepne sekundární databáze, která bude primární k zahájení převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="cefd6-127">Switches a secondary database to be primary to initiate failover.</span></span>|
| [<span data-ttu-id="cefd6-128">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="cefd6-128">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="cefd6-129">Získá odkazy geografická replikace mezi Azure SQL Database a skupinu prostředků nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cefd6-129">Gets the geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="cefd6-130">Odebrat AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="cefd6-130">Remove-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | <span data-ttu-id="cefd6-131">Ukončí replikaci dat mezi SQL Database a zadaný sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="cefd6-131">Terminates data replication between a SQL Database and the specified secondary database.</span></span> |
| [<span data-ttu-id="cefd6-132">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cefd6-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="cefd6-133">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="cefd6-133">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="cefd6-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cefd6-134">Next steps</span></span>

<span data-ttu-id="cefd6-135">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cefd6-135">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="cefd6-136">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cefd6-136">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
