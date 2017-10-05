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
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 285e17ed6d0ce700cb518864df7a3b5f5e55bee5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a><span data-ttu-id="7e078-103">Importovat soubor souboru BACPAC pro novou databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="7e078-103">Import a BACPAC file to a new Azure SQL Database</span></span>

<span data-ttu-id="7e078-104">Pokud je třeba importovat databázi z archivu nebo při migraci z jiné platformy, můžete importovat schéma databáze a dat z [souboru BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru.</span><span class="sxs-lookup"><span data-stu-id="7e078-104">When you need to import a database from an archive or when migrating from another platform, you can import the database schema and data from a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="7e078-105">Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující metadata a data z databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7e078-105">A BACPAC file is a ZIP file with an extension of BACPAC containing the metadata and data from a SQL Server database.</span></span> <span data-ttu-id="7e078-106">Soubor souboru BACPAC lze importovat z Azure blob storage (pouze standardní úložiště) nebo z místního úložiště v místní umístění.</span><span class="sxs-lookup"><span data-stu-id="7e078-106">A BACPAC file can be imported from Azure blob storage (standard storage only) or from local storage in an on-premises location.</span></span> <span data-ttu-id="7e078-107">Pokud chcete maximalizovat rychlost import, doporučujeme, abyste zadejte vyšší úrovně a výkonu úrovní služeb, jako je například P6 a pak škálovat podle potřeby až po úspěšném importu.</span><span class="sxs-lookup"><span data-stu-id="7e078-107">To maximize the import speed, we recommend that you specify a higher service tier and performance level, such as a P6, and then scale to down as appropriate after the import is successful.</span></span> <span data-ttu-id="7e078-108">Navíc úroveň kompatibility databáze po dokončení importu je založena na úroveň kompatibility zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="7e078-108">Also, the database compatibility level after the import is based on the compatibility level of the source database.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="7e078-109">Po dokončení migrace databáze do Azure SQL Database, můžete pracovat databázi na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2) nebo na vyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="7e078-109">After you migrate your database to Azure SQL Database, you can choose to operate the database at its current compatibility level (level 100 for the AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="7e078-110">Další informace o důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="7e078-110">For more information on the implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="7e078-111">Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s úrovní kompatibility.</span><span class="sxs-lookup"><span data-stu-id="7e078-111">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related to compatibility levels.</span></span>   >

> [!NOTE]
> <span data-ttu-id="7e078-112">K importu souboru BACPAC pro novou databázi, musíte nejdřív vytvoření logického serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7e078-112">To import a BACPAC to a new database, you must first create an Azure SQL Database logical server.</span></span> <span data-ttu-id="7e078-113">Kurz ukazuje, jak migrovat databázi systému SQL Server do Azure SQL Database pomocí SQLPackage, najdete v části [migraci databáze systému SQL Server](sql-database-migrate-your-sql-server-database.md)</span><span class="sxs-lookup"><span data-stu-id="7e078-113">For a tutorial showing you how to migrate a SQL Server database to Azure SQL Database using SQLPackage, see [Migrate a SQL Server Database](sql-database-migrate-your-sql-server-database.md)</span></span>
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a><span data-ttu-id="7e078-114">Import ze souboru BACPAC souboru pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7e078-114">Import from a BACPAC file using Azure portal</span></span>

<span data-ttu-id="7e078-115">Tento článek obsahuje pokyny pro vytvoření Azure SQL database ze souboru BACPAC souboru je uložen v pomocí úložiště objektů blob v Azure [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e078-115">This article provides directions for creating an Azure SQL database from a BACPAC file stored in Azure blob storage using the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7e078-116">Import pomocí webu Azure portal podporuje importem souboru souboru BACPAC z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="7e078-116">Import using the Azure portal only supports importing a BACPAC file from Azure blob storage.</span></span>

<span data-ttu-id="7e078-117">Chcete-li importovat databázi pomocí portálu Azure, otevřete stránku pro vaši databázi a klikněte na **importovat** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="7e078-117">To import a database using the Azure portal, open the page for your database and click **Import** on the toolbar.</span></span> <span data-ttu-id="7e078-118">Zadejte účet úložiště a kontejneru a vyberte soubor souboru BACPAC, který chcete importovat.</span><span class="sxs-lookup"><span data-stu-id="7e078-118">Specify the storage account and container, and select the BACPAC file you want to import.</span></span> <span data-ttu-id="7e078-119">Vyberte velikost novou databázi (obvykle stejné jako původní) a zadejte cíl pověření systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7e078-119">Select the size of the new database (usually the same as origin) and provide the destination SQL Server credentials.</span></span>  

   ![Import databáze.](./media/sql-database-import/import.png)

<span data-ttu-id="7e078-121">Pokud chcete sledovat průběh operace importu, otevřete stránku pro logický server obsahující databáze importovaných.</span><span class="sxs-lookup"><span data-stu-id="7e078-121">To monitor the progress of the import operation, open the page for the logical server containing the database being imported.</span></span> <span data-ttu-id="7e078-122">Přejděte dolů k položce **operace** a pak klikněte na **importu a exportu** historie.</span><span class="sxs-lookup"><span data-stu-id="7e078-122">Scroll down to **Operations** and then click **Import/Export** history.</span></span>

### <a name="monitor-the-progress-of-an-import-operation"></a><span data-ttu-id="7e078-123">Sledujte průběh operace importu</span><span class="sxs-lookup"><span data-stu-id="7e078-123">Monitor the progress of an import operation</span></span>

<span data-ttu-id="7e078-124">Pokud chcete sledovat průběh operace importu, otevřete stránku pro logický server do které se importují databázi naimportována.</span><span class="sxs-lookup"><span data-stu-id="7e078-124">To monitor the progress of the import operation, open the page for the logical server into which the database is being imported imported.</span></span> <span data-ttu-id="7e078-125">Přejděte dolů k položce **operace** a pak klikněte na **importu a exportu** historie.</span><span class="sxs-lookup"><span data-stu-id="7e078-125">Scroll down to **Operations** and then click **Import/Export** history.</span></span>
   
   <span data-ttu-id="7e078-126">![Import](./media/sql-database-import/import-history.png) ![stav importu](./media/sql-database-import/import-status.png)</span><span class="sxs-lookup"><span data-stu-id="7e078-126">![import](./media/sql-database-import/import-history.png) ![import status](./media/sql-database-import/import-status.png)</span></span>

<span data-ttu-id="7e078-127">Chcete-li ověřit, databáze je za provozu na serveru, klikněte na tlačítko **databází SQL** a ověřte je nové databáze **Online**.</span><span class="sxs-lookup"><span data-stu-id="7e078-127">To verify the database is live on the server, click **SQL databases** and verify the new database is **Online**.</span></span>

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a><span data-ttu-id="7e078-128">Import ze souboru BACPAC souboru pomocí SQLPackage</span><span class="sxs-lookup"><span data-stu-id="7e078-128">Import from a BACPAC file using SQLPackage</span></span>

<span data-ttu-id="7e078-129">Import pomocí databáze SQL [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) nástroj příkazového řádku najdete v části [importovat parametry a vlastnosti](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="7e078-129">To import a SQL database using the [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Import parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span></span> <span data-ttu-id="7e078-130">Nástroj SQLPackage se dodává s nejnovější verze [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools pro sadu Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), nebo si můžete stáhnout nejnovější verzi [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) přímo z webu Microsoft download center.</span><span class="sxs-lookup"><span data-stu-id="7e078-130">The SQLPackage utility ships with the latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download the latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from the Microsoft download center.</span></span>

<span data-ttu-id="7e078-131">Doporučujeme použít nástroj SQLPackage pro škálování a výkon ve většině produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="7e078-131">We recommend the use of the SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="7e078-132">Příspěvek na blogu zákaznického poradního týmu SQL Serveru o migraci pomocí souborů BACPAC najdete v tématu popisujícím [migraci z SQL Serveru do služby SQL Database pomocí souborů BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="7e078-132">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="7e078-133">Viz následující příkaz SQLPackage příklad skriptu pro import **AdventureWorks2008R2** databázi z místního úložiště do logického serveru Azure SQL Database, nazývá **mynewserver20170403** v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="7e078-133">See the following SQLPackage command for a script example for how to import the **AdventureWorks2008R2** database from local storage to an Azure SQL Database logical server, called **mynewserver20170403** in this example.</span></span> <span data-ttu-id="7e078-134">Tento skript je ukázkou, vytvoření nové databáze názvem **myMigratedDatabase**, s vrstvy služby **Premium**a cíl služby z **P6**.</span><span class="sxs-lookup"><span data-stu-id="7e078-134">This script shows the creation of a new database called **myMigratedDatabase**, with a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="7e078-135">Změňte tyto hodnoty v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="7e078-135">Change these values as appropriate to your environment.</span></span>

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="7e078-137">Logický server Azure SQL Database naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="7e078-137">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="7e078-138">Pokud se pokoušíte připojovat k logickému serveru Azure SQL Database z oblasti za podnikovou bránou firewall, je třeba v podnikové brány firewall otevřít tento port, abyste se mohli úspěšně připojit.</span><span class="sxs-lookup"><span data-stu-id="7e078-138">If you are attempting to connect to an Azure SQL Database logical server from within a corporate firewall, this port must be open in the corporate firewall for you to successfully connect.</span></span>
>

<span data-ttu-id="7e078-139">Tento příklad ukazuje postup importování databáze pomocí SqlPackage.exe Universal ověřování služby Active Directory:</span><span class="sxs-lookup"><span data-stu-id="7e078-139">This example shows how to import a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a><span data-ttu-id="7e078-140">Import ze souboru BACPAC souboru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e078-140">Import from a BACPAC file using PowerShell</span></span>

<span data-ttu-id="7e078-141">Použití [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) rutiny import databáze žádost o službu Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7e078-141">Use the [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet to submit an import database request to the Azure SQL Database service.</span></span> <span data-ttu-id="7e078-142">V závislosti na velikosti vaší databáze operace importu může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="7e078-142">Depending on the size of your database, the import operation may take some time to complete.</span></span>

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

<span data-ttu-id="7e078-143">Chcete-li zkontrolovat stav pro žádost o import, použijte [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7e078-143">To check the status of the import request, use the [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="7e078-144">Spuštění to hned po dokončení žádosti o obvykle vrátí **stav: InProgress**.</span><span class="sxs-lookup"><span data-stu-id="7e078-144">Running this immediately after the request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="7e078-145">Až se zobrazí **stav: úspěšné** dokončení importu.</span><span class="sxs-lookup"><span data-stu-id="7e078-145">When you see **Status: Succeeded** the import is complete.</span></span>

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
<span data-ttu-id="7e078-146">Jiný příklad skriptu najdete v tématu [Import databáze ze souboru BACPAC souboru](scripts/sql-database-import-from-bacpac-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7e078-146">For another script example, see [Import a database from a BACPAC file](scripts/sql-database-import-from-bacpac-powershell.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e078-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e078-147">Next steps</span></span>
* <span data-ttu-id="7e078-148">Informace o tom, k připojení a dotazování importované SQL Database, najdete v části [připojit k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="7e078-148">To learn how to connect to and query an imported SQL Database, see [Connect to SQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>
* <span data-ttu-id="7e078-149">Příspěvek na blogu zákaznického poradního týmu SQL Serveru o migraci pomocí souborů BACPAC najdete v tématu popisujícím [migraci z SQL Serveru do služby SQL Database pomocí souborů BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="7e078-149">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="7e078-150">Informace celého systému SQL Server databáze procesu migrace, včetně doporučení výkonu, naleznete v [migrovat do databáze SQL serveru do Azure SQL Database](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="7e078-150">For a discussion of the entire SQL Server database migration process, including performance recommendations, see [Migrate a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md).</span></span>



