---
title: "Importovat soubor souboru BACPAC k vytvoření Azure SQL database | Microsoft Docs"
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
ms.workload: Active
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 34dee9511822acec46ba4854729939b84f3c06c6
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Importovat soubor souboru BACPAC pro novou databázi SQL Azure

Pokud je třeba importovat databázi z archivu nebo při migraci z jiné platformy, můžete importovat schéma databáze a dat z [souboru BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru. Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující metadata a data z databáze systému SQL Server. Soubor souboru BACPAC lze importovat z Azure blob storage (pouze standardní úložiště) nebo z místního úložiště v místní umístění. Pokud chcete maximalizovat rychlost import, doporučujeme, abyste zadejte vyšší úrovně a výkonu úrovní služeb, jako je například P6 a pak škálovat podle potřeby až po úspěšném importu. Navíc úroveň kompatibility databáze po dokončení importu je založena na úroveň kompatibility zdrojové databáze. 

> [!IMPORTANT] 
> Po dokončení migrace databáze do Azure SQL Database, můžete pracovat databázi na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2) nebo na vyšší úrovni. Další informace o důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s úrovní kompatibility.   >

> [!NOTE]
> K importu souboru BACPAC pro novou databázi, musíte nejdřív vytvoření logického serveru Azure SQL Database. Kurz ukazuje, jak migrovat databázi systému SQL Server do Azure SQL Database pomocí SQLPackage, najdete v části [migraci databáze systému SQL Server](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Import ze souboru BACPAC souboru pomocí portálu Azure

Tento článek obsahuje pokyny pro vytvoření Azure SQL database ze souboru BACPAC souboru je uložen v pomocí úložiště objektů blob v Azure [portál Azure](https://portal.azure.com). Import pomocí webu Azure portal podporuje importem souboru souboru BACPAC z Azure blob storage.

Pokud chcete importovat databáze pomocí portálu Azure, otevřete stránku pro server pro přidružení k databázi na a pak klikněte na tlačítko **importovat** na panelu nástrojů. Zadejte účet úložiště a kontejneru a vyberte soubor souboru BACPAC, který chcete importovat. Vyberte velikost novou databázi (obvykle stejné jako původní) a zadejte cíl pověření systému SQL Server.  

   ![Import databáze.](./media/sql-database-import/import.png)

Pokud chcete sledovat průběh operace importu, otevřete stránku pro logický server obsahující databáze importovaných. Přejděte dolů k položce **operace** a pak klikněte na **importu a exportu** historie.

### <a name="monitor-the-progress-of-an-import-operation"></a>Sledujte průběh operace importu

Pokud chcete sledovat průběh operace importu, otevřete stránku pro logický server do které se importují databázi naimportována. Přejděte dolů k položce **operace** a pak klikněte na **importu a exportu** historie.
   
   ![Import](./media/sql-database-import/import-history.png) ![stav importu](./media/sql-database-import/import-status.png)

Chcete-li ověřit, databáze je za provozu na serveru, klikněte na tlačítko **databází SQL** a ověřte je nové databáze **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Import ze souboru BACPAC souboru pomocí SQLPackage

Import pomocí databáze SQL [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) nástroj příkazového řádku najdete v části [importovat parametry a vlastnosti](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). Nástroj SQLPackage se dodává s nejnovější verze [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools pro sadu Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), nebo si můžete stáhnout nejnovější verzi [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) přímo z webu Microsoft download center.

Doporučujeme použít nástroj SQLPackage pro škálování a výkon ve většině produkční prostředí. Příspěvek na blogu zákaznického poradního týmu SQL Serveru o migraci pomocí souborů BACPAC najdete v tématu popisujícím [migraci z SQL Serveru do služby SQL Database pomocí souborů BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Viz následující příkaz SQLPackage příklad skriptu pro import **AdventureWorks2008R2** databázi z místního úložiště do logického serveru Azure SQL Database, nazývá **mynewserver20170403** v tomto příkladu. Tento skript je ukázkou, vytvoření nové databáze názvem **myMigratedDatabase**, s vrstvy služby **Premium**a cíl služby z **P6**. Změňte tyto hodnoty v závislosti na vašem prostředí.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Logický server Azure SQL Database naslouchá na portu 1433. Pokud se pokoušíte připojovat k logickému serveru Azure SQL Database z oblasti za podnikovou bránou firewall, je třeba v podnikové brány firewall otevřít tento port, abyste se mohli úspěšně připojit.
>

Tento příklad ukazuje postup importování databáze pomocí SqlPackage.exe Universal ověřování služby Active Directory:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Import ze souboru BACPAC souboru pomocí prostředí PowerShell

Použití [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) rutiny import databáze žádost o službu Azure SQL Database. V závislosti na velikosti vaší databáze operace importu může trvat delší dobu.

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

Chcete-li zkontrolovat stav pro žádost o import, použijte [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) rutiny. Spuštění to hned po dokončení žádosti o obvykle vrátí **stav: InProgress**. Až se zobrazí **stav: úspěšné** dokončení importu.

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
* Informace o tom, k připojení a dotazování importované SQL Database, najdete v části [připojit k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).
* Příspěvek na blogu zákaznického poradního týmu SQL Serveru o migraci pomocí souborů BACPAC najdete v tématu popisujícím [migraci z SQL Serveru do služby SQL Database pomocí souborů BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Informace celého systému SQL Server databáze procesu migrace, včetně doporučení výkonu, naleznete v [migrovat do databáze SQL serveru do Azure SQL Database](sql-database-cloud-migrate.md).



