---
title: "schéma databáze SQL Azure aaaManage v víceklientské aplikace | Microsoft Docs"
description: "Správa schématu pro více tenantů v aplikaci s více tenanty využívající službu Azure SQL Database"
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
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: ea946e556808dabd60dd39cb8173d0512d4bddec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-schema-for-multiple-tenants-in-hello-wingtip-saas-application"></a>Správa schématu pro více klientů v hello Wingtip SaaS aplikace

Hello [první kurz Wingtip SaaS](sql-database-saas-tutorial.md) ukazuje, jak zřídit databázi klienta a zaregistrujte ho v katalogu hello aplikace hello. Podobně jako všechny aplikace hello Wingtip SaaS aplikace bude momentální v čase a v některých případech bude vyžadovat změny toohello databáze. Změny mohou zahrnovat nové nebo změněné schéma, nové nebo změněné referenčních dat a běžných databáze údržby úlohy tooensure optimální výkon. S aplikací SaaS třeba tyto změny toobe nasazený koordinovaným způsobem na potenciálně masivní firemního vozového klienta databází. Pro tyto změny toobe v budoucnosti klienta databáze, potřebují toobe součástí hello procesu zřizování.

Jsou zde popsány v tomto kurzu dva scénáře - nasazení aktualizací referenční data pro všechny klienty, a retuning indexu na hello tabulky obsahující hello referenční data. Hello [elastické úlohy](sql-database-elastic-jobs-overview.md) funkce je použité tooexecute těchto operací ve všech klientů a hello *zlaté* klienta databáze, která se používá jako šablonu pro nové databáze.

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Vytvoření účtu úlohy
> * Dotazování mezi několik klientů
> * Aktualizovat data ve všech databázích tenantů
> * Vytvořit index tabulky ve všech databázích tenantů


toocomplete splnění tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* je nainstalovaná nejnovější verze Hello systému SQL Server Management Studio (SSMS). [Stažení a instalace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Tento kurz používá funkce hello služby databáze SQL, které jsou v omezené preview (úlohy elastické databáze). Pokud chcete v tomto kurzu toodo, zadejte svoje id předplatného tooSaaSFeedback@microsoft.com s předmětem = elastické úlohy Preview. Po přijetí potvrzení, že je povoleno vašeho předplatného, [stáhnout a nainstalovat nejnovější rutiny předběžné verze úlohy hello](https://github.com/jaredmoo/azure-powershell/releases). Tato předběžná verze je omezená, takže obraťte se na SaaSFeedback@microsoft.com pro dotazy související s ani nepodporuje.*


## <a name="introduction-toosaas-schema-management-patterns"></a>Úvod tooSaaS vzory Správa schématu

Hello jednoho klienta na databázi SaaS vzor výhody v mnoha směrech z izolace hello data, která způsobí, že, ale v hello současně zavádí další složitosti hello údržbu a správu velkého počtu databází. [Elastické úlohy](sql-database-elastic-jobs-overview.md) usnadňuje administraci a správu hello SQL datové vrstvy. Úlohy umožňují toosecurely a spolehlivě, spouštět úlohy (skriptů T-SQL) nezávislé na vstupu, nebo interakci uživatelů pro skupinu databází. Tato metoda může být použité toodeploy schéma a běžných změn referenčních dat napříč všechny klienty v aplikaci. Elastické úlohy lze také použít toomaintain *zlaté* kopii databáze hello používá toocreate nové klienty, zajištění vždy obsahuje nejnovější schématu a referenčních dat hello.

![obrazovka](media/sql-database-saas-tutorial-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Elastic Jobs verze Limited Preview

K dispozici je nová verze služby Elastic Jobs. Jde o integrovanou funkci Azure SQL Database, která nevyžaduje další služby ani součásti. Tato nová verze služby Elastic Jobs je v současnosti ve verzi Limited Preview. Tato omezená preview aktuálně podporuje účty úlohy toocreate prostředí PowerShell a toocreate T-SQL a spravovat úlohy.

> [!NOTE]
> *Tento kurz používá funkce hello služby databáze SQL, které jsou v omezené preview (úlohy elastické databáze). Pokud chcete v tomto kurzu toodo, zadejte svoje id předplatného tooSaaSFeedback@microsoft.com s předmětem = elastické úlohy Preview. Po přijetí potvrzení, že je povoleno vašeho předplatného, [stáhnout a nainstalovat nejnovější rutiny předběžné verze úlohy hello](https://github.com/jaredmoo/azure-powershell/releases). Tato předběžná verze je omezená, takže obraťte se na SaaSFeedback@microsoft.com pro dotazy související s ani nepodporuje.*

## <a name="get-hello-wingtip-application-scripts"></a>Získat hello Wingtip aplikační skripty

Hello Wingtip SaaS skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Kroky toodownload hello Wingtip SaaS skripty](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-a-job-account-database-and-new-job-account"></a>Vytvoření databáze účtu úlohy a nového účtu úlohy

Tento kurz vyžaduje, že abyste použili prostředí PowerShell toocreate hello úlohy databáze a úlohy účtu. Jako databázi MSDB a agenta serveru SQL databáze elastické úlohy používá Azure SQL toostore definice úlohy, stav úlohy a historie. Po vytvoření účtu hello úlohy můžete vytvořit a monitorujte úlohy, okamžitě.

1. Otevřete... \\Learning moduly\\Správa schématu\\*ukázku SchemaManagement.ps1* v hello **prostředí PowerShell ISE**.
1. Stiskněte klávesu **F5** toorun hello skriptu.

Hello *ukázku SchemaManagement.ps1* hello volání skriptu *nasadit SchemaManagement.ps1* skriptu toocreate *S2* databáze s názvem **jobaccount** na server hello katalogu. Pak vytvoří účet hello úlohy, předávání hello jobaccount databáze jako parametr toohello úlohy účet vytvoření volání.

## <a name="create-a-job-toodeploy-new-reference-data-tooall-tenants"></a>Vytvořit úlohu toodeploy nový odkaz datové tooall klientů

Každá databáze klienta zahrnuje sadu místo typů, které definují hello druh události, které jsou hostované na místo. V tomto cvičení nasadit aktualizaci tooall hello klienta databází tooadd dva další místo typy: *Motorky Racing* a *křížovou kartou plaveckých*. Tyto typy místo odpovídají toohello obrázek pozadí, které se zobrazí v aplikaci události hello klienta.

Klikněte na tlačítko hello místo typu rozevírací nabídky a ověření, že jsou k dispozici jenom 10 možnosti Typ místo a konkrétně tento Motorky Racing a 'plaveckých křížovou kartou, nejsou zahrnuty v seznamu hello.

Nyní vytvoříme úlohy tooupdate hello *VenueTypes* tabulky v všechny databáze hello klienta a přidat nové typy místo hello.

toocreate novou úlohu používáme sadu úloh, které systém uložené procedury v hello jobaccount databázi vytvořena, když byl vytvořen účet projektu hello.

1. Otevřete aplikaci SSMS a připojit server katalogu toohello: katalogu -\<uživatele\>. database.windows.net serveru
1. Také připojit server klienta toohello: tenants1 -\<uživatele\>. database.windows.net
1. Procházet toohello *contosoconcerthall* databáze na hello *tenants1* serveru a dotaz hello *VenueTypes* tooconfirm tabulky, *Motorky Racing*  a *křížovou kartou plaveckých* **nejsou** v seznamu výsledků hello.
1. Otevřete hello souboru... \\Učení moduly\\Správa schématu\\DeployReferenceData.sql
1. Upravit příkaz hello: nastavte @wtpUser = &lt;uživatele&gt; a nahraďte hodnotu uživatele hello použít při nasazení aplikace Wingtip hello
1. Ujistěte se, jsou připojené toohello jobaccount databáze a stiskněte klávesu **F5** pro spuštění skriptu hello

* **SP\_přidat\_cíl\_skupiny** vytvoří hello cílová skupina DemoServerGroup, nyní potřebujeme tooadd cíl členy.
* **SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je hello tenants1-&lt; Uživatel&gt; server obsahující databáze klienta hello) v době úlohy spuštění by měl být součástí hello úlohy, je přidání druhý hello *databáze* cílové typ člena, konkrétně hello ("zlatá" databáze basetenantdb), se nachází v katalogu -&lt;uživatele&gt; serveru a nakonec jiné *databáze* cílové skupiny člena typu tooinclude hello adhocanalytics databázi, která se používá novější kurzu.
* **sp\_add\_job** vytvoří úlohu s názvem „Reference Data Deployment“.
* **SP\_přidat\_krok úlohy** vytvoří obsahující T-SQL příkazu text tooupdate hello referenční tabulku, VenueTypes krok úlohy hello
* Zbývající zobrazení Hello ve skriptu hello zobrazení hello existenci hello objekty a provádění úlohy monitorování. Tyto dotazy tooreview hello stav hodnotu použít v hello **životního cyklu** sloupec toodetermine při hello úloha úspěšně dokončí na všechny databáze klienta a hello dva další databáze, které obsahují hello referenční tabulku.

1. V aplikaci SSMS, procházet toohello *contosoconcerthall* databáze na hello *tenants1* serveru a dotaz hello *VenueTypes* tooconfirm tabulky, *Motorky Racing* a *křížovou kartou plaveckých* **jsou** nyní v seznamu výsledků hello.


## <a name="create-a-job-toomanage-hello-reference-table-index"></a>Vytvořte index úlohy toomanage hello Referenční tabulka

Podobné toohello předchozím cvičení, tento postup vytvoří indexem úlohy toorebuild hello na primární klíč tabulky hello odkaz, operace správy typické databáze Správce může provádět po zatížení velkých objemů dat do tabulky.

Vytvoření úlohy pomocí hello stejné úlohy "systém" uložené procedury.

1. Otevřete aplikaci SSMS a připojte toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru
1. Otevřete hello souboru... \\Učení moduly\\Správa schématu\\OnlineReindex.sql
1. Klikněte pravým tlačítkem, vyberte připojení a připojení toohello katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.
1. Zkontrolujte, jestli jsou připojené toohello jobaccount databáze a stiskněte klávesu F5 toorun hello skriptu

* sp\_add\_job vytvoří novou úlohu s názvem „Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885“.
* SP\_přidat\_krok úlohy vytvoří krok úlohy hello obsahující T-SQL příkazu text tooupdate hello indexu




## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Vytvořit účet tooquery úlohy v rámci více klientů
> * Aktualizovat data ve všech databázích tenantů
> * Vytvořit index tabulky ve všech databázích tenantů

[Kurz náhodné analýzy](sql-database-saas-tutorial-adhoc-analytics.md)


## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které stavějí hello nasazení Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Správa cloudových databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-overview.md)
* [Vytvoření a správa databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-create-and-manage.md)
