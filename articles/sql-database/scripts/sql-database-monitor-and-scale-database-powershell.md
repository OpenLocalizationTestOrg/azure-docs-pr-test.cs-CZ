---
title: "Databáze Azure SQL příklad monitorování škálování jedním prostředí PowerShell | Microsoft Docs"
description: "Azure PowerShell ukázkový skript ke sledování a škálování jedné databáze Azure SQL"
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
ms.openlocfilehash: 5d08335f4b1d6c5c6a120cbfb86ab2c62b79bb9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a><span data-ttu-id="bebf4-103">Pomocí prostředí PowerShell ke sledování a škálování jedné databáze SQL</span><span class="sxs-lookup"><span data-stu-id="bebf4-103">Use PowerShell to monitor and scale a single SQL database</span></span>

<span data-ttu-id="bebf4-104">Tento ukázkový skript prostředí PowerShell monitoruje metriky výkonu databáze, škálovatelná pro vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="bebf4-104">This PowerShell script example monitors the performance metrics of a database, scales it to a higher performance level, and creates an alert rule on one of the performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="bebf4-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bebf4-105">Sample script</span></span>

<span data-ttu-id="bebf4-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "sledování a škálování jedna databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="bebf4-106">[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="bebf4-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bebf4-107">Clean up deployment</span></span>

<span data-ttu-id="bebf4-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="bebf4-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="bebf4-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bebf4-109">Script explanation</span></span>

<span data-ttu-id="bebf4-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="bebf4-110">This script uses the following commands.</span></span> <span data-ttu-id="bebf4-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="bebf4-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="bebf4-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bebf4-112">Command</span></span> | <span data-ttu-id="bebf4-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bebf4-113">Notes</span></span> |
|---|---|
 [<span data-ttu-id="bebf4-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bebf4-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="bebf4-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bebf4-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bebf4-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="bebf4-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="bebf4-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="bebf4-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="bebf4-118">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="bebf4-118">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="bebf4-119">Zobrazuje informace o využití velikost pro databázi.</span><span class="sxs-lookup"><span data-stu-id="bebf4-119">Shows the size usage information for the database.</span></span>|
| [<span data-ttu-id="bebf4-120">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="bebf4-120">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="bebf4-121">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="bebf4-121">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="bebf4-122">Přidat AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="bebf4-122">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="bebf4-123">Nastaví pravidlo výstrahy automaticky monitorování Dtu v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="bebf4-123">Sets an alert rule to automatically monitor DTUs in the future.</span></span> |
| [<span data-ttu-id="bebf4-124">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bebf4-124">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="bebf4-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="bebf4-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="bebf4-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bebf4-126">Next steps</span></span>

<span data-ttu-id="bebf4-127">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bebf4-127">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="bebf4-128">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bebf4-128">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
