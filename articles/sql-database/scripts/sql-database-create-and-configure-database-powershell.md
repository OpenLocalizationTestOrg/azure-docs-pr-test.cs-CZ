---
title: "Prostředí PowerShell příklad vytvoření Azure SQL database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript k vytvoření databáze Azure SQL"
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
ms.openlocfilehash: 1b6809007e6717b7f8847452b2fa5b38d4ab5ccc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="da2f0-103">Použít PowerShell k vytvoření jedné databáze Azure SQL a nakonfigurujte pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="da2f0-103">Use PowerShell to create a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="da2f0-104">Tento příklad skriptu prostředí PowerShell vytvoří Azure SQL database a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="da2f0-104">This PowerShell script example creates an Azure SQL database and configures a server-level firewall rule.</span></span> <span data-ttu-id="da2f0-105">Jakmile úspěšně spustit skript SQL Database je přístupná ze všech služeb Azure a nakonfigurovanou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="da2f0-105">Once the script has been successfully run, the SQL Database can be accessed from all Azure services and the configured IP address.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="da2f0-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="da2f0-106">Sample script</span></span>

<span data-ttu-id="da2f0-107">[!code-powershell[hlavní](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "vytvoření databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="da2f0-107">[!code-powershell[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="da2f0-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="da2f0-108">Clean up deployment</span></span>

<span data-ttu-id="da2f0-109">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="da2f0-109">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="da2f0-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="da2f0-110">Script explanation</span></span>

<span data-ttu-id="da2f0-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="da2f0-111">This script uses the following commands.</span></span> <span data-ttu-id="da2f0-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="da2f0-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="da2f0-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="da2f0-113">Command</span></span> | <span data-ttu-id="da2f0-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="da2f0-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="da2f0-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="da2f0-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="da2f0-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="da2f0-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="da2f0-117">Nový AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="da2f0-117">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="da2f0-118">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="da2f0-118">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="da2f0-119">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="da2f0-119">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="da2f0-120">Vytvoří pravidlo brány firewall pro povolení přístupu pro všechny SQL databáze na serveru ze zadané rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="da2f0-120">Creates a firewall rule to allow access to all SQL Databases on the server from the entered IP address range.</span></span> |
| [<span data-ttu-id="da2f0-121">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="da2f0-121">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="da2f0-122">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="da2f0-122">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="da2f0-123">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="da2f0-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="da2f0-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="da2f0-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="da2f0-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da2f0-125">Next steps</span></span>

<span data-ttu-id="da2f0-126">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="da2f0-126">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="da2f0-127">Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="da2f0-127">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>



