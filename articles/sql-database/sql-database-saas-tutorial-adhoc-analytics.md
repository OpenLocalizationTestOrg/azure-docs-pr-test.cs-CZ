---
title: "Analýza dotazů ad-hoc aaaRun napříč více databází Azure SQL | Microsoft Docs"
description: "Spuštění dotazů ad-hoc analytics napříč více databází SQL v hello Wingtip SaaS víceklientské aplikace."
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
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 140cd51fdd03b5a548147282b51ceb0ad80c944d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-ad-hoc-analytics-queries-across-all-wingtip-saas-tenants"></a>Spuštění analytics dotazů ad-hoc napříč všichni klienti Wingtip SaaS

V tomto kurzu spustíte distribuované dotazy napříč celou sadu databází, klienta hello tooenable ad-hoc analytics. Elastické dotaz je použité tooenable distribuovaných dotazů, což vyžaduje, aby další analýze databáze je nasazena (toohello katalogu server). Tyto dotazy můžete extrahovat Statistika schovaný v každodenní provozních dat hello hello Wingtip SaaS aplikace.


V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * O hello globální zobrazení v každé databázi, které umožňují efektivní dotazování mezi klienty
> * Jak toodeploy databázi analýzy ad-hoc
> * Jak toorun distribuované dotazy mezi všechny databáze klienta



toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* SQL Server Management Studio (SSMS) je nainstalována. toodownload a instalace aplikace SSMS, najdete v části [stáhnout SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Vzor analytics ad-hoc

Jedním z velké příležitosti hello s aplikacemi SaaS je toouse hello velká množství dat klienta centrálně uložené v cloudu hello. Použijte tato data toogain přehledy hello operace a využití vaší aplikace vašich klientů, jejich, předvolby, chování, uživatelů atd. Funkce vývoj, vylepšení použitelnost a další investice do vaší aplikace a služby, můžete tyto přehledy průvodce.

V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jeden z přístupů je toouse [elastické dotazu](sql-database-elastic-query-overview.md), což umožňuje dotazování na distribuovanou sadu databází s společné schéma. Elastické dotaz používá jeden *head* databáze, ve kterém jsou definovány externí tabulky, že zrcadlení tabulky a zobrazení hello distribuovaných databází (klientů). Dotazy odeslané toothis head databáze jsou kompilované tooproduce plán distribuovaného dotazu s části dotazu hello posunuta dolů toohello klienta databáze podle potřeby. Elastické dotaz používá hello horizontálního oddílu mapy v umístění hello hello katalogu databáze tooprovide hello klienta databází. Instalační program a dotaz jsou jednoduché, pomocí standardní [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)a podporují ad hoc dotazy z nástroje, například Power BI a Excel.

Mezi databázemi klienta hello distribucí dotazy, poskytuje elastické dotazu okamžitý přehled o provozu provozními daty. Však jako elastické dotaz získává data ze potenciálně velký počet databází, odeslat dotaz latence v některých případech může být vyšší než pro dotazy na ekvivalentní tooa jedné víceklientské databáze. Potřeba dát pozor při navrhování dotazuje toominimize hello data, která je vrácena. Elastické dotazu je často nejvhodnější pro dotazy malých objemů dat v reálném čase, jako názvem na rozdíl od toobuilding často používají složité analytické dotazy nebo sestavy. Pokud dotazy neprovádí dobře, podívejte se na hello [plán spuštění](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) toosee, jaká část dotazu hello bylo posunuto toohello vzdálené databáze a kolik dat se vrací. Dotazy, které vyžadují komplexní analytického zpracování může být lepší obsluhuje v některých případech extrahování dat klienta do vyhrazené databáze nebo datového skladu optimalizované pro analytické dotazy. Tento vzor je vysvětleno v hello [kurzu analýza klienta](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-hello-wingtip-application-scripts"></a>Získat hello Wingtip aplikační skripty

Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>Vytvořit data prodeje lístků

spuštěním hello lístek – generátor toorun dotazy proti zajímavějšího datové sady, vytvořte prodejní data lístku.

1. V hello *prostředí PowerShell ISE*, otevřete hello... \\Learning moduly\\provozní Analytics\\ad hoc Analytics\\*ukázku AdhocAnalytics.ps1* skript a nastavte hello následující hodnoty:
   * **$DemoScenario** = 1, **zakoupit lístky pro události na všechna místa**.
2. Stiskněte klávesu **F5** a spusťte skript hello generovat prodeje lístků. Je spuštěn skript hello, pokračujte hello kroky v tomto kurzu. data lístku Hello je dotazován v hello *spouštění dotazů ad-hoc distribuované* části, proto počkejte toocomplete generátor lístku hello, pokud je stále spuštěn, když získat toothat cvičení.

## <a name="explore-hello-global-views"></a>Prozkoumejte hello globální zobrazení

Hello aplikace Wingtip SaaS vytvořená s využitím modelu klienta pro databáze, tak z hlediska jednoho klienta je definována schéma databáze klienta hello. Informace specifické pro klienta existuje v jedné tabulce *místo*, vždy má jeden řádek a je implementovaný jako haldy, bez primárního klíče. Ostatní tabulky v hello schématu nepotřebujete toobe související toohello *místo* tabulky, protože při běžném použití, se nikdy pochybností datových hello klienta patří.

Ale při dotazování mezi všechny databáze, je důležité, aby můžete elastické dotazu, pokud je součástí jedné logické databáze horizontálně dělené klientem považovala hello data. tooachieve to sadu 'globální' zobrazení jsou přidané toohello klienta databáze, která projektu id klienta do každé hello tabulek, které jsou předmětem dotazování globálně. Například hello *VenueEvents* zobrazení přidá počítaný *VenueId* toohello sloupce projektovat z hello *události* tabulky. Definováním hello externí tabulky v databázi head hello přes *VenueEvents* (místo hello základní *události* tabulky), elastické dotazu je možné toopush dolů na základě spojení *VenueId* , mohou být provedeny souběžně na každém vzdálené databáze (ne na hello head databáze). Tím se výrazně snižuje hello množství dat, která je vrácena, což vede k podstatné zvýšení výkonu pro mnoho dotazů. Tato globální zobrazení byly předem vytvořené v všechny databáze klienta (a v *basetenantdb*).

1. Otevřete aplikaci SSMS a [připojit toohello tenants1 -&lt;uživatele&gt; server](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Rozbalte položku **databáze**, klikněte pravým tlačítkem na **contosoconcerthall**a vyberte **nový dotaz**.
3. Spusťte následující dotazy tooexplore hello rozdíl mezi hello jednoho klienta tabulek a zobrazení globální hello hello:

   ```T-SQL
   -- hello base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice hello plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- hello base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects hello VenueId retrieved from hello Venues table.
   SELECT * FROM VenueEvents
   ```

V těchto zobrazeních hello *VenueId* se vypočítá jako hodnota hash hello místo názvu, ale žádné přístup může být použité toointroduce jedinečnou hodnotu. Tento přístup je podobné toohello způsob, jakým je počítaný hello klíč klienta pro použití v katalogu hello.

definice hello tooexamine hello *místa* zobrazení:

1. V **Průzkumník objektů**, rozbalte položku **contosoconcethall** > **zobrazení**:

   ![zobrazení](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Klikněte pravým tlačítkem na **dbo. Místa**.
3. Vyberte **skript zobrazení jako** > **vytvořit** > **nové okno editoru dotazů**

Některé z hello další skript *místo* zobrazení toosee jak zvyšují hello *VenueId*.

## <a name="deploy-hello-database-used-for-ad-hoc-distributed-queries"></a>Nasazení databáze hello použít pro ad-hoc distribuované dotazy

Toto cvičení nasadí hello *adhocanalytics* databáze. Toto je hello head databáze, která bude obsahovat hello schématu použitého k dotazování mezi všechny databáze klienta. Hello databáze je nasazené toohello existující katalogu server, který je použit pro všechny týkajících se správy databáze v ukázkové aplikace hello server hello.

1. Otevřete... \\Learning moduly\\provozní Analytics\\ad hoc Analytics\\*ukázku AdhocAnalytics.ps1* v hello *prostředí PowerShell ISE* a sadu hello následující hodnoty:
   * **$DemoScenario** = 2, **Deploy Ad-hoc analytics database** (Nasadit analytickou databázi ad hoc).

2. Stiskněte klávesu **F5** toorun hello skriptu a vytvořit hello *adhocanalytics* databáze.

V další části hello přidáte schématu toohello databáze, aby ho bylo možné použít toorun distribuované dotazy.

## <a name="configure-hello-database-for-running-distributed-queries"></a>Konfigurace hello databáze pro spuštění distribuovaných dotazů

Tento postup přidá schématu (hello externí zdroj dat a externí tabulky definice) toohello ad-hoc analytics databázi, která umožňuje dotazování mezi všechny databáze klienta.

1. Otevřete aplikaci SQL Server Management Studio a připojte toohello ad hoc Analytics databáze, kterou jste vytvořili v předchozím kroku hello. Název Hello hello databáze bude adhocanalytics.
2. Otevřete ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *inicializovat AdhocAnalyticsDB.sql* v aplikaci SSMS.
3. Zkontrolujte skript SQL hello a poznamenejte si hello následující:

   Elastické dotaz používá přihlašovacích údajů platných pro databázi tooaccess jednotlivých databází hello klienta. Tento přihlašovací údaj potřebuje k dispozici ve všech databází hello toobe a by měla být poskytnuta normálně hello minimální oprávnění vyžaduje tooenable těchto dotazů ad-hoc.

    ![Vytvoření pověření](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   Hello externí zdroj dat, který je definován toouse hello klienta horizontálního oddílu mapy v databázi katalogu hello. Prostřednictvím tohoto jako zdroj externích dat hello dotazy jsou distribuované tooall databáze zaregistrované v katalogu hello při spuštění dotazu hello. Vzhledem k tomu, že názvy serverů se liší pro každé nasazení, získá tento skript inicializace hello umístění databáze katalogu hello načtením aktuální server hello (@@servername) kde hello skript se spustí.

    ![Vytvoření externího zdroje dat](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   Hello externí tabulky, které odkazují na globální zobrazení hello popsané v předchozí části hello a definovaný s **distribuční = SHARDED(VenueId)**. Protože každý *VenueId* mapuje tooa jedné databáze, to zvyšuje výkon pro mnoho scénářů, jak je znázorněno v další části hello.

    ![Vytvoření externí tabulky](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   místní tabulky Hello *VenueTypes* , je vytvořeny a naplněny. Tuto referenční tabulku dat je běžné v všechny databáze klienta, takže může být reprezentován jako na místní tabulku a vyplněná běžné daty hello. Pro některé dotazy hello to může snížit množství dat přesunout mezi databázemi hello klienta a hello *adhocanalytics* databáze.

    ![Vytvoření tabulky](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Pokud zahrnete referenční tabulky tímto způsobem, být jisti tooupdate hello tabulky schéma a data při každé aktualizaci databáze klienta hello.

4. Stiskněte klávesu **F5** toorun hello skriptu a inicializace hello *adhocanalytics* databáze. 

Nyní můžete spustit distribuované dotazy a shromažďovat statistiky napříč všichni klienti!

## <a name="run-ad-hoc-distributed-queries"></a>Spuštění dotazů ad-hoc distribuované

Teď tento hello *adhocanalytics* databáze je nastavit, aby ihned začít a spustit některé distribuované dotazy. Zahrnout plán spuštění hello lépe porozumět tomu, kde se hello zpracování dotazu děje. 

Při kontrole plán spuštění hello, pozastavte ukazatel myši nad hello plán ikony podrobnosti. 

Toto nastavení je důležité toonote **distribuční = SHARDED(VenueId)** když jsme definovali hello externí zdroj dat, zlepšuje výkon pro mnoho scénářů. Protože každý *VenueId* mapuje tooa jedné databáze filtrování je snadno done vzdáleně, vracejících pouze hello dat potřebujeme.

1. Otevřete skript …\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* v SSMS.
2. Ujistěte se, jsou připojené toohello **adhocanalytics** databáze.
3. Vyberte hello **dotazu** nabídky a klikněte na tlačítko **zahrnují skutečné naplánovat spuštění**
4. Zvýrazněte hello *místa, které jsou aktuálně registrované?* dotazu a stiskněte klávesu **F5**.

   Hello dotaz vrátí seznamu celý místo hello ilustrující způsob rychlé a snadné je tooquery ve všech klientů a vracená data z každého klienta.

   Zkontrolujte hello plán a zjistíte, že náklady hello je hello vzdálený dotaz, protože jsme jednoduše probíhající tooeach klienta databáze a výběrem hello místo informace.

   ![Vybrat * z dbo. Místa](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. Vyberte hello další dotaz a stiskněte klávesu **F5**.

   Tento dotaz připojí data z databáze hello klienta a místní hello *VenueTypes* tabulky (místní počítač, protože je tabulka v hello *adhocanalytics* databáze).

   Zkontrolujte plán hello a najdete v části tohoto hello většina náklady je hello vzdálený dotaz, protože jsme dotaz na informace místo každého klienta (dbo. Místa), a poté proveďte rychlé místní spojení s místní hello *VenueTypes* popisný název tabulky toodisplay hello.

   ![Připojení k vzdálené a místní data](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Nyní vyberte hello *den, kdy byly hello většina lístků prodaných?* dotazu a stiskněte klávesu **F5**.

   Tento dotaz nemá trochu složitější spojování a agregaci. Co je důležité toonote je, že většina hello zpracování je provést vzdáleně a ještě jednou budeme navrácení pouze řádky hello, potřebujeme, vrácení pouze jeden řádek pro každý místo agregační lístku prodej počet za den.

   ![query](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Spouštění distribuovaných dotazů ve všech tenantských databázích
> * Nasazení databáze služby ad-hoc analýzy a přidejte schématu tooit toorun distribuované dotazy.


Nyní zkuste hello [klienta Analytics kurzu](sql-database-saas-tutorial-tenant-analytics.md) tooexplore extrahování dat tooa samostatné analytics databáze pro zpracování složitějších analýzy...

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavějí hello Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastic Query](sql-database-elastic-query-overview.md)
