---
title: aaaExport soubor Azure SQL database tooa souboru BACPAC | Microsoft Docs
description: "Exportujte soubor Azure SQL database tooa souboru BACPAC musí být pomocí hello portálu Azure"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: cb3b4227318e0fd2114529c86c9792615fe7fd1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-sql-database-tooa-bacpac-file"></a>Exportujte soubor souboru BACPAC tooa databáze Azure SQL

Pokud budete potřebovat tooexport databáze pro archivaci nebo pro přesunutí tooanother platformy, můžete exportovat hello databáze schéma a data tooa [souboru BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru. Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující hello metadata a data z databáze systému SQL Server. Soubor souboru BACPAC lze uložená v úložišti objektů blob Azure nebo v místním úložišti v místní umístění a později importovat zpět do Azure SQL Database nebo do místní instalace SQL serveru. 

> [!IMPORTANT] 
> Azure SQL Database automatizované exportovat vyřazenou na 1. března 2017. Můžete použít [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md
) nebo [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) tooperiodically archivu SQL databáze pomocí prostředí PowerShell podle plánu tooa podle svého výběru. Ukázku, stáhněte si hello [ukázkový skript prostředí PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) z Githubu.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Aspekty při exportu Azure SQL database

* O export toobe stavu transakční konzistence, je nutné zajistit buď, že žádná aktivita zápisu dochází při exportu hello nebo že, můžete exportovat z [stavu transakční konzistence kopie](sql-database-copy.md) vaší databáze Azure SQL.
* Pokud exportujete tooblob úložiště, je maximální velikost souboru souboru BACPAC hello 200 GB. tooarchive soubor větší souboru BACPAC exportovat toolocal úložiště.
* Export souboru BACPAC souboru tooAzure premium úložiště pomocí hello metody popsané v tomto článku není podporován.
* Pokud operace exportu hello z databáze SQL Azure je větší než 20 hodin, může být zrušena. tooincrease výkonu během exportu, můžete postupovat následovně:
  * Dočasně zvýšit úroveň vaší služby.
  * Ukončí všechny čtení a zápisu aktivit během exportu hello.
  * Použití [clusterovaný index](https://msdn.microsoft.com/library/ms190457.aspx) s nenulové hodnoty pro všechny velké tabulky. Bez Clusterované indexy exportu může selhat, pokud trvá déle než 6 – 12 hodin. Je to proto, že služba export hello musí toocomplete kontroly tootry tooexport celou tabulku. Toodetermine vhodný způsob, pokud vaše tabulky jsou optimalizované pro export je toorun **DBCC SHOW_STATISTICS** a ujistěte se, že hello *RANGE_HI_KEY* není null a jeho hodnota má správné distribuční. Podrobnosti najdete v tématu [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs nejsou určený toobe používaných pro operace zálohování a obnovení. Databáze SQL Azure automaticky vytvoří zálohy pro každé uživatelské databáze. Podrobnosti najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md) a [zálohy databáze SQL](sql-database-automated-backups.md).  
> 

## <a name="export-tooa-bacpac-file-using-hello-azure-portal"></a>Exportovat soubor souboru BACPAC tooa pomocí hello portálu Azure

hello tooexport databázi pomocí [portál Azure](https://portal.azure.com), otevřete stránku hello pro vaši databázi a klikněte na **exportovat** na panelu nástrojů hello. Zadejte název souboru souboru BACPAC hello, zadejte účet úložiště Azure hello a kontejner pro hello export a poskytnout hello pověření tooconnect toohello zdrojové databáze.  

![Export databáze.](./media/sql-database-export/database-export.png)

průběh hello toomonitor hello operace exportu, otevřete stránku hello hello logický server obsahující hello databáze která je exportována. Posuňte se dolů příliš**operace** a pak klikněte na **importu a exportu** historie.

![Export historie](./media/sql-database-export/export-history.png)
![exportovat historie stavu](./media/sql-database-export/export-history2.png)

## <a name="export-tooa-bacpac-file-using-hello-sqlpackage-utility"></a>Exportovat soubor souboru BACPAC tooa nástroj SQLPackage hello

tooexport SQL databáze pomocí hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) nástroj příkazového řádku najdete v části [exportovat parametry a vlastnosti](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). Hello SQLPackage nástroj se dodává s nejnovější verzí hello [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools pro sadu Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), nebo můžete stáhnout nejnovější verzi hello [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) přímo z hello Microsoft služby Stažení softwaru.

Doporučujeme, abyste hello použití hello SQLPackage nástroj pro škálování a výkon ve většině produkční prostředí. SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Tento příklad ukazuje, jak tooexport a databáze pomocí SqlPackage.exe Universal ověřování služby Active Directory:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-tooa-bacpac-file-using-sql-server-management-studio-ssms"></a>Exportovat soubor souboru BACPAC tooa pomocí SQL Server Management Studio (SSMS)

Hello nejnovější verze SQL Server Management Studio také poskytují tooexport Průvodce soubor souboru BACPAC tooa Azure SQL Database. V tématu hello [exportovat aplikace na datové vrstvě](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-tooa-bacpac-file-using-powershell"></a>Exportovat soubor souboru BACPAC tooa pomocí prostředí PowerShell

Použití hello [New-AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) toosubmit rutiny požadavku toohello databáze exportu služby Azure SQL Database. V závislosti na velikosti hello vaší databáze může trvat operace exportu hello některé toocomplete čas.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

Stav hello toocheck hello požadavek export, použijte hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) rutiny. Obvykle to spuštěna ihned po hello vrátí požadavku **stav: InProgress**. Až se zobrazí **stav: úspěšné** hello export je dokončena.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Další kroky

* toolearn o dlouhodobé uchovávání záloh zálohy databáze Azure SQL jako alternativní tooexported databázi pro účely archivace, najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).
- SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* toolearn o import souboru BACPAC tooa databáze SQL serveru, najdete v části [importovat do databáze serveru SQL tooa BACPCAC](https://msdn.microsoft.com/library/hh710052.aspx).
* toolearn o export souboru BACPAC z databáze systému SQL Server, najdete v části [exportovat aplikace na datové vrstvě](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) a [migrovat svoji první databázi](sql-database-migrate-your-sql-server-database.md).
* Chcete-li exportovat ze serveru SQL Server jako prelude toomigration tooAzure SQL Database, najdete v části [migrovat tooAzure databáze systému SQL Server databáze SQL](sql-database-cloud-migrate.md).
