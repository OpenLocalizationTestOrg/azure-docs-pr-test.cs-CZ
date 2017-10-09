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
# <a name="get-started-with-threat-detection"></a>Začínáme s detekce hrozeb.
> [!div class="op_single_selector"]
> * [Auditování](sql-data-warehouse-auditing-overview.md)
> * [Detekce hrozeb](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Přehled
Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze. Detekce hrozeb je ve verzi preview a je podporována pro SQL Data Warehouse.

Detekce hrozeb poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity. Uživatele můžete prozkoumat hello podezřelé události pomocí [auditování Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) toodetermine pokud vyplývají z tooaccess pokusu o porušení nebo zneužití dat v datovém skladu hello.
Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello datového skladu bez nutnosti toobe hello expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.

Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL. Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello. Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.

## <a name="set-up-threat-detection-for-your-database"></a>Nastavení detekce hrozeb pro vaši databázi
1. Spuštění hello Azure Portal na [https://portal.azure.com](https://portal.azure.com).
2. Přejděte okno Konfigurace toohello hello chcete toomonitor SQL Data Warehouse. V okně Nastavení hello, vyberte **auditování a detekce hrozeb**.
   
    ![Navigační podokno][1]
3. V hello **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby hello.
   
    ![Navigační podokno][2]
4. Zapnout **ON** detekce hrozby.
5. Konfigurace seznamu hello e-mailů, které budou dostávat upozornění zabezpečení při zjištění neobvyklé datového skladu aktivit.
6. Klikněte na tlačítko **Uložit** v hello **auditování a detekce hrozeb** toosave okno Konfigurace hello nové nebo aktualizované auditování a hrozeb zásady detekce.
   
    ![Navigační podokno][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Prozkoumejte data neobvyklé aktivity skladu při zjištění podezřelé události
1. Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity. <br/>
   e-mailu Hello se poskytují informace o událostí hello podezřelé zabezpečení včetně hello povaha hello neobvyklé aktivity, název databáze, čas serveru název a hello události. Kromě toho se budou poskytují informace o možných příčin a doporučená akce tooinvestigate a zmírnit hello potenciální hrozby toohello databáze.<br/>
   
    ![Navigační podokno][4]
2. V e-mailu hello, klikněte na hello **protokolu auditování databáze SQL Azure** odkaz, který bude spusťte hello portálu Azure Classic a zobrazit hello příslušné záznamy auditování době hello hello podezřelé události.
   
    ![Navigační podokno][5]
3. Klikněte na tooview záznamy auditu hello další podrobnosti o hello databáze podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.
   
    ![Navigační podokno][6]
4. V okně hello auditování záznamy, klikněte na tlačítko **otevřít v aplikaci Excel** tooimport šablony a spuštění hlubší analýzu protokolů auditu hello době hello hello podezřelé události v aplikaci excel tooopen předem nakonfigurovaná.<br/>
   **Poznámka:** v aplikaci Excel 2010 nebo novější, Power Query a hello **rychle zkombinovat** nastavení je povinné
   
    ![Navigační podokno][7]
5. tooconfigure hello **rychle zkombinovat** nastavení - hello **POWER QUERY** pásu karet vyberte **možnosti** dialogovém okně Možnosti toodisplay hello. Vyberte část hello ochrany osobních údajů a zvolte hello druhá možnost - 'Ignorovat hello úrovně ochrany osobních údajů a potenciálně tak vylepšit výkon':
   
    ![Navigační podokno][8]
6. protokoly auditu tooload SQL, zajistěte, aby hello parametry v kartě nastavení hello jsou správně nastavena a poté vyberte pásu karet "Data" hello a klikněte na tlačítko "Aktualizovat všechny" hello.
   
    ![Navigační podokno][9]
7. Hello výsledky se zobrazí v hello **protokoly auditu SQL** listu, která vám umožní toorun hlubší analýzu hello neobvyklé aktivity, které byly zjištěny a zmírnit dopad hello hello událostí zabezpečení ve vaší aplikaci.

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
