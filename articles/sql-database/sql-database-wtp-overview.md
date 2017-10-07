---
title: "aaaIntro Wingtip SaaS - víceklientské aplikace Azure SQL Database | Microsoft Docs"
description: "Další informace pomocí víceklientské ukázkovou aplikaci, která používá Azure SQL Database, hello Wingtip SaaS aplikace"
keywords: kurz k sql database
services: sql-database
author: stevestein
manager: jhubbard
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: daeed293116fca22718831b780533be6ef2ad178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-wingtip-saas-application"></a>Úvod toohello Wingtip SaaS aplikace

Hello *Wingtip SaaS* aplikace je víceklientské ukázkovou aplikaci, která demonstruje hello jedinečných výhod databáze SQL. aplikace Hello používá databázi za klienta, vzor aplikací SaaS, tooservice několik klientů. aplikace Hello je funkce navrženou tooshowcase databáze SQL Azure, které umožňují SaaS scénáře, včetně několika vzory návrhu a správu SaaS. tooquickly zprovoznění a spuštěna, hello Wingtip SaaS aplikace nasadí za méně než pět minut!

Aplikace zdrojový kód a správu skriptů jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. skripty hello toorun, [hello Learning moduly složka pro stahování](#download-and-unblock-the-wingtip-saas-scripts) tooyour místního počítače.

## <a name="sql-database-wingtip-saas-tutorials"></a>Kurzy SaaS Wingtip databáze SQL

Po nasazení aplikace hello, prozkoumejte hello následující kurzy, které stavějí hello počátečního nasazení. Tyto kurzy prozkoumejte běžných vzorů SaaS, které využít integrované funkce SQL Database, SQL Data Warehouse a jinými službami Azure. Kurzy zahrnout skripty prostředí PowerShell, s podrobné vysvětlení, které výrazně zjednodušit pochopení, a implementace hello stejné vzory správu SaaS ve svých aplikacích.


| Kurz | Popis |
|:--|:--|
|[Nasazení a prozkoumejte hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)| **ZAČNĚTE ZDE!** Nasazení a prozkoumejte hello Wingtip SaaS aplikace tooyour předplatného Azure. |
|[Zřizování a katalog klientů](sql-database-saas-tutorial-provision-and-catalog.md)| Zjistěte, jak se aplikace hello připojuje tootenants pomocí databáze katalogu a jak mapuje hello katalogu klienty tootheir data. |
|[Sledování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md)| Zjistěte, jak funkce monitorování toouse databáze SQL a jak tooset výstrahy, když jsou překročení prahové hodnoty výkonu. |
|[Monitorování s analýzy protokolů (OMS)](sql-database-saas-tutorial-log-analytics.md) | Další informace o použití [analýzy protokolů](../log-analytics/log-analytics-overview.md) toomonitor velké množství prostředků, napříč více fondů. |
|[Obnovení jednoho klienta](sql-database-saas-tutorial-restore-single-tenant.md)| Zjistěte, jak toorestore tooa klienta databáze, předchozí bod v čase. Kroky toorestore tooa paralelní databáze, výstupu hello existující klienta databázi do režimu online, jsou také zahrnuty. |
|[Správa schématu klienta](sql-database-saas-tutorial-schema-management.md)| Zjistěte, jak tooupdate schématu a aktualizaci referenční data, přes všechny klienty Wingtip SaaS. |
|[Spustit analytics ad-hoc](sql-database-saas-tutorial-adhoc-analytics.md) | Vytvoření databáze analýzy ad-hoc a spusťte v reálném čase distribuované dotazy mezi všechny klienty.  |
|[Spusťte klienta analytics](sql-database-saas-tutorial-tenant-analytics.md) | Extrahování dat klienta do služby analytics databáze nebo data warehouse ke spuštění v režimu offline analytické dotazy. |



## <a name="application-architecture"></a>Architektura aplikace

aplikace Wingtip SaaS Hello používá model databáze za klienta hello a efektivitu toomaximize elastické fondy SQL. Pro zřizování a mapování dat tootheir klientů, se používá databázi katalogu. Hello aplikace Wingtip SaaS jádra, používá fond s klienty tři ukázkové databáze katalogu hello navíc. Dokončení řadu hello Wingtip SaaS kurzy mít za následek doplňky toohello vyhledá nasazení se zavedením analytické databáze mezidatabázové Správa schématu, atd.


![Architektura Wingtip SaaS](media/sql-database-wtp-overview/app-architecture.png)


Při průchodu přes hello kurzy a práci s hello aplikace, je důležité toofocus hello SaaS vzory které se vztahují toohello datové vrstvy. Jinými slovy soustředit na datové vrstvě hello a nemáte přes analyzovat hello aplikace. Principy hello implementace tyto vzory SaaS je klíče tooimplementing tyto vzory ve svých aplikacích při zvažování veškeré nezbytné úpravy pro vaše konkrétní obchodní požadavky.

## <a name="download-and-unblock-hello-wingtip-saas-scripts"></a>Stáhněte si a odblokování hello Wingtip SaaS skriptů

Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené. Při extrahování hello skripty ze souboru zip, ***postupujte podle kroků hello v souboru ZIP hello toounblock před extrahování***. Tím se zajistí, že hello skriptů jsou povolené toorun.

1. Procházet příliš[úložiště github Wingtip SaaS hello](https://github.com/Microsoft/WingtipSaaS).
1. Klikněte na tlačítko **klonovat nebo stáhnout**.
1. Klikněte na tlačítko **stáhnout ZIP** a uložte soubor hello.
1. Klikněte pravým tlačítkem na hello **WingtipSaaS-master.zip** soubor a vyberte **vlastnosti**.
1. Na hello **Obecné** vyberte **Odblokovat**.
1. Klikněte na **OK**.
1. Extrahujte soubory hello.

Skripty, které jsou umístěné v hello *... \\WingtipSaaS hlavní\\Learning moduly* složky.


## <a name="working-with-hello-wingtip-saas-powershell-scripts"></a>Práce s hello skriptů prostředí PowerShell Wingtip SaaS

tooget hello nejvíce mimo hello ukázka potřebujete toodive do hello poskytuje skripty. Použití zarážek a krok prostřednictvím hello skriptů, prozkoumání hello podrobnosti o tom, jak jsou implementované hello různé vzorce SaaS. Krok tooeasily prostřednictvím hello poskytuje skriptech a modulech pro hello nejlépe porozumět, doporučujeme používat hello [prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-hello-configuration-file-for-your-deployment"></a>Aktualizovat hello konfigurační soubor pro vaše nasazení

Upravit hello **UserConfig.psm1** soubor s hello prostředků skupiny a uživatele hodnotu, která nastavíte během nasazení:

1. Otevřete hello *prostředí PowerShell ISE* a zatížení... \\Učení moduly\\*UserConfig.psm1* 
1. Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami hello pro vaše nasazení (na řádky 10 a 11 pouze).
1. Uložte změny hello!

Tyto hodnoty nastavení tady jednoduše nebude nutné tooupdate tyto hodnoty specifické pro nasazení v každé skriptu.

### <a name="execute-scripts-by-pressing-f5"></a>Provádění skriptů stisknutím klávesy F5

Používat několik skriptů *$PSScriptRoot* toonavigate složek, a *$PSScriptRoot* je Vyhodnocená jenom když jsou spouštět skripty, stisknutím klávesy **F5**.  Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.

### <a name="step-through-hello-scripts-tooexamine-hello-implementation"></a>Krok prostřednictvím hello skripty tooexamine hello implementace

Hello nejlepší způsob, jak toounderstand hello skripty je procházení je toosee co dělají. Podívejte se na hello zahrnuté **Demo -** skripty, které jsou k dispozici snadný toofollow zkratce. Hello **Demo -** skriptech hello kroky požadované tooaccomplish každý úkol, takže nastavit zarážky, a přejděte k podrobnostem hlubší jednotlivec hello vyžaduje podrobnosti implementace toosee hello různé vzorce SaaS.

Tipy pro zkoumání a procházení skriptů prostředí PowerShell:

* Otevřete **Demo -** skriptů v prostředí PowerShell ISE hello.
* Spustit nebo pokračovat s **F5** (pomocí **F8** se nedoporučuje, protože *$PSScriptRoot* při spuštění výběr skriptu, nebude hodnocen).
* Pokud chcete umístit zarážky, klikněte na řádek nebo ho vyberte a stiskněte klávesu **F9**.
* Funkci nebo volání skriptu můžete vynechat stisknutím klávesy **F10**.
* Na funkci nebo volání skriptu můžete přejít stisknutím klávesy **F11**.
* Krok mimo funkci current hello nebo skriptu pomocí volání **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Prozkoumání databázového schématu a spouštění dotazů SQL pomocí SSMS

Použití [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) tooconnect a procházení hello aplikačních serverů a databází.

nasazení Hello původně má dvě databáze SQL servery tooconnect příliš hello *tenants1 -&lt;uživatele&gt;*  server a hello *katalogu -&lt;uživatele&gt;* serveru. tooensure připojení úspěšné ukázku, mají oba servery [pravidlo brány firewall](sql-database-firewall-configure.md) povolení všechny IP adresy prostřednictvím.


1. Otevřete *SSMS* a připojte toohello *tenants1 -&lt;uživatele&gt;. database.windows.net* serveru.
1. Klikněte na **Připojit** > **Databázový stroj...**:

   ![katalogový server](media/sql-database-wtp-overview/connect.png)

1. Přihlašovací údaje pro ukázku jsou: Přihlašovací jméno = *developer*, Heslo = *P@ssword1*

   ![připojení](media\sql-database-wtp-overview\tenants1-connect.png)

1. Opakujte kroky 2 až 3 a připojte toohello *katalogu -&lt;uživatele&gt;. database.windows.net* serveru.

Po úspěšném připojení byste měli vidět oba servery. Seznam databází se může lišit v závislosti na hello klientům, kterou jste zřídili:

![průzkumník objektů](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Další kroky

[Nasazení aplikace Wingtip SaaS hello](sql-database-saas-tutorial.md)
