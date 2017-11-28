---
title: "aaaPowerShell příklad monitorování škálování jedním Azure SQL database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu toomonitor a škálování jedné databáze Azure SQL"
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
ms.openlocfilehash: bd8f880fb47b1360ae4962d2b039faa742de258e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-single-sql-database"></a><span data-ttu-id="d1e91-103">Pomocí prostředí PowerShell toomonitor a škálování jedné databáze SQL</span><span class="sxs-lookup"><span data-stu-id="d1e91-103">Use PowerShell toomonitor and scale a single SQL database</span></span>

<span data-ttu-id="d1e91-104">Tento ukázkový skript prostředí PowerShell, že monitorování hello metriky výkonu databáze, se škáluje tooa vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom hello metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="d1e91-104">This PowerShell script example monitors hello performance metrics of a database, scales it tooa higher performance level, and creates an alert rule on one of hello performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="d1e91-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d1e91-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d1e91-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d1e91-106">Clean up deployment</span></span>

<span data-ttu-id="d1e91-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="d1e91-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="d1e91-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d1e91-108">Script explanation</span></span>

<span data-ttu-id="d1e91-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d1e91-109">This script uses hello following commands.</span></span> <span data-ttu-id="d1e91-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="d1e91-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d1e91-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d1e91-111">Command</span></span> | <span data-ttu-id="d1e91-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d1e91-112">Notes</span></span> |
|---|---|
 [<span data-ttu-id="d1e91-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d1e91-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d1e91-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d1e91-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d1e91-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="d1e91-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="d1e91-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="d1e91-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="d1e91-117">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="d1e91-117">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="d1e91-118">Zobrazuje informace o databázi hello využití velikost hello.</span><span class="sxs-lookup"><span data-stu-id="d1e91-118">Shows hello size usage information for hello database.</span></span>|
| [<span data-ttu-id="d1e91-119">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="d1e91-119">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="d1e91-120">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="d1e91-120">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="d1e91-121">Přidat AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="d1e91-121">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="d1e91-122">Nastaví monitor tooautomatically pravidlo výstrahy Dtu hello budoucí.</span><span class="sxs-lookup"><span data-stu-id="d1e91-122">Sets an alert rule tooautomatically monitor DTUs in hello future.</span></span> |
| [<span data-ttu-id="d1e91-123">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d1e91-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="d1e91-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="d1e91-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="d1e91-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1e91-125">Next steps</span></span>

<span data-ttu-id="d1e91-126">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d1e91-126">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d1e91-127">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d1e91-127">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
