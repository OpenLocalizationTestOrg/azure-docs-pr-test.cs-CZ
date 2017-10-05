---
title: "Detekce – Azure SQL Database hrozby | Microsoft Docs"
description: "Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální ohrožení databáze."
services: sql-database
documentationcenter: 
author: rmatchoro
manager: jhubbard
editor: v-romcal
ms.assetid: b50d232a-4225-46ed-91e7-75288f55ee84
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/19/2017
ms.author: ronmat; ronitr
ms.openlocfilehash: bd3de9ed0131edc683763b0fe7f4a2ae74533944
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sql-database-threat-detection"></a><span data-ttu-id="62d92-103">Detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="62d92-103">SQL Database Threat Detection</span></span>

<span data-ttu-id="62d92-104">SQL detekce hrozeb zjistila neobvyklé aktivity, které indikují neobvyklou a potenciálně škodlivé pokusy o přístup k nebo zneužití databáze.</span><span class="sxs-lookup"><span data-stu-id="62d92-104">SQL Threat Detection detects anomalous activities indicating unusual and potentially harmful attempts to access or exploit databases.</span></span>

## <a name="overview"></a><span data-ttu-id="62d92-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="62d92-105">Overview</span></span>

<span data-ttu-id="62d92-106">Detekce hrozeb SQL poskytuje novou vrstvu zabezpečení, která uživatelům umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="62d92-106">SQL Threat Detection provides a new layer of security, which enables customers to detect and respond to potential threats as they occur by providing security alerts on anomalous activities.</span></span>  <span data-ttu-id="62d92-107">Uživatelé obdrží výstrahu při databáze podezřelé aktivity, potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL, jakož i nezvyklé databázové přístupové vzorce.</span><span class="sxs-lookup"><span data-stu-id="62d92-107">Users will receive an alert upon suspicious database activities, potential vulnerabilities, and SQL injection attacks, as well as anomalous database access patterns.</span></span> <span data-ttu-id="62d92-108">Detekce hrozeb SQL výstrahy zadejte podrobnosti podezřelých aktivit a doporučujeme akce o tom, jak prozkoumat a zmírnit riziko.</span><span class="sxs-lookup"><span data-stu-id="62d92-108">SQL Threat Detection alerts provide details of suspicious activity and recommend action on how to investigate and mitigate the threat.</span></span> <span data-ttu-id="62d92-109">Uživatele můžete prozkoumat podezřelé události pomocí [auditování databáze SQL](sql-database-auditing.md) k určení, pokud vyplývají z pokus o přístup, porušení nebo využívat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="62d92-109">Users can explore the suspicious events using [SQL Database Auditing](sql-database-auditing.md) to determine if they result from an attempt to access, breach, or exploit data in the database.</span></span> <span data-ttu-id="62d92-110">Detekce hrozeb zjednodušuje na potenciální hrozby adres do databáze bez nutnosti odborné zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="62d92-110">Threat Detection makes it simple to address potential threats to the database without the need to be a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="62d92-111">Například Injektáž SQL je jedním z běžné problémy zabezpečení webových aplikací na Internetu, slouží k útokům na základě dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="62d92-111">For example, SQL injection is one of the common Web application security issues on the Internet, used to attack data-driven applications.</span></span> <span data-ttu-id="62d92-112">Útočníci využít výhod ohrožení zabezpečení aplikace se zlými úmysly příkazy SQL, do pole pro zadání aplikací, vložit před nedodržením nebo upravovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="62d92-112">Attackers take advantage of application vulnerabilities to inject malicious SQL statements into application entry fields, breaching or modifying data in the database.</span></span>

<span data-ttu-id="62d92-113">Detekce hrozeb SQL integruje výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), a každé chráněné databáze SQL server se bude účtovat podle za stejnou cenu jako Azure Security Center standardní vrstvy v $15 uzlu/měsíc, kde každé chráněné databáze SQL serveru se počítá jako jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="62d92-113">SQL Threat Detection integrates alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), and, each protected SQL Database server will be billed at the same price as Azure Security Center Standard tier, at $15/node/month, where each protected SQL Database server is counted as one node.</span></span> <span data-ttu-id="62d92-114">Zveme vás k bezplatnému vyzkoušení po dobu 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="62d92-114">We invite you to try it out for 60 days for free.</span></span> 

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a><span data-ttu-id="62d92-115">Nastavení detekce hrozeb pro vaši databázi na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="62d92-115">Set up threat detection for your database in the Azure portal</span></span>
1. <span data-ttu-id="62d92-116">Spuštění portálu Azure v [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62d92-116">Launch the Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="62d92-117">Přejděte do okna konfigurace databáze SQL, kterou chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="62d92-117">Navigate to the configuration blade of the SQL Database you want to monitor.</span></span> <span data-ttu-id="62d92-118">V okně Nastavení vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="62d92-118">In the Settings blade, select **Auditing & Threat Detection**.</span></span> 
    <span data-ttu-id="62d92-119">![Navigační podokno][1]</span><span class="sxs-lookup"><span data-stu-id="62d92-119">![Navigation pane][1]</span></span>
3. <span data-ttu-id="62d92-120">V **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="62d92-120">In the **Auditing & Threat Detection** configuration blade turn **ON** Auditing, which will display the threat detection settings.</span></span>
  
    ![Navigační podokno][2]
4. <span data-ttu-id="62d92-122">Zapnout **ON** detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="62d92-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="62d92-123">Konfigurace seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="62d92-123">Configure the list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>
6. <span data-ttu-id="62d92-124">Klikněte na tlačítko **Uložit** v **auditování a detekce hrozeb** okno a uložte nové nebo aktualizované auditování a hrozeb nastavením detekce.</span><span class="sxs-lookup"><span data-stu-id="62d92-124">Click **Save** in the **Auditing & Threat detection** blade to save the new or updated auditing and threat detection settings.</span></span>
       
    ![Navigační podokno][3]

## <a name="set-up-threat-detection-using-powershell"></a><span data-ttu-id="62d92-126">Nastavení detekce hrozeb pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="62d92-126">Set up threat detection using PowerShell</span></span>

<span data-ttu-id="62d92-127">Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="62d92-127">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="62d92-128">Prozkoumejte nezvyklé databázové aktivity, při zjištění podezřelé události</span><span class="sxs-lookup"><span data-stu-id="62d92-128">Explore anomalous database activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="62d92-129">Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="62d92-129">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="62d92-130">E-mailu bude poskytují informace o události podezřelé zabezpečení, včetně povahu neobvyklé aktivity, název databáze, název serveru, název aplikace a čas události.</span><span class="sxs-lookup"><span data-stu-id="62d92-130">The email will provide information on the suspicious security event including the nature of the anomalous activities, database name, server name, application name, and the event time.</span></span> <span data-ttu-id="62d92-131">Kromě toho e-mailu bude poskytovat informace na možné příčiny a doporučené akce ke zkoumání a zmírnit potenciální hrozbu do databáze.</span><span class="sxs-lookup"><span data-stu-id="62d92-131">In addition, the email will provide information on possible causes and recommended actions to investigate and mitigate the potential threat to the database.</span></span><br/>
     
    ![Navigační podokno][4]
2. <span data-ttu-id="62d92-133">E-mailové upozornění obsahuje přímý odkaz na protokol auditování SQL.</span><span class="sxs-lookup"><span data-stu-id="62d92-133">The email alert includes a direct link to the SQL Audit log.</span></span> <span data-ttu-id="62d92-134">Kliknutím na tento odkaz spuštění portálu Azure a otevře se záznamy auditu SQL v době podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="62d92-134">Clicking on this link launches the Azure portal and opens the SQL Audit records around the time of the suspicious event.</span></span> <span data-ttu-id="62d92-135">Klikněte na záznam auditu zobrazíte další podrobnosti o činnostech podezřelé databáze, bylo snazší najít příkazy SQL, které byly provedeny (který přístup, co se a kdy) a zjistěte, zda událost legitimní nebo škodlivý (například byl zneužití ohrožení zabezpečení aplikace k Injektáž SQL, někdo nedodržení citlivá data, atd.).</span><span class="sxs-lookup"><span data-stu-id="62d92-135">Click on an audit record to view more details on the suspicious database activities, making it easier to find the SQL statements that were executed (who accessed, what they did and when) and determine if the event was legitimate or malicious (e.g. application vulnerability to SQL injection was exploited, someone breached sensitive data, etc.).</span></span><br/><span data-ttu-id="62d92-136">
   ![Navigační podokno][5]</span><span class="sxs-lookup"><span data-stu-id="62d92-136">
![Navigation pane][5]</span></span>


## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a><span data-ttu-id="62d92-137">Seznamte se výstrah o zjištěných hrozbách pro vaši databázi na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="62d92-137">Explore threat detection alerts for your database in the Azure portal</span></span>

<span data-ttu-id="62d92-138">Detekce hrozeb databáze SQL se integruje se jeho výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span><span class="sxs-lookup"><span data-stu-id="62d92-138">SQL Database Threat Detection integrates its alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span></span> <span data-ttu-id="62d92-139">Živé dlaždice zabezpečení SQL v okně databáze na portálu Azure sleduje stav active hrozeb.</span><span class="sxs-lookup"><span data-stu-id="62d92-139">A live SQL security tile within the database blade in the Azure portal tracks the status of active threats.</span></span> 

   ![Navigační podokno][6]
   
1. <span data-ttu-id="62d92-141">Kliknutím na SQL dlaždice zabezpečení spustí okno výstrahy Azure Security Center a nabízí přehled active SQL hrozby zjištěné v databázi.</span><span class="sxs-lookup"><span data-stu-id="62d92-141">Clicking on the SQL security tile launches the Azure Security Center alerts blade and provides an overview of active SQL threats detected on the database.</span></span> 

  ![Navigační podokno][7]

2. <span data-ttu-id="62d92-143">Kliknutím na konkrétní výstrahu nabízí další podrobnosti a akcí pro příčin této hrozby a nápravě budoucí hrozeb.</span><span class="sxs-lookup"><span data-stu-id="62d92-143">Clicking on a specific alert provides additional details and actions for investigating this threat and remediating future threats.</span></span>

  ![Navigační podokno][8]


## <a name="next-steps"></a><span data-ttu-id="62d92-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62d92-145">Next steps</span></span>

* <span data-ttu-id="62d92-146">Další informace o detekce hrozeb najdete [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span><span class="sxs-lookup"><span data-stu-id="62d92-146">Learn more about Threat Detection, visit the [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span></span> 
* <span data-ttu-id="62d92-147">Další informace o [auditování databáze SQL Azure](sql-database-auditing.md)</span><span class="sxs-lookup"><span data-stu-id="62d92-147">Learn more about [Azure SQL Database Auditing](sql-database-auditing.md)</span></span>
* <span data-ttu-id="62d92-148">Další informace o [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span><span class="sxs-lookup"><span data-stu-id="62d92-148">Learn more about [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span></span>
* <span data-ttu-id="62d92-149">Další podrobnosti o cenách najdete [stránky SQL Database – ceny](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span><span class="sxs-lookup"><span data-stu-id="62d92-149">For more details on pricing, please see the [SQL Database Pricing page](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span></span>  
* <span data-ttu-id="62d92-150">Příklad skriptu prostředí PowerShell, najdete v části [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="62d92-150">For a PowerShell script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span></span>



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


