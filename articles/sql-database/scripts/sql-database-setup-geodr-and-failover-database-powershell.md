---
title: "aaaPowerShell příklad aktivní geo replikace jedním Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu tooset až aktivní geografickou replikací pro jednu databázi Azure SQL"
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
ms.openlocfilehash: 9ad424d313a5aa96e31b50a1a39c71fd346a7ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-active-geo-replication-for-a-single-azure-sql-database"></a><span data-ttu-id="cb64a-103">Pomocí prostředí PowerShell tooconfigure active geografická replikace pro jednu databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="cb64a-103">Use PowerShell tooconfigure active geo-replication for a single Azure SQL database</span></span>

<span data-ttu-id="cb64a-104">Tento ukázkový skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro jednu databázi Azure SQL a převezme tooa sekundární replika databáze Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="cb64a-104">This PowerShell script example configures active geo-replication for a single Azure SQL database and fails it over tooa secondary replica of hello Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="cb64a-105">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="cb64a-105">Sample scripts</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database.ps1?highlight=17-20 "Set up active geo-replication for single database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cb64a-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="cb64a-106">Clean up deployment</span></span>

<span data-ttu-id="cb64a-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="cb64a-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="cb64a-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="cb64a-108">Script explanation</span></span>

<span data-ttu-id="cb64a-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="cb64a-109">This script uses hello following commands.</span></span> <span data-ttu-id="cb64a-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="cb64a-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cb64a-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="cb64a-111">Command</span></span> | <span data-ttu-id="cb64a-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cb64a-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cb64a-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cb64a-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="cb64a-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="cb64a-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cb64a-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="cb64a-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="cb64a-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="cb64a-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="cb64a-117">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="cb64a-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="cb64a-118">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="cb64a-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="cb64a-119">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="cb64a-119">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="cb64a-120">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="cb64a-120">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="cb64a-121">Nové AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="cb64a-121">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="cb64a-122">Sekundární databáze pro databázi vytvoří a spustí replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="cb64a-122">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="cb64a-123">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="cb64a-123">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="cb64a-124">Získá jednu nebo více databází.</span><span class="sxs-lookup"><span data-stu-id="cb64a-124">Gets one or more databases.</span></span> |
| [<span data-ttu-id="cb64a-125">Set-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="cb64a-125">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="cb64a-126">Přepne sekundární databáze toobe primární tooinitiate převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="cb64a-126">Switches a secondary database toobe primary tooinitiate failover.</span></span>|
| [<span data-ttu-id="cb64a-127">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="cb64a-127">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="cb64a-128">Získá hello geografická replikace propojení mezi Azure SQL Database a skupinu prostředků nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cb64a-128">Gets hello geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="cb64a-129">Odebrat AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="cb64a-129">Remove-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | <span data-ttu-id="cb64a-130">Ukončí replikaci dat mezi SQL Database a hello zadaná sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="cb64a-130">Terminates data replication between a SQL Database and hello specified secondary database.</span></span> |
| [<span data-ttu-id="cb64a-131">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cb64a-131">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="cb64a-132">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="cb64a-132">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="cb64a-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb64a-133">Next steps</span></span>

<span data-ttu-id="cb64a-134">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cb64a-134">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="cb64a-135">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cb64a-135">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
