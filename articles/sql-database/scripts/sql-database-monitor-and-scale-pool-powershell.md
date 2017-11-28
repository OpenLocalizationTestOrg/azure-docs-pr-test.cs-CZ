---
title: "aaaPowerShell příklad monitorování škálování SQL elastického fondu – Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu toomonitor a škálování elastický fond SQL v databázi SQL Azure"
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
ms.openlocfilehash: 149a45174ccb8072ea21753364196c7f98fd4101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="eee94-103">Pomocí prostředí PowerShell toomonitor a škálování elastický fond SQL v databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="eee94-103">Use PowerShell toomonitor and scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="eee94-104">Tento ukázkový skript prostředí PowerShell, že monitorování hello metriky výkonu fondu elastické databáze škáluje ho tooa vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom hello metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="eee94-104">This PowerShell script example monitors hello performance metrics of an elastic pool, scales it tooa higher performance level, and creates an alert rule on one of hello performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="eee94-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="eee94-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="eee94-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="eee94-106">Clean up deployment</span></span>

<span data-ttu-id="eee94-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="eee94-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="eee94-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="eee94-108">Script explanation</span></span>

<span data-ttu-id="eee94-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="eee94-109">This script uses hello following commands.</span></span> <span data-ttu-id="eee94-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="eee94-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="eee94-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="eee94-111">Command</span></span> | <span data-ttu-id="eee94-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="eee94-112">Notes</span></span> |
|---|---|
 [<span data-ttu-id="eee94-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eee94-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="eee94-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="eee94-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="eee94-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="eee94-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="eee94-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="eee94-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="eee94-117">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="eee94-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="eee94-118">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="eee94-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="eee94-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="eee94-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="eee94-120">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="eee94-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="eee94-121">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="eee94-121">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="eee94-122">Zobrazuje informace o databázi hello využití velikost hello.</span><span class="sxs-lookup"><span data-stu-id="eee94-122">Shows hello size usage information for hello database.</span></span>|
| [<span data-ttu-id="eee94-123">Přidat AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="eee94-123">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="eee94-124">Přidá nebo aktualizuje pravidlo výstrahy založené na metriky.</span><span class="sxs-lookup"><span data-stu-id="eee94-124">Adds or updates a metric-based alert rule.</span></span> |
| [<span data-ttu-id="eee94-125">Set-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="eee94-125">Set-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | <span data-ttu-id="eee94-126">Aktualizace vlastnosti elastického fondu</span><span class="sxs-lookup"><span data-stu-id="eee94-126">Updates elastic pool properties</span></span> |
| [<span data-ttu-id="eee94-127">Přidat AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="eee94-127">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="eee94-128">Nastaví monitor tooautomatically pravidlo výstrahy Dtu hello budoucí.</span><span class="sxs-lookup"><span data-stu-id="eee94-128">Sets an alert rule tooautomatically monitor DTUs in hello future.</span></span> |
| [<span data-ttu-id="eee94-129">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eee94-129">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="eee94-130">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="eee94-130">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="eee94-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eee94-131">Next steps</span></span>

<span data-ttu-id="eee94-132">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eee94-132">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="eee94-133">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="eee94-133">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
