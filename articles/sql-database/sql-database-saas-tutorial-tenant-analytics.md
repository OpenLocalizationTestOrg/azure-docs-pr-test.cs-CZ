---
title: "Spouštění analytických dotazů pro více databází SQL Azure | Dokumentace Microsoftu"
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
ms.openlocfilehash: 4e32407d5f321198358e07980907c3420aaf56c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a><span data-ttu-id="200f9-104">Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu</span><span class="sxs-lookup"><span data-stu-id="200f9-104">Extract data from tenant databases into an analytics database for offline analysis</span></span>

<span data-ttu-id="200f9-105">V tomto kurzu použijete elastické úlohy ke spouštění dotazů na každou databázi klienta.</span><span class="sxs-lookup"><span data-stu-id="200f9-105">In this tutorial, you use an elastic job to run queries against each tenant database.</span></span> <span data-ttu-id="200f9-106">Úloha extrahuje data prodeje lístků a načte ji do databáze analýzy (nebo datového skladu) pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="200f9-106">The job extracts ticket sales data and loads it into an analytics database (or data warehouse) for analysis.</span></span> <span data-ttu-id="200f9-107">Databáze analýzy je pak dotazován extrahovat statistiky z této každodenní provozních dat všech klientů.</span><span class="sxs-lookup"><span data-stu-id="200f9-107">The analytics database is then queried to extract insights from this day-to-day operational data of all tenants.</span></span>


<span data-ttu-id="200f9-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="200f9-108">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="200f9-109">Vytvoření analytické databáze tenantů</span><span class="sxs-lookup"><span data-stu-id="200f9-109">Create the tenant analytics database</span></span>
> * <span data-ttu-id="200f9-110">Vytvoření plánované úlohy k načtení dat a naplnění analytické databáze</span><span class="sxs-lookup"><span data-stu-id="200f9-110">Create a scheduled job to retrieve data and populate the analytics database</span></span>

<span data-ttu-id="200f9-111">Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:</span><span class="sxs-lookup"><span data-stu-id="200f9-111">To complete this tutorial, make sure the following prerequisites are met:</span></span>

* <span data-ttu-id="200f9-112">Adresář Wingtip SaaS aplikace je nasazená.</span><span class="sxs-lookup"><span data-stu-id="200f9-112">The Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="200f9-113">Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="200f9-113">To deploy in less than five minutes, see [Deploy and explore the Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="200f9-114">Je nainstalované prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="200f9-114">Azure PowerShell is installed.</span></span> <span data-ttu-id="200f9-115">Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="200f9-115">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>
* <span data-ttu-id="200f9-116">Je nainstalovaná nejnovější verze SQL Server Management Studia (SSMS).</span><span class="sxs-lookup"><span data-stu-id="200f9-116">The latest version of SQL Server Management Studio (SSMS) is installed.</span></span> [<span data-ttu-id="200f9-117">Stažení a instalace SSMS</span><span class="sxs-lookup"><span data-stu-id="200f9-117">Download and Install SSMS</span></span>](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a><span data-ttu-id="200f9-118">Vzor provozní analýzy tenanta</span><span class="sxs-lookup"><span data-stu-id="200f9-118">Tenant Operational Analytics pattern</span></span>

<span data-ttu-id="200f9-119">Jednou ze skvělých příležitostí, které nabízejí aplikace SaaS, je používání rozsáhlého množství dat tenantů, která jsou uložená v cloudu.</span><span class="sxs-lookup"><span data-stu-id="200f9-119">One of the great opportunities with SaaS applications is to use the rich tenant data that is stored in the cloud.</span></span> <span data-ttu-id="200f9-120">Tato data je možné využívat k získání přehledu o provozu a používání vaší aplikace a tenanta.</span><span class="sxs-lookup"><span data-stu-id="200f9-120">Use this data to gain insights into the operation and usage of your application, and your tenants.</span></span> <span data-ttu-id="200f9-121">Můžou řídit vývoj funkcí, vylepšení použitelnosti a další investice do aplikace a platformy.</span><span class="sxs-lookup"><span data-stu-id="200f9-121">This data can guide feature development, usability improvements, and other investments in the app and platform.</span></span> <span data-ttu-id="200f9-122">V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje.</span><span class="sxs-lookup"><span data-stu-id="200f9-122">Accessing this data when it's in a single multi-tenant database is easy, but not so easy when distributed at scale across potentially thousands of databases.</span></span> <span data-ttu-id="200f9-123">Jedním z přístupů k těmto datům je použití elastických úloh, které umožňují zaznamenat výsledky dotazů vracejících výsledky z provedení úlohy do výstupní databáze a tabulky.</span><span class="sxs-lookup"><span data-stu-id="200f9-123">One approach to accessing this data is to use Elastic jobs, which enable result-returning query results from job execution to be captured in an output database and table.</span></span>

## <a name="get-the-wingtip-application-scripts"></a><span data-ttu-id="200f9-124">Získání skriptů aplikace Wingtip</span><span class="sxs-lookup"><span data-stu-id="200f9-124">Get the Wingtip application scripts</span></span>

<span data-ttu-id="200f9-125">Adresář Wingtip SaaS skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github.</span><span class="sxs-lookup"><span data-stu-id="200f9-125">The Wingtip SaaS scripts and application source code are available in the [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="200f9-126">[Postup stažení skripty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span><span class="sxs-lookup"><span data-stu-id="200f9-126">[Steps to download the Wingtip SaaS scripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span></span>

## <a name="deploy-a-database-for-tenant-analytics-results"></a><span data-ttu-id="200f9-127">Nasazení databáze pro výsledky analýzy tenanta</span><span class="sxs-lookup"><span data-stu-id="200f9-127">Deploy a database for tenant analytics results</span></span>

<span data-ttu-id="200f9-128">Tento kurz vyžaduje, abyste měli nasazenou databázi, do které se budou zaznamenávat výsledky z provedení úlohy se skripty, která obsahuje dotazy vracející výsledky.</span><span class="sxs-lookup"><span data-stu-id="200f9-128">This tutorial requires you to have a database deployed to capture the results from job execution of scripts, which contain results-returning queries.</span></span> <span data-ttu-id="200f9-129">Pro tento účel si vytvoříme databázi nazvanou tenantanalytics.</span><span class="sxs-lookup"><span data-stu-id="200f9-129">Let's create a database called tenantanalytics for this purpose.</span></span>

1. <span data-ttu-id="200f9-130">Otevřete skript …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* ve složce *PowerShell ISE* a nastavte následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="200f9-130">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in the *PowerShell ISE* and set the following value:</span></span>
   * <span data-ttu-id="200f9-131">**$DemoScenario** = **2** *Nasazení provozní analytické databáze* </span><span class="sxs-lookup"><span data-stu-id="200f9-131">**$DemoScenario** = **2** *Deploy operational analytics database*</span></span>
1. <span data-ttu-id="200f9-132">Stiskněte klávesu **F5** ke spuštění ukázkového skriptu (který volá skript *Deploy-TenantAnalyticsDB.ps1*), který vytvoří databázi analýzy tenanta.</span><span class="sxs-lookup"><span data-stu-id="200f9-132">Press **F5** to run the demo script (that calls the *Deploy-TenantAnalyticsDB.ps1* script) which creates the tenant analytics database.</span></span>

## <a name="create-some-data-for-the-demo"></a><span data-ttu-id="200f9-133">Vytvoření dat pro ukázku</span><span class="sxs-lookup"><span data-stu-id="200f9-133">Create some data for the demo</span></span>

1. <span data-ttu-id="200f9-134">Otevřete skript …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* ve složce *PowerShell ISE* a nastavte následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="200f9-134">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in the *PowerShell ISE* and set the following value:</span></span>
   * <span data-ttu-id="200f9-135">**$DemoScenario** = **1** *Nákup lístků na akce na všech místech*</span><span class="sxs-lookup"><span data-stu-id="200f9-135">**$DemoScenario** = **1** *Purchase tickets for events at all venues*</span></span>
1. <span data-ttu-id="200f9-136">Stiskněte klávesu **F5** ke spuštění skriptu a vytvoření historie nákupu lístků.</span><span class="sxs-lookup"><span data-stu-id="200f9-136">Press **F5** to run the script and create ticket purchasing history.</span></span>


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a><span data-ttu-id="200f9-137">Vytvoření plánované úlohy k načtení analýzy tenanta k nákupu lístků</span><span class="sxs-lookup"><span data-stu-id="200f9-137">Create a scheduled job to retrieve tenant analytics about ticket purchases</span></span>

<span data-ttu-id="200f9-138">Tento skript vytvoří úlohu k načtení informací o nákupu lístků ze všech tenantů.</span><span class="sxs-lookup"><span data-stu-id="200f9-138">This script creates a job to retrieve ticket purchase information from all tenants.</span></span> <span data-ttu-id="200f9-139">Po shrnutí do jedné tabulky můžete získat bohatou informační metriku o vzorcích nákupu lístků napříč tenanty.</span><span class="sxs-lookup"><span data-stu-id="200f9-139">Once aggregated into a single table, you can gain rich insightful metrics about ticket purchasing patterns across the tenants.</span></span>

1. <span data-ttu-id="200f9-140">Otevřete SSMS a připojte se k serveru catalog-&lt;user&gt;.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="200f9-140">Open SSMS and connect to the catalog-&lt;user&gt;.database.windows.net server</span></span>
1. <span data-ttu-id="200f9-141">Otevřete složku ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*</span><span class="sxs-lookup"><span data-stu-id="200f9-141">Open ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="200f9-142">Upravit &lt;uživatele&gt;, použít uživatelské jméno použít při nasazení aplikace Wingtip SaaS v horní části na skript, **sp\_přidat\_cíl\_skupiny\_člen** a **sp\_přidat\_krok úlohy**</span><span class="sxs-lookup"><span data-stu-id="200f9-142">Modify &lt;User&gt;, use the user name used when you deployed the Wingtip SaaS app at the top of the script, **sp\_add\_target\_group\_member** and **sp\_add\_jobstep**</span></span>
1. <span data-ttu-id="200f9-143">Klikněte pravým tlačítkem, vyberte **připojení**a připojte se k katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.</span><span class="sxs-lookup"><span data-stu-id="200f9-143">Right click, select **Connection**, and connect to the catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="200f9-144">Zkontrolujte, že jste připojení k databázi **jobaccount**, a stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="200f9-144">Ensure you are connected to the **jobaccount** database and press **F5** to run the script</span></span>

* <span data-ttu-id="200f9-145">**sp\_add\_target\_group** vytvoří cílovou skupinu s názvem *TenantGroup*. Teď potřebujeme přidat cílové členy.</span><span class="sxs-lookup"><span data-stu-id="200f9-145">**sp\_add\_target\_group** creates the target group name *TenantGroup*, now we need to add target members.</span></span>
* <span data-ttu-id="200f9-146">**SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je customer1-&lt; Uživatel&gt; server obsahující databáze klienta) v čase úlohy by měl být součástí provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="200f9-146">**sp\_add\_target\_group\_member** adds a *server* target member type, which deems all databases within that server (note this is the customer1-&lt;User&gt; server containing the tenant databases) at time of job execution should be included in the job.</span></span>
* <span data-ttu-id="200f9-147">**sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou Nákup lístků ze všech tenantů.</span><span class="sxs-lookup"><span data-stu-id="200f9-147">**sp\_add\_job** creates a new weekly scheduled job called “Ticket Purchases from all Tenants”</span></span>
* <span data-ttu-id="200f9-148">**sp\_add\_jobstep** vytvoří krok úlohy, který obsahuje text příkazu T-SQL k načtení všech informací o nákupu lístků ze všech tenantů, a zkopíruje výslednou sadu výsledků do tabulky nazvané *AllTicketsPurchasesfromAllTenants*</span><span class="sxs-lookup"><span data-stu-id="200f9-148">**sp\_add\_jobstep** creates the job step containing T-SQL command text to retrieve all the ticket purchase information from all tenants and copy the returning result set into a table called *AllTicketsPurchasesfromAllTenants*</span></span>
* <span data-ttu-id="200f9-149">Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="200f9-149">The remaining views in the script display the existence of the objects and monitor job execution.</span></span> <span data-ttu-id="200f9-150">Zkontrolujte stavovou hodnotu ze sloupce **lifecycle**, která vám umožní monitorovat stav.</span><span class="sxs-lookup"><span data-stu-id="200f9-150">Review the status value from the **lifecycle** column to monitor the status.</span></span> <span data-ttu-id="200f9-151">V případě úspěchu je úloha zdárně dokončena na všech databázích tenantů i na dvou dalších databázích obsahujících referenční tabulku.</span><span class="sxs-lookup"><span data-stu-id="200f9-151">Once, Succeeded, the job has successfully finished on all tenant databases and the two additional databases containing the reference table.</span></span>

<span data-ttu-id="200f9-152">Úspěšné spuštění skriptu by mělo vrátit podobné výsledky:</span><span class="sxs-lookup"><span data-stu-id="200f9-152">Successfully running the script should result in similar results:</span></span>

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a><span data-ttu-id="200f9-154">Vytvoření úlohy k načtení souhrnného počtu nákupů lístků ze všech tenantů</span><span class="sxs-lookup"><span data-stu-id="200f9-154">Create a job to retrieve a summary count of ticket purchases from all tenants</span></span>

<span data-ttu-id="200f9-155">Tento skript vytvoří úlohu k načtení souhrnu všech nákupů lístků ze všech tenantů.</span><span class="sxs-lookup"><span data-stu-id="200f9-155">This script creates a job to retrieve sum of all ticket purchases from all tenants.</span></span>

1. <span data-ttu-id="200f9-156">Otevřete SSMS a připojte se k serveru*catalog-&lt;User&gt;.database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="200f9-156">Open SSMS and connect to the *catalog-&lt;User&gt;.database.windows.net* server</span></span>
1. <span data-ttu-id="200f9-157">Otevřete skript …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*</span><span class="sxs-lookup"><span data-stu-id="200f9-157">Open the file …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="200f9-158">Upravit &lt;uživatele&gt;, použít uživatelské jméno použít při nasazení aplikace Wingtip SaaS ve skriptu, v **sp\_přidat\_krok úlohy** uložené procedury</span><span class="sxs-lookup"><span data-stu-id="200f9-158">Modify &lt;User&gt;, use the user name used when you deployed the Wingtip SaaS app in the script, in the **sp\_add\_jobstep** stored procedure</span></span>
1. <span data-ttu-id="200f9-159">Klikněte pravým tlačítkem, vyberte **připojení**a připojte se k katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.</span><span class="sxs-lookup"><span data-stu-id="200f9-159">Right click, select **Connection**, and connect to the catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="200f9-160">Zkontrolujte, že jste připojení k databázi **tenantanalytics**, a stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="200f9-160">Ensure you are connected to the **tenantanalytics** database and press **F5** to run the script</span></span>

<span data-ttu-id="200f9-161">Úspěšné spuštění skriptu by mělo vrátit podobné výsledky:</span><span class="sxs-lookup"><span data-stu-id="200f9-161">Successfully running the script should result in similar results:</span></span>

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* <span data-ttu-id="200f9-163">**sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou ResultsTicketsOrders</span><span class="sxs-lookup"><span data-stu-id="200f9-163">**sp\_add\_job** creates a new weekly scheduled job called “ResultsTicketsOrders”</span></span>

* <span data-ttu-id="200f9-164">**sp\_add\_jobstep** vytvoří krok úlohy, který obsahuje text příkazu T-SQL k načtení všech informací o nákupu lístků ze všech tenantů, a zkopíruje výslednou sadu výsledků do tabulky nazvané CountofTicketOrders</span><span class="sxs-lookup"><span data-stu-id="200f9-164">**sp\_add\_jobstep** creates the job step containing T-SQL command text to retrieve all the ticket purchase information from all tenants and copy the returning result set into a table called CountofTicketOrders</span></span>

* <span data-ttu-id="200f9-165">Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="200f9-165">The remaining views in the script display the existence of the objects and monitor job execution.</span></span> <span data-ttu-id="200f9-166">Zkontrolujte stavovou hodnotu ze sloupce **lifecycle**, která vám umožní monitorovat stav.</span><span class="sxs-lookup"><span data-stu-id="200f9-166">Review the status value from the **lifecycle** column to monitor the status.</span></span> <span data-ttu-id="200f9-167">V případě úspěchu je úloha zdárně dokončena na všech databázích tenantů i na dvou dalších databázích obsahujících referenční tabulku.</span><span class="sxs-lookup"><span data-stu-id="200f9-167">Once, Succeeded, the job has successfully finished on all tenant databases and the two additional databases containing the reference table.</span></span>


## <a name="next-steps"></a><span data-ttu-id="200f9-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="200f9-168">Next steps</span></span>

<span data-ttu-id="200f9-169">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="200f9-169">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="200f9-170">Nasazení analytické databáze tenantů</span><span class="sxs-lookup"><span data-stu-id="200f9-170">Deploy a tenant analytics database</span></span>
> * <span data-ttu-id="200f9-171">Vytvoření plánované úlohy k načtení analytických dat mezi tenanty</span><span class="sxs-lookup"><span data-stu-id="200f9-171">Create a scheduled job to retrieve analytical data across tenants</span></span>

<span data-ttu-id="200f9-172">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="200f9-172">Congratulations!</span></span>

## <a name="additional-resources"></a><span data-ttu-id="200f9-173">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="200f9-173">Additional resources</span></span>

* <span data-ttu-id="200f9-174">Další [návodů, které stavět na adresář Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span><span class="sxs-lookup"><span data-stu-id="200f9-174">Additional [tutorials that build upon the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span></span>
* [<span data-ttu-id="200f9-175">Elastické úlohy</span><span class="sxs-lookup"><span data-stu-id="200f9-175">Elastic Jobs</span></span>](sql-database-elastic-jobs-overview.md)
