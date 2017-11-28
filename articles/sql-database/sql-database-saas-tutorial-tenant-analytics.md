---
title: "analytické dotazy aaaRun proti více databází Azure SQL | Microsoft Docs"
description: "Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: f2664e4aafd2fecc98d20d229342bca19b0b08c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a><span data-ttu-id="51b8e-104">Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu</span><span class="sxs-lookup"><span data-stu-id="51b8e-104">Extract data from tenant databases into an analytics database for offline analysis</span></span>

<span data-ttu-id="51b8e-105">V tomto kurzu použijete elastické úlohy toorun dotazy pro každou databázi klienta.</span><span class="sxs-lookup"><span data-stu-id="51b8e-105">In this tutorial, you use an elastic job toorun queries against each tenant database.</span></span> <span data-ttu-id="51b8e-106">Úloha Hello extrahuje data prodeje lístků a načte ji do databáze analýzy (nebo datového skladu) pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="51b8e-106">hello job extracts ticket sales data and loads it into an analytics database (or data warehouse) for analysis.</span></span> <span data-ttu-id="51b8e-107">Hello analytics databáze je potom dotaz tooextract přehledy z této každodenní provozních dat všech klientů.</span><span class="sxs-lookup"><span data-stu-id="51b8e-107">hello analytics database is then queried tooextract insights from this day-to-day operational data of all tenants.</span></span>


<span data-ttu-id="51b8e-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="51b8e-108">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="51b8e-109">Vytvoření databáze analýzy hello klienta</span><span class="sxs-lookup"><span data-stu-id="51b8e-109">Create hello tenant analytics database</span></span>
> * <span data-ttu-id="51b8e-110">Vytvoření tooretrieve dat naplánovanou úlohu a naplnění databáze analýzy hello</span><span class="sxs-lookup"><span data-stu-id="51b8e-110">Create a scheduled job tooretrieve data and populate hello analytics database</span></span>

<span data-ttu-id="51b8e-111">toocomplete splnění tohoto kurzu, ujistěte se, hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="51b8e-111">toocomplete this tutorial, make sure hello following prerequisites are met:</span></span>

* <span data-ttu-id="51b8e-112">Hello Wingtip SaaS aplikace je nasazená.</span><span class="sxs-lookup"><span data-stu-id="51b8e-112">hello Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="51b8e-113">toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="51b8e-113">toodeploy in less than five minutes, see [Deploy and explore hello Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="51b8e-114">Je nainstalované prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="51b8e-114">Azure PowerShell is installed.</span></span> <span data-ttu-id="51b8e-115">Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="51b8e-115">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>
* <span data-ttu-id="51b8e-116">je nainstalovaná nejnovější verze Hello systému SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="51b8e-116">hello latest version of SQL Server Management Studio (SSMS) is installed.</span></span> [<span data-ttu-id="51b8e-117">Stažení a instalace SSMS</span><span class="sxs-lookup"><span data-stu-id="51b8e-117">Download and Install SSMS</span></span>](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a><span data-ttu-id="51b8e-118">Vzor provozní analýzy tenanta</span><span class="sxs-lookup"><span data-stu-id="51b8e-118">Tenant Operational Analytics pattern</span></span>

<span data-ttu-id="51b8e-119">Jedním z velké příležitosti hello s aplikacemi SaaS je toouse hello bohaté klienta data, která je uložená v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="51b8e-119">One of hello great opportunities with SaaS applications is toouse hello rich tenant data that is stored in hello cloud.</span></span> <span data-ttu-id="51b8e-120">Použijte tato data toogain přehledy hello operace a využití vaší aplikace a klienty.</span><span class="sxs-lookup"><span data-stu-id="51b8e-120">Use this data toogain insights into hello operation and usage of your application, and your tenants.</span></span> <span data-ttu-id="51b8e-121">Tato data můžete Průvodce vývoj funkce, vylepšení použitelnost a další investice do aplikace hello a platformu.</span><span class="sxs-lookup"><span data-stu-id="51b8e-121">This data can guide feature development, usability improvements, and other investments in hello app and platform.</span></span> <span data-ttu-id="51b8e-122">V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje.</span><span class="sxs-lookup"><span data-stu-id="51b8e-122">Accessing this data when it's in a single multi-tenant database is easy, but not so easy when distributed at scale across potentially thousands of databases.</span></span> <span data-ttu-id="51b8e-123">Jeden způsob tooaccessing tato data jsou toouse elastické úlohy, které umožňují vracení výsledků výsledků dotazu z toobe provádění úlohy zaznamenat do výstupní databáze a tabulky.</span><span class="sxs-lookup"><span data-stu-id="51b8e-123">One approach tooaccessing this data is toouse Elastic jobs, which enable result-returning query results from job execution toobe captured in an output database and table.</span></span>

## <a name="get-hello-wingtip-application-scripts"></a><span data-ttu-id="51b8e-124">Získat hello Wingtip aplikační skripty</span><span class="sxs-lookup"><span data-stu-id="51b8e-124">Get hello Wingtip application scripts</span></span>

<span data-ttu-id="51b8e-125">Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github.</span><span class="sxs-lookup"><span data-stu-id="51b8e-125">hello Wingtip SaaS scripts and application source code are available in hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="51b8e-126">[Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span><span class="sxs-lookup"><span data-stu-id="51b8e-126">[Steps toodownload hello Wingtip SaaS scripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span></span>

## <a name="deploy-a-database-for-tenant-analytics-results"></a><span data-ttu-id="51b8e-127">Nasazení databáze pro výsledky analýzy tenanta</span><span class="sxs-lookup"><span data-stu-id="51b8e-127">Deploy a database for tenant analytics results</span></span>

<span data-ttu-id="51b8e-128">Tento kurz vyžaduje toohave, které databáze nasazené toocapture hello výsledkem úlohy spouštění skriptů, které obsahují vrací výsledky dotazů.</span><span class="sxs-lookup"><span data-stu-id="51b8e-128">This tutorial requires you toohave a database deployed toocapture hello results from job execution of scripts, which contain results-returning queries.</span></span> <span data-ttu-id="51b8e-129">Pro tento účel si vytvoříme databázi nazvanou tenantanalytics.</span><span class="sxs-lookup"><span data-stu-id="51b8e-129">Let's create a database called tenantanalytics for this purpose.</span></span>

1. <span data-ttu-id="51b8e-130">Otevřete... \\Learning moduly\\provozní Analytics\\klienta Analytics\\*ukázku TenantAnalyticsDB.ps1* v hello *prostředí PowerShell ISE* a nastavte Hello následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="51b8e-130">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in hello *PowerShell ISE* and set hello following value:</span></span>
   * <span data-ttu-id="51b8e-131">**$DemoScenario** = **2***Nasazení provozní analytické databáze* </span><span class="sxs-lookup"><span data-stu-id="51b8e-131">**$DemoScenario** = **2** *Deploy operational analytics database*</span></span>
1. <span data-ttu-id="51b8e-132">Stiskněte klávesu **F5** toorun hello ukázkový skript (hello tohoto volání *nasadit TenantAnalyticsDB.ps1* skriptu) vytváří databáze analýzy hello klienta.</span><span class="sxs-lookup"><span data-stu-id="51b8e-132">Press **F5** toorun hello demo script (that calls hello *Deploy-TenantAnalyticsDB.ps1* script) which creates hello tenant analytics database.</span></span>

## <a name="create-some-data-for-hello-demo"></a><span data-ttu-id="51b8e-133">Vytvoření některá data pro ukázku hello</span><span class="sxs-lookup"><span data-stu-id="51b8e-133">Create some data for hello demo</span></span>

1. <span data-ttu-id="51b8e-134">Otevřete... \\Learning moduly\\provozní Analytics\\klienta Analytics\\*ukázku TenantAnalyticsDB.ps1* v hello *prostředí PowerShell ISE* a nastavte Hello následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="51b8e-134">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in hello *PowerShell ISE* and set hello following value:</span></span>
   * <span data-ttu-id="51b8e-135">**$DemoScenario** = **1***Nákup lístků na akce na všech místech*</span><span class="sxs-lookup"><span data-stu-id="51b8e-135">**$DemoScenario** = **1** *Purchase tickets for events at all venues*</span></span>
1. <span data-ttu-id="51b8e-136">Stiskněte klávesu **F5** toorun hello skriptu a vytvoření lístku zakoupení historie.</span><span class="sxs-lookup"><span data-stu-id="51b8e-136">Press **F5** toorun hello script and create ticket purchasing history.</span></span>


## <a name="create-a-scheduled-job-tooretrieve-tenant-analytics-about-ticket-purchases"></a><span data-ttu-id="51b8e-137">Vytvoření analýza naplánovaná úloha tooretrieve klienta o lístek nákupy</span><span class="sxs-lookup"><span data-stu-id="51b8e-137">Create a scheduled job tooretrieve tenant analytics about ticket purchases</span></span>

<span data-ttu-id="51b8e-138">Tento skript vytvoří informace o nákupu úlohy tooretrieve lístek od všech klientů.</span><span class="sxs-lookup"><span data-stu-id="51b8e-138">This script creates a job tooretrieve ticket purchase information from all tenants.</span></span> <span data-ttu-id="51b8e-139">Jakmile agregován do jedné tabulky, můžete získat bohaté pronikavého metriky o lístek zakoupení vzory napříč hello klientům.</span><span class="sxs-lookup"><span data-stu-id="51b8e-139">Once aggregated into a single table, you can gain rich insightful metrics about ticket purchasing patterns across hello tenants.</span></span>

1. <span data-ttu-id="51b8e-140">Otevřete aplikaci SSMS a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru</span><span class="sxs-lookup"><span data-stu-id="51b8e-140">Open SSMS and connect toohello catalog-&lt;user&gt;.database.windows.net server</span></span>
1. <span data-ttu-id="51b8e-141">Otevřete složku ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*</span><span class="sxs-lookup"><span data-stu-id="51b8e-141">Open ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="51b8e-142">Upravit &lt;uživatele&gt;, použijte hello uživatelské jméno používané při nasazení aplikace Wingtip SaaS hello v horní části hello hello skriptu **sp\_přidat\_cíl\_skupiny\_člen** a **sp\_přidat\_krok úlohy**</span><span class="sxs-lookup"><span data-stu-id="51b8e-142">Modify &lt;User&gt;, use hello user name used when you deployed hello Wingtip SaaS app at hello top of hello script, **sp\_add\_target\_group\_member** and **sp\_add\_jobstep**</span></span>
1. <span data-ttu-id="51b8e-143">Klikněte pravým tlačítkem, vyberte **připojení**a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.</span><span class="sxs-lookup"><span data-stu-id="51b8e-143">Right click, select **Connection**, and connect toohello catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="51b8e-144">Ujistěte se, jsou připojené toohello **jobaccount** databáze a stiskněte klávesu **F5** pro spuštění skriptu hello</span><span class="sxs-lookup"><span data-stu-id="51b8e-144">Ensure you are connected toohello **jobaccount** database and press **F5** to run hello script</span></span>

* <span data-ttu-id="51b8e-145">**SP\_přidat\_cíl\_skupiny** vytvoří hello cílová skupina *TenantGroup*, nyní potřebujeme tooadd cíl členy.</span><span class="sxs-lookup"><span data-stu-id="51b8e-145">**sp\_add\_target\_group** creates hello target group name *TenantGroup*, now we need tooadd target members.</span></span>
* <span data-ttu-id="51b8e-146">**SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je hello customer1 - &lt;Uživatele&gt; server obsahující databáze klienta hello) v čase úlohy by měl být součástí provádění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="51b8e-146">**sp\_add\_target\_group\_member** adds a *server* target member type, which deems all databases within that server (note this is hello customer1-&lt;User&gt; server containing hello tenant databases) at time of job execution should be included in hello job.</span></span>
* <span data-ttu-id="51b8e-147">**sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou Nákup lístků ze všech tenantů.</span><span class="sxs-lookup"><span data-stu-id="51b8e-147">**sp\_add\_job** creates a new weekly scheduled job called “Ticket Purchases from all Tenants”</span></span>
* <span data-ttu-id="51b8e-148">**SP\_přidat\_krok úlohy** vytvoří krok úlohy hello obsahující tooretrieve text T-SQL příkazu všechny informace o nákupu lístku hello ze všech klientů a kopírování hello vrací sadu výsledků do tabulce s názvem  *AllTicketsPurchasesfromAllTenants*</span><span class="sxs-lookup"><span data-stu-id="51b8e-148">**sp\_add\_jobstep** creates hello job step containing T-SQL command text tooretrieve all hello ticket purchase information from all tenants and copy hello returning result set into a table called *AllTicketsPurchasesfromAllTenants*</span></span>
* <span data-ttu-id="51b8e-149">Zbývající zobrazení Hello ve skriptu hello zobrazení hello existenci hello objekty a provádění úlohy monitorování.</span><span class="sxs-lookup"><span data-stu-id="51b8e-149">hello remaining views in hello script display hello existence of hello objects and monitor job execution.</span></span> <span data-ttu-id="51b8e-150">Zkontrolujte hodnotu stavu hello z hello **životního cyklu** sloupec toomonitor hello stavu.</span><span class="sxs-lookup"><span data-stu-id="51b8e-150">Review hello status value from hello **lifecycle** column toomonitor hello status.</span></span> <span data-ttu-id="51b8e-151">Jednou byly úspěšné, hello úloha úspěšně dokončí na všechny databáze klienta a hello dva další databáze, které obsahují hello referenční tabulce.</span><span class="sxs-lookup"><span data-stu-id="51b8e-151">Once, Succeeded, hello job has successfully finished on all tenant databases and hello two additional databases containing hello reference table.</span></span>

<span data-ttu-id="51b8e-152">Úspěšném spuštění skriptu hello by měl mít za následek podobné výsledky:</span><span class="sxs-lookup"><span data-stu-id="51b8e-152">Successfully running hello script should result in similar results:</span></span>

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-tooretrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a><span data-ttu-id="51b8e-154">Vytvoření úlohy tooretrieve, souhrnné počet lístku zakoupí od všech klientů</span><span class="sxs-lookup"><span data-stu-id="51b8e-154">Create a job tooretrieve a summary count of ticket purchases from all tenants</span></span>

<span data-ttu-id="51b8e-155">Tento skript vytvoří úlohu tooretrieve součet všech nákupů lístek od všech klientů.</span><span class="sxs-lookup"><span data-stu-id="51b8e-155">This script creates a job tooretrieve sum of all ticket purchases from all tenants.</span></span>

1. <span data-ttu-id="51b8e-156">Otevřete aplikaci SSMS a připojte toohello *katalogu -&lt;uživatele&gt;. database.windows.net* serveru</span><span class="sxs-lookup"><span data-stu-id="51b8e-156">Open SSMS and connect toohello *catalog-&lt;User&gt;.database.windows.net* server</span></span>
1. <span data-ttu-id="51b8e-157">Otevřete hello souboru... \\Učení moduly\\zřídit a katalog\\provozní Analytics\\klienta Analytics\\*TicketPurchasesfromAllTenants.sql výsledky*</span><span class="sxs-lookup"><span data-stu-id="51b8e-157">Open hello file …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="51b8e-158">Upravit &lt;uživatele&gt;, použijte hello uživatelské jméno používané při nasazení aplikace Wingtip SaaS hello ve skriptu hello v hello **sp\_přidat\_krok úlohy** uložené procedury</span><span class="sxs-lookup"><span data-stu-id="51b8e-158">Modify &lt;User&gt;, use hello user name used when you deployed hello Wingtip SaaS app in hello script, in hello **sp\_add\_jobstep** stored procedure</span></span>
1. <span data-ttu-id="51b8e-159">Klikněte pravým tlačítkem, vyberte **připojení**a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.</span><span class="sxs-lookup"><span data-stu-id="51b8e-159">Right click, select **Connection**, and connect toohello catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="51b8e-160">Ujistěte se, jsou připojené toohello **tenantanalytics** databáze a stiskněte klávesu **F5** pro spuštění skriptu hello</span><span class="sxs-lookup"><span data-stu-id="51b8e-160">Ensure you are connected toohello **tenantanalytics** database and press **F5** to run hello script</span></span>

<span data-ttu-id="51b8e-161">Úspěšném spuštění skriptu hello by měl mít za následek podobné výsledky:</span><span class="sxs-lookup"><span data-stu-id="51b8e-161">Successfully running hello script should result in similar results:</span></span>

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* <span data-ttu-id="51b8e-163">**sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou ResultsTicketsOrders</span><span class="sxs-lookup"><span data-stu-id="51b8e-163">**sp\_add\_job** creates a new weekly scheduled job called “ResultsTicketsOrders”</span></span>

* <span data-ttu-id="51b8e-164">**SP\_přidat\_krok úlohy** vytvoří krok úlohy hello obsahující tooretrieve text T-SQL příkazu všechny informace o nákupu lístku hello ze všech klientů a vrací sadu výsledků do tabulce s názvem CountofTicketOrders hello kopie</span><span class="sxs-lookup"><span data-stu-id="51b8e-164">**sp\_add\_jobstep** creates hello job step containing T-SQL command text tooretrieve all hello ticket purchase information from all tenants and copy hello returning result set into a table called CountofTicketOrders</span></span>

* <span data-ttu-id="51b8e-165">Zbývající zobrazení Hello ve skriptu hello zobrazení hello existenci hello objekty a provádění úlohy monitorování.</span><span class="sxs-lookup"><span data-stu-id="51b8e-165">hello remaining views in hello script display hello existence of hello objects and monitor job execution.</span></span> <span data-ttu-id="51b8e-166">Zkontrolujte hodnotu stavu hello z hello **životního cyklu** sloupec toomonitor hello stavu.</span><span class="sxs-lookup"><span data-stu-id="51b8e-166">Review hello status value from hello **lifecycle** column toomonitor hello status.</span></span> <span data-ttu-id="51b8e-167">Jednou byly úspěšné, hello úloha úspěšně dokončí na všechny databáze klienta a hello dva další databáze, které obsahují hello referenční tabulce.</span><span class="sxs-lookup"><span data-stu-id="51b8e-167">Once, Succeeded, hello job has successfully finished on all tenant databases and hello two additional databases containing hello reference table.</span></span>


## <a name="next-steps"></a><span data-ttu-id="51b8e-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51b8e-168">Next steps</span></span>

<span data-ttu-id="51b8e-169">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="51b8e-169">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="51b8e-170">Nasazení analytické databáze tenantů</span><span class="sxs-lookup"><span data-stu-id="51b8e-170">Deploy a tenant analytics database</span></span>
> * <span data-ttu-id="51b8e-171">Vytvoření naplánované úlohy analytická data tooretrieve mezi klienty</span><span class="sxs-lookup"><span data-stu-id="51b8e-171">Create a scheduled job tooretrieve analytical data across tenants</span></span>

<span data-ttu-id="51b8e-172">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="51b8e-172">Congratulations!</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51b8e-173">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="51b8e-173">Additional resources</span></span>

* <span data-ttu-id="51b8e-174">Další [návodů, které stavějí hello Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span><span class="sxs-lookup"><span data-stu-id="51b8e-174">Additional [tutorials that build upon hello Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span></span>
* [<span data-ttu-id="51b8e-175">Elastické úlohy</span><span class="sxs-lookup"><span data-stu-id="51b8e-175">Elastic Jobs</span></span>](sql-database-elastic-jobs-overview.md)
