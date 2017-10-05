---
title: "Optimalizace prostředí systému SQL Server s nástrojem Azure Log Analytics | Microsoft Docs"
description: "S Azure Log Analytics můžete řešení pro vyhodnocení SQL pro vyhodnocení rizik a stavu prostředí serveru SQL v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2aed3315fe60ace46dfb4176dc13aa417257b0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a><span data-ttu-id="12a23-103">Optimalizace prostředí SQL serveru do řešení pro vyhodnocení SQL v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="12a23-103">Optimize your SQL Server environment with the SQL Assessment solution in Log Analytics</span></span>

![Symbol vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

<span data-ttu-id="12a23-105">Řešení pro vyhodnocení SQL můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="12a23-105">You can use the SQL Assessment solution to assess the risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="12a23-106">Tento článek vám pomůže nainstalovat řešení tak, že lze provádět opravné akce pro potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="12a23-106">This article will help you install the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="12a23-107">Toto řešení poskytuje seznam doporučení, které jsou specifické pro infrastrukturu nasazené serverů seřazený podle priority.</span><span class="sxs-lookup"><span data-stu-id="12a23-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="12a23-108">Doporučení jsou rozdělené mezi šest konkrétní oblasti, které vám pomůžou rychle pochopit riziko a proveďte opravné akce.</span><span class="sxs-lookup"><span data-stu-id="12a23-108">The recommendations are categorized across six focus areas which help you quickly understand the risk and take corrective action.</span></span>

<span data-ttu-id="12a23-109">Doporučení, která jsou založené na znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách.</span><span class="sxs-lookup"><span data-stu-id="12a23-109">The recommendations made are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="12a23-110">Každé doporučení obsahuje informace, proč může pro vás důležitá problém a implementaci navrhované změny.</span><span class="sxs-lookup"><span data-stu-id="12a23-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="12a23-111">Můžete vybrat konkrétní oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.</span><span class="sxs-lookup"><span data-stu-id="12a23-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="12a23-112">Poté, co jste přidali řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazený na **vyhodnocení SQL** řídicí panel infrastruktury ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="12a23-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **SQL Assessment** dashboard for the infrastructure in your environment.</span></span> <span data-ttu-id="12a23-113">Následující části popisují, jak používat informace na **vyhodnocení SQL** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="12a23-113">The following sections describe how to use the information on the **SQL Assessment** dashboard, where you can view and then take recommended actions for your SQL server infrastructure.</span></span>

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="12a23-116">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="12a23-116">Installing and configuring the solution</span></span>
<span data-ttu-id="12a23-117">Vyhodnocení SQL pracuje s všechny aktuálně podporované verze systému SQL Server v edicích Standard, Developer a Enterprise.</span><span class="sxs-lookup"><span data-stu-id="12a23-117">SQL Assessment works with all currently supported versions of SQL Server for the Standard, Developer, and Enterprise editions.</span></span>

<span data-ttu-id="12a23-118">Použijte následující informace k instalaci a konfiguraci řešení.</span><span class="sxs-lookup"><span data-stu-id="12a23-118">Use the following information to install and configure the solution.</span></span>

* <span data-ttu-id="12a23-119">Na serverech, které mají nainstalovaný server SQL Server je nutné nainstalovat agenty.</span><span class="sxs-lookup"><span data-stu-id="12a23-119">Agents must be installed on servers that have SQL Server installed.</span></span>
* <span data-ttu-id="12a23-120">Řešení pro vyhodnocení SQL vyžaduje podporovanou verzi rozhraní .NET Framework 4 nainstalován na každý počítač, který má OMS agent.</span><span class="sxs-lookup"><span data-stu-id="12a23-120">The SQL Assessment solution requires a supported version of .NET Framework 4 installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="12a23-121">Abyste mohli nainstalovat řešení, musí být uživatel správce nebo Přispěvatel k předplatnému Azure při použití portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="12a23-121">In order to install the solution, the user must be an administrator or contributor to the Azure subscription when using the Azure portal.</span></span> <span data-ttu-id="12a23-122">Navíc musí být uživatel členem skupiny přispěvatelů pracovního prostoru OMS nebo mít roli administrátora na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="12a23-122">In addition, the user must be a member of the OMS workspace contributor or administrator role in the OMS portal.</span></span>
* <span data-ttu-id="12a23-123">Při použití agenta nástroje Operations Manager s vyhodnocení SQL, musíte použít účet Operations Manager Run-As.</span><span class="sxs-lookup"><span data-stu-id="12a23-123">When using the Operations Manager agent with SQL Assessment, you'll need to use an Operations Manager Run-As account.</span></span> <span data-ttu-id="12a23-124">V tématu [účty nástroje Operations Manager spustit jako pro OMS](#operations-manager-run-as-accounts-for-oms) níže Další informace.</span><span class="sxs-lookup"><span data-stu-id="12a23-124">See [Operations Manager run-as accounts for OMS](#operations-manager-run-as-accounts-for-oms) below for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="12a23-125">MMA agent nepodporuje účty Operations Manager Run-As.</span><span class="sxs-lookup"><span data-stu-id="12a23-125">The MMA agent does not support Operations Manager Run-As accounts.</span></span>
  >
  >
* <span data-ttu-id="12a23-126">Přidat do pracovního prostoru OMS pomocí postupu popsaného v řešení pro vyhodnocení SQL [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="12a23-126">Add the SQL Assessment solution to your OMS workspace using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="12a23-127">Není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="12a23-127">There is no further configuration required.</span></span>

> [!NOTE]
> <span data-ttu-id="12a23-128">Po přidání řešení, soubor AdvisorAssessment.exe je přidán na servery s agenty.</span><span class="sxs-lookup"><span data-stu-id="12a23-128">After you've added the solution, the AdvisorAssessment.exe file is added to servers with agents.</span></span> <span data-ttu-id="12a23-129">Konfigurační data je čtení a pak se odešle službě OMS v cloudu pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="12a23-129">Configuration data is read and then sent to the OMS service in the cloud for processing.</span></span> <span data-ttu-id="12a23-130">Logika se použije pro přijatá data a cloudové služby zaznamenává data.</span><span class="sxs-lookup"><span data-stu-id="12a23-130">Logic is applied to the received data and the cloud service records the data.</span></span>

## <a name="sql-assessment-data-collection-details"></a><span data-ttu-id="12a23-131">Podrobnosti kolekce dat vyhodnocení SQL</span><span class="sxs-lookup"><span data-stu-id="12a23-131">SQL Assessment data collection details</span></span>
<span data-ttu-id="12a23-132">Vyhodnocení SQL shromažďuje data rozhraní WMI, registru dat, údaje o výkonu a výsledky zobrazení dynamické správy SQL Server pomocí agentů, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="12a23-132">SQL Assessment collects WMI data, registry data, performance data, and SQL Server dynamic management view results using the agents that you have enabled.</span></span>

<span data-ttu-id="12a23-133">Následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.</span><span class="sxs-lookup"><span data-stu-id="12a23-133">The following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="12a23-134">Platforma</span><span class="sxs-lookup"><span data-stu-id="12a23-134">platform</span></span> | <span data-ttu-id="12a23-135">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="12a23-135">Direct Agent</span></span> | <span data-ttu-id="12a23-136">Agenta nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="12a23-136">SCOM agent</span></span> | <span data-ttu-id="12a23-137">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="12a23-137">Azure Storage</span></span> | <span data-ttu-id="12a23-138">SCOM vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="12a23-138">SCOM required?</span></span> | <span data-ttu-id="12a23-139">Data agenta SCOM odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="12a23-139">SCOM agent data sent via management group</span></span> | <span data-ttu-id="12a23-140">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="12a23-140">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="12a23-141">Windows</span><span class="sxs-lookup"><span data-stu-id="12a23-141">Windows</span></span> | <span data-ttu-id="12a23-142">&#8226;</span><span class="sxs-lookup"><span data-stu-id="12a23-142">&#8226;</span></span> | <span data-ttu-id="12a23-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="12a23-143">&#8226;</span></span> |  |  | <span data-ttu-id="12a23-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="12a23-144">&#8226;</span></span> |<span data-ttu-id="12a23-145">7 dní</span><span class="sxs-lookup"><span data-stu-id="12a23-145">7 days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="12a23-146">Účty spustit jako nástroje Operations Manager pro OMS</span><span class="sxs-lookup"><span data-stu-id="12a23-146">Operations Manager run-as accounts for OMS</span></span>
<span data-ttu-id="12a23-147">Analýzy protokolů v OMS skupinu pro správu a agent nástroje Operations Manager používá ke shromažďování a odesílání dat do služby OMS.</span><span class="sxs-lookup"><span data-stu-id="12a23-147">Log Analytics in OMS uses the Operations Manager agent and management group to collect and send data to the OMS service.</span></span> <span data-ttu-id="12a23-148">Sestavení OMS na sady management Pack pro úlohy zajistit přidanou hodnotou services.</span><span class="sxs-lookup"><span data-stu-id="12a23-148">OMS builds upon management packs for workloads to provide value-add services.</span></span> <span data-ttu-id="12a23-149">Každé zatížení vyžaduje určeného pro konkrétní úlohy oprávnění ke spouštění sad management Pack v odlišném kontextu zabezpečení, jako je například účet domény.</span><span class="sxs-lookup"><span data-stu-id="12a23-149">Each workload requires workload-specific privileges to run management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="12a23-150">Budete muset zadat přihlašovací údaje nakonfigurováním Operations Manager účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="12a23-150">You need to provide credential information by configuring an Operations Manager Run As account.</span></span>

<span data-ttu-id="12a23-151">K nastavení účet Spustit jako nástroje Operations Manager pro vyhodnocení SQL použijte následující informace.</span><span class="sxs-lookup"><span data-stu-id="12a23-151">Use the following information to set the Operations Manager run-as account for SQL Assessment.</span></span>

### <a name="set-the-run-as-account-for-sql-assessment"></a><span data-ttu-id="12a23-152">Nastavit účet Spustit jako pro vyhodnocení SQL</span><span class="sxs-lookup"><span data-stu-id="12a23-152">Set the Run As account for SQL assessment</span></span>
 <span data-ttu-id="12a23-153">Pokud už používáte sadu management pack systému SQL Server, používejte tento účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="12a23-153">If you are already using the SQL Server management pack, you should use that Run As account.</span></span>

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a><span data-ttu-id="12a23-154">Jak nakonfigurovat účet Spustit jako SQL v konzoli Operations console</span><span class="sxs-lookup"><span data-stu-id="12a23-154">To configure the SQL Run As account in the Operations console</span></span>
> [!NOTE]
> <span data-ttu-id="12a23-155">Pokud používáte přímé agenta OMS, nikoli agenta nástroje SCOM, sada management pack vždy běží v kontextu zabezpečení účtu místního systému.</span><span class="sxs-lookup"><span data-stu-id="12a23-155">If you are using the OMS direct agent, rather than the SCOM agent, the management pack always runs in the security context of the Local System account.</span></span> <span data-ttu-id="12a23-156">Přeskočit kroky 1 až 5 níže a spusťte buď T-SQL nebo ukázce prostředí Powershell zadáním NT AUTHORITY\SYSTEM jako uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="12a23-156">Skip steps 1-5 below, and run either the T-SQL or Powershell sample, specifying NT AUTHORITY\SYSTEM as the user name.</span></span>
>
>

1. <span data-ttu-id="12a23-157">V nástroji Operations Manager, otevřete konzoli Operations console a pak klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="12a23-157">In Operations Manager, open the Operations console, and then click **Administration**.</span></span>
2. <span data-ttu-id="12a23-158">V části **konfigurace spustit jako**, klikněte na tlačítko **profily**a otevřete **OMS SQL Assessment profilu spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="12a23-158">Under **Run As Configuration**, click **Profiles**, and open **OMS SQL Assessment Run As Profile**.</span></span>
3. <span data-ttu-id="12a23-159">Na **účty spustit jako** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="12a23-159">On the **Run As Accounts** page, click **Add**.</span></span>
4. <span data-ttu-id="12a23-160">Vyberte účet Spustit jako systému Windows, který obsahuje přihlašovacích údajů nezbytných pro systém SQL Server, nebo klikněte na **nový** k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="12a23-160">Select a Windows Run As account that contains the credentials needed for SQL Server, or click **New** to create one.</span></span>

   > [!NOTE]
   > <span data-ttu-id="12a23-161">Typ účtu spustit jako musí být Windows.</span><span class="sxs-lookup"><span data-stu-id="12a23-161">The Run As account type must be Windows.</span></span> <span data-ttu-id="12a23-162">Účet Spustit jako musí být také součástí místní skupiny Administrators na všech serverech Windows, který je hostitelem instance serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="12a23-162">The Run As account must also be part of Local Administrators group on all Windows Servers hosting SQL Server Instances.</span></span>
   >
   >
5. <span data-ttu-id="12a23-163">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="12a23-163">Click **Save**.</span></span>
6. <span data-ttu-id="12a23-164">Upravit a potom spusťte následující ukázka T-SQL na každou instanci systému SQL Server k udělení minimální oprávnění potřebná k provedení vyhodnocení SQL pro účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="12a23-164">Modify and then execute the following T-SQL sample on each SQL Server Instance to grant minimum permissions required to Run As Account to perform SQL Assessment.</span></span> <span data-ttu-id="12a23-165">Ale nemusíte to dělat, když se účet Spustit jako už je součástí role serveru sysadmin na instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="12a23-165">However, you don’t need to do this if a Run As Account is already part of the sysadmin server role on SQL Server Instances.</span></span>

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a><span data-ttu-id="12a23-166">Jak nakonfigurovat účet Spustit jako SQL pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="12a23-166">To configure the SQL Run As account using Windows PowerShell</span></span>
<span data-ttu-id="12a23-167">Otevřete okno prostředí PowerShell a spusťte následující skript po aktualizaci s vaše informace:</span><span class="sxs-lookup"><span data-stu-id="12a23-167">Open a PowerShell window and run the following script after you’ve updated it with your information:</span></span>

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="12a23-168">Pochopení, jak budou doporučení mít vyšší prioritu</span><span class="sxs-lookup"><span data-stu-id="12a23-168">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="12a23-169">Každé doporučení je zadána hodnota vyvážení, která určuje relativní důležitost doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-169">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="12a23-170">Zobrazí se jenom deset nejdůležitějších doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-170">Only the ten most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="12a23-171">Jak jsou vypočítávány váhu</span><span class="sxs-lookup"><span data-stu-id="12a23-171">How weights are calculated</span></span>
<span data-ttu-id="12a23-172">Váhy jsou agregovaných hodnot založena na tři klíčové faktory:</span><span class="sxs-lookup"><span data-stu-id="12a23-172">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="12a23-173">*Pravděpodobnosti* způsobí, že problém identifikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="12a23-173">The *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="12a23-174">Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-174">A higher probability equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="12a23-175">*Dopad* problému na vaší organizaci, pokud ji způsobovat problémy.</span><span class="sxs-lookup"><span data-stu-id="12a23-175">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="12a23-176">Vyšší dopad rovná větší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-176">A higher impact equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="12a23-177">*Úsilí* potřebnou k implementaci doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-177">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="12a23-178">Vyšší úsilí rovná menší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-178">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="12a23-179">Vyvážení pro každé doporučení je vyjádřený jako procentní podíl celkové skóre, které jsou k dispozici pro každou oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="12a23-179">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="12a23-180">Například pokud doporučení v oblasti zabezpečení a dodržování předpisů fokus je skóre % 5, implementace tímto doporučením zvýší celkové skóre podle 5 % zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="12a23-180">For example, if a recommendation in the Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="12a23-181">Konkrétní oblasti</span><span class="sxs-lookup"><span data-stu-id="12a23-181">Focus areas</span></span>
<span data-ttu-id="12a23-182">**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.</span><span class="sxs-lookup"><span data-stu-id="12a23-182">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="12a23-183">**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.</span><span class="sxs-lookup"><span data-stu-id="12a23-183">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="12a23-184">**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení, která pomůžou vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a schopné reagovat na měnící se infrastruktury potřebuje.</span><span class="sxs-lookup"><span data-stu-id="12a23-184">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="12a23-185">**Upgrade, nasazení a migrace** – v tomto poli fokus zobrazí doporučení, která vám pomohou upgradovat, migrace a nasazení systému SQL Server k vaší stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="12a23-185">**Upgrade, Migration and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy SQL Server to your existing infrastructure.</span></span>

<span data-ttu-id="12a23-186">**Operace a monitorování** – v tomto poli fokus zobrazí doporučení, která pomůžou zjednodušení operací IT a implementaci preventivní údržby, maximalizovat výkon.</span><span class="sxs-lookup"><span data-stu-id="12a23-186">**Operations and Monitoring** - This focus area shows recommendations to help streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

<span data-ttu-id="12a23-187">**Správa změn a konfigurací** – v tomto poli fokus zobrazí doporučení pomáhají chránit každodenní operace, zkontrolujte, zda nemáte změny negativně ovlivnit vaši infrastrukturu, vytvořit procedury řízení změn a ke sledování a monitorování Konfigurace systému.</span><span class="sxs-lookup"><span data-stu-id="12a23-187">**Change and Configuration Management** - This focus area shows recommendations to help protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and to track and audit system configurations.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="12a23-188">Mají usilovat o stanovení skóre 100 % v každé oblasti fokus?</span><span class="sxs-lookup"><span data-stu-id="12a23-188">Should you aim to score 100% in every focus area?</span></span>
<span data-ttu-id="12a23-189">Ne nutně.</span><span class="sxs-lookup"><span data-stu-id="12a23-189">Not necessarily.</span></span> <span data-ttu-id="12a23-190">Doporučení jsou založené na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a znalostní báze.</span><span class="sxs-lookup"><span data-stu-id="12a23-190">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="12a23-191">Ale žádné serverové infrastruktury jsou stejné, a konkrétní doporučení může být vyšší nebo nižší relevantní pro vás.</span><span class="sxs-lookup"><span data-stu-id="12a23-191">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="12a23-192">Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou vystaveny v Internetu.</span><span class="sxs-lookup"><span data-stu-id="12a23-192">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="12a23-193">Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="12a23-193">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="12a23-194">Problémy, které jsou důležité, abyste vyspělých firmy může být méně důležité Startup.</span><span class="sxs-lookup"><span data-stu-id="12a23-194">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="12a23-195">Můžete určit, které konkrétní oblasti mají vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.</span><span class="sxs-lookup"><span data-stu-id="12a23-195">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="12a23-196">Každé doporučení obsahuje pokyny o tom, proč je důležité.</span><span class="sxs-lookup"><span data-stu-id="12a23-196">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="12a23-197">Měli byste použít tyto pokyny k vyhodnocení, zda implementace doporučení je vhodné pro vás, daná povaze vašich IT služeb a obchodním potřebám vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="12a23-197">You should use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="12a23-198">Použít assessment fokus oblasti doporučení</span><span class="sxs-lookup"><span data-stu-id="12a23-198">Use assessment focus area recommendations</span></span>
<span data-ttu-id="12a23-199">Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení.</span><span class="sxs-lookup"><span data-stu-id="12a23-199">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="12a23-200">Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="12a23-200">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="12a23-201">Po instalaci, zobrazí se souhrn doporučení pomocí dlaždice vyhodnocení SQL na stránce Přehled v OMS.</span><span class="sxs-lookup"><span data-stu-id="12a23-201">After it is installed, you can view the summary of recommendations by using the SQL Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="12a23-202">Zobrazte vyhodnocování souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-202">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="12a23-203">Proveďte opravné akce a zobrazit doporučení pro oblastí zájmu</span><span class="sxs-lookup"><span data-stu-id="12a23-203">To view recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="12a23-204">Na **přehled** klikněte na tlačítko **vyhodnocení SQL** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="12a23-204">On the **Overview** page, click the **SQL Assessment** tile.</span></span>
2. <span data-ttu-id="12a23-205">Na **vyhodnocení SQL** zkontrolujte souhrnné informace v jednom z okna oblasti fokus a pak klikněte na jednu zobrazíte doporučení pro tuto oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="12a23-205">On the **SQL Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="12a23-206">Na všech stránkách oblasti fokus můžete zobrazit seřazený podle priority doporučení, která se pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="12a23-206">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="12a23-207">Klikněte na tlačítko doporučení v části **vliv na objekty** Chcete-li zobrazit podrobnosti, proč se provádí doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-207">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="12a23-208">![Obrázek doporučení vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span><span class="sxs-lookup"><span data-stu-id="12a23-208">![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span></span>
4. <span data-ttu-id="12a23-209">Můžete provést nápravné akce navržený v **doporučované akce**.</span><span class="sxs-lookup"><span data-stu-id="12a23-209">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="12a23-210">Když položka vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="12a23-210">When the item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="12a23-211">Opravené položky se zobrazí jako **předán objekty**.</span><span class="sxs-lookup"><span data-stu-id="12a23-211">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="12a23-212">Ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="12a23-212">Ignore recommendations</span></span>
<span data-ttu-id="12a23-213">Pokud máte doporučení, které chcete ignorovat, můžete vytvořit textový soubor, který OMS použije k zabránění doporučení ze storu ve výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="12a23-213">If you have recommendations that you want to ignore, you can create a text file that OMS will use to prevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a><span data-ttu-id="12a23-214">K identifikaci doporučení, které se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="12a23-214">To identify recommendations that you will ignore</span></span>
1. <span data-ttu-id="12a23-215">Přihlaste se do pracovního prostoru a otevřete vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="12a23-215">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="12a23-216">Pro počítače ve vašem prostředí použijte následující dotaz, který seznam doporučení, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="12a23-216">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   <span data-ttu-id="12a23-217">Zde je snímek obrazovky zobrazující protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="12a23-217">Here's a screen shot showing the Log Search query: ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span></span>
2. <span data-ttu-id="12a23-218">Zvolte doporučení, které chcete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="12a23-218">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="12a23-219">Hodnoty pro RecommendationId budete používat v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="12a23-219">You’ll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="12a23-220">Vytváření a používání textový soubor s IgnoreRecommendations.txt</span><span class="sxs-lookup"><span data-stu-id="12a23-220">To create and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="12a23-221">Vytvořte soubor s názvem IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="12a23-221">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="12a23-222">Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení, které chcete OMS ignorovat na samostatném řádku a potom uložte a zavřete soubor.</span><span class="sxs-lookup"><span data-stu-id="12a23-222">Paste or type each RecommendationId for each recommendation that you want OMS to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="12a23-223">Uložte soubor v následující složce na každém počítači, kam chcete OMS ignorovat doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-223">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
   * <span data-ttu-id="12a23-224">Na počítačích s Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování</span><span class="sxs-lookup"><span data-stu-id="12a23-224">On computers with the Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="12a23-225">Na serveru pro správu nástroje Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span><span class="sxs-lookup"><span data-stu-id="12a23-225">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="12a23-226">Chcete-li ověřit, že se ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="12a23-226">To verify that recommendations are ignored</span></span>
1. <span data-ttu-id="12a23-227">Po na další naplánované vyhodnocení běží ve výchozím nastavení každých 7 dní, zadaný doporučení jsou označeny ignorovaná a nezobrazí se na řídicím panelu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="12a23-227">After the next scheduled assessment runs, by default every 7 days, the specified recommendations are marked Ignored and will not appear on the assessment dashboard.</span></span>
2. <span data-ttu-id="12a23-228">Následující dotazy hledání protokolů můžete použít k zobrazení seznamu všech ignorováno doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-228">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. <span data-ttu-id="12a23-229">Pokud se později rozhodnete, zda chcete zobrazit ignorováno doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.</span><span class="sxs-lookup"><span data-stu-id="12a23-229">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="sql-assessment-solution-faq"></a><span data-ttu-id="12a23-230">Řešení pro vyhodnocení SQL – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="12a23-230">SQL Assessment solution FAQ</span></span>
<span data-ttu-id="12a23-231">*Jak často posouzení spustit?*</span><span class="sxs-lookup"><span data-stu-id="12a23-231">*How often does an assessment run?*</span></span>

* <span data-ttu-id="12a23-232">Posouzení spouští každých 7 dní.</span><span class="sxs-lookup"><span data-stu-id="12a23-232">The assessment runs every 7 days.</span></span>

<span data-ttu-id="12a23-233">*Existuje způsob, jak nakonfigurovat, jak často se hodnocení spouštět?*</span><span class="sxs-lookup"><span data-stu-id="12a23-233">*Is there a way to configure how often the assessment runs?*</span></span>

* <span data-ttu-id="12a23-234">V tuto chvíli to není možné.</span><span class="sxs-lookup"><span data-stu-id="12a23-234">Not at this time.</span></span>

<span data-ttu-id="12a23-235">*Pokud je zjištěno jiný server, poté, co byly přidány řešení pro vyhodnocení SQL, bude ho vyhodnocena?*</span><span class="sxs-lookup"><span data-stu-id="12a23-235">*If another server is discovered after I’ve added the SQL assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="12a23-236">Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.</span><span class="sxs-lookup"><span data-stu-id="12a23-236">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="12a23-237">*Pokud dojde k deaktivaci serveru, když ho se odebere z hodnocení?*</span><span class="sxs-lookup"><span data-stu-id="12a23-237">*If a server is decommissioned, when will it be removed from the assessment?*</span></span>

* <span data-ttu-id="12a23-238">Pokud server není odesílání dat 3 týdny, bude odebrán.</span><span class="sxs-lookup"><span data-stu-id="12a23-238">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="12a23-239">*Jaký je název procesu, který nemá shromažďování dat?*</span><span class="sxs-lookup"><span data-stu-id="12a23-239">*What is the name of the process that does the data collection?*</span></span>

* <span data-ttu-id="12a23-240">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="12a23-240">AdvisorAssessment.exe</span></span>

<span data-ttu-id="12a23-241">*Jak dlouho trvá na shromáždění dat?*</span><span class="sxs-lookup"><span data-stu-id="12a23-241">*How long does it take for data to be collected?*</span></span>

* <span data-ttu-id="12a23-242">Kolekce skutečná data na serveru trvá asi 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="12a23-242">The actual data collection on the server takes about 1 hour.</span></span> <span data-ttu-id="12a23-243">Může trvat déle na serverech, které mají velký počet instancí SQL nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="12a23-243">It may take longer on servers that have a large number of SQL instances or databases.</span></span>

<span data-ttu-id="12a23-244">*Jaký typ dat shromažďovaných?*</span><span class="sxs-lookup"><span data-stu-id="12a23-244">*What type of data is collected?*</span></span>

* <span data-ttu-id="12a23-245">Se shromažďují následující typy dat:</span><span class="sxs-lookup"><span data-stu-id="12a23-245">The following types of data are collected:</span></span>
  * <span data-ttu-id="12a23-246">ROZHRANÍ WMI</span><span class="sxs-lookup"><span data-stu-id="12a23-246">WMI</span></span>
  * <span data-ttu-id="12a23-247">Registru</span><span class="sxs-lookup"><span data-stu-id="12a23-247">Registry</span></span>
  * <span data-ttu-id="12a23-248">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="12a23-248">Performance counters</span></span>
  * <span data-ttu-id="12a23-249">Zobrazení dynamické správy SQL (DMV).</span><span class="sxs-lookup"><span data-stu-id="12a23-249">SQL dynamic management views (DMV).</span></span>

<span data-ttu-id="12a23-250">*Existuje způsob, jak nakonfigurovat, když jsou shromažďována data?*</span><span class="sxs-lookup"><span data-stu-id="12a23-250">*Is there a way to configure when data is collected?*</span></span>

* <span data-ttu-id="12a23-251">V tuto chvíli to není možné.</span><span class="sxs-lookup"><span data-stu-id="12a23-251">Not at this time.</span></span>

<span data-ttu-id="12a23-252">*Proč je nutné nakonfigurovat účet Spustit jako?*</span><span class="sxs-lookup"><span data-stu-id="12a23-252">*Why do I have to configure a Run As Account?*</span></span>

* <span data-ttu-id="12a23-253">Pro systém SQL Server se spouštějí na malý počet dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="12a23-253">For SQL Server, a small number of SQL queries are run.</span></span> <span data-ttu-id="12a23-254">Aby, aby se spouštěly musíte použít účet Spustit jako s oprávnění VIEW SERVER STATE do systému SQL.</span><span class="sxs-lookup"><span data-stu-id="12a23-254">In order for them to run, a Run As Account with VIEW SERVER STATE permissions to SQL must be used.</span></span>  <span data-ttu-id="12a23-255">Kromě toho aby bylo možné odeslat dotaz rozhraní WMI, jsou požadované přihlašovací údaje místního správce.</span><span class="sxs-lookup"><span data-stu-id="12a23-255">In addition, in order to query WMI, local administrator credentials are required.</span></span>

<span data-ttu-id="12a23-256">*Proč zobrazit pouze prvních 10 doporučení?*</span><span class="sxs-lookup"><span data-stu-id="12a23-256">*Why display only the top 10 recommendations?*</span></span>

* <span data-ttu-id="12a23-257">Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování seřazený podle priority doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-257">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="12a23-258">Po jejich řešení, bude k dispozici další doporučení.</span><span class="sxs-lookup"><span data-stu-id="12a23-258">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="12a23-259">Pokud ji chcete zobrazit podrobný seznam, můžete zobrazit všechna doporučení pomocí vyhledávání protokolu OMS.</span><span class="sxs-lookup"><span data-stu-id="12a23-259">If you prefer to see the detailed list, you can view all recommendations using the OMS log search.</span></span>

<span data-ttu-id="12a23-260">*Existuje způsob, jak ignorovat doporučení?*</span><span class="sxs-lookup"><span data-stu-id="12a23-260">*Is there a way to ignore a recommendation?*</span></span>

* <span data-ttu-id="12a23-261">Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.</span><span class="sxs-lookup"><span data-stu-id="12a23-261">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12a23-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12a23-262">Next steps</span></span>
* <span data-ttu-id="12a23-263">[V protokolech Hledat](log-analytics-log-searches.md) zobrazíte doporučení a podrobné údaje o vyhodnocení SQL.</span><span class="sxs-lookup"><span data-stu-id="12a23-263">[Search logs](log-analytics-log-searches.md) to view detailed SQL Assessment data and recommendations.</span></span>
