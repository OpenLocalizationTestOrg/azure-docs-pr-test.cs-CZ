---
title: Obnovit Azure SQL Data Warehouse (PowerShell) | Microsoft Docs
description: "Prostředí PowerShell úlohy pro obnovení Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: ac62f154-c8b0-4c33-9c42-f480808aa1d2
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 6286c0e682bae2d3bf0435a25b8077a53b117b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="491a6-103">Obnovit Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="491a6-103">Restore an Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="491a6-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="491a6-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="491a6-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="491a6-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="491a6-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="491a6-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="491a6-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="491a6-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="491a6-108">V tomto článku se dozvíte, jak obnovit Azure SQL Data Warehouse pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="491a6-108">In this article you will learn how to restore an Azure SQL Data Warehouse using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="491a6-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="491a6-109">Before you begin</span></span>
<span data-ttu-id="491a6-110">**Ověření vaší DTU kapacity.**</span><span class="sxs-lookup"><span data-stu-id="491a6-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="491a6-111">Každý datový sklad SQL je hostitelem serveru SQL (např. myserver.database.windows.net), který má kvóty DTU.</span><span class="sxs-lookup"><span data-stu-id="491a6-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="491a6-112">Před obnovením SQL Data Warehouse, ověřte, zda serveru SQL server má dostatek zbývající kvóty DTU pro databáze obnovena.</span><span class="sxs-lookup"><span data-stu-id="491a6-112">Before you can restore a SQL Data Warehouse, verify that the your SQL server has enough remaining DTU quota for the database being restored.</span></span> <span data-ttu-id="491a6-113">Informace o výpočtu DTU potřeby nebo požádejte o další DTU najdete v tématu [žádosti o změnu kvóty DTU][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="491a6-113">To learn how to calculate DTU needed or to request more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="491a6-114">Instalace PowerShellu</span><span class="sxs-lookup"><span data-stu-id="491a6-114">Install PowerShell</span></span>
<span data-ttu-id="491a6-115">Abyste mohli používat Azure PowerShell s SQL Data Warehouse, musíte nainstalovat Azure PowerShell verze 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="491a6-115">In order to use Azure PowerShell with SQL Data Warehouse, you will need to install Azure PowerShell version 1.0 or greater.</span></span>  <span data-ttu-id="491a6-116">Vaše verze můžete zkontrolovat spuštěním **Get-Module - ListAvailable-AzureRM název**.</span><span class="sxs-lookup"><span data-stu-id="491a6-116">You can check your version by running **Get-Module -ListAvailable -Name AzureRM**.</span></span>  <span data-ttu-id="491a6-117">Nejnovější verzi můžete nainstalovat z [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="491a6-117">The latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="491a6-118">Další informace o instalaci nejnovější verze najdete v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell][How to install and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="491a6-118">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="491a6-119">Obnovit databázi active nebo pozastavena</span><span class="sxs-lookup"><span data-stu-id="491a6-119">Restore an active or paused database</span></span>
<span data-ttu-id="491a6-120">K obnovení databázi z použití snímku [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="491a6-120">To restore a database from a snapshot use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet.</span></span>

1. <span data-ttu-id="491a6-121">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="491a6-121">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="491a6-122">Připojte se ke svému účtu Azure a seznamu všechna předplatná spojená s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="491a6-122">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="491a6-123">Vyberte odběr, který obsahuje databázi obnovit.</span><span class="sxs-lookup"><span data-stu-id="491a6-123">Select the subscription that contains the database to be restored.</span></span>
4. <span data-ttu-id="491a6-124">Zobrazí seznam bodů obnovení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="491a6-124">List the restore points for the database.</span></span>
5. <span data-ttu-id="491a6-125">Vyberte bod požadované obnovení pomocí RestorePointCreationDate.</span><span class="sxs-lookup"><span data-stu-id="491a6-125">Pick the desired restore point using the RestorePointCreationDate.</span></span>
6. <span data-ttu-id="491a6-126">Obnovení databáze do bodu požadované obnovení.</span><span class="sxs-lookup"><span data-stu-id="491a6-126">Restore the database to the desired restore point.</span></span>
7. <span data-ttu-id="491a6-127">Ověřte, že obnovené databáze je online.</span><span class="sxs-lookup"><span data-stu-id="491a6-127">Verify that the restored database is online.</span></span>

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> <span data-ttu-id="491a6-128">Po dokončení obnovení můžete nakonfigurovat obnovené databáze pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="491a6-128">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="491a6-129">Obnovení odstraněné databáze</span><span class="sxs-lookup"><span data-stu-id="491a6-129">Restore a deleted database</span></span>
<span data-ttu-id="491a6-130">Chcete-li obnovit odstraněnou databázi, použijte [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny.</span><span class="sxs-lookup"><span data-stu-id="491a6-130">To restore a deleted database, use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="491a6-131">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="491a6-131">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="491a6-132">Připojte se ke svému účtu Azure a seznamu všechna předplatná spojená s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="491a6-132">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="491a6-133">Vyberte odběr, který obsahuje obnovit odstraněnou databázi.</span><span class="sxs-lookup"><span data-stu-id="491a6-133">Select the subscription that contains the deleted database to be restored.</span></span>
4. <span data-ttu-id="491a6-134">Získejte konkrétní odstraněnou databázi.</span><span class="sxs-lookup"><span data-stu-id="491a6-134">Get the specific deleted database.</span></span>
5. <span data-ttu-id="491a6-135">Obnovte odstraněnou databázi.</span><span class="sxs-lookup"><span data-stu-id="491a6-135">Restore the deleted database.</span></span>
6. <span data-ttu-id="491a6-136">Ověřte, že obnovené databáze je online.</span><span class="sxs-lookup"><span data-stu-id="491a6-136">Verify that the restored database is online.</span></span>

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="491a6-137">Po dokončení obnovení můžete nakonfigurovat obnovené databáze pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="491a6-137">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a><span data-ttu-id="491a6-138">Obnovení z Azure geografické oblasti</span><span class="sxs-lookup"><span data-stu-id="491a6-138">Restore from an Azure geographical region</span></span>
<span data-ttu-id="491a6-139">Chcete-li obnovit databázi, použijte [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny.</span><span class="sxs-lookup"><span data-stu-id="491a6-139">To recover a database, use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="491a6-140">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="491a6-140">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="491a6-141">Připojte se ke svému účtu Azure a seznamu všechna předplatná spojená s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="491a6-141">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="491a6-142">Vyberte odběr, který obsahuje databázi obnovit.</span><span class="sxs-lookup"><span data-stu-id="491a6-142">Select the subscription that contains the database to be restored.</span></span>
4. <span data-ttu-id="491a6-143">Získáte databázi, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="491a6-143">Get the database you want to recover.</span></span>
5. <span data-ttu-id="491a6-144">Vytvořte žádost o obnovení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="491a6-144">Create the recovery request for the database.</span></span>
6. <span data-ttu-id="491a6-145">Zkontrolujte stav databáze geografické obnovení.</span><span class="sxs-lookup"><span data-stu-id="491a6-145">Verify the status of the geo-restored database.</span></span>

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="491a6-146">Konfigurace databáze po dokončení obnovení najdete v tématu [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="491a6-146">To configure your database after the restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

<span data-ttu-id="491a6-147">Pokud zdrojové databáze je povolené šifrování TDE, budou obnovené databáze povolené šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="491a6-147">The recovered database will be TDE-enabled if the source database is TDE-enabled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="491a6-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="491a6-148">Next steps</span></span>
<span data-ttu-id="491a6-149">Další informace o funkcích kontinuity obchodních edice Azure SQL Database, přečtěte si [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="491a6-149">To learn about the business continuity features of Azure SQL Database editions, please read the [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
