---
title: "Obnovení jedné tabulky ze zálohy databáze Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak obnovit jednu tabulku ze zálohy databáze Azure SQL Database."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: 340b41bd-9df8-47fb-adfc-03216de38a5e
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: 8c750c503d10ea63b9665958b96db2dfea3d9a3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a><span data-ttu-id="cec35-103">Postup při obnovení jedné tabulky ze zálohy Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="cec35-103">How to restore a single table from an Azure SQL Database backup</span></span>
<span data-ttu-id="cec35-104">Mohou nastat situace, ve kterém omylem změnil některá data v databázi SQL a teď chcete obnovit jednu tabulku ovlivněné.</span><span class="sxs-lookup"><span data-stu-id="cec35-104">You may encounter a situation in which you accidentally modified some data in a SQL database and now you want to recover the single affected table.</span></span> <span data-ttu-id="cec35-105">Tento článek popisuje, jak obnovit jednu tabulku v databázi z jednoho z databáze SQL [automatické zálohování](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="cec35-105">This article describes how to restore a single table in a database from one of the SQL Database [automatic backups](sql-database-automated-backups.md).</span></span>

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a><span data-ttu-id="cec35-106">Přípravné kroky: Přejmenovat tabulku a obnovte kopii databáze</span><span class="sxs-lookup"><span data-stu-id="cec35-106">Preparation steps: Rename the table and restore a copy of the database</span></span>
1. <span data-ttu-id="cec35-107">Identifikujte tabulku v databázi Azure SQL, kterou chcete nahradit obnovené kopií.</span><span class="sxs-lookup"><span data-stu-id="cec35-107">Identify the table in your Azure SQL database that you want to replace with the restored copy.</span></span> <span data-ttu-id="cec35-108">Pomocí Microsoft SQL Management Studio přejmenovat tabulku.</span><span class="sxs-lookup"><span data-stu-id="cec35-108">Use Microsoft SQL Management Studio to rename the table.</span></span> <span data-ttu-id="cec35-109">Můžete například přejmenovat tabulku jako &lt;název tabulky&gt;_old.</span><span class="sxs-lookup"><span data-stu-id="cec35-109">For example, rename the table as &lt;table name&gt;_old.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cec35-110">Abyste se vyhnuli blokován, ujistěte se, že nebude žádná aktivita spuštěná na tabulku, která chcete přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="cec35-110">To avoid being blocked, make sure that there's no activity running on the table that you are renaming.</span></span> <span data-ttu-id="cec35-111">Pokud narazíte na potíže, ujistěte se, který tento postup provést během časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="cec35-111">If you encounter issues, make sure that perform this procedure during a maintenance window.</span></span>
   >

2. <span data-ttu-id="cec35-112">Obnovit zálohu databáze k určitému bodu v čase, který chcete obnovit pomocí [bodu-In_Time obnovení](sql-database-recovery-using-backups.md#point-in-time-restore) kroky.</span><span class="sxs-lookup"><span data-stu-id="cec35-112">Restore a backup of your database to a point in time that you want to recover to using the [Point-In_Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cec35-113">Název obnovené databáze bude ve formátu DBName + časové razítko. například **Adventureworks2012_2016-01-01T22-12Z**.</span><span class="sxs-lookup"><span data-stu-id="cec35-113">The name of the restored database will be in the DBName+TimeStamp format; for example, **Adventureworks2012_2016-01-01T22-12Z**.</span></span> <span data-ttu-id="cec35-114">Tento krok není přepsat existující název databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="cec35-114">This step doesn't overwrite the existing database name on the server.</span></span> <span data-ttu-id="cec35-115">Toto je bezpečnostní opatření a je určená umožňují ověření obnovenou databázi předtím, než vyřadit své aktuální databázi a přejmenujte obnovenou databázi pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cec35-115">This is a safety measure, and it's intended to allow you to verify the restored database before they drop their current database and rename the restored database for production use.</span></span>
   
## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a><span data-ttu-id="cec35-116">Kopírování tabulky z obnovené databáze pomocí nástroje Migrace databáze SQL</span><span class="sxs-lookup"><span data-stu-id="cec35-116">Copying the table from the restored database by using the SQL Database Migration tool</span></span>

1. <span data-ttu-id="cec35-117">Stáhněte a nainstalujte [Průvodce migrací databáze SQL](https://sqlazuremw.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="cec35-117">Download and install the [SQL Database Migration Wizard](https://sqlazuremw.codeplex.com).</span></span>
2. <span data-ttu-id="cec35-118">Otevřete Průvodce migrací databáze SQL na **vyberte proces** vyberte **databáze v rámci analyzovat nebo migrací**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cec35-118">Open the SQL Database Migration Wizard, on the **Select Process** page, select **Database under Analyze/Migrate**, and then click **Next**.</span></span>

   ![Průvodce migrací databáze SQL - vyberte procesu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. <span data-ttu-id="cec35-120">V **připojit k serveru** dialogové okno pole, použijte následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="cec35-120">In the **Connect to Server** dialog box, apply the following settings:</span></span>

   * <span data-ttu-id="cec35-121">Název serveru: **váš systém SQL server**</span><span class="sxs-lookup"><span data-stu-id="cec35-121">Server name: **Your SQL server**</span></span>
   * <span data-ttu-id="cec35-122">Ověřování: **ověřování systému SQL Server**</span><span class="sxs-lookup"><span data-stu-id="cec35-122">Authentication: **SQL Server Authentication**</span></span>
   * <span data-ttu-id="cec35-123">Přihlášení: **vaše přihlášení**</span><span class="sxs-lookup"><span data-stu-id="cec35-123">Login: **Your login**</span></span>
   * <span data-ttu-id="cec35-124">Heslo: **heslo**</span><span class="sxs-lookup"><span data-stu-id="cec35-124">Password: **Your password**</span></span>
   * <span data-ttu-id="cec35-125">Databáze: **databázi Master DB (Vypsat všechny databáze)**</span><span class="sxs-lookup"><span data-stu-id="cec35-125">Database: **Master DB (List all databases)**</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cec35-126">Ve výchozím nastavení uloží přihlašovací informace.</span><span class="sxs-lookup"><span data-stu-id="cec35-126">By default the wizard saves your login information.</span></span> <span data-ttu-id="cec35-127">Pokud nechcete, aby mohla, vyberte **zapomněli přihlašovací informace**.</span><span class="sxs-lookup"><span data-stu-id="cec35-127">If you don't want it to, select **Forget Login Information**.</span></span>
   >
   
     ![Migrace databáze SQL průvodce - vyberte zdroj – krok 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. <span data-ttu-id="cec35-129">V **vybrat zdroj** dialogové okno, vyberte název obnovené databáze z **přípravné kroky** jako svůj zdroj a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cec35-129">In the **Select Source** dialog box, select the restored database name from the **Preparation steps** section as your source, and then click **Next**.</span></span>
   
    ![Migrace databáze SQL průvodce - vyberte zdroj – krok 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. <span data-ttu-id="cec35-131">V **zvolte objekty** dialogové okno, vyberte **vyberte konkrétní databázové objekty** možnost a potom vyberte table(or tables), kterou chcete migrovat na cílový server.</span><span class="sxs-lookup"><span data-stu-id="cec35-131">In the **Choose Objects** dialog box, select the **Select specific database objects** option, and then select the table(or tables) that you want to migrate to the target server.</span></span>
   <span data-ttu-id="cec35-132">![Průvodce migrací databáze SQL - zvolte objekty](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span><span class="sxs-lookup"><span data-stu-id="cec35-132">![SQL Database Migration wizard - Choose Objects](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span></span>
6. <span data-ttu-id="cec35-133">Na **skriptu Průvodce Souhrn** klikněte na tlačítko **Ano** když se zobrazí výzva o tom, jestli jste připraveni generování skriptu SQL.</span><span class="sxs-lookup"><span data-stu-id="cec35-133">On the **Script Wizard Summary** page, click **Yes** when you’re prompted about whether you’re ready to generate a SQL script.</span></span> <span data-ttu-id="cec35-134">Máte také možnost uložení TSQL skriptu pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="cec35-134">You also have the option to save the TSQL Script for later use.</span></span>
   <span data-ttu-id="cec35-135">![Průvodce migrací databáze SQL - souhrn Průvodce skriptu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span><span class="sxs-lookup"><span data-stu-id="cec35-135">![SQL Database Migration wizard - Script Wizard Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span></span>
7. <span data-ttu-id="cec35-136">Na **shrnutí výsledků** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cec35-136">On the **Results Summary** page, click **Next**.</span></span>
   <span data-ttu-id="cec35-137">![Průvodce migrací databáze SQL - shrnutí výsledků](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span><span class="sxs-lookup"><span data-stu-id="cec35-137">![SQL Database Migration wizard - Results Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span></span>
8. <span data-ttu-id="cec35-138">Na **nastavit připojení k serveru cíl** klikněte na tlačítko **připojit k serveru**a potom zadejte následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="cec35-138">On the **Setup Target Server Connection** page, click **Connect to Server**, and then enter the details as follows:</span></span>
   
   * <span data-ttu-id="cec35-139">**Název serveru**: Cílová instance serveru</span><span class="sxs-lookup"><span data-stu-id="cec35-139">**Server Name**: Target server instance</span></span>
   * <span data-ttu-id="cec35-140">**Ověřování**: **ověřování serveru SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cec35-140">**Authentication**: **SQL Server authentication**.</span></span> <span data-ttu-id="cec35-141">Zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="cec35-141">Enter your login credentials.</span></span>
   * <span data-ttu-id="cec35-142">**Databáze**: **databázi Master DB (seznam všech databází)**.</span><span class="sxs-lookup"><span data-stu-id="cec35-142">**Database**: **Master DB (List all databases)**.</span></span> <span data-ttu-id="cec35-143">Tato možnost zobrazí všechny databáze na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="cec35-143">This option lists all the databases on the target server.</span></span>
     
     ![Průvodce migrací databáze SQL - nastavit připojení k cílového serveru](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. <span data-ttu-id="cec35-145">Klikněte na tlačítko **připojit**, vyberte cílovou databázi, který chcete přesunout tabulka, která se a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cec35-145">Click **Connect**, select the target database that you want to move the table to, and then click **Next**.</span></span> <span data-ttu-id="cec35-146">To by měl dokončit skriptu vytvořených předtím a měli byste vidět nově přesunutý tabulky zkopírován do cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="cec35-146">This should finish running the previously generated script, and you should see the newly moved table copied to the target database.</span></span>

## <a name="verification-step"></a><span data-ttu-id="cec35-147">Krok ověření</span><span class="sxs-lookup"><span data-stu-id="cec35-147">Verification step</span></span>

- <span data-ttu-id="cec35-148">Dotaz a otestovat na nově zkopírovaný tabulku a ujistěte se, že data jsou beze změn.</span><span class="sxs-lookup"><span data-stu-id="cec35-148">Query and test the newly copied table to make sure that the data is intact.</span></span> <span data-ttu-id="cec35-149">Po potvrzení, mohou vyřadit formuláře přejmenovat tabulku **přípravné kroky** části.</span><span class="sxs-lookup"><span data-stu-id="cec35-149">Upon confirmation, you can drop the renamed table form **Preparation steps** section.</span></span> <span data-ttu-id="cec35-150">(například &lt;název tabulky&gt;_old).</span><span class="sxs-lookup"><span data-stu-id="cec35-150">(for example, &lt;table name&gt;_old).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cec35-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cec35-151">Next steps</span></span>
[<span data-ttu-id="cec35-152">Automatické zálohování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="cec35-152">SQL Database automatic backups</span></span>](sql-database-automated-backups.md)

