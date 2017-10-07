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
# <a name="import-a-bacpac-file-tooa-new-azure-sql-database"></a><span data-ttu-id="56668-103">Import soubor tooa souboru BACPAC novou databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="56668-103">Import a BACPAC file tooa new Azure SQL Database</span></span>

<span data-ttu-id="56668-104">Pokud potřebujete tooimport databázi z archivu nebo při migraci z jiné platformy, můžete importovat hello schéma databáze a dat z [souboru BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) souboru.</span><span class="sxs-lookup"><span data-stu-id="56668-104">When you need tooimport a database from an archive or when migrating from another platform, you can import hello database schema and data from a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="56668-105">Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující hello metadata a data z databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="56668-105">A BACPAC file is a ZIP file with an extension of BACPAC containing hello metadata and data from a SQL Server database.</span></span> <span data-ttu-id="56668-106">Soubor souboru BACPAC lze importovat z Azure blob storage (pouze standardní úložiště) nebo z místního úložiště v místní umístění.</span><span class="sxs-lookup"><span data-stu-id="56668-106">A BACPAC file can be imported from Azure blob storage (standard storage only) or from local storage in an on-premises location.</span></span> <span data-ttu-id="56668-107">Rychlost importu hello toomaximize, doporučujeme zadat vyšší úrovně a výkonu úrovní služeb, jako je například P6 a pak vertikálně toodown podle potřeby po úspěšném importu hello.</span><span class="sxs-lookup"><span data-stu-id="56668-107">toomaximize hello import speed, we recommend that you specify a higher service tier and performance level, such as a P6, and then scale toodown as appropriate after hello import is successful.</span></span> <span data-ttu-id="56668-108">Navíc úroveň kompatibility databáze hello po importu hello je založena na úroveň kompatibility hello hello zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="56668-108">Also, hello database compatibility level after hello import is based on hello compatibility level of hello source database.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="56668-109">Po dokončení migrace tooAzure vaší databáze SQL Database, můžete toooperate hello databáze na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2 hello) nebo na vyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="56668-109">After you migrate your database tooAzure SQL Database, you can choose toooperate hello database at its current compatibility level (level 100 for hello AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="56668-110">Další informace o hello důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="56668-110">For more information on hello implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="56668-111">Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s toocompatibility úrovně.</span><span class="sxs-lookup"><span data-stu-id="56668-111">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related toocompatibility levels.</span></span>   >

> [!NOTE]
> <span data-ttu-id="56668-112">tooimport souboru BACPAC tooa novou databázi, musíte nejdřív vytvoření logického serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56668-112">tooimport a BACPAC tooa new database, you must first create an Azure SQL Database logical server.</span></span> <span data-ttu-id="56668-113">Kurz ukazuje, jak toomigrate systému SQL Server databáze tooAzure databáze SQL pomocí SQLPackage, najdete v tématu [migraci databáze systému SQL Server](sql-database-migrate-your-sql-server-database.md)</span><span class="sxs-lookup"><span data-stu-id="56668-113">For a tutorial showing you how toomigrate a SQL Server database tooAzure SQL Database using SQLPackage, see [Migrate a SQL Server Database](sql-database-migrate-your-sql-server-database.md)</span></span>
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a><span data-ttu-id="56668-114">Import ze souboru BACPAC souboru pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="56668-114">Import from a BACPAC file using Azure portal</span></span>

<span data-ttu-id="56668-115">Tento článek obsahuje pokyny pro vytvoření Azure SQL database ze souboru BACPAC souboru je uložen v úložišti objektů blob v Azure pomocí hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="56668-115">This article provides directions for creating an Azure SQL database from a BACPAC file stored in Azure blob storage using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="56668-116">Import pomocí hello jenom portál Azure podporuje importem souboru souboru BACPAC z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="56668-116">Import using hello Azure portal only supports importing a BACPAC file from Azure blob storage.</span></span>

<span data-ttu-id="56668-117">hello tooimport databázi pomocí portálu Azure, otevřete hello stránky pro databázi a klikněte na tlačítko **Import** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="56668-117">tooimport a database using hello Azure portal, open hello page for your database and click **Import** on hello toolbar.</span></span> <span data-ttu-id="56668-118">Zadejte hello účtu úložiště a kontejneru a vyberte soubor souboru BACPAC hello chcete tooimport.</span><span class="sxs-lookup"><span data-stu-id="56668-118">Specify hello storage account and container, and select hello BACPAC file you want tooimport.</span></span> <span data-ttu-id="56668-119">Vyberte velikost hello hello nové databáze (obvykle hello stejné jako původní) a zadejte přihlašovací údaje systému SQL Server cílového hello.</span><span class="sxs-lookup"><span data-stu-id="56668-119">Select hello size of hello new database (usually hello same as origin) and provide hello destination SQL Server credentials.</span></span>  

   ![Import databáze.](./media/sql-database-import/import.png)

<span data-ttu-id="56668-121">průběh hello toomonitor hello operace importu, otevřete stránku hello hello logický server obsahující hello importované databázi.</span><span class="sxs-lookup"><span data-stu-id="56668-121">toomonitor hello progress of hello import operation, open hello page for hello logical server containing hello database being imported.</span></span> <span data-ttu-id="56668-122">Posuňte se dolů příliš**operace** a pak klikněte na **importu a exportu** historie.</span><span class="sxs-lookup"><span data-stu-id="56668-122">Scroll down too**Operations** and then click **Import/Export** history.</span></span>

### <a name="monitor-hello-progress-of-an-import-operation"></a><span data-ttu-id="56668-123">Monitorování hello průběh operace importu</span><span class="sxs-lookup"><span data-stu-id="56668-123">Monitor hello progress of an import operation</span></span>

<span data-ttu-id="56668-124">průběh hello toomonitor hello operace importu, otevřete stránku hello hello logického serveru do které hello se importují databázi naimportována.</span><span class="sxs-lookup"><span data-stu-id="56668-124">toomonitor hello progress of hello import operation, open hello page for hello logical server into which hello database is being imported imported.</span></span> <span data-ttu-id="56668-125">Posuňte se dolů příliš**operace** a pak klikněte na **importu a exportu** historie.</span><span class="sxs-lookup"><span data-stu-id="56668-125">Scroll down too**Operations** and then click **Import/Export** history.</span></span>
   
   <span data-ttu-id="56668-126">![Import](./media/sql-database-import/import-history.png) ![stav importu](./media/sql-database-import/import-status.png)</span><span class="sxs-lookup"><span data-stu-id="56668-126">![import](./media/sql-database-import/import-history.png) ![import status](./media/sql-database-import/import-status.png)</span></span>

<span data-ttu-id="56668-127">tooverify hello databáze je na serveru hello za provozu, klikněte na tlačítko **databází SQL** a ověřte nové databáze hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="56668-127">tooverify hello database is live on hello server, click **SQL databases** and verify hello new database is **Online**.</span></span>

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a><span data-ttu-id="56668-128">Import ze souboru BACPAC souboru pomocí SQLPackage</span><span class="sxs-lookup"><span data-stu-id="56668-128">Import from a BACPAC file using SQLPackage</span></span>

<span data-ttu-id="56668-129">tooimport SQL databáze pomocí hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) nástroj příkazového řádku najdete v části [importovat parametry a vlastnosti](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="56668-129">tooimport a SQL database using hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Import parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span></span> <span data-ttu-id="56668-130">Hello SQLPackage nástroj se dodává s nejnovější verzí hello [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools pro sadu Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), nebo můžete stáhnout nejnovější verzi hello [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) přímo z hello Microsoft služby Stažení softwaru.</span><span class="sxs-lookup"><span data-stu-id="56668-130">hello SQLPackage utility ships with hello latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download hello latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from hello Microsoft download center.</span></span>

<span data-ttu-id="56668-131">Doporučujeme, abyste hello použití hello SQLPackage nástroj pro škálování a výkon ve většině produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="56668-131">We recommend hello use of hello SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="56668-132">SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="56668-132">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="56668-133">Viz následující příkaz SQLPackage skriptu příklad, jak hello tooimport hello **AdventureWorks2008R2** databáze z místního úložiště tooan Azure SQL Database logického serveru, nazývá **mynewserver20170403** v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="56668-133">See hello following SQLPackage command for a script example for how tooimport hello **AdventureWorks2008R2** database from local storage tooan Azure SQL Database logical server, called **mynewserver20170403** in this example.</span></span> <span data-ttu-id="56668-134">Tento skript je ukázkou hello vytvoření nové databáze názvem **myMigratedDatabase**, s vrstvy služby **Premium**a cíl služby z **P6**.</span><span class="sxs-lookup"><span data-stu-id="56668-134">This script shows hello creation of a new database called **myMigratedDatabase**, with a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="56668-135">Tyto hodnoty změňte jako odpovídající tooyour prostředí.</span><span class="sxs-lookup"><span data-stu-id="56668-135">Change these values as appropriate tooyour environment.</span></span>

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="56668-137">Logický server Azure SQL Database naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="56668-137">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="56668-138">Pokud se pokoušíte tooconnect tooan Azure SQL Database logického serveru z podniková brána firewall, musí být tento port otevřít v hello podniková brána firewall pro připojení je toosuccessfully.</span><span class="sxs-lookup"><span data-stu-id="56668-138">If you are attempting tooconnect tooan Azure SQL Database logical server from within a corporate firewall, this port must be open in hello corporate firewall for you toosuccessfully connect.</span></span>
>

<span data-ttu-id="56668-139">Tento příklad ukazuje, jak tooimport a databáze pomocí SqlPackage.exe Universal ověřování služby Active Directory:</span><span class="sxs-lookup"><span data-stu-id="56668-139">This example shows how tooimport a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a><span data-ttu-id="56668-140">Import ze souboru BACPAC souboru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="56668-140">Import from a BACPAC file using PowerShell</span></span>

<span data-ttu-id="56668-141">Použití hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) toosubmit rutiny požadavku toohello databáze import služby Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56668-141">Use hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet toosubmit an import database request toohello Azure SQL Database service.</span></span> <span data-ttu-id="56668-142">V závislosti na velikosti hello vaší databáze hello operace importu může trvat některé toocomplete čas.</span><span class="sxs-lookup"><span data-stu-id="56668-142">Depending on hello size of your database, hello import operation may take some time toocomplete.</span></span>

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

<span data-ttu-id="56668-143">Stav hello toocheck hello žádost o import, použijte hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) rutiny.</span><span class="sxs-lookup"><span data-stu-id="56668-143">toocheck hello status of hello import request, use hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="56668-144">Obvykle to spuštěna ihned po hello vrátí požadavku **stav: InProgress**.</span><span class="sxs-lookup"><span data-stu-id="56668-144">Running this immediately after hello request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="56668-145">Až se zobrazí **stav: úspěšné** dokončení importu hello.</span><span class="sxs-lookup"><span data-stu-id="56668-145">When you see **Status: Succeeded** hello import is complete.</span></span>

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
<span data-ttu-id="56668-146">Jiný příklad skriptu najdete v tématu [Import databáze ze souboru BACPAC souboru](scripts/sql-database-import-from-bacpac-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="56668-146">For another script example, see [Import a database from a BACPAC file](scripts/sql-database-import-from-bacpac-powershell.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56668-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56668-147">Next steps</span></span>
* <span data-ttu-id="56668-148">toolearn způsobu tooconnect tooand dotaz importované SQL Database, najdete v [připojení tooSQL databáze s SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="56668-148">toolearn how tooconnect tooand query an imported SQL Database, see [Connect tooSQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>
* <span data-ttu-id="56668-149">SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="56668-149">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="56668-150">Informace hello celého systému SQL Server databáze procesu migrace, včetně doporučení výkonu, naleznete v [migrovat tooAzure databáze systému SQL Server databáze SQL](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="56668-150">For a discussion of hello entire SQL Server database migration process, including performance recommendations, see [Migrate a SQL Server database tooAzure SQL Database](sql-database-cloud-migrate.md).</span></span>



