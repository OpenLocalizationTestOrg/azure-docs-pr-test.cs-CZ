---
title: "aaaThreat detekce – Azure SQL Database | Microsoft Docs"
description: "Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze."
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
ms.openlocfilehash: 0879d20eff515a4e69358b5a98ceccf57fbd0ea2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-threat-detection"></a><span data-ttu-id="82c9e-103">Detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="82c9e-103">SQL Database Threat Detection</span></span>

<span data-ttu-id="82c9e-104">SQL detekce hrozeb zjistila neobvyklé aktivity, které indikují pokusy o neobvyklou a potenciálně škodlivé tooaccess nebo zneužití databází.</span><span class="sxs-lookup"><span data-stu-id="82c9e-104">SQL Threat Detection detects anomalous activities indicating unusual and potentially harmful attempts tooaccess or exploit databases.</span></span>

## <a name="overview"></a><span data-ttu-id="82c9e-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="82c9e-105">Overview</span></span>

<span data-ttu-id="82c9e-106">Detekce hrozeb SQL poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="82c9e-106">SQL Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span>  <span data-ttu-id="82c9e-107">Uživatelé obdrží výstrahu při databáze podezřelé aktivity, potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL, jakož i nezvyklé databázové přístupové vzorce.</span><span class="sxs-lookup"><span data-stu-id="82c9e-107">Users will receive an alert upon suspicious database activities, potential vulnerabilities, and SQL injection attacks, as well as anomalous database access patterns.</span></span> <span data-ttu-id="82c9e-108">Detekce hrozeb SQL výstrahy zadejte podrobnosti podezřelých aktivit a doporučujeme akce jak tooinvestigate a zmírnit hrozby hello.</span><span class="sxs-lookup"><span data-stu-id="82c9e-108">SQL Threat Detection alerts provide details of suspicious activity and recommend action on how tooinvestigate and mitigate hello threat.</span></span> <span data-ttu-id="82c9e-109">Uživatele můžete prozkoumat hello podezřelé události pomocí [auditování databáze SQL](sql-database-auditing.md) toodetermine pokud vyplývají z tooaccess pokusu o porušení nebo zneužití data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="82c9e-109">Users can explore hello suspicious events using [SQL Database Auditing](sql-database-auditing.md) toodetermine if they result from an attempt tooaccess, breach, or exploit data in hello database.</span></span> <span data-ttu-id="82c9e-110">Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello databáze bez hello nutné toobe expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="82c9e-110">Threat Detection makes it simple tooaddress potential threats toohello database without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="82c9e-111">Například Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="82c9e-111">For example, SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="82c9e-112">Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="82c9e-112">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, breaching or modifying data in hello database.</span></span>

<span data-ttu-id="82c9e-113">Detekce hrozeb SQL integruje výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), a každé chráněné databáze SQL server se bude účtovat podle hello stejné cena jako Azure Security Center standardní vrstvy v $15 uzlu/měsíc, kde každé chráněné SQL Databázový server se počítá jako jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="82c9e-113">SQL Threat Detection integrates alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), and, each protected SQL Database server will be billed at hello same price as Azure Security Center Standard tier, at $15/node/month, where each protected SQL Database server is counted as one node.</span></span> <span data-ttu-id="82c9e-114">Doporučujeme vám tootry ho za 60 dnů pro uvolnění.</span><span class="sxs-lookup"><span data-stu-id="82c9e-114">We invite you tootry it out for 60 days for free.</span></span> 

## <a name="set-up-threat-detection-for-your-database-in-hello-azure-portal"></a><span data-ttu-id="82c9e-115">Nastavení detekce hrozeb pro vaše databáze v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82c9e-115">Set up threat detection for your database in hello Azure portal</span></span>
1. <span data-ttu-id="82c9e-116">Spusťte hello Azure portálu na [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82c9e-116">Launch hello Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="82c9e-117">Přejděte okno Konfigurace toohello hello chcete toomonitor databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="82c9e-117">Navigate toohello configuration blade of hello SQL Database you want toomonitor.</span></span> <span data-ttu-id="82c9e-118">V okně Nastavení hello, vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="82c9e-118">In hello Settings blade, select **Auditing & Threat Detection**.</span></span> 
    <span data-ttu-id="82c9e-119">![Navigační podokno][1]</span><span class="sxs-lookup"><span data-stu-id="82c9e-119">![Navigation pane][1]</span></span>
3. <span data-ttu-id="82c9e-120">V hello **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby hello.</span><span class="sxs-lookup"><span data-stu-id="82c9e-120">In hello **Auditing & Threat Detection** configuration blade turn **ON** Auditing, which will display hello threat detection settings.</span></span>
  
    ![Navigační podokno][2]
4. <span data-ttu-id="82c9e-122">Zapnout **ON** detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="82c9e-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="82c9e-123">Konfigurace seznamu hello e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="82c9e-123">Configure hello list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>
6. <span data-ttu-id="82c9e-124">Klikněte na tlačítko **Uložit** v hello **auditování a detekce hrozeb** okno toosave hello nové nebo aktualizované auditování a hrozeb detekce nastavení.</span><span class="sxs-lookup"><span data-stu-id="82c9e-124">Click **Save** in hello **Auditing & Threat detection** blade toosave hello new or updated auditing and threat detection settings.</span></span>
       
    ![Navigační podokno][3]

## <a name="set-up-threat-detection-using-powershell"></a><span data-ttu-id="82c9e-126">Nastavení detekce hrozeb pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="82c9e-126">Set up threat detection using PowerShell</span></span>

<span data-ttu-id="82c9e-127">Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="82c9e-127">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="82c9e-128">Prozkoumejte nezvyklé databázové aktivity, při zjištění podezřelé události</span><span class="sxs-lookup"><span data-stu-id="82c9e-128">Explore anomalous database activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="82c9e-129">Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="82c9e-129">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="82c9e-130">e-mailu Hello se poskytují informace o události hello podezřelé zabezpečení včetně hello povaha hello neobvyklé aktivity, název databáze, název serveru, název aplikace a čas události hello.</span><span class="sxs-lookup"><span data-stu-id="82c9e-130">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name, application name, and hello event time.</span></span> <span data-ttu-id="82c9e-131">Kromě toho hello e-mailu bude poskytovat informace na možné příčiny a doporučené akce tooinvestigate a zmírnit hello potenciální hrozby toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="82c9e-131">In addition, hello email will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span><br/>
     
    ![Navigační podokno][4]
2. <span data-ttu-id="82c9e-133">e-mailové výstrahy Hello zahrnuje protokol auditování SQL toohello přímý odkaz.</span><span class="sxs-lookup"><span data-stu-id="82c9e-133">hello email alert includes a direct link toohello SQL Audit log.</span></span> <span data-ttu-id="82c9e-134">Kliknutím na tento odkaz spustí hello Azure portal a otevře se okno hello SQL záznamy auditu době hello hello podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="82c9e-134">Clicking on this link launches hello Azure portal and opens hello SQL Audit records around hello time of hello suspicious event.</span></span> <span data-ttu-id="82c9e-135">Klikněte na tooview záznamů auditu další podrobnosti o hello databáze podezřelé aktivity, takže je jednodušší toofind hello SQL příkazy, které byly provedeny (který přístup, co se a kdy) a zjistěte, zda text hello událostí legitimní nebo škodlivý (například aplikace bylo by vkládání tooSQL ohrožení zabezpečení, někdo nedodržení citlivá data, atd.).</span><span class="sxs-lookup"><span data-stu-id="82c9e-135">Click on an audit record tooview more details on hello suspicious database activities, making it easier toofind hello SQL statements that were executed (who accessed, what they did and when) and determine if hello event was legitimate or malicious (e.g. application vulnerability tooSQL injection was exploited, someone breached sensitive data, etc.).</span></span><br/><span data-ttu-id="82c9e-136">
   ![Navigační podokno][5]</span><span class="sxs-lookup"><span data-stu-id="82c9e-136">
![Navigation pane][5]</span></span>


## <a name="explore-threat-detection-alerts-for-your-database-in-hello-azure-portal"></a><span data-ttu-id="82c9e-137">Seznamte se výstrah o zjištěných hrozbách pro vaši databázi v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82c9e-137">Explore threat detection alerts for your database in hello Azure portal</span></span>

<span data-ttu-id="82c9e-138">Detekce hrozeb databáze SQL se integruje se jeho výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span><span class="sxs-lookup"><span data-stu-id="82c9e-138">SQL Database Threat Detection integrates its alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span></span> <span data-ttu-id="82c9e-139">Za provozu zabezpečení dlaždice SQL v rámci okna databáze hello v hello portálu Azure sleduje stav hello active hrozeb.</span><span class="sxs-lookup"><span data-stu-id="82c9e-139">A live SQL security tile within hello database blade in hello Azure portal tracks hello status of active threats.</span></span> 

   ![Navigační podokno][6]
   
1. <span data-ttu-id="82c9e-141">Kliknutím na dlaždici zabezpečení SQL hello spustí okno výstrahy hello Azure Security Center a nabízí přehled active hrozeb SQL na databázi hello zjištěna.</span><span class="sxs-lookup"><span data-stu-id="82c9e-141">Clicking on hello SQL security tile launches hello Azure Security Center alerts blade and provides an overview of active SQL threats detected on hello database.</span></span> 

  ![Navigační podokno][7]

2. <span data-ttu-id="82c9e-143">Kliknutím na konkrétní výstrahu nabízí další podrobnosti a akcí pro příčin této hrozby a nápravě budoucí hrozeb.</span><span class="sxs-lookup"><span data-stu-id="82c9e-143">Clicking on a specific alert provides additional details and actions for investigating this threat and remediating future threats.</span></span>

  ![Navigační podokno][8]


## <a name="next-steps"></a><span data-ttu-id="82c9e-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82c9e-145">Next steps</span></span>

* <span data-ttu-id="82c9e-146">Další informace o detekce hrozeb, navštivte hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span><span class="sxs-lookup"><span data-stu-id="82c9e-146">Learn more about Threat Detection, visit hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span></span> 
* <span data-ttu-id="82c9e-147">Další informace o [auditování databáze SQL Azure](sql-database-auditing.md)</span><span class="sxs-lookup"><span data-stu-id="82c9e-147">Learn more about [Azure SQL Database Auditing](sql-database-auditing.md)</span></span>
* <span data-ttu-id="82c9e-148">Další informace o [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span><span class="sxs-lookup"><span data-stu-id="82c9e-148">Learn more about [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span></span>
* <span data-ttu-id="82c9e-149">Další podrobnosti o cenách najdete v tématu hello [stránky SQL Database – ceny](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span><span class="sxs-lookup"><span data-stu-id="82c9e-149">For more details on pricing, please see hello [SQL Database Pricing page](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span></span>  
* <span data-ttu-id="82c9e-150">Příklad skriptu prostředí PowerShell, najdete v části [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="82c9e-150">For a PowerShell script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span></span>



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


