---
title: aaaImport souboru BACPAC souboru toocreate Azure SQL database | Microsoft Docs
description: "Vytvoření databáze SQL newAzure importováním souboru BACPAC souboru."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/26/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 0d5fc93acf27b79502969fcd6199d11161915b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-a-bacpac-file-tooa-new-azure-sql-database"></a>Import soubor tooa souboru BACPAC novou databázi SQL Azure

Pokud potřebujete tooimport databázi z archivu nebo při migraci z jiné platformy, můžete importovat hello schéma databáze a dat z [souboru BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru. Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující hello metadata a data z databáze systému SQL Server. Soubor souboru BACPAC lze importovat z Azure blob storage (pouze standardní úložiště) nebo z místního úložiště v místní umístění. Rychlost importu hello toomaximize, doporučujeme zadat vyšší úrovně a výkonu úrovní služeb, jako je například P6 a pak vertikálně toodown podle potřeby po úspěšném importu hello. Navíc úroveň kompatibility databáze hello po importu hello je založena na úroveň kompatibility hello hello zdrojové databáze. 

> [!IMPORTANT] 
> Po dokončení migrace tooAzure vaší databáze SQL Database, můžete toooperate hello databáze na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2 hello) nebo na vyšší úrovni. Další informace o hello důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s toocompatibility úrovně.   >

> [!NOTE]
> tooimport souboru BACPAC tooa novou databázi, musíte nejdřív vytvoření logického serveru Azure SQL Database. Kurz ukazuje, jak toomigrate systému SQL Server databáze tooAzure databáze SQL pomocí SQLPackage, najdete v tématu [migraci databáze systému SQL Server](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Import ze souboru BACPAC souboru pomocí portálu Azure

Tento článek obsahuje pokyny pro vytvoření Azure SQL database ze souboru BACPAC souboru je uložen v úložišti objektů blob v Azure pomocí hello [portál Azure](https://portal.azure.com). Import pomocí hello jenom portál Azure podporuje importem souboru souboru BACPAC z Azure blob storage.

hello tooimport databázi pomocí portálu Azure, otevřete hello stránky pro databázi a klikněte na tlačítko **Import** na panelu nástrojů hello. Zadejte hello účtu úložiště a kontejneru a vyberte soubor souboru BACPAC hello chcete tooimport. Vyberte velikost hello hello nové databáze (obvykle hello stejné jako původní) a zadejte přihlašovací údaje systému SQL Server cílového hello.  

   ![Import databáze.](./media/sql-database-import/import.png)

průběh hello toomonitor hello operace importu, otevřete stránku hello hello logický server obsahující hello importované databázi. Posuňte se dolů příliš**operace** a pak klikněte na **importu a exportu** historie.

### <a name="monitor-hello-progress-of-an-import-operation"></a>Monitorování hello průběh operace importu

průběh hello toomonitor hello operace importu, otevřete stránku hello hello logického serveru do které hello se importují databázi naimportována. Posuňte se dolů příliš**operace** a pak klikněte na **importu a exportu** historie.
   
   ![Import](./media/sql-database-import/import-history.png) ![stav importu](./media/sql-database-import/import-status.png)

tooverify hello databáze je na serveru hello za provozu, klikněte na tlačítko **databází SQL** a ověřte nové databáze hello **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Import ze souboru BACPAC souboru pomocí SQLPackage

tooimport SQL databáze pomocí hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) nástroj příkazového řádku najdete v části [importovat parametry a vlastnosti](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). Hello SQLPackage nástroj se dodává s nejnovější verzí hello [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools pro sadu Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), nebo můžete stáhnout nejnovější verzi hello [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) přímo z hello Microsoft služby Stažení softwaru.

Doporučujeme, abyste hello použití hello SQLPackage nástroj pro škálování a výkon ve většině produkční prostředí. SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Viz následující příkaz SQLPackage skriptu příklad, jak hello tooimport hello **AdventureWorks2008R2** databáze z místního úložiště tooan Azure SQL Database logického serveru, nazývá **mynewserver20170403** v tomto příkladu. Tento skript je ukázkou hello vytvoření nové databáze názvem **myMigratedDatabase**, s vrstvy služby **Premium**a cíl služby z **P6**. Tyto hodnoty změňte jako odpovídající tooyour prostředí.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Logický server Azure SQL Database naslouchá na portu 1433. Pokud se pokoušíte tooconnect tooan Azure SQL Database logického serveru z podniková brána firewall, musí být tento port otevřít v hello podniková brána firewall pro připojení je toosuccessfully.
>

Tento příklad ukazuje, jak tooimport a databáze pomocí SqlPackage.exe Universal ověřování služby Active Directory:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Import ze souboru BACPAC souboru pomocí prostředí PowerShell

Použití hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) toosubmit rutiny požadavku toohello databáze import služby Azure SQL Database. V závislosti na velikosti hello vaší databáze hello operace importu může trvat některé toocomplete čas.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

Stav hello toocheck hello žádost o import, použijte hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) rutiny. Obvykle to spuštěna ihned po hello vrátí požadavku **stav: InProgress**. Až se zobrazí **stav: úspěšné** dokončení importu hello.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Jiný příklad skriptu najdete v tématu [Import databáze ze souboru BACPAC souboru](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="next-steps"></a>Další kroky
* toolearn způsobu tooconnect tooand dotaz importované SQL Database, najdete v [připojení tooSQL databáze s SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).
* SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Informace hello celého systému SQL Server databáze procesu migrace, včetně doporučení výkonu, naleznete v [migrovat tooAzure databáze systému SQL Server databáze SQL](sql-database-cloud-migrate.md).



