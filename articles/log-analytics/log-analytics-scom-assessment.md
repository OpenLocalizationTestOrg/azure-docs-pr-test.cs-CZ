---
title: "Optimalizace prostředí System Center Operations Manager s nástrojem Azure Log Analytics | Microsoft Docs"
description: "Řešení System Center Operations Manager posouzení můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4992d98397da409f7c1cfbdeb40fdb0cdd0d2f19
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-assessment-preview-solution"></a><span data-ttu-id="08285-103">Optimalizace prostředí s nástrojem System Center Operations Manager Assessment (Preview) řešení</span><span class="sxs-lookup"><span data-stu-id="08285-103">Optimize your environment with the System Center Operations Manager Assessment (Preview) solution</span></span>

![System Center Operations Manager Assessment symbol](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

<span data-ttu-id="08285-105">Řešení System Center Operations Manager posouzení můžete použít k vyhodnocení rizik a stavu prostředí serveru System Center Operations Manager v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="08285-105">You can use the System Center Operations Manager Assessment solution to assess the risk and health of your System Center Operations Manager server environments on a regular interval.</span></span> <span data-ttu-id="08285-106">Tento článek vám umožňuje instalovat, konfigurovat a používat řešení, takže můžete provést nápravné akce pro potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="08285-106">This article helps you install, configure, and use the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="08285-107">Toto řešení poskytuje seznam doporučení, které jsou specifické pro infrastrukturu nasazené serverů seřazený podle priority.</span><span class="sxs-lookup"><span data-stu-id="08285-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="08285-108">Doporučení jsou rozdělené mezi čtyři konkrétní oblasti, které vám pomůžou rychle pochopit riziko a proveďte opravné akce.</span><span class="sxs-lookup"><span data-stu-id="08285-108">The recommendations are categorized across four focus areas, which help you quickly understand the risk and take corrective action.</span></span>

<span data-ttu-id="08285-109">Doporučení, která jsou založené na znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách.</span><span class="sxs-lookup"><span data-stu-id="08285-109">The recommendations made are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="08285-110">Každé doporučení obsahuje informace, proč může pro vás důležitá problém a implementaci navrhované změny.</span><span class="sxs-lookup"><span data-stu-id="08285-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="08285-111">Můžete vybrat konkrétní oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.</span><span class="sxs-lookup"><span data-stu-id="08285-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="08285-112">Když jste přidali řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na **System Center Operations Manager Assessment** řídicí panel pro vaši infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="08285-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **System Center Operations Manager Assessment** dashboard for your infrastructure.</span></span> <span data-ttu-id="08285-113">Následující části popisují, jak používat informace na **System Center Operations Manager Assessment** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu SCOM.</span><span class="sxs-lookup"><span data-stu-id="08285-113">The following sections describe how to use the information on the **System Center Operations Manager Assessment** dashboard, where you can view and then take recommended actions for your SCOM infrastructure.</span></span>

![Dlaždice řešení System Center Operations Manager](./media/log-analytics-scom-assessment/scom-tile.png)

![System Center Operations Manager Assessment řídicí panel](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="08285-116">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="08285-116">Installing and configuring the solution</span></span>

<span data-ttu-id="08285-117">Toto řešení funguje s Microsoft systému Operations Manager 2012 R2 a 2012 s aktualizací SP1.</span><span class="sxs-lookup"><span data-stu-id="08285-117">The solution works with Microsoft System Operations Manager 2012 R2 and 2012 SP1.</span></span>

<span data-ttu-id="08285-118">Použijte následující informace k instalaci a konfiguraci řešení.</span><span class="sxs-lookup"><span data-stu-id="08285-118">Use the following information to install and configure the solution.</span></span>

 - <span data-ttu-id="08285-119">Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení.</span><span class="sxs-lookup"><span data-stu-id="08285-119">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="08285-120">Instalaci řešení z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) nebo podle pokynů v [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="08285-120">Install the solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) or by following the instructions in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>

 - <span data-ttu-id="08285-121">Po přidání řešení do pracovního prostoru, System Center Operations Manager Assessment dlaždice na řídicím panelu zobrazí zpráva vyžaduje další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="08285-121">After adding the solution to the workspace, the System Center Operations Manager Assessment tile on the dashboard displays the additional configuration required message.</span></span> <span data-ttu-id="08285-122">Klikněte na dlaždici a postupujte podle kroků konfigurace uvedených na stránce</span><span class="sxs-lookup"><span data-stu-id="08285-122">Click on the tile and follow the configuration steps mentioned in the page</span></span>

 ![Dlaždice řídicího panelu System Center Operations Manager](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 <span data-ttu-id="08285-124">Konfigurace nástroje System Center Operations Manager lze provést prostřednictvím skript podle kroků uvedených na stránce konfigurace řešení v OMS.</span><span class="sxs-lookup"><span data-stu-id="08285-124">Configuration of the System Center Operations Manager can be done through the script by following the steps mentioned in the configuration page of the solution in OMS.</span></span>

 <span data-ttu-id="08285-125">Místo toho nakonfigurovat assessment prostřednictvím konzoly SCOM, použijte následujících kroků ve stejném pořadí</span><span class="sxs-lookup"><span data-stu-id="08285-125">Instead, to configure the assessment through SCOM Console, follow the below steps in the same order</span></span>
1. [<span data-ttu-id="08285-126">Nastavit účet Spustit jako pro System Center Operations Manager hodnocení</span><span class="sxs-lookup"><span data-stu-id="08285-126">Set the Run As account for System Center Operations Manager Assessment</span></span>](#operations-manager-run-as-accounts-for-oms)  
2. [<span data-ttu-id="08285-127">Konfigurace pravidla System Center Operations Manager hodnocení</span><span class="sxs-lookup"><span data-stu-id="08285-127">Configure the System Center Operations Manager Assessment rule</span></span>](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a><span data-ttu-id="08285-128">Informace o System Center Operations Manager assessment datových kolekce</span><span class="sxs-lookup"><span data-stu-id="08285-128">System Center Operations Manager assessment data collection details</span></span>

<span data-ttu-id="08285-129">Hodnocení nástroje System Center Operations Manager shromažďuje data rozhraní WMI, registru dat, data protokolu událostí, data nástroje Operations Manager prostřednictvím prostředí Windows PowerShell a dotazy SQL, souborů informace kolekci pomocí serveru, který jste povolili.</span><span class="sxs-lookup"><span data-stu-id="08285-129">The System Center Operations Manager assessment collects WMI data, Registry data, EventLog data, Operations Manager data through Windows PowerShell, SQL Queries, File information collector using the server that you have enabled.</span></span>

<span data-ttu-id="08285-130">Následující tabulka uvádí metody shromažďování dat pro System Center Operations Manager hodnocení a jak často se data shromážděná pomocí agenta.</span><span class="sxs-lookup"><span data-stu-id="08285-130">The following table shows data collection methods for System Center Operations Manager Assessment, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="08285-131">Platforma</span><span class="sxs-lookup"><span data-stu-id="08285-131">platform</span></span> | <span data-ttu-id="08285-132">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="08285-132">Direct Agent</span></span> | <span data-ttu-id="08285-133">Agenta nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="08285-133">SCOM agent</span></span> | <span data-ttu-id="08285-134">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="08285-134">Azure Storage</span></span> | <span data-ttu-id="08285-135">SCOM vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="08285-135">SCOM required?</span></span> | <span data-ttu-id="08285-136">Data agenta SCOM odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="08285-136">SCOM agent data sent via management group</span></span> | <span data-ttu-id="08285-137">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="08285-137">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="08285-138">Windows</span><span class="sxs-lookup"><span data-stu-id="08285-138">Windows</span></span> | | | | <span data-ttu-id="08285-139">&#8226;</span><span class="sxs-lookup"><span data-stu-id="08285-139">&#8226;</span></span> | | <span data-ttu-id="08285-140">sedm dní</span><span class="sxs-lookup"><span data-stu-id="08285-140">seven days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="08285-141">Účty spustit jako nástroje Operations Manager pro OMS</span><span class="sxs-lookup"><span data-stu-id="08285-141">Operations Manager run-as accounts for OMS</span></span>

<span data-ttu-id="08285-142">OMS staví na sady management Pack pro úlohy zajistit přidanou hodnotou services.</span><span class="sxs-lookup"><span data-stu-id="08285-142">OMS builds on management packs for workloads to provide value-add services.</span></span> <span data-ttu-id="08285-143">Každé zatížení vyžaduje určeného pro konkrétní úlohy oprávnění ke spouštění sad management Pack v odlišném kontextu zabezpečení, jako je například účet domény.</span><span class="sxs-lookup"><span data-stu-id="08285-143">Each workload requires workload-specific privileges to run management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="08285-144">Konfigurace Operations Manager účet Spustit jako a zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="08285-144">Configure an Operations Manager Run As account to provide credential information.</span></span>

<span data-ttu-id="08285-145">K nastavení účet Spustit jako nástroje Operations Manager pro System Center Operations Manager Assessment použijte následující informace.</span><span class="sxs-lookup"><span data-stu-id="08285-145">Use the following information to set the Operations Manager run-as account for System Center Operations Manager Assessment.</span></span>

### <a name="set-the-run-as-account"></a><span data-ttu-id="08285-146">Nastavit účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="08285-146">Set the Run As account</span></span>

1. <span data-ttu-id="08285-147">V konzole nástroje Operations Manager, přejděte do **správy** kartě.</span><span class="sxs-lookup"><span data-stu-id="08285-147">In the Operations Manager Console, go to the **Administration** tab.</span></span>
2. <span data-ttu-id="08285-148">V části **konfigurace spustit jako**, klikněte na tlačítko **účty**.</span><span class="sxs-lookup"><span data-stu-id="08285-148">Under the **Run As Configuration**, click **Accounts**.</span></span>
3. <span data-ttu-id="08285-149">Vytvořte účet Spustit jako, následující pomocí Průvodce vytvoření účtu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="08285-149">Create the Run As Account, following through the Wizard, creating a Windows account.</span></span> <span data-ttu-id="08285-150">Účet, který chcete použít, je ten, identifikovat a má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="08285-150">The account to use is the one identified and having all the prerequisites below:</span></span>

    >[!NOTE]
    <span data-ttu-id="08285-151">Účet Spustit jako, musí splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="08285-151">The Run As account must meet following requirements:</span></span>
    - <span data-ttu-id="08285-152">Domény účtu, členem místní skupiny Administrators na všech serverech v prostředí (všechny Operations Manager role - Server pro správu, databáze OpsMgr, datového skladu, vytváření sestav, webové konzole, brány)</span><span class="sxs-lookup"><span data-stu-id="08285-152">A domain account member of the local Administrators group on all servers in the environment (All Operations Manager Roles - Management Server, OpsMgr Database, Data Warehouse, Reporting, Web Console, Gateway)</span></span>
    - <span data-ttu-id="08285-153">Role správce Manager operace pro skupinu pro správu probíhá vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="08285-153">Operation Manager Administrator Role for the management group being assessed</span></span>
    - <span data-ttu-id="08285-154">Spuštění [skriptu](#sql-script-to-grant-granular-permissions-to-the-run-as-account) udělit oprávnění na podrobné úrovni k účtu v instanci SQL používaných nástrojem Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="08285-154">Execute the [script](#sql-script-to-grant-granular-permissions-to-the-run-as-account) to grant granular permissions to the account on SQL instance used by Operations Manager.</span></span>
      <span data-ttu-id="08285-155">Poznámka: Pokud tento účet již má oprávnění správce systému, potom přeskočte provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="08285-155">Note: If this account has sysadmin rights already, then skip the script execution.</span></span>

4. <span data-ttu-id="08285-156">V části **zabezpečení distribuce**, vyberte **bezpečnější**.</span><span class="sxs-lookup"><span data-stu-id="08285-156">Under **Distribution Security**, select **More secure**.</span></span>
5. <span data-ttu-id="08285-157">Určení serveru správy, kde je účet distribuován.</span><span class="sxs-lookup"><span data-stu-id="08285-157">Specify the management server where the account is distributed.</span></span>
3. <span data-ttu-id="08285-158">Vraťte se zpátky a konfigurace spustit jako a klikněte na tlačítko **profily**.</span><span class="sxs-lookup"><span data-stu-id="08285-158">Go back to the Run As Configuration and click **Profiles**.</span></span>
4. <span data-ttu-id="08285-159">Vyhledejte *SCOM Assessment profil*.</span><span class="sxs-lookup"><span data-stu-id="08285-159">Search for the *SCOM Assessment Profile*.</span></span>
5. <span data-ttu-id="08285-160">Název profilu musí být: *Microsoft System Center Advisor SCOM Assessment profilu spustit jako*.</span><span class="sxs-lookup"><span data-stu-id="08285-160">The profile name should be: *Microsoft System Center Advisor SCOM Assessment Run As Profile*.</span></span>
6. <span data-ttu-id="08285-161">Klikněte pravým tlačítkem myši a aktualizujte jeho vlastnosti a přidejte naposledy vytvořený účet Spustit jako jste vytvořili v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="08285-161">Right-click and update its properties and add the recently created Run As Account you created in step 3.</span></span>

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a><span data-ttu-id="08285-162">Skript SQL k udělení oprávnění na podrobné úrovni k účtu spustit jako</span><span class="sxs-lookup"><span data-stu-id="08285-162">SQL script to grant granular permissions to the Run As account</span></span>

<span data-ttu-id="08285-163">Spusťte následující skript SQL udělit potřebná oprávnění k účtu spustit jako v instanci SQL používaných nástrojem Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="08285-163">Execute the following SQL script to grant required permissions to the Run As account on the SQL instance used by Operations Manager.</span></span>

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-the-assessment-rule"></a><span data-ttu-id="08285-164">Konfigurace pravidla hodnocení</span><span class="sxs-lookup"><span data-stu-id="08285-164">Configure the assessment rule</span></span>

<span data-ttu-id="08285-165">Sada management pack řešení System Center Operations Manager Assessment zahrnuje na pravidlo s názvem *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo*.</span><span class="sxs-lookup"><span data-stu-id="08285-165">The System Center Operations Manager Assessment solution’s management pack includes a rule named *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule*.</span></span> <span data-ttu-id="08285-166">Toto pravidlo je zodpovědná za spuštění hodnocení.</span><span class="sxs-lookup"><span data-stu-id="08285-166">This rule is responsible for running the assessment.</span></span> <span data-ttu-id="08285-167">Následující postupy použijte, pokud chcete povolit pravidlo a nastavte četnost.</span><span class="sxs-lookup"><span data-stu-id="08285-167">To enable the rule and configure the frequency, use the procedures below.</span></span>

<span data-ttu-id="08285-168">Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo je ve výchozím nastavení zakázaná.</span><span class="sxs-lookup"><span data-stu-id="08285-168">By default, the Microsoft System Center Advisor SCOM Assessment Run Assessment Rule is disabled.</span></span> <span data-ttu-id="08285-169">Chcete-li spustit vyhodnocení, je nutné povolit pravidlo na serveru pro správu.</span><span class="sxs-lookup"><span data-stu-id="08285-169">To run the assessment, you must enable the rule on a management server.</span></span> <span data-ttu-id="08285-170">Pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="08285-170">Use the following steps.</span></span>

#### <a name="enable-the-rule-for-a-specific-management-server"></a><span data-ttu-id="08285-171">Povolení pravidla pro konkrétní management server</span><span class="sxs-lookup"><span data-stu-id="08285-171">Enable the rule for a specific management server</span></span>

1. <span data-ttu-id="08285-172">V **vytváření** prostoru konzoly nástroje Operations Manager, vyhledejte pravidlo *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo* v **pravidla** podokno.</span><span class="sxs-lookup"><span data-stu-id="08285-172">In the **Authoring** workspace of the Operations Manager console, search for the rule *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* in the **Rules** pane.</span></span>
2. <span data-ttu-id="08285-173">Ve výsledcích hledání vyberte ten, který obsahuje text *typu: Server pro správu*.</span><span class="sxs-lookup"><span data-stu-id="08285-173">In the search results, select the one that includes the text *Type: Management Server*.</span></span>
3. <span data-ttu-id="08285-174">Klikněte pravým tlačítkem na pravidlo a pak klikněte na **přepsání** > **pro konkrétní objekt třídy: Server pro správu**.</span><span class="sxs-lookup"><span data-stu-id="08285-174">Right-click the rule and then click **Overrides** > **For a specific object of class: Management Server**.</span></span>
4.  <span data-ttu-id="08285-175">V seznamu serverů pro správu k dispozici vyberte server pro správu kde se má pravidlo spustit.</span><span class="sxs-lookup"><span data-stu-id="08285-175">In the available management servers list, select the management server where the rule should run.</span></span>
5.  <span data-ttu-id="08285-176">Ujistěte se, že změníte hodnotu přepsání **True** pro **povoleno** hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="08285-176">Ensure that you change override value to **True** for the **Enabled** parameter value.</span></span>  
    <span data-ttu-id="08285-177">![Přepište parametr](./media/log-analytics-scom-assessment/rule.png)</span><span class="sxs-lookup"><span data-stu-id="08285-177">![override parameter](./media/log-analytics-scom-assessment/rule.png)</span></span>

<span data-ttu-id="08285-178">Při stále v tomto okně nakonfigurujte četnost spustit pomocí dalšího postupu.</span><span class="sxs-lookup"><span data-stu-id="08285-178">While still in this window, configure the frequency of the run using the next procedure.</span></span>

#### <a name="configure-the-run-frequency"></a><span data-ttu-id="08285-179">Nastavte četnost, spuštění</span><span class="sxs-lookup"><span data-stu-id="08285-179">Configure the run frequency</span></span>

<span data-ttu-id="08285-180">Hodnocení je nakonfigurovaná pro spuštění každých 10 080 minut (nebo sedm dní), je výchozí interval.</span><span class="sxs-lookup"><span data-stu-id="08285-180">The assessment is configured to run every 10,080 minutes (or seven days), the default interval.</span></span> <span data-ttu-id="08285-181">Hodnota, která má minimální hodnotu 1 440 minut (nebo jeden den), můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="08285-181">You can override the value to a minimum value of 1440 minutes (or one day).</span></span> <span data-ttu-id="08285-182">Hodnota představuje minimální časové prodlevy požadované mezi následných vyhodnocení běží.</span><span class="sxs-lookup"><span data-stu-id="08285-182">The value represents the minimum time gap required between successive assessment runs.</span></span> <span data-ttu-id="08285-183">K přepsání intervalu, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="08285-183">To override the interval, use the steps below.</span></span>

1. <span data-ttu-id="08285-184">V **vytváření** prostoru konzoly nástroje Operations Manager, vyhledejte pravidlo *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo* v **pravidla** podokno.</span><span class="sxs-lookup"><span data-stu-id="08285-184">In the **Authoring** workspace of the Operations Manager console, search for the rule *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* in the **Rules** pane.</span></span>
2. <span data-ttu-id="08285-185">Ve výsledcích hledání vyberte ten, který obsahuje text *typu: Server pro správu*.</span><span class="sxs-lookup"><span data-stu-id="08285-185">In the search results, select the one that includes the text *Type: Management Server*.</span></span>
3. <span data-ttu-id="08285-186">Klikněte pravým tlačítkem na pravidlo a pak klikněte na **přepsat pravidlo** > **pro všechny objekty třídy: Server pro správu**.</span><span class="sxs-lookup"><span data-stu-id="08285-186">Right-click the rule and then click **Override the Rule** > **For all objects of class: Management Server**.</span></span>
4. <span data-ttu-id="08285-187">Změna **Interval** hodnota parametru hodnotu požadovaného intervalu pro.</span><span class="sxs-lookup"><span data-stu-id="08285-187">Change the **Interval** parameter value to your desired interval value.</span></span> <span data-ttu-id="08285-188">V následujícím příkladu je hodnota nastavena na 1 440 minut (jeden den).</span><span class="sxs-lookup"><span data-stu-id="08285-188">In the example below, the value is set to 1440 minutes (one day).</span></span>  
    <span data-ttu-id="08285-189">![Parametr interval](./media/log-analytics-scom-assessment/interval.png)</span><span class="sxs-lookup"><span data-stu-id="08285-189">![interval parameter](./media/log-analytics-scom-assessment/interval.png)</span></span>  
    <span data-ttu-id="08285-190">Pokud je hodnota nastavena na méně než 1 440 minut, se pravidlo spustí v intervalu jeden den.</span><span class="sxs-lookup"><span data-stu-id="08285-190">If the value is set to less than 1440 minutes, then the rule runs at a one day interval.</span></span> <span data-ttu-id="08285-191">V tomto příkladu pravidlo ignoruje hodnota intervalu a spustí frekvencí jeden den.</span><span class="sxs-lookup"><span data-stu-id="08285-191">In this example, the rule ignores the interval value and runs at a frequency of one day.</span></span>


## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="08285-192">Pochopení, jak budou doporučení mít vyšší prioritu</span><span class="sxs-lookup"><span data-stu-id="08285-192">Understanding how recommendations are prioritized</span></span>

<span data-ttu-id="08285-193">Každé doporučení je zadána hodnota vyvážení, která určuje relativní důležitost doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-193">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="08285-194">Jsou zobrazeny pouze 10 nejdůležitějších doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-194">Only the 10 most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="08285-195">Jak jsou vypočítávány váhu</span><span class="sxs-lookup"><span data-stu-id="08285-195">How weights are calculated</span></span>

<span data-ttu-id="08285-196">Váhy jsou agregovaných hodnot založena na tři klíčové faktory:</span><span class="sxs-lookup"><span data-stu-id="08285-196">Weightings are aggregate values based on three key factors:</span></span>

- <span data-ttu-id="08285-197">*Pravděpodobnosti* způsobí, že problém identifikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="08285-197">The *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="08285-198">Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-198">A higher probability equates to a larger overall score for the recommendation.</span></span>
- <span data-ttu-id="08285-199">*Dopad* problému na vaší organizaci, pokud ji způsobovat problémy.</span><span class="sxs-lookup"><span data-stu-id="08285-199">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="08285-200">Vyšší dopad rovná větší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-200">A higher impact equates to a larger overall score for the recommendation.</span></span>
- <span data-ttu-id="08285-201">*Úsilí* potřebnou k implementaci doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-201">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="08285-202">Vyšší úsilí rovná menší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-202">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="08285-203">Vyvážení pro každé doporučení je vyjádřený jako procentní podíl celkové skóre, které jsou k dispozici pro každou oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="08285-203">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="08285-204">Například pokud doporučení v oblasti fokus dostupnost a provozní kontinuita je skóre % 5, implementace tímto doporučením zvyšuje celkové skóre podle 5 % dostupnost a provozní kontinuita.</span><span class="sxs-lookup"><span data-stu-id="08285-204">For example, if a recommendation in the Availability and Business Continuity focus area has a score of 5%, implementing that recommendation increases your overall Availability and Business Continuity score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="08285-205">Konkrétní oblasti</span><span class="sxs-lookup"><span data-stu-id="08285-205">Focus areas</span></span>

<span data-ttu-id="08285-206">**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.</span><span class="sxs-lookup"><span data-stu-id="08285-206">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="08285-207">**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení, která pomůžou vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a schopné reagovat na měnící se infrastruktury potřebuje.</span><span class="sxs-lookup"><span data-stu-id="08285-207">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="08285-208">**Nasazení, upgrade a migrace** – v tomto poli fokus zobrazí doporučení, která vám pomohou upgradovat, migrace a nasazení systému SQL Server k vaší stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="08285-208">**Upgrade, Migration, and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy SQL Server to your existing infrastructure.</span></span>

<span data-ttu-id="08285-209">**Operace a monitorování** – v tomto poli fokus zobrazí doporučení, která pomůžou zjednodušení operací IT a implementaci preventivní údržby, maximalizovat výkon.</span><span class="sxs-lookup"><span data-stu-id="08285-209">**Operations and Monitoring** - This focus area shows recommendations to help streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="08285-210">Mají usilovat o stanovení skóre 100 % v každé oblasti fokus?</span><span class="sxs-lookup"><span data-stu-id="08285-210">Should you aim to score 100% in every focus area?</span></span>

<span data-ttu-id="08285-211">Ne nutně.</span><span class="sxs-lookup"><span data-stu-id="08285-211">Not necessarily.</span></span> <span data-ttu-id="08285-212">Doporučení jsou založené na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a znalostní báze.</span><span class="sxs-lookup"><span data-stu-id="08285-212">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="08285-213">Ale žádné serverové infrastruktury jsou stejné, a konkrétní doporučení může být vyšší nebo nižší relevantní pro vás.</span><span class="sxs-lookup"><span data-stu-id="08285-213">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="08285-214">Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou vystaveny v Internetu.</span><span class="sxs-lookup"><span data-stu-id="08285-214">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="08285-215">Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="08285-215">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="08285-216">Problémy, které jsou důležité, abyste vyspělých firmy může být méně důležité Startup.</span><span class="sxs-lookup"><span data-stu-id="08285-216">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="08285-217">Můžete určit, které konkrétní oblasti mají vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.</span><span class="sxs-lookup"><span data-stu-id="08285-217">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="08285-218">Každé doporučení obsahuje pokyny o tom, proč je důležité.</span><span class="sxs-lookup"><span data-stu-id="08285-218">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="08285-219">Tyto pokyny slouží k vyhodnocení, zda implementace doporučení je vhodné pro vás, daná povaze vašich IT služeb a obchodním potřebám vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="08285-219">Use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="08285-220">Použít assessment fokus oblasti doporučení</span><span class="sxs-lookup"><span data-stu-id="08285-220">Use assessment focus area recommendations</span></span>

<span data-ttu-id="08285-221">Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení.</span><span class="sxs-lookup"><span data-stu-id="08285-221">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="08285-222">Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="08285-222">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="08285-223">Po instalaci, zobrazí se souhrn doporučení pomocí System Center Operations Manager Assessment dlaždice na stránce Přehled v OMS.</span><span class="sxs-lookup"><span data-stu-id="08285-223">After it is installed, you can view the summary of recommendations by using the System Center Operations Manager Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="08285-224">Zobrazte vyhodnocování souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-224">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="08285-225">Proveďte opravné akce a zobrazit doporučení pro oblastí zájmu</span><span class="sxs-lookup"><span data-stu-id="08285-225">To view recommendations for a focus area and take corrective action</span></span>

1. <span data-ttu-id="08285-226">Na **přehled** klikněte na tlačítko **System Center Operations Manager Assessment** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="08285-226">On the **Overview** page, click the **System Center Operations Manager Assessment** tile.</span></span>
2. <span data-ttu-id="08285-227">Na **System Center Operations Manager Assessment** zkontrolujte souhrnné informace v jednom z okna oblasti fokus a pak klikněte na jednu zobrazíte doporučení pro tuto oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="08285-227">On the **System Center Operations Manager Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="08285-228">Na všech stránkách oblasti fokus můžete zobrazit seřazený podle priority doporučení, která se pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="08285-228">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="08285-229">Klikněte na tlačítko doporučení v části **vliv na objekty** Chcete-li zobrazit podrobnosti, proč se provádí doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-229">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="08285-230">![oblastí zájmu](./media/log-analytics-scom-assessment/focus-area.png)</span><span class="sxs-lookup"><span data-stu-id="08285-230">![focus area](./media/log-analytics-scom-assessment/focus-area.png)</span></span>
4. <span data-ttu-id="08285-231">Můžete provést nápravné akce navržený v **doporučované akce**.</span><span class="sxs-lookup"><span data-stu-id="08285-231">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="08285-232">Když položka vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="08285-232">When the item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="08285-233">Opravené položky se zobrazí jako **předán objekty**.</span><span class="sxs-lookup"><span data-stu-id="08285-233">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="08285-234">Ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="08285-234">Ignore recommendations</span></span>

<span data-ttu-id="08285-235">Pokud máte doporučení, které chcete ignorovat, můžete vytvořit textový soubor, který OMS používá k ochraně před doporučení ze storu ve výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="08285-235">If you have recommendations that you want to ignore, you can create a text file that OMS uses to prevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a><span data-ttu-id="08285-236">K identifikaci doporučení, které chcete ignorovat</span><span class="sxs-lookup"><span data-stu-id="08285-236">To identify recommendations that you want to ignore</span></span>

1. <span data-ttu-id="08285-237">Přihlaste se do pracovního prostoru a otevřete vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="08285-237">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="08285-238">Pro počítače ve vašem prostředí použijte následující dotaz, který seznam doporučení, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="08285-238">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    <span data-ttu-id="08285-239">Zde je snímek obrazovky zobrazující protokolu vyhledávací dotaz:</span><span class="sxs-lookup"><span data-stu-id="08285-239">Here's a screen shot showing the Log Search query:</span></span>  
    ![prohledávání protokolů](./media/log-analytics-scom-assessment/scom-log-search.png)

2. <span data-ttu-id="08285-241">Zvolte doporučení, které chcete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="08285-241">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="08285-242">Hodnoty pro RecommendationId budete používat v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="08285-242">You'll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="08285-243">Vytváření a používání textový soubor s IgnoreRecommendations.txt</span><span class="sxs-lookup"><span data-stu-id="08285-243">To create and use an IgnoreRecommendations.txt text file</span></span>

1. <span data-ttu-id="08285-244">Vytvořte soubor s názvem IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="08285-244">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="08285-245">Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení, které chcete OMS ignorovat na samostatném řádku a potom uložte a zavřete soubor.</span><span class="sxs-lookup"><span data-stu-id="08285-245">Paste or type each RecommendationId for each recommendation that you want OMS to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="08285-246">Uložte soubor v následující složce na každém počítači, kam chcete OMS ignorovat doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-246">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
4. <span data-ttu-id="08285-247">Na serveru pro správu nástroje Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.</span><span class="sxs-lookup"><span data-stu-id="08285-247">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="08285-248">Chcete-li ověřit, že se ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="08285-248">To verify that recommendations are ignored</span></span>

1. <span data-ttu-id="08285-249">Po na další naplánované vyhodnocení běží ve výchozím nastavení každých sedm dní, zadaný doporučení jsou označeny ignorovaná a nezobrazí se na řídicím panelu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="08285-249">After the next scheduled assessment runs, by default every seven days, the specified recommendations are marked Ignored and will not appear on the assessment dashboard.</span></span>
2. <span data-ttu-id="08285-250">Následující dotazy hledání protokolů můžete použít k zobrazení seznamu všech ignorováno doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-250">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. <span data-ttu-id="08285-251">Pokud se později rozhodnete, zda chcete zobrazit ignorováno doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.</span><span class="sxs-lookup"><span data-stu-id="08285-251">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="system-center-operations-manager-assessment-solution-faq"></a><span data-ttu-id="08285-252">Řešení System Center Operations Manager Assessment – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="08285-252">System Center Operations Manager Assessment solution FAQ</span></span>

<span data-ttu-id="08285-253">*Po přidání řešení pro vyhodnocení do části pracovní prostor OMS. Ale doporučení se nezobrazí. Proč ne?*</span><span class="sxs-lookup"><span data-stu-id="08285-253">*I added the assessment solution to my OMS workspace. But I don’t see the recommendations. Why not?*</span></span> <span data-ttu-id="08285-254">Po přidání řešení, použijte následující postup zobrazení doporučení na řídicím panelu OMS.</span><span class="sxs-lookup"><span data-stu-id="08285-254">After adding the solution, use the following steps view the recommendations on the OMS dashboard.</span></span>  

- [<span data-ttu-id="08285-255">Nastavit účet Spustit jako pro System Center Operations Manager hodnocení</span><span class="sxs-lookup"><span data-stu-id="08285-255">Set the Run As account for System Center Operations Manager Assessment</span></span>](#operations-manager-run-as-accounts-for-oms)  
- [<span data-ttu-id="08285-256">Konfigurace pravidla System Center Operations Manager hodnocení</span><span class="sxs-lookup"><span data-stu-id="08285-256">Configure the System Center Operations Manager Assessment rule</span></span>](#configure-the-assessment-rule)


<span data-ttu-id="08285-257">*Existuje způsob, jak nakonfigurovat, jak často se hodnocení spouštět?*</span><span class="sxs-lookup"><span data-stu-id="08285-257">*Is there a way to configure how often the assessment runs?*</span></span> <span data-ttu-id="08285-258">Ano.</span><span class="sxs-lookup"><span data-stu-id="08285-258">Yes.</span></span> <span data-ttu-id="08285-259">V tématu [nakonfigurovat spuštění četnost](#configure-the-run-frequency).</span><span class="sxs-lookup"><span data-stu-id="08285-259">See [Configure the run frequency](#configure-the-run-frequency).</span></span>

<span data-ttu-id="08285-260">*Pokud je zjištěno jiný server, poté, co byly přidány řešení System Center Operations Manager hodnocení, bude ho vyhodnocena?*</span><span class="sxs-lookup"><span data-stu-id="08285-260">*If another server is discovered after I’ve added the System Center Operations Manager Assessment solution, will it be assessed?*</span></span> <span data-ttu-id="08285-261">Ano, po zjišťování, se bude posouzeno chvíle – ve výchozím nastavení, každých sedm dní.</span><span class="sxs-lookup"><span data-stu-id="08285-261">Yes, after discovery, it is assessed from then on--by default, every seven days.</span></span>

<span data-ttu-id="08285-262">*Jaký je název procesu, který nemá shromažďování dat?*</span><span class="sxs-lookup"><span data-stu-id="08285-262">*What is the name of the process that does the data collection?*</span></span> <span data-ttu-id="08285-263">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="08285-263">AdvisorAssessment.exe</span></span>

<span data-ttu-id="08285-264">*Kde proces AdvisorAssessment.exe spustit?*</span><span class="sxs-lookup"><span data-stu-id="08285-264">*Where does the AdvisorAssessment.exe process run?*</span></span> <span data-ttu-id="08285-265">AdvisorAssessment.exe kompatibilní serveru pro správu služby Health Service se podporou pravidlo hodnocení.</span><span class="sxs-lookup"><span data-stu-id="08285-265">AdvisorAssessment.exe runs under the HealthService of the management server where the assessment rule is enabled.</span></span> <span data-ttu-id="08285-266">Pomocí tohoto procesu, zjišťování celé prostředí je dosaženo pomocí vzdálených dat kolekce.</span><span class="sxs-lookup"><span data-stu-id="08285-266">Using that process, discovery of your entire environment is achieved through remote data collection.</span></span>

<span data-ttu-id="08285-267">*Jak dlouho trvá pro shromažďování dat?*</span><span class="sxs-lookup"><span data-stu-id="08285-267">*How long does it take for data collection?*</span></span> <span data-ttu-id="08285-268">Shromažďování dat na serveru trvá asi jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="08285-268">Data collection on the server takes about one hour.</span></span> <span data-ttu-id="08285-269">Může trvat déle v prostředích, která mají mnoho instancí nástroje Operations Manager nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="08285-269">It may take longer in environments that have many Operations Manager instances or databases.</span></span>

<span data-ttu-id="08285-270">*Co když lze nastavit interval hodnocení na méně než 1 440 minut?*</span><span class="sxs-lookup"><span data-stu-id="08285-270">*What if I set the interval of the assessment to less than 1440 minutes?*</span></span> <span data-ttu-id="08285-271">Hodnocení je předem nakonfigurovaný pro spouštění na maximálně jednou za den.</span><span class="sxs-lookup"><span data-stu-id="08285-271">The assessment is pre-configured to run at a maximum of once per day.</span></span> <span data-ttu-id="08285-272">Pokud přepíšete hodnota intervalu na hodnotu menší než 1 440 minut, pak hodnocení používá jako hodnota intervalu 1 440 minut.</span><span class="sxs-lookup"><span data-stu-id="08285-272">If you override the interval value to a value less than 1440 minutes, then the assessment uses 1440 minutes as the interval value.</span></span>

<span data-ttu-id="08285-273">*Jak vědět, pokud se předběžné selhání?*</span><span class="sxs-lookup"><span data-stu-id="08285-273">*How to know if there are pre-requisite failures?*</span></span> <span data-ttu-id="08285-274">Pokud spustili hodnocení a se nezobrazí výsledky, je pravděpodobné, že některé předpoklady pro hodnocení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="08285-274">If the assessment ran and you don't see results, then it is likely that some of the pre-requisites for the assessment failed.</span></span> <span data-ttu-id="08285-275">Můžete provést dotazy: `Type=Operation Solution=SCOMAssessment` a `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` v protokolu hledat neúspěšné požadavky.</span><span class="sxs-lookup"><span data-stu-id="08285-275">You can execute queries: `Type=Operation Solution=SCOMAssessment` and `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` in Log Search to see the failed pre-requisites.</span></span>

<span data-ttu-id="08285-276">*Došlo `Failed to connect to the SQL Instance (….).` zpráva předběžné selhání. Co je problém?*</span><span class="sxs-lookup"><span data-stu-id="08285-276">*There is a `Failed to connect to the SQL Instance (….).` message in pre-requisite failures. What is the issue?*</span></span> <span data-ttu-id="08285-277">AdvisorAssessment.exe, proces, který shromažďuje data, se spustí v serveru pro správu služby Health Service.</span><span class="sxs-lookup"><span data-stu-id="08285-277">AdvisorAssessment.exe, the process that collects data, runs under the HealthService of the management server.</span></span> <span data-ttu-id="08285-278">V rámci hodnocení se proces pokusí připojit k systému SQL Server, kde databáze nástroje Operations Manager nachází.</span><span class="sxs-lookup"><span data-stu-id="08285-278">As part of the assessment, the process attempts to connect to the SQL Server where the Operations Manager database is present.</span></span> <span data-ttu-id="08285-279">Této chybě může dojít, když pravidla brány firewall zablokovat připojení k instanci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="08285-279">This error can occur when firewall rules block the connection to the SQL Server instance.</span></span>

<span data-ttu-id="08285-280">*Jaký typ dat shromažďovaných?*</span><span class="sxs-lookup"><span data-stu-id="08285-280">*What type of data is collected?*</span></span> <span data-ttu-id="08285-281">Se shromažďují následující typy dat: - dat služby WMI – registru data – data protokolu událostí – Operations Manager data prostřednictvím prostředí Windows PowerShell, dotazy SQL a soubor informace kolekce.</span><span class="sxs-lookup"><span data-stu-id="08285-281">The following types of data are collected: - WMI data - Registry data - EventLog data - Operations Manager data through Windows PowerShell, SQL Queries and File information collector.</span></span>

<span data-ttu-id="08285-282">*Proč je nutné nakonfigurovat účet Spustit jako?*</span><span class="sxs-lookup"><span data-stu-id="08285-282">*Why do I have to configure a Run As Account?*</span></span> <span data-ttu-id="08285-283">Pro server Operations Manager se spouštějí různé dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="08285-283">For an Operations Manager server, various SQL queries are run.</span></span> <span data-ttu-id="08285-284">Aby mohly spustit musíte použít účet Spustit jako s potřebnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="08285-284">In order for them to run, you must use a Run As Account with necessary permissions.</span></span> <span data-ttu-id="08285-285">Kromě toho je potřeba dotaz WMI přihlašovací údaje místního správce.</span><span class="sxs-lookup"><span data-stu-id="08285-285">In addition, local administrator credentials are required to query WMI.</span></span>

<span data-ttu-id="08285-286">*Proč zobrazit pouze prvních 10 doporučení?*</span><span class="sxs-lookup"><span data-stu-id="08285-286">*Why display only the top 10 recommendations?*</span></span> <span data-ttu-id="08285-287">Namísto udělení vyčerpávající, čtenáře seznam úloh, doporučujeme, byste se zaměřit na první adresování seřazený podle priority doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-287">Instead of giving you an exhaustive, overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="08285-288">Po jejich řešení, bude k dispozici další doporučení.</span><span class="sxs-lookup"><span data-stu-id="08285-288">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="08285-289">Pokud ji chcete zobrazit podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="08285-289">If you prefer to see the detailed list, you can view all recommendations using Log Search.</span></span>

<span data-ttu-id="08285-290">*Existuje způsob, jak ignorovat doporučení?*</span><span class="sxs-lookup"><span data-stu-id="08285-290">*Is there a way to ignore a recommendation?*</span></span> <span data-ttu-id="08285-291">Ano, najdete v článku [ignorovat doporučení](#Ignore-recommendations).</span><span class="sxs-lookup"><span data-stu-id="08285-291">Yes, see the [Ignore recommendations](#Ignore-recommendations).</span></span>


## <a name="next-steps"></a><span data-ttu-id="08285-292">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08285-292">Next steps</span></span>

- <span data-ttu-id="08285-293">[V protokolech Hledat](log-analytics-log-searches.md) zobrazíte doporučení a podrobné údaje o System Center Operations Manager hodnocení.</span><span class="sxs-lookup"><span data-stu-id="08285-293">[Search logs](log-analytics-log-searches.md) to view detailed System Center Operations Manager Assessment data and recommendations.</span></span>
