---
title: "Začínáme s auditování databáze Azure SQL | Microsoft Docs"
description: "Začínáme s auditování databáze Azure SQL"
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: giladm
ms.openlocfilehash: f4324a59b5fa4c2e1ab5b1d7fc7e5fe986ea80f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-sql-database-auditing"></a><span data-ttu-id="9353b-103">Začínáme s auditem databáze SQL</span><span class="sxs-lookup"><span data-stu-id="9353b-103">Get started with SQL database auditing</span></span>
<span data-ttu-id="9353b-104">Auditování databáze SQL Azure sleduje události databáze a zápisu, které mají auditu přihlášení účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9353b-104">Azure SQL database auditing tracks database events and writes them to an audit log in your Azure storage account.</span></span> <span data-ttu-id="9353b-105">Auditování také:</span><span class="sxs-lookup"><span data-stu-id="9353b-105">Auditing also:</span></span>

* <span data-ttu-id="9353b-106">Pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9353b-106">Helps you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

* <span data-ttu-id="9353b-107">Umožňuje a zjednodušuje dodržování standardů dodržování předpisů, i když není zárukou toho, dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="9353b-107">Enables and facilitates adherence to compliance standards, although it doesn't guarantee compliance.</span></span> <span data-ttu-id="9353b-108">Další informace o Azure programy dodržování standardů tuto podporu, najdete v části [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/compliance/).</span><span class="sxs-lookup"><span data-stu-id="9353b-108">For more information about Azure programs that support standards compliance, see the [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).</span></span>


## <span data-ttu-id="9353b-109"><a id="subheading-1"></a>Databáze SQL Azure auditování – přehled</span><span class="sxs-lookup"><span data-stu-id="9353b-109"><a id="subheading-1"></a>Azure SQL database auditing overview</span></span>
<span data-ttu-id="9353b-110">Můžete použít auditování databáze SQL pro:</span><span class="sxs-lookup"><span data-stu-id="9353b-110">You can use SQL database auditing to:</span></span>


* <span data-ttu-id="9353b-111">**Zachovat** monitorovat vybrané události.</span><span class="sxs-lookup"><span data-stu-id="9353b-111">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="9353b-112">Můžete definovat kategorie akce databáze k auditování.</span><span class="sxs-lookup"><span data-stu-id="9353b-112">You can define categories of database actions to be audited.</span></span>
* <span data-ttu-id="9353b-113">**Sestava** v databázové aktivitě.</span><span class="sxs-lookup"><span data-stu-id="9353b-113">**Report** on database activity.</span></span> <span data-ttu-id="9353b-114">Abyste mohli rychle začít s aktivity a události vytváření sestav můžete předem nakonfigurované sestavy a řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="9353b-114">You can use preconfigured reports and a dashboard to get started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="9353b-115">**Analýza** sestavy.</span><span class="sxs-lookup"><span data-stu-id="9353b-115">**Analyze** reports.</span></span> <span data-ttu-id="9353b-116">Můžete najít podezřelé události, neobvyklé aktivity a trendy.</span><span class="sxs-lookup"><span data-stu-id="9353b-116">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="9353b-117">Můžete nakonfigurovat auditování pro různé typy událostí kategorie, jak je popsáno v [nastavit auditování pro databázi](#subheading-2) části.</span><span class="sxs-lookup"><span data-stu-id="9353b-117">You can configure auditing for different types of event categories, as explained in the [Set up auditing for your database](#subheading-2) section.</span></span>

<span data-ttu-id="9353b-118">Protokoly auditu se zapisují do úložiště objektů Blob v Azure na vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9353b-118">Audit logs are written to Azure Blob storage on your Azure subscription.</span></span>


## <span data-ttu-id="9353b-119"><a id="subheading-8"></a>Definovat úroveň serveru oproti auditování zásady na úrovni databáze</span><span class="sxs-lookup"><span data-stu-id="9353b-119"><a id="subheading-8"></a>Define server-level vs. database-level auditing policy</span></span>

<span data-ttu-id="9353b-120">Zásady auditu je možné definovat pro konkrétní databázi nebo jako výchozí zásady serveru:</span><span class="sxs-lookup"><span data-stu-id="9353b-120">An auditing policy can be defined for a specific database or as a default server policy:</span></span>

* <span data-ttu-id="9353b-121">Server zásady platí pro všechny stávající a nově vytvořené databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="9353b-121">A server policy applies to all existing and newly created databases on the server.</span></span>

* <span data-ttu-id="9353b-122">Pokud *auditování objektů blob serveru zapnutá*, ho *vždy aplikuje na databázi* (to znamená, že databáze bude vždycky možné auditovat), bez ohledu na databázi nastavení auditování.</span><span class="sxs-lookup"><span data-stu-id="9353b-122">If *server blob auditing is enabled*, it *always applies to the database* (that is, the database will be audited), regardless of the database auditing settings.</span></span>

* <span data-ttu-id="9353b-123">Povolení auditování objektů blob v databázi, kromě povolení na serveru, bude *není* přepsat nebo změnit libovolné nastavení auditování serveru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9353b-123">Enabling blob auditing on the database, in addition to enabling it on the server, will *not* override or change any of the settings of the server blob auditing.</span></span> <span data-ttu-id="9353b-124">Obě audity, budou existovat vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="9353b-124">Both audits will exist side by side.</span></span> <span data-ttu-id="9353b-125">Jinými slovy databáze bude vždycky možné auditovat paralelně dvakrát (jednou zásadami serveru a jednou zásadami databáze).</span><span class="sxs-lookup"><span data-stu-id="9353b-125">In other words, the database will be audited twice in parallel (once by the server policy and once by the database policy).</span></span>

   > [!NOTE]
   > <span data-ttu-id="9353b-126">Povolení auditování objektů blob serveru i auditování databáze blob společně, pokud byste neměli:</span><span class="sxs-lookup"><span data-stu-id="9353b-126">You should avoid enabling both server blob auditing and database blob auditing together, unless:</span></span>
    > * <span data-ttu-id="9353b-127">Chcete použít jiné *účet úložiště* nebo *dobu uchování* pro konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="9353b-127">You want to use a different *storage account* or *retention period* for a specific database.</span></span>
    > * <span data-ttu-id="9353b-128">Chcete auditovat typů událostí nebo kategorie pro konkrétní databázi, která se liší od typů událostí nebo kategorie, auditovaných pro zbytek databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="9353b-128">You want to audit event types or categories for a specific database that differ from event types or categories that are being audited for the rest of the databases on the server.</span></span> <span data-ttu-id="9353b-129">Například můžete mít vložení tabulky, které je nutné auditovat jenom pro konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="9353b-129">For example, you might have table inserts that need to be audited only for a specific database.</span></span>
   > 
   > <span data-ttu-id="9353b-130">Jinak doporučujeme povolit auditování objektů blob jenom úrovni serveru a nechte úroveň databáze auditování zakázáno pro všechny databáze.</span><span class="sxs-lookup"><span data-stu-id="9353b-130">Otherwise, we recommended that you enable only server-level blob auditing and leave the database-level auditing disabled for all databases.</span></span>


## <span data-ttu-id="9353b-131"><a id="subheading-2"></a>Nastavení auditování pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="9353b-131"><a id="subheading-2"></a>Set up auditing for your database</span></span>
<span data-ttu-id="9353b-132">Následující část popisuje konfigurace auditování pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9353b-132">The following section describes the configuration of auditing using the Azure portal.</span></span>

1. <span data-ttu-id="9353b-133">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9353b-133">Go to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9353b-134">Přejděte na **nastavení** okno chcete auditovat serveru SQL database nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9353b-134">Go to the **Settings** blade of the SQL database/SQL server you want to audit.</span></span> <span data-ttu-id="9353b-135">V **nastavení** vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="9353b-135">In the **Settings** blade, select **Auditing & Threat detection**.</span></span>

    <span data-ttu-id="9353b-136"><a id="auditing-screenshot"></a>![Navigačním podokně][1]</span><span class="sxs-lookup"><span data-stu-id="9353b-136"><a id="auditing-screenshot"></a> ![Navigation pane][1]</span></span>
3. <span data-ttu-id="9353b-137">Pokud chcete nastavit zásady auditu serveru (která bude použita pro všechny stávající a nově vytvořené databáze na tomto serveru), můžete vybrat **zobrazit nastavení serveru** odkaz v okně auditování databáze.</span><span class="sxs-lookup"><span data-stu-id="9353b-137">If you prefer to set up a server auditing policy (which will apply to all existing and newly created databases on this server), you can select the **View server settings** link in the database auditing blade.</span></span> <span data-ttu-id="9353b-138">Můžete pak zobrazit nebo upravit nastavení auditování serveru.</span><span class="sxs-lookup"><span data-stu-id="9353b-138">You can then view or modify the server auditing settings.</span></span>

    ![Navigační podokno][2]
4. <span data-ttu-id="9353b-140">Pokud chcete povolit auditování objektů blob na úrovni databáze (kromě nebo místo auditování na úrovni serveru), pro **auditování**, vyberte **ON**a pro **auditování typ**, Vyberte **Blob**.</span><span class="sxs-lookup"><span data-stu-id="9353b-140">If you prefer to enable blob auditing on the database level (in addition to or instead of server-level auditing), for **Auditing**, select **ON**, and for **Auditing type**, select  **Blob**.</span></span>

    <span data-ttu-id="9353b-141">Pokud je auditování objektů blob serveru je povoleno, budou existovat audit databáze konfigurována node souběžně s auditování objektů blob serveru.</span><span class="sxs-lookup"><span data-stu-id="9353b-141">If server blob auditing is enabled, the database-configured audit will exist side by side with the server blob audit.</span></span>  

    ![Navigační podokno][3]
5. <span data-ttu-id="9353b-143">Chcete-li otevřít **úložiště protokolů auditu** vyberte **podrobnosti úložiště**.</span><span class="sxs-lookup"><span data-stu-id="9353b-143">To open the **Audit Logs Storage** blade, select **Storage Details**.</span></span> <span data-ttu-id="9353b-144">Vyberte účet úložiště Azure, kde bude uložena protokoly a pak vyberte dobu uchování, po jejímž uplynutí se odstraní starých protokolů.</span><span class="sxs-lookup"><span data-stu-id="9353b-144">Select the Azure storage account where logs will be saved, and then select the retention period, after which the old logs will be deleted.</span></span> <span data-ttu-id="9353b-145">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9353b-145">Then click **OK**.</span></span> 
   >[!TIP] 
   ><span data-ttu-id="9353b-146">K plnému využití mimo auditování šablon sestav, použijte stejný účet úložiště pro všechny auditování databáze.</span><span class="sxs-lookup"><span data-stu-id="9353b-146">To get the most out of the auditing reports templates, use the same storage account for all audited databases.</span></span> 

    <span data-ttu-id="9353b-147"><a id="storage-screenshot"></a>![Navigačním podokně][4]</span><span class="sxs-lookup"><span data-stu-id="9353b-147"><a id="storage-screenshot"></a> ![Navigation pane][4]</span></span>
6. <span data-ttu-id="9353b-148">Pokud chcete přizpůsobit auditované události, můžete k tomu pomocí prostředí PowerShell nebo rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9353b-148">If you want to customize the audited events, you can do this via PowerShell or the REST API.</span></span> <span data-ttu-id="9353b-149">Další podrobnosti najdete v tématu [automatizace (prostředí PowerShell nebo REST API)](#subheading-7) části.</span><span class="sxs-lookup"><span data-stu-id="9353b-149">For more details, see the [Automation (PowerShell/REST API)](#subheading-7) section.</span></span>
7. <span data-ttu-id="9353b-150">Po dokončení konfigurace nastavení auditování, můžete zapnout funkci nové detekce hrozeb a konfigurovat výstrahy zabezpečení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="9353b-150">After you've configured your auditing settings, you can turn on the new threat detection feature and configure emails to receive security alerts.</span></span> <span data-ttu-id="9353b-151">Pokud používáte detekce hrozeb, obdržíte proaktivní výstrahy na nezvyklé databázové aktivity, které může znamenat potenciální bezpečnostní hrozby.</span><span class="sxs-lookup"><span data-stu-id="9353b-151">When you use threat detection, you receive proactive alerts on anomalous database activities that can indicate potential security threats.</span></span> <span data-ttu-id="9353b-152">Další podrobnosti najdete v tématu [Začínáme s detekce hrozeb](sql-database-threat-detection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9353b-152">For more details, see [Getting started with threat detection](sql-database-threat-detection-get-started.md).</span></span>
8. <span data-ttu-id="9353b-153">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9353b-153">Click **Save**.</span></span>





## <span data-ttu-id="9353b-154"><a id="subheading-3"></a>Analýza protokolů auditu a sestavy</span><span class="sxs-lookup"><span data-stu-id="9353b-154"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="9353b-155">Protokoly auditu je agregován v účtu úložiště Azure, které jste zvolili během instalace.</span><span class="sxs-lookup"><span data-stu-id="9353b-155">Audit logs are aggregated in the Azure storage account you chose during setup.</span></span> <span data-ttu-id="9353b-156">Protokoly auditu můžete prozkoumat pomocí nástroje, jako například [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="9353b-156">You can explore audit logs by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="9353b-157">Protokoly auditování objektů BLOB jsou uloženy jako kolekce souborů, objektů blob do kontejneru, s názvem **sqldbauditlogs**.</span><span class="sxs-lookup"><span data-stu-id="9353b-157">Blob auditing logs are saved as a collection of blob files within a container named **sqldbauditlogs**.</span></span>

<span data-ttu-id="9353b-158">Další podrobnosti o hierarchii složce úložiště protokolů auditování objektů blob, zásady vytváření názvů objektů blob a formát protokolu najdete v tématu [Blob auditu protokolu referenční příručka pro formátování (soubor .docx ke stažení)](https://go.microsoft.com/fwlink/?linkid=829599).</span><span class="sxs-lookup"><span data-stu-id="9353b-158">For further details about the hierarchy of the blob audit logs storage folder, blob naming conventions, and log format, see the [Blob Audit Log Format Reference (.docx file download)](https://go.microsoft.com/fwlink/?linkid=829599).</span></span>

<span data-ttu-id="9353b-159">Existuje několik metod, které můžete použít k zobrazení protokolů auditování objektů blob:</span><span class="sxs-lookup"><span data-stu-id="9353b-159">There are several methods you can use to view blob auditing logs:</span></span>

* <span data-ttu-id="9353b-160">Použití [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9353b-160">Use the [Azure portal](https://portal.azure.com).</span></span>  <span data-ttu-id="9353b-161">Otevře příslušné databáze.</span><span class="sxs-lookup"><span data-stu-id="9353b-161">Open the relevant database.</span></span> <span data-ttu-id="9353b-162">V horní části této databáze **auditování a detekce hrozeb** okně klikněte na tlačítko **zobrazit protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="9353b-162">At the top of the database's **Auditing & Threat detection** blade, click **View audit logs**.</span></span>

    ![Navigační podokno][7]

    <span data-ttu-id="9353b-164">**Audit záznamy** otevře se okno, z nichž budete moci zobrazit protokoly.</span><span class="sxs-lookup"><span data-stu-id="9353b-164">An **Audit records** blade opens, from which you'll be able to view the logs.</span></span>

    - <span data-ttu-id="9353b-165">Kliknutím můžete zobrazit konkrétní kalendářní data **filtru** v horní části **Audit záznamy** okno.</span><span class="sxs-lookup"><span data-stu-id="9353b-165">You can view specific dates by clicking **Filter** at the top of the **Audit records** blade.</span></span>
    - <span data-ttu-id="9353b-166">Můžete přepínat mezi záznamy auditu, které byly vytvořeny auditu pro zásady zásady nebo databázi serveru.</span><span class="sxs-lookup"><span data-stu-id="9353b-166">You can switch between audit records that were created by a server policy or database policy audit.</span></span>

       ![Navigační podokno][8]

* <span data-ttu-id="9353b-168">Pomocí funkce systému **sys.fn_get_audit_file** (T-SQL) se vrátit daty protokolu auditování v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="9353b-168">Use the system function **sys.fn_get_audit_file** (T-SQL) to return the audit log data in tabular format.</span></span> <span data-ttu-id="9353b-169">Další informace o použití této funkce najdete v tématu [sys.fn_get_audit_file dokumentaci](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="9353b-169">For more information on using this function, see the [sys.fn_get_audit_file documentation](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span></span>


* <span data-ttu-id="9353b-170">Použití **sloučení soubory auditu** v aplikaci SQL Server Management Studio (počínaje SSMS 17):</span><span class="sxs-lookup"><span data-stu-id="9353b-170">Use **Merge Audit Files** in SQL Server Management Studio (starting with SSMS 17):</span></span>  
    1. <span data-ttu-id="9353b-171">V nabídce aplikace SSMS vyberte **soubor** > **otevřete** > **sloučení soubory auditu**.</span><span class="sxs-lookup"><span data-stu-id="9353b-171">From the SSMS menu, select **File** > **Open** > **Merge Audit Files**.</span></span>

        ![Navigační podokno][9]
    2. <span data-ttu-id="9353b-173">**Přidat soubory auditu** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9353b-173">The **Add Audit Files** dialog box opens.</span></span> <span data-ttu-id="9353b-174">Vyberte jednu z **přidat** možnost vyberte, zda chcete sloučení auditu soubory z místního disku nebo importovat z Azure Storage (bude se budete muset zadat podrobnosti o úložiště Azure a klíč účtu).</span><span class="sxs-lookup"><span data-stu-id="9353b-174">Select one of the **Add** options to choose whether to merge audit files from a local disk or import them from Azure Storage (you will be required to provide your Azure Storage details and account key).</span></span>

    3. <span data-ttu-id="9353b-175">Po přidání všech souborů, sloučení, klikněte na tlačítko **OK** k dokončení operace sloučení.</span><span class="sxs-lookup"><span data-stu-id="9353b-175">After all files to merge have been added, click **OK** to complete the merge operation.</span></span>

    4. <span data-ttu-id="9353b-176">Sloučené soubor se otevře v aplikaci SSMS, kde je můžete zobrazit a analyzujte ji, a také ho exportovat do souboru souboru XEL nebo sdíleného svazku clusteru nebo do tabulky.</span><span class="sxs-lookup"><span data-stu-id="9353b-176">The merged file opens in SSMS, where you can view and analyze it, as well as export it to an XEL or CSV file or to a table.</span></span>

* <span data-ttu-id="9353b-177">Použití [synchronizace aplikace](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) které jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9353b-177">Use the [sync application](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) that we have created.</span></span> <span data-ttu-id="9353b-178">Jeho spuštění v Azure a využívá Operations Management Suite (OMS) veřejná rozhraní API analýzy protokolů tak, aby nabízel protokoly auditu SQL do OMS.</span><span class="sxs-lookup"><span data-stu-id="9353b-178">It runs in Azure and utilizes Operations Management Suite (OMS) Log Analytics public APIs to push SQL audit logs into OMS.</span></span> <span data-ttu-id="9353b-179">Synchronizace aplikace doručí protokoly auditu SQL do OMS analýzy protokolů pro používání prostřednictvím řídicího panelu analýzy protokolů OMS.</span><span class="sxs-lookup"><span data-stu-id="9353b-179">The sync application pushes SQL audit logs into OMS Log Analytics for consumption via the OMS Log Analytics dashboard.</span></span> 

* <span data-ttu-id="9353b-180">Pomocí Power BI.</span><span class="sxs-lookup"><span data-stu-id="9353b-180">Use Power BI.</span></span> <span data-ttu-id="9353b-181">Můžete zobrazit a analyzovat data protokolu auditování v Power BI.</span><span class="sxs-lookup"><span data-stu-id="9353b-181">You can view and analyze audit log data in Power BI.</span></span> <span data-ttu-id="9353b-182">Další informace o [Power BI a přístup ke stažení šablony](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span><span class="sxs-lookup"><span data-stu-id="9353b-182">Learn more about [Power BI, and access a downloadable template](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span></span>

* <span data-ttu-id="9353b-183">Stáhnout soubory protokolů z vašeho kontejneru objektů blob Azure Storage prostřednictvím portálu nebo pomocí nástroje, jako například [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="9353b-183">Download log files from your Azure Storage blob container via the portal or by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
    * <span data-ttu-id="9353b-184">Po stažení souboru protokolu, který je místně, můžete dvakrát kliknete na soubor otevřít, zobrazit a analyzovat protokoly v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="9353b-184">After you have downloaded a log file locally, you can double-click the file to open, view, and analyze the logs in SSMS.</span></span>
    * <span data-ttu-id="9353b-185">Můžete také stáhnout víc souborů současně prostřednictvím Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="9353b-185">You can also download multiple files simultaneously via Azure Storage Explorer.</span></span> <span data-ttu-id="9353b-186">Klikněte pravým tlačítkem na konkrétní podsložku (například do podsložky, která obsahuje všechny soubory protokolu pro konkrétní datum) a vyberte **uložit jako** uložení do místní složky.</span><span class="sxs-lookup"><span data-stu-id="9353b-186">Right-click a specific subfolder (for example, a subfolder that includes all log files for a specific date) and select **Save as** to save in a local folder.</span></span>

* <span data-ttu-id="9353b-187">Další metody:</span><span class="sxs-lookup"><span data-stu-id="9353b-187">Additional methods:</span></span>
   * <span data-ttu-id="9353b-188">Po stažení několik souborů (nebo na podsložku, která zahrnuje soubory protokolu pro celý den, jak je popsáno v předchozí položce v tomto seznamu), můžete je sloučit místně popsané v pokynech soubory auditu sloučení SSMS popsané výše.</span><span class="sxs-lookup"><span data-stu-id="9353b-188">After downloading several files (or a subfolder that includes log files for an entire day, as described in the previous item in this list), you can merge them locally as described in the SSMS Merge Audit Files instructions described earlier.</span></span>

   * <span data-ttu-id="9353b-189">Auditování objektů blob k zobrazení protokolů prostřednictvím kódu programu:</span><span class="sxs-lookup"><span data-stu-id="9353b-189">View blob auditing logs programmatically:</span></span>

     * <span data-ttu-id="9353b-190">Použití [rozšířené události čtečky](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) knihovna jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="9353b-190">Use the [Extended Events Reader](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# library.</span></span>
     * <span data-ttu-id="9353b-191">[Dotaz Rozšířené události soubory](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9353b-191">[Query Extended Events Files](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) by using PowerShell.</span></span>




## <span data-ttu-id="9353b-192"><a id="subheading-5"></a>Provozní postupy</span><span class="sxs-lookup"><span data-stu-id="9353b-192"><a id="subheading-5"></a>Production practices</span></span>
<!--The description in this section refers to preceding screen captures.-->

### <span data-ttu-id="9353b-193"><a id="subheading-6">Auditování geograficky replikované databáze</a></span><span class="sxs-lookup"><span data-stu-id="9353b-193"><a id="subheading-6">Auditing geo-replicated databases</a></span></span>
<span data-ttu-id="9353b-194">Pokud používáte geograficky replikované databáze, je možné nastavit auditování přístupu k primární databázi, sekundární databázi nebo obojí, v závislosti na typu auditu.</span><span class="sxs-lookup"><span data-stu-id="9353b-194">When you use geo-replicated databases, it is possible to set up auditing on either the primary database, the secondary database, or both, depending on the audit type.</span></span>

<span data-ttu-id="9353b-195">Postupujte podle těchto pokynů (Nezapomeňte, že auditování objektů blob lze zapnout nebo vypnout pouze z primární databáze nastavení auditování):</span><span class="sxs-lookup"><span data-stu-id="9353b-195">Follow these instructions (remember that blob auditing can be turned on or off only from the primary database auditing settings):</span></span>

* <span data-ttu-id="9353b-196">**Primární databáze**.</span><span class="sxs-lookup"><span data-stu-id="9353b-196">**Primary database**.</span></span> <span data-ttu-id="9353b-197">Zapnout auditování objektů blob na serveru nebo v databázi, samostatně, jak je popsáno v [nastavit auditování pro databázi](#subheading-2) části.</span><span class="sxs-lookup"><span data-stu-id="9353b-197">Turn on blob auditing, either on the server or on the database itself, as described in the [Set up auditing for your database](#subheading-2) section.</span></span>
* <span data-ttu-id="9353b-198">**Sekundární databáze**.</span><span class="sxs-lookup"><span data-stu-id="9353b-198">**Secondary database**.</span></span> <span data-ttu-id="9353b-199">Zapnout auditování objektů blob v primární databázi, jak je popsáno v [nastavit auditování pro databázi](#subheading-2) části.</span><span class="sxs-lookup"><span data-stu-id="9353b-199">Turn on blob auditing on the primary database, as described in the [Set up auditing for your database](#subheading-2) section.</span></span> 
   * <span data-ttu-id="9353b-200">Auditování objektů BLOB musí být povolená na *primární databázi, samotné*, nikoli na server.</span><span class="sxs-lookup"><span data-stu-id="9353b-200">Blob auditing must be enabled on the *primary database itself*, not the server.</span></span>
   * <span data-ttu-id="9353b-201">Povolíte auditování objektů blob v primární databázi, bude ho také přístupné v sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="9353b-201">After blob auditing is enabled on the primary database, it will also become enabled on the secondary database.</span></span>

     >[!IMPORTANT]
     ><span data-ttu-id="9353b-202">Ve výchozím nastavení úložiště pro sekundární databázi bude stejné jako primární databáze, způsobuje provoz mezi místní.</span><span class="sxs-lookup"><span data-stu-id="9353b-202">By default, the storage settings for the secondary database will be identical to those of the primary database, causing cross-regional traffic.</span></span> <span data-ttu-id="9353b-203">To se můžete vyhnout povolením auditování objektů blob na sekundárním serveru a konfigurace místní úložiště v nastavení úložiště sekundární server.</span><span class="sxs-lookup"><span data-stu-id="9353b-203">You can avoid this by enabling blob auditing on the secondary server and configuring local storage in the secondary server storage settings.</span></span> <span data-ttu-id="9353b-204">Tím se přepíše umístění úložiště pro sekundární databáze a výsledek v každé databázi ukládání protokoly auditu do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="9353b-204">This will override the storage location for the secondary database and result in each database saving its audit logs to local storage.</span></span>  
<br>

### <span data-ttu-id="9353b-205"><a id="subheading-6">Opětovné generování klíče úložiště</a></span><span class="sxs-lookup"><span data-stu-id="9353b-205"><a id="subheading-6">Storage key regeneration</a></span></span>
<span data-ttu-id="9353b-206">V produkčním prostředí budete pravděpodobně pravidelně aktualizovat klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="9353b-206">In production, you are likely to refresh your storage keys periodically.</span></span> <span data-ttu-id="9353b-207">Při aktualizaci klíče, je nutné k opakovanému uložení zásad auditu.</span><span class="sxs-lookup"><span data-stu-id="9353b-207">When refreshing your keys, you need to resave the auditing policy.</span></span> <span data-ttu-id="9353b-208">Proces je následující:</span><span class="sxs-lookup"><span data-stu-id="9353b-208">The process is as follows:</span></span>

1. <span data-ttu-id="9353b-209">Otevřete **podrobnosti úložiště** okno.</span><span class="sxs-lookup"><span data-stu-id="9353b-209">Open the **Storage Details** blade.</span></span> <span data-ttu-id="9353b-210">V **přístupový klíč k úložišti** vyberte **sekundární**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9353b-210">In the **Storage Access Key** box, select **Secondary**, and click **OK**.</span></span> <span data-ttu-id="9353b-211">Pak klikněte na tlačítko **Uložit** v horní části okna Konfigurace auditování.</span><span class="sxs-lookup"><span data-stu-id="9353b-211">Then click **Save** at the top of the auditing configuration blade.</span></span>

    ![Navigační podokno][5]
2. <span data-ttu-id="9353b-213">Přejděte do okna konfigurace úložiště a znovu vygenerovat primární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="9353b-213">Go to the storage configuration blade and regenerate the primary access key.</span></span>

    ![Navigační podokno][6]
3. <span data-ttu-id="9353b-215">Přejděte zpět do okna auditování konfigurace přepínače ze sekundární pro primární přístupový klíč úložiště a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9353b-215">Go back to the auditing configuration blade, switch the storage access key from secondary to primary, and then click **OK**.</span></span> <span data-ttu-id="9353b-216">Pak klikněte na tlačítko **Uložit** v horní části okna Konfigurace auditování.</span><span class="sxs-lookup"><span data-stu-id="9353b-216">Then click **Save** at the top of the auditing configuration blade.</span></span>
4. <span data-ttu-id="9353b-217">Přejděte zpět do okna konfigurace úložiště a znovu vygenerovat sekundární přístupový klíč (v rámci přípravy cyklus aktualizace Další klíč).</span><span class="sxs-lookup"><span data-stu-id="9353b-217">Go back to the storage configuration blade and regenerate the secondary access key (in preparation for the next key's refresh cycle).</span></span>

## <span data-ttu-id="9353b-218"><a id="subheading-7"></a>Automatizace (prostředí PowerShell nebo REST API)</span><span class="sxs-lookup"><span data-stu-id="9353b-218"><a id="subheading-7"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="9353b-219">Můžete taky nakonfigurovat auditování ve službě Azure SQL Database pomocí následujících nástrojů automatizace:</span><span class="sxs-lookup"><span data-stu-id="9353b-219">You can also configure auditing in Azure SQL Database by using the following automation tools:</span></span>

* <span data-ttu-id="9353b-220">**Rutiny prostředí PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="9353b-220">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="9353b-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="9353b-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="9353b-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="9353b-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="9353b-223">[Odebrat AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="9353b-223">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="9353b-224">[Odebrat AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="9353b-224">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="9353b-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="9353b-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="9353b-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="9353b-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="9353b-227">[Použití AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="9353b-227">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

   <span data-ttu-id="9353b-228">Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9353b-228">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

* <span data-ttu-id="9353b-229">**REST API – auditování objektů Blob**:</span><span class="sxs-lookup"><span data-stu-id="9353b-229">**REST API - Blob auditing**:</span></span>

   * [<span data-ttu-id="9353b-230">Vytvořit nebo aktualizovat zásady auditování Blob databáze</span><span class="sxs-lookup"><span data-stu-id="9353b-230">Create or Update Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [<span data-ttu-id="9353b-231">Vytvořit nebo aktualizovat Server Blob zásady auditování</span><span class="sxs-lookup"><span data-stu-id="9353b-231">Create or Update Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [<span data-ttu-id="9353b-232">Získání objektu Blob databáze zásady auditování</span><span class="sxs-lookup"><span data-stu-id="9353b-232">Get Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [<span data-ttu-id="9353b-233">Získání objektu Blob serveru zásady auditování</span><span class="sxs-lookup"><span data-stu-id="9353b-233">Get Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [<span data-ttu-id="9353b-234">Získání objektu Blob serveru auditování výsledek operace</span><span class="sxs-lookup"><span data-stu-id="9353b-234">Get Server Blob Auditing Operation Result</span></span>](https://msdn.microsoft.com/library/azure/mt771862.aspx)


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy
