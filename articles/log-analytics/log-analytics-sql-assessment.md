---
title: "aaaOptimize prostředí systému SQL Server s nástrojem Azure Log Analytics | Microsoft Docs"
description: "S Azure Log Analytics můžete hello vyhodnocení SQL řešení tooassess hello rizika a stavu prostředí serveru SQL v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f31326d8cdad3ef5d5a190614d1a18c1dac826ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-sql-server-environment-with-hello-sql-assessment-solution-in-log-analytics"></a>Optimalizace prostředí SQL serveru s hello řešení pro vyhodnocení SQL v analýzy protokolů

![Symbol vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

Můžete použít hello vyhodnocení SQL řešení tooassess hello stavu serveru prostředí a riziko v pravidelných intervalech. Tento článek vám pomůže nainstalovat řešení hello, takže můžete provést nápravné akce pro potenciální problémy.

Toto řešení poskytuje seznam doporučení konkrétní tooyour nasazená serverová infrastruktura seřazený podle priority. Hello doporučení jsou rozdělené mezi šesti fokus, které oblasti, které vám pomůžou rychle pochopit hello riziko a proveďte opravné akce.

Hello doporučení, která jsou založené na hello znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách. Každé doporučení obsahuje informace, proč problému, může vás tooyou a jak tooimplement hello navrhované změny.

Můžete vybrat konkrétní oblasti, které jsou nejdůležitější tooyour organizace a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.

Když jste přidali hello řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na hello **vyhodnocení SQL** řídicí panel pro hello infrastruktury ve vašem prostředí. Hello následující části popisují, jak toouse hello informace o hello **vyhodnocení SQL** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu serveru SQL.

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Vyhodnocení SQL pracuje s všechny aktuálně podporované verze systému SQL Server pro hello edice Standard, Developer a Enterprise.

Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

* Na serverech, které mají nainstalovaný server SQL Server je nutné nainstalovat agenty.
* Hello řešení pro vyhodnocení SQL vyžaduje podporovanou verzi rozhraní .NET Framework 4 nainstalován na každý počítač, který má OMS agent.
* V pořadí tooinstall hello řešení hello uživatel musí být správce nebo Přispěvatel toohello předplatné Azure, při použití hello portálu Azure. Kromě toho hello uživatel musí být členem skupiny hello OMS prostoru Přispěvatel správce rolí nebo na portálu OMS hello.
* Když pomocí agenta nástroje Operations Manager hello vyhodnocení SQL, budete potřebovat toouse účet Operations Manager Run-As. V tématu [účty nástroje Operations Manager spustit jako pro OMS](#operations-manager-run-as-accounts-for-oms) níže Další informace.

  > [!NOTE]
  > Hello MMA agent nepodporuje účty Operations Manager Run-As.
  >
  >
* Přidat hello vyhodnocení SQL řešení tooyour pracovním prostorem OMS pomocí procesu hello popsané v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Není nutná žádná další konfigurace.

> [!NOTE]
> Po přidání hello řešení, se přidá soubor AdvisorAssessment.exe hello tooservers s agenty. Konfigurační data je čtení a pak se odešle toohello OMS služba v cloudu hello ke zpracování. Logika je použité toohello přijatých dat a hello Cloudová služba zaznamenává hello data.

## <a name="sql-assessment-data-collection-details"></a>Podrobnosti kolekce dat vyhodnocení SQL
Vyhodnocení SQL shromažďuje data rozhraní WMI, registru dat, údaje o výkonu a výsledky zobrazení dynamické správy SQL Server pomocí hello agentů, které jste povolili.

Hello následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  | &#8226; |7 dní |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Účty spustit jako nástroje Operations Manager pro OMS
Analýzy protokolů v OMS používá hello agent nástroje Operations Manager a toocollect skupiny správy a posílat data toohello OMS služby. Sestavení OMS na sady management Pack pro úlohy tooprovide přidanou hodnotou services. Každé zatížení vyžaduje oprávnění konkrétních úloh toorun management Pack v odlišném kontextu zabezpečení, jako je například účet domény. Potřebujete přihlašovací údaje tooprovide nakonfigurováním Operations Manager účet Spustit jako.

Použijte následující informace tooset hello nástroje Operations Manager účet Spustit jako pro vyhodnocení SQL hello.

### <a name="set-hello-run-as-account-for-sql-assessment"></a>Nastavit hello spustit jako účet pro vyhodnocení SQL
 Pokud už používáte hello systému SQL Server management pack, používejte tento účet Spustit jako.

#### <a name="tooconfigure-hello-sql-run-as-account-in-hello-operations-console"></a>tooconfigure hello účet Spustit jako SQL v hello Operations console
> [!NOTE]
> Pokud používáte přímé agenta, nikoli agenta nástroje SCOM hello zprostředkovatele hello OMS, sada management pack hello vždy běží v kontextu zabezpečení hello hello místního systémového účtu. Přeskočit kroky 1 až 5 níže a spusťte buď hello T-SQL nebo prostředí Powershell ukázce zadání NT AUTHORITY\SYSTEM jako hello uživatelské jméno.
>
>

1. V nástroji Operations Manager, otevřete hello Operations console a pak klikněte na tlačítko **správy**.
2. V části **konfigurace spustit jako**, klikněte na tlačítko **profily**a otevřete **OMS SQL Assessment profilu spustit jako**.
3. Na hello **účty spustit jako** klikněte na tlačítko **přidat**.
4. Vyberte účet Spustit jako systému Windows, který obsahuje hello přihlašovacích údajů nezbytných pro systém SQL Server, nebo klikněte na **nový** toocreate jeden.

   > [!NOTE]
   > Hello typ účtu spustit jako musí být Windows. Hello účet Spustit jako musí být také součástí místní skupiny Administrators na všech serverech Windows, který je hostitelem instance serveru SQL.
   >
   >
5. Klikněte na **Uložit**.
6. Upravit a potom spusťte následující ukázka T-SQL v jednotlivých webu požadované tooRun pro instanci systému SQL Server toogrant minimální oprávnění jako účet tooperform vyhodnocení SQL hello. Ale nepotřebujete toodo to pokud účet Spustit jako už je součástí role serveru sysadmin hello na instance systému SQL Server.

```
---
    -- Replace <UserName> with hello actual user name being used as Run As Account.
    USE master

    -- Create login for hello user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions toouser.
    GRANT VIEW SERVER STATE too[<UserName>]
    GRANT VIEW ANY DEFINITION too[<UserName>]
    GRANT VIEW ANY DATABASE too[<UserName>]

    -- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
    -- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="tooconfigure-hello-sql-run-as-account-using-windows-powershell"></a>tooconfigure hello SQL spustit jako účtu pomocí prostředí Windows PowerShell
Otevřete okno prostředí PowerShell a spusťte následující skript po aktualizaci s vaše informace hello:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Pochopení, jak budou doporučení mít vyšší prioritu
Každé doporučení je uveden vyvážení hodnotu, která identifikuje hello relativní důležitost hello doporučení. Jsou zobrazeny pouze hello deset nejdůležitějších doporučení.

### <a name="how-weights-are-calculated"></a>Jak jsou vypočítávány váhu
Váhy jsou agregovaných hodnot založena na tři klíčové faktory:

* Hello *pravděpodobnosti* způsobí, že problém identifikovat problémy. Vyšší pravděpodobnost znamená zároveň tooa větší celkové skóre pro hello doporučení.
* Hello *dopad* hello problému na vaší organizaci, pokud ji způsobovat problémy. Vyšší dopad znamená zároveň tooa větší celkové skóre pro hello doporučení.
* Hello *úsilí* požadované tooimplement hello doporučení. Vyšší úsilí znamená zároveň tooa menší celkové skóre pro hello doporučení.

Hello vážení v případě každé doporučení je vyjádřený jako procentní podíl celkové skóre hello k dispozici pro každou oblast fokus. Například pokud doporučení v hello zabezpečení a dodržování předpisů oblastí zájmu je skóre % 5, implementace tímto doporučením zvýší celkové skóre podle 5 % zabezpečení a dodržování předpisů.

### <a name="focus-areas"></a>Konkrétní oblasti
**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.

**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.

**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení toohelp vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a je možné toorespond toochanging musí se infrastruktura.

**Upgrade, nasazení a migrace** – upgradu toohelp doporučení v tomto poli fokus zobrazí, migrace a nasazení systému SQL Server tooyour stávající infrastruktury.

**Operace a monitorování** – v tomto poli fokus zobrazí doporučení toohelp zjednodušit vaše IT oddělení, implementovat preventivní údržby a maximalizovat výkon.

**Správa změn a konfigurací** -doporučení v tomto poli fokus zobrazí toohelp chránit každodenních operací, zajistěte, aby změny nemáte negativně ovlivnit vaši infrastrukturu, vytvořte procedury řízení změn a tootrack a auditování Konfigurace systému.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>By odpovídajícím způsobem tooscore 100 % v každé oblasti fokus?
Ne nutně. Hello doporučení jsou založená na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a hello znalostní báze. Žádné serverové infrastruktury jsou však stejné hello a konkrétní doporučení může být vyšší nebo nižší relevantní tooyou. Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou zveřejněné toohello Internetu. Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav. Problémy, které jsou důležité tooa vyspělá firmy může být méně důležité tooa spuštění. Možná chcete tooidentify, které konkrétní oblasti jsou vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.

Každé doporučení obsahuje pokyny o tom, proč je důležité. Jestli implementace hello doporučení je vhodné pro vás, daná hello povaha IT služeb a hello obchodních potřeb vaší organizace, měli byste použít tento tooevaluate pokyny.

## <a name="use-assessment-focus-area-recommendations"></a>Použít assessment fokus oblasti doporučení
Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení hello. tooread Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Po instalaci, zobrazí se souhrn hello doporučení pomocí dlaždice vyhodnocení SQL hello na stránce Přehled hello v OMS.

Hello zobrazení souhrnu vyhodnocování dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>tooview doporučení pro výběr oblasti a proveďte opravnou akci
1. Na hello **přehled** klikněte na tlačítko hello **vyhodnocení SQL** dlaždici.
2. Na hello **vyhodnocení SQL** zkontrolujte hello souhrnné informace v jednom z hello fokus oblast okna a pak klikněte na jednu tooview doporučení pro tuto oblast fokus.
3. Na žádném z hello fokus oblast stránky můžete zobrazit hello nastavovat doporučení, která pro vaše prostředí. Klikněte na tlačítko doporučení v části **vliv na objekty** tooview podrobnosti, proč se provádí doporučení hello.  
    ![Obrázek doporučení vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Můžete provést nápravné akce navržený v **doporučované akce**. Při hello položky vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů. Opravené položky se zobrazí jako **předán objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení
Pokud máte doporučení, které chcete tooignore, můžete vytvořit textový soubor, který OMS použije tooprevent doporučení ze storu ve výsledky hodnocení.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-will-ignore"></a>tooidentify doporučení, které se budou ignorovat.
1. Přihlaste se tooyour prostoru a otevřete vyhledávání protokolu. Použití hello následující dotaz toolist doporučení, které selhaly pro počítače ve vašem prostředí.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Zde je snímek obrazovky znázorňující hello protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)
2. Zvolte doporučení, které chcete tooignore. Hodnoty hello budete používat pro RecommendationId v dalším postupu hello.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate a použít textový soubor s IgnoreRecommendations.txt
1. Vytvořte soubor s názvem IgnoreRecommendations.txt.
2. Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení má tooignore OMS na samostatném řádku a potom uložte a zavřete soubor hello.
3. Uložte soubor hello do následující složky na každém počítači, kam chcete OMS tooignore doporučení hello.
   * Na počítačích s hello agenta Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování
   * Na serveru pro správu nástroje Operations Manager hello - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify, že se ignorovat doporučení
1. Po spuštění hello další naplánované vyhodnocení ve výchozím nastavení každých 7 dní hello zadané doporučení jsou označeny ignorovaná a nezobrazí se na řídicí panel assessment hello.
2. Hello následující dotazy toolist hledání protokolů můžete použít všechny hello ignorovat doporučení.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Pokud se později rozhodnete, že chcete toosee ignorovat doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.

## <a name="sql-assessment-solution-faq"></a>Řešení pro vyhodnocení SQL – nejčastější dotazy
*Jak často posouzení spustit?*

* hodnocení Hello spouští každých 7 dní.

*Je k dispozici způsob tooconfigure jak často hello assessment běží?*

* V tuto chvíli to není možné.

*Pokud je zjištěno jiný server, poté, co byly přidány hello řešení pro vyhodnocení SQL, bude ho vyhodnocena?*

* Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.

*Pokud dojde k deaktivaci serveru, když ho se odebere z hello assessment?*

* Pokud server není odesílání dat 3 týdny, bude odebrán.

*Jaké je jméno hello hello procesu, který hello shromažďování dat?*

* AdvisorAssessment.exe

*Jak dlouho trvá toobe dat shromážděných?*

* kolekce Hello skutečná data na serveru hello trvá asi 1 hodina. Může trvat déle na serverech, které mají velký počet instancí SQL nebo databáze.

*Jaký typ dat shromažďovaných?*

* se shromažďují Hello následující typy dat:
  * ROZHRANÍ WMI
  * Registru
  * Čítače výkonu
  * Zobrazení dynamické správy SQL (DMV).

*Existuje způsob, jak tooconfigure když jsou shromažďována data?*

* V tuto chvíli to není možné.

*Proč musím tooconfigure účet Spustit jako?*

* Pro systém SQL Server se spouštějí na malý počet dotazů SQL. Aby se musí použít toorun, účet Spustit jako s tooSQL oprávnění VIEW SERVER STATE.  Kromě toho v pořadí tooquery rozhraní WMI, jsou vyžadovány přihlašovací údaje místního správce.

*Proč zobrazit pouze prvních 10 doporučení hello?*

* Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování hello nastavovat doporučení. Po jejich řešení, bude k dispozici další doporučení. Pokud dáváte přednost toosee hello podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů hello OMS.

*Existuje způsob, jak tooignore doporučení?*

* Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.

## <a name="next-steps"></a>Další kroky
* [V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné doporučení a údaje o vyhodnocení SQL.
