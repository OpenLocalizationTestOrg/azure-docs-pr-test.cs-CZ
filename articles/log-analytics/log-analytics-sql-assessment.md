---
title: "aaaOptimize prostředí systému SQL Server s nástrojem Azure Log Analytics | Microsoft Docs"
description: "S Azure Log Analytics můžete hello vyhodnocení SQL řešení tooassess hello rizika a stavu prostředí serveru SQL v pravidelných intervalech."
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
ms.openlocfilehash: f31326d8cdad3ef5d5a190614d1a18c1dac826ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-sql-server-environment-with-hello-sql-assessment-solution-in-log-analytics"></a><span data-ttu-id="b11ac-103">Optimalizace prostředí SQL serveru s hello řešení pro vyhodnocení SQL v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="b11ac-103">Optimize your SQL Server environment with hello SQL Assessment solution in Log Analytics</span></span>

![Symbol vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

<span data-ttu-id="b11ac-105">Můžete použít hello vyhodnocení SQL řešení tooassess hello stavu serveru prostředí a riziko v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="b11ac-105">You can use hello SQL Assessment solution tooassess hello risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="b11ac-106">Tento článek vám pomůže nainstalovat řešení hello, takže můžete provést nápravné akce pro potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="b11ac-106">This article will help you install hello solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="b11ac-107">Toto řešení poskytuje seznam doporučení konkrétní tooyour nasazená serverová infrastruktura seřazený podle priority.</span><span class="sxs-lookup"><span data-stu-id="b11ac-107">This solution provides a prioritized list of recommendations specific tooyour deployed server infrastructure.</span></span> <span data-ttu-id="b11ac-108">Hello doporučení jsou rozdělené mezi šesti fokus, které oblasti, které vám pomůžou rychle pochopit hello riziko a proveďte opravné akce.</span><span class="sxs-lookup"><span data-stu-id="b11ac-108">hello recommendations are categorized across six focus areas which help you quickly understand hello risk and take corrective action.</span></span>

<span data-ttu-id="b11ac-109">Hello doporučení, která jsou založené na hello znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách.</span><span class="sxs-lookup"><span data-stu-id="b11ac-109">hello recommendations made are based on hello knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="b11ac-110">Každé doporučení obsahuje informace, proč problému, může vás tooyou a jak tooimplement hello navrhované změny.</span><span class="sxs-lookup"><span data-stu-id="b11ac-110">Each recommendation provides guidance about why an issue might matter tooyou and how tooimplement hello suggested changes.</span></span>

<span data-ttu-id="b11ac-111">Můžete vybrat konkrétní oblasti, které jsou nejdůležitější tooyour organizace a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.</span><span class="sxs-lookup"><span data-stu-id="b11ac-111">You can choose focus areas that are most important tooyour organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="b11ac-112">Když jste přidali hello řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na hello **vyhodnocení SQL** řídicí panel pro hello infrastruktury ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="b11ac-112">After you've added hello solution and an assessment is completed, summary information for focus areas is shown on hello **SQL Assessment** dashboard for hello infrastructure in your environment.</span></span> <span data-ttu-id="b11ac-113">Hello následující části popisují, jak toouse hello informace o hello **vyhodnocení SQL** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="b11ac-113">hello following sections describe how toouse hello information on hello **SQL Assessment** dashboard, where you can view and then take recommended actions for your SQL server infrastructure.</span></span>

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="b11ac-116">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="b11ac-116">Installing and configuring hello solution</span></span>
<span data-ttu-id="b11ac-117">Vyhodnocení SQL pracuje s všechny aktuálně podporované verze systému SQL Server pro hello edice Standard, Developer a Enterprise.</span><span class="sxs-lookup"><span data-stu-id="b11ac-117">SQL Assessment works with all currently supported versions of SQL Server for hello Standard, Developer, and Enterprise editions.</span></span>

<span data-ttu-id="b11ac-118">Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-118">Use hello following information tooinstall and configure hello solution.</span></span>

* <span data-ttu-id="b11ac-119">Na serverech, které mají nainstalovaný server SQL Server je nutné nainstalovat agenty.</span><span class="sxs-lookup"><span data-stu-id="b11ac-119">Agents must be installed on servers that have SQL Server installed.</span></span>
* <span data-ttu-id="b11ac-120">Hello řešení pro vyhodnocení SQL vyžaduje podporovanou verzi rozhraní .NET Framework 4 nainstalován na každý počítač, který má OMS agent.</span><span class="sxs-lookup"><span data-stu-id="b11ac-120">hello SQL Assessment solution requires a supported version of .NET Framework 4 installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="b11ac-121">V pořadí tooinstall hello řešení hello uživatel musí být správce nebo Přispěvatel toohello předplatné Azure, při použití hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b11ac-121">In order tooinstall hello solution, hello user must be an administrator or contributor toohello Azure subscription when using hello Azure portal.</span></span> <span data-ttu-id="b11ac-122">Kromě toho hello uživatel musí být členem skupiny hello OMS prostoru Přispěvatel správce rolí nebo na portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-122">In addition, hello user must be a member of hello OMS workspace contributor or administrator role in hello OMS portal.</span></span>
* <span data-ttu-id="b11ac-123">Když pomocí agenta nástroje Operations Manager hello vyhodnocení SQL, budete potřebovat toouse účet Operations Manager Run-As.</span><span class="sxs-lookup"><span data-stu-id="b11ac-123">When using hello Operations Manager agent with SQL Assessment, you'll need toouse an Operations Manager Run-As account.</span></span> <span data-ttu-id="b11ac-124">V tématu [účty nástroje Operations Manager spustit jako pro OMS](#operations-manager-run-as-accounts-for-oms) níže Další informace.</span><span class="sxs-lookup"><span data-stu-id="b11ac-124">See [Operations Manager run-as accounts for OMS](#operations-manager-run-as-accounts-for-oms) below for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b11ac-125">Hello MMA agent nepodporuje účty Operations Manager Run-As.</span><span class="sxs-lookup"><span data-stu-id="b11ac-125">hello MMA agent does not support Operations Manager Run-As accounts.</span></span>
  >
  >
* <span data-ttu-id="b11ac-126">Přidat hello vyhodnocení SQL řešení tooyour pracovním prostorem OMS pomocí procesu hello popsané v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="b11ac-126">Add hello SQL Assessment solution tooyour OMS workspace using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="b11ac-127">Není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b11ac-127">There is no further configuration required.</span></span>

> [!NOTE]
> <span data-ttu-id="b11ac-128">Po přidání hello řešení, se přidá soubor AdvisorAssessment.exe hello tooservers s agenty.</span><span class="sxs-lookup"><span data-stu-id="b11ac-128">After you've added hello solution, hello AdvisorAssessment.exe file is added tooservers with agents.</span></span> <span data-ttu-id="b11ac-129">Konfigurační data je čtení a pak se odešle toohello OMS služba v cloudu hello ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="b11ac-129">Configuration data is read and then sent toohello OMS service in hello cloud for processing.</span></span> <span data-ttu-id="b11ac-130">Logika je použité toohello přijatých dat a hello Cloudová služba zaznamenává hello data.</span><span class="sxs-lookup"><span data-stu-id="b11ac-130">Logic is applied toohello received data and hello cloud service records hello data.</span></span>

## <a name="sql-assessment-data-collection-details"></a><span data-ttu-id="b11ac-131">Podrobnosti kolekce dat vyhodnocení SQL</span><span class="sxs-lookup"><span data-stu-id="b11ac-131">SQL Assessment data collection details</span></span>
<span data-ttu-id="b11ac-132">Vyhodnocení SQL shromažďuje data rozhraní WMI, registru dat, údaje o výkonu a výsledky zobrazení dynamické správy SQL Server pomocí hello agentů, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="b11ac-132">SQL Assessment collects WMI data, registry data, performance data, and SQL Server dynamic management view results using hello agents that you have enabled.</span></span>

<span data-ttu-id="b11ac-133">Hello následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.</span><span class="sxs-lookup"><span data-stu-id="b11ac-133">hello following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="b11ac-134">Platforma</span><span class="sxs-lookup"><span data-stu-id="b11ac-134">platform</span></span> | <span data-ttu-id="b11ac-135">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="b11ac-135">Direct Agent</span></span> | <span data-ttu-id="b11ac-136">Agenta nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="b11ac-136">SCOM agent</span></span> | <span data-ttu-id="b11ac-137">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b11ac-137">Azure Storage</span></span> | <span data-ttu-id="b11ac-138">SCOM vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="b11ac-138">SCOM required?</span></span> | <span data-ttu-id="b11ac-139">Data agenta SCOM odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="b11ac-139">SCOM agent data sent via management group</span></span> | <span data-ttu-id="b11ac-140">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="b11ac-140">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="b11ac-141">Windows</span><span class="sxs-lookup"><span data-stu-id="b11ac-141">Windows</span></span> | <span data-ttu-id="b11ac-142">&#8226;</span><span class="sxs-lookup"><span data-stu-id="b11ac-142">&#8226;</span></span> | <span data-ttu-id="b11ac-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="b11ac-143">&#8226;</span></span> |  |  | <span data-ttu-id="b11ac-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="b11ac-144">&#8226;</span></span> |<span data-ttu-id="b11ac-145">7 dní</span><span class="sxs-lookup"><span data-stu-id="b11ac-145">7 days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="b11ac-146">Účty spustit jako nástroje Operations Manager pro OMS</span><span class="sxs-lookup"><span data-stu-id="b11ac-146">Operations Manager run-as accounts for OMS</span></span>
<span data-ttu-id="b11ac-147">Analýzy protokolů v OMS používá hello agent nástroje Operations Manager a toocollect skupiny správy a posílat data toohello OMS služby.</span><span class="sxs-lookup"><span data-stu-id="b11ac-147">Log Analytics in OMS uses hello Operations Manager agent and management group toocollect and send data toohello OMS service.</span></span> <span data-ttu-id="b11ac-148">Sestavení OMS na sady management Pack pro úlohy tooprovide přidanou hodnotou services.</span><span class="sxs-lookup"><span data-stu-id="b11ac-148">OMS builds upon management packs for workloads tooprovide value-add services.</span></span> <span data-ttu-id="b11ac-149">Každé zatížení vyžaduje oprávnění konkrétních úloh toorun management Pack v odlišném kontextu zabezpečení, jako je například účet domény.</span><span class="sxs-lookup"><span data-stu-id="b11ac-149">Each workload requires workload-specific privileges toorun management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="b11ac-150">Potřebujete přihlašovací údaje tooprovide nakonfigurováním Operations Manager účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="b11ac-150">You need tooprovide credential information by configuring an Operations Manager Run As account.</span></span>

<span data-ttu-id="b11ac-151">Použijte následující informace tooset hello nástroje Operations Manager účet Spustit jako pro vyhodnocení SQL hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-151">Use hello following information tooset hello Operations Manager run-as account for SQL Assessment.</span></span>

### <a name="set-hello-run-as-account-for-sql-assessment"></a><span data-ttu-id="b11ac-152">Nastavit hello spustit jako účet pro vyhodnocení SQL</span><span class="sxs-lookup"><span data-stu-id="b11ac-152">Set hello Run As account for SQL assessment</span></span>
 <span data-ttu-id="b11ac-153">Pokud už používáte hello systému SQL Server management pack, používejte tento účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="b11ac-153">If you are already using hello SQL Server management pack, you should use that Run As account.</span></span>

#### <a name="tooconfigure-hello-sql-run-as-account-in-hello-operations-console"></a><span data-ttu-id="b11ac-154">tooconfigure hello účet Spustit jako SQL v hello Operations console</span><span class="sxs-lookup"><span data-stu-id="b11ac-154">tooconfigure hello SQL Run As account in hello Operations console</span></span>
> [!NOTE]
> <span data-ttu-id="b11ac-155">Pokud používáte přímé agenta, nikoli agenta nástroje SCOM hello zprostředkovatele hello OMS, sada management pack hello vždy běží v kontextu zabezpečení hello hello místního systémového účtu.</span><span class="sxs-lookup"><span data-stu-id="b11ac-155">If you are using hello OMS direct agent, rather than hello SCOM agent, hello management pack always runs in hello security context of hello Local System account.</span></span> <span data-ttu-id="b11ac-156">Přeskočit kroky 1 až 5 níže a spusťte buď hello T-SQL nebo prostředí Powershell ukázce zadání NT AUTHORITY\SYSTEM jako hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b11ac-156">Skip steps 1-5 below, and run either hello T-SQL or Powershell sample, specifying NT AUTHORITY\SYSTEM as hello user name.</span></span>
>
>

1. <span data-ttu-id="b11ac-157">V nástroji Operations Manager, otevřete hello Operations console a pak klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="b11ac-157">In Operations Manager, open hello Operations console, and then click **Administration**.</span></span>
2. <span data-ttu-id="b11ac-158">V části **konfigurace spustit jako**, klikněte na tlačítko **profily**a otevřete **OMS SQL Assessment profilu spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="b11ac-158">Under **Run As Configuration**, click **Profiles**, and open **OMS SQL Assessment Run As Profile**.</span></span>
3. <span data-ttu-id="b11ac-159">Na hello **účty spustit jako** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b11ac-159">On hello **Run As Accounts** page, click **Add**.</span></span>
4. <span data-ttu-id="b11ac-160">Vyberte účet Spustit jako systému Windows, který obsahuje hello přihlašovacích údajů nezbytných pro systém SQL Server, nebo klikněte na **nový** toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="b11ac-160">Select a Windows Run As account that contains hello credentials needed for SQL Server, or click **New** toocreate one.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b11ac-161">Hello typ účtu spustit jako musí být Windows.</span><span class="sxs-lookup"><span data-stu-id="b11ac-161">hello Run As account type must be Windows.</span></span> <span data-ttu-id="b11ac-162">Hello účet Spustit jako musí být také součástí místní skupiny Administrators na všech serverech Windows, který je hostitelem instance serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="b11ac-162">hello Run As account must also be part of Local Administrators group on all Windows Servers hosting SQL Server Instances.</span></span>
   >
   >
5. <span data-ttu-id="b11ac-163">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b11ac-163">Click **Save**.</span></span>
6. <span data-ttu-id="b11ac-164">Upravit a potom spusťte následující ukázka T-SQL v jednotlivých webu požadované tooRun pro instanci systému SQL Server toogrant minimální oprávnění jako účet tooperform vyhodnocení SQL hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-164">Modify and then execute hello following T-SQL sample on each SQL Server Instance toogrant minimum permissions required tooRun As Account tooperform SQL Assessment.</span></span> <span data-ttu-id="b11ac-165">Ale nepotřebujete toodo to pokud účet Spustit jako už je součástí role serveru sysadmin hello na instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b11ac-165">However, you don’t need toodo this if a Run As Account is already part of hello sysadmin server role on SQL Server Instances.</span></span>

```
---
    -- Replace <UserName> with hello actual user name being used as Run As Account.
    USE master

    -- Create login for hello user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions toouser.
    GRANT VIEW SERVER STATE too[<UserName>]
    GRANT VIEW ANY DEFINITION too[<UserName>]
    GRANT VIEW ANY DATABASE too[<UserName>]

    -- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
    -- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="tooconfigure-hello-sql-run-as-account-using-windows-powershell"></a><span data-ttu-id="b11ac-166">tooconfigure hello SQL spustit jako účtu pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="b11ac-166">tooconfigure hello SQL Run As account using Windows PowerShell</span></span>
<span data-ttu-id="b11ac-167">Otevřete okno prostředí PowerShell a spusťte následující skript po aktualizaci s vaše informace hello:</span><span class="sxs-lookup"><span data-stu-id="b11ac-167">Open a PowerShell window and run hello following script after you’ve updated it with your information:</span></span>

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="b11ac-168">Pochopení, jak budou doporučení mít vyšší prioritu</span><span class="sxs-lookup"><span data-stu-id="b11ac-168">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="b11ac-169">Každé doporučení je uveden vyvážení hodnotu, která identifikuje hello relativní důležitost hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-169">Every recommendation made is given a weighting value that identifies hello relative importance of hello recommendation.</span></span> <span data-ttu-id="b11ac-170">Jsou zobrazeny pouze hello deset nejdůležitějších doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-170">Only hello ten most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="b11ac-171">Jak jsou vypočítávány váhu</span><span class="sxs-lookup"><span data-stu-id="b11ac-171">How weights are calculated</span></span>
<span data-ttu-id="b11ac-172">Váhy jsou agregovaných hodnot založena na tři klíčové faktory:</span><span class="sxs-lookup"><span data-stu-id="b11ac-172">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="b11ac-173">Hello *pravděpodobnosti* způsobí, že problém identifikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="b11ac-173">hello *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="b11ac-174">Vyšší pravděpodobnost znamená zároveň tooa větší celkové skóre pro hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-174">A higher probability equates tooa larger overall score for hello recommendation.</span></span>
* <span data-ttu-id="b11ac-175">Hello *dopad* hello problému na vaší organizaci, pokud ji způsobovat problémy.</span><span class="sxs-lookup"><span data-stu-id="b11ac-175">hello *impact* of hello issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="b11ac-176">Vyšší dopad znamená zároveň tooa větší celkové skóre pro hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-176">A higher impact equates tooa larger overall score for hello recommendation.</span></span>
* <span data-ttu-id="b11ac-177">Hello *úsilí* požadované tooimplement hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-177">hello *effort* required tooimplement hello recommendation.</span></span> <span data-ttu-id="b11ac-178">Vyšší úsilí znamená zároveň tooa menší celkové skóre pro hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-178">A higher effort equates tooa smaller overall score for hello recommendation.</span></span>

<span data-ttu-id="b11ac-179">Hello vážení v případě každé doporučení je vyjádřený jako procentní podíl celkové skóre hello k dispozici pro každou oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="b11ac-179">hello weighting for each recommendation is expressed as a percentage of hello total score available for each focus area.</span></span> <span data-ttu-id="b11ac-180">Například pokud doporučení v hello zabezpečení a dodržování předpisů oblastí zájmu je skóre % 5, implementace tímto doporučením zvýší celkové skóre podle 5 % zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="b11ac-180">For example, if a recommendation in hello Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="b11ac-181">Konkrétní oblasti</span><span class="sxs-lookup"><span data-stu-id="b11ac-181">Focus areas</span></span>
<span data-ttu-id="b11ac-182">**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.</span><span class="sxs-lookup"><span data-stu-id="b11ac-182">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="b11ac-183">**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.</span><span class="sxs-lookup"><span data-stu-id="b11ac-183">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="b11ac-184">**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení toohelp vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a je možné toorespond toochanging musí se infrastruktura.</span><span class="sxs-lookup"><span data-stu-id="b11ac-184">**Performance and Scalability** - This focus area shows recommendations toohelp your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able toorespond toochanging infrastructure needs.</span></span>

<span data-ttu-id="b11ac-185">**Upgrade, nasazení a migrace** – upgradu toohelp doporučení v tomto poli fokus zobrazí, migrace a nasazení systému SQL Server tooyour stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="b11ac-185">**Upgrade, Migration and Deployment** - This focus area shows recommendations toohelp you upgrade, migrate, and deploy SQL Server tooyour existing infrastructure.</span></span>

<span data-ttu-id="b11ac-186">**Operace a monitorování** – v tomto poli fokus zobrazí doporučení toohelp zjednodušit vaše IT oddělení, implementovat preventivní údržby a maximalizovat výkon.</span><span class="sxs-lookup"><span data-stu-id="b11ac-186">**Operations and Monitoring** - This focus area shows recommendations toohelp streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

<span data-ttu-id="b11ac-187">**Správa změn a konfigurací** -doporučení v tomto poli fokus zobrazí toohelp chránit každodenních operací, zajistěte, aby změny nemáte negativně ovlivnit vaši infrastrukturu, vytvořte procedury řízení změn a tootrack a auditování Konfigurace systému.</span><span class="sxs-lookup"><span data-stu-id="b11ac-187">**Change and Configuration Management** - This focus area shows recommendations toohelp protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and tootrack and audit system configurations.</span></span>

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a><span data-ttu-id="b11ac-188">By odpovídajícím způsobem tooscore 100 % v každé oblasti fokus?</span><span class="sxs-lookup"><span data-stu-id="b11ac-188">Should you aim tooscore 100% in every focus area?</span></span>
<span data-ttu-id="b11ac-189">Ne nutně.</span><span class="sxs-lookup"><span data-stu-id="b11ac-189">Not necessarily.</span></span> <span data-ttu-id="b11ac-190">Hello doporučení jsou založená na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a hello znalostní báze.</span><span class="sxs-lookup"><span data-stu-id="b11ac-190">hello recommendations are based on hello knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="b11ac-191">Žádné serverové infrastruktury jsou však stejné hello a konkrétní doporučení může být vyšší nebo nižší relevantní tooyou.</span><span class="sxs-lookup"><span data-stu-id="b11ac-191">However, no two server infrastructures are hello same, and specific recommendations may be more or less relevant tooyou.</span></span> <span data-ttu-id="b11ac-192">Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou zveřejněné toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="b11ac-192">For example, some security recommendations might be less relevant if your virtual machines are not exposed toohello Internet.</span></span> <span data-ttu-id="b11ac-193">Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="b11ac-193">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="b11ac-194">Problémy, které jsou důležité tooa vyspělá firmy může být méně důležité tooa spuštění.</span><span class="sxs-lookup"><span data-stu-id="b11ac-194">Issues that are important tooa mature business may be less important tooa start-up.</span></span> <span data-ttu-id="b11ac-195">Možná chcete tooidentify, které konkrétní oblasti jsou vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.</span><span class="sxs-lookup"><span data-stu-id="b11ac-195">You may want tooidentify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="b11ac-196">Každé doporučení obsahuje pokyny o tom, proč je důležité.</span><span class="sxs-lookup"><span data-stu-id="b11ac-196">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="b11ac-197">Jestli implementace hello doporučení je vhodné pro vás, daná hello povaha IT služeb a hello obchodních potřeb vaší organizace, měli byste použít tento tooevaluate pokyny.</span><span class="sxs-lookup"><span data-stu-id="b11ac-197">You should use this guidance tooevaluate whether implementing hello recommendation is appropriate for you, given hello nature of your IT services and hello business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="b11ac-198">Použít assessment fokus oblasti doporučení</span><span class="sxs-lookup"><span data-stu-id="b11ac-198">Use assessment focus area recommendations</span></span>
<span data-ttu-id="b11ac-199">Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-199">Before you can use an assessment solution in OMS, you must have hello solution installed.</span></span> <span data-ttu-id="b11ac-200">tooread Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="b11ac-200">tooread more about installing solutions, see [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="b11ac-201">Po instalaci, zobrazí se souhrn hello doporučení pomocí dlaždice vyhodnocení SQL hello na stránce Přehled hello v OMS.</span><span class="sxs-lookup"><span data-stu-id="b11ac-201">After it is installed, you can view hello summary of recommendations by using hello SQL Assessment tile on hello Overview page in OMS.</span></span>

<span data-ttu-id="b11ac-202">Hello zobrazení souhrnu vyhodnocování dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-202">View hello summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="b11ac-203">tooview doporučení pro výběr oblasti a proveďte opravnou akci</span><span class="sxs-lookup"><span data-stu-id="b11ac-203">tooview recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="b11ac-204">Na hello **přehled** klikněte na tlačítko hello **vyhodnocení SQL** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="b11ac-204">On hello **Overview** page, click hello **SQL Assessment** tile.</span></span>
2. <span data-ttu-id="b11ac-205">Na hello **vyhodnocení SQL** zkontrolujte hello souhrnné informace v jednom z hello fokus oblast okna a pak klikněte na jednu tooview doporučení pro tuto oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="b11ac-205">On hello **SQL Assessment** page, review hello summary information in one of hello focus area blades and then click one tooview recommendations for that focus area.</span></span>
3. <span data-ttu-id="b11ac-206">Na žádném z hello fokus oblast stránky můžete zobrazit hello nastavovat doporučení, která pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="b11ac-206">On any of hello focus area pages, you can view hello prioritized recommendations made for your environment.</span></span> <span data-ttu-id="b11ac-207">Klikněte na tlačítko doporučení v části **vliv na objekty** tooview podrobnosti, proč se provádí doporučení hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-207">Click a recommendation under **Affected Objects** tooview details about why hello recommendation is made.</span></span>  
    <span data-ttu-id="b11ac-208">![Obrázek doporučení vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span><span class="sxs-lookup"><span data-stu-id="b11ac-208">![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span></span>
4. <span data-ttu-id="b11ac-209">Můžete provést nápravné akce navržený v **doporučované akce**.</span><span class="sxs-lookup"><span data-stu-id="b11ac-209">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="b11ac-210">Při hello položky vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="b11ac-210">When hello item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="b11ac-211">Opravené položky se zobrazí jako **předán objekty**.</span><span class="sxs-lookup"><span data-stu-id="b11ac-211">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="b11ac-212">Ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="b11ac-212">Ignore recommendations</span></span>
<span data-ttu-id="b11ac-213">Pokud máte doporučení, které chcete tooignore, můžete vytvořit textový soubor, který OMS použije tooprevent doporučení ze storu ve výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-213">If you have recommendations that you want tooignore, you can create a text file that OMS will use tooprevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-will-ignore"></a><span data-ttu-id="b11ac-214">tooidentify doporučení, které se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="b11ac-214">tooidentify recommendations that you will ignore</span></span>
1. <span data-ttu-id="b11ac-215">Přihlaste se tooyour prostoru a otevřete vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="b11ac-215">Sign in tooyour workspace and open Log Search.</span></span> <span data-ttu-id="b11ac-216">Použití hello následující dotaz toolist doporučení, které selhaly pro počítače ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="b11ac-216">Use hello following query toolist recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   <span data-ttu-id="b11ac-217">Zde je snímek obrazovky znázorňující hello protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="b11ac-217">Here's a screen shot showing hello Log Search query: ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span></span>
2. <span data-ttu-id="b11ac-218">Zvolte doporučení, které chcete tooignore.</span><span class="sxs-lookup"><span data-stu-id="b11ac-218">Choose recommendations that you want tooignore.</span></span> <span data-ttu-id="b11ac-219">Hodnoty hello budete používat pro RecommendationId v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-219">You’ll use hello values for RecommendationId in hello next procedure.</span></span>

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="b11ac-220">toocreate a použít textový soubor s IgnoreRecommendations.txt</span><span class="sxs-lookup"><span data-stu-id="b11ac-220">toocreate and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="b11ac-221">Vytvořte soubor s názvem IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="b11ac-221">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="b11ac-222">Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení má tooignore OMS na samostatném řádku a potom uložte a zavřete soubor hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-222">Paste or type each RecommendationId for each recommendation that you want OMS tooignore on a separate line and then save and close hello file.</span></span>
3. <span data-ttu-id="b11ac-223">Uložte soubor hello do následující složky na každém počítači, kam chcete OMS tooignore doporučení hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-223">Put hello file in hello following folder on each computer where you want OMS tooignore recommendations.</span></span>
   * <span data-ttu-id="b11ac-224">Na počítačích s hello agenta Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování</span><span class="sxs-lookup"><span data-stu-id="b11ac-224">On computers with hello Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="b11ac-225">Na serveru pro správu nástroje Operations Manager hello - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span><span class="sxs-lookup"><span data-stu-id="b11ac-225">On hello Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="tooverify-that-recommendations-are-ignored"></a><span data-ttu-id="b11ac-226">tooverify, že se ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="b11ac-226">tooverify that recommendations are ignored</span></span>
1. <span data-ttu-id="b11ac-227">Po spuštění hello další naplánované vyhodnocení ve výchozím nastavení každých 7 dní hello zadané doporučení jsou označeny ignorovaná a nezobrazí se na řídicí panel assessment hello.</span><span class="sxs-lookup"><span data-stu-id="b11ac-227">After hello next scheduled assessment runs, by default every 7 days, hello specified recommendations are marked Ignored and will not appear on hello assessment dashboard.</span></span>
2. <span data-ttu-id="b11ac-228">Hello následující dotazy toolist hledání protokolů můžete použít všechny hello ignorovat doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-228">You can use hello following Log Search queries toolist all hello ignored recommendations.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. <span data-ttu-id="b11ac-229">Pokud se později rozhodnete, že chcete toosee ignorovat doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.</span><span class="sxs-lookup"><span data-stu-id="b11ac-229">If you decide later that you want toosee ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="sql-assessment-solution-faq"></a><span data-ttu-id="b11ac-230">Řešení pro vyhodnocení SQL – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="b11ac-230">SQL Assessment solution FAQ</span></span>
<span data-ttu-id="b11ac-231">*Jak často posouzení spustit?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-231">*How often does an assessment run?*</span></span>

* <span data-ttu-id="b11ac-232">hodnocení Hello spouští každých 7 dní.</span><span class="sxs-lookup"><span data-stu-id="b11ac-232">hello assessment runs every 7 days.</span></span>

<span data-ttu-id="b11ac-233">*Je k dispozici způsob tooconfigure jak často hello assessment běží?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-233">*Is there a way tooconfigure how often hello assessment runs?*</span></span>

* <span data-ttu-id="b11ac-234">V tuto chvíli to není možné.</span><span class="sxs-lookup"><span data-stu-id="b11ac-234">Not at this time.</span></span>

<span data-ttu-id="b11ac-235">*Pokud je zjištěno jiný server, poté, co byly přidány hello řešení pro vyhodnocení SQL, bude ho vyhodnocena?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-235">*If another server is discovered after I’ve added hello SQL assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="b11ac-236">Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.</span><span class="sxs-lookup"><span data-stu-id="b11ac-236">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="b11ac-237">*Pokud dojde k deaktivaci serveru, když ho se odebere z hello assessment?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-237">*If a server is decommissioned, when will it be removed from hello assessment?*</span></span>

* <span data-ttu-id="b11ac-238">Pokud server není odesílání dat 3 týdny, bude odebrán.</span><span class="sxs-lookup"><span data-stu-id="b11ac-238">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="b11ac-239">*Jaké je jméno hello hello procesu, který hello shromažďování dat?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-239">*What is hello name of hello process that does hello data collection?*</span></span>

* <span data-ttu-id="b11ac-240">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="b11ac-240">AdvisorAssessment.exe</span></span>

<span data-ttu-id="b11ac-241">*Jak dlouho trvá toobe dat shromážděných?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-241">*How long does it take for data toobe collected?*</span></span>

* <span data-ttu-id="b11ac-242">kolekce Hello skutečná data na serveru hello trvá asi 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="b11ac-242">hello actual data collection on hello server takes about 1 hour.</span></span> <span data-ttu-id="b11ac-243">Může trvat déle na serverech, které mají velký počet instancí SQL nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="b11ac-243">It may take longer on servers that have a large number of SQL instances or databases.</span></span>

<span data-ttu-id="b11ac-244">*Jaký typ dat shromažďovaných?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-244">*What type of data is collected?*</span></span>

* <span data-ttu-id="b11ac-245">se shromažďují Hello následující typy dat:</span><span class="sxs-lookup"><span data-stu-id="b11ac-245">hello following types of data are collected:</span></span>
  * <span data-ttu-id="b11ac-246">ROZHRANÍ WMI</span><span class="sxs-lookup"><span data-stu-id="b11ac-246">WMI</span></span>
  * <span data-ttu-id="b11ac-247">Registru</span><span class="sxs-lookup"><span data-stu-id="b11ac-247">Registry</span></span>
  * <span data-ttu-id="b11ac-248">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="b11ac-248">Performance counters</span></span>
  * <span data-ttu-id="b11ac-249">Zobrazení dynamické správy SQL (DMV).</span><span class="sxs-lookup"><span data-stu-id="b11ac-249">SQL dynamic management views (DMV).</span></span>

<span data-ttu-id="b11ac-250">*Existuje způsob, jak tooconfigure když jsou shromažďována data?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-250">*Is there a way tooconfigure when data is collected?*</span></span>

* <span data-ttu-id="b11ac-251">V tuto chvíli to není možné.</span><span class="sxs-lookup"><span data-stu-id="b11ac-251">Not at this time.</span></span>

<span data-ttu-id="b11ac-252">*Proč musím tooconfigure účet Spustit jako?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-252">*Why do I have tooconfigure a Run As Account?*</span></span>

* <span data-ttu-id="b11ac-253">Pro systém SQL Server se spouštějí na malý počet dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="b11ac-253">For SQL Server, a small number of SQL queries are run.</span></span> <span data-ttu-id="b11ac-254">Aby se musí použít toorun, účet Spustit jako s tooSQL oprávnění VIEW SERVER STATE.</span><span class="sxs-lookup"><span data-stu-id="b11ac-254">In order for them toorun, a Run As Account with VIEW SERVER STATE permissions tooSQL must be used.</span></span>  <span data-ttu-id="b11ac-255">Kromě toho v pořadí tooquery rozhraní WMI, jsou vyžadovány přihlašovací údaje místního správce.</span><span class="sxs-lookup"><span data-stu-id="b11ac-255">In addition, in order tooquery WMI, local administrator credentials are required.</span></span>

<span data-ttu-id="b11ac-256">*Proč zobrazit pouze prvních 10 doporučení hello?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-256">*Why display only hello top 10 recommendations?*</span></span>

* <span data-ttu-id="b11ac-257">Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování hello nastavovat doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-257">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing hello prioritized recommendations first.</span></span> <span data-ttu-id="b11ac-258">Po jejich řešení, bude k dispozici další doporučení.</span><span class="sxs-lookup"><span data-stu-id="b11ac-258">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="b11ac-259">Pokud dáváte přednost toosee hello podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů hello OMS.</span><span class="sxs-lookup"><span data-stu-id="b11ac-259">If you prefer toosee hello detailed list, you can view all recommendations using hello OMS log search.</span></span>

<span data-ttu-id="b11ac-260">*Existuje způsob, jak tooignore doporučení?*</span><span class="sxs-lookup"><span data-stu-id="b11ac-260">*Is there a way tooignore a recommendation?*</span></span>

* <span data-ttu-id="b11ac-261">Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.</span><span class="sxs-lookup"><span data-stu-id="b11ac-261">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b11ac-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b11ac-262">Next steps</span></span>
* <span data-ttu-id="b11ac-263">[V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné doporučení a údaje o vyhodnocení SQL.</span><span class="sxs-lookup"><span data-stu-id="b11ac-263">[Search logs](log-analytics-log-searches.md) tooview detailed SQL Assessment data and recommendations.</span></span>
