---
title: "Elastický fond prostředí PowerShell příklad přesunutí Azure SQL database SQL | Microsoft Docs"
description: "Azure PowerShell ukázkový skript přesunout databázi SQL mezi elastické fondy pomocí prostředí PowerShell"
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
ms.openlocfilehash: 58f14dc668f25f17e93fcaf30f72b15a46d71484
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-create-elastic-pools-and-move-databases-between-elastic-pools"></a><span data-ttu-id="bc2a7-103">Použít PowerShell k vytvoření elastické fondy a přesunutí databází mezi elastické fondy</span><span class="sxs-lookup"><span data-stu-id="bc2a7-103">Use PowerShell to create elastic pools and move databases between elastic pools</span></span>

<span data-ttu-id="bc2a7-104">Tento ukázkový skript prostředí PowerShell vytvoří dvě elastické fondy a přesune do jiného fondu elastické databáze z jednoho fondu elastické a pak se posouvá databázi z fondu elastické databáze na úroveň výkonu jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-104">This PowerShell script example creates two elastic pools and moves a database from one elastic pool into another elastic pool, and then moves a database out of an elastic pool to a single database performance level.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="bc2a7-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bc2a7-105">Sample script</span></span>

<span data-ttu-id="bc2a7-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "přesun databáze mezi fondy")]</span><span class="sxs-lookup"><span data-stu-id="bc2a7-106">[!code-powershell[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "Move database between pools")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="bc2a7-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bc2a7-107">Clean up deployment</span></span>

<span data-ttu-id="bc2a7-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="bc2a7-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bc2a7-109">Script explanation</span></span>

<span data-ttu-id="bc2a7-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-110">This script uses the following commands.</span></span> <span data-ttu-id="bc2a7-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="bc2a7-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bc2a7-112">Command</span></span> | <span data-ttu-id="bc2a7-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bc2a7-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bc2a7-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bc2a7-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="bc2a7-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bc2a7-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="bc2a7-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="bc2a7-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="bc2a7-118">Nový AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="bc2a7-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="bc2a7-119">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="bc2a7-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="bc2a7-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="bc2a7-121">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="bc2a7-122">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="bc2a7-122">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="bc2a7-123">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-123">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="bc2a7-124">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bc2a7-124">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="bc2a7-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="bc2a7-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="bc2a7-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc2a7-126">Next steps</span></span>

<span data-ttu-id="bc2a7-127">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bc2a7-127">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="bc2a7-128">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a7-128">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
