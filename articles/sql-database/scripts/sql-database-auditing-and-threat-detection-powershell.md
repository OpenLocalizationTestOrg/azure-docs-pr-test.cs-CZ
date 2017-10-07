---
title: "Příklad auditování hrozby aaaPowerShell detekce – Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu tooconfigure auditování a detekce hrozeb v databázi SQL Azure"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 97e057ac6efe5e730404ae796bc01e7e5c70df35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-sql-database-auditing-and-threat-detection"></a><span data-ttu-id="67b7f-103">Pomocí prostředí PowerShell tooconfigure SQL Database auditování a zjišťování hrozeb</span><span class="sxs-lookup"><span data-stu-id="67b7f-103">Use PowerShell tooconfigure SQL Database auditing and threat detection</span></span>

<span data-ttu-id="67b7f-104">Tento příklad skriptu prostředí PowerShell konfiguruje SQL Database auditování a zjišťování hrozeb.</span><span class="sxs-lookup"><span data-stu-id="67b7f-104">This PowerShell script example configures SQL Database auditing and threat detection.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="67b7f-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="67b7f-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=13-14 "Configure auditing and threat detection")]

## <a name="clean-up-deployment"></a><span data-ttu-id="67b7f-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="67b7f-106">Clean up deployment</span></span>

<span data-ttu-id="67b7f-107">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="67b7f-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="67b7f-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="67b7f-108">Script explanation</span></span>

<span data-ttu-id="67b7f-109">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="67b7f-109">This script uses hello following commands.</span></span> <span data-ttu-id="67b7f-110">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="67b7f-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="67b7f-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="67b7f-111">Command</span></span> | <span data-ttu-id="67b7f-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="67b7f-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="67b7f-113">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="67b7f-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="67b7f-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="67b7f-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="67b7f-115">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="67b7f-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="67b7f-116">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="67b7f-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="67b7f-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="67b7f-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="67b7f-118">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="67b7f-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="67b7f-119">Nové AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="67b7f-119">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="67b7f-120">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="67b7f-120">Creates a Storage account.</span></span> |
| [<span data-ttu-id="67b7f-121">Set-AzureRmSqlDatabaseAuditingPolicy</span><span class="sxs-lookup"><span data-stu-id="67b7f-121">Set-AzureRmSqlDatabaseAuditingPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabaseauditingpolicy) | <span data-ttu-id="67b7f-122">Nastaví hello zásady auditování pro databázi.</span><span class="sxs-lookup"><span data-stu-id="67b7f-122">Sets hello auditing policy for a database.</span></span> |
| [<span data-ttu-id="67b7f-123">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span><span class="sxs-lookup"><span data-stu-id="67b7f-123">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasethreatdetectionpolicy) | <span data-ttu-id="67b7f-124">Nastaví zásadu detekce hrozeb v databázi.</span><span class="sxs-lookup"><span data-stu-id="67b7f-124">Sets a threat detection policy on a database.</span></span> |
| [<span data-ttu-id="67b7f-125">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="67b7f-125">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="67b7f-126">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="67b7f-126">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="67b7f-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67b7f-127">Next steps</span></span>

<span data-ttu-id="67b7f-128">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67b7f-128">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="67b7f-129">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="67b7f-129">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
