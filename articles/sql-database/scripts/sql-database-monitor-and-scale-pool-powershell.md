---
title: "Prostředí PowerShell příklad monitorování škálování SQL elastického fondu Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript ke sledování a škálování elastický fond SQL v databázi SQL Azure"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 7b6a5bd43aadbed33882cf0736ec70436ffdb47b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="994eb-103">Ke sledování a škálování elastický fond SQL v Azure SQL Database pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="994eb-103">Use PowerShell to monitor and scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="994eb-104">Tento ukázkový skript prostředí PowerShell monitoruje metriky výkonu fondu elastické databáze, škálovatelná pro vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="994eb-104">This PowerShell script example monitors the performance metrics of an elastic pool, scales it to a higher performance level, and creates an alert rule on one of the performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="994eb-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="994eb-105">Sample script</span></span>

<span data-ttu-id="994eb-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "sledování a škálování jedna databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="994eb-106">[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="994eb-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="994eb-107">Clean up deployment</span></span>

<span data-ttu-id="994eb-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="994eb-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="994eb-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="994eb-109">Script explanation</span></span>

<span data-ttu-id="994eb-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="994eb-110">This script uses the following commands.</span></span> <span data-ttu-id="994eb-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="994eb-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="994eb-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="994eb-112">Command</span></span> | <span data-ttu-id="994eb-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="994eb-113">Notes</span></span> |
|---|---|
 [<span data-ttu-id="994eb-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="994eb-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="994eb-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="994eb-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="994eb-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="994eb-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="994eb-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="994eb-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="994eb-118">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="994eb-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="994eb-119">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="994eb-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="994eb-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="994eb-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="994eb-121">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="994eb-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="994eb-122">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="994eb-122">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="994eb-123">Zobrazuje informace o využití velikost pro databázi.</span><span class="sxs-lookup"><span data-stu-id="994eb-123">Shows the size usage information for the database.</span></span>|
| [<span data-ttu-id="994eb-124">Přidat AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="994eb-124">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="994eb-125">Přidá nebo aktualizuje pravidlo výstrahy založené na metriky.</span><span class="sxs-lookup"><span data-stu-id="994eb-125">Adds or updates a metric-based alert rule.</span></span> |
| [<span data-ttu-id="994eb-126">Set-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="994eb-126">Set-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | <span data-ttu-id="994eb-127">Aktualizace vlastnosti elastického fondu</span><span class="sxs-lookup"><span data-stu-id="994eb-127">Updates elastic pool properties</span></span> |
| [<span data-ttu-id="994eb-128">Přidat AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="994eb-128">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="994eb-129">Nastaví pravidlo výstrahy automaticky monitorování Dtu v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="994eb-129">Sets an alert rule to automatically monitor DTUs in the future.</span></span> |
| [<span data-ttu-id="994eb-130">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="994eb-130">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="994eb-131">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="994eb-131">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="994eb-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="994eb-132">Next steps</span></span>

<span data-ttu-id="994eb-133">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="994eb-133">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="994eb-134">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="994eb-134">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
