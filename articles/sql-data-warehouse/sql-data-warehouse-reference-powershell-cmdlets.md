---
title: aaaPowerShell rutiny pro Azure SQL Data Warehouse
description: "Najít hello nejvyšší rutiny prostředí PowerShell pro Azure SQL Data Warehouse včetně jak toopause a obnovit databázi."
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
ms.openlocfilehash: 84353b56131cf856e0724d338d7ed186fd2ceeaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a><span data-ttu-id="6d592-103">Rutiny prostředí PowerShell a rozhraní REST API pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6d592-103">PowerShell cmdlets and REST APIs for SQL Data Warehouse</span></span>
<span data-ttu-id="6d592-104">Mnoho úloh správy serveru SQL datového skladu lze spravovat pomocí rutin prostředí Azure PowerShell nebo rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="6d592-104">Many SQL Data Warehouse administration tasks can be managed using either Azure PowerShell cmdlets or REST APIs.</span></span>  <span data-ttu-id="6d592-105">Zde jsou některé příklady jak toouse prostředí PowerShell příkazy tooautomate běžné úlohy v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6d592-105">Below are some examples of how toouse PowerShell commands tooautomate common tasks in your SQL Data Warehouse.</span></span>  <span data-ttu-id="6d592-106">Některé dobrými příklady REST, najdete v článku hello [spravovat škálovatelnost REST][Manage scalability with REST].</span><span class="sxs-lookup"><span data-stu-id="6d592-106">For some good REST examples, see hello article [Manage scalability with REST][Manage scalability with REST].</span></span>

> [!NOTE]
> <span data-ttu-id="6d592-107">Pořadí toouse prostředí Azure PowerShell s SQL Data Warehouse, je nutné prostředí Azure PowerShell verze 1.0.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6d592-107">In order toouse Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="6d592-108">Vaše verze můžete zkontrolovat spuštěním **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="6d592-108">You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="6d592-109">je možné nainstalovat nejnovější verzi Hello od [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="6d592-109">hello latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="6d592-110">Další informace o instalaci nejnovější verze hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="6d592-110">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="6d592-111">Začínáme s rutinami prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d592-111">Get started with Azure PowerShell cmdlets</span></span>
1. <span data-ttu-id="6d592-112">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6d592-112">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="6d592-113">Na příkazovém řádku prostředí PowerShell text hello spusťte tyto příkazy toosign v toohello Azure Resource Manager a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="6d592-113">At hello PowerShell prompt, run these commands toosign in toohello Azure Resource Manager and select your subscription.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a><span data-ttu-id="6d592-114">Pozastavení SQL Data Warehouse příklad</span><span class="sxs-lookup"><span data-stu-id="6d592-114">Pause SQL Data Warehouse Example</span></span>
<span data-ttu-id="6d592-115">Pozastavení databáze s názvem "Database02", které jsou hostované na serveru s názvem "Server01."</span><span class="sxs-lookup"><span data-stu-id="6d592-115">Pause a database named "Database02" hosted on a server named "Server01."</span></span>  <span data-ttu-id="6d592-116">Hello server je ve skupině prostředků Azure s názvem "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="6d592-116">hello server is in an Azure resource group named "ResourceGroup1."</span></span>

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="6d592-117">Variace, v tomto příkladu prostřednictvím kanálu předá objekt hello načíst příliš[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="6d592-117">A variation, this example pipes hello retrieved object too[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span>  <span data-ttu-id="6d592-118">V důsledku toho se pozastaví hello databáze.</span><span class="sxs-lookup"><span data-stu-id="6d592-118">As a result, hello database is paused.</span></span> <span data-ttu-id="6d592-119">poslední příkaz Hello zobrazuje výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="6d592-119">hello final command shows hello results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a><span data-ttu-id="6d592-120">Spusťte SQL Data Warehouse příklad</span><span class="sxs-lookup"><span data-stu-id="6d592-120">Start SQL Data Warehouse Example</span></span>
<span data-ttu-id="6d592-121">Operace obnovení databáze s názvem "Database02", které jsou hostované na serveru s názvem "Server01."</span><span class="sxs-lookup"><span data-stu-id="6d592-121">Resume operation of a database named "Database02" hosted on a server named "Server01."</span></span> <span data-ttu-id="6d592-122">Hello server je obsažen ve skupině prostředků s názvem "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="6d592-122">hello server is contained in a resource group named "ResourceGroup1."</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="6d592-123">Variace, načte tento příklad databáze s názvem "Database02" ze serveru s názvem "Server01", který je obsažen ve skupině prostředků s názvem "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="6d592-123">A variation, this example retrieves a database named "Database02" from a server named "Server01" that is contained in a resource group named "ResourceGroup1."</span></span> <span data-ttu-id="6d592-124">Ji prostřednictvím kanálu předá objekt hello načíst příliš[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="6d592-124">It pipes hello retrieved object too[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> <span data-ttu-id="6d592-125">Všimněte si, že pokud je váš server foo.database.windows.net, použijte "foo" jak hello - ServerName hello rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6d592-125">Note that if your server is foo.database.windows.net, use "foo" as hello -ServerName in hello PowerShell cmdlets.</span></span>
> 
> 

## <a name="other-supported-powershell-cmdlets"></a><span data-ttu-id="6d592-126">Ostatní podporované rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d592-126">Other supported PowerShell cmdlets</span></span>
<span data-ttu-id="6d592-127">Tyto rutiny prostředí PowerShell jsou podporovány službou Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6d592-127">These PowerShell cmdlets are supported with Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="6d592-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="6d592-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span><span class="sxs-lookup"><span data-stu-id="6d592-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span></span>
* <span data-ttu-id="6d592-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span><span class="sxs-lookup"><span data-stu-id="6d592-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span></span>
* <span data-ttu-id="6d592-131">[Nový AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-131">[New-AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="6d592-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="6d592-133">[Obnovení AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-133">[Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="6d592-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="6d592-135">[Select-AzureRmSubscription][Select-AzureRmSubscription]</span><span class="sxs-lookup"><span data-stu-id="6d592-135">[Select-AzureRmSubscription][Select-AzureRmSubscription]</span></span>
* <span data-ttu-id="6d592-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="6d592-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="6d592-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d592-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d592-138">Next steps</span></span>
<span data-ttu-id="6d592-139">Další příklady prostředí PowerShell najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="6d592-139">For more PowerShell examples, see:</span></span>

* <span data-ttu-id="6d592-140">[Vytvoření SQL Data Warehouse pomocí prostředí PowerShell][Create a SQL Data Warehouse using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="6d592-140">[Create a SQL Data Warehouse using PowerShell][Create a SQL Data Warehouse using PowerShell]</span></span>
* <span data-ttu-id="6d592-141">[Obnovení databáze][Database restore]</span><span class="sxs-lookup"><span data-stu-id="6d592-141">[Database restore][Database restore]</span></span>

<span data-ttu-id="6d592-142">Další úlohy, které je možné automatizovat pomocí prostředí PowerShell, najdete v části [rutiny databáze SQL Azure][Azure SQL Database Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="6d592-142">For other tasks which can be automated with PowerShell, see [Azure SQL Database Cmdlets][Azure SQL Database Cmdlets].</span></span> <span data-ttu-id="6d592-143">Všimněte si, že ne všechny rutiny Azure SQL Database jsou podporovány pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6d592-143">Note that not all Azure SQL Database cmdlets are supported for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="6d592-144">Seznam úloh, které je možné automatizovat se zbytkem najdete v tématu [operace u databází SQL Azure][Operations for Azure SQL Databases].</span><span class="sxs-lookup"><span data-stu-id="6d592-144">For a list of tasks which can be automated with REST, see [Operations for Azure SQL Databases][Operations for Azure SQL Databases].</span></span>

<!--Image references-->

<!--Article references-->
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
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
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points tooSelect-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
