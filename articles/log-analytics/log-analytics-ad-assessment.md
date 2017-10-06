---
title: "aaaOptimize prostředí služby Active Directory s Azure Log Analytics | Microsoft Docs"
description: "Můžete použít hello Active Directory Assessment řešení tooassess hello stavu serveru prostředí a riziko v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 63290d95302a9e1d243cd993ac50556ed42b97bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-active-directory-environment-with-hello-active-directory-assessment-solution-in-log-analytics"></a>Optimalizace prostředí služby Active Directory s hello řešení pro vyhodnocení Active Directory v analýzy protokolů

![Symbol hodnocení AD](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Můžete použít hello Active Directory Assessment řešení tooassess hello stavu serveru prostředí a riziko v pravidelných intervalech. Tento článek vám pomůže nainstalovat a používat řešení hello, takže můžete provést nápravné akce pro potenciální problémy.

Toto řešení poskytuje seznam doporučení konkrétní tooyour nasazená serverová infrastruktura seřazený podle priority. Hello doporučení jsou rozdělené mezi čtyři fokus, které oblasti, které vám pomůžou rychle pochopit hello riziko a provést akci.

Hello doporučení jsou založená na hello znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách. Každé doporučení obsahuje informace, proč problému, může vás tooyou a jak tooimplement hello navrhované změny.

Můžete vybrat konkrétní oblasti, které jsou nejdůležitější tooyour organizace a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.

Když jste přidali hello řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na hello **hodnocení AD** řídicí panel pro hello infrastruktury ve vašem prostředí. Hello následující části popisují, jak toouse hello informace o hello **hodnocení AD** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro serverové infrastruktury služby Active Directory.

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-ad-assessment/ad-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

* Na řadičích domény, které jsou členy toobe domény hello vyhodnotit, je nutné nainstalovat agenty.
* Hello Active Directory Assessment řešení vyžaduje podporovanou verzi rozhraní .NET Framework 4 (4.5.2 nebo novější) nainstalován na každý počítač, který má OMS agent.
* Přidat pracovní prostor hello Active Directory Assessment řešení tooyour OMS z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

  > [!NOTE]
  > Po přidání hello řešení, se přidá soubor AdvisorAssessment.exe hello tooservers s agenty. Konfigurační data je čtení a pak se odešle toohello OMS služba v cloudu hello ke zpracování. Logika je použité toohello přijatých dat a hello Cloudová služba zaznamenává hello data.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Informace o kolekci datových Active Directory hodnocení

Active Directory Assessment shromažďuje data z následující zdroje pomocí hello agentů, které jste povolili hello:

- Kolektory registru
- Kolektory LDAP
- Rozhraní .NET framework
- Kolektory protokolů událostí
- Služba Active Directory rozhraní (ADSI)
- Windows PowerShell
- Kolekce dat souboru
- Windows Management Instrumentation (WMI)
- Nástroj DCDIAG rozhraní API
- Rozhraní API služby (NTFRS) replikace souborů
- Kód vlastní C#


Hello následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 dní |

## <a name="understanding-how-recommendations-are-prioritized"></a>Pochopení, jak budou doporučení mít vyšší prioritu
Každé doporučení je uveden vyvážení hodnotu, která identifikuje hello relativní důležitost hello doporučení. Jsou zobrazeny pouze hello 10 nejdůležitějších doporučení.

### <a name="how-weights-are-calculated"></a>Jak jsou vypočítávány váhu
Váhy jsou agregovaných hodnot založena na tři klíčové faktory:

* Hello *pravděpodobnosti* , způsobuje problémy, problém identifikovat. Vyšší pravděpodobnost znamená zároveň tooa větší celkové skóre pro hello doporučení.
* Hello *dopad* hello problému na vaší organizaci, pokud ji způsobovat problémy. Vyšší dopad znamená zároveň tooa větší celkové skóre pro hello doporučení.
* Hello *úsilí* požadované tooimplement hello doporučení. Vyšší úsilí znamená zároveň tooa menší celkové skóre pro hello doporučení.

Hello vážení v případě každé doporučení je vyjádřený jako procentní podíl celkové skóre hello k dispozici pro každou oblast fokus. Například pokud doporučení v hello zabezpečení a dodržování předpisů oblastí zájmu je skóre % 5, implementace tímto doporučením zvyšuje celkové skóre podle 5 % zabezpečení a dodržování předpisů.

### <a name="focus-areas"></a>Konkrétní oblasti
**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.

**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.

**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení toohelp vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a je možné toorespond toochanging musí se infrastruktura.

**Upgrade, nasazení a migrace** – v tomto poli fokus zobrazí doporučení toohelp upgradu, migrace a nasazení stávající infrastrukturu služby Active Directory tooyour.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>By odpovídajícím způsobem tooscore 100 % v každé oblasti fokus?
Ne nutně. Hello doporučení jsou založená na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a hello znalostní báze. Žádné serverové infrastruktury jsou však stejné hello a konkrétní doporučení může být vyšší nebo nižší relevantní tooyou. Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou zveřejněné toohello Internetu. Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav. Problémy, které jsou důležité tooa vyspělá firmy může být méně důležité tooa spuštění. Možná chcete tooidentify, které konkrétní oblasti jsou vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.

Každé doporučení obsahuje pokyny o tom, proč je důležité. Jestli implementace hello doporučení je vhodné pro vás, daná hello povaha IT služeb a hello obchodních potřeb vaší organizace, měli byste použít tento tooevaluate pokyny.

## <a name="use-assessment-focus-area-recommendations"></a>Použít assessment fokus oblasti doporučení
Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení hello. tooread Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Po instalaci, zobrazí se souhrn hello doporučení pomocí dlaždice Assessment hello na stránce Přehled hello v OMS.

Hello zobrazení souhrnu vyhodnocování dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>tooview doporučení pro výběr oblasti a proveďte opravnou akci
1. Na hello **přehled** klikněte na tlačítko hello **Assessment** dlaždici pro váš server infrastruktury.
2. Na hello **Assessment** zkontrolujte hello souhrnné informace v jednom z hello fokus oblast okna a pak klikněte na jednu tooview doporučení pro tuto oblast fokus.
3. Na žádném z hello fokus oblast stránky můžete zobrazit hello nastavovat doporučení, která pro vaše prostředí. Klikněte na tlačítko doporučení v části **vliv na objekty** tooview podrobnosti, proč se provádí doporučení hello.  
    ![Obrázek doporučení pro interní hodnocení](./media/log-analytics-ad-assessment/ad-focus.png)
4. Můžete provést nápravné akce navržený v **doporučované akce**. Když hello položky byla řešit, novější vyhodnocování záznamy, které doporučené akce provedené a zvýší vaše skóre dodržování předpisů. Opravené položky se zobrazí jako **předán objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení
Pokud máte doporučení, které chcete tooignore, můžete vytvořit textový soubor, který OMS použije tooprevent doporučení ze storu ve výsledky hodnocení.

### <a name="tooidentify-recommendations-that-you-will-ignore"></a>tooidentify doporučení, které se budou ignorovat.
1. Přihlaste se tooyour prostoru a otevřete vyhledávání protokolu. Použití hello následující dotaz toolist doporučení, které selhaly pro počítače ve vašem prostředí.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazu by změňte následující toohello.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Zde je snímek obrazovky znázorňující hello protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Zvolte doporučení, které chcete tooignore. Hodnoty hello budete používat pro RecommendationId v dalším postupu hello.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate a použít textový soubor s IgnoreRecommendations.txt
1. Vytvořte soubor s názvem IgnoreRecommendations.txt.
2. Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení má tooignore analýzy protokolů na samostatném řádku a potom uložte a zavřete soubor hello.
3. Uložte soubor hello do následující složky na každém počítači, kam chcete OMS tooignore doporučení hello.
   * Na počítačích s hello agenta Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování
   * Na serveru pro správu nástroje Operations Manager hello - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify, že se ignorovat doporučení
Po spuštění hello další naplánované vyhodnocení ve výchozím nastavení každých 7 dní hello zadaný doporučení jsou označeny *ignorovaná* a nezobrazí se na řídicím panelu assessment hello.

1. Hello následující dotazy toolist hledání protokolů můžete použít všechny hello ignorovat doporučení.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazu by změňte následující toohello.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Pokud se později rozhodnete, že chcete toosee ignorovat doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.

## <a name="ad-assessment-solutions-faq"></a>Řešení hodnocení AD – nejčastější dotazy
*Jak často posouzení spustit?*

* hodnocení Hello spouští každých 7 dní.

*Je k dispozici způsob tooconfigure jak často hello assessment běží?*

* V tuto chvíli to není možné.

*Pokud jiný server pro je zjištěno po byly přidány řešení pro vyhodnocení, bude ho vyhodnocena?*

* Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.

*Pokud dojde k deaktivaci serveru, když ho se odebere z hello assessment?*

* Pokud server není odesílání dat 3 týdny, bude odebrán.

*Jaké je jméno hello hello procesu, který hello shromažďování dat?*

* AdvisorAssessment.exe

*Jak dlouho trvá toobe dat shromážděných?*

* kolekce Hello skutečná data na serveru hello trvá asi 1 hodina. Může trvat déle na serverech, které mají velký počet servery služby Active Directory.

*Existuje způsob, jak tooconfigure když jsou shromažďována data?*

* V tuto chvíli to není možné.

*Proč zobrazit pouze prvních 10 doporučení hello?*

* Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování hello nastavovat doporučení. Po jejich řešení, bude k dispozici další doporučení. Pokud dáváte přednost toosee hello podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů.

*Existuje způsob, jak tooignore doporučení?*

* Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobné dat hodnocení AD a doporučení.
