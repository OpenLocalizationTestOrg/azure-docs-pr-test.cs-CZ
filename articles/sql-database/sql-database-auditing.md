---
title: "aaaGet začít s auditování databáze Azure SQL | Microsoft Docs"
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
ms.openlocfilehash: 5494c602d702ac41992520f900c393a98cc7c989
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-auditing"></a><span data-ttu-id="fbd49-103">Začínáme s auditem databáze SQL</span><span class="sxs-lookup"><span data-stu-id="fbd49-103">Get started with SQL database auditing</span></span>
<span data-ttu-id="fbd49-104">Auditování databáze SQL Azure sleduje události databáze a zapisuje je tooan protokolu auditování v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="fbd49-104">Azure SQL database auditing tracks database events and writes them tooan audit log in your Azure storage account.</span></span> <span data-ttu-id="fbd49-105">Auditování také:</span><span class="sxs-lookup"><span data-stu-id="fbd49-105">Auditing also:</span></span>

* <span data-ttu-id="fbd49-106">Pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fbd49-106">Helps you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

* <span data-ttu-id="fbd49-107">Umožňuje a zjednodušuje dodržování standardů toocompliance, i když není zárukou toho, dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="fbd49-107">Enables and facilitates adherence toocompliance standards, although it doesn't guarantee compliance.</span></span> <span data-ttu-id="fbd49-108">Další informace o Azure programy dodržování standardů tuto podporu, najdete v části hello [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/compliance/).</span><span class="sxs-lookup"><span data-stu-id="fbd49-108">For more information about Azure programs that support standards compliance, see hello [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).</span></span>


## <span data-ttu-id="fbd49-109"><a id="subheading-1"></a>Databáze SQL Azure auditování – přehled</span><span class="sxs-lookup"><span data-stu-id="fbd49-109"><a id="subheading-1"></a>Azure SQL database auditing overview</span></span>
<span data-ttu-id="fbd49-110">Můžete použít auditování databáze SQL pro:</span><span class="sxs-lookup"><span data-stu-id="fbd49-110">You can use SQL database auditing to:</span></span>


* <span data-ttu-id="fbd49-111">**Zachovat** monitorovat vybrané události.</span><span class="sxs-lookup"><span data-stu-id="fbd49-111">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="fbd49-112">Můžete definovat kategorie toobe databáze akce auditovat.</span><span class="sxs-lookup"><span data-stu-id="fbd49-112">You can define categories of database actions toobe audited.</span></span>
* <span data-ttu-id="fbd49-113">**Sestava** v databázové aktivitě.</span><span class="sxs-lookup"><span data-stu-id="fbd49-113">**Report** on database activity.</span></span> <span data-ttu-id="fbd49-114">Můžete vytvořit předem nakonfigurované sestavy a řídicí panel tooget, rychle začít s aktivity a události vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="fbd49-114">You can use preconfigured reports and a dashboard tooget started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="fbd49-115">**Analýza** sestavy.</span><span class="sxs-lookup"><span data-stu-id="fbd49-115">**Analyze** reports.</span></span> <span data-ttu-id="fbd49-116">Můžete najít podezřelé události, neobvyklé aktivity a trendy.</span><span class="sxs-lookup"><span data-stu-id="fbd49-116">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="fbd49-117">Můžete nakonfigurovat auditování pro různé typy událostí kategorie, jak je popsáno v hello [nastavit auditování pro databázi](#subheading-2) části.</span><span class="sxs-lookup"><span data-stu-id="fbd49-117">You can configure auditing for different types of event categories, as explained in hello [Set up auditing for your database](#subheading-2) section.</span></span>

<span data-ttu-id="fbd49-118">Protokoly auditu se zapisují tooAzure úložiště objektů Blob na vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fbd49-118">Audit logs are written tooAzure Blob storage on your Azure subscription.</span></span>


## <span data-ttu-id="fbd49-119"><a id="subheading-8"></a>Definovat úroveň serveru oproti auditování zásady na úrovni databáze</span><span class="sxs-lookup"><span data-stu-id="fbd49-119"><a id="subheading-8"></a>Define server-level vs. database-level auditing policy</span></span>

<span data-ttu-id="fbd49-120">Zásady auditu je možné definovat pro konkrétní databázi nebo jako výchozí zásady serveru:</span><span class="sxs-lookup"><span data-stu-id="fbd49-120">An auditing policy can be defined for a specific database or as a default server policy:</span></span>

* <span data-ttu-id="fbd49-121">Zásady serveru platí tooall stávající a nově vytvořené databáze na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-121">A server policy applies tooall existing and newly created databases on hello server.</span></span>

* <span data-ttu-id="fbd49-122">Pokud *auditování objektů blob serveru zapnutá*, ho *vždy použije toohello databáze* (tedy hello databáze auditování), bez ohledu na to databáze hello nastavení auditování.</span><span class="sxs-lookup"><span data-stu-id="fbd49-122">If *server blob auditing is enabled*, it *always applies toohello database* (that is, hello database will be audited), regardless of hello database auditing settings.</span></span>

* <span data-ttu-id="fbd49-123">Povolení auditování pro databázi hello v přidání tooenabling ho na hello server, objektů blob budou *není* přepsat nebo změnit libovolné nastavení hello auditování objektů blob server hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-123">Enabling blob auditing on hello database, in addition tooenabling it on hello server, will *not* override or change any of hello settings of hello server blob auditing.</span></span> <span data-ttu-id="fbd49-124">Obě audity, budou existovat vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="fbd49-124">Both audits will exist side by side.</span></span> <span data-ttu-id="fbd49-125">Jinými slovy databáze hello dvakrát paralelně auditování (jednou zásadami hello serveru a jednou zásadami hello databáze).</span><span class="sxs-lookup"><span data-stu-id="fbd49-125">In other words, hello database will be audited twice in parallel (once by hello server policy and once by hello database policy).</span></span>

   > [!NOTE]
   > <span data-ttu-id="fbd49-126">Povolení auditování objektů blob serveru i auditování databáze blob společně, pokud byste neměli:</span><span class="sxs-lookup"><span data-stu-id="fbd49-126">You should avoid enabling both server blob auditing and database blob auditing together, unless:</span></span>
    > * <span data-ttu-id="fbd49-127">Chcete toouse jiné *účet úložiště* nebo *dobu uchování* pro konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="fbd49-127">You want toouse a different *storage account* or *retention period* for a specific database.</span></span>
    > * <span data-ttu-id="fbd49-128">Chcete typů událostí tooaudit nebo kategorie pro konkrétní databázi, která se liší od typů událostí nebo kategorie, auditovaných pro hello zbytek hello databáze na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-128">You want tooaudit event types or categories for a specific database that differ from event types or categories that are being audited for hello rest of hello databases on hello server.</span></span> <span data-ttu-id="fbd49-129">Například můžete mít vložení tabulky, které je třeba toobe Audituje jenom pro konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="fbd49-129">For example, you might have table inserts that need toobe audited only for a specific database.</span></span>
   > 
   > <span data-ttu-id="fbd49-130">Jinak doporučujeme, abyste povolili auditování objektů blob jenom úrovni serveru a nechte hello databáze auditování na úrovni zakázáno pro všechny databáze.</span><span class="sxs-lookup"><span data-stu-id="fbd49-130">Otherwise, we recommended that you enable only server-level blob auditing and leave hello database-level auditing disabled for all databases.</span></span>


## <span data-ttu-id="fbd49-131"><a id="subheading-2"></a>Nastavení auditování pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="fbd49-131"><a id="subheading-2"></a>Set up auditing for your database</span></span>
<span data-ttu-id="fbd49-132">Hello následující část popisuje hello konfigurace auditování pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fbd49-132">hello following section describes hello configuration of auditing using hello Azure portal.</span></span>

1. <span data-ttu-id="fbd49-133">Přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fbd49-133">Go toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fbd49-134">Přejděte toohello **nastavení** okno hello chcete tooaudit serveru SQL database nebo SQL server.</span><span class="sxs-lookup"><span data-stu-id="fbd49-134">Go toohello **Settings** blade of hello SQL database/SQL server you want tooaudit.</span></span> <span data-ttu-id="fbd49-135">V hello **nastavení** vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-135">In hello **Settings** blade, select **Auditing & Threat detection**.</span></span>

    <span data-ttu-id="fbd49-136"><a id="auditing-screenshot"></a>![Navigačním podokně][1]</span><span class="sxs-lookup"><span data-stu-id="fbd49-136"><a id="auditing-screenshot"></a> ![Navigation pane][1]</span></span>
3. <span data-ttu-id="fbd49-137">Pokud dáváte přednost tooset až zásady auditování serveru (která bude použita existující tooall a nově vytvořené databáze na tomto serveru), můžete vybrat hello **zobrazit nastavení serveru** odkaz v okně auditování databáze hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-137">If you prefer tooset up a server auditing policy (which will apply tooall existing and newly created databases on this server), you can select hello **View server settings** link in hello database auditing blade.</span></span> <span data-ttu-id="fbd49-138">Můžete pak zobrazit nebo upravit nastavení auditování serveru hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-138">You can then view or modify hello server auditing settings.</span></span>

    ![Navigační podokno][2]
4. <span data-ttu-id="fbd49-140">Pokud dáváte přednost tooenable auditování objektů blob na úrovni databáze hello (v přidání tooor místo auditování na úrovni serveru), pro **auditování**, vyberte **ON**a pro **auditování typ** , vyberte **Blob**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-140">If you prefer tooenable blob auditing on hello database level (in addition tooor instead of server-level auditing), for **Auditing**, select **ON**, and for **Auditing type**, select  **Blob**.</span></span>

    <span data-ttu-id="fbd49-141">Pokud je auditování objektů blob serveru je povoleno, budou existovat audit databáze konfigurována hello node souběžně s auditování objektů blob server hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-141">If server blob auditing is enabled, hello database-configured audit will exist side by side with hello server blob audit.</span></span>  

    ![Navigační podokno][3]
5. <span data-ttu-id="fbd49-143">tooopen hello **úložiště protokolů auditu** vyberte **podrobnosti úložiště**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-143">tooopen hello **Audit Logs Storage** blade, select **Storage Details**.</span></span> <span data-ttu-id="fbd49-144">Vyberte účet úložiště Azure hello, kde bude uložena protokoly a pak vyberte dobu uchování hello, po které hello se odstraní starých protokolů.</span><span class="sxs-lookup"><span data-stu-id="fbd49-144">Select hello Azure storage account where logs will be saved, and then select hello retention period, after which hello old logs will be deleted.</span></span> <span data-ttu-id="fbd49-145">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-145">Then click **OK**.</span></span> 
   >[!TIP] 
   ><span data-ttu-id="fbd49-146">hello tooget hello naplno hello auditování šablon sestav, použijte stejný účet úložiště pro všechny auditování databáze.</span><span class="sxs-lookup"><span data-stu-id="fbd49-146">tooget hello most out of hello auditing reports templates, use hello same storage account for all audited databases.</span></span> 

    <span data-ttu-id="fbd49-147"><a id="storage-screenshot"></a>![Navigačním podokně][4]</span><span class="sxs-lookup"><span data-stu-id="fbd49-147"><a id="storage-screenshot"></a> ![Navigation pane][4]</span></span>
6. <span data-ttu-id="fbd49-148">Pokud chcete toocustomize hello auditovat události, můžete to provést pomocí prostředí PowerShell nebo hello REST API.</span><span class="sxs-lookup"><span data-stu-id="fbd49-148">If you want toocustomize hello audited events, you can do this via PowerShell or hello REST API.</span></span> <span data-ttu-id="fbd49-149">Další podrobnosti najdete v tématu hello [automatizace (prostředí PowerShell nebo REST API)](#subheading-7) části.</span><span class="sxs-lookup"><span data-stu-id="fbd49-149">For more details, see hello [Automation (PowerShell/REST API)](#subheading-7) section.</span></span>
7. <span data-ttu-id="fbd49-150">Po dokončení konfigurace nastavení auditování, můžete zapnout hello novou funkci detekce hrozeb a konfigurovat výstrahy zabezpečení tooreceive e-mailů.</span><span class="sxs-lookup"><span data-stu-id="fbd49-150">After you've configured your auditing settings, you can turn on hello new threat detection feature and configure emails tooreceive security alerts.</span></span> <span data-ttu-id="fbd49-151">Pokud používáte detekce hrozeb, obdržíte proaktivní výstrahy na nezvyklé databázové aktivity, které může znamenat potenciální bezpečnostní hrozby.</span><span class="sxs-lookup"><span data-stu-id="fbd49-151">When you use threat detection, you receive proactive alerts on anomalous database activities that can indicate potential security threats.</span></span> <span data-ttu-id="fbd49-152">Další podrobnosti najdete v tématu [Začínáme s detekce hrozeb](sql-database-threat-detection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fbd49-152">For more details, see [Getting started with threat detection](sql-database-threat-detection-get-started.md).</span></span>
8. <span data-ttu-id="fbd49-153">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-153">Click **Save**.</span></span>





## <span data-ttu-id="fbd49-154"><a id="subheading-3"></a>Analýza protokolů auditu a sestavy</span><span class="sxs-lookup"><span data-stu-id="fbd49-154"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="fbd49-155">Protokoly auditu je agregován v hello účtu úložiště Azure, které jste zvolili během instalace.</span><span class="sxs-lookup"><span data-stu-id="fbd49-155">Audit logs are aggregated in hello Azure storage account you chose during setup.</span></span> <span data-ttu-id="fbd49-156">Protokoly auditu můžete prozkoumat pomocí nástroje, jako například [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="fbd49-156">You can explore audit logs by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="fbd49-157">Protokoly auditování objektů BLOB jsou uloženy jako kolekce souborů, objektů blob do kontejneru, s názvem **sqldbauditlogs**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-157">Blob auditing logs are saved as a collection of blob files within a container named **sqldbauditlogs**.</span></span>

<span data-ttu-id="fbd49-158">Další podrobnosti o hierarchii hello složce úložiště protokolů auditování objektů blob hello, zásady vytváření názvů objektů blob a formát protokolu najdete v tématu hello [Blob auditu protokolu referenční příručka pro formátování (soubor .docx ke stažení)](https://go.microsoft.com/fwlink/?linkid=829599).</span><span class="sxs-lookup"><span data-stu-id="fbd49-158">For further details about hello hierarchy of hello blob audit logs storage folder, blob naming conventions, and log format, see hello [Blob Audit Log Format Reference (.docx file download)](https://go.microsoft.com/fwlink/?linkid=829599).</span></span>

<span data-ttu-id="fbd49-159">Existuje několik metod, které můžete použít objekt blob tooview auditování protokoly:</span><span class="sxs-lookup"><span data-stu-id="fbd49-159">There are several methods you can use tooview blob auditing logs:</span></span>

* <span data-ttu-id="fbd49-160">Použití hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fbd49-160">Use hello [Azure portal](https://portal.azure.com).</span></span>  <span data-ttu-id="fbd49-161">Otevřete hello příslušné databáze.</span><span class="sxs-lookup"><span data-stu-id="fbd49-161">Open hello relevant database.</span></span> <span data-ttu-id="fbd49-162">Na nejvyšší hello hello databáze **auditování a detekce hrozeb** okně klikněte na tlačítko **zobrazit protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-162">At hello top of hello database's **Auditing & Threat detection** blade, click **View audit logs**.</span></span>

    ![Navigační podokno][7]

    <span data-ttu-id="fbd49-164">**Audit záznamy** otevře se okno, ze kterých budete moct tooview hello protokoly.</span><span class="sxs-lookup"><span data-stu-id="fbd49-164">An **Audit records** blade opens, from which you'll be able tooview hello logs.</span></span>

    - <span data-ttu-id="fbd49-165">Kliknutím můžete zobrazit konkrétní kalendářní data **filtru** hello horní části hello **Audit záznamy** okno.</span><span class="sxs-lookup"><span data-stu-id="fbd49-165">You can view specific dates by clicking **Filter** at hello top of hello **Audit records** blade.</span></span>
    - <span data-ttu-id="fbd49-166">Můžete přepínat mezi záznamy auditu, které byly vytvořeny auditu pro zásady zásady nebo databázi serveru.</span><span class="sxs-lookup"><span data-stu-id="fbd49-166">You can switch between audit records that were created by a server policy or database policy audit.</span></span>

       ![Navigační podokno][8]

* <span data-ttu-id="fbd49-168">Pomocí funkce systému hello **sys.fn_get_audit_file** data protokolu (T-SQL) tooreturn hello auditu v tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="fbd49-168">Use hello system function **sys.fn_get_audit_file** (T-SQL) tooreturn hello audit log data in tabular format.</span></span> <span data-ttu-id="fbd49-169">Další informace o použití této funkce najdete v tématu hello [sys.fn_get_audit_file dokumentaci](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="fbd49-169">For more information on using this function, see hello [sys.fn_get_audit_file documentation](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span></span>


* <span data-ttu-id="fbd49-170">Použití **sloučení soubory auditu** v aplikaci SQL Server Management Studio (počínaje SSMS 17):</span><span class="sxs-lookup"><span data-stu-id="fbd49-170">Use **Merge Audit Files** in SQL Server Management Studio (starting with SSMS 17):</span></span>  
    1. <span data-ttu-id="fbd49-171">Hello SSMS nabídce vyberte **soubor** > **otevřete** > **sloučení soubory auditu**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-171">From hello SSMS menu, select **File** > **Open** > **Merge Audit Files**.</span></span>

        ![Navigační podokno][9]
    2. <span data-ttu-id="fbd49-173">Hello **přidat soubory auditu** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fbd49-173">hello **Add Audit Files** dialog box opens.</span></span> <span data-ttu-id="fbd49-174">Vyberte jednu z hello **přidat** možnost zvolte, zda toomerge auditu soubory z místního disku nebo importovat z Azure Storage (vám bude požadované tooprovide podrobnosti úložiště Azure a klíč účtu).</span><span class="sxs-lookup"><span data-stu-id="fbd49-174">Select one of hello **Add** options to choose whether toomerge audit files from a local disk or import them from Azure Storage (you will be required tooprovide your Azure Storage details and account key).</span></span>

    3. <span data-ttu-id="fbd49-175">Po přidání všech souborů toomerge, klikněte na tlačítko **OK** operace sloučení toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-175">After all files toomerge have been added, click **OK** toocomplete hello merge operation.</span></span>

    4. <span data-ttu-id="fbd49-176">Hello sloučené soubor se otevře v aplikaci SSMS, kde můžete zobrazit a analyzovat je i jako export ho tooan souboru XEL nebo CSV souboru nebo tooa tabulky.</span><span class="sxs-lookup"><span data-stu-id="fbd49-176">hello merged file opens in SSMS, where you can view and analyze it, as well as export it tooan XEL or CSV file or tooa table.</span></span>

* <span data-ttu-id="fbd49-177">Použití hello [synchronizace aplikace](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) které jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fbd49-177">Use hello [sync application](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) that we have created.</span></span> <span data-ttu-id="fbd49-178">Jeho spuštění v Azure a využívá analýzy protokolů Operations Management Suite (OMS) veřejné rozhraní API toopush SQL protokoly auditu do OMS.</span><span class="sxs-lookup"><span data-stu-id="fbd49-178">It runs in Azure and utilizes Operations Management Suite (OMS) Log Analytics public APIs toopush SQL audit logs into OMS.</span></span> <span data-ttu-id="fbd49-179">synchronizace aplikace Hello doručí protokoly auditu SQL do OMS analýzy protokolů pro používání prostřednictvím řídicího panelu analýzy protokolů OMS hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-179">hello sync application pushes SQL audit logs into OMS Log Analytics for consumption via hello OMS Log Analytics dashboard.</span></span> 

* <span data-ttu-id="fbd49-180">Pomocí Power BI.</span><span class="sxs-lookup"><span data-stu-id="fbd49-180">Use Power BI.</span></span> <span data-ttu-id="fbd49-181">Můžete zobrazit a analyzovat data protokolu auditování v Power BI.</span><span class="sxs-lookup"><span data-stu-id="fbd49-181">You can view and analyze audit log data in Power BI.</span></span> <span data-ttu-id="fbd49-182">Další informace o [Power BI a přístup ke stažení šablony](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span><span class="sxs-lookup"><span data-stu-id="fbd49-182">Learn more about [Power BI, and access a downloadable template](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span></span>

* <span data-ttu-id="fbd49-183">Stáhnout soubory protokolů z vašeho kontejneru objektů blob Azure Storage prostřednictvím hello portálu nebo pomocí nástroje, jako například [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="fbd49-183">Download log files from your Azure Storage blob container via hello portal or by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
    * <span data-ttu-id="fbd49-184">Po stažení souboru protokolu, který je místně, můžete dvakrát kliknete na soubor tooopen hello, zobrazit a analyzovat protokoly hello v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="fbd49-184">After you have downloaded a log file locally, you can double-click hello file tooopen, view, and analyze hello logs in SSMS.</span></span>
    * <span data-ttu-id="fbd49-185">Můžete také stáhnout víc souborů současně prostřednictvím Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="fbd49-185">You can also download multiple files simultaneously via Azure Storage Explorer.</span></span> <span data-ttu-id="fbd49-186">Klikněte pravým tlačítkem na konkrétní podsložku (například do podsložky, která obsahuje všechny soubory protokolu pro konkrétní datum) a vyberte **uložit jako** toosave do místní složky.</span><span class="sxs-lookup"><span data-stu-id="fbd49-186">Right-click a specific subfolder (for example, a subfolder that includes all log files for a specific date) and select **Save as** toosave in a local folder.</span></span>

* <span data-ttu-id="fbd49-187">Další metody:</span><span class="sxs-lookup"><span data-stu-id="fbd49-187">Additional methods:</span></span>
   * <span data-ttu-id="fbd49-188">Po stažení několik souborů (nebo na podsložku, která zahrnuje soubory protokolu pro celý den, jak je popsáno v předchozí položce hello v tomto seznamu), můžete je sloučit místně popsané v pokynech soubory auditu sloučení SSMS hello popsané výše.</span><span class="sxs-lookup"><span data-stu-id="fbd49-188">After downloading several files (or a subfolder that includes log files for an entire day, as described in hello previous item in this list), you can merge them locally as described in hello SSMS Merge Audit Files instructions described earlier.</span></span>

   * <span data-ttu-id="fbd49-189">Auditování objektů blob k zobrazení protokolů prostřednictvím kódu programu:</span><span class="sxs-lookup"><span data-stu-id="fbd49-189">View blob auditing logs programmatically:</span></span>

     * <span data-ttu-id="fbd49-190">Použití hello [rozšířené události čtečky](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) knihovna jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="fbd49-190">Use hello [Extended Events Reader](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# library.</span></span>
     * <span data-ttu-id="fbd49-191">[Dotaz Rozšířené události soubory](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fbd49-191">[Query Extended Events Files](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) by using PowerShell.</span></span>




## <span data-ttu-id="fbd49-192"><a id="subheading-5"></a>Provozní postupy</span><span class="sxs-lookup"><span data-stu-id="fbd49-192"><a id="subheading-5"></a>Production practices</span></span>
<!--hello description in this section refers toopreceding screen captures.-->

### <span data-ttu-id="fbd49-193"><a id="subheading-6">Auditování geograficky replikované databáze</a></span><span class="sxs-lookup"><span data-stu-id="fbd49-193"><a id="subheading-6">Auditing geo-replicated databases</a></span></span>
<span data-ttu-id="fbd49-194">Pokud používáte geograficky replikované databáze, je možné tooset až auditování na hello primární databázi, hello sekundární databáze nebo obojí, v závislosti na typu auditu hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-194">When you use geo-replicated databases, it is possible tooset up auditing on either hello primary database, hello secondary database, or both, depending on hello audit type.</span></span>

<span data-ttu-id="fbd49-195">Postupujte podle těchto pokynů (Nezapomeňte, že auditování objektů blob lze zapnout nebo vypnout pouze z primární databáze hello nastavení auditování):</span><span class="sxs-lookup"><span data-stu-id="fbd49-195">Follow these instructions (remember that blob auditing can be turned on or off only from hello primary database auditing settings):</span></span>

* <span data-ttu-id="fbd49-196">**Primární databáze**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-196">**Primary database**.</span></span> <span data-ttu-id="fbd49-197">Zapnout auditování objektů blob, hello server buď hello databáze, jak je popsáno v hello [nastavit auditování pro databázi](#subheading-2) části.</span><span class="sxs-lookup"><span data-stu-id="fbd49-197">Turn on blob auditing, either on hello server or on hello database itself, as described in hello [Set up auditing for your database](#subheading-2) section.</span></span>
* <span data-ttu-id="fbd49-198">**Sekundární databáze**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-198">**Secondary database**.</span></span> <span data-ttu-id="fbd49-199">Zapnout auditování objektů blob v hello primární databázi, jak je popsáno v hello [nastavit auditování pro databázi](#subheading-2) části.</span><span class="sxs-lookup"><span data-stu-id="fbd49-199">Turn on blob auditing on hello primary database, as described in hello [Set up auditing for your database](#subheading-2) section.</span></span> 
   * <span data-ttu-id="fbd49-200">Auditování objektů BLOB musí být povolená na hello *primární databázi, samotné*, ne server hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-200">Blob auditing must be enabled on hello *primary database itself*, not hello server.</span></span>
   * <span data-ttu-id="fbd49-201">Povolíte auditování objektů blob na hello primární databázi, bude ho také přístupné na hello sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="fbd49-201">After blob auditing is enabled on hello primary database, it will also become enabled on hello secondary database.</span></span>

     >[!IMPORTANT]
     ><span data-ttu-id="fbd49-202">Ve výchozím nastavení úložiště hello hello sekundární databáze bude identické toothose hello primární databázi, způsobuje provoz mezi místní.</span><span class="sxs-lookup"><span data-stu-id="fbd49-202">By default, hello storage settings for hello secondary database will be identical toothose of hello primary database, causing cross-regional traffic.</span></span> <span data-ttu-id="fbd49-203">To se můžete vyhnout povolením auditování objektů blob na sekundárním serveru hello a konfigurace místní úložiště v nastavení úložiště sekundární server hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-203">You can avoid this by enabling blob auditing on hello secondary server and configuring local storage in hello secondary server storage settings.</span></span> <span data-ttu-id="fbd49-204">Tím se přepíše umístění úložiště hello hello sekundární databáze a výsledkem každou databázi ukládání jeho úložiště toolocal protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="fbd49-204">This will override hello storage location for hello secondary database and result in each database saving its audit logs toolocal storage.</span></span>  
<br>

### <span data-ttu-id="fbd49-205"><a id="subheading-6">Opětovné generování klíče úložiště</a></span><span class="sxs-lookup"><span data-stu-id="fbd49-205"><a id="subheading-6">Storage key regeneration</a></span></span>
<span data-ttu-id="fbd49-206">V produkčním prostředí budete pravděpodobně toorefresh úložiště klíčů pravidelně.</span><span class="sxs-lookup"><span data-stu-id="fbd49-206">In production, you are likely toorefresh your storage keys periodically.</span></span> <span data-ttu-id="fbd49-207">Při aktualizaci klíče, je nutné zásad auditu tooresave hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-207">When refreshing your keys, you need tooresave hello auditing policy.</span></span> <span data-ttu-id="fbd49-208">proces Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fbd49-208">hello process is as follows:</span></span>

1. <span data-ttu-id="fbd49-209">Otevřete hello **podrobnosti úložiště** okno.</span><span class="sxs-lookup"><span data-stu-id="fbd49-209">Open hello **Storage Details** blade.</span></span> <span data-ttu-id="fbd49-210">V hello **přístupový klíč k úložišti** vyberte **sekundární**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-210">In hello **Storage Access Key** box, select **Secondary**, and click **OK**.</span></span> <span data-ttu-id="fbd49-211">Pak klikněte na tlačítko **Uložit** hello horní části hello okno Konfigurace auditování.</span><span class="sxs-lookup"><span data-stu-id="fbd49-211">Then click **Save** at hello top of hello auditing configuration blade.</span></span>

    ![Navigační podokno][5]
2. <span data-ttu-id="fbd49-213">Přejděte toohello úložiště konfigurace okno a znovu vygenerovat primární přístupový klíč hello.</span><span class="sxs-lookup"><span data-stu-id="fbd49-213">Go toohello storage configuration blade and regenerate hello primary access key.</span></span>

    ![Navigační podokno][6]
3. <span data-ttu-id="fbd49-215">Přejděte zpět toohello auditování okno konfigurace, přepínač přístupový klíč k úložišti hello ze sekundární tooprimary a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="fbd49-215">Go back toohello auditing configuration blade, switch hello storage access key from secondary tooprimary, and then click **OK**.</span></span> <span data-ttu-id="fbd49-216">Pak klikněte na tlačítko **Uložit** hello horní části hello okno Konfigurace auditování.</span><span class="sxs-lookup"><span data-stu-id="fbd49-216">Then click **Save** at hello top of hello auditing configuration blade.</span></span>
4. <span data-ttu-id="fbd49-217">Vraťte se zpátky toohello úložiště konfigurace okno a znovu generovat hello sekundární přístupový klíč (v rámci přípravy cyklus aktualizace hello Další klíč).</span><span class="sxs-lookup"><span data-stu-id="fbd49-217">Go back toohello storage configuration blade and regenerate hello secondary access key (in preparation for hello next key's refresh cycle).</span></span>

## <span data-ttu-id="fbd49-218"><a id="subheading-7"></a>Automatizace (prostředí PowerShell nebo REST API)</span><span class="sxs-lookup"><span data-stu-id="fbd49-218"><a id="subheading-7"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="fbd49-219">Můžete taky nakonfigurovat auditování ve službě Azure SQL Database pomocí hello následující automatizace nástroje:</span><span class="sxs-lookup"><span data-stu-id="fbd49-219">You can also configure auditing in Azure SQL Database by using hello following automation tools:</span></span>

* <span data-ttu-id="fbd49-220">**Rutiny prostředí PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="fbd49-220">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="fbd49-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="fbd49-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="fbd49-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="fbd49-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="fbd49-223">[Odebrat AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="fbd49-223">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="fbd49-224">[Odebrat AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="fbd49-224">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="fbd49-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="fbd49-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="fbd49-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="fbd49-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="fbd49-227">[Použití AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="fbd49-227">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

   <span data-ttu-id="fbd49-228">Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fbd49-228">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

* <span data-ttu-id="fbd49-229">**REST API – auditování objektů Blob**:</span><span class="sxs-lookup"><span data-stu-id="fbd49-229">**REST API - Blob auditing**:</span></span>

   * [<span data-ttu-id="fbd49-230">Vytvořit nebo aktualizovat zásady auditování Blob databáze</span><span class="sxs-lookup"><span data-stu-id="fbd49-230">Create or Update Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [<span data-ttu-id="fbd49-231">Vytvořit nebo aktualizovat Server Blob zásady auditování</span><span class="sxs-lookup"><span data-stu-id="fbd49-231">Create or Update Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [<span data-ttu-id="fbd49-232">Získání objektu Blob databáze zásady auditování</span><span class="sxs-lookup"><span data-stu-id="fbd49-232">Get Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [<span data-ttu-id="fbd49-233">Získání objektu Blob serveru zásady auditování</span><span class="sxs-lookup"><span data-stu-id="fbd49-233">Get Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [<span data-ttu-id="fbd49-234">Získání objektu Blob serveru auditování výsledek operace</span><span class="sxs-lookup"><span data-stu-id="fbd49-234">Get Server Blob Auditing Operation Result</span></span>](https://msdn.microsoft.com/library/azure/mt771862.aspx)


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
