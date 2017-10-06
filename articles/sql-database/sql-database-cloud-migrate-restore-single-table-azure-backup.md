---
title: "aaaRestore jedné tabulky ze zálohy databáze Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak toorestore jedné tabulky ze zálohy databáze SQL Azure."
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
ms.openlocfilehash: 696d2ac87a70bccdf063bfecb8255723404aa5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorestore-a-single-table-from-an-azure-sql-database-backup"></a><span data-ttu-id="e5462-103">Jak toorestore jedné tabulky ze zálohy databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="e5462-103">How toorestore a single table from an Azure SQL Database backup</span></span>
<span data-ttu-id="e5462-104">Mohou nastat situace, ve kterém můžete upravit omylem některá data v databázi SQL a teď chcete toorecover hello jedné ovlivněných tabulky.</span><span class="sxs-lookup"><span data-stu-id="e5462-104">You may encounter a situation in which you accidentally modified some data in a SQL database and now you want toorecover hello single affected table.</span></span> <span data-ttu-id="e5462-105">Tento článek popisuje, jak toorestore jedné tabulky v databázi z jednoho z hello SQL Database [automatické zálohování](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="e5462-105">This article describes how toorestore a single table in a database from one of hello SQL Database [automatic backups](sql-database-automated-backups.md).</span></span>

## <a name="preparation-steps-rename-hello-table-and-restore-a-copy-of-hello-database"></a><span data-ttu-id="e5462-106">Přípravné kroky: hello tabulku přejmenovat a obnovit kopii databáze hello</span><span class="sxs-lookup"><span data-stu-id="e5462-106">Preparation steps: Rename hello table and restore a copy of hello database</span></span>
1. <span data-ttu-id="e5462-107">Identifikujte hello tabulky v databázi Azure SQL, které chcete tooreplace s hello obnovit kopii.</span><span class="sxs-lookup"><span data-stu-id="e5462-107">Identify hello table in your Azure SQL database that you want tooreplace with hello restored copy.</span></span> <span data-ttu-id="e5462-108">Pomocí tabulky hello toorename Microsoft SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="e5462-108">Use Microsoft SQL Management Studio toorename hello table.</span></span> <span data-ttu-id="e5462-109">Můžete například přejmenovat tabulku hello jako &lt;název tabulky&gt;_old.</span><span class="sxs-lookup"><span data-stu-id="e5462-109">For example, rename hello table as &lt;table name&gt;_old.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e5462-110">tooavoid blokován, ujistěte se, že nebude žádná aktivita systémem hello tabulky, který chcete přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="e5462-110">tooavoid being blocked, make sure that there's no activity running on hello table that you are renaming.</span></span> <span data-ttu-id="e5462-111">Pokud narazíte na potíže, ujistěte se, který tento postup provést během časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="e5462-111">If you encounter issues, make sure that perform this procedure during a maintenance window.</span></span>
   >

2. <span data-ttu-id="e5462-112">Obnovení zálohy databáze tooa bodu v čase, které chcete toorecover toousing hello [bodu-In_Time obnovení](sql-database-recovery-using-backups.md#point-in-time-restore) kroky.</span><span class="sxs-lookup"><span data-stu-id="e5462-112">Restore a backup of your database tooa point in time that you want toorecover toousing hello [Point-In_Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e5462-113">Název Hello hello obnovit databáze bude mít formát DBName + časové razítko hello; například **Adventureworks2012_2016-01-01T22-12Z**.</span><span class="sxs-lookup"><span data-stu-id="e5462-113">hello name of hello restored database will be in hello DBName+TimeStamp format; for example, **Adventureworks2012_2016-01-01T22-12Z**.</span></span> <span data-ttu-id="e5462-114">Tento krok není přepsat hello existující název databáze na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e5462-114">This step doesn't overwrite hello existing database name on hello server.</span></span> <span data-ttu-id="e5462-115">Toto je bezpečnostní opatření a je určena tooallow tooverify hello obnovit databáze předtím, než vyřadit své aktuální databázi a přejmenujte hello obnovit databázi pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e5462-115">This is a safety measure, and it's intended tooallow you tooverify hello restored database before they drop their current database and rename hello restored database for production use.</span></span>
   
## <a name="copying-hello-table-from-hello-restored-database-by-using-hello-sql-database-migration-tool"></a><span data-ttu-id="e5462-116">Kopírování hello tabulku z hello obnovit databáze pomocí nástroje Migrace databáze SQL hello</span><span class="sxs-lookup"><span data-stu-id="e5462-116">Copying hello table from hello restored database by using hello SQL Database Migration tool</span></span>

1. <span data-ttu-id="e5462-117">Stáhněte a nainstalujte hello [Průvodce migrací databáze SQL](https://sqlazuremw.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="e5462-117">Download and install hello [SQL Database Migration Wizard](https://sqlazuremw.codeplex.com).</span></span>
2. <span data-ttu-id="e5462-118">Průvodce migrací databáze SQL, otevřete hello na hello **vyberte proces** vyberte **databáze v rámci analyzovat nebo migrací**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e5462-118">Open hello SQL Database Migration Wizard, on hello **Select Process** page, select **Database under Analyze/Migrate**, and then click **Next**.</span></span>

   ![Průvodce migrací databáze SQL - vyberte procesu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. <span data-ttu-id="e5462-120">V hello **připojit tooServer** dialogové okno pole, použít hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="e5462-120">In hello **Connect tooServer** dialog box, apply hello following settings:</span></span>

   * <span data-ttu-id="e5462-121">Název serveru: **váš systém SQL server**</span><span class="sxs-lookup"><span data-stu-id="e5462-121">Server name: **Your SQL server**</span></span>
   * <span data-ttu-id="e5462-122">Ověřování: **ověřování systému SQL Server**</span><span class="sxs-lookup"><span data-stu-id="e5462-122">Authentication: **SQL Server Authentication**</span></span>
   * <span data-ttu-id="e5462-123">Přihlášení: **vaše přihlášení**</span><span class="sxs-lookup"><span data-stu-id="e5462-123">Login: **Your login**</span></span>
   * <span data-ttu-id="e5462-124">Heslo: **heslo**</span><span class="sxs-lookup"><span data-stu-id="e5462-124">Password: **Your password**</span></span>
   * <span data-ttu-id="e5462-125">Databáze: **databázi Master DB (Vypsat všechny databáze)**</span><span class="sxs-lookup"><span data-stu-id="e5462-125">Database: **Master DB (List all databases)**</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e5462-126">Ve výchozím nastavení Průvodce hello uloží přihlašovací informace.</span><span class="sxs-lookup"><span data-stu-id="e5462-126">By default hello wizard saves your login information.</span></span> <span data-ttu-id="e5462-127">Pokud nechcete, aby mohla, vyberte **zapomněli přihlašovací informace**.</span><span class="sxs-lookup"><span data-stu-id="e5462-127">If you don't want it to, select **Forget Login Information**.</span></span>
   >
   
     ![Migrace databáze SQL průvodce - vyberte zdroj – krok 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. <span data-ttu-id="e5462-129">V hello **vybrat zdroj** dialogové okno, název databáze vyberte hello obnovit z hello **přípravné kroky** jako svůj zdroj a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e5462-129">In hello **Select Source** dialog box, select hello restored database name from hello **Preparation steps** section as your source, and then click **Next**.</span></span>
   
    ![Migrace databáze SQL průvodce - vyberte zdroj – krok 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. <span data-ttu-id="e5462-131">V hello **zvolte objekty** dialogové okno, vyberte hello **vyberte konkrétní databázové objekty** možnost a potom vyberte hello table(or tables) má toomigrate toohello cílový server.</span><span class="sxs-lookup"><span data-stu-id="e5462-131">In hello **Choose Objects** dialog box, select hello **Select specific database objects** option, and then select hello table(or tables) that you want toomigrate toohello target server.</span></span>
   <span data-ttu-id="e5462-132">![Průvodce migrací databáze SQL - zvolte objekty](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span><span class="sxs-lookup"><span data-stu-id="e5462-132">![SQL Database Migration wizard - Choose Objects](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span></span>
6. <span data-ttu-id="e5462-133">Na hello **skriptu Průvodce Souhrn** klikněte na tlačítko **Ano** když se zobrazí výzva o tom, jestli jste připravené toogenerate skript SQL.</span><span class="sxs-lookup"><span data-stu-id="e5462-133">On hello **Script Wizard Summary** page, click **Yes** when you’re prompted about whether you’re ready toogenerate a SQL script.</span></span> <span data-ttu-id="e5462-134">Máte také hello možnost toosave hello TSQL skript pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="e5462-134">You also have hello option toosave hello TSQL Script for later use.</span></span>
   <span data-ttu-id="e5462-135">![Průvodce migrací databáze SQL - souhrn Průvodce skriptu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span><span class="sxs-lookup"><span data-stu-id="e5462-135">![SQL Database Migration wizard - Script Wizard Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span></span>
7. <span data-ttu-id="e5462-136">Na hello **shrnutí výsledků** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e5462-136">On hello **Results Summary** page, click **Next**.</span></span>
   <span data-ttu-id="e5462-137">![Průvodce migrací databáze SQL - shrnutí výsledků](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span><span class="sxs-lookup"><span data-stu-id="e5462-137">![SQL Database Migration wizard - Results Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span></span>
8. <span data-ttu-id="e5462-138">Na hello **nastavit připojení k serveru cíl** klikněte na tlačítko **připojit tooServer**a potom zadejte podrobnosti hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e5462-138">On hello **Setup Target Server Connection** page, click **Connect tooServer**, and then enter hello details as follows:</span></span>
   
   * <span data-ttu-id="e5462-139">**Název serveru**: Cílová instance serveru</span><span class="sxs-lookup"><span data-stu-id="e5462-139">**Server Name**: Target server instance</span></span>
   * <span data-ttu-id="e5462-140">**Ověřování**: **ověřování serveru SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e5462-140">**Authentication**: **SQL Server authentication**.</span></span> <span data-ttu-id="e5462-141">Zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e5462-141">Enter your login credentials.</span></span>
   * <span data-ttu-id="e5462-142">**Databáze**: **databázi Master DB (seznam všech databází)**.</span><span class="sxs-lookup"><span data-stu-id="e5462-142">**Database**: **Master DB (List all databases)**.</span></span> <span data-ttu-id="e5462-143">Tato možnost zobrazí všechny databáze hello na cílový server hello.</span><span class="sxs-lookup"><span data-stu-id="e5462-143">This option lists all hello databases on hello target server.</span></span>
     
     ![Průvodce migrací databáze SQL - nastavit připojení k cílového serveru](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. <span data-ttu-id="e5462-145">Klikněte na tlačítko **připojit**, vyberte hello cílová databáze, který chcete toomove hello tabulky a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e5462-145">Click **Connect**, select hello target database that you want toomove hello table to, and then click **Next**.</span></span> <span data-ttu-id="e5462-146">To by měl dokončit skriptu hello dříve generované a měli byste vidět, že hello nově přesouvána tabulky zkopírovaný toohello cílovou databázi.</span><span class="sxs-lookup"><span data-stu-id="e5462-146">This should finish running hello previously generated script, and you should see hello newly moved table copied toohello target database.</span></span>

## <a name="verification-step"></a><span data-ttu-id="e5462-147">Krok ověření</span><span class="sxs-lookup"><span data-stu-id="e5462-147">Verification step</span></span>

- <span data-ttu-id="e5462-148">Dotaz a testování hello nově zkopírovat toomake tabulky se, že hello data beze změn.</span><span class="sxs-lookup"><span data-stu-id="e5462-148">Query and test hello newly copied table toomake sure that hello data is intact.</span></span> <span data-ttu-id="e5462-149">Po potvrzení, mohou vyřadit hello přejmenovat tabulku formuláře **přípravné kroky** části.</span><span class="sxs-lookup"><span data-stu-id="e5462-149">Upon confirmation, you can drop hello renamed table form **Preparation steps** section.</span></span> <span data-ttu-id="e5462-150">(například &lt;název tabulky&gt;_old).</span><span class="sxs-lookup"><span data-stu-id="e5462-150">(for example, &lt;table name&gt;_old).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5462-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5462-151">Next steps</span></span>
[<span data-ttu-id="e5462-152">Automatické zálohování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e5462-152">SQL Database automatic backups</span></span>](sql-database-automated-backups.md)

