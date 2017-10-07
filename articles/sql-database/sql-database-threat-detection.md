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
# <a name="sql-database-threat-detection"></a>Detekce hrozeb databáze SQL

SQL detekce hrozeb zjistila neobvyklé aktivity, které indikují pokusy o neobvyklou a potenciálně škodlivé tooaccess nebo zneužití databází.

## <a name="overview"></a>Přehled

Detekce hrozeb SQL poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.  Uživatelé obdrží výstrahu při databáze podezřelé aktivity, potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL, jakož i nezvyklé databázové přístupové vzorce. Detekce hrozeb SQL výstrahy zadejte podrobnosti podezřelých aktivit a doporučujeme akce jak tooinvestigate a zmírnit hrozby hello. Uživatele můžete prozkoumat hello podezřelé události pomocí [auditování databáze SQL](sql-database-auditing.md) toodetermine pokud vyplývají z tooaccess pokusu o porušení nebo zneužití data v databázi hello. Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello databáze bez hello nutné toobe expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.

Například Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello. Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.

Detekce hrozeb SQL integruje výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), a každé chráněné databáze SQL server se bude účtovat podle hello stejné cena jako Azure Security Center standardní vrstvy v $15 uzlu/měsíc, kde každé chráněné SQL Databázový server se počítá jako jeden uzel. Doporučujeme vám tootry ho za 60 dnů pro uvolnění. 

## <a name="set-up-threat-detection-for-your-database-in-hello-azure-portal"></a>Nastavení detekce hrozeb pro vaše databáze v hello portálu Azure
1. Spusťte hello Azure portálu na [https://portal.azure.com](https://portal.azure.com).
2. Přejděte okno Konfigurace toohello hello chcete toomonitor databáze SQL. V okně Nastavení hello, vyberte **auditování a detekce hrozeb**. 
    ![Navigační podokno][1]
3. V hello **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby hello.
  
    ![Navigační podokno][2]
4. Zapnout **ON** detekce hrozby.
5. Konfigurace seznamu hello e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.
6. Klikněte na tlačítko **Uložit** v hello **auditování a detekce hrozeb** okno toosave hello nové nebo aktualizované auditování a hrozeb detekce nastavení.
       
    ![Navigační podokno][3]

## <a name="set-up-threat-detection-using-powershell"></a>Nastavení detekce hrozeb pomocí prostředí PowerShell

Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Prozkoumejte nezvyklé databázové aktivity, při zjištění podezřelé události
1. Obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity. <br/>
   e-mailu Hello se poskytují informace o události hello podezřelé zabezpečení včetně hello povaha hello neobvyklé aktivity, název databáze, název serveru, název aplikace a čas události hello. Kromě toho hello e-mailu bude poskytovat informace na možné příčiny a doporučené akce tooinvestigate a zmírnit hello potenciální hrozby toohello databáze.<br/>
     
    ![Navigační podokno][4]
2. e-mailové výstrahy Hello zahrnuje protokol auditování SQL toohello přímý odkaz. Kliknutím na tento odkaz spustí hello Azure portal a otevře se okno hello SQL záznamy auditu době hello hello podezřelé události. Klikněte na tooview záznamů auditu další podrobnosti o hello databáze podezřelé aktivity, takže je jednodušší toofind hello SQL příkazy, které byly provedeny (který přístup, co se a kdy) a zjistěte, zda text hello událostí legitimní nebo škodlivý (například aplikace bylo by vkládání tooSQL ohrožení zabezpečení, někdo nedodržení citlivá data, atd.).<br/>
   ![Navigační podokno][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-hello-azure-portal"></a>Seznamte se výstrah o zjištěných hrozbách pro vaši databázi v hello portálu Azure

Detekce hrozeb databáze SQL se integruje se jeho výstrahy s [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/). Za provozu zabezpečení dlaždice SQL v rámci okna databáze hello v hello portálu Azure sleduje stav hello active hrozeb. 

   ![Navigační podokno][6]
   
1. Kliknutím na dlaždici zabezpečení SQL hello spustí okno výstrahy hello Azure Security Center a nabízí přehled active hrozeb SQL na databázi hello zjištěna. 

  ![Navigační podokno][7]

2. Kliknutím na konkrétní výstrahu nabízí další podrobnosti a akcí pro příčin této hrozby a nápravě budoucí hrozeb.

  ![Navigační podokno][8]


## <a name="next-steps"></a>Další kroky

* Další informace o detekce hrozeb, navštivte hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* Další informace o [auditování databáze SQL Azure](sql-database-auditing.md)
* Další informace o [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)
* Další podrobnosti o cenách najdete v tématu hello [stránky SQL Database – ceny](https://azure.microsoft.com/en-us/pricing/details/sql-database/)  
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


