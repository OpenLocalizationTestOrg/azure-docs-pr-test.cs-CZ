---
title: "aaaRestore Azure SQL database v aplikaci víceklientské | Microsoft Docs"
description: "Zjistěte, jak toorestore jedné klienty SQL databáze po omylem odstraňování dat"
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
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: 8507ecec2424c135f1859b88ebf2bb4e17538a58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-wingtip-saas-tenants-sql-database"></a>Obnovení databáze SQL Wingtip SaaS klientů

aplikace Wingtip SaaS Hello vytvořená s využitím modelu databáze za klienta, kde každý klient má své vlastní databázi. Jednou z výhod hello tohoto modelu je to je snadno toorestore data jednoho klienta v izolaci bez dopadu na ostatních klientů.

V tomto kurzu zjistíte, dva vzory obnovení dat:

> [!div class="checklist"]

> * Obnovení databáze do paralelní databáze (-souběžného)
> * Obnovení databáze na místě


|||
|:--|:--|
| **Klienta tooa předchozí bod v čase, obnovení do paralelní databáze** | Tento vzor mohou využívat klienta hello kontrolní, auditování, dodržování předpisů, atd. hello původní databáze zůstane online a beze změny. |
| **Obnovení klienta na místě** | Tento vzor je obvykle používanými toorecover na klienta tooa předchozí bod v čase, poté, co klient omylem odstraní data. Hello původní databáze je uveden do režimu offline a nahradit hello obnovené databáze. |
|||

toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-toohello-saas-tenant-restore-pattern"></a>Úvod toohello SaaS klienta obnovení vzor

Pro klienta hello obnovení vzor, existují dva jednoduché vzory pro obnovení dat jednoho klienta. Protože jsou od sebe navzájem oddělené klienta databáze, obnovení jednoho klienta nemá žádný vliv na data žádným jiným klientem na.

V prvním vzoru hello je obnovit data do nové databáze. Hello klienta potom bude mít přístup k databázi toothis spolu s jejich provozními daty. Tento vzor umožňuje klienta správce tooreview hello obnovit data a potenciálně použití tooselectively přepsat aktuální hodnoty data. Je to toohello SaaS aplikace návrháře toodetermine právě jak sofistikované hello obnovení dat, které by měl být možnosti. Všechno, co je potřeba v některých scénářích možná jednoduše je možné tooreview data v hello stavu, ve kterém se nacházel k danému bodu v čase. Pokud databáze hello používá [geografická replikace](sql-database-geo-replication-overview.md), doporučujeme, abyste kopírování dat vyžaduje hello z kopie hello obnovení do původní databáze hello. Pokud je původní databáze hello nahradit hello obnovit databázi, potřebujete tooreconfigure a poté proveďte opakovanou synchronizaci geografická replikace.

V druhém vzoru hello, která předpokládá, že hello klienta se vyskytla ztráty nebo poškození dat, hello klienta produkční databázi obnovit tooa předchozí bod v čase. V hello obnovit v místě vzoru hello klienta do režimu offline po krátkou dobu při hello databáze je obnovit a vrátí do režimu online. Hello původní databáze odstraněn, ale můžete pořád obnovit z Pokud potřebujete toogo back tooan i dřívějšího bodu v čase. Varianta tohoto vzoru přejmenovat databázi hello místo odstraněním, i když přejmenování databáze hello nabízí žádné další výhody z hlediska zabezpečení dat.

## <a name="get-hello-wingtip-application-scripts"></a>Získat hello Wingtip aplikační skripty

Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simulovat klienta omylem odstraňování dat

toodemonstrate tyto scénáře obnovení potřebujeme příliš*omylem* odstranit některá data v jedné z databází hello klienta. Když odstraníte všechny záznam, získat hello další krok nastaví hello ukázku toonot blokována porušení referenční integrity! Přidává také některé data nákupu lístku můžete použít později v hello *Wingtip SaaS Analytics kurzy*.

Spusťte skript generátor hello lístku a vytvořte další data. Generátor lístku Hello záměrně není zakoupit lístky pro všechny klienty poslední události.

1. Otevřete... \\Learning moduly\\nástroje\\*ukázku TicketGenerator.ps1* v hello *prostředí PowerShell ISE*
1. Stiskněte klávesu **F5** toorun hello skriptu a generovat zákazníků a lístek data prodeje.


### <a name="open-hello-events-app-tooreview-hello-current-events"></a>Otevřete hello události aplikace tooreview hello aktuální události

1. Otevřete hello *události rozbočovače* (http://events.wtp.&lt; Uživatel&gt;. trafficmanager.net) a klikněte na tlačítko **Contoso vzájemné součinnosti Hall**:

   ![centrum akcí](media/sql-database-saas-tutorial-restore-single-tenant/events-hub.png)

1. Posuňte se hello seznam událostí a poznamenejte si hello poslední událost v seznamu hello:

   ![poslední událost](media/sql-database-saas-tutorial-restore-single-tenant/last-event.png)


### <a name="run-hello-demo-scenario-tooaccidentally-delete-hello-last-event"></a>Spustit hello ukázkový scénář tooaccidentally odstranění hello poslední událost

1. Otevřete... \\Learning moduly\\kontinuity podnikových procesů a zotavení po havárii\\RestoreTenant\\*ukázku RestoreTenant.ps1* v hello *prostředí PowerShell ISE*, a sadu hello následující hodnotu:
   * **$DemoScenario** = **1**, nastavte příliš**1** – odstranit události s žádné prodeje lístků.
1. Stiskněte klávesu **F5** toorun hello skriptu a odstranit poslední událost hello. Měli byste vidět podobné toohello následující zpráva potvrzení:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Otevře se stránka události Contoso Hello. Přejděte dolů a ověřte, zda text hello událostí je pryč. Pokud je stále v seznamu hello hello událost, klikněte na tlačítko Aktualizovat a ověřte, zda že je pryč.

   ![poslední událost](media/sql-database-saas-tutorial-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-hello-production-database"></a>Obnovení databáze klienta paralelně s hello provozní databáze

Toto cvičení obnoví hello Contoso vzájemné součinnosti Hall databáze tooa bodu v čase před hello událost byla odstraněna. Po odstranění hello událostí v předchozích krocích hello chcete toorecover a zobrazit hello odstranit data. Nepotřebujete toorestore vaši produkční databázi s hello odstranit záznam, ale potřebujete toorecover hello staré databáze tooaccess hello stará data z jiných důvodů firmy.

 Hello *obnovení TenantInParallel.ps1* skript vytvoří paralelní klienta databáze a paralelní položky katalogu i s názvem *ContosoConcertHall\_staré*. Tento vzor obnovení je nejvhodnější pro obnovení před ztrátou dat menší nebo pro dodržování předpisů a auditování scénáře obnovení. Je také hello doporučenému přístupu při použití [geografická replikace](sql-database-geo-replication-overview.md).

1. Dokončení hello [simulovat uživatele nechtěně odstraní data](#simulate-a-tenant-accidentally-deleting-data) části.
1. Otevřete... \\Learning moduly\\kontinuity podnikových procesů a zotavení po havárii\\RestoreTenant\\_ukázku RestoreTenant.ps1_ v hello *prostředí PowerShell ISE*.
1. Nastavit **$DemoScenario** = **2**, nastavte příliš**2** příliš*obnovení klienta paralelně*.
1. Stiskněte klávesu **F5** toorun hello skriptu.

skript Hello obnoví hello klienta database (databáze paralelní tooa) tooa bodu v čase předtím, než jste odstranili hello událost v předchozí části hello. Vytvoří druhý databáze, odstraní všechny existující katalogu metadata, která existuje v této databázi a přidá hello databáze toohello katalogu pod hello *ContosoConcertHall\_staré* položku.

Ukázkový skript Hello otevře hello události stránku v prohlížeči. Poznámka: z adresy URL hello: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` , to se zobrazuje data z databáze hello obnovit kde *_old* je přidali toohello název.

Posuv hello události uvedené v prohlížeči tooconfirm hello které hello událostí v předchozí části hello odstranit byla obnovena.

Poznámka: Tento zpřístupňuje hello obnovit klienta, jako další klienta, s vlastní události aplikace toobrowse lístků, je pravděpodobně toobe, jak by zadejte, že data toorestored přístupu klienta, ale slouží tooeasily znázornění vzor obnovení hello.

Ve skutečnosti byste pravděpodobně pouze zachovat tato obnovená databáze za definované období. Po dokončení pomocí volání hello odstraníte záznam klienta hello obnovit *odebrat RestoredTenant.ps1* skriptu.

1. Nastavit **$DemoScenario** příliš**4** tooselect hello *klienta odebrat obnovit* scénář.
1. **Spuštění** **pomocí** **F5**
1. Hello *ContosoConcertHall\_staré* položka je nyní odstraněny z katalogu hello. Pokračujte a zavřete stránku hello události pro tohoto klienta v prohlížeči.


## <a name="restore-a-tenant-in-place-replacing-hello-existing-tenant-database"></a>Obnovení klienta v místě, nahraďte existující databázi klienta hello

Toto cvičení obnoví hello Contoso vzájemné součinnosti Hall klienta tooa bodu v čase před hello událost byla odstraněna. Hello *obnovení TenantInPlace* skriptu obnoví hello aktuální klienta tooa nové databáze odkazující tooa předchozího bodu v čase a odstranění hello původní databázi. Tento vzor obnovení je nejvhodnější pro obnovení z poškození závažné dat jako může být důležité datových ztrát hello klienta by mít tooaccommodate.

1. Otevřete **ukázku RestoreTenant.ps1** souboru v integrovaném Skriptovacím prostředí PowerShell
1. Nastavit **$DemoScenario** příliš**5** tooselect hello *obnovení klienta v případě místní*.
1. Spuštění pomocí **F5**.

skript Hello obnoví hello klienta databáze tooa bodu pět minut, než hello událost byla odstraněna. Dělá to pomocí prvního trvá hello Contoso vzájemné součinnosti Hall klienta do režimu offline, takže neexistují žádné další aktualizace toohello data. Pak je paralelní databáze vytvořené obnovení z bodu obnovení hello a s názvem s časovým razítkem název tooensure hello databáze není v konfliktu s hello název existující databáze klienta. V dalším kroku hello staré databáze klienta se odstraní a hello obnovené databáze je přejmenován toohello původní název databáze. Nakonec Contoso vzájemné součinnosti Hall uvedena do režimu online tooallow hello aplikace přístup toohello obnovit databázi.

Úspěšně obnovil hello databáze tooa bodu v čase před hello událost byla odstraněna. Dobrý den otevře stránka události, takže potvrďte byla obnovena poslední událost hello.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Obnovení databáze do paralelní databáze (-souběžného)
> * Obnovení databáze na místě

[Správa schématu databáze klienta](sql-database-saas-tutorial-schema-management.md)

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavějí hello Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Přehled kontinuity podnikových procesů s Azure SQL Database](sql-database-business-continuity.md)
* [Další informace o zálohování databáze SQL](sql-database-automated-backups.md)
