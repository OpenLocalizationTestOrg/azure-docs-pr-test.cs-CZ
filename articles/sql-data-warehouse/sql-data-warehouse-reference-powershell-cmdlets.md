---
title: "Rutiny prostředí PowerShell pro Azure SQL Data Warehouse"
description: "Najít nejvyšší rutin prostředí PowerShell pro Azure SQL Data Warehouse, včetně toho, jak pozastavení a obnovení databáze."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: ce3e11587c2e0cb92923868a4f26d7f59c7ef4ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a><span data-ttu-id="d9f40-103">Rutiny prostředí PowerShell a rozhraní REST API pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="d9f40-103">PowerShell cmdlets and REST APIs for SQL Data Warehouse</span></span>
<span data-ttu-id="d9f40-104">Mnoho úloh správy serveru SQL datového skladu lze spravovat pomocí rutin prostředí Azure PowerShell nebo rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="d9f40-104">Many SQL Data Warehouse administration tasks can be managed using either Azure PowerShell cmdlets or REST APIs.</span></span>  <span data-ttu-id="d9f40-105">Níže jsou uvedeny příklady použití příkazů prostředí PowerShell pro automatizaci běžných úkolů v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d9f40-105">Below are some examples of how to use PowerShell commands to automate common tasks in your SQL Data Warehouse.</span></span>  <span data-ttu-id="d9f40-106">Některé dobrými příklady REST, najdete v článku [spravovat škálovatelnost REST][Manage scalability with REST].</span><span class="sxs-lookup"><span data-stu-id="d9f40-106">For some good REST examples, see the article [Manage scalability with REST][Manage scalability with REST].</span></span>

> [!NOTE]
> <span data-ttu-id="d9f40-107">Abyste mohli používat Azure PowerShell s SQL Data Warehouse, je nutné prostředí Azure PowerShell verze 1.0.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d9f40-107">In order to use Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="d9f40-108">Vaše verze můžete zkontrolovat spuštěním **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="d9f40-108">You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="d9f40-109">Nejnovější verzi můžete nainstalovat z [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="d9f40-109">The latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="d9f40-110">Další informace o instalaci nejnovější verze najdete v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell][How to install and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="d9f40-110">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="d9f40-111">Začínáme s rutinami prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d9f40-111">Get started with Azure PowerShell cmdlets</span></span>
1. <span data-ttu-id="d9f40-112">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d9f40-112">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="d9f40-113">Do příkazového řádku prostředí PowerShell spusťte tyto příkazy a přihlaste se k Azure Resource Manager a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="d9f40-113">At the PowerShell prompt, run these commands to sign in to the Azure Resource Manager and select your subscription.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a><span data-ttu-id="d9f40-114">Pozastavení SQL Data Warehouse příklad</span><span class="sxs-lookup"><span data-stu-id="d9f40-114">Pause SQL Data Warehouse Example</span></span>
<span data-ttu-id="d9f40-115">Pozastavení databáze s názvem "Database02", které jsou hostované na serveru s názvem "Server01."</span><span class="sxs-lookup"><span data-stu-id="d9f40-115">Pause a database named "Database02" hosted on a server named "Server01."</span></span>  <span data-ttu-id="d9f40-116">Server je ve skupině prostředků Azure s názvem "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="d9f40-116">The server is in an Azure resource group named "ResourceGroup1."</span></span>

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="d9f40-117">Prostřednictvím kanálu variace, v tomto příkladu předá načtený objekt, který má [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="d9f40-117">A variation, this example pipes the retrieved object to [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span>  <span data-ttu-id="d9f40-118">V důsledku toho je databáze pozastavena.</span><span class="sxs-lookup"><span data-stu-id="d9f40-118">As a result, the database is paused.</span></span> <span data-ttu-id="d9f40-119">Poslední příkaz zobrazí výsledky.</span><span class="sxs-lookup"><span data-stu-id="d9f40-119">The final command shows the results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a><span data-ttu-id="d9f40-120">Spusťte SQL Data Warehouse příklad</span><span class="sxs-lookup"><span data-stu-id="d9f40-120">Start SQL Data Warehouse Example</span></span>
<span data-ttu-id="d9f40-121">Operace obnovení databáze s názvem "Database02", které jsou hostované na serveru s názvem "Server01."</span><span class="sxs-lookup"><span data-stu-id="d9f40-121">Resume operation of a database named "Database02" hosted on a server named "Server01."</span></span> <span data-ttu-id="d9f40-122">Server je součástí skupiny prostředků s názvem "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="d9f40-122">The server is contained in a resource group named "ResourceGroup1."</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="d9f40-123">Variace, načte tento příklad databáze s názvem "Database02" ze serveru s názvem "Server01", který je obsažen ve skupině prostředků s názvem "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="d9f40-123">A variation, this example retrieves a database named "Database02" from a server named "Server01" that is contained in a resource group named "ResourceGroup1."</span></span> <span data-ttu-id="d9f40-124">Ji prostřednictvím kanálu předá načtený objekt, který má [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="d9f40-124">It pipes the retrieved object to [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> <span data-ttu-id="d9f40-125">Všimněte si, že pokud je váš server foo.database.windows.net, použijte "foo" jako parametr ServerName - rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d9f40-125">Note that if your server is foo.database.windows.net, use "foo" as the -ServerName in the PowerShell cmdlets.</span></span>
> 
> 

## <a name="other-supported-powershell-cmdlets"></a><span data-ttu-id="d9f40-126">Ostatní podporované rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d9f40-126">Other supported PowerShell cmdlets</span></span>
<span data-ttu-id="d9f40-127">Tyto rutiny prostředí PowerShell jsou podporovány službou Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d9f40-127">These PowerShell cmdlets are supported with Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="d9f40-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="d9f40-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span><span class="sxs-lookup"><span data-stu-id="d9f40-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span></span>
* <span data-ttu-id="d9f40-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span><span class="sxs-lookup"><span data-stu-id="d9f40-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span></span>
* <span data-ttu-id="d9f40-131">[Nový AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-131">[New-AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="d9f40-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="d9f40-133">[Obnovení AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-133">[Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="d9f40-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="d9f40-135">[Select-AzureRmSubscription][Select-AzureRmSubscription]</span><span class="sxs-lookup"><span data-stu-id="d9f40-135">[Select-AzureRmSubscription][Select-AzureRmSubscription]</span></span>
* <span data-ttu-id="d9f40-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="d9f40-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="d9f40-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9f40-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9f40-138">Next steps</span></span>
<span data-ttu-id="d9f40-139">Další příklady prostředí PowerShell najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d9f40-139">For more PowerShell examples, see:</span></span>

* <span data-ttu-id="d9f40-140">[Vytvoření SQL Data Warehouse pomocí prostředí PowerShell][Create a SQL Data Warehouse using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="d9f40-140">[Create a SQL Data Warehouse using PowerShell][Create a SQL Data Warehouse using PowerShell]</span></span>
* <span data-ttu-id="d9f40-141">[Obnovení databáze][Database restore]</span><span class="sxs-lookup"><span data-stu-id="d9f40-141">[Database restore][Database restore]</span></span>

<span data-ttu-id="d9f40-142">Další úlohy, které je možné automatizovat pomocí prostředí PowerShell, najdete v části [rutiny databáze SQL Azure][Azure SQL Database Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="d9f40-142">For other tasks which can be automated with PowerShell, see [Azure SQL Database Cmdlets][Azure SQL Database Cmdlets].</span></span> <span data-ttu-id="d9f40-143">Všimněte si, že ne všechny rutiny Azure SQL Database jsou podporovány pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d9f40-143">Note that not all Azure SQL Database cmdlets are supported for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="d9f40-144">Seznam úloh, které je možné automatizovat se zbytkem najdete v tématu [operace u databází SQL Azure][Operations for Azure SQL Databases].</span><span class="sxs-lookup"><span data-stu-id="d9f40-144">For a list of tasks which can be automated with REST, see [Operations for Azure SQL Databases][Operations for Azure SQL Databases].</span></span>

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
