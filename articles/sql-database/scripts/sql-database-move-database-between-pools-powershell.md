---
title: "elastický fond aaaPowerShell příklad přesunutí Azure SQL database SQL | Microsoft Docs"
description: "Azure PowerShell příklad skriptu toomove databázi SQL mezi elastické fondy pomocí prostředí PowerShell"
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
ms.openlocfilehash: 501e82ce93a31264d0625fb0243b4e44dcb2d007
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-elastic-pools-and-move-databases-between-elastic-pools"></a><span data-ttu-id="74f0e-103">Pomocí prostředí PowerShell toocreate elastické fondy a přesunutí databází mezi elastické fondy</span><span class="sxs-lookup"><span data-stu-id="74f0e-103">Use PowerShell toocreate elastic pools and move databases between elastic pools</span></span>

<span data-ttu-id="74f0e-104">Tento ukázkový skript prostředí PowerShell vytvoří dvě elastické fondy a přesune do jiného fondu elastické databáze z jednoho fondu elastické a pak se posouvá databázi mimo úroveň výkonu izolovaných databází tooa elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="74f0e-104">This PowerShell script example creates two elastic pools and moves a database from one elastic pool into another elastic pool, and then moves a database out of an elastic pool tooa single database performance level.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="74f0e-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="74f0e-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="74f0e-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="74f0e-106">Clean up deployment</span></span>

<span data-ttu-id="74f0e-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="74f0e-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="74f0e-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="74f0e-108">Script explanation</span></span>

<span data-ttu-id="74f0e-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="74f0e-109">This script uses hello following commands.</span></span> <span data-ttu-id="74f0e-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="74f0e-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="74f0e-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="74f0e-111">Command</span></span> | <span data-ttu-id="74f0e-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="74f0e-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="74f0e-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="74f0e-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="74f0e-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="74f0e-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="74f0e-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="74f0e-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="74f0e-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="74f0e-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="74f0e-117">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="74f0e-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="74f0e-118">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="74f0e-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="74f0e-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="74f0e-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="74f0e-120">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="74f0e-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="74f0e-121">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="74f0e-121">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="74f0e-122">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="74f0e-122">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="74f0e-123">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="74f0e-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="74f0e-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="74f0e-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="74f0e-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74f0e-125">Next steps</span></span>

<span data-ttu-id="74f0e-126">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="74f0e-126">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="74f0e-127">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="74f0e-127">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
