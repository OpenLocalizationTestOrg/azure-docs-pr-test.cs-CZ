---
title: "aaaPowerShell příklad aktivní ve fondu geografická replikace Azure SQL database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu tooset až aktivní geografickou replikací pro ve fondu Azure SQL database"
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/25/2017
ms.author: carlrab
ms.openlocfilehash: 9d183f08dcc07ba864e42fe70a562fef8bd572f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-active-geo-replication-for-a-pooled-azure-sql-database"></a><span data-ttu-id="25d14-103">Pomocí prostředí PowerShell tooconfigure active geografická replikace pro databázi Azure SQL ve fondu</span><span class="sxs-lookup"><span data-stu-id="25d14-103">Use PowerShell tooconfigure active geo-replication for a pooled Azure SQL database</span></span>

<span data-ttu-id="25d14-104">Tento ukázkový skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro databázi Azure SQL v elastickém fondu a převezme toohello sekundární replika databáze Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="25d14-104">This PowerShell script example configures active geo-replication for an Azure SQL database in an elastic pool and fails it over toohello secondary replica of hello Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="25d14-105">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="25d14-105">Sample scripts</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-pool/setup-geodr-and-failover-pool.ps1?highlight=16-19 "Set up active geo-replication for elastic pool")]

## <a name="clean-up-deployment"></a><span data-ttu-id="25d14-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="25d14-106">Clean up deployment</span></span>

<span data-ttu-id="25d14-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="25d14-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="25d14-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="25d14-108">Script explanation</span></span>

<span data-ttu-id="25d14-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="25d14-109">This script uses hello following commands.</span></span> <span data-ttu-id="25d14-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="25d14-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="25d14-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="25d14-111">Command</span></span> | <span data-ttu-id="25d14-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="25d14-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="25d14-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="25d14-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="25d14-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="25d14-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="25d14-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="25d14-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="25d14-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="25d14-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="25d14-117">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="25d14-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="25d14-118">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="25d14-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="25d14-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="25d14-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="25d14-120">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="25d14-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="25d14-121">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="25d14-121">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="25d14-122">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="25d14-122">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="25d14-123">Nové AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="25d14-123">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="25d14-124">Sekundární databáze pro databázi vytvoří a spustí replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="25d14-124">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="25d14-125">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="25d14-125">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="25d14-126">Získá jednu nebo více databází.</span><span class="sxs-lookup"><span data-stu-id="25d14-126">Gets one or more databases.</span></span> |
| [<span data-ttu-id="25d14-127">Set-AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="25d14-127">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="25d14-128">Přepne toobe primární sekundární databáze v pořadí tooinitiate převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="25d14-128">Switches a secondary database toobe primary in order tooinitiate failover.</span></span>|
| [<span data-ttu-id="25d14-129">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="25d14-129">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="25d14-130">Získá hello geografická replikace propojení mezi Azure SQL Database a skupinu prostředků nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="25d14-130">Gets hello geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="25d14-131">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="25d14-131">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="25d14-132">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="25d14-132">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="25d14-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25d14-133">Next steps</span></span>

<span data-ttu-id="25d14-134">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="25d14-134">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="25d14-135">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="25d14-135">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>