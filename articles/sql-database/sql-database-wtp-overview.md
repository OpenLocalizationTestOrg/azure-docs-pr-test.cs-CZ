---
title: "Úvod Wingtip SaaS - víceklientské aplikace Azure SQL Database | Microsoft Docs"
description: "Další informace pomocí víceklientské ukázkovou aplikaci, která používá Azure SQL Database, Wingtip SaaS aplikace"
keywords: kurz k sql database
services: sql-database
author: stevestein
manager: jhubbard
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: 6d4a5df599137e95ca5458fae74b8daa565b0338
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-the-wingtip-saas-application"></a><span data-ttu-id="68cd2-104">Úvod k aplikaci Wingtip SaaS</span><span class="sxs-lookup"><span data-stu-id="68cd2-104">Introduction to the Wingtip SaaS application</span></span>

<span data-ttu-id="68cd2-105">*Wingtip SaaS* aplikace je víceklientské ukázkovou aplikaci, která demonstruje jedinečných výhod databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="68cd2-105">The *Wingtip SaaS* application is a sample multi-tenant app, that demonstrates the unique advantages of SQL Database.</span></span> <span data-ttu-id="68cd2-106">Aplikace používá vzorec SaaS aplikace databází na tenanta k poskytování služeb více tenantům.</span><span class="sxs-lookup"><span data-stu-id="68cd2-106">The app uses a database-per-tenant, SaaS application pattern, to service multiple tenants.</span></span> <span data-ttu-id="68cd2-107">Tato aplikace je určená k prezentují funkce databáze SQL Azure, které umožňují SaaS scénáře, včetně několika vzory návrhu a správu SaaS.</span><span class="sxs-lookup"><span data-stu-id="68cd2-107">The app is designed to showcase features of Azure SQL Database that enable SaaS scenarios, including several SaaS design and management patterns.</span></span> <span data-ttu-id="68cd2-108">Pro rychlé zprovoznění, nasadí aplikaci Wingtip SaaS za méně než pět minut!</span><span class="sxs-lookup"><span data-stu-id="68cd2-108">To quickly get up and running, the Wingtip SaaS app deploys in less than five minutes!</span></span>

<span data-ttu-id="68cd2-109">Jsou k dispozici v aplikaci zdrojový kód a správu skriptů [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github.</span><span class="sxs-lookup"><span data-stu-id="68cd2-109">Application source code and management scripts are available in the [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="68cd2-110">Ke spouštění skriptů, [Learning moduly složka pro stahování](#download-and-unblock-the-wingtip-saas-scripts) do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="68cd2-110">To run the scripts, [download the Learning Modules folder](#download-and-unblock-the-wingtip-saas-scripts) to your local computer.</span></span>

## <a name="sql-database-wingtip-saas-tutorials"></a><span data-ttu-id="68cd2-111">Kurzy SaaS Wingtip databáze SQL</span><span class="sxs-lookup"><span data-stu-id="68cd2-111">SQL Database Wingtip SaaS tutorials</span></span>

<span data-ttu-id="68cd2-112">Po nasazení aplikace, prozkoumejte následující kurzy, které sestavení po počátečním nasazení.</span><span class="sxs-lookup"><span data-stu-id="68cd2-112">After deploying the app, explore the following tutorials that build upon the initial deployment.</span></span> <span data-ttu-id="68cd2-113">Tyto kurzy prozkoumejte běžných vzorů SaaS, které využít integrované funkce SQL Database, SQL Data Warehouse a jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="68cd2-113">These tutorials explore common SaaS patterns that take advantage of built-in features of SQL Database, SQL Data Warehouse, and other Azure services.</span></span> <span data-ttu-id="68cd2-114">Kurzy zahrnují skripty prostředí PowerShell, s podrobné vysvětlení, které výrazně zjednodušit pochopení a implementace stejné vzorů správu SaaS ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="68cd2-114">Tutorials include PowerShell scripts, with detailed explanations that greatly simplify understanding, and implementing the same SaaS management patterns in your applications.</span></span>


| <span data-ttu-id="68cd2-115">Kurz</span><span class="sxs-lookup"><span data-stu-id="68cd2-115">Tutorial</span></span> | <span data-ttu-id="68cd2-116">Popis</span><span class="sxs-lookup"><span data-stu-id="68cd2-116">Description</span></span> |
|:--|:--|
|[<span data-ttu-id="68cd2-117">Nasazení a prozkoumejte Wingtip SaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="68cd2-117">Deploy and explore the Wingtip SaaS application</span></span>](sql-database-saas-tutorial.md)| <span data-ttu-id="68cd2-118">**ZAČNĚTE ZDE!**</span><span class="sxs-lookup"><span data-stu-id="68cd2-118">**START HERE!**</span></span> <span data-ttu-id="68cd2-119">Nasazení a prozkoumejte aplikace Wingtip SaaS k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="68cd2-119">Deploy and explore the Wingtip SaaS application to your Azure subscription.</span></span> |
|[<span data-ttu-id="68cd2-120">Zřizování a katalog klientů</span><span class="sxs-lookup"><span data-stu-id="68cd2-120">Provision and catalog tenants</span></span>](sql-database-saas-tutorial-provision-and-catalog.md)| <span data-ttu-id="68cd2-121">Zjistěte, jak se aplikace připojí klientům pomocí databáze katalogu a jak katalogu mapuje klienty na svá data.</span><span class="sxs-lookup"><span data-stu-id="68cd2-121">Learn how the application connects to tenants using a catalog database, and how the catalog maps tenants to their data.</span></span> |
|[<span data-ttu-id="68cd2-122">Sledování a správa výkonu</span><span class="sxs-lookup"><span data-stu-id="68cd2-122">Monitor and manage performance</span></span>](sql-database-saas-tutorial-performance-monitoring.md)| <span data-ttu-id="68cd2-123">Zjistěte, jak používat monitorování funkcí služby SQL Database a jak nastavit upozornění při překročení prahové hodnoty výkonu.</span><span class="sxs-lookup"><span data-stu-id="68cd2-123">Learn how to use monitoring features of SQL Database, and how to set alerts when performance thresholds are exceeded.</span></span> |
|[<span data-ttu-id="68cd2-124">Monitorování s analýzy protokolů (OMS)</span><span class="sxs-lookup"><span data-stu-id="68cd2-124">Monitor with Log Analytics (OMS)</span></span>](sql-database-saas-tutorial-log-analytics.md) | <span data-ttu-id="68cd2-125">Další informace o použití [analýzy protokolů](../log-analytics/log-analytics-overview.md) monitorování velké množství prostředků, napříč více fondů.</span><span class="sxs-lookup"><span data-stu-id="68cd2-125">Learn about using [Log Analytics](../log-analytics/log-analytics-overview.md) to monitor large amounts of resources, across multiple pools.</span></span> |
|[<span data-ttu-id="68cd2-126">Obnovení jednoho klienta</span><span class="sxs-lookup"><span data-stu-id="68cd2-126">Restore a single tenant</span></span>](sql-database-saas-tutorial-restore-single-tenant.md)| <span data-ttu-id="68cd2-127">Zjistěte, jak k obnovení databázi klienta do předchozího bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="68cd2-127">Learn how to restore a tenant database to a prior point in time.</span></span> <span data-ttu-id="68cd2-128">Kroky k obnovení databázi paralelní ponechat stávající databázi klienta online, jsou také zahrnuté.</span><span class="sxs-lookup"><span data-stu-id="68cd2-128">Steps to restore to a parallel database, leaving the existing tenant database online, are also included.</span></span> |
|[<span data-ttu-id="68cd2-129">Správa schématu klienta</span><span class="sxs-lookup"><span data-stu-id="68cd2-129">Manage tenant schema</span></span>](sql-database-saas-tutorial-schema-management.md)| <span data-ttu-id="68cd2-130">Zjistěte, jak aktualizovat schéma a aktualizovat referenční data napříč všichni klienti Wingtip SaaS.</span><span class="sxs-lookup"><span data-stu-id="68cd2-130">Learn how to update schema, and update reference data, across all Wingtip SaaS tenants.</span></span> |
|[<span data-ttu-id="68cd2-131">Spustit analytics ad-hoc</span><span class="sxs-lookup"><span data-stu-id="68cd2-131">Run ad-hoc analytics</span></span>](sql-database-saas-tutorial-adhoc-analytics.md) | <span data-ttu-id="68cd2-132">Vytvoření databáze analýzy ad-hoc a spusťte v reálném čase distribuované dotazy mezi všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="68cd2-132">Create an ad-hoc analytics database and run real-time distributed queries across all tenants.</span></span>  |
|[<span data-ttu-id="68cd2-133">Spusťte klienta analytics</span><span class="sxs-lookup"><span data-stu-id="68cd2-133">Run tenant analytics</span></span>](sql-database-saas-tutorial-tenant-analytics.md) | <span data-ttu-id="68cd2-134">Extrahování dat klienta do služby analytics databáze nebo data warehouse ke spuštění v režimu offline analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="68cd2-134">Extract tenant data into an analytics database or data warehouse for running offline analytic queries.</span></span> |



## <a name="application-architecture"></a><span data-ttu-id="68cd2-135">Architektura aplikace</span><span class="sxs-lookup"><span data-stu-id="68cd2-135">Application architecture</span></span>

<span data-ttu-id="68cd2-136">Aplikace Wingtip SaaS používá model databáze za klienta a elastické fondy SQL Pokud chcete maximalizovat efektivitu.</span><span class="sxs-lookup"><span data-stu-id="68cd2-136">The Wingtip SaaS app uses the database-per-tenant model, and uses SQL elastic pools to maximize efficiency.</span></span> <span data-ttu-id="68cd2-137">Pro zřizování a mapování klientům svá data se používá databázi katalogu.</span><span class="sxs-lookup"><span data-stu-id="68cd2-137">For provisioning and mapping tenants to their data, a catalog database is used.</span></span> <span data-ttu-id="68cd2-138">Základní aplikace Wingtip SaaS používá fond s tři klienty ukázka plus databáze katalogu.</span><span class="sxs-lookup"><span data-stu-id="68cd2-138">The core Wingtip SaaS application, uses a pool with three sample tenants, plus the catalog database.</span></span> <span data-ttu-id="68cd2-139">Dokončení řadu SaaS Wingtip kurzy za následek doplňky vyhledá nasazení se zavedením analytické databáze mezidatabázové Správa schématu, atd.</span><span class="sxs-lookup"><span data-stu-id="68cd2-139">Completing many of the Wingtip SaaS tutorials result in add-ons to the intial deployment, by introducing analytic databases, cross-database schema management, etc.</span></span>


![Architektura Wingtip SaaS](media/sql-database-wtp-overview/app-architecture.png)


<span data-ttu-id="68cd2-141">Při průchodu přes kurzů k a práci s aplikací, je potřeba zaměřit se na vzory SaaS, které se vztahují na datové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="68cd2-141">While going through the tutorials and working with the app, it is important to focus on the SaaS patterns as they relate to the data tier.</span></span> <span data-ttu-id="68cd2-142">Jinými slovy je třeba se zaměřit na datovou vrstvu a ne na analýzu aplikace jako takové.</span><span class="sxs-lookup"><span data-stu-id="68cd2-142">In other words, focus on the data tier, and don't over analyze the app itself.</span></span> <span data-ttu-id="68cd2-143">Principy provádění těchto SaaS vzory je klíč k implementaci tyto vzory ve svých aplikacích při zvažování veškeré nezbytné úpravy pro vaše konkrétní obchodní požadavky.</span><span class="sxs-lookup"><span data-stu-id="68cd2-143">Understanding the implementation of these SaaS patterns is key to implementing these patterns in your applications, while considering any necessary modifications for your specific business requirements.</span></span>

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a><span data-ttu-id="68cd2-144">Stáhněte si a odblokování skripty Wingtip SaaS</span><span class="sxs-lookup"><span data-stu-id="68cd2-144">Download and unblock the Wingtip SaaS scripts</span></span>

<span data-ttu-id="68cd2-145">Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené.</span><span class="sxs-lookup"><span data-stu-id="68cd2-145">Executable contents (scripts, dlls) may be blocked by Windows when zip files are downloaded from an external source and extracted.</span></span> <span data-ttu-id="68cd2-146">Při extrahování skripty ze souboru zip, ***použijte následující postup, chcete-li odblokovat soubor .zip před extrahování***.</span><span class="sxs-lookup"><span data-stu-id="68cd2-146">When extracting the scripts from a zip file, ***follow the steps below to unblock the .zip file before extracting***.</span></span> <span data-ttu-id="68cd2-147">Tím se zajistí, že je povoleno spustit skripty.</span><span class="sxs-lookup"><span data-stu-id="68cd2-147">This ensures the scripts are allowed to run.</span></span>

1. <span data-ttu-id="68cd2-148">Přejděte do [úložiště github Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).</span><span class="sxs-lookup"><span data-stu-id="68cd2-148">Browse to [the Wingtip SaaS github repo](https://github.com/Microsoft/WingtipSaaS).</span></span>
1. <span data-ttu-id="68cd2-149">Klikněte na tlačítko **klonovat nebo stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-149">Click **Clone or download**.</span></span>
1. <span data-ttu-id="68cd2-150">Klikněte na tlačítko **stáhnout ZIP** a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="68cd2-150">Click **Download ZIP** and save the file.</span></span>
1. <span data-ttu-id="68cd2-151">Klikněte pravým tlačítkem myši **WingtipSaaS-master.zip** soubor a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-151">Right-click the **WingtipSaaS-master.zip** file, and select **Properties**.</span></span>
1. <span data-ttu-id="68cd2-152">Na **Obecné** vyberte **Odblokovat**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-152">On the **General** tab, select **Unblock**.</span></span>
1. <span data-ttu-id="68cd2-153">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-153">Click **OK**.</span></span>
1. <span data-ttu-id="68cd2-154">Extrahujte soubory.</span><span class="sxs-lookup"><span data-stu-id="68cd2-154">Extract the files.</span></span>

<span data-ttu-id="68cd2-155">Skripty, které jsou umístěné v *... \\WingtipSaaS hlavní\\Learning moduly* složky.</span><span class="sxs-lookup"><span data-stu-id="68cd2-155">Scripts are located in the *..\\WingtipSaaS-master\\Learning Modules* folder.</span></span>


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a><span data-ttu-id="68cd2-156">Práce s skriptů prostředí PowerShell Wingtip SaaS</span><span class="sxs-lookup"><span data-stu-id="68cd2-156">Working with the Wingtip SaaS PowerShell Scripts</span></span>

<span data-ttu-id="68cd2-157">Chcete-li maximální využití ukázku ponořte do zadané skripty.</span><span class="sxs-lookup"><span data-stu-id="68cd2-157">To get the most out of the sample you need to dive into the provided scripts.</span></span> <span data-ttu-id="68cd2-158">Použití zarážek a krok prostřednictvím skriptů, prozkoumání podrobnosti o tom, jak jsou implementované různé vzorce SaaS.</span><span class="sxs-lookup"><span data-stu-id="68cd2-158">Use breakpoints and step through the scripts, examining the details of how the different SaaS patterns are implemented.</span></span> <span data-ttu-id="68cd2-159">Snadno procházet postupně zadaný skriptech a modulech pro nejlepší pochopení, doporučujeme používat [prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span><span class="sxs-lookup"><span data-stu-id="68cd2-159">To easily step through the provided scripts and modules for the best understanding, we recommend using the [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span></span>

### <a name="update-the-configuration-file-for-your-deployment"></a><span data-ttu-id="68cd2-160">Aktualizovat konfigurační soubor pro vaše nasazení</span><span class="sxs-lookup"><span data-stu-id="68cd2-160">Update the configuration file for your deployment</span></span>

<span data-ttu-id="68cd2-161">Upravit **UserConfig.psm1** soubor s hodnotou prostředků skupiny a uživatele, který nastavíte během nasazení:</span><span class="sxs-lookup"><span data-stu-id="68cd2-161">Edit the **UserConfig.psm1** file with the resource group and user value that you set during deployment:</span></span>

1. <span data-ttu-id="68cd2-162">Otevřete *prostředí PowerShell ISE* a zatížení... \\Učení moduly\\*UserConfig.psm1*</span><span class="sxs-lookup"><span data-stu-id="68cd2-162">Open the *PowerShell ISE* and load ...\\Learning Modules\\*UserConfig.psm1*</span></span> 
1. <span data-ttu-id="68cd2-163">Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami pro vaše nasazení (na řádky 10 a 11 pouze).</span><span class="sxs-lookup"><span data-stu-id="68cd2-163">Update *ResourceGroupName* and *Name* with the specific values for your deployment (on lines 10 and 11 only).</span></span>
1. <span data-ttu-id="68cd2-164">Uložte změny!</span><span class="sxs-lookup"><span data-stu-id="68cd2-164">Save the changes!</span></span>

<span data-ttu-id="68cd2-165">Tyto hodnoty nastavení tady jednoduše nebude nutné aktualizovat tyto hodnoty specifické pro nasazení v každé skriptu.</span><span class="sxs-lookup"><span data-stu-id="68cd2-165">Setting these values here simply keeps you from having to update these deployment-specific values in every script.</span></span>

### <a name="execute-scripts-by-pressing-f5"></a><span data-ttu-id="68cd2-166">Provádění skriptů stisknutím klávesy F5</span><span class="sxs-lookup"><span data-stu-id="68cd2-166">Execute Scripts by pressing F5</span></span>

<span data-ttu-id="68cd2-167">Používat několik skriptů *$PSScriptRoot* k procházení složek, a *$PSScriptRoot* je Vyhodnocená jenom když jsou spouštět skripty, stisknutím klávesy **F5**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-167">Several scripts use *$PSScriptRoot* to navigate folders, and *$PSScriptRoot* is only evaluated when scripts are executed by pressing **F5**.</span></span>  <span data-ttu-id="68cd2-168">Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="68cd2-168">Highlighting and running a selection (**F8**) can result in errors, so press **F5** when running scripts.</span></span>

### <a name="step-through-the-scripts-to-examine-the-implementation"></a><span data-ttu-id="68cd2-169">Procházení skriptů k vyzkoušení implementace</span><span class="sxs-lookup"><span data-stu-id="68cd2-169">Step through the scripts to examine the implementation</span></span>

<span data-ttu-id="68cd2-170">Nejlepší způsob, jak porozumět skripty je procházení je chcete zobrazit, co dělají.</span><span class="sxs-lookup"><span data-stu-id="68cd2-170">The best way to understand the scripts is by stepping through them to see what they do.</span></span> <span data-ttu-id="68cd2-171">Podívejte se na zahrnutou **Demo -** skripty, které jsou k dispozici snadno postupujte podle pracovní postup vysoké úrovně.</span><span class="sxs-lookup"><span data-stu-id="68cd2-171">Check out the included **Demo-** scripts that present an easy to follow high-level workflow.</span></span> <span data-ttu-id="68cd2-172">**Demo -** skripty zobrazit kroky potřebné k provedení každý úkol, takže nastavit zarážky a přejít k podrobnostem hlubší do jednotlivých volání zobrazíte podrobnosti implementace pro různé vzorce SaaS.</span><span class="sxs-lookup"><span data-stu-id="68cd2-172">The **Demo-** scripts show the steps required to accomplish each task, so set breakpoints and drill deeper into the individual calls to see implementation details for the different SaaS patterns.</span></span>

<span data-ttu-id="68cd2-173">Tipy pro zkoumání a procházení skriptů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="68cd2-173">Tips for exploring and stepping through PowerShell scripts:</span></span>

* <span data-ttu-id="68cd2-174">Otevřete **Demo -** skripty v integrovaném Skriptovacím prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="68cd2-174">Open **Demo-** scripts in the PowerShell ISE.</span></span>
* <span data-ttu-id="68cd2-175">Spustit nebo pokračovat s **F5** (pomocí **F8** se nedoporučuje, protože *$PSScriptRoot* při spuštění výběr skriptu, nebude hodnocen).</span><span class="sxs-lookup"><span data-stu-id="68cd2-175">Execute or continue with **F5** (using **F8** is not advised because *$PSScriptRoot* is not evaluated when running selections of a script).</span></span>
* <span data-ttu-id="68cd2-176">Pokud chcete umístit zarážky, klikněte na řádek nebo ho vyberte a stiskněte klávesu **F9**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-176">Place breakpoints by clicking or selecting a line and pressing **F9**.</span></span>
* <span data-ttu-id="68cd2-177">Funkci nebo volání skriptu můžete vynechat stisknutím klávesy **F10**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-177">Step over a function or script call using **F10**.</span></span>
* <span data-ttu-id="68cd2-178">Na funkci nebo volání skriptu můžete přejít stisknutím klávesy **F11**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-178">Step into a function or script call using **F11**.</span></span>
* <span data-ttu-id="68cd2-179">Aktuální funkci nebo vlání skriptu můžete ukončit stisknutím kombinace **Shift + F11**.</span><span class="sxs-lookup"><span data-stu-id="68cd2-179">Step out of the current function or script call using **Shift + F11**.</span></span>


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a><span data-ttu-id="68cd2-180">Prozkoumání databázového schématu a spouštění dotazů SQL pomocí SSMS</span><span class="sxs-lookup"><span data-stu-id="68cd2-180">Explore database schema and execute SQL queries using SSMS</span></span>

<span data-ttu-id="68cd2-181">Použití [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) k připojení a procházet aplikačních serverů a databází.</span><span class="sxs-lookup"><span data-stu-id="68cd2-181">Use [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) to connect and browse the application servers and databases.</span></span>

<span data-ttu-id="68cd2-182">Nasazení původně má dva servery SQL Database pro připojení k – *tenants1 -&lt;uživatele&gt;*  serveru a *katalogu -&lt;uživatele&gt;*  Server.</span><span class="sxs-lookup"><span data-stu-id="68cd2-182">The deployment initially has two SQL Database servers to connect to - the *tenants1-&lt;User&gt;* server, and the *catalog-&lt;User&gt;* server.</span></span> <span data-ttu-id="68cd2-183">Aby se zajistilo úspěšné ukázku připojení, mají oba servery [pravidlo brány firewall](sql-database-firewall-configure.md) povolení všechny IP adresy prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="68cd2-183">To ensure a successful demo connection, both servers have a [firewall rule](sql-database-firewall-configure.md) allowing all IPs through.</span></span>


1. <span data-ttu-id="68cd2-184">Otevřete *SSMS* a připojte se k serveru *tenants1-&lt;User&gt;.database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="68cd2-184">Open *SSMS* and connect to the *tenants1-&lt;User&gt;.database.windows.net* server.</span></span>
1. <span data-ttu-id="68cd2-185">Klikněte na **Připojit** > **Databázový stroj...**:</span><span class="sxs-lookup"><span data-stu-id="68cd2-185">Click **Connect** > **Database Engine...**:</span></span>

   ![katalogový server](media/sql-database-wtp-overview/connect.png)

1. <span data-ttu-id="68cd2-187">Přihlašovací údaje pro ukázku jsou: Přihlašovací jméno = *developer*, Heslo = *P@ssword1*</span><span class="sxs-lookup"><span data-stu-id="68cd2-187">Demo credentials are: Login = *developer*, Password = *P@ssword1*</span></span>

   ![připojení](media\sql-database-wtp-overview\tenants1-connect.png)

1. <span data-ttu-id="68cd2-189">Opakujte kroky 2–3 a připojte se k serveru *catalog-&lt;User&gt;.database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="68cd2-189">Repeat steps 2-3 and connect to the *catalog-&lt;User&gt;.database.windows.net* server.</span></span>

<span data-ttu-id="68cd2-190">Po úspěšném připojení byste měli vidět oba servery.</span><span class="sxs-lookup"><span data-stu-id="68cd2-190">After successfully connecting you should see both servers.</span></span> <span data-ttu-id="68cd2-191">Seznam databází se může lišit v závislosti na klienty, kterou jste zřídili:</span><span class="sxs-lookup"><span data-stu-id="68cd2-191">Your list of databases might be different, depending on the tenants you've provisioned:</span></span>

![průzkumník objektů](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a><span data-ttu-id="68cd2-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68cd2-193">Next steps</span></span>

[<span data-ttu-id="68cd2-194">Nasazení aplikace Wingtip SaaS</span><span class="sxs-lookup"><span data-stu-id="68cd2-194">Deploy the Wingtip SaaS application</span></span>](sql-database-saas-tutorial.md)