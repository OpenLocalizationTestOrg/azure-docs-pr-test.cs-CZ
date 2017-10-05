---
title: "Začínáme s Azure synchronizaci dat SQL (Preview) | Microsoft Docs"
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
ms.openlocfilehash: 2d0f9d7f32ad79f49d58165d734b9df4af862835
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a><span data-ttu-id="2b716-103">Začínáme s synchronizaci dat Azure SQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="2b716-103">Getting Started with Azure SQL Data Sync (Preview)</span></span>
<span data-ttu-id="2b716-104">V tomto kurzu zjistěte, jak nastavit synchronizaci dat SQL Azure tak, že vytvoříte skupinu hybridních synchronizace, která obsahuje instance Azure SQL Database a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b716-104">In this tutorial, you learn how to set up Azure SQL Data Sync by creating a hybrid sync group that contains both Azure SQL Database and SQL Server instances.</span></span> <span data-ttu-id="2b716-105">Do nové skupiny synchronizace plně konfigurována a synchronizuje podle plánu, který nastavíte.</span><span class="sxs-lookup"><span data-stu-id="2b716-105">The new sync group is fully configured and synchronizes on the schedule you set.</span></span>

<span data-ttu-id="2b716-106">Tento kurz předpokládá, že máte alespoň zkušenosti s SQL Database a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b716-106">This tutorial assumes that you have at least some prior experience with SQL Database and with SQL Server.</span></span> 

<span data-ttu-id="2b716-107">Přehled synchronizaci dat SQL najdete v tématu [synchronizaci dat](sql-database-sync-data.md).</span><span class="sxs-lookup"><span data-stu-id="2b716-107">For an overview of SQL Data Sync, see [Sync data](sql-database-sync-data.md).</span></span>

<span data-ttu-id="2b716-108">Pro dokončení příklady prostředí PowerShell, které ukazují, jak nakonfigurovat synchronizaci dat SQL, najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="2b716-108">For complete PowerShell examples that show how to configure SQL Data Sync, see the following articles:</span></span>
-   [<span data-ttu-id="2b716-109">Pomocí prostředí PowerShell k synchronizaci mezi více databází Azure SQL</span><span class="sxs-lookup"><span data-stu-id="2b716-109">Use PowerShell to sync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
-   [<span data-ttu-id="2b716-110">Synchronizace mezi databáze SQL Azure a místní databáze SQL serveru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2b716-110">Use PowerShell to sync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> <span data-ttu-id="2b716-111">Dokončení technická dokumentace pro synchronizaci dat SQL Azure, byly dříve umístěny na webu MSDN, nastavit je jako. Dokument PDF.</span><span class="sxs-lookup"><span data-stu-id="2b716-111">The complete technical documentation set for Azure SQL Data Sync, formerly located on MSDN, is available as a .PDF document.</span></span> <span data-ttu-id="2b716-112">Stažení [zde](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span><span class="sxs-lookup"><span data-stu-id="2b716-112">Download it [here](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span></span>

## <a name="step-1---create-sync-group"></a><span data-ttu-id="2b716-113">Krok 1 – Vytvoření skupiny synchronizace</span><span class="sxs-lookup"><span data-stu-id="2b716-113">Step 1 - Create sync group</span></span>

### <a name="locate-the-data-sync-settings"></a><span data-ttu-id="2b716-114">Najít nastavení synchronizace dat</span><span class="sxs-lookup"><span data-stu-id="2b716-114">Locate the Data Sync settings</span></span>

1.  <span data-ttu-id="2b716-115">V prohlížeči přejděte na portál Azure.</span><span class="sxs-lookup"><span data-stu-id="2b716-115">In your browser, navigate to the Azure portal.</span></span>

2.  <span data-ttu-id="2b716-116">Na portálu vyhledejte vaší databáze SQL z řídicího panelu nebo na ikonu databází SQL na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="2b716-116">In the portal, locate your SQL databases from your Dashboard or from the SQL Databases icon on the toolbar.</span></span>

    ![Seznam databází Azure SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  <span data-ttu-id="2b716-118">Na **databází SQL** okně vyberte existující databázi SQL, který chcete použít jako databázi rozbočovače pro synchronizaci dat. Otevře se okno databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="2b716-118">On the **SQL databases** blade, select the existing SQL database that you want to use as the hub database for Data Sync. The SQL database blade opens.</span></span>

4.  <span data-ttu-id="2b716-119">V okně databáze SQL pro vybranou databázi, vyberte **synchronizace do jiné databáze**.</span><span class="sxs-lookup"><span data-stu-id="2b716-119">On the SQL database blade for the selected database, select **Sync to other databases**.</span></span> <span data-ttu-id="2b716-120">Otevře se okno synchronizaci dat.</span><span class="sxs-lookup"><span data-stu-id="2b716-120">The Data Sync blade opens.</span></span>

    ![Synchronizaci další možnost databáze](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a><span data-ttu-id="2b716-122">Vytvořit novou skupinu synchronizace</span><span class="sxs-lookup"><span data-stu-id="2b716-122">Create a new Sync Group</span></span>

1.  <span data-ttu-id="2b716-123">V okně synchronizaci dat vyberte **nové synchronizace skupiny**.</span><span class="sxs-lookup"><span data-stu-id="2b716-123">On the Data Sync blade, select **New Sync Group**.</span></span> <span data-ttu-id="2b716-124">**Nové synchronizace skupiny** otevře se okno s krokem 1 **vytvořit skupinu synchronizace**, zvýrazněné.</span><span class="sxs-lookup"><span data-stu-id="2b716-124">The **New sync group** blade opens with Step 1, **Create sync group**, highlighted.</span></span> <span data-ttu-id="2b716-125">**Vytvořit skupinu synchronizace dat** také otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-125">The **Create Data Sync Group** blade also opens.</span></span>

2.  <span data-ttu-id="2b716-126">Na **vytvořit skupinu synchronizace dat** okně provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="2b716-126">On the **Create Data Sync Group** blade, do the following things:</span></span>

    1.  <span data-ttu-id="2b716-127">V **název skupiny synchronizace** pole, zadejte název nové skupiny synchronizace.</span><span class="sxs-lookup"><span data-stu-id="2b716-127">In the **Sync Group Name** field, enter a name for the new sync group.</span></span>

    2.  <span data-ttu-id="2b716-128">V **Metadata databáze Sync** zvolte, zda chcete vytvořit novou databázi (doporučeno) nebo použijte existující databázi.</span><span class="sxs-lookup"><span data-stu-id="2b716-128">In the **Sync Metadata Database** section, choose whether to create a new database (recommended) or to use an existing database.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2b716-129">Společnost Microsoft doporučuje, že můžete vytvořit nový, prázdný databázi chcete použít jako databázi synchronizace metadat.</span><span class="sxs-lookup"><span data-stu-id="2b716-129">Microsoft recommends that you create a new, empty database to use as the Sync Metadata Database.</span></span> <span data-ttu-id="2b716-130">Synchronizaci dat vytváří tabulky v této databázi a spustí Časté úlohy.</span><span class="sxs-lookup"><span data-stu-id="2b716-130">Data Sync creates tables in this database and runs a frequent workload.</span></span> <span data-ttu-id="2b716-131">Tato databáze je automaticky sdílen jako databázi Metadata synchronizace pro všechny skupiny synchronizace ve vybrané oblasti.</span><span class="sxs-lookup"><span data-stu-id="2b716-131">This database is automatically shared as the Sync Metadata Database for all of your Sync groups in the selected region.</span></span> <span data-ttu-id="2b716-132">Metadata databáze Sync, jeho název nebo jeho úrovně služby nelze změnit bez vyřadíte.</span><span class="sxs-lookup"><span data-stu-id="2b716-132">You can't change the Sync Metadata Database, its name, or its service level without dropping it.</span></span>

        <span data-ttu-id="2b716-133">Pokud jste zvolili **novou databázi**, vyberte **vytvořit novou databázi.**</span><span class="sxs-lookup"><span data-stu-id="2b716-133">If you chose **New database**, select **Create new database.**</span></span> <span data-ttu-id="2b716-134">**SQL Database** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-134">The **SQL Database** blade opens.</span></span> <span data-ttu-id="2b716-135">Na **SQL Database** okno, název a nakonfigurujte novou databázi.</span><span class="sxs-lookup"><span data-stu-id="2b716-135">On the **SQL Database** blade, name and configure the new database.</span></span> <span data-ttu-id="2b716-136">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="2b716-136">Then select **OK**.</span></span>

        <span data-ttu-id="2b716-137">Pokud jste zvolili **použít existující databázi**, vyberte databázi ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="2b716-137">If you chose **Use existing database**, select the database from the list.</span></span>

    3.  <span data-ttu-id="2b716-138">V **automatické synchronizace** část, vyberte nejdřív **na** nebo **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="2b716-138">In the **Automatic Sync** section, first select **On** or **Off**.</span></span>

        <span data-ttu-id="2b716-139">Pokud jste zvolili **na**v **frekvence synchronizace** , zadejte číslo a vyberte sekund, minut, hodin nebo dnů.</span><span class="sxs-lookup"><span data-stu-id="2b716-139">If you chose **On**, in the **Sync Frequency** section, enter a number and select Seconds, Minutes, Hours, or Days.</span></span>

        ![Zadejte četnost synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  <span data-ttu-id="2b716-141">V **řešení konfliktů** vyberte "Rozbočovače wins" nebo "Člen wins."</span><span class="sxs-lookup"><span data-stu-id="2b716-141">In the **Conflict Resolution** section, select "Hub wins" or "Member wins."</span></span>

        ![Zadejte způsob řešení konfliktů](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  <span data-ttu-id="2b716-143">Vyberte **OK** a počkejte na novou skupinu synchronizace vytvoření a nasazení.</span><span class="sxs-lookup"><span data-stu-id="2b716-143">Select **OK** and wait for the new sync group to be created and deployed.</span></span>

## <a name="step-2---add-sync-members"></a><span data-ttu-id="2b716-144">Krok 2 – přidání členů synchronizace</span><span class="sxs-lookup"><span data-stu-id="2b716-144">Step 2 - Add sync members</span></span>

<span data-ttu-id="2b716-145">Po nové synchronizace skupiny se vytvoří a nasadí, krok 2, **přidat členy synchronizace**, je označený na **nové synchronizace skupiny** okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-145">After the new sync group is created and deployed, Step 2, **Add sync members**, is highlighted in the **New sync group** blade.</span></span>

<span data-ttu-id="2b716-146">V **rozbočovače databáze** zadejte existující přihlašovací údaje pro databázi SQL server, na kterém je umístěna databáze rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2b716-146">In the **Hub Database** section, enter the existing credentials for the SQL Database server on which the hub database is located.</span></span> <span data-ttu-id="2b716-147">Nezadávejte *nové* přihlašovacích údajů v této části.</span><span class="sxs-lookup"><span data-stu-id="2b716-147">Don't enter *new* credentials in this section.</span></span>

![Rozbočovače databáze byl přidán do skupiny synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a><span data-ttu-id="2b716-149">Přidat k Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2b716-149">Add an Azure SQL Database</span></span>

<span data-ttu-id="2b716-150">V **databázi člena** část, volitelně přidejte databáze SQL Azure do skupiny synchronizace výběrem **přidat databázi Azure**.</span><span class="sxs-lookup"><span data-stu-id="2b716-150">In the **Member Database** section, optionally add an Azure SQL Database to the sync group by selecting **Add an Azure Database**.</span></span> <span data-ttu-id="2b716-151">**Konfigurace databáze Azure** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-151">The **Configure Azure Database** blade opens.</span></span>

<span data-ttu-id="2b716-152">Na **konfigurace databáze Azure** okně provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="2b716-152">On the **Configure Azure Database** blade, do the following things:</span></span>

1.  <span data-ttu-id="2b716-153">V **název člena synchronizace** pole, zadejte název nového člena synchronizace.</span><span class="sxs-lookup"><span data-stu-id="2b716-153">In the **Sync Member Name** field, provide a name for the new sync member.</span></span> <span data-ttu-id="2b716-154">Tento název se liší od názvu samotná databáze.</span><span class="sxs-lookup"><span data-stu-id="2b716-154">This name is distinct from the name of the database itself.</span></span>

2.  <span data-ttu-id="2b716-155">V **předplatné** pole, vyberte přidružené předplatné Azure pro účely fakturace.</span><span class="sxs-lookup"><span data-stu-id="2b716-155">In the **Subscription** field, select the associated Azure subscription for billing purposes.</span></span>

3.  <span data-ttu-id="2b716-156">V **Azure SQL Server** pole, vyberte existující databázový server SQL.</span><span class="sxs-lookup"><span data-stu-id="2b716-156">In the **Azure SQL Server** field, select the existing SQL database server.</span></span>

4.  <span data-ttu-id="2b716-157">V **Azure SQL Database** pole, vyberte existující databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="2b716-157">In the **Azure SQL Database** field, select the existing SQL database.</span></span>

5.  <span data-ttu-id="2b716-158">V **synchronizace pokynů** pole, vyberte obousměrné synchronizace, do rozbočovače nebo z rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2b716-158">In the **Sync Directions** field, select Bi-directional Sync, To the Hub, or From the Hub.</span></span>

    ![Přidání nového člena synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  <span data-ttu-id="2b716-160">V **uživatelské jméno** a **heslo** pole, zadejte existující přihlašovací údaje pro databázi SQL server, na kterém je umístěna databáze člena.</span><span class="sxs-lookup"><span data-stu-id="2b716-160">In the **Username** and **Password** fields, enter the existing credentials for the SQL Database server on which the member database is located.</span></span> <span data-ttu-id="2b716-161">Nezadávejte *nové* přihlašovacích údajů v této části.</span><span class="sxs-lookup"><span data-stu-id="2b716-161">Don't enter *new* credentials in this section.</span></span>

7.  <span data-ttu-id="2b716-162">Vyberte **OK** a počkejte na vytvoření a nasazení nového člena synchronizace.</span><span class="sxs-lookup"><span data-stu-id="2b716-162">Select **OK** and wait for the new sync member to be created and deployed.</span></span>

    ![Byl přidán nový člen synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a><span data-ttu-id="2b716-164">Přidat místní databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="2b716-164">Add an on-premises SQL Server database</span></span>

<span data-ttu-id="2b716-165">V **databázi člena** část, volitelně přidat místní SQL Server do skupiny synchronizace výběrem **přidat místní databázi**.</span><span class="sxs-lookup"><span data-stu-id="2b716-165">In the **Member Database** section, optionally add an on-premises SQL Server to the sync group by selecting **Add an On-Premises Database**.</span></span> <span data-ttu-id="2b716-166">**Nakonfigurovat místní** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-166">The **Configure On-Premises** blade opens.</span></span>

<span data-ttu-id="2b716-167">Na **nakonfigurovat místní** okně provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="2b716-167">On the **Configure On-Premises** blade, do the following things:</span></span>

1.  <span data-ttu-id="2b716-168">Vyberte **zvolte bránu agenta synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="2b716-168">Select **Choose the Sync Agent Gateway**.</span></span> <span data-ttu-id="2b716-169">**Vyberte agenta synchronizace** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-169">The **Select Sync Agent** blade opens.</span></span>

    ![Zvolte bránu agenta synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  <span data-ttu-id="2b716-171">Na **zvolte bránu agenta synchronizace** okně vyberte, zda chcete použít existující agenta nebo vytvořit nový agent.</span><span class="sxs-lookup"><span data-stu-id="2b716-171">On the **Choose the Sync Agent Gateway** blade, choose whether to use an existing agent or create a new agent.</span></span>

    <span data-ttu-id="2b716-172">Pokud jste zvolili **existující agenty**, vyberte existující agenta ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="2b716-172">If you chose **Existing agents**, select the existing agent from the list.</span></span>

    <span data-ttu-id="2b716-173">Pokud jste zvolili **vytvořit nový agent**, provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="2b716-173">If you chose **Create a new agent**, do the following things:</span></span>

    1.  <span data-ttu-id="2b716-174">Stáhněte si klientský software agenta synchronizace z uvedeného odkazu a nainstalujte ji na počítači, kde je umístěný SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b716-174">Download the client sync agent software from the link provided and install it on the computer where the SQL Server is located.</span></span>
 
        > [!IMPORTANT]
        > <span data-ttu-id="2b716-175">Budete muset otevřít odchozí port TCP 1433 v bráně firewall umožníte komunikaci se serverem pro agenta klienta.</span><span class="sxs-lookup"><span data-stu-id="2b716-175">You have to open outbound TCP port 1433 in the firewall to let the client agent communicate with the server.</span></span>


    2.  <span data-ttu-id="2b716-176">Zadejte název pro agenta.</span><span class="sxs-lookup"><span data-stu-id="2b716-176">Enter a name for the agent.</span></span>

    3.  <span data-ttu-id="2b716-177">Vyberte **vytvořit a vygenerovat klíč**.</span><span class="sxs-lookup"><span data-stu-id="2b716-177">Select **Create and Generate Key**.</span></span>

    4.  <span data-ttu-id="2b716-178">Klíč agenta zkopírujte do schránky.</span><span class="sxs-lookup"><span data-stu-id="2b716-178">Copy the agent key to the clipboard.</span></span>
        
        ![Vytvoření nového agenta synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  <span data-ttu-id="2b716-180">Vyberte **OK** zavřete **vyberte agenta synchronizace** okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-180">Select **OK** to close the **Select Sync Agent** blade.</span></span>

    6.  <span data-ttu-id="2b716-181">Na počítači serveru SQL najít a spuštění aplikace synchronizace agenta klienta.</span><span class="sxs-lookup"><span data-stu-id="2b716-181">On the SQL Server computer, locate and run the Client Sync Agent app.</span></span>

        ![Data synchronizovat klientskou aplikaci pro agenta](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  <span data-ttu-id="2b716-183">V aplikaci agenta synchronizace, vyberte **odeslání klíč agenta**.</span><span class="sxs-lookup"><span data-stu-id="2b716-183">In the sync agent app, select **Submit Agent Key**.</span></span> <span data-ttu-id="2b716-184">**Konfigurace databáze synchronizace metadat** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-184">The **Sync Metadata Database Configuration** dialog box opens.</span></span>

    8.  <span data-ttu-id="2b716-185">V **konfigurace databáze synchronizace metadat** dialogové okno, vložte klíč agenta zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2b716-185">In the **Sync Metadata Database Configuration** dialog box, paste in the agent key copied from the Azure portal.</span></span> <span data-ttu-id="2b716-186">Zadejte také existující přihlašovací údaje pro server Azure SQL Database, na kterém je umístěna databáze metadat.</span><span class="sxs-lookup"><span data-stu-id="2b716-186">Also provide the existing credentials for the Azure SQL Database server on which the metadata database is located.</span></span> <span data-ttu-id="2b716-187">(Pokud jste vytvořili novou databázi metadat, na stejném serveru jako databázi centra je tato databáze.) Vyberte **OK** a počkejte na dokončení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2b716-187">(If you created a new metadata database, this database is on the same server as the hub database.) Select **OK** and wait for the configuration to finish.</span></span>

        ![Zadejte přihlašovací údaje pro klíč a server agenta](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   <span data-ttu-id="2b716-189">Pokud dojde k chybě brány firewall v tomto okamžiku, budete muset vytvořit pravidlo brány firewall na platformě Azure, které chcete povolit příchozí přenosy z počítače systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b716-189">If you get a firewall error at this point, you have to create a firewall rule on Azure to allow incoming traffic from the SQL Server computer.</span></span> <span data-ttu-id="2b716-190">Pravidlo můžete vytvořit ručně na portálu, ale může být snadněji vytvořit v serveru SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="2b716-190">You can create the rule manually in the portal, but you may find it easier to create it in SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="2b716-191">V aplikaci SSMS pokuste se připojit k databázi rozbočovače na Azure.</span><span class="sxs-lookup"><span data-stu-id="2b716-191">In SSMS, try to connect to the hub database on Azure.</span></span> <span data-ttu-id="2b716-192">Zadejte jeho název jako \<hub_database_name\>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="2b716-192">Enter its name as \<hub_database_name\>.database.windows.net.</span></span> <span data-ttu-id="2b716-193">Postupujte podle kroků v dialogovém okně nakonfigurovat pravidlo brány firewall Azure.</span><span class="sxs-lookup"><span data-stu-id="2b716-193">Follow the steps in the dialog box to configure the Azure firewall rule.</span></span> <span data-ttu-id="2b716-194">Pak se vraťte do agenta synchronizace klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b716-194">Then return to the Client Sync Agent app.</span></span>

    9.  <span data-ttu-id="2b716-195">V aplikaci agenta synchronizace klienta, klikněte na tlačítko **zaregistrovat** k registraci do databáze SQL serveru s agentem.</span><span class="sxs-lookup"><span data-stu-id="2b716-195">In the Client Sync Agent app, click **Register** to register a SQL Server database with the agent.</span></span> <span data-ttu-id="2b716-196">**Konfigurace serveru SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-196">The **SQL Server Configuration** dialog box opens.</span></span>

        ![Přidejte a nakonfigurujte databázi systému SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. <span data-ttu-id="2b716-198">V **konfigurace serveru SQL Server** dialogovém okně vyberte, jestli se mají připojit pomocí ověřování systému SQL Server nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2b716-198">In the **SQL Server Configuration** dialog box, choose whether to connect by using SQL Server authentication or Windows authentication.</span></span> <span data-ttu-id="2b716-199">Pokud jste vybrali ověřování SQL serveru, zadejte existující přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2b716-199">If you chose SQL Server authentication, enter the existing credentials.</span></span> <span data-ttu-id="2b716-200">Zadejte název serveru SQL Server a název databáze, který chcete synchronizovat. Vyberte **testovací připojení** můžete zkontrolovat nastavení.</span><span class="sxs-lookup"><span data-stu-id="2b716-200">Provide the SQL Server name and the name of the database that you want to sync. Select **Test connection** to test your settings.</span></span> <span data-ttu-id="2b716-201">Potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2b716-201">Then select **Save**.</span></span> <span data-ttu-id="2b716-202">V seznamu se zobrazí registrované databáze.</span><span class="sxs-lookup"><span data-stu-id="2b716-202">The registered database appears in the list.</span></span>

        ![Databáze systému SQL Server je teď zaregistrovaný.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. <span data-ttu-id="2b716-204">Teď můžete zavřít aplikaci synchronizace agenta klienta.</span><span class="sxs-lookup"><span data-stu-id="2b716-204">You can now close the Client Sync Agent app.</span></span>

    12. <span data-ttu-id="2b716-205">Na portálu na **nakonfigurovat místní** vyberte **vyberte databázi.**</span><span class="sxs-lookup"><span data-stu-id="2b716-205">In the portal, on the **Configure On-Premises** blade, select **Select the Database.**</span></span> <span data-ttu-id="2b716-206">**Vybrat databázi** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-206">The **Select Database** blade opens.</span></span>

    13. <span data-ttu-id="2b716-207">Na **vybrat databázi** okno v **název člena synchronizace** pole, zadejte název nového člena synchronizace.</span><span class="sxs-lookup"><span data-stu-id="2b716-207">On the **Select Database** blade, in the **Sync Member Name** field, provide a name for the new sync member.</span></span> <span data-ttu-id="2b716-208">Tento název se liší od názvu samotná databáze.</span><span class="sxs-lookup"><span data-stu-id="2b716-208">This name is distinct from the name of the database itself.</span></span> <span data-ttu-id="2b716-209">Vyberte databázi ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="2b716-209">Select the database from the list.</span></span> <span data-ttu-id="2b716-210">V **synchronizace pokynů** pole, vyberte obousměrné synchronizace, do rozbočovače nebo z rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2b716-210">In the **Sync Directions** field, select Bi-directional Sync, To the Hub, or From the Hub.</span></span>

        ![Vyberte v místní databázi](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. <span data-ttu-id="2b716-212">Vyberte **OK** zavřete **vybrat databázi** okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-212">Select **OK** to close the **Select Database** blade.</span></span> <span data-ttu-id="2b716-213">Potom vyberte **OK** zavřete **nakonfigurovat místní** okno a počkejte na vytvoření a nasazení nového člena synchronizace.</span><span class="sxs-lookup"><span data-stu-id="2b716-213">Then select **OK** to close the **Configure On-Premises** blade and wait for the new sync member to be created and deployed.</span></span> <span data-ttu-id="2b716-214">Nakonec klikněte na **OK** zavřete **vyberte synchronizace členů** okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-214">Finally, click **OK** to close the **Select sync members** blade.</span></span>

        ![V místní databázi přidán do skupiny synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  <span data-ttu-id="2b716-216">Pro připojení k synchronizaci dat SQL a místní agent, přidejte uživatelské jméno k roli `DataSync_Executor`.</span><span class="sxs-lookup"><span data-stu-id="2b716-216">To connect to SQL Data Sync and the local agent, add your user name to the role `DataSync_Executor`.</span></span> <span data-ttu-id="2b716-217">Synchronizaci dat vytvoří tuto roli na instanci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b716-217">Data Sync creates this role on the SQL Server instance.</span></span>

## <a name="step-3---configure-sync-group"></a><span data-ttu-id="2b716-218">Krok 3: Konfigurace skupiny synchronizace</span><span class="sxs-lookup"><span data-stu-id="2b716-218">Step 3 - Configure sync group</span></span>

<span data-ttu-id="2b716-219">Po nové členy skupiny synchronizace se vytváří a nasazují, krok 3 **skupiny synchronizace konfigurace**, je označený na **nové synchronizace skupiny** okno.</span><span class="sxs-lookup"><span data-stu-id="2b716-219">After the new sync group members are created and deployed, Step 3, **Configure sync group**, is highlighted in the **New sync group** blade.</span></span>

1.  <span data-ttu-id="2b716-220">Na **tabulky** okně, vyberte databázi ze seznamu synchronizačních členy skupiny a potom vyberte **aktualizace schématu**.</span><span class="sxs-lookup"><span data-stu-id="2b716-220">On the **Tables** blade, select a database from the list of sync group members, and then select **Refresh schema**.</span></span>

2.  <span data-ttu-id="2b716-221">Ze seznamu dostupných tabulek vyberte tabulky, které chcete synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="2b716-221">From the list of available tables, select the tables that you want to sync.</span></span>

    ![Vyberte tabulky, které chcete synchronizovat](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  <span data-ttu-id="2b716-223">Ve výchozím nastavení jsou vybrány všechny sloupce v tabulce.</span><span class="sxs-lookup"><span data-stu-id="2b716-223">By default, all columns in the table are selected.</span></span> <span data-ttu-id="2b716-224">Pokud nechcete, aby k synchronizaci všech sloupců, zakažte políčko pro sloupce, které nechcete synchronizovat. Ujistěte se, že nechte vybraný sloupec primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="2b716-224">If you don't want to sync all the columns, disable the checkbox for the columns that you don't want to sync. Be sure to leave the primary key column selected.</span></span>

    ![Vyberte pole pro synchronizaci](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  <span data-ttu-id="2b716-226">Nakonec vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2b716-226">Finally, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b716-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b716-227">Next steps</span></span>
<span data-ttu-id="2b716-228">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="2b716-228">Congratulations.</span></span> <span data-ttu-id="2b716-229">Vytvořili jste skupinu synchronizace, která obsahuje instance databáze SQL a databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="2b716-229">You have created a sync group that includes both a SQL Database instance and a SQL Server database.</span></span>

<span data-ttu-id="2b716-230">Další informace o SQL Database a synchronizaci dat SQL najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="2b716-230">For more info about SQL Database and SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="2b716-231">Stáhněte si úplnou synchronizaci dat SQL technická dokumentace</span><span class="sxs-lookup"><span data-stu-id="2b716-231">Download the complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [<span data-ttu-id="2b716-232">Stáhněte si dokumentaci rozhraní API REST synchronizaci dat SQL</span><span class="sxs-lookup"><span data-stu-id="2b716-232">Download the SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [<span data-ttu-id="2b716-233">Databáze SQL – přehled</span><span class="sxs-lookup"><span data-stu-id="2b716-233">SQL Database Overview</span></span>](sql-database-technical-overview.md)
-   [<span data-ttu-id="2b716-234">Správa životního cyklu databáze</span><span class="sxs-lookup"><span data-stu-id="2b716-234">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
