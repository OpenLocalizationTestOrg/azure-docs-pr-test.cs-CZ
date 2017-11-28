---
title: "aaaPowerShell příklad. kopírování Azure SQL databáze – nový server | Microsoft Docs"
description: "Azure PowerShell příklad skriptu toocopy nový server tooa databáze SQL"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: c08f993bd75913481b1d534857ac057263e1d02b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocopy-a-sql-database-tooa-new-server"></a><span data-ttu-id="7ddf2-103">Pomocí prostředí PowerShell toocopy nový server tooa databáze SQL</span><span class="sxs-lookup"><span data-stu-id="7ddf2-103">Use PowerShell toocopy a SQL database tooa new server</span></span>

<span data-ttu-id="7ddf2-104">Tento ukázkový skript prostředí PowerShell vytvoří kopii existující databázi v nový server.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-104">This PowerShell script example creates a copy of an existing database in a new server.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-tooa-new-server"></a><span data-ttu-id="7ddf2-105">Zkopírujte nový server databáze tooa</span><span class="sxs-lookup"><span data-stu-id="7ddf2-105">Copy a database tooa new server</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database toonew server")]

## <a name="clean-up-deployment"></a><span data-ttu-id="7ddf2-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="7ddf2-106">Clean up deployment</span></span>

<span data-ttu-id="7ddf2-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="7ddf2-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7ddf2-108">Script explanation</span></span>

<span data-ttu-id="7ddf2-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-109">This script uses hello following commands.</span></span> <span data-ttu-id="7ddf2-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7ddf2-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7ddf2-111">Command</span></span> | <span data-ttu-id="7ddf2-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7ddf2-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7ddf2-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7ddf2-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="7ddf2-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7ddf2-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="7ddf2-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="7ddf2-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="7ddf2-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="7ddf2-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="7ddf2-118">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="7ddf2-119">Nové AzureRmSqlDatabaseCopy</span><span class="sxs-lookup"><span data-stu-id="7ddf2-119">New-AzureRmSqlDatabaseCopy</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | <span data-ttu-id="7ddf2-120">Vytvoří kopii databáze, který používá hello snímku na hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-120">Creates a copy of a database that uses hello snapshot at hello current time.</span></span> |
| [<span data-ttu-id="7ddf2-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7ddf2-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="7ddf2-122">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ddf2-122">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="7ddf2-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ddf2-123">Next steps</span></span>

<span data-ttu-id="7ddf2-124">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7ddf2-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7ddf2-125">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7ddf2-125">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
