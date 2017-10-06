---
title: "aaaUsing hello hledání protokolů portálu v Azure Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje kurz, který popisuje, jak toocreate protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů hello.  kurz Hello zahrnuje spouštění několik jednoduchých dotazů tooreturn různé typy dat a analýza výsledků."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 2e6633d548bb508edc0c650d11d2c32fc6ee536c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-hello-log-search-portal"></a>Vytvoření protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů hello

> [!NOTE]
> Tento článek popisuje hello hledání protokolů portálu v Azure Log Analytics pomocí dotazovacího jazyka pro nové hello.  Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).  
>
> Pokud pracovní prostor nebyla upgradovaná toohello nové dotazovací jazyk, se seznamte s příliš[najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů hello hello.

Tento článek obsahuje kurz, který popisuje, jak toocreate protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů hello.  kurz Hello zahrnuje spouštění několik jednoduchých dotazů tooreturn různé typy dat a analýza výsledků.  Zaměřuje se na funkce portálu hello hledání protokolů pro úpravy hello dotazu a nikoli změny přímo.  Podrobnosti o přímou úpravou dotazu hello, najdete v části hello [referenční příručka jazyka dotazů](https://go.microsoft.com/fwlink/?linkid=856079).

hledání toocreate portálu hello pokročilé analýzy místo hello hledání protokolů portálu, najdete v části [Začínáme s hello portálu analýza](https://go.microsoft.com/fwlink/?linkid=856587).  Oba Portály použít hello stejný dotaz jazyka tooaccess hello stejná data v pracovní prostor analýzy protokolů hello.

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že už máte pracovní prostor analýzy protokolů s alespoň jeden připojený zdroj, který generuje data pro dotazy tooanalyze hello.  

- Pokud nemáte pracovní prostor, můžete vytvořit volné jedním postupem hello v [začít pracovat s pracovní prostor analýzy protokolů](log-analytics-get-started.md).
- Připojit aspoň jeden [agenta Windows](log-analytics-windows-agents.md) nebo jednu [agenta systému Linux](log-analytics-linux-agents.md) toohello prostoru.  

## <a name="open-hello-log-search-portal"></a>Otevřete hello hledání protokolů portálu
Začněte otevřením portálu hledání protokolů hello.  Můžete k němu přístup v hello portál Azure nebo portálu OMS hello.

1. Otevřete hello portálu Azure.
2. Přejděte tooLog analýzy a vyberte pracovní prostor.
3. Vyberte buď **hledání protokolů** toostay v hello Azure portal nebo spusťte hello portálu OMS výběrem **portálu OMS** a potom kliknutím na tlačítko hledání protokolů hello.

![Tlačítko vyhledat protokolu](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Vytvoření jednoduché hledání
Hello nejrychlejší způsob, jak tooretrieve některé toowork data s je jednoduchý dotaz, který vrátí všechny záznamy v tabulce.  Pokud máte jakékoli systému Windows nebo Linux prostoru připojené tooyour klientů, budete mít data v buď hello událostí (Windows) nebo tabulka, Syslog (Linux).

Zadejte jeden hello následující dotazy hello vyhledávacího pole a klikněte na tlačítko Hledat hello.  

```
Event
```
```
Syslog
```

Data jsou vrácena v zobrazení seznamu výchozí hello a můžete zobrazit celkový počet záznamů vrácených.

![Jednoduchý dotaz](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Pouze hello první několik vlastností každý záznam se zobrazí.  Klikněte na tlačítko **zobrazit další** toodisplay všechny vlastnosti pro konkrétní záznam.

![Podrobnosti záznamu](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-hello-time-scope"></a>Nastavit obor čas hello
Každý záznam shromažďují analýzy protokolů má **TimeGenerated** vlastnost, která obsahuje hello datum a čas vytvoření tohoto záznamu hello.  Dotaz portálu hledání protokolů hello pouze vrátí záznamy s **TimeGenerated** v rámci oboru hello čas, který se zobrazí na levé straně obrazovky hello hello.  

Filtr času hello můžete změnit tak, že vyberete hello rozevírací nebo změnou hello posuvníku.  posuvník Hello Zobrazí pruhový graf, který ukazuje hello relativní počet záznamů pro každý segment čas v rozsahu hello.  Tento segment se bude lišit v závislosti na rozsahu hello.

Hello výchozí čas obor je **1 den**.  Tuto hodnotu změnit příliš**7 dní**, a celkový počet záznamů hello měli zvýšit.

![Datum čas oboru](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-hello-query"></a>Filtrování výsledků dotazu hello
Na hello je levé straně obrazovky hello hello podokno filtru, která vám umožní tooadd filtrování toohello dotazu bez změny přímo.  Zobrazí se několik vlastnosti hello záznamů vrácených s jejich top deset hodnoty s jejich počet záznamů.

Pokud pracujete s **událostí**, vyberte hello zaškrtávací políčko vedle příliš**chyba** pod **EVENTLEVELNAME**.   Pokud pracujete s **Syslog**, vyberte hello zaškrtávací políčko vedle příliš**chyba** pod **úroveň ZÁVAŽNOSTI**.  Tato operace změní hello dotazů tooone hello následující toolimit hello výsledků tooerror události.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Přidat vlastnosti toohello podokno filtru tak, že vyberete **přidat toofilters** z nabídky vlastnost hello na jednom hello záznamů.

![Přidat nabídku toofilter](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Můžete nastavit hello stejné filtrovat výběrem **filtru** nabídce hello vlastnost pro záznam s hodnotou hello chcete toofilter.  

Máte hello **filtru** možnost Vlastnosti s jejich název modře.  Jedná se o *prohledávatelné* pole, které jsou indexované pro podmínky vyhledávání.  Pole šedě jsou *volné text prohledávatelné* pole, které mají jenom hello **zobrazit odkazy** možnost.  Tato možnost vrátí záznamy, které mají tuto hodnotu v libovolné vlastnosti.

![Filtr nabídky](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Můžete seskupit hello výsledky na jedinou vlastnost výběrem hello **Seskupit podle** možnost v nabídce záznam hello.  Bude přidáno [shrnout](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) dotazu tooyour operátor, který zobrazí výsledky hello v grafu.  Můžete seskupit na více než jednu vlastnost, ale měli byste tooedit hello dotaz přímo.  Vyberte hello záznamů nabídky Další hello hello **počítače** vlastnost a vyberte **Seskupit podle "Počítač"**.  

![Seskupit podle počítače](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Práce s výsledky
portál hledání protokolů Hello obsahuje řadu funkcí pro práci s hello výsledků dotazu.  Můžete řadit, filtr a seskupení výsledků tooanalyze hello dat bez úpravy dotazu skutečné hello.  Ve výchozím nastavení nejsou seřazeny výsledků dotazu.

tooview hello dat v tabulce formulář, který nabízí další možnosti pro filtrování a řazení, klikněte na tlačítko **tabulky**.  

![Zobrazení tabulky](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Klikněte na šipku hello pomocí záznamu tooview hello podrobnosti pro tento záznam.

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Řazení v některém poli kliknutím na záhlaví sloupce.

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtrování výsledků hello na konkrétní hodnotu ve sloupci hello kliknutím na tlačítko Filtrovat hello a poskytnutím podmínku filtrování.

![Filtrování výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Skupiny na sloupci tak, že přetáhnete nahoře toohello záhlaví sloupce hello výsledků.  Můžete seskupit více polí přetažením horní toohello více sloupců.

![Výsledky skupiny](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Práce s data výkonu
Údaje o výkonu pro systém Windows a Linux agentů je uložen v prostoru analýzy protokolů hello hello **výkonu** tabulky.  Zaznamenává výkonu vypadají stejně jako jakýkoli jiný záznam a jsme může zapisovat jednoduchý dotaz, který vrátí že všechny záznamy výkonu stejně jako s událostmi.

```
Perf
```

![Údaje o výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Vrácení miliony záznamů pro všechny objekty výkonu a čítače, když není velmi užitečné.  Hello stejné metody, které můžete použít výše toofilter hello dat nebo jednoduše zadejte hello následující dotaz můžete použít přímo do vyhledávacího pole hello protokolu.  Tento příkaz vrátí jenom procesoru záznamů využití pro počítače se systémy Windows a Linux.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Využití procesoru](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Toto nastavení omezuje hello data tooa konkrétní čítače, ale je stále není pro něj formulář, který je obzvláště užitečné.  Můžete zobrazit hello data v spojnicový graf, ale nejprve toogroup ji tak, že počítač a TimeGenerated.  toogroup více polí, je nutné dotaz hello toomodify přímo, takže upravit následující toohello hello dotazu.  Tato služba využívá hello [průměr](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) funkce na hello **přepočtené** průměrnou hodnotu vlastnosti toocalculate hello přes každou hodinu.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data grafu výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Teď, když jsou seskupena vhodně hello data, můžete zobrazit jeho v visual graf přidáním hello [vykreslení](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Spojnicový graf](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Další kroky

- Další informace o hello analýzy protokolů dotazu jazyka v [Začínáme s hello portálu analýza](https://go.microsoft.com/fwlink/?linkid=856079).
- Provede kurz pomocí hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) který vám umožní toorun hello stejné dotazů a přístup hello stejná data jako portál hledání protokolů hello.
