---
title: Exportovat do souboru BACPAC souboru Azure SQL database | Microsoft Docs
description: "Azure SQL database exportovat do souboru BACPAC souboru pomocí portálu Azure"
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
ms.openlocfilehash: faa567ec615a07da8633629fc98e3454c84a8f5f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a><span data-ttu-id="2f921-103">Exportovat do souboru BACPAC souboru Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="2f921-103">Export an Azure SQL database to a BACPAC file</span></span>

<span data-ttu-id="2f921-104">Pokud budete potřebovat na export databáze pro archivaci nebo pro přesun na jiné platformě, můžete exportovat schéma databáze a dat [souboru BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru.</span><span class="sxs-lookup"><span data-stu-id="2f921-104">When you need to export a database for archiving or for moving to another platform, you can export the database schema and data to a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="2f921-105">Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující metadata a data z databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2f921-105">A BACPAC file is a ZIP file with an extension of BACPAC containing the metadata and data from a SQL Server database.</span></span> <span data-ttu-id="2f921-106">Soubor souboru BACPAC lze uložená v úložišti objektů blob Azure nebo v místním úložišti v místní umístění a později importovat zpět do Azure SQL Database nebo do místní instalace SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="2f921-106">A BACPAC file can be stored in Azure blob storage or in local storage in an on-premises location and later imported back into Azure SQL Database or into a SQL Server on-premises installation.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="2f921-107">Azure SQL Database automatizované exportovat vyřazenou na 1. března 2017.</span><span class="sxs-lookup"><span data-stu-id="2f921-107">Azure SQL Database Automated Export was retired on March 1, 2017.</span></span> <span data-ttu-id="2f921-108">Můžete použít [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md
) nebo [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) pravidelně archivovat SQL databáze pomocí prostředí PowerShell podle plánu, podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="2f921-108">You can use [long-term backup retention](sql-database-long-term-retention.md
) or [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) to periodically archive SQL databases using PowerShell according to a schedule of your choice.</span></span> <span data-ttu-id="2f921-109">Ukázku, stáhněte si [ukázkový skript prostředí PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="2f921-109">For a sample, download the [sample PowerShell script](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) from Github.</span></span>
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a><span data-ttu-id="2f921-110">Aspekty při exportu Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="2f921-110">Considerations when exporting an Azure SQL database</span></span>

* <span data-ttu-id="2f921-111">Pro export být stavu transakční konzistence, je nutné zajistit buď žádné zápisu aktivity dochází při exportu nebo které exportujete z [stavu transakční konzistence kopie](sql-database-copy.md) vaší databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="2f921-111">For an export to be transactionally consistent, you must ensure either that no write activity is occurring during the export, or that you are exporting from a [transactionally consistent copy](sql-database-copy.md) of your Azure SQL database.</span></span>
* <span data-ttu-id="2f921-112">Pokud chcete exportovat do úložiště objektů blob, je maximální velikost souboru BACPAC souboru 200 GB.</span><span class="sxs-lookup"><span data-stu-id="2f921-112">If you are exporting to blob storage, the maximum size of a BACPAC file is 200 GB.</span></span> <span data-ttu-id="2f921-113">K archivaci větší soubor souboru BACPAC, exportovat do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="2f921-113">To archive a larger BACPAC file, export to local storage.</span></span>
* <span data-ttu-id="2f921-114">Export souboru BACPAC souboru na Azure premium storage pomocí metody popsané v tomto článku není podporován.</span><span class="sxs-lookup"><span data-stu-id="2f921-114">Exporting a BACPAC file to Azure premium storage using the methods discussed in this article is not supported.</span></span>
* <span data-ttu-id="2f921-115">Pokud operace exportu z databáze SQL Azure je větší než 20 hodin, může být zrušena.</span><span class="sxs-lookup"><span data-stu-id="2f921-115">If the export operation from Azure SQL Database exceeds 20 hours, it may be canceled.</span></span> <span data-ttu-id="2f921-116">Chcete-li zvýšit výkon při exportu, můžete:</span><span class="sxs-lookup"><span data-stu-id="2f921-116">To increase performance during export, you can:</span></span>
  * <span data-ttu-id="2f921-117">Dočasně zvýšit úroveň vaší služby.</span><span class="sxs-lookup"><span data-stu-id="2f921-117">Temporarily increase your service level.</span></span>
  * <span data-ttu-id="2f921-118">Ukončí všechny čtení a zápisu aktivit během exportu.</span><span class="sxs-lookup"><span data-stu-id="2f921-118">Cease all read and write activity during the export.</span></span>
  * <span data-ttu-id="2f921-119">Použití [clusterovaný index](https://msdn.microsoft.com/library/ms190457.aspx) s nenulové hodnoty pro všechny velké tabulky.</span><span class="sxs-lookup"><span data-stu-id="2f921-119">Use a [clustered index](https://msdn.microsoft.com/library/ms190457.aspx) with non-null values on all large tables.</span></span> <span data-ttu-id="2f921-120">Bez Clusterované indexy exportu může selhat, pokud trvá déle než 6 – 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="2f921-120">Without clustered indexes, an export may fail if it takes longer than 6-12 hours.</span></span> <span data-ttu-id="2f921-121">Je to proto, že služba export potřebuje k dokončení tabulky vyhledávání s cílem při exportu celou tabulku.</span><span class="sxs-lookup"><span data-stu-id="2f921-121">This is because the export service needs to complete a table scan to try to export entire table.</span></span> <span data-ttu-id="2f921-122">Dobrým způsobem, jak určit, pokud vaše tabulky jsou optimalizované pro export je spuštění **DBCC SHOW_STATISTICS** a ujistěte se, že *RANGE_HI_KEY* není null a jeho hodnota má správné distribuční.</span><span class="sxs-lookup"><span data-stu-id="2f921-122">A good way to determine if your tables are optimized for export is to run **DBCC SHOW_STATISTICS** and make sure that the *RANGE_HI_KEY* is not null and its value has good distribution.</span></span> <span data-ttu-id="2f921-123">Podrobnosti najdete v tématu [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f921-123">For details, see [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="2f921-124">BACPACs nejsou určeny k použít pro zálohování a obnovení operací.</span><span class="sxs-lookup"><span data-stu-id="2f921-124">BACPACs are not intended to be used for backup and restore operations.</span></span> <span data-ttu-id="2f921-125">Databáze SQL Azure automaticky vytvoří zálohy pro každé uživatelské databáze.</span><span class="sxs-lookup"><span data-stu-id="2f921-125">Azure SQL Database automatically creates backups for every user database.</span></span> <span data-ttu-id="2f921-126">Podrobnosti najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md) a [zálohy databáze SQL](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="2f921-126">For details, see [Business Continuity Overview](sql-database-business-continuity.md) and [SQL Database backups](sql-database-automated-backups.md).</span></span>  
> 

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a><span data-ttu-id="2f921-127">Exportovat do souboru BACPAC souboru pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2f921-127">Export to a BACPAC file using the Azure portal</span></span>

<span data-ttu-id="2f921-128">Export databáze pomocí [portál Azure](https://portal.azure.com), otevřete stránku pro vaši databázi a klikněte na **exportovat** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="2f921-128">To export a database using the [Azure portal](https://portal.azure.com), open the page for your database and click **Export** on the toolbar.</span></span> <span data-ttu-id="2f921-129">Zadejte název souboru BACPAC souboru, zadejte účet úložiště Azure a kontejner pro export a zadejte přihlašovací údaje pro připojení k databázi zdrojové.</span><span class="sxs-lookup"><span data-stu-id="2f921-129">Specify the BACPAC filename, provide the Azure storage account and container for the export, and provide the credentials to connect to the source database.</span></span>  

![Export databáze.](./media/sql-database-export/database-export.png)

<span data-ttu-id="2f921-131">Pokud chcete sledovat průběh operace exportu, otevřete stránku pro logický server obsahující databáze, která je exportována.</span><span class="sxs-lookup"><span data-stu-id="2f921-131">To monitor the progress of the export operation, open the page for the logical server containing the database being exported.</span></span> <span data-ttu-id="2f921-132">Přejděte dolů k položce **operace** a pak klikněte na **importu a exportu** historie.</span><span class="sxs-lookup"><span data-stu-id="2f921-132">Scroll down to **Operations** and then click **Import/Export** history.</span></span>

<span data-ttu-id="2f921-133">![Export historie](./media/sql-database-export/export-history.png)
![exportovat historie stavu](./media/sql-database-export/export-history2.png)</span><span class="sxs-lookup"><span data-stu-id="2f921-133">![export history](./media/sql-database-export/export-history.png)
![export history status](./media/sql-database-export/export-history2.png)</span></span>

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a><span data-ttu-id="2f921-134">Exportovat do souboru BACPAC souboru pomocí nástroje SQLPackage</span><span class="sxs-lookup"><span data-stu-id="2f921-134">Export to a BACPAC file using the SQLPackage utility</span></span>

<span data-ttu-id="2f921-135">Export databáze SQL pomocí [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) nástroj příkazového řádku najdete v části [exportovat parametry a vlastnosti](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="2f921-135">To export a SQL database using the [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Export parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties).</span></span> <span data-ttu-id="2f921-136">Nástroj SQLPackage se dodává s nejnovější verze [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools pro sadu Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), nebo si můžete stáhnout nejnovější verzi [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) přímo z webu Microsoft download center.</span><span class="sxs-lookup"><span data-stu-id="2f921-136">The SQLPackage utility ships with the latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download the latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from the Microsoft download center.</span></span>

<span data-ttu-id="2f921-137">Doporučujeme použít nástroj SQLPackage pro škálování a výkon ve většině produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f921-137">We recommend the use of the SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="2f921-138">Příspěvek na blogu zákaznického poradního týmu SQL Serveru o migraci pomocí souborů BACPAC najdete v tématu popisujícím [migraci z SQL Serveru do služby SQL Database pomocí souborů BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="2f921-138">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="2f921-139">Tento příklad ukazuje, jak exportovat databázi pomocí SqlPackage.exe Universal ověřování služby Active Directory:</span><span class="sxs-lookup"><span data-stu-id="2f921-139">This example shows how to export a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a><span data-ttu-id="2f921-140">Exportovat do souboru BACPAC souboru pomocí SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="2f921-140">Export to a BACPAC file using SQL Server Management Studio (SSMS)</span></span>

<span data-ttu-id="2f921-141">Nejnovější verze SQL Server Management Studio také poskytují Průvodce exportovat do souboru BACPAC soubor databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="2f921-141">The newest versions of SQL Server Management Studio also provide a wizard to export an Azure SQL Database to a BACPAC file.</span></span> <span data-ttu-id="2f921-142">Najdete v článku [exportovat aplikace na datové vrstvě](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).</span><span class="sxs-lookup"><span data-stu-id="2f921-142">See the [Export a Data-tier Application](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).</span></span>

## <a name="export-to-a-bacpac-file-using-powershell"></a><span data-ttu-id="2f921-143">Exportovat do souboru BACPAC soubor pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f921-143">Export to a BACPAC file using PowerShell</span></span>

<span data-ttu-id="2f921-144">Použití [New-AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) rutiny odeslání žádosti o export databáze ke službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2f921-144">Use the [New-AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet to submit an export database request to the Azure SQL Database service.</span></span> <span data-ttu-id="2f921-145">V závislosti na velikosti vaší databáze operace exportu může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="2f921-145">Depending on the size of your database, the export operation may take some time to complete.</span></span>

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

<span data-ttu-id="2f921-146">Chcete-li zkontrolovat stav žádosti exportu, použijte [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) rutiny.</span><span class="sxs-lookup"><span data-stu-id="2f921-146">To check the status of the export request, use the [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="2f921-147">Spuštění to hned po dokončení žádosti o obvykle vrátí **stav: InProgress**.</span><span class="sxs-lookup"><span data-stu-id="2f921-147">Running this immediately after the request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="2f921-148">Až se zobrazí **stav: úspěšné** dokončení exportu.</span><span class="sxs-lookup"><span data-stu-id="2f921-148">When you see **Status: Succeeded** the export is complete.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2f921-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f921-149">Next steps</span></span>

* <span data-ttu-id="2f921-150">Další informace o dlouhodobé uchovávání záloh zálohy databáze Azure SQL, jako alternativu k exportovány databázi pro účely archivace, najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="2f921-150">To learn about long-term backup retention of an Azure SQL database backup as an alternative to exported a database for archive purposes, see [Long-term backup retention](sql-database-long-term-retention.md).</span></span>
- <span data-ttu-id="2f921-151">Příspěvek na blogu zákaznického poradního týmu SQL Serveru o migraci pomocí souborů BACPAC najdete v tématu popisujícím [migraci z SQL Serveru do služby SQL Database pomocí souborů BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="2f921-151">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="2f921-152">Další informace o import souboru BACPAC k databázi systému SQL Server najdete v tématu [importovat do databáze serveru SQL BACPCAC](https://msdn.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f921-152">To learn about importing a BACPAC to a SQL Server database, see [Import a BACPCAC to a SQL Server database](https://msdn.microsoft.com/library/hh710052.aspx).</span></span>
* <span data-ttu-id="2f921-153">Další informace o export souboru BACPAC z databáze SQL serveru najdete v tématu [exportovat aplikace na datové vrstvě](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) a [migrovat svoji první databázi](sql-database-migrate-your-sql-server-database.md).</span><span class="sxs-lookup"><span data-stu-id="2f921-153">To learn about exporting a BACPAC from a SQL Server database, see [Export a Data-tier Application](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) and [Migrate your first database](sql-database-migrate-your-sql-server-database.md).</span></span>
* <span data-ttu-id="2f921-154">Chcete-li exportovat ze serveru SQL Server jako prelude migrace do Azure SQL Database, najdete v části [migrovat do databáze SQL serveru do Azure SQL Database](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="2f921-154">If you are exporting from SQL Server as a prelude to migration to Azure SQL Database, see [Migrate a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md).</span></span>
