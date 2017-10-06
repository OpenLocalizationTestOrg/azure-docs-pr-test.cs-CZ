---
title: "analytické dotazy aaaRun proti více databází Azure SQL | Microsoft Docs"
description: "Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: f2664e4aafd2fecc98d20d229342bca19b0b08c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu

V tomto kurzu použijete elastické úlohy toorun dotazy pro každou databázi klienta. Úloha Hello extrahuje data prodeje lístků a načte ji do databáze analýzy (nebo datového skladu) pro analýzu. Hello analytics databáze je potom dotaz tooextract přehledy z této každodenní provozních dat všech klientů.


V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze analýzy hello klienta
> * Vytvoření tooretrieve dat naplánovanou úlohu a naplnění databáze analýzy hello

toocomplete splnění tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* je nainstalovaná nejnovější verze Hello systému SQL Server Management Studio (SSMS). [Stažení a instalace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Vzor provozní analýzy tenanta

Jedním z velké příležitosti hello s aplikacemi SaaS je toouse hello bohaté klienta data, která je uložená v cloudu hello. Použijte tato data toogain přehledy hello operace a využití vaší aplikace a klienty. Tato data můžete Průvodce vývoj funkce, vylepšení použitelnost a další investice do aplikace hello a platformu. V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jeden způsob tooaccessing tato data jsou toouse elastické úlohy, které umožňují vracení výsledků výsledků dotazu z toobe provádění úlohy zaznamenat do výstupní databáze a tabulky.

## <a name="get-hello-wingtip-application-scripts"></a>Získat hello Wingtip aplikační skripty

Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Nasazení databáze pro výsledky analýzy tenanta

Tento kurz vyžaduje toohave, které databáze nasazené toocapture hello výsledkem úlohy spouštění skriptů, které obsahují vrací výsledky dotazů. Pro tento účel si vytvoříme databázi nazvanou tenantanalytics.

1. Otevřete... \\Learning moduly\\provozní Analytics\\klienta Analytics\\*ukázku TenantAnalyticsDB.ps1* v hello *prostředí PowerShell ISE* a nastavte Hello následující hodnotu:
   * **$DemoScenario** = **2***Nasazení provozní analytické databáze* 
1. Stiskněte klávesu **F5** toorun hello ukázkový skript (hello tohoto volání *nasadit TenantAnalyticsDB.ps1* skriptu) vytváří databáze analýzy hello klienta.

## <a name="create-some-data-for-hello-demo"></a>Vytvoření některá data pro ukázku hello

1. Otevřete... \\Learning moduly\\provozní Analytics\\klienta Analytics\\*ukázku TenantAnalyticsDB.ps1* v hello *prostředí PowerShell ISE* a nastavte Hello následující hodnotu:
   * **$DemoScenario** = **1***Nákup lístků na akce na všech místech*
1. Stiskněte klávesu **F5** toorun hello skriptu a vytvoření lístku zakoupení historie.


## <a name="create-a-scheduled-job-tooretrieve-tenant-analytics-about-ticket-purchases"></a>Vytvoření analýza naplánovaná úloha tooretrieve klienta o lístek nákupy

Tento skript vytvoří informace o nákupu úlohy tooretrieve lístek od všech klientů. Jakmile agregován do jedné tabulky, můžete získat bohaté pronikavého metriky o lístek zakoupení vzory napříč hello klientům.

1. Otevřete aplikaci SSMS a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru
1. Otevřete složku ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Upravit &lt;uživatele&gt;, použijte hello uživatelské jméno používané při nasazení aplikace Wingtip SaaS hello v horní části hello hello skriptu **sp\_přidat\_cíl\_skupiny\_člen** a **sp\_přidat\_krok úlohy**
1. Klikněte pravým tlačítkem, vyberte **připojení**a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.
1. Ujistěte se, jsou připojené toohello **jobaccount** databáze a stiskněte klávesu **F5** pro spuštění skriptu hello

* **SP\_přidat\_cíl\_skupiny** vytvoří hello cílová skupina *TenantGroup*, nyní potřebujeme tooadd cíl členy.
* **SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je hello customer1 - &lt;Uživatele&gt; server obsahující databáze klienta hello) v čase úlohy by měl být součástí provádění úlohy hello.
* **sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou Nákup lístků ze všech tenantů.
* **SP\_přidat\_krok úlohy** vytvoří krok úlohy hello obsahující tooretrieve text T-SQL příkazu všechny informace o nákupu lístku hello ze všech klientů a kopírování hello vrací sadu výsledků do tabulce s názvem  *AllTicketsPurchasesfromAllTenants*
* Zbývající zobrazení Hello ve skriptu hello zobrazení hello existenci hello objekty a provádění úlohy monitorování. Zkontrolujte hodnotu stavu hello z hello **životního cyklu** sloupec toomonitor hello stavu. Jednou byly úspěšné, hello úloha úspěšně dokončí na všechny databáze klienta a hello dva další databáze, které obsahují hello referenční tabulce.

Úspěšném spuštění skriptu hello by měl mít za následek podobné výsledky:

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-tooretrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Vytvoření úlohy tooretrieve, souhrnné počet lístku zakoupí od všech klientů

Tento skript vytvoří úlohu tooretrieve součet všech nákupů lístek od všech klientů.

1. Otevřete aplikaci SSMS a připojte toohello *katalogu -&lt;uživatele&gt;. database.windows.net* serveru
1. Otevřete hello souboru... \\Učení moduly\\zřídit a katalog\\provozní Analytics\\klienta Analytics\\*TicketPurchasesfromAllTenants.sql výsledky*
1. Upravit &lt;uživatele&gt;, použijte hello uživatelské jméno používané při nasazení aplikace Wingtip SaaS hello ve skriptu hello v hello **sp\_přidat\_krok úlohy** uložené procedury
1. Klikněte pravým tlačítkem, vyberte **připojení**a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.
1. Ujistěte se, jsou připojené toohello **tenantanalytics** databáze a stiskněte klávesu **F5** pro spuštění skriptu hello

Úspěšném spuštění skriptu hello by měl mít za následek podobné výsledky:

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou ResultsTicketsOrders

* **SP\_přidat\_krok úlohy** vytvoří krok úlohy hello obsahující tooretrieve text T-SQL příkazu všechny informace o nákupu lístku hello ze všech klientů a vrací sadu výsledků do tabulce s názvem CountofTicketOrders hello kopie

* Zbývající zobrazení Hello ve skriptu hello zobrazení hello existenci hello objekty a provádění úlohy monitorování. Zkontrolujte hodnotu stavu hello z hello **životního cyklu** sloupec toomonitor hello stavu. Jednou byly úspěšné, hello úloha úspěšně dokončí na všechny databáze klienta a hello dva další databáze, které obsahují hello referenční tabulce.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Nasazení analytické databáze tenantů
> * Vytvoření naplánované úlohy analytická data tooretrieve mezi klienty

Blahopřejeme!

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavějí hello Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastické úlohy](sql-database-elastic-jobs-overview.md)
