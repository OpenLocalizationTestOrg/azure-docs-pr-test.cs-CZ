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
# <a name="sql-database-threat-detection"></a>Detekce hrozeb databáze SQL

SQL detekce hrozeb zjistila neobvyklé aktivity, které indikují neobvyklou a potenciálně škodlivé pokusy o přístup k nebo zneužití databáze.

## <a name="overview"></a>Přehled

Detekce hrozeb SQL poskytuje novou vrstvu zabezpečení, která uživatelům umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.  Uživatelé obdrží výstrahu při databáze podezřelé aktivity, potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL, jakož i nezvyklé databázové přístupové vzorce. Detekce hrozeb SQL výstrahy zadejte podrobnosti podezřelých aktivit a doporučujeme akce o tom, jak prozkoumat a zmírnit riziko. Uživatele můžete prozkoumat podezřelé události pomocí [auditování databáze SQL](sql-database-auditing.md) k určení, pokud vyplývají z pokus o přístup, porušení nebo využívat data v databázi. Detekce hrozeb zjednodušuje na potenciální hrozby adres do databáze bez nutnosti odborné zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.

Například Injektáž SQL je jedním z běžné problémy zabezpečení webových aplikací na Internetu, slouží k útokům na základě dat aplikace. Útočníci využít výhod ohrožení zabezpečení aplikace se zlými úmysly příkazy SQL, do pole pro zadání aplikací, vložit před nedodržením nebo upravovat data v databázi.

Detekce hrozeb SQL integruje výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), a každé chráněné databáze SQL server se bude účtovat podle za stejnou cenu jako Azure Security Center standardní vrstvy v $15 uzlu/měsíc, kde každé chráněné databáze SQL serveru se počítá jako jeden uzel. Zveme vás k bezplatnému vyzkoušení po dobu 60 dnů. 

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Nastavení detekce hrozeb pro vaši databázi na portálu Azure
1. Spuštění portálu Azure v [https://portal.azure.com](https://portal.azure.com).
2. Přejděte do okna konfigurace databáze SQL, kterou chcete sledovat. V okně Nastavení vyberte **auditování a detekce hrozeb**. 
    ![Navigační podokno][1]
3. V **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby.
  
    ![Navigační podokno][2]
4. Zapnout **ON** detekce hrozby.
5. Konfigurace seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.
6. Klikněte na tlačítko **Uložit** v **auditování a detekce hrozeb** okno a uložte nové nebo aktualizované auditování a hrozeb nastavením detekce.
       
    ![Navigační podokno][3]

## <a name="set-up-threat-detection-using-powershell"></a>Nastavení detekce hrozeb pomocí prostředí PowerShell

Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Prozkoumejte nezvyklé databázové aktivity, při zjištění podezřelé události
1. Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity. <br/>
   E-mailu bude poskytují informace o události podezřelé zabezpečení, včetně povahu neobvyklé aktivity, název databáze, název serveru, název aplikace a čas události. Kromě toho e-mailu bude poskytovat informace na možné příčiny a doporučené akce ke zkoumání a zmírnit potenciální hrozbu do databáze.<br/>
     
    ![Navigační podokno][4]
2. E-mailové upozornění obsahuje přímý odkaz na protokol auditování SQL. Kliknutím na tento odkaz spuštění portálu Azure a otevře se záznamy auditu SQL v době podezřelé události. Klikněte na záznam auditu zobrazíte další podrobnosti o činnostech podezřelé databáze, bylo snazší najít příkazy SQL, které byly provedeny (který přístup, co se a kdy) a zjistěte, zda událost legitimní nebo škodlivý (například byl zneužití ohrožení zabezpečení aplikace k Injektáž SQL, někdo nedodržení citlivá data, atd.).<br/>
   ![Navigační podokno][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Seznamte se výstrah o zjištěných hrozbách pro vaši databázi na portálu Azure

Detekce hrozeb databáze SQL se integruje se jeho výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/). Živé dlaždice zabezpečení SQL v okně databáze na portálu Azure sleduje stav active hrozeb. 

   ![Navigační podokno][6]
   
1. Kliknutím na SQL dlaždice zabezpečení spustí okno výstrahy Azure Security Center a nabízí přehled active SQL hrozby zjištěné v databázi. 

  ![Navigační podokno][7]

2. Kliknutím na konkrétní výstrahu nabízí další podrobnosti a akcí pro příčin této hrozby a nápravě budoucí hrozeb.

  ![Navigační podokno][8]


## <a name="next-steps"></a>Další kroky

* Další informace o detekce hrozeb najdete [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* Další informace o [auditování databáze SQL Azure](sql-database-auditing.md)
* Další informace o [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)
* Další podrobnosti o cenách najdete [stránky SQL Database – ceny](https://azure.microsoft.com/en-us/pricing/details/sql-database/)  
* Příklad skriptu prostředí PowerShell, najdete v části [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


