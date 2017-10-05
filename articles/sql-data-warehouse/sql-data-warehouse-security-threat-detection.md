---
title: "Začínáme s SQL Data Warehouse služba detekce hrozeb"
description: "Jak začít pracovat s detekce hrozeb."
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: f4a2376fe4fb710d031c35ca7fdbf4c7bb0f3caa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-threat-detection"></a><span data-ttu-id="aa874-103">Začínáme s detekce hrozeb.</span><span class="sxs-lookup"><span data-stu-id="aa874-103">Get started with threat detection</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aa874-104">Auditování</span><span class="sxs-lookup"><span data-stu-id="aa874-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="aa874-105">Detekce hrozeb</span><span class="sxs-lookup"><span data-stu-id="aa874-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="aa874-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="aa874-106">Overview</span></span>
<span data-ttu-id="aa874-107">Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální ohrožení databáze.</span><span class="sxs-lookup"><span data-stu-id="aa874-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="aa874-108">Detekce hrozeb je ve verzi preview a je podporována pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="aa874-108">Threat Detection is in preview and is supported for SQL Data Warehouse.</span></span>

<span data-ttu-id="aa874-109">Detekce hrozeb poskytuje novou vrstvu zabezpečení, která uživatelům umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="aa874-109">Threat Detection provides a new layer of security, which enables customers to detect and respond to potential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="aa874-110">Uživatele můžete prozkoumat podezřelé události pomocí [auditování Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) k určení, pokud vyplývají z pokus o přístup, porušení nebo využívat data v datovém skladu.</span><span class="sxs-lookup"><span data-stu-id="aa874-110">Users can explore the suspicious events using [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) to determine if they result from an attempt to access, breach or exploit data in the data warehouse.</span></span>
<span data-ttu-id="aa874-111">Detekce hrozeb zjednodušuje na potenciální hrozby adresu do datového skladu, aniž by museli být odborné zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="aa874-111">Threat Detection makes it simple to address potential threats to the data warehouse without the need to be a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="aa874-112">Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="aa874-112">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="aa874-113">Injektáž SQL je jedním z běžné problémy zabezpečení webových aplikací na Internetu, slouží k útokům na základě dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa874-113">SQL injection is one of the common Web application security issues on the Internet, used to attack data-driven applications.</span></span> <span data-ttu-id="aa874-114">Útočníci využít výhod vložit škodlivý příkazy SQL, do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi aplikace ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa874-114">Attackers take advantage of application vulnerabilities to inject malicious SQL statements into application entry fields, for breaching or modifying data in the database.</span></span>

## <a name="set-up-threat-detection-for-your-database"></a><span data-ttu-id="aa874-115">Nastavení detekce hrozeb pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="aa874-115">Set up threat detection for your database</span></span>
1. <span data-ttu-id="aa874-116">Spuštění portálu Azure v [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa874-116">Launch the Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="aa874-117">Přejděte do okna konfigurace služby SQL Data Warehouse, kterou chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="aa874-117">Navigate to the configuration blade of the SQL Data Warehouse you want to monitor.</span></span> <span data-ttu-id="aa874-118">V okně Nastavení vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="aa874-118">In the Settings blade, select **Auditing & Threat Detection**.</span></span>
   
    ![Navigační podokno][1]
3. <span data-ttu-id="aa874-120">V **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="aa874-120">In the **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display the Threat detection settings.</span></span>
   
    ![Navigační podokno][2]
4. <span data-ttu-id="aa874-122">Zapnout **ON** detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="aa874-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="aa874-123">Konfigurace seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění neobvyklé datového skladu aktivit.</span><span class="sxs-lookup"><span data-stu-id="aa874-123">Configure the list of emails that will receive security alerts upon detection of anomalous data warehouse activities.</span></span>
6. <span data-ttu-id="aa874-124">Klikněte na tlačítko **Uložit** v **auditování a detekce hrozeb** okno Konfigurace a uložení nové nebo aktualizované auditování a zásady detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="aa874-124">Click **Save** in the **Auditing & Threat detection** configuration blade to save the new or updated auditing and threat detection policy.</span></span>
   
    ![Navigační podokno][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="aa874-126">Prozkoumejte data neobvyklé aktivity skladu při zjištění podezřelé události</span><span class="sxs-lookup"><span data-stu-id="aa874-126">Explore anomalous data warehouse activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="aa874-127">Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="aa874-127">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="aa874-128">E-mailu bude poskytují informace o události podezřelé zabezpečení, včetně povahu neobvyklé aktivity, název databáze, název serveru a čas události.</span><span class="sxs-lookup"><span data-stu-id="aa874-128">The email will provide information on the suspicious security event including the nature of the anomalous activities, database name, server name and the event time.</span></span> <span data-ttu-id="aa874-129">Kromě toho bude poskytovat informace na možné příčiny a doporučené akce ke zkoumání a zmírnit potenciální hrozbu do databáze.</span><span class="sxs-lookup"><span data-stu-id="aa874-129">In addition, it will provide information on possible causes and recommended actions to investigate and mitigate the potential threat to the database.</span></span><br/>
   
    ![Navigační podokno][4]
2. <span data-ttu-id="aa874-131">V e-mailu, klikněte na **protokolu auditování databáze SQL Azure** odkaz, který bude spuštění portálu Azure Classic a zobrazit příslušné záznamy v době podezřelé události auditování.</span><span class="sxs-lookup"><span data-stu-id="aa874-131">In the email, click on the **Azure SQL Auditing Log** link, which will launch the Azure Classic Portal and show the relevant Auditing records around the time of the suspicious event.</span></span>
   
    ![Navigační podokno][5]
3. <span data-ttu-id="aa874-133">Klikněte na záznamy auditu zobrazíte další podrobnosti v databázi podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.</span><span class="sxs-lookup"><span data-stu-id="aa874-133">Click on the audit records to view more details on the suspicious database activities such as SQL statement, failure reason and client IP.</span></span>
   
    ![Navigační podokno][6]
4. <span data-ttu-id="aa874-135">V okně auditování záznamy, klikněte na tlačítko **otevřete v aplikaci Excel** otevřete předem nakonfigurovaná v aplikaci excel šablony k importu a spuštění hlubší analýzy protokol auditu v době podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="aa874-135">In the Auditing Records blade, click  **Open in Excel** to open a pre-configured excel template to import and run deeper analysis of the audit log around the time of the suspicious event.</span></span><br/><span data-ttu-id="aa874-136">
   **Poznámka:** v aplikaci Excel 2010 nebo novější, Power Query a **rychle zkombinovat** nastavení je povinné</span><span class="sxs-lookup"><span data-stu-id="aa874-136">
**Note:** In Excel 2010 or later, Power Query and the **Fast Combine** setting is required</span></span>
   
    ![Navigační podokno][7]
5. <span data-ttu-id="aa874-138">Ke konfiguraci **rychle zkombinovat** nastavení - v **POWER QUERY** pásu karet vyberte **možnosti** zobrazíte dialogové okno Možnosti.</span><span class="sxs-lookup"><span data-stu-id="aa874-138">To configure the **Fast Combine** setting - In the **POWER QUERY** ribbon tab, select **Options** to display the Options dialog.</span></span> <span data-ttu-id="aa874-139">Vyberte v části o ochraně osobních údajů a vyberte druhou možnost - 'Ignorovat úrovně soukromí a potenciálně tak vylepšit výkon':</span><span class="sxs-lookup"><span data-stu-id="aa874-139">Select the Privacy section and choose the second option - 'Ignore the Privacy Levels and potentially improve performance':</span></span>
   
    ![Navigační podokno][8]
6. <span data-ttu-id="aa874-141">Načíst protokoly auditu SQL, zajistěte, aby parametrů v nastavení kartě jsou správně nastavena a poté vyberte pásu karet, Data a klikněte na tlačítko 'aktualizovat vše..</span><span class="sxs-lookup"><span data-stu-id="aa874-141">To load SQL audit logs, ensure that the parameters in the settings tab are set correctly and then select the 'Data' ribbon and click the 'Refresh All' button.</span></span>
   
    ![Navigační podokno][9]
7. <span data-ttu-id="aa874-143">Výsledky se zobrazí v **protokoly auditu SQL** listu, která umožňuje spustit hlubší analýzu neobvyklé aktivity, které byly zjištěny a zmírnit dopad událostí zabezpečení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa874-143">The results appear in the **SQL Audit Logs** sheet which enables you to run deeper analysis of the anomalous activities that were detected, and mitigate the impact of the security event in your application.</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
