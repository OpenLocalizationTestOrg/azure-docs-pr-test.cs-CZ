---
title: "aaaOptimize prostředí System Center Operations Manager s nástrojem Azure Log Analytics | Microsoft Docs"
description: "Můžete použít hello System Center Operations Manager Assessment řešení tooassess hello stavu serveru prostředí a riziko v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c024e53826e91524c120bdb98ae7d96d6dc37d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-environment-with-hello-system-center-operations-manager-assessment-preview-solution"></a>Optimalizace prostředí s hello řešení System Center Operations Manager Assessment (Preview)

![System Center Operations Manager Assessment symbol](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

Můžete použít hello System Center Operations Manager Assessment řešení tooassess hello rizika a stavu prostředí serveru System Center Operations Manager v pravidelných intervalech. Tento článek vám umožňuje instalovat, konfigurovat a používat řešení hello, takže můžete provést nápravné akce pro potenciální problémy.

Toto řešení poskytuje seznam doporučení konkrétní tooyour nasazená serverová infrastruktura seřazený podle priority. Hello doporučení jsou rozdělené mezi čtyři fokus, které oblasti, které vám pomůžou rychle pochopit hello riziko a proveďte opravné akce.

Hello doporučení, která jsou založené na hello znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách. Každé doporučení obsahuje informace, proč problému, může vás tooyou a jak tooimplement hello navrhované změny.

Můžete vybrat konkrétní oblasti, které jsou nejdůležitější tooyour organizace a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.

Když jste přidali hello řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na hello **System Center Operations Manager Assessment** řídicí panel pro vaši infrastrukturu. Hello následující části popisují, jak toouse hello informace o hello **System Center Operations Manager Assessment** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu SCOM.

![Dlaždice řešení System Center Operations Manager](./media/log-analytics-scom-assessment/scom-tile.png)

![System Center Operations Manager Assessment řídicí panel](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello

řešení Hello funguje pro Microsoft System Operations Manager 2012 R2 a 2012 SP1.

Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

 - Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení hello. Instalaci hello řešení z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) nebo podle pokynů hello v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).

 - Po přidání prostoru toohello hello řešení, zobrazuje hello System Center Operations Manager Assessment dlaždice na řídicím panelu hello uvítací zprávu vyžaduje další konfiguraci. Klikněte na dlaždici hello a postupujte podle kroků konfigurace hello uvedený v stránku hello

 ![Dlaždice řídicího panelu System Center Operations Manager](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 Konfigurace hello System Center Operations Manager lze provést prostřednictvím hello skript podle následujících kroků hello v stránku hello konfigurace hello řešení v OMS.

 Místo toho tooconfigure hello assessment prostřednictvím konzoly SCOM, hello postupujte podle níže uvedené kroky v hello stejné pořadí
1. [Nastavit účet Spustit jako hello pro System Center Operations Manager hodnocení](#operations-manager-run-as-accounts-for-oms)  
2. [Konfigurace pravidla System Center Operations Manager Assessment hello](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>Informace o System Center Operations Manager assessment datových kolekce

Hello assessment System Center Operations Manager shromažďuje data rozhraní WMI, registru dat, data protokolu událostí, data nástroje Operations Manager prostřednictvím prostředí Windows PowerShell a dotazy SQL, souborů informace kolekci pomocí hello serveru, který jste povolili.

Hello následující tabulka uvádí metody shromažďování dat pro System Center Operations Manager hodnocení a jak často se data shromážděná pomocí agenta.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | | | | &#8226; | | sedm dní |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Účty spustit jako nástroje Operations Manager pro OMS

OMS staví na sady management Pack pro úlohy tooprovide přidanou hodnotou services. Každé zatížení vyžaduje oprávnění konkrétních úloh toorun management Pack v odlišném kontextu zabezpečení, jako je například účet domény. Konfigurace Operations Manager spustit jako účet tooprovide přihlašovacích údajů.

Použijte následující informace tooset hello účet Spustit jako nástroje Operations Manager pro System Center Operations Manager Assessment hello.

### <a name="set-hello-run-as-account"></a>Sada hello účet Spustit jako

1. V konzole nástroje Operations Manager hello, přejděte toohello **správy** kartě.
2. V části hello **konfigurace spustit jako**, klikněte na tlačítko **účty**.
3. Vytvořte účet Spustit jako, následující prostřednictvím hello Průvodce vytvoření účtu Windows hello. účet toouse Hello je hello jeden identifikovat, které mají všechny požadavky hello níže:

    >[!NOTE]
    Hello účet Spustit jako, musí splňovat následující požadavky:
    - Účet členem domény hello místní skupině Administrators na všech serverech v prostředí hello (všechny Operations Manager role - Server pro správu, databáze OpsMgr, datového skladu, vytváření sestav, webové konzole, brány)
    - Role správce Manager operace pro skupinu pro správu hello hodnotí
    - Spuštění hello [skriptu](#sql-script-to-grant-granular-permissions-to-the-run-as-account) toogrant oprávnění na podrobné úrovni toohello účtu v instanci SQL používaných nástrojem Operations Manager.
      Poznámka: Pokud tento účet již má oprávnění správce systému, potom přeskočte hello provádění skriptu.

4. V části **zabezpečení distribuce**, vyberte **bezpečnější**.
5. Zadejte server pro správu hello kde je hello účet distribuován.
3. Přejděte zpět toohello konfigurace spustit jako a klikněte na tlačítko **profily**.
4. Vyhledejte hello *SCOM Assessment profil*.
5. Název profilu Hello by měl být: *Microsoft System Center Advisor SCOM Assessment profilu spustit jako*.
6. Klikněte pravým tlačítkem myši a aktualizujte jeho vlastnosti a přidejte hello nedávno vytvořen účet Spustit jako jste vytvořili v kroku 3.

### <a name="sql-script-toogrant-granular-permissions-toohello-run-as-account"></a>SQL skriptu toogrant oprávnění na podrobné úrovni toohello účet Spustit jako

Spusťte následující skript toogrant požadované oprávnění toohello spustit jako účet služby SQL v instanci SQL hello používaných nástrojem Operations Manager hello.

```
-- Replace <UserName> with hello actual user name being used as Run As Account.
USE master

-- Create login for hello user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions toouser.
GRANT VIEW SERVER STATE too[UserName]
GRANT VIEW ANY DEFINITION too[UserName]
GRANT VIEW ANY DATABASE too[UserName]

-- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
-- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT too[UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace hello Operations Manager database name with hello one in your environment
Use [OperationsManager];
GRANT SELECT too[UserName]
GO

--Replace hello Operations Manager DatawareHouse database name with hello one in your environment
Use [OperationsManagerDW];
GRANT SELECT too[UserName]
GO

--Replace hello Operations Manager Audit Collection database name with hello one in your environment
Use [OperationsManagerAC];
GRANT SELECT too[UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace hello Operations Manager database name with hello one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-hello-assessment-rule"></a>Konfigurace pravidla assessment hello

Hello System Center Operations Manager Assessment sada řešení management pack obsahuje pravidla s názvem *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo*. Toto pravidlo je zodpovědná za spuštění hello hodnocení. tooenable hello pravidlo a nakonfigurovat četnost hello, použijte následující postupy hello.

Hello Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo je ve výchozím nastavení zakázaná. hodnocení hello toorun, je nutné povolit pravidlo hello na serveru pro správu. Pomocí následujících kroků hello.

#### <a name="enable-hello-rule-for-a-specific-management-server"></a>Povolení hello pravidla pro konkrétní management server

1. V hello **vytváření** prostoru hello Konzola nástroje Operations Manager, vyhledejte pravidlo hello *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo* v hello **pravidla** podokně.
2. Ve výsledcích hledání hello, vyberte text, které obsahuje hello textu hello *typu: Server pro správu*.
3. Klikněte pravým tlačítkem hello pravidlo a pak klikněte na **přepsání** > **pro konkrétní objekt třídy: Server pro správu**.
4.  V seznamu serverů pro správu k dispozici hello vyberte server pro správu hello kde hello pravidlo spustit.
5.  Ujistěte se, že změníte hodnotu přepsání příliš**True** pro hello **povoleno** hodnota parametru.  
    ![Přepište parametr](./media/log-analytics-scom-assessment/rule.png)

Během tohoto okna nakonfigurujte četnost hello hello spustí pomocí dalšího postupu hello.

#### <a name="configure-hello-run-frequency"></a>Nakonfigurovat četnost hello spustit

posouzení Hello je výchozí interval hello nakonfigurované toorun každých 10 080 minut (nebo sedm dní). Minimální hodnota tooa hello hodnota 1 440 minut (nebo jeden den), můžete přepsat. Hodnota Hello představuje minimální časové prodlevy hello požadované mezi následných vyhodnocení běží. toooverride hello interval, použijte následující postup hello.

1. V hello **vytváření** prostoru hello Konzola nástroje Operations Manager, vyhledejte pravidlo hello *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo* v hello **pravidla** podokně.
2. Ve výsledcích hledání hello, vyberte text, které obsahuje hello textu hello *typu: Server pro správu*.
3. Klikněte pravým tlačítkem hello pravidlo a pak klikněte na **hello přepsat pravidlo** > **pro všechny objekty třídy: Server pro správu**.
4. Změna hello **Interval** hodnota intervalu požadovaného tooyour hodnotu parametru. V příkladu hello níže nastavena hodnota hello too1440 minut (jeden den).  
    ![Parametr interval](./media/log-analytics-scom-assessment/interval.png)  
    Pokud tooless než 1 440 minut je nastavena hodnota hello, hello pravidlo spouští v intervalu jeden den. V tomto příkladu hello pravidlo ignoruje hodnota intervalu hello a spustí frekvencí jeden den.


## <a name="understanding-how-recommendations-are-prioritized"></a>Pochopení, jak budou doporučení mít vyšší prioritu

Každé doporučení je uveden vyvážení hodnotu, která identifikuje hello relativní důležitost hello doporučení. Jsou zobrazeny pouze hello 10 nejdůležitějších doporučení.

### <a name="how-weights-are-calculated"></a>Jak jsou vypočítávány váhu

Váhy jsou agregovaných hodnot založena na tři klíčové faktory:

- Hello *pravděpodobnosti* způsobí, že problém identifikovat problémy. Vyšší pravděpodobnost znamená zároveň tooa větší celkové skóre pro hello doporučení.
- Hello *dopad* hello problému na vaší organizaci, pokud ji způsobovat problémy. Vyšší dopad znamená zároveň tooa větší celkové skóre pro hello doporučení.
- Hello *úsilí* požadované tooimplement hello doporučení. Vyšší úsilí znamená zároveň tooa menší celkové skóre pro hello doporučení.

Hello vážení v případě každé doporučení je vyjádřený jako procentní podíl celkové skóre hello k dispozici pro každou oblast fokus. Například pokud doporučení v hello dostupnost a provozní kontinuita oblastí zájmu je skóre % 5, implementace tímto doporučením zvyšuje celkové skóre podle 5 % dostupnost a provozní kontinuita.

### <a name="focus-areas"></a>Konkrétní oblasti

**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.

**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení toohelp vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a je možné toorespond toochanging musí se infrastruktura.

**Nasazení, upgrade a migrace** – upgradu toohelp doporučení v tomto poli fokus zobrazí, migrace a nasazení systému SQL Server tooyour stávající infrastruktury.

**Operace a monitorování** – v tomto poli fokus zobrazí doporučení toohelp zjednodušit vaše IT oddělení, implementovat preventivní údržby a maximalizovat výkon.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>By odpovídajícím způsobem tooscore 100 % v každé oblasti fokus?

Ne nutně. Hello doporučení jsou založená na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a hello znalostní báze. Žádné serverové infrastruktury jsou však stejné hello a konkrétní doporučení může být vyšší nebo nižší relevantní tooyou. Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou zveřejněné toohello Internetu. Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav. Problémy, které jsou důležité tooa vyspělá firmy může být méně důležité tooa spuštění. Možná chcete tooidentify, které konkrétní oblasti jsou vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.

Každé doporučení obsahuje pokyny o tom, proč je důležité. Pomocí této tooevaluate pokyny, zda implementace hello doporučení je vhodné pro vás, daná hello povaha IT služeb a hello obchodních potřeb vaší organizace.

## <a name="use-assessment-focus-area-recommendations"></a>Použít assessment fokus oblasti doporučení

Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení hello. tooread Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Po instalaci, zobrazí se souhrn hello doporučení pomocí dlaždice System Center Operations Manager Assessment hello na stránce Přehled hello v OMS.

Hello zobrazení souhrnu vyhodnocování dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>tooview doporučení pro výběr oblasti a proveďte opravnou akci

1. Na hello **přehled** klikněte na tlačítko hello **System Center Operations Manager Assessment** dlaždici.
2. Na hello **System Center Operations Manager Assessment** zkontrolujte hello souhrnné informace v jednom z hello fokus oblast okna a pak klikněte na jednu tooview doporučení pro tuto oblast fokus.
3. Na žádném z hello fokus oblast stránky můžete zobrazit hello nastavovat doporučení, která pro vaše prostředí. Klikněte na tlačítko doporučení v části **vliv na objekty** tooview podrobnosti, proč se provádí doporučení hello.  
    ![oblastí zájmu](./media/log-analytics-scom-assessment/focus-area.png)
4. Můžete provést nápravné akce navržený v **doporučované akce**. Při hello položky vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů. Opravené položky se zobrazí jako **předán objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení

Pokud máte doporučení, které chcete tooignore, můžete vytvořit textový soubor, který používá OMS tooprevent doporučení ze storu ve výsledky hodnocení.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-want-tooignore"></a>tooidentify doporučení, které chcete tooignore

1. Přihlaste se tooyour prostoru a otevřete vyhledávání protokolu. Použití hello následující dotaz toolist doporučení, které selhaly pro počítače ve vašem prostředí.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Zde je snímek obrazovky znázorňující hello protokolu vyhledávací dotaz:  
    ![prohledávání protokolů](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Zvolte doporučení, které chcete tooignore. Hodnoty hello budete používat pro RecommendationId v dalším postupu hello.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate a použít textový soubor s IgnoreRecommendations.txt

1. Vytvořte soubor s názvem IgnoreRecommendations.txt.
2. Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení má tooignore OMS na samostatném řádku a potom uložte a zavřete soubor hello.
3. Uložte soubor hello do následující složky na každém počítači, kam chcete OMS tooignore doporučení hello.
4. Na serveru pro správu nástroje Operations Manager hello - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify, že se ignorovat doporučení

1. Po spuštění hello další naplánované vyhodnocení ve výchozím nastavení každých sedm dní, hello zadané doporučení jsou označeny ignorovaná a nezobrazí se na řídicí panel assessment hello.
2. Hello následující dotazy toolist hledání protokolů můžete použít všechny hello ignorovat doporučení.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Pokud se později rozhodnete, že chcete toosee ignorovat doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>Řešení System Center Operations Manager Assessment – nejčastější dotazy

*Po přidání hello assessment řešení toomy pracovním prostorem OMS. Ale hello doporučení se nezobrazí. Proč ne?* Po přidání hello řešení, použijte následující postup zobrazení hello doporučení na řídicím panelu OMS hello hello.  

- [Nastavit účet Spustit jako hello pro System Center Operations Manager hodnocení](#operations-manager-run-as-accounts-for-oms)  
- [Konfigurace pravidla System Center Operations Manager Assessment hello](#configure-the-assessment-rule)


*Je k dispozici způsob tooconfigure jak často hello assessment běží?* Ano. V tématu [hello konfigurace spustit frekvence](#configure-the-run-frequency).

*Pokud po byly přidány řešení System Center Operations Manager Assessment hello je zjistit jiný server, bude ho vyhodnocena?* Ano, po zjišťování, se bude posouzeno chvíle – ve výchozím nastavení, každých sedm dní.

*Jaké je jméno hello hello procesu, který hello shromažďování dat?* AdvisorAssessment.exe

*Kde hello AdvisorAssessment.exe proces spustit?* AdvisorAssessment.exe běží pod hello stavu serveru pro správu hello podporou hello assessment pravidlo. Pomocí tohoto procesu, zjišťování celé prostředí je dosaženo pomocí vzdálených dat kolekce.

*Jak dlouho trvá pro shromažďování dat?* Shromažďování dat na serveru hello trvá asi jednu hodinu. Může trvat déle v prostředích, která mají mnoho instancí nástroje Operations Manager nebo databáze.

*Co když nastavit interval hello hello assessment tooless než 1 440 minut?*  hello assessment je předem nakonfigurovaný toorun maximálně jednou za den. Pokud přepíšete hello interval hodnota tooa hodnotu menší než 1 440 minut, pak hello assessment používá jako hodnota intervalu hello 1 440 minut.

*Jak tooknow, pokud jsou předběžné selhání?* Pokud spustili hello hodnocení a se nezobrazí výsledky, je pravděpodobné, že některé hello předpoklady pro vyhodnocení hello se nezdařilo. Můžete provést dotazy: `Type=Operation Solution=SCOMAssessment` a `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` v hello toosee vyhledávání protokolu se nezdařilo předpoklady.

*Došlo `Failed tooconnect toohello SQL Instance (….).` zpráva předběžné selhání. Co je hello problém?* AdvisorAssessment.exe hello proces, který shromažďuje data, je spuštěn pod hello HealthService hello serveru pro správu. V rámci vyhodnocování hello se pokusí hello proces tooconnect toohello SQL Server, kde se nachází databáze nástroje Operations Manager hello. Této chybě může dojít, když pravidla brány firewall zablokovat připojení hello, toohello instance systému SQL Server.

*Jaký typ dat shromažďovaných?*  se shromažďují hello následující typy dat: - dat služby WMI – registru data – data protokolu událostí – Operations Manager data prostřednictvím prostředí Windows PowerShell, dotazy SQL a soubor informace kolekce.

*Proč musím tooconfigure účet Spustit jako?* Pro server Operations Manager se spouštějí různé dotazy SQL. Aby k nim toorun, musíte použít účet Spustit jako s potřebnými oprávněními. Kromě toho jsou přihlašovací údaje místního správce požadované tooquery rozhraní WMI.

*Proč zobrazit pouze prvních 10 doporučení hello?* Namísto udělení vyčerpávající, čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování hello nastavovat doporučení. Po jejich řešení, bude k dispozici další doporučení. Pokud dáváte přednost toosee hello podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů.

*Existuje způsob, jak tooignore doporučení?* Ano, najdete v části hello [ignorovat doporučení](#Ignore-recommendations).


## <a name="next-steps"></a>Další kroky

- [V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné doporučení a údaje o System Center Operations Manager hodnocení.
