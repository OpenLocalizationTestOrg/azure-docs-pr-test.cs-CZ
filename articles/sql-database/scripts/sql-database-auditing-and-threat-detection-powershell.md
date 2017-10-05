---
title: "Prostředí PowerShell příklad auditování hrozby detekce Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript konfigurace auditování a detekce hrozeb v databázi SQL Azure"
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
ms.openlocfilehash: e039d35474bff3b066a6605a97e04d4a81c365eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-configure-sql-database-auditing-and-threat-detection"></a><span data-ttu-id="c5e95-103">Konfigurace SQL Database auditování a zjišťování hrozeb pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5e95-103">Use PowerShell to configure SQL Database auditing and threat detection</span></span>

<span data-ttu-id="c5e95-104">Tento příklad skriptu prostředí PowerShell konfiguruje SQL Database auditování a zjišťování hrozeb.</span><span class="sxs-lookup"><span data-stu-id="c5e95-104">This PowerShell script example configures SQL Database auditing and threat detection.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="c5e95-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c5e95-105">Sample script</span></span>

<span data-ttu-id="c5e95-106">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=13-14 "konfigurace auditování a zjišťování hrozeb")]</span><span class="sxs-lookup"><span data-stu-id="c5e95-106">[!code-powershell[main](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=13-14 "Configure auditing and threat detection")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="c5e95-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="c5e95-107">Clean up deployment</span></span>

<span data-ttu-id="c5e95-108">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="c5e95-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="c5e95-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c5e95-109">Script explanation</span></span>

<span data-ttu-id="c5e95-110">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="c5e95-110">This script uses the following commands.</span></span> <span data-ttu-id="c5e95-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c5e95-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c5e95-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c5e95-112">Command</span></span> | <span data-ttu-id="c5e95-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c5e95-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c5e95-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c5e95-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c5e95-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="c5e95-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c5e95-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="c5e95-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="c5e95-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="c5e95-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="c5e95-118">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="c5e95-118">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="c5e95-119">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="c5e95-119">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="c5e95-120">Nové AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="c5e95-120">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="c5e95-121">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c5e95-121">Creates a Storage account.</span></span> |
| [<span data-ttu-id="c5e95-122">Set-AzureRmSqlDatabaseAuditingPolicy</span><span class="sxs-lookup"><span data-stu-id="c5e95-122">Set-AzureRmSqlDatabaseAuditingPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabaseauditingpolicy) | <span data-ttu-id="c5e95-123">Nastavení zásad auditování pro databázi.</span><span class="sxs-lookup"><span data-stu-id="c5e95-123">Sets the auditing policy for a database.</span></span> |
| [<span data-ttu-id="c5e95-124">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span><span class="sxs-lookup"><span data-stu-id="c5e95-124">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasethreatdetectionpolicy) | <span data-ttu-id="c5e95-125">Nastaví zásadu detekce hrozeb v databázi.</span><span class="sxs-lookup"><span data-stu-id="c5e95-125">Sets a threat detection policy on a database.</span></span> |
| [<span data-ttu-id="c5e95-126">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c5e95-126">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="c5e95-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="c5e95-127">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="c5e95-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5e95-128">Next steps</span></span>

<span data-ttu-id="c5e95-129">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c5e95-129">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c5e95-130">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c5e95-130">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
