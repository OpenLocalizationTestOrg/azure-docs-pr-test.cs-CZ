---
title: "aaaGetting začít s Azure synchronizaci dat SQL (Preview) | Microsoft Docs"
description: "Tento kurz vám pomůže začít pracovat s synchronizaci dat SQL Azure (Preview)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.openlocfilehash: 666d09237e42acc23ae3c8c81e60734a413f5949
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a><span data-ttu-id="22d32-103">Začínáme s synchronizaci dat Azure SQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="22d32-103">Getting Started with Azure SQL Data Sync (Preview)</span></span>
<span data-ttu-id="22d32-104">V tomto kurzu zjistíte, jak tooset až synchronizaci dat SQL Azure vytvořit skupinu hybridních synchronizace, který obsahuje instance Azure SQL Database a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="22d32-104">In this tutorial, you learn how tooset up Azure SQL Data Sync by creating a hybrid sync group that contains both Azure SQL Database and SQL Server instances.</span></span> <span data-ttu-id="22d32-105">Hello nové synchronizace skupiny plně konfigurována a synchronizuje na hello nastaveného plánu.</span><span class="sxs-lookup"><span data-stu-id="22d32-105">hello new sync group is fully configured and synchronizes on hello schedule you set.</span></span>

<span data-ttu-id="22d32-106">Tento kurz předpokládá, že máte alespoň zkušenosti s SQL Database a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="22d32-106">This tutorial assumes that you have at least some prior experience with SQL Database and with SQL Server.</span></span> 

<span data-ttu-id="22d32-107">Přehled synchronizaci dat SQL najdete v tématu [synchronizaci dat](sql-database-sync-data.md).</span><span class="sxs-lookup"><span data-stu-id="22d32-107">For an overview of SQL Data Sync, see [Sync data](sql-database-sync-data.md).</span></span>

<span data-ttu-id="22d32-108">Pro dokončení příklady prostředí PowerShell, které ukazují, jak tooconfigure synchronizaci dat SQL, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="22d32-108">For complete PowerShell examples that show how tooconfigure SQL Data Sync, see hello following articles:</span></span>
-   [<span data-ttu-id="22d32-109">Použití prostředí PowerShell toosync mezi více databází Azure SQL</span><span class="sxs-lookup"><span data-stu-id="22d32-109">Use PowerShell toosync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
-   [<span data-ttu-id="22d32-110">Použití prostředí PowerShell toosync mezi databáze SQL Azure a místní databáze SQL serveru</span><span class="sxs-lookup"><span data-stu-id="22d32-110">Use PowerShell toosync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> <span data-ttu-id="22d32-111">Hello dokončení technická dokumentace pro synchronizaci dat SQL Azure, byly dříve umístěny na webu MSDN, nastavit je jako. Dokument PDF.</span><span class="sxs-lookup"><span data-stu-id="22d32-111">hello complete technical documentation set for Azure SQL Data Sync, formerly located on MSDN, is available as a .PDF document.</span></span> <span data-ttu-id="22d32-112">Stažení [zde](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span><span class="sxs-lookup"><span data-stu-id="22d32-112">Download it [here](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span></span>

## <a name="step-1---create-sync-group"></a><span data-ttu-id="22d32-113">Krok 1 – Vytvoření skupiny synchronizace</span><span class="sxs-lookup"><span data-stu-id="22d32-113">Step 1 - Create sync group</span></span>

### <a name="locate-hello-data-sync-settings"></a><span data-ttu-id="22d32-114">Najít nastavení synchronizace dat hello</span><span class="sxs-lookup"><span data-stu-id="22d32-114">Locate hello Data Sync settings</span></span>

1.  <span data-ttu-id="22d32-115">V prohlížeči přejděte toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22d32-115">In your browser, navigate toohello Azure portal.</span></span>

2.  <span data-ttu-id="22d32-116">Hello portálu vyhledejte vaší databáze SQL z řídicího panelu nebo hello databází SQL ikonu na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-116">In hello portal, locate your SQL databases from your Dashboard or from hello SQL Databases icon on hello toolbar.</span></span>

    ![Seznam databází Azure SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  <span data-ttu-id="22d32-118">Na hello **databází SQL** okně, vyberte hello stávající má toouse jako databáze hello rozbočovače pro synchronizace dat. hello SQL databáze okno otevře databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="22d32-118">On hello **SQL databases** blade, select hello existing SQL database that you want toouse as hello hub database for Data Sync. hello SQL database blade opens.</span></span>

4.  <span data-ttu-id="22d32-119">V okně databáze SQL hello hello zvolené databázi, vyberte **synchronizaci databázích tooother**.</span><span class="sxs-lookup"><span data-stu-id="22d32-119">On hello SQL database blade for hello selected database, select **Sync tooother databases**.</span></span> <span data-ttu-id="22d32-120">Otevře se okno synchronizaci dat Hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-120">hello Data Sync blade opens.</span></span>

    ![Možnosti databáze tooother synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a><span data-ttu-id="22d32-122">Vytvořit novou skupinu synchronizace</span><span class="sxs-lookup"><span data-stu-id="22d32-122">Create a new Sync Group</span></span>

1.  <span data-ttu-id="22d32-123">V okně hello synchronizaci dat, vyberte **nové synchronizace skupiny**.</span><span class="sxs-lookup"><span data-stu-id="22d32-123">On hello Data Sync blade, select **New Sync Group**.</span></span> <span data-ttu-id="22d32-124">Hello **nové synchronizace skupiny** otevře se okno s krokem 1 **vytvořit skupinu synchronizace**, zvýrazněné.</span><span class="sxs-lookup"><span data-stu-id="22d32-124">hello **New sync group** blade opens with Step 1, **Create sync group**, highlighted.</span></span> <span data-ttu-id="22d32-125">Hello **vytvořit skupinu synchronizace dat** také otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-125">hello **Create Data Sync Group** blade also opens.</span></span>

2.  <span data-ttu-id="22d32-126">Na hello **vytvořit skupinu synchronizace dat** okně hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="22d32-126">On hello **Create Data Sync Group** blade, do hello following things:</span></span>

    1.  <span data-ttu-id="22d32-127">V hello **název skupiny synchronizace** pole, zadejte název nové skupiny synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-127">In hello **Sync Group Name** field, enter a name for hello new sync group.</span></span>

    2.  <span data-ttu-id="22d32-128">V hello **Metadata databáze Sync** zvolte, zda toocreate novou databázi (doporučeno) nebo toouse existující databáze.</span><span class="sxs-lookup"><span data-stu-id="22d32-128">In hello **Sync Metadata Database** section, choose whether toocreate a new database (recommended) or toouse an existing database.</span></span>

        > [!NOTE]
        > <span data-ttu-id="22d32-129">Společnost Microsoft doporučuje, abyste vytvořili novou prázdnou databázi toouse jako hello Metadata databáze Sync.</span><span class="sxs-lookup"><span data-stu-id="22d32-129">Microsoft recommends that you create a new, empty database toouse as hello Sync Metadata Database.</span></span> <span data-ttu-id="22d32-130">Synchronizaci dat vytváří tabulky v této databázi a spustí Časté úlohy.</span><span class="sxs-lookup"><span data-stu-id="22d32-130">Data Sync creates tables in this database and runs a frequent workload.</span></span> <span data-ttu-id="22d32-131">Tato databáze je automaticky sdílen jako hello databáze Metadata synchronizace pro všechny skupiny synchronizace v hello vybrané oblasti.</span><span class="sxs-lookup"><span data-stu-id="22d32-131">This database is automatically shared as hello Sync Metadata Database for all of your Sync groups in hello selected region.</span></span> <span data-ttu-id="22d32-132">Hello Metadata databáze Sync, jeho název nebo jeho úrovně služby nelze změnit bez vyřadíte.</span><span class="sxs-lookup"><span data-stu-id="22d32-132">You can't change hello Sync Metadata Database, its name, or its service level without dropping it.</span></span>

        <span data-ttu-id="22d32-133">Pokud jste zvolili **novou databázi**, vyberte **vytvořit novou databázi.**</span><span class="sxs-lookup"><span data-stu-id="22d32-133">If you chose **New database**, select **Create new database.**</span></span> <span data-ttu-id="22d32-134">Hello **SQL Database** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-134">hello **SQL Database** blade opens.</span></span> <span data-ttu-id="22d32-135">Na hello **SQL Database** okno, název a nakonfigurujte novou databázi hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-135">On hello **SQL Database** blade, name and configure hello new database.</span></span> <span data-ttu-id="22d32-136">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="22d32-136">Then select **OK**.</span></span>

        <span data-ttu-id="22d32-137">Pokud jste zvolili **použít existující databázi**, vyberte databázi hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="22d32-137">If you chose **Use existing database**, select hello database from hello list.</span></span>

    3.  <span data-ttu-id="22d32-138">V hello **automatické synchronizace** část, vyberte nejdřív **na** nebo **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="22d32-138">In hello **Automatic Sync** section, first select **On** or **Off**.</span></span>

        <span data-ttu-id="22d32-139">Pokud jste zvolili **na**, v hello **frekvence synchronizace** , zadejte číslo a vyberte sekund, minut, hodin nebo dnů.</span><span class="sxs-lookup"><span data-stu-id="22d32-139">If you chose **On**, in hello **Sync Frequency** section, enter a number and select Seconds, Minutes, Hours, or Days.</span></span>

        ![Zadejte četnost synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  <span data-ttu-id="22d32-141">V hello **řešení konfliktů** vyberte "Rozbočovače wins" nebo "Člen wins."</span><span class="sxs-lookup"><span data-stu-id="22d32-141">In hello **Conflict Resolution** section, select "Hub wins" or "Member wins."</span></span>

        ![Zadejte způsob řešení konfliktů](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  <span data-ttu-id="22d32-143">Vyberte **OK** a počkejte hello nové synchronizace skupiny toobe vytvoření a nasazení.</span><span class="sxs-lookup"><span data-stu-id="22d32-143">Select **OK** and wait for hello new sync group toobe created and deployed.</span></span>

## <a name="step-2---add-sync-members"></a><span data-ttu-id="22d32-144">Krok 2 – přidání členů synchronizace</span><span class="sxs-lookup"><span data-stu-id="22d32-144">Step 2 - Add sync members</span></span>

<span data-ttu-id="22d32-145">Po hello nové synchronizace skupiny se vytvoří a nasadí, krok 2, **přidat členy synchronizace**, je označený na hello **nové synchronizace skupiny** okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-145">After hello new sync group is created and deployed, Step 2, **Add sync members**, is highlighted in hello **New sync group** blade.</span></span>

<span data-ttu-id="22d32-146">V hello **rozbočovače databáze** zadejte hello existující přihlašovací údaje pro hello databáze SQL server, na které hello je umístěna databáze rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="22d32-146">In hello **Hub Database** section, enter hello existing credentials for hello SQL Database server on which hello hub database is located.</span></span> <span data-ttu-id="22d32-147">Nezadávejte *nové* přihlašovacích údajů v této části.</span><span class="sxs-lookup"><span data-stu-id="22d32-147">Don't enter *new* credentials in this section.</span></span>

![Centrum databáze byla přidána toosync skupiny](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a><span data-ttu-id="22d32-149">Přidat k Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="22d32-149">Add an Azure SQL Database</span></span>

<span data-ttu-id="22d32-150">V hello **databázi člena** část, volitelně přidat skupinu Azure SQL Database toohello synchronizaci výběrem **přidat databázi Azure**.</span><span class="sxs-lookup"><span data-stu-id="22d32-150">In hello **Member Database** section, optionally add an Azure SQL Database toohello sync group by selecting **Add an Azure Database**.</span></span> <span data-ttu-id="22d32-151">Hello **konfigurace databáze Azure** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-151">hello **Configure Azure Database** blade opens.</span></span>

<span data-ttu-id="22d32-152">Na hello **konfigurace databáze Azure** okně hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="22d32-152">On hello **Configure Azure Database** blade, do hello following things:</span></span>

1.  <span data-ttu-id="22d32-153">V hello **název člena synchronizace** pole, zadejte název nového člena synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-153">In hello **Sync Member Name** field, provide a name for hello new sync member.</span></span> <span data-ttu-id="22d32-154">Tento název se liší od názvu hello samotná databáze hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-154">This name is distinct from hello name of hello database itself.</span></span>

2.  <span data-ttu-id="22d32-155">V hello **předplatné** pole, vyberte hello přidružené předplatné Azure pro účely fakturace.</span><span class="sxs-lookup"><span data-stu-id="22d32-155">In hello **Subscription** field, select hello associated Azure subscription for billing purposes.</span></span>

3.  <span data-ttu-id="22d32-156">V hello **Azure SQL Server** hello pole, vyberte existující databázový server SQL.</span><span class="sxs-lookup"><span data-stu-id="22d32-156">In hello **Azure SQL Server** field, select hello existing SQL database server.</span></span>

4.  <span data-ttu-id="22d32-157">V hello **Azure SQL Database** hello pole, vyberte existující databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="22d32-157">In hello **Azure SQL Database** field, select hello existing SQL database.</span></span>

5.  <span data-ttu-id="22d32-158">V hello **synchronizace pokynů** vyberte obousměrné synchronizace, toohello rozbočovače, nebo z hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="22d32-158">In hello **Sync Directions** field, select Bi-directional Sync, toohello Hub, or From hello Hub.</span></span>

    ![Přidání nového člena synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  <span data-ttu-id="22d32-160">V hello **uživatelské jméno** a **heslo** pole, zadejte hello existující přihlašovací údaje pro hello databáze SQL server, na které hello je umístěna databáze člena.</span><span class="sxs-lookup"><span data-stu-id="22d32-160">In hello **Username** and **Password** fields, enter hello existing credentials for hello SQL Database server on which hello member database is located.</span></span> <span data-ttu-id="22d32-161">Nezadávejte *nové* přihlašovacích údajů v této části.</span><span class="sxs-lookup"><span data-stu-id="22d32-161">Don't enter *new* credentials in this section.</span></span>

7.  <span data-ttu-id="22d32-162">Vyberte **OK** a počkejte hello nové synchronizace člen toobe vytvoření a nasazení.</span><span class="sxs-lookup"><span data-stu-id="22d32-162">Select **OK** and wait for hello new sync member toobe created and deployed.</span></span>

    ![Byl přidán nový člen synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a><span data-ttu-id="22d32-164">Přidat místní databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="22d32-164">Add an on-premises SQL Server database</span></span>

<span data-ttu-id="22d32-165">V hello **databázi člena** část, volitelně přidat skupinu místní toohello systému SQL Server synchronizace výběrem **přidat místní databázi**.</span><span class="sxs-lookup"><span data-stu-id="22d32-165">In hello **Member Database** section, optionally add an on-premises SQL Server toohello sync group by selecting **Add an On-Premises Database**.</span></span> <span data-ttu-id="22d32-166">Hello **nakonfigurovat místní** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-166">hello **Configure On-Premises** blade opens.</span></span>

<span data-ttu-id="22d32-167">Na hello **nakonfigurovat místní** okně hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="22d32-167">On hello **Configure On-Premises** blade, do hello following things:</span></span>

1.  <span data-ttu-id="22d32-168">Vyberte **zvolte hello brány agenta synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="22d32-168">Select **Choose hello Sync Agent Gateway**.</span></span> <span data-ttu-id="22d32-169">Hello **vyberte agenta synchronizace** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-169">hello **Select Sync Agent** blade opens.</span></span>

    ![Zvolte bránu agenta synchronizace hello](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  <span data-ttu-id="22d32-171">Na hello **zvolte hello brány agenta synchronizace** okně zvolte zda toouse existujícího agenta nebo vytvořte nový agent.</span><span class="sxs-lookup"><span data-stu-id="22d32-171">On hello **Choose hello Sync Agent Gateway** blade, choose whether toouse an existing agent or create a new agent.</span></span>

    <span data-ttu-id="22d32-172">Pokud jste zvolili **existující agenty**, vyberte hello existujícího agenta ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-172">If you chose **Existing agents**, select hello existing agent from hello list.</span></span>

    <span data-ttu-id="22d32-173">Pokud jste zvolili **vytvořit nový agent**, hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="22d32-173">If you chose **Create a new agent**, do hello following things:</span></span>

    1.  <span data-ttu-id="22d32-174">Stáhnout klientský software agenta synchronizace, hello z uvedeného odkazu hello a nainstalujte ji na hello počítači, kde se nachází hello systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="22d32-174">Download hello client sync agent software from hello link provided and install it on hello computer where hello SQL Server is located.</span></span>
 
        > [!IMPORTANT]
        > <span data-ttu-id="22d32-175">Máte tooopen odchozí port TCP 1433 v hello brány firewall toolet hello Klientský agent komunikovat se serverem hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-175">You have tooopen outbound TCP port 1433 in hello firewall toolet hello client agent communicate with hello server.</span></span>


    2.  <span data-ttu-id="22d32-176">Zadejte název pro agenta hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-176">Enter a name for hello agent.</span></span>

    3.  <span data-ttu-id="22d32-177">Vyberte **vytvořit a vygenerovat klíč**.</span><span class="sxs-lookup"><span data-stu-id="22d32-177">Select **Create and Generate Key**.</span></span>

    4.  <span data-ttu-id="22d32-178">Kopírování hello agenta klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="22d32-178">Copy hello agent key toohello clipboard.</span></span>
        
        ![Vytvoření nového agenta synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  <span data-ttu-id="22d32-180">Vyberte **OK** tooclose hello **vyberte agenta synchronizace** okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-180">Select **OK** tooclose hello **Select Sync Agent** blade.</span></span>

    6.  <span data-ttu-id="22d32-181">Na počítači systému SQL Server hello vyhledání a spuštění aplikace hello synchronizace agenta klienta.</span><span class="sxs-lookup"><span data-stu-id="22d32-181">On hello SQL Server computer, locate and run hello Client Sync Agent app.</span></span>

        ![synchronizace dat Hello klientskou aplikaci pro agenta](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  <span data-ttu-id="22d32-183">V aplikaci agenta hello synchronizace, vyberte **odeslání klíč agenta**.</span><span class="sxs-lookup"><span data-stu-id="22d32-183">In hello sync agent app, select **Submit Agent Key**.</span></span> <span data-ttu-id="22d32-184">Hello **konfigurace databáze synchronizace metadat** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-184">hello **Sync Metadata Database Configuration** dialog box opens.</span></span>

    8.  <span data-ttu-id="22d32-185">V hello **konfigurace databáze synchronizace metadat** dialogové okno, vložte klíč agenta hello zkopírovaných z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22d32-185">In hello **Sync Metadata Database Configuration** dialog box, paste in hello agent key copied from hello Azure portal.</span></span> <span data-ttu-id="22d32-186">Zadejte také hello existující pověření pro server hello Azure SQL Database, na které hello je umístěna databáze metadat.</span><span class="sxs-lookup"><span data-stu-id="22d32-186">Also provide hello existing credentials for hello Azure SQL Database server on which hello metadata database is located.</span></span> <span data-ttu-id="22d32-187">(Pokud jste vytvořili novou databázi metadat, tato databáze je v hello stejný server jako databáze rozbočovače hello.) Vyberte **OK** a počkejte toofinish konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-187">(If you created a new metadata database, this database is on hello same server as hello hub database.) Select **OK** and wait for hello configuration toofinish.</span></span>

        ![Zadejte pověření agenta klíč a server hello](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   <span data-ttu-id="22d32-189">Pokud dojde k chybě brány firewall v tomto okamžiku, máte toocreate pravidlo brány firewall na Azure tooallow příchozí provoz z počítače serveru SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-189">If you get a firewall error at this point, you have toocreate a firewall rule on Azure tooallow incoming traffic from hello SQL Server computer.</span></span> <span data-ttu-id="22d32-190">Můžete vytvořit pravidlo hello ručně hello portálu, ale možná bude snazší toocreate je v serveru SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="22d32-190">You can create hello rule manually in hello portal, but you may find it easier toocreate it in SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="22d32-191">V aplikaci SSMS zkuste tooconnect toohello rozbočovače databáze v Azure.</span><span class="sxs-lookup"><span data-stu-id="22d32-191">In SSMS, try tooconnect toohello hub database on Azure.</span></span> <span data-ttu-id="22d32-192">Zadejte jeho název jako \<hub_database_name\>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="22d32-192">Enter its name as \<hub_database_name\>.database.windows.net.</span></span> <span data-ttu-id="22d32-193">Postupujte podle kroků hello v hello dialogové okno pole tooconfigure hello brány Azure pravidlo.</span><span class="sxs-lookup"><span data-stu-id="22d32-193">Follow hello steps in hello dialog box tooconfigure hello Azure firewall rule.</span></span> <span data-ttu-id="22d32-194">Pak se vraťte toohello agenta synchronizace klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="22d32-194">Then return toohello Client Sync Agent app.</span></span>

    9.  <span data-ttu-id="22d32-195">V aplikaci hello klientský Agent synchronizace, klikněte na tlačítko **zaregistrovat** tooregister databázi systému SQL Server s agentem hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-195">In hello Client Sync Agent app, click **Register** tooregister a SQL Server database with hello agent.</span></span> <span data-ttu-id="22d32-196">Hello **konfigurace serveru SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-196">hello **SQL Server Configuration** dialog box opens.</span></span>

        ![Přidejte a nakonfigurujte databázi systému SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. <span data-ttu-id="22d32-198">V hello **konfigurace serveru SQL Server** dialogovém okně vyberte zda tooconnect pomocí ověřování systému SQL Server nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="22d32-198">In hello **SQL Server Configuration** dialog box, choose whether tooconnect by using SQL Server authentication or Windows authentication.</span></span> <span data-ttu-id="22d32-199">Pokud jste vybrali ověřování SQL serveru, zadejte existující přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-199">If you chose SQL Server authentication, enter hello existing credentials.</span></span> <span data-ttu-id="22d32-200">Zadejte název serveru SQL Server hello a hello název hello databáze, které chcete toosync.</span><span class="sxs-lookup"><span data-stu-id="22d32-200">Provide hello SQL Server name and hello name of hello database that you want toosync.</span></span> <span data-ttu-id="22d32-201">Vyberte **testovací připojení** tootest nastavení.</span><span class="sxs-lookup"><span data-stu-id="22d32-201">Select **Test connection** tootest your settings.</span></span> <span data-ttu-id="22d32-202">Potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="22d32-202">Then select **Save**.</span></span> <span data-ttu-id="22d32-203">Hello registrované databáze se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-203">hello registered database appears in hello list.</span></span>

        ![Databáze systému SQL Server je teď zaregistrovaný.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. <span data-ttu-id="22d32-205">Teď můžete zavřít hello agenta synchronizace klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="22d32-205">You can now close hello Client Sync Agent app.</span></span>

    12. <span data-ttu-id="22d32-206">Hello portálu na hello **nakonfigurovat místní** vyberte **vyberte hello databáze.**</span><span class="sxs-lookup"><span data-stu-id="22d32-206">In hello portal, on hello **Configure On-Premises** blade, select **Select hello Database.**</span></span> <span data-ttu-id="22d32-207">Hello **vybrat databázi** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-207">hello **Select Database** blade opens.</span></span>

    13. <span data-ttu-id="22d32-208">Na hello **vybrat databázi** okno, v hello **název člena synchronizace** pole, zadejte název nového člena synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-208">On hello **Select Database** blade, in hello **Sync Member Name** field, provide a name for hello new sync member.</span></span> <span data-ttu-id="22d32-209">Tento název se liší od názvu hello samotná databáze hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-209">This name is distinct from hello name of hello database itself.</span></span> <span data-ttu-id="22d32-210">Vyberte v seznamu hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="22d32-210">Select hello database from hello list.</span></span> <span data-ttu-id="22d32-211">V hello **synchronizace pokynů** vyberte obousměrné synchronizace, toohello rozbočovače, nebo z hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="22d32-211">In hello **Sync Directions** field, select Bi-directional Sync, toohello Hub, or From hello Hub.</span></span>

        ![Vyberte hello v místní databázi](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. <span data-ttu-id="22d32-213">Vyberte **OK** tooclose hello **vybrat databázi** okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-213">Select **OK** tooclose hello **Select Database** blade.</span></span> <span data-ttu-id="22d32-214">Potom vyberte **OK** tooclose hello **nakonfigurovat místní** okno a počkejte hello nové synchronizace člen toobe vytvoření a nasazení.</span><span class="sxs-lookup"><span data-stu-id="22d32-214">Then select **OK** tooclose hello **Configure On-Premises** blade and wait for hello new sync member toobe created and deployed.</span></span> <span data-ttu-id="22d32-215">Nakonec klikněte na **OK** tooclose hello **vyberte synchronizace členů** okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-215">Finally, click **OK** tooclose hello **Select sync members** blade.</span></span>

        ![V místní databázi přidat toosync skupiny](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  <span data-ttu-id="22d32-217">tooconnect tooSQL synchronizaci dat a hello místní agent, přidejte vaše uživatelská role toohello název `DataSync_Executor`.</span><span class="sxs-lookup"><span data-stu-id="22d32-217">tooconnect tooSQL Data Sync and hello local agent, add your user name toohello role `DataSync_Executor`.</span></span> <span data-ttu-id="22d32-218">Synchronizaci dat vytvoří této role v instanci systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-218">Data Sync creates this role on hello SQL Server instance.</span></span>

## <a name="step-3---configure-sync-group"></a><span data-ttu-id="22d32-219">Krok 3: Konfigurace skupiny synchronizace</span><span class="sxs-lookup"><span data-stu-id="22d32-219">Step 3 - Configure sync group</span></span>

<span data-ttu-id="22d32-220">Po hello nové synchronizace členů skupiny se vytváří a nasazují, krok 3 **skupiny synchronizace konfigurace**, je označený na hello **nové synchronizace skupiny** okno.</span><span class="sxs-lookup"><span data-stu-id="22d32-220">After hello new sync group members are created and deployed, Step 3, **Configure sync group**, is highlighted in hello **New sync group** blade.</span></span>

1.  <span data-ttu-id="22d32-221">Na hello **tabulky** okně, vyberte databázi z hello seznam synchronizace členy skupiny a potom vyberte **aktualizace schématu**.</span><span class="sxs-lookup"><span data-stu-id="22d32-221">On hello **Tables** blade, select a database from hello list of sync group members, and then select **Refresh schema**.</span></span>

2.  <span data-ttu-id="22d32-222">Hello seznam dostupných tabulek vyberte hello tabulek, které chcete toosync.</span><span class="sxs-lookup"><span data-stu-id="22d32-222">From hello list of available tables, select hello tables that you want toosync.</span></span>

    ![Vyberte tabulky toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  <span data-ttu-id="22d32-224">Ve výchozím nastavení jsou vybrané všechny sloupce v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="22d32-224">By default, all columns in hello table are selected.</span></span> <span data-ttu-id="22d32-225">Pokud nechcete, aby toosync všechny sloupce hello, zakažte hello zaškrtávací políčko pro hello sloupce, které nechcete toosync.</span><span class="sxs-lookup"><span data-stu-id="22d32-225">If you don't want toosync all hello columns, disable hello checkbox for hello columns that you don't want toosync.</span></span> <span data-ttu-id="22d32-226">Být vybrána tooleave hello sloupec primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="22d32-226">Be sure tooleave hello primary key column selected.</span></span>

    ![Vyberte pole toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  <span data-ttu-id="22d32-228">Nakonec vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="22d32-228">Finally, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22d32-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22d32-229">Next steps</span></span>
<span data-ttu-id="22d32-230">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="22d32-230">Congratulations.</span></span> <span data-ttu-id="22d32-231">Vytvořili jste skupinu synchronizace, která obsahuje instance databáze SQL a databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="22d32-231">You have created a sync group that includes both a SQL Database instance and a SQL Server database.</span></span>

<span data-ttu-id="22d32-232">Další informace o SQL Database a synchronizaci dat SQL najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="22d32-232">For more info about SQL Database and SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="22d32-233">Stáhnout hello úplnou synchronizaci dat SQL technická dokumentace</span><span class="sxs-lookup"><span data-stu-id="22d32-233">Download hello complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [<span data-ttu-id="22d32-234">Stáhněte si dokumentaci k REST API služby SQL Data synchronizace hello</span><span class="sxs-lookup"><span data-stu-id="22d32-234">Download hello SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [<span data-ttu-id="22d32-235">Databáze SQL – přehled</span><span class="sxs-lookup"><span data-stu-id="22d32-235">SQL Database Overview</span></span>](sql-database-technical-overview.md)
-   [<span data-ttu-id="22d32-236">Správa životního cyklu databáze</span><span class="sxs-lookup"><span data-stu-id="22d32-236">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
