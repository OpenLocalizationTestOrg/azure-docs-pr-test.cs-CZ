---
title: "aaaPowerShell příklad vytvoření Azure SQL database | Microsoft Docs"
description: "Azure PowerShell příklad skriptu toocreate databázi Azure SQL"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: ae57b2018f4a550bf2c6da688d6e49bdadf3d3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="85882-103">Použijte toocreate prostředí PowerShell jednu databázi Azure SQL a nakonfigurujte pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="85882-103">Use PowerShell toocreate a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="85882-104">Tento příklad skriptu prostředí PowerShell vytvoří Azure SQL database a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="85882-104">This PowerShell script example creates an Azure SQL database and configures a server-level firewall rule.</span></span> <span data-ttu-id="85882-105">Po úspěšně spustit skript hello hello databáze SQL je přístupná ze všech služeb Azure a hello nakonfigurovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="85882-105">Once hello script has been successfully run, hello SQL Database can be accessed from all Azure services and hello configured IP address.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="85882-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="85882-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="85882-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="85882-107">Clean up deployment</span></span>

<span data-ttu-id="85882-108">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="85882-108">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="85882-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="85882-109">Script explanation</span></span>

<span data-ttu-id="85882-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="85882-110">This script uses hello following commands.</span></span> <span data-ttu-id="85882-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="85882-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="85882-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="85882-112">Command</span></span> | <span data-ttu-id="85882-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="85882-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="85882-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="85882-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="85882-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="85882-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="85882-116">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="85882-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="85882-117">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="85882-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="85882-118">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="85882-118">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="85882-119">Vytvoří brány firewall pravidla tooallow přístup tooall databází SQL na serveru hello z hello zadaný rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="85882-119">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="85882-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="85882-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="85882-121">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="85882-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="85882-122">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="85882-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="85882-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="85882-123">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="85882-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85882-124">Next steps</span></span>

<span data-ttu-id="85882-125">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85882-125">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="85882-126">Další ukázky skriptu PowerShell databáze SQL naleznete v hello [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="85882-126">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>



