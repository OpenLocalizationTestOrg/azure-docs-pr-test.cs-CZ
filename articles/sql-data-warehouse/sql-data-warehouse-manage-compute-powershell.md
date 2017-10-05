---
title: "Spravovat výpočetní výkon v Azure SQL Data Warehouse (PowerShell) | Microsoft Docs"
description: "Úkoly prostředí PowerShell pro správu výpočetního výkonu. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6a185d96447c2e1b0b463439dd062081e783da5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="7db3b-105">Spravovat výpočetní výkon v Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="7db3b-105">Manage compute power in Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7db3b-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="7db3b-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="7db3b-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7db3b-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="7db3b-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7db3b-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="7db3b-109">REST</span><span class="sxs-lookup"><span data-stu-id="7db3b-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="7db3b-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="7db3b-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a><span data-ttu-id="7db3b-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7db3b-111">Before you begin</span></span>
### <a name="install-the-latest-version-of-azure-powershell"></a><span data-ttu-id="7db3b-112">Nainstalujte nejnovější verzi prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7db3b-112">Install the latest version of Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="7db3b-113">Pomocí prostředí Azure PowerShell s SQL Data Warehouse, je nutné prostředí Azure PowerShell verze 1.0.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7db3b-113">To use Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="7db3b-114">K ověření vaší aktuální verzí, spusťte příkaz **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="7db3b-114">To verify your current version run the command **Get-Module -ListAvailable -Name Azure**.</span></span> <span data-ttu-id="7db3b-115">Můžete nainstalovat nejnovější verzi z [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="7db3b-115">You can install the latest version from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="7db3b-116">Další informace najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell][How to install and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="7db3b-116">For more information, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="7db3b-117">Začínáme s rutinami prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7db3b-117">Get started with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="7db3b-118">Abyste mohli začít:</span><span class="sxs-lookup"><span data-stu-id="7db3b-118">To get started:</span></span>

1. <span data-ttu-id="7db3b-119">Otevřete prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7db3b-119">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="7db3b-120">Do příkazového řádku prostředí PowerShell spusťte tyto příkazy a přihlaste se k Azure Resource Manager a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="7db3b-120">At the PowerShell prompt, run these commands to sign in to the Azure Resource Manager and select your subscription.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a><span data-ttu-id="7db3b-121">Škálování výpočetní výkon</span><span class="sxs-lookup"><span data-stu-id="7db3b-121">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="7db3b-122">Chcete-li změnit jednotkami Dwu, použijte [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7db3b-122">To change the DWUs, use the [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase] PowerShell cmdlet.</span></span> <span data-ttu-id="7db3b-123">Následující příklad nastaví DW1000 cíle na úrovni služby pro databázi MySQLDW, který je hostován na serveru MyServer.</span><span class="sxs-lookup"><span data-stu-id="7db3b-123">The following example sets the service level objective to DW1000 for the database MySQLDW which is hosted on server MyServer.</span></span>

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="7db3b-124">Pozastavit výpočetní</span><span class="sxs-lookup"><span data-stu-id="7db3b-124">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="7db3b-125">Chcete-li pozastavit databázi, použijte [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] rutiny.</span><span class="sxs-lookup"><span data-stu-id="7db3b-125">To pause a database, use the [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="7db3b-126">Následující příklad pozastaví databáze s názvem Database02 hostovaná na serveru s názvem Server01.</span><span class="sxs-lookup"><span data-stu-id="7db3b-126">The following example pauses a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="7db3b-127">Server je ve skupině prostředků Azure s názvem ResourceGroup1.</span><span class="sxs-lookup"><span data-stu-id="7db3b-127">The server is in an Azure resource group named ResourceGroup1.</span></span>

> [!NOTE]
> <span data-ttu-id="7db3b-128">Všimněte si, že pokud je váš server foo.database.windows.net, použijte "foo" jako parametr ServerName - rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7db3b-128">Note that if your server is foo.database.windows.net, use "foo" as the -ServerName in the PowerShell cmdlets.</span></span>
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="7db3b-129">Variace, načte tento další příklad databázi do $database objektu.</span><span class="sxs-lookup"><span data-stu-id="7db3b-129">A variation, this next example retrieves the database into the $database object.</span></span> <span data-ttu-id="7db3b-130">Je následně prostřednictvím kanálu předá objekt, který má [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="7db3b-130">It then pipes the object to [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span> <span data-ttu-id="7db3b-131">Výsledky jsou uloženy v resultDatabase objektu.</span><span class="sxs-lookup"><span data-stu-id="7db3b-131">The results are stored in the object resultDatabase.</span></span> <span data-ttu-id="7db3b-132">Poslední příkaz zobrazí výsledky.</span><span class="sxs-lookup"><span data-stu-id="7db3b-132">The final command shows the results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="7db3b-133">Obnovit výpočetní</span><span class="sxs-lookup"><span data-stu-id="7db3b-133">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="7db3b-134">Chcete-li spustit databázi, použijte [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] rutiny.</span><span class="sxs-lookup"><span data-stu-id="7db3b-134">To start a database, use the [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="7db3b-135">Následující příklad spustí databáze s názvem Database02 hostovaná na serveru s názvem Server01.</span><span class="sxs-lookup"><span data-stu-id="7db3b-135">The following example starts a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="7db3b-136">Server je ve skupině prostředků Azure s názvem ResourceGroup1.</span><span class="sxs-lookup"><span data-stu-id="7db3b-136">The server is in an Azure resource group named ResourceGroup1.</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="7db3b-137">Variace, načte tento další příklad databázi do $database objektu.</span><span class="sxs-lookup"><span data-stu-id="7db3b-137">A variation, this next example retrieves the database into the $database object.</span></span> <span data-ttu-id="7db3b-138">Je následně prostřednictvím kanálu předá objekt, který má [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] a ukládá výsledky do $resultDatabase.</span><span class="sxs-lookup"><span data-stu-id="7db3b-138">It then pipes the object to [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] and stores the results in $resultDatabase.</span></span> <span data-ttu-id="7db3b-139">Poslední příkaz zobrazí výsledky.</span><span class="sxs-lookup"><span data-stu-id="7db3b-139">The final command shows the results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="7db3b-140">Zkontrolujte stav databáze</span><span class="sxs-lookup"><span data-stu-id="7db3b-140">Check database state</span></span>

<span data-ttu-id="7db3b-141">Jak je znázorněno v předchozích příkladech, můžete použít jednu [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] rutiny získat informace o databázi, a tím Kontrola stavu, ale také použít jako argument.</span><span class="sxs-lookup"><span data-stu-id="7db3b-141">As shown in the above examples, one can use [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase] cmdlet to get information on a database, thereby checking the status, but also to use as an argument.</span></span> 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

<span data-ttu-id="7db3b-142">Které budou jako výsledek v něco</span><span class="sxs-lookup"><span data-stu-id="7db3b-142">Which will result in something like</span></span> 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

<span data-ttu-id="7db3b-143">Kde je můžete pak zkontrolovat *stav* databáze.</span><span class="sxs-lookup"><span data-stu-id="7db3b-143">Where you can then check to see the *Status* of the database.</span></span> <span data-ttu-id="7db3b-144">V takovém případě se zobrazí, že tato databáze je online.</span><span class="sxs-lookup"><span data-stu-id="7db3b-144">In this case, you can see that this database is online.</span></span> 

<span data-ttu-id="7db3b-145">Když spustíte tento příkaz, měli byste obdržet hodnotou stavu buď Online, pozastavení, obnovování, škálování a pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="7db3b-145">When you run this command, you should receive a Status value of either Online, Pausing, Resuming, Scaling, and Paused.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="7db3b-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7db3b-146">Next steps</span></span>
<span data-ttu-id="7db3b-147">Další úlohy správy, najdete v části [přehled správy][Management overview].</span><span class="sxs-lookup"><span data-stu-id="7db3b-147">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
