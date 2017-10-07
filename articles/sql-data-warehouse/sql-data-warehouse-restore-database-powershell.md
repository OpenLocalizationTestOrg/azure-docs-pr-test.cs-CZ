---
title: aaaRestore Azure SQL Data Warehouse (PowerShell) | Microsoft Docs
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
ms.openlocfilehash: aa29a315080b1ed477cc6a051ce15a3202630cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="ba6b8-103">Obnovit Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="ba6b8-103">Restore an Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="ba6b8-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="ba6b8-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="ba6b8-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="ba6b8-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="ba6b8-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="ba6b8-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="ba6b8-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="ba6b8-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="ba6b8-108">V tomto článku se dozvíte, jak toorestore Azure SQL datového skladu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-108">In this article you will learn how toorestore an Azure SQL Data Warehouse using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ba6b8-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ba6b8-109">Before you begin</span></span>
<span data-ttu-id="ba6b8-110">**Ověření vaší DTU kapacity.**</span><span class="sxs-lookup"><span data-stu-id="ba6b8-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="ba6b8-111">Každý datový sklad SQL je hostitelem serveru SQL (např. myserver.database.windows.net), který má kvóty DTU.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="ba6b8-112">Před obnovením SQL Data Warehouse, ověřte, že hello systému SQL server má dostatek zbývající kvóty DTU pro hello databáze obnovena.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-112">Before you can restore a SQL Data Warehouse, verify that hello your SQL server has enough remaining DTU quota for hello database being restored.</span></span> <span data-ttu-id="ba6b8-113">toolearn jak toocalculate DTU potřebná nebo toorequest více DTU, najdete v části [žádosti o změnu kvóty DTU][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-113">toolearn how toocalculate DTU needed or toorequest more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="ba6b8-114">Instalace PowerShellu</span><span class="sxs-lookup"><span data-stu-id="ba6b8-114">Install PowerShell</span></span>
<span data-ttu-id="ba6b8-115">V pořadí toouse prostředí Azure PowerShell s SQL Data Warehouse budete potřebovat tooinstall prostředí Azure PowerShell verze 1.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-115">In order toouse Azure PowerShell with SQL Data Warehouse, you will need tooinstall Azure PowerShell version 1.0 or greater.</span></span>  <span data-ttu-id="ba6b8-116">Vaše verze můžete zkontrolovat spuštěním **Get-Module - ListAvailable-AzureRM název**.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-116">You can check your version by running **Get-Module -ListAvailable -Name AzureRM**.</span></span>  <span data-ttu-id="ba6b8-117">je možné nainstalovat nejnovější verzi Hello od [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-117">hello latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="ba6b8-118">Další informace o instalaci nejnovější verze hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-118">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="ba6b8-119">Obnovit databázi active nebo pozastavena</span><span class="sxs-lookup"><span data-stu-id="ba6b8-119">Restore an active or paused database</span></span>
<span data-ttu-id="ba6b8-120">toorestore databáze ze snímku použít hello [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-120">toorestore a database from a snapshot use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet.</span></span>

1. <span data-ttu-id="ba6b8-121">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-121">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="ba6b8-122">Seznam všech odběrů hello spojené s vaším účtem a připojit tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-122">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="ba6b8-123">Vyberte předplatné hello, který obsahuje toobe hello databáze obnovena.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-123">Select hello subscription that contains hello database toobe restored.</span></span>
4. <span data-ttu-id="ba6b8-124">Seznam hello obnovit body pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-124">List hello restore points for hello database.</span></span>
5. <span data-ttu-id="ba6b8-125">Vyberte bod obnovení hello potřeby pomocí hello RestorePointCreationDate.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-125">Pick hello desired restore point using hello RestorePointCreationDate.</span></span>
6. <span data-ttu-id="ba6b8-126">Obnovení bodu obnovení toohello potřeby hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-126">Restore hello database toohello desired restore point.</span></span>
7. <span data-ttu-id="ba6b8-127">Ověřte, že hello obnovit databáze je online.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-127">Verify that hello restored database is online.</span></span>

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List hello last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get hello specific database toorestore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify hello status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> <span data-ttu-id="ba6b8-128">Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-128">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="ba6b8-129">Obnovení odstraněné databáze</span><span class="sxs-lookup"><span data-stu-id="ba6b8-129">Restore a deleted database</span></span>
<span data-ttu-id="ba6b8-130">toorestore odstraněnou databázi, použijte hello [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-130">toorestore a deleted database, use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="ba6b8-131">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-131">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="ba6b8-132">Seznam všech odběrů hello spojené s vaším účtem a připojit tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-132">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="ba6b8-133">Vyberte předplatné hello, který obsahuje toobe hello odstranit databázi obnovit.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-133">Select hello subscription that contains hello deleted database toobe restored.</span></span>
4. <span data-ttu-id="ba6b8-134">Získáte hello konkrétní odstranit databázi.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-134">Get hello specific deleted database.</span></span>
5. <span data-ttu-id="ba6b8-135">Hello odstranit databázi obnovte.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-135">Restore hello deleted database.</span></span>
6. <span data-ttu-id="ba6b8-136">Ověřte, že hello obnovit databáze je online.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-136">Verify that hello restored database is online.</span></span>

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get hello deleted database toorestore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify hello status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="ba6b8-137">Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-137">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a><span data-ttu-id="ba6b8-138">Obnovení z Azure geografické oblasti</span><span class="sxs-lookup"><span data-stu-id="ba6b8-138">Restore from an Azure geographical region</span></span>
<span data-ttu-id="ba6b8-139">toorecover databázi, použijte hello [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-139">toorecover a database, use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="ba6b8-140">Otevřete Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-140">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="ba6b8-141">Seznam všech odběrů hello spojené s vaším účtem a připojit tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-141">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="ba6b8-142">Vyberte předplatné hello, který obsahuje toobe hello databáze obnovena.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-142">Select hello subscription that contains hello database toobe restored.</span></span>
4. <span data-ttu-id="ba6b8-143">Získáte požadovaná databáze hello toorecover.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-143">Get hello database you want toorecover.</span></span>
5. <span data-ttu-id="ba6b8-144">Vytvořte žádost o obnovení hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-144">Create hello recovery request for hello database.</span></span>
6. <span data-ttu-id="ba6b8-145">Zkontrolujte stav hello hello geografické obnovení databáze.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-145">Verify hello status of hello geo-restored database.</span></span>

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get hello database you want toorecover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that hello geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="ba6b8-146">v tématu vaše databáze po dokončení obnovení hello tooconfigure [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-146">tooconfigure your database after hello restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

<span data-ttu-id="ba6b8-147">Hello obnovené databáze bude povolené šifrování TDE Pokud hello zdrojové databáze je povolené šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="ba6b8-147">hello recovered database will be TDE-enabled if hello source database is TDE-enabled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba6b8-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba6b8-148">Next steps</span></span>
<span data-ttu-id="ba6b8-149">toolearn o hello firmy kontinuitu podnikových procesů jednotlivých edice Azure SQL Database, přečtěte si prosím hello [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="ba6b8-149">toolearn about hello business continuity features of Azure SQL Database editions, please read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
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
