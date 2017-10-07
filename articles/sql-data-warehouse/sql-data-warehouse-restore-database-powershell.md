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
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Obnovit Azure SQL Data Warehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Portál][Portal]
> * [Prostředí PowerShell][PowerShell]
> * [REST][REST]
> 
> 

V tomto článku se dozvíte, jak toorestore Azure SQL datového skladu pomocí prostředí PowerShell.

## <a name="before-you-begin"></a>Než začnete
**Ověření vaší DTU kapacity.** Každý datový sklad SQL je hostitelem serveru SQL (např. myserver.database.windows.net), který má kvóty DTU.  Před obnovením SQL Data Warehouse, ověřte, že hello systému SQL server má dostatek zbývající kvóty DTU pro hello databáze obnovena. toolearn jak toocalculate DTU potřebná nebo toorequest více DTU, najdete v části [žádosti o změnu kvóty DTU][Request a DTU quota change].

### <a name="install-powershell"></a>Instalace PowerShellu
V pořadí toouse prostředí Azure PowerShell s SQL Data Warehouse budete potřebovat tooinstall prostředí Azure PowerShell verze 1.0 nebo vyšší.  Vaše verze můžete zkontrolovat spuštěním **Get-Module - ListAvailable-AzureRM název**.  je možné nainstalovat nejnovější verzi Hello od [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].  Další informace o instalaci nejnovější verze hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell][How tooinstall and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Obnovit databázi active nebo pozastavena
toorestore databáze ze snímku použít hello [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny prostředí PowerShell.

1. Otevřete Windows PowerShell.
2. Seznam všech odběrů hello spojené s vaším účtem a připojit tooyour účet Azure.
3. Vyberte předplatné hello, který obsahuje toobe hello databáze obnovena.
4. Seznam hello obnovit body pro databázi hello.
5. Vyberte bod obnovení hello potřeby pomocí hello RestorePointCreationDate.
6. Obnovení bodu obnovení toohello potřeby hello databáze.
7. Ověřte, že hello obnovit databáze je online.

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
> Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze
toorestore odstraněnou databázi, použijte hello [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny.

1. Otevřete Windows PowerShell.
2. Seznam všech odběrů hello spojené s vaším účtem a připojit tooyour účet Azure.
3. Vyberte předplatné hello, který obsahuje toobe hello odstranit databázi obnovit.
4. Získáte hello konkrétní odstranit databázi.
5. Hello odstranit databázi obnovte.
6. Ověřte, že hello obnovit databáze je online.

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
> Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Obnovení z Azure geografické oblasti
toorecover databázi, použijte hello [obnovení-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] rutiny.

1. Otevřete Windows PowerShell.
2. Seznam všech odběrů hello spojené s vaším účtem a připojit tooyour účet Azure.
3. Vyberte předplatné hello, který obsahuje toobe hello databáze obnovena.
4. Získáte požadovaná databáze hello toorecover.
5. Vytvořte žádost o obnovení hello hello databáze.
6. Zkontrolujte stav hello hello geografické obnovení databáze.

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
> v tématu vaše databáze po dokončení obnovení hello tooconfigure [nakonfigurovat databázi po obnovení][Configure your database after recovery].
> 
> 

Hello obnovené databáze bude povolené šifrování TDE Pokud hello zdrojové databáze je povolené šifrování TDE.

## <a name="next-steps"></a>Další kroky
toolearn o hello firmy kontinuitu podnikových procesů jednotlivých edice Azure SQL Database, přečtěte si prosím hello [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].

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
