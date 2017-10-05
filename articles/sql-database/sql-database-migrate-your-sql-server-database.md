---
title: Migrace serveru SQL DB do Azure SQL Database | Microsoft Docs
description: "Naučte se migrovat databázi SQL serveru do Azure SQL Database."
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/27/2017
ms.author: janeng
ms.openlocfilehash: 375d3ea0230e7d3fd0fc02ca7e0b8a7a76c24a27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a><span data-ttu-id="07649-103">Migrovat databázi SQL serveru do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="07649-103">Migrate your SQL Server database to Azure SQL Database</span></span>

<span data-ttu-id="07649-104">Přesunutí databázi SQL serveru do Azure SQL Database je proces tři části – nejdřív připravit, pak je exportovat a pak importování databáze.</span><span class="sxs-lookup"><span data-stu-id="07649-104">Moving your SQL Server database to Azure SQL Database is a three part process - you first prepare, then export, and then import the database.</span></span> <span data-ttu-id="07649-105">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="07649-105">In this tutorial, you learn to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07649-106">Příprava databáze v systému SQL Server pro migraci na Azure SQL Database pomocí [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span><span class="sxs-lookup"><span data-stu-id="07649-106">Prepare a database in a SQL Server for migration to Azure SQL Database using the [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)</span></span>
> * <span data-ttu-id="07649-107">Exportovat do souboru BACPAC souboru databáze</span><span class="sxs-lookup"><span data-stu-id="07649-107">Export the database to a BACPAC file</span></span>
> * <span data-ttu-id="07649-108">Souboru BACPAC soubor importovat do databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="07649-108">Import the BACPAC file into an Azure SQL Database</span></span>

<span data-ttu-id="07649-109">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="07649-109">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07649-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07649-110">Prerequisites</span></span>

<span data-ttu-id="07649-111">Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:</span><span class="sxs-lookup"><span data-stu-id="07649-111">To complete this tutorial, make sure the following prerequisites are completed:</span></span>

- <span data-ttu-id="07649-112">Nainstalovanou nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="07649-112">Installed the newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> <span data-ttu-id="07649-113">Instalace aplikace SSMS také nainstaluje nejnovější verzi SQLPackage, nástroje příkazového řádku, které je možné automatizovat řadu úloh vývoj databáze.</span><span class="sxs-lookup"><span data-stu-id="07649-113">Installing SSMS also installs the newest version of SQLPackage, a command-line utility that can be used to automate a range of database development tasks.</span></span> 
- <span data-ttu-id="07649-114">Nainstalována [asistent migrace dat](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span><span class="sxs-lookup"><span data-stu-id="07649-114">Installed the [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span></span>
- <span data-ttu-id="07649-115">Zjištění a mít přístup k databázi pro migraci.</span><span class="sxs-lookup"><span data-stu-id="07649-115">You have identified and have access to a database to migrate.</span></span> <span data-ttu-id="07649-116">Tento kurz používá [SQL Server 2008 R2 databáze AdventureWorks OLTP](https://msftdbprodsamples.codeplex.com/releases/view/59211) v instanci systému SQL Server 2008 R2 nebo novější, ale můžete použít libovolnou databázi podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="07649-116">This tutorial uses the [SQL Server 2008R2 AdventureWorks OLTP database](https://msftdbprodsamples.codeplex.com/releases/view/59211) on an instance of SQL Server 2008R2 or newer, but you can use any database of your choice.</span></span> <span data-ttu-id="07649-117">Chcete-li opravit problémy s kompatibilitou, použijte [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)</span><span class="sxs-lookup"><span data-stu-id="07649-117">To fix compatibility issues, use [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)</span></span>

## <a name="prepare-for-migration"></a><span data-ttu-id="07649-118">Příprava na migraci</span><span class="sxs-lookup"><span data-stu-id="07649-118">Prepare for migration</span></span>

<span data-ttu-id="07649-119">Jste připraveni pro přípravu na migraci.</span><span class="sxs-lookup"><span data-stu-id="07649-119">You are ready to prepare for migration.</span></span> <span data-ttu-id="07649-120">Použijte následující postup použijte  **[Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595)**  k vyhodnocení připravenosti databáze pro migraci do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07649-120">Follow these steps to use the **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)** to assess the readiness of your database for migration to Azure SQL Database.</span></span>

1. <span data-ttu-id="07649-121">Otevřete **asistent migrace dat**.</span><span class="sxs-lookup"><span data-stu-id="07649-121">Open the **Data Migration Assistant**.</span></span> <span data-ttu-id="07649-122">Přímý přístup do paměti lze spustit na libovolném počítači s připojením k instanci systému SQL Server obsahující databáze, která chcete migrovat, nemusíte jej nainstalovat na počítač, který je hostitelem instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07649-122">You can run DMA on any computer with connectivity to the SQL Server instance containing the database that you plan to migrate, you do not need to install it on the computer hosting the SQL Server instance.</span></span>

     ![Asistent migrace systému Open data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. <span data-ttu-id="07649-124">V levé nabídce klikněte na tlačítko **+ nový** vytvořit **Assessment** projektu.</span><span class="sxs-lookup"><span data-stu-id="07649-124">In the left-hand menu, click **+ New** to create an **Assessment** project.</span></span> <span data-ttu-id="07649-125">Vyplňte formulář s **název projektu** (všechny ostatní hodnoty musí být ponechány na jejich výchozí hodnoty) a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="07649-125">Fill in the form with a **Project name** (all other values should be left at their default values) and click **Create**.</span></span> <span data-ttu-id="07649-126">**Možnosti** otevře se stránka.</span><span class="sxs-lookup"><span data-stu-id="07649-126">The **Options** page opens.</span></span>

     ![Nový projekt asistenta migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. <span data-ttu-id="07649-128">Na **možnosti** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="07649-128">On the **Options** page, click **Next**.</span></span> <span data-ttu-id="07649-129">**Vyberte zdroje** otevře se stránka.</span><span class="sxs-lookup"><span data-stu-id="07649-129">The **Select sources** page opens.</span></span>

     ![nové možnosti migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. <span data-ttu-id="07649-131">Na **vyberte zdroje** stránky, zadejte název instance systému SQL Server obsahující server, kterou chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="07649-131">On the **Select sources** page, enter the name of SQL Server instance containing the server you plan to migrate.</span></span> <span data-ttu-id="07649-132">V případě potřeby změňte jiné hodnoty na této stránce.</span><span class="sxs-lookup"><span data-stu-id="07649-132">Change the other values on this page if necessary.</span></span> <span data-ttu-id="07649-133">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="07649-133">Click **Connect**.</span></span>

     ![nové vyberte zdroje dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. <span data-ttu-id="07649-135">V **přidejte zdroje** část **vyberte zdroje** vyberte zaškrtávací políčka pro databáze, která má být testována pro kompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="07649-135">In the **Add sources** portion of the **Select sources** page, select the checkboxes for the databases to be tested for compatibility.</span></span> <span data-ttu-id="07649-136">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="07649-136">Click **Add**.</span></span>

     ![nové vyberte zdroje dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. <span data-ttu-id="07649-138">Klikněte na tlačítko **spustit Assessment**.</span><span class="sxs-lookup"><span data-stu-id="07649-138">Click **Start Assessment**.</span></span>

     ![nové vyhodnocení zahájení migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. <span data-ttu-id="07649-140">Po dokončení hodnocení první pohled, pokud je databáze dostatečně kompatibilní k migraci.</span><span class="sxs-lookup"><span data-stu-id="07649-140">When the assessment completes, first look to see if the database is sufficiently compatible to migrate.</span></span> <span data-ttu-id="07649-141">Podívejte se na značku zaškrtnutí v zelená kruh.</span><span class="sxs-lookup"><span data-stu-id="07649-141">Look for the checkmark in a green circle.</span></span>

     ![výsledky kompatibilní nové vyhodnocení migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. <span data-ttu-id="07649-143">Zkontrolujte výsledky.</span><span class="sxs-lookup"><span data-stu-id="07649-143">Review the results.</span></span> <span data-ttu-id="07649-144">**Parity funkcí systému SQL Server** výsledky zobrazeny jsou výsledky výchozí ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="07649-144">The **SQL Server feature parity** results shown are the default results to review.</span></span> <span data-ttu-id="07649-145">Konkrétně zkontrolujte informace o nepodporované a částečně podporované funkce a zadané informace o doporučených akcí.</span><span class="sxs-lookup"><span data-stu-id="07649-145">Specifically review the information about unsupported and partially supported features, and the provided information about recommended actions.</span></span> 

     ![nové paritní assessment migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. <span data-ttu-id="07649-147">Zkontrolujte **problémy s kompatibilitou** kliknutím na tuto možnost v levé horní části.</span><span class="sxs-lookup"><span data-stu-id="07649-147">Review the **Compatibility issues** by clicking that option in the upper left.</span></span> <span data-ttu-id="07649-148">Konkrétně zkontrolujte informace o blokování migrace, změny chování a zastaralé funkce pro každou úroveň kompatibility.</span><span class="sxs-lookup"><span data-stu-id="07649-148">Specifically review the information about migration blockers, behavior changes, and deprecated features for each compatibility level.</span></span> <span data-ttu-id="07649-149">Pro databázi AdventureWorks2008R2 zkontrolujte změny Full-Text Search od SQL Server 2008 a změny SERVERPROPERTY('LCID') systémem SQL Server 2000.</span><span class="sxs-lookup"><span data-stu-id="07649-149">For the AdventureWorks2008R2 database, review the changes to Full-Text Search since SQL Server 2008 and the changes to SERVERPROPERTY('LCID') since SQL Server 2000.</span></span> <span data-ttu-id="07649-150">Podrobnosti o těchto změnách se poskytuje odkazy na další informace.</span><span class="sxs-lookup"><span data-stu-id="07649-150">For details on these changes, links for more information is provided.</span></span> <span data-ttu-id="07649-151">Mnoho možnosti vyhledávání a změnili nastavení pro fulltextové vyhledávání</span><span class="sxs-lookup"><span data-stu-id="07649-151">Many search options and settings for Full-Text Search have changed</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="07649-152">Po dokončení migrace databáze do Azure SQL Database, můžete pracovat databázi na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2) nebo na vyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="07649-152">After you migrate your database to Azure SQL Database, you can choose to operate the database at its current compatibility level (level 100 for the AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="07649-153">Další informace o důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="07649-153">For more information on the implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="07649-154">Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s úrovní kompatibility.</span><span class="sxs-lookup"><span data-stu-id="07649-154">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related to compatibility levels.</span></span>
   >

10. <span data-ttu-id="07649-155">Volitelně klikněte na **exportovat sestavu** pro uložení sestavy jako soubor ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="07649-155">Optionally, click **Export report** to save the report as a JSON file.</span></span>
11. <span data-ttu-id="07649-156">Asistent migrace dat zavřete.</span><span class="sxs-lookup"><span data-stu-id="07649-156">Close the Data Migration Assistant.</span></span>

## <a name="export-to-bacpac-file"></a><span data-ttu-id="07649-157">Export souboru BACPAC souboru</span><span class="sxs-lookup"><span data-stu-id="07649-157">Export to BACPAC file</span></span> 

<span data-ttu-id="07649-158">Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující metadata a data z databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07649-158">A BACPAC file is a ZIP file with an extension of BACPAC containing the metadata and data from a SQL Server database.</span></span> <span data-ttu-id="07649-159">Soubor souboru BACPAC lze uložit v úložišti objektů blob v Azure nebo v místní úložiště pro archivaci nebo migrace – například jako v systému SQL Server do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07649-159">A BACPAC file can be stored in Azure blob storage or in local storage for archiving or for migration - such as from SQL Server to Azure SQL Database.</span></span> <span data-ttu-id="07649-160">Pro export být stavu transakční konzistence, je nutné zajistit buď žádné zápisu aktivity dochází při exportu.</span><span class="sxs-lookup"><span data-stu-id="07649-160">For an export to be transactionally consistent, you must ensure either that no write activity is occurring during the export.</span></span>

<span data-ttu-id="07649-161">Použijte následující postup pomocí nástroje příkazového řádku SQLPackage se exportovat databázi AdventureWorks2008R2 do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="07649-161">Follow these steps to use the SQLPackage command-line utility to export the AdventureWorks2008R2 database to local storage.</span></span>

1. <span data-ttu-id="07649-162">Otevřete příkazový řádek systému Windows a změňte adresáře na složku, ve které máte **130** verze SQLPackage – například **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.</span><span class="sxs-lookup"><span data-stu-id="07649-162">Open a Windows command prompt and change your directory to a folder in which you have the **130** version of SQLPackage - such as **C:\Program Files (x86)\Microsoft SQL Server\130\DAC\bin**.</span></span>

2. <span data-ttu-id="07649-163">Spusťte následující příkaz SQLPackage příkazového řádku pro export **AdventureWorks2008R2** databáze z **localhost** k **AdventureWorks2008R2.bacpac**.</span><span class="sxs-lookup"><span data-stu-id="07649-163">Execute the following SQLPackage command at the command prompt to export the **AdventureWorks2008R2** database from **localhost** to **AdventureWorks2008R2.bacpac**.</span></span> <span data-ttu-id="07649-164">Změna těchto hodnot v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="07649-164">Change any of these values as appropriate to your environment.</span></span>

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

<span data-ttu-id="07649-166">Po dokončení provádění generovaného souboru BACPAC soubor je uložen v adresáři, kde se nachází sqlpackage spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="07649-166">Once the execution is complete the generated BACPAC file is stored in the directory where the sqlpackage executable is located.</span></span> <span data-ttu-id="07649-167">V tomto příkladu C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin.</span><span class="sxs-lookup"><span data-stu-id="07649-167">In this example C:\Program Files (x86)\Microsoft SQL Server\130\DAC\bin.</span></span> 

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="07649-168">Přihlášení k portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="07649-168">Log in to the Azure portal</span></span>

<span data-ttu-id="07649-169">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="07649-169">Log in to the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="07649-170">Přihlášení z počítače, ze kterého spouštíte nástroj příkazového řádku SQLPackage usnadňuje vytvoření pravidla brány firewall v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="07649-170">Logging on from the computer from which you are running the SQLPackage command-line utility eases the creation of the firewall rule in step 5.</span></span>

## <a name="create-a-sql-server-logical-server"></a><span data-ttu-id="07649-171">Vytvoření logického serveru SQL server</span><span class="sxs-lookup"><span data-stu-id="07649-171">Create a SQL server logical server</span></span>

<span data-ttu-id="07649-172">A [logickému serveru SQL server](sql-database-features.md) funguje jako centrální administrativní bod pro více databází.</span><span class="sxs-lookup"><span data-stu-id="07649-172">A [SQL server logical server](sql-database-features.md) acts as a central administrative point for multiple databases.</span></span> <span data-ttu-id="07649-173">Postupujte podle těchto kroků k vytvoření logického serveru SQL server tak, aby obsahovala migrované databázi společnosti Adventure Works OLTP SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07649-173">Follow these steps to create a SQL server logical server to contain the migrated Adventure Works OLTP SQL Server database.</span></span> 

1. <span data-ttu-id="07649-174">Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="07649-174">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="07649-175">Typ **systému sql server** v okně hledání na **nový** a vyberte **databáze SQL (logický server)** ze seznamu Filtrované.</span><span class="sxs-lookup"><span data-stu-id="07649-175">Type **sql server** in the search window on the **New** page, and select **SQL database (logical server)** from the filtered list.</span></span>

    ![výběr logického serveru](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. <span data-ttu-id="07649-177">Na **všechno, co** klikněte na tlačítko **systému SQL server (logický server)** a pak klikněte na **vytvořit** na **systému SQL Server (logický server)** stránky.</span><span class="sxs-lookup"><span data-stu-id="07649-177">On the **Everything** page, click **SQL server (logical server)** and then click **Create** on the **SQL Server (logical server)** page.</span></span>

    ![vytvoření logického serveru](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. <span data-ttu-id="07649-179">Vyplňte formulář serveru SQL (logický server) pomocí následujících informací, jak je vidět na předchozím obrázku:</span><span class="sxs-lookup"><span data-stu-id="07649-179">Fill out the SQL server (logical server) form with the following information, as shown on the preceding image:</span></span>     

   | <span data-ttu-id="07649-180">Nastavení</span><span class="sxs-lookup"><span data-stu-id="07649-180">Setting</span></span>       | <span data-ttu-id="07649-181">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="07649-181">Suggested value</span></span> | <span data-ttu-id="07649-182">Popis</span><span class="sxs-lookup"><span data-stu-id="07649-182">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="07649-183">**Název serveru**</span><span class="sxs-lookup"><span data-stu-id="07649-183">**Server name**</span></span> | <span data-ttu-id="07649-184">Zadejte všechny globálně jedinečného názvu</span><span class="sxs-lookup"><span data-stu-id="07649-184">Enter any globally unique name</span></span> | <span data-ttu-id="07649-185">Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="07649-185">For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> | 
   | <span data-ttu-id="07649-186">**Přihlašovací jméno správce serveru**</span><span class="sxs-lookup"><span data-stu-id="07649-186">**Server admin login**</span></span> | <span data-ttu-id="07649-187">Zadat libovolný platný název</span><span class="sxs-lookup"><span data-stu-id="07649-187">Enter any valid name</span></span> | <span data-ttu-id="07649-188">Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="07649-188">For valid login names, see [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span></span> |
   | <span data-ttu-id="07649-189">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="07649-189">**Password**</span></span> | <span data-ttu-id="07649-190">Zadejte všechny platné heslo</span><span class="sxs-lookup"><span data-stu-id="07649-190">Enter any valid password</span></span> | <span data-ttu-id="07649-191">Heslo musí mít aspoň 8 znaků a musí obsahovat znaky ze tří z následujících kategorií: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="07649-191">Your password must have at least 8 characters and must contain characters from three of the following categories: upper case characters, lower case characters, numbers, and non-alphanumeric characters.</span></span> |
   | <span data-ttu-id="07649-192">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="07649-192">**Subscription**</span></span> | <span data-ttu-id="07649-193">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="07649-193">Select a subscription</span></span> | <span data-ttu-id="07649-194">Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="07649-194">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="07649-195">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="07649-195">**Resource group**</span></span> | <span data-ttu-id="07649-196">Vyberte existující skupinu prostředků nebo vytvořte novou skupinu, například **myResourceGroup**</span><span class="sxs-lookup"><span data-stu-id="07649-196">Choose an existing resource group or create a new group, such as **myResourceGroup**</span></span> |  <span data-ttu-id="07649-197">Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="07649-197">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="07649-198">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="07649-198">**Location**</span></span> | <span data-ttu-id="07649-199">Zadejte všechny platné umístění pro nový server</span><span class="sxs-lookup"><span data-stu-id="07649-199">Enter any valid location for the new server</span></span> | <span data-ttu-id="07649-200">Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="07649-200">For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/).</span></span> |

   ![vytvoření logického serveru byla dokončena formuláře](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. <span data-ttu-id="07649-202">Klikněte na tlačítko **vytvořit** ke zřízení logického serveru.</span><span class="sxs-lookup"><span data-stu-id="07649-202">Click **Create** to provision the logical server.</span></span> <span data-ttu-id="07649-203">Zřizování trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="07649-203">Provisioning takes a few minutes.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="07649-204">Mějte na paměti, váš název serveru, serveru správce přihlašovací jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="07649-204">Remember your server name, server admin login name, and password.</span></span> <span data-ttu-id="07649-205">Je nutné tyto hodnoty později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="07649-205">You need these values later in this tutorial.</span></span>
>

## <a name="create-a-server-level-firewall-rule"></a><span data-ttu-id="07649-206">Vytvoření pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="07649-206">Create a server-level firewall rule</span></span>

<span data-ttu-id="07649-207">Vytvoří služba SQL Database [brány firewall na úrovni serveru](sql-database-firewall-configure.md) , který brání externí aplikace a nástroje pro připojení k serveru nebo kterékoli databázi na serveru, pokud není vytvořená pravidla brány firewall pro otevření brány firewall pro konkrétní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="07649-207">The SQL Database service creates a [firewall at the server-level](sql-database-firewall-configure.md) that prevents external applications and tools from connecting to the server or any databases on the server unless a firewall rule is created to open the firewall for specific IP addresses.</span></span> <span data-ttu-id="07649-208">Postupujte podle těchto kroků k vytvoření pravidla brány firewall na úrovni serveru, databáze SQL pro IP adresu počítače, ze kterého spouštíte nástroj příkazového řádku SQLPackage.</span><span class="sxs-lookup"><span data-stu-id="07649-208">Follow these steps to create a SQL Database server-level firewall rule for the IP address of the computer from which you are running the SQLPackage command-line utility.</span></span> <span data-ttu-id="07649-209">To umožňuje SQLPackage pro připojení k logickému serveru SQL serverDatabase přes bránu firewall databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07649-209">This enables SQLPackage to connect to the SQL serverDatabase logical server through the Azure SQL Database firewall.</span></span> 

1. <span data-ttu-id="07649-210">Klikněte na tlačítko **všechny prostředky** z nabídky na levé straně a klikněte na nový server **všechny prostředky** stránky.</span><span class="sxs-lookup"><span data-stu-id="07649-210">Click **All resources** from the left-hand menu and click your new server on the **All resources** page.</span></span> <span data-ttu-id="07649-211">Na stránce Přehled pro váš server otevře a poskytuje možnosti pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="07649-211">The overview page for your server opens and provides options for further configuration.</span></span>

     ![Přehled logický server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. <span data-ttu-id="07649-213">Klikněte na tlačítko **brány Firewall** v levé nabídce v části **nastavení** na stránce Přehled.</span><span class="sxs-lookup"><span data-stu-id="07649-213">Click **Firewall** in the left-hand menu under **Settings** on the overview page.</span></span> <span data-ttu-id="07649-214">Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07649-214">The **Firewall settings** page for the SQL Database server opens.</span></span> 

3. <span data-ttu-id="07649-215">Klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů přidat IP adresu počítače jsou aktuálně používá a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07649-215">Click **Add client IP** on the toolbar to add the IP address of the computer you are currently using and then click **Save**.</span></span> <span data-ttu-id="07649-216">Pravidlo brány firewall na úrovni serveru se vytvoří pro tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="07649-216">A server-level firewall rule is created for this IP address.</span></span>

     ![nastavení pravidla brány firewall serveru](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. <span data-ttu-id="07649-218">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="07649-218">Click **OK**.</span></span>

<span data-ttu-id="07649-219">Nyní můžete připojit pro všechny databáze na tomto serveru pomocí SQL Server Management Studio nebo jiný nástroj podle svého výběru z tuto IP adresu pomocí účtu správce serveru, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="07649-219">You can now connect to all databases on this server using SQL Server Management Studio or another tool of your choice from this IP address using the server admin account created previously.</span></span>

> [!NOTE]
> <span data-ttu-id="07649-220">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="07649-220">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="07649-221">Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="07649-221">If you are trying to connect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="07649-222">Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="07649-222">If so, you cannot connect to your Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="import-a-bacpac-file-to-azure-sql-database"></a><span data-ttu-id="07649-223">Importovat soubor souboru BACPAC do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="07649-223">Import a BACPAC file to Azure SQL Database</span></span> 

<span data-ttu-id="07649-224">Nejnovější verze nástroje příkazového řádku SQLPackage poskytovat podporu pro vytvoření Azure SQL database v zadané [služby úroveň a úroveň výkonu](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="07649-224">The newest versions of the SQLPackage command-line utility provide support for creating an Azure SQL database at a specified [service tier and performance level](sql-database-service-tiers.md).</span></span> <span data-ttu-id="07649-225">Pro nejlepší výkon při importu vyberte vysokou úroveň služby a výkonu úroveň a pak vertikálně snížit kapacitu po importu Pokud úrovně služby a výkonu je vyšší, než je nutné okamžitě.</span><span class="sxs-lookup"><span data-stu-id="07649-225">For best performance during import, select a high service tier and performance level and then scale down after import if the service tier and performance level is higher than you need immediately.</span></span>

<span data-ttu-id="07649-226">Použijte tyto kroky použijte nástroj příkazového řádku SQLPackage k importování databáze AdventureWorks2008R2 do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07649-226">Follow these steps use the SQLPackage command-line utility to import the AdventureWorks2008R2 database to Azure SQL Database.</span></span> <span data-ttu-id="07649-227">Když používáte SQL Server Management Studio pro tuto úlohu je SQLPackage upřednostňovanou metodou pro většinu produkčního prostředí pro maximální flexibilitu a nejlepší výkon.</span><span class="sxs-lookup"><span data-stu-id="07649-227">While you can use SQL Server Management Studio for this task, SQLPackage is the preferred method for most production environments for maximum flexibility and best performance.</span></span> <span data-ttu-id="07649-228">V tématu [migrace ze systému SQL Server do Azure SQL Database pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="07649-228">See [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

- <span data-ttu-id="07649-229">Spusťte následující příkaz SQLPackage příkazového řádku k importu **AdventureWorks2008R2** databáze z místního úložiště k logickému serveru služby SQL server, který jste dříve vytvořili pro novou databázi, úroveň služby **Premium**a cíl služby z **P6**.</span><span class="sxs-lookup"><span data-stu-id="07649-229">Execute the following SQLPackage command at the command prompt to import the **AdventureWorks2008R2** database from local storage to the SQL server logical server that you previously created to a new database, a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="07649-230">Nahraďte hodnoty v lomených závorkách s příslušnými hodnotami pro logický server SQL server a zadejte název pro novou databázi (taky nahradit lomené závorky).</span><span class="sxs-lookup"><span data-stu-id="07649-230">Replace the values in angle brackets with appropriate values for your SQL server logical server and specify a name for the new database (also replace the angle brackets).</span></span> <span data-ttu-id="07649-231">Můžete také změnit hodnoty edici databáze a objectgive služby v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="07649-231">You can also choose to change the values for database edition and service objectgive as appropriate to your environment.</span></span> <span data-ttu-id="07649-232">Pro účely tohoto kurzu migrované databázi se říká **myMigratedDatabase**.</span><span class="sxs-lookup"><span data-stu-id="07649-232">For the purpose of this tutorial, the migrated database is called **myMigratedDatabase**.</span></span>

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="07649-234">Logickému serveru SQL server naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="07649-234">A SQL server logical server listens on port 1433.</span></span> <span data-ttu-id="07649-235">Pokud se pokoušíte připojit k serveru logickému serveru SQL z v rámci podnikové brány firewall, je třeba otevřít v podnikové brány firewall můžete úspěšně připojit tento port.</span><span class="sxs-lookup"><span data-stu-id="07649-235">If you are attempting to connect to a SQL server logical server from within a corporate firewall, this port must be open in the corporate firewall for you to successfully connect.</span></span>
>

## <a name="connect-using-sql-server-management-studio-ssms"></a><span data-ttu-id="07649-236">Připojení pomocí SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="07649-236">Connect using SQL Server Management Studio (SSMS)</span></span>

<span data-ttu-id="07649-237">Použít SQL Server Management Studio k navázání připojení k serveru Azure SQL Database a nově migrovat databázi, nazývá **myMigratedDatabase** v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="07649-237">Use SQL Server Management Studio to establish a connection to your Azure SQL Database server and newly migrated database, called **myMigratedDatabase** in this tutorial.</span></span> <span data-ttu-id="07649-238">Pokud používáte systém SQL Server Management Studio do jiného počítače, z níž jste spustili SQLPackage, vytvořte pravidlo brány firewall pro tento počítač pomocí kroků v předchozím postupu.</span><span class="sxs-lookup"><span data-stu-id="07649-238">If you are running SQL Server Management Studio on a different computer from which you ran SQLPackage, create a firewall rule for this computer using the steps in the previous procedure.</span></span>

1. <span data-ttu-id="07649-239">Otevřete SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="07649-239">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="07649-240">V dialogovém okně **Připojení k serveru** zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="07649-240">In the **Connect to Server** dialog box, enter the following information:</span></span>
   - <span data-ttu-id="07649-241">**Typ serveru:** Zadejte Databázový stroj.</span><span class="sxs-lookup"><span data-stu-id="07649-241">**Server type**: Specify Database engine</span></span>
   - <span data-ttu-id="07649-242">**Název serveru**: Zadejte název plně kvalifikovaný serveru, jako například **mynewserver20170403.database.windows.net**</span><span class="sxs-lookup"><span data-stu-id="07649-242">**Server name**: Enter your fully qualified server name, such as **mynewserver20170403.database.windows.net**</span></span>
   - <span data-ttu-id="07649-243">**Ověřování:** Zadejte Ověřování SQL Serveru.</span><span class="sxs-lookup"><span data-stu-id="07649-243">**Authentication**: Specify SQL Server Authentication</span></span>
   - <span data-ttu-id="07649-244">**Přihlášení:** Zadejte účet správce serveru.</span><span class="sxs-lookup"><span data-stu-id="07649-244">**Login**: Enter your server admin account</span></span>
   - <span data-ttu-id="07649-245">**Heslo:** Zadejte heslo pro účet správce serveru.</span><span class="sxs-lookup"><span data-stu-id="07649-245">**Password**: Enter the password for your server admin account</span></span>
 
    ![připojit pomocí ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. <span data-ttu-id="07649-247">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="07649-247">Click **Connect**.</span></span> <span data-ttu-id="07649-248">Otevře se okno Průzkumník objektů.</span><span class="sxs-lookup"><span data-stu-id="07649-248">The Object Explorer window opens.</span></span> 

4. <span data-ttu-id="07649-249">V Průzkumníku objektů rozbalte **databáze** a potom rozbalte **myMigratedDatabase** zobrazit objekty v ukázkové databázi.</span><span class="sxs-lookup"><span data-stu-id="07649-249">In Object Explorer, expand **Databases** and then expand **myMigratedDatabase** to view the objects in the sample database.</span></span>

## <a name="change-database-properties"></a><span data-ttu-id="07649-250">Změnit vlastnosti databáze</span><span class="sxs-lookup"><span data-stu-id="07649-250">Change database properties</span></span>

<span data-ttu-id="07649-251">Můžete změnit úroveň služby, úroveň výkonu a úroveň kompatibility pomocí SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="07649-251">You can change the service tier, performance level, and compatibility level using SQL Server Management Studio.</span></span> <span data-ttu-id="07649-252">Během fáze importu doporučujeme je importovat do databáze vyšší úroveň výkonu pro nejlepší výkon, ale po dokončení importu jak ušetřit peníze, dokud nebudete připraveni aktivně používá databázi importované škálování směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="07649-252">During the import phase, we recommend that you import to a higher performance tier database for best performance, but that you scale down after the import completes to save money until you are ready to actively use the imported database.</span></span> <span data-ttu-id="07649-253">Změna úrovně kompatibility mohou vést k vyšší výkon a přístup k nejnovější funkcím služby Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07649-253">Changing the compatibility level may yield better performance and access to the newest capabilities of the Azure SQL Database service.</span></span> <span data-ttu-id="07649-254">Když provádíte migraci starší databázi, její úroveň kompatibility databáze je udržován na úrovni nejnižší podporovaná úroveň, který je kompatibilní s databází importována.</span><span class="sxs-lookup"><span data-stu-id="07649-254">When you migrate an older database, its database compatibility level is maintained at the lowest supported level that is compatible with the database being imported.</span></span> <span data-ttu-id="07649-255">Další informace najdete v tématu [vylepšuje výkon dotazů 130 úroveň kompatibility v Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).</span><span class="sxs-lookup"><span data-stu-id="07649-255">For more information, see [Improved query performance with compatibility Level 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).</span></span>

1. <span data-ttu-id="07649-256">V Průzkumníku objektů, klikněte pravým tlačítkem na **myMigratedDatabase** a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="07649-256">In Object Explorer, right-click **myMigratedDatabase** and click **New Query**.</span></span> <span data-ttu-id="07649-257">Otevře se okno dotazu připojené k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="07649-257">A query window opens connected to your database.</span></span>

2. <span data-ttu-id="07649-258">Spusťte následující příkaz pro danou vrstvu služeb **standardní** a na úroveň výkonu **S1**.</span><span class="sxs-lookup"><span data-stu-id="07649-258">Execute the following command to set the service tier to **Standard** and the performance level to **S1**.</span></span>

    ```
    ALTER DATABASE myMigratedDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

    ![změnit úroveň služby](./media/sql-database-migrate-your-sql-server-database/service-tier.png)

3. <span data-ttu-id="07649-260">Spusťte následující příkaz, chcete-li změnit úroveň kompatibility databáze **130**.</span><span class="sxs-lookup"><span data-stu-id="07649-260">Execute the following command to change the database compatibility level to **130**.</span></span>

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Změňte úroveň kompatibility](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a><span data-ttu-id="07649-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07649-262">Next steps</span></span> 
<span data-ttu-id="07649-263">V tomto kurzu připravená, exportovat a importovat vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="07649-263">In this tutorial you prepared, exported and imported your database.</span></span> <span data-ttu-id="07649-264">Jste se dozvěděli, do:</span><span class="sxs-lookup"><span data-stu-id="07649-264">You learned to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07649-265">Příprava databáze v systému SQL Server pro migraci do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="07649-265">Prepare a database in a SQL Server for migration to Azure SQL Database</span></span>
> * <span data-ttu-id="07649-266">Exportovat do souboru BACPAC souboru databáze</span><span class="sxs-lookup"><span data-stu-id="07649-266">Export the database to a BACPAC file</span></span>
> * <span data-ttu-id="07649-267">Souboru BACPAC soubor importovat do databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="07649-267">Import the BACPAC file into an Azure SQL Database</span></span>

<span data-ttu-id="07649-268">Přechodu na v dalším kurzu se dozvíte, jak zabezpečit vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="07649-268">Advance to the next tutorial to learn how to secure your database.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="07649-269">[Zabezpečení databáze Azure SQL](sql-database-security-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="07649-269">[Secure your Azure SQL Database](sql-database-security-tutorial.md).</span></span>


