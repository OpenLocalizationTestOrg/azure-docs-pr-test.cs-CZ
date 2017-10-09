---
title: "aaaGet začít s SQL Data Warehouse služba detekce hrozeb"
description: Jak tooget pracovat s detekce hrozeb.
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
ms.openlocfilehash: dec0b734849e7f52434e099db0b38fbf0bf6ad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-threat-detection"></a><span data-ttu-id="2c699-103">Začínáme s detekce hrozeb.</span><span class="sxs-lookup"><span data-stu-id="2c699-103">Get started with threat detection</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c699-104">Auditování</span><span class="sxs-lookup"><span data-stu-id="2c699-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="2c699-105">Detekce hrozeb</span><span class="sxs-lookup"><span data-stu-id="2c699-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="2c699-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="2c699-106">Overview</span></span>
<span data-ttu-id="2c699-107">Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="2c699-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="2c699-108">Detekce hrozeb je ve verzi preview a je podporována pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2c699-108">Threat Detection is in preview and is supported for SQL Data Warehouse.</span></span>

<span data-ttu-id="2c699-109">Detekce hrozeb poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="2c699-109">Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="2c699-110">Uživatele můžete prozkoumat hello podezřelé události pomocí [auditování Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) toodetermine pokud vyplývají z tooaccess pokusu o porušení nebo zneužití dat v datovém skladu hello.</span><span class="sxs-lookup"><span data-stu-id="2c699-110">Users can explore hello suspicious events using [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) toodetermine if they result from an attempt tooaccess, breach or exploit data in hello data warehouse.</span></span>
<span data-ttu-id="2c699-111">Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello datového skladu bez nutnosti toobe hello expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="2c699-111">Threat Detection makes it simple tooaddress potential threats toohello data warehouse without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="2c699-112">Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="2c699-112">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="2c699-113">Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2c699-113">SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="2c699-114">Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="2c699-114">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, for breaching or modifying data in hello database.</span></span>

## <a name="set-up-threat-detection-for-your-database"></a><span data-ttu-id="2c699-115">Nastavení detekce hrozeb pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="2c699-115">Set up threat detection for your database</span></span>
1. <span data-ttu-id="2c699-116">Spuštění hello Azure Portal na [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2c699-116">Launch hello Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2c699-117">Přejděte okno Konfigurace toohello hello chcete toomonitor SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2c699-117">Navigate toohello configuration blade of hello SQL Data Warehouse you want toomonitor.</span></span> <span data-ttu-id="2c699-118">V okně Nastavení hello, vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="2c699-118">In hello Settings blade, select **Auditing & Threat Detection**.</span></span>
   
    ![Navigační podokno][1]
3. <span data-ttu-id="2c699-120">V hello **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby hello.</span><span class="sxs-lookup"><span data-stu-id="2c699-120">In hello **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display hello Threat detection settings.</span></span>
   
    ![Navigační podokno][2]
4. <span data-ttu-id="2c699-122">Zapnout **ON** detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="2c699-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="2c699-123">Konfigurace seznamu hello e-mailů, které budou dostávat upozornění zabezpečení při zjištění neobvyklé datového skladu aktivit.</span><span class="sxs-lookup"><span data-stu-id="2c699-123">Configure hello list of emails that will receive security alerts upon detection of anomalous data warehouse activities.</span></span>
6. <span data-ttu-id="2c699-124">Klikněte na tlačítko **Uložit** v hello **auditování a detekce hrozeb** toosave okno Konfigurace hello nové nebo aktualizované auditování a hrozeb zásady detekce.</span><span class="sxs-lookup"><span data-stu-id="2c699-124">Click **Save** in hello **Auditing & Threat detection** configuration blade toosave hello new or updated auditing and threat detection policy.</span></span>
   
    ![Navigační podokno][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="2c699-126">Prozkoumejte data neobvyklé aktivity skladu při zjištění podezřelé události</span><span class="sxs-lookup"><span data-stu-id="2c699-126">Explore anomalous data warehouse activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="2c699-127">Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="2c699-127">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="2c699-128">e-mailu Hello se poskytují informace o událostí hello podezřelé zabezpečení včetně hello povaha hello neobvyklé aktivity, název databáze, čas serveru název a hello události.</span><span class="sxs-lookup"><span data-stu-id="2c699-128">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name and hello event time.</span></span> <span data-ttu-id="2c699-129">Kromě toho se budou poskytují informace o možných příčin a doporučená akce tooinvestigate a zmírnit hello potenciální hrozby toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="2c699-129">In addition, it will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span><br/>
   
    ![Navigační podokno][4]
2. <span data-ttu-id="2c699-131">V e-mailu hello, klikněte na hello **protokolu auditování databáze SQL Azure** odkaz, který bude spusťte hello portálu Azure Classic a zobrazit hello příslušné záznamy auditování době hello hello podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="2c699-131">In hello email, click on hello **Azure SQL Auditing Log** link, which will launch hello Azure Classic Portal and show hello relevant Auditing records around hello time of hello suspicious event.</span></span>
   
    ![Navigační podokno][5]
3. <span data-ttu-id="2c699-133">Klikněte na tooview záznamy auditu hello další podrobnosti o hello databáze podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.</span><span class="sxs-lookup"><span data-stu-id="2c699-133">Click on hello audit records tooview more details on hello suspicious database activities such as SQL statement, failure reason and client IP.</span></span>
   
    ![Navigační podokno][6]
4. <span data-ttu-id="2c699-135">V okně hello auditování záznamy, klikněte na tlačítko **otevřít v aplikaci Excel** tooimport šablony a spuštění hlubší analýzu protokolů auditu hello době hello hello podezřelé události v aplikaci excel tooopen předem nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="2c699-135">In hello Auditing Records blade, click  **Open in Excel** tooopen a pre-configured excel template tooimport and run deeper analysis of hello audit log around hello time of hello suspicious event.</span></span><br/><span data-ttu-id="2c699-136">
   **Poznámka:** v aplikaci Excel 2010 nebo novější, Power Query a hello **rychle zkombinovat** nastavení je povinné</span><span class="sxs-lookup"><span data-stu-id="2c699-136">
**Note:** In Excel 2010 or later, Power Query and hello **Fast Combine** setting is required</span></span>
   
    ![Navigační podokno][7]
5. <span data-ttu-id="2c699-138">tooconfigure hello **rychle zkombinovat** nastavení - hello **POWER QUERY** pásu karet vyberte **možnosti** dialogovém okně Možnosti toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="2c699-138">tooconfigure hello **Fast Combine** setting - In hello **POWER QUERY** ribbon tab, select **Options** toodisplay hello Options dialog.</span></span> <span data-ttu-id="2c699-139">Vyberte část hello ochrany osobních údajů a zvolte hello druhá možnost - 'Ignorovat hello úrovně ochrany osobních údajů a potenciálně tak vylepšit výkon':</span><span class="sxs-lookup"><span data-stu-id="2c699-139">Select hello Privacy section and choose hello second option - 'Ignore hello Privacy Levels and potentially improve performance':</span></span>
   
    ![Navigační podokno][8]
6. <span data-ttu-id="2c699-141">protokoly auditu tooload SQL, zajistěte, aby hello parametry v kartě nastavení hello jsou správně nastavena a poté vyberte pásu karet "Data" hello a klikněte na tlačítko "Aktualizovat všechny" hello.</span><span class="sxs-lookup"><span data-stu-id="2c699-141">tooload SQL audit logs, ensure that hello parameters in hello settings tab are set correctly and then select hello 'Data' ribbon and click hello 'Refresh All' button.</span></span>
   
    ![Navigační podokno][9]
7. <span data-ttu-id="2c699-143">Hello výsledky se zobrazí v hello **protokoly auditu SQL** listu, která vám umožní toorun hlubší analýzu hello neobvyklé aktivity, které byly zjištěny a zmírnit dopad hello hello událostí zabezpečení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2c699-143">hello results appear in hello **SQL Audit Logs** sheet which enables you toorun deeper analysis of hello anomalous activities that were detected, and mitigate hello impact of hello security event in your application.</span></span>

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
