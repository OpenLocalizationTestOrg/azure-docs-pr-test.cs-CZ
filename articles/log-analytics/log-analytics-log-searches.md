---
title: "aaaFind dat pomocí protokolu hledání v Azure Log Analytics | Microsoft Docs"
description: "Protokol hledání umožňují toocombine a korelovat žádná data počítače z více zdrojů ve vašem prostředí."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: 1161857a0027f05726492417362cb24a8fe21ef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a>Najít data pomocí protokolu hledání v analýzy protokolů

>[!NOTE]
> Tento článek popisuje vyhledávání protokolu pomocí dotazovacího jazyka pro aktuální hello v analýzy protokolů.  Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak by měl odkazovat příliš[principy protokolu vyhledá v analýzy protokolů (Nový)](log-analytics-log-search-new.md).


Jádrem hello analýzy protokolů je hello protokolu vyhledávací funkce, která vám umožní toocombine a korelovat žádná data počítače z více zdrojů ve vašem prostředí. Řešení se také používá technologii toobring vyhledávání protokolu metriky můžete seskupit kolem oblasti konkrétní problém.

Na stránce hello vyhledávání můžete vytvořit dotaz a pak při hledání, můžete filtrovat výsledky hello pomocí ovládacích prvků omezující vlastnosti. Můžete také vytvořit tootransform pokročilými dotazy, filtr a sestavy na výsledky.

Běžné dotazy vyhledávání protokolu se zobrazí na většina stránek řešení. V rámci konzoly hello OMS můžete kliknutím na tlačítko dlaždice nebo přejít k podrobnostem tooother položky tooview podrobnosti o položce hello pomocí protokolu vyhledávání.

V tomto kurzu budeme zabývat příklady toocover všechny základy hello při použití hledání protokolů.

Jsme budete začínat jednoduchý, praktické příklady a potom stavět na je, abyste měli představu o případy praktická použití o toouse hello syntaxe tooextract hello Statistika způsob z dat hello.

Poté, co jste obeznámeni s vyhledávání techniky, můžete zkontrolovat hello [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Použití základní filtrů
Hello nejprve thing tooknow je, že hello první část dotazu vyhledávání, před spuštěním "|" znak svislé čáry znak, je vždy *filtru*. Můžete si ho představit jako klauzule WHERE v TSQL – Určuje *co* podmnožinu toopull dat z úložiště dat hello OMS. Hledání v úložišti dat hello je z velké části o zadání vlastnosti hello hello dat, který chcete tooextract, takže je přirozené dotazu by začínat hello klauzule WHERE.

Hello nejzákladnější filtry můžete použít jsou *klíčová slova*, jako je například "Chyba" nebo "časový limit nebo název počítače. Tyto typy dotazů jednoduché obecně vrátit různých tvarů dat v rámci hello stejná sada výsledků. Je to proto analýzy protokolů má jiný *typy* dat v systému hello.

### <a name="tooconduct-a-simple-search"></a>tooconduct jednoduché hledání
1. Na portálu OMS hello, klikněte na tlačítko **hledání protokolů**.  
    ![dlaždice hledání](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. V poli hello dotazů, zadejte `error` a pak klikněte na **vyhledávání**.  
    ![došlo k chybě hledání](./media/log-analytics-log-searches/oms-search-error.png)  
    Například hello dotazu pro `error` v hello následující obrázek vrátí 100 000 **událostí** záznamy (shromažďují Správa protokolu), 18 **ConfigurationAlert** záznamů (vygenerovaných konfigurace. Vyhodnocení) a 12 **ConfigurationChange** záznamy (zachycenou hello sledování změn).   
    ![výsledky hledání](./media/log-analytics-log-searches/oms-search-results01.png)  

Tyto filtry nejsou skutečně typy/tříd objektů. *Typ* je právě značku, vlastnost nebo řetězec nebo název nebo kategorii, která je připojena tooa část data. Některé dokumenty v systému hello jsou označené jako **typu: ConfigurationAlert** a některé jsou označené jako **typu: výkonu**, nebo **typu: událost**a tak dále. Každý výsledek hledání, dokument, záznamu nebo položka zobrazí všechny vlastnosti nezpracovaná hello a jejich hodnoty pro každou z těchto položek dat, a můžete použít tyto názvy toospecify pole ve filtru hello Pokud chcete pouze záznamy hello tooretrieve kde hello pole má, který přiřazen hodnota.

*Typ* je pouze pole, které mají všechny záznamy, není liší od jiné pole. To bylo vytvořeno na základě hodnoty hello hello typ pole. Tento záznam bude mít různých formě. Náhodně **typ = výkonu**, nebo **typ = událostí** je také hello syntaxe je třeba toolearn tooquery pro data výkonu nebo události.

Po název pole hello a před hello hodnotu, můžete použít dvojtečkou (:) nebo symbol rovná se (=). **Typ: událost** a **typ = událostí** odpovídají v význam, můžete zvolit hello styl dáváte přednost.

Ano, pokud hello zadejte = výkonu záznamy mají pole s názvem "název_čítače, a pak můžete napsat dotaz tvaru `Type=Perf CounterName="% Processor Time"`.

Tím získáte pouze údaje o výkonu hello kde název čítače výkonu hello je "% času procesoru".

### <a name="toosearch-for-processor-time-performance-data"></a>toosearch pro data výkonu času procesoru
* Do pole vyhledávání dotazu hello zadejte`Type=Perf CounterName="% Processor Time"`

Můžete také vybrat určité a použít **InstanceName = _ "Celkem"** v hello dotazu, který je čítačů výkonu systému Windows. Můžete také vybrat omezující vlastnost a jiné **: hodnota pole**. Filtr Hello je automaticky přidán filtr tooyour panelu hello dotazu. Uvidíte to v hello následující obrázek. Zobrazuje kde tooclick tooadd **InstanceName: "_Total"** toohello dotazu, aniž by museli zadávat nic.

![omezující vlastnost vyhledávání](./media/log-analytics-log-searches/oms-search-facet.png)

Stává dotazu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

V tomto příkladu nemáte toospecify **typ = výkonu** tooget toothis výsledek. Protože hello pole název_čítače a InstanceName existovat pouze pro záznamy typu = výkonu, hello dotazu je dost konkrétní tooreturn hello stejné výsledky jako hello déle, předchozí jeden:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Důvodem je, že všechny filtry hello v dotazu hello, jsou vyhodnoceny jako používán *a* mezi sebou. Prakticky hello další pole přidat toohello kritéria, můžete získat menší, konkrétní a přesnějších výsledků.

Například hello dotazu `Type=Event EventLog="Windows PowerShell"` je stejný jako příliš`Type=Event AND EventLog="Windows PowerShell"`. Vrátí všechny události, které byly přihlášení a shromažďují z protokolu událostí hello prostředí Windows PowerShell. Pokud filtr přidáte opakovaně výběrem hello stejné charakteristiky, pak problém hello je čistě kosmetické – ho může zaplnit panelu Hledat text hello, ale stále vrátí hello stejné výsledky protože implicitní operátor AND hello je vždycky k dispozici více než jednou.

Implicitní operátor AND hello můžete snadno vrátit explicitně pomocí operátoru NOT. Například:

`Type:Event NOT(EventLog:"Windows PowerShell")`nebo jeho ekvivalent `Type=Event EventLog!="Windows PowerShell"` vrátí všechny události z všechny protokoly, které nejsou protokolu hello prostředí Windows PowerShell.

Nebo můžete například použít jiné logický operátor 'Nebo'. Hello následující dotaz vrátí záznamy, pro které hello protokolu událostí buď systém nebo aplikace.

```
EventLog=Application OR EventLog=System
```

Pomocí hello výše dotazu, získáte položky pro oba protokoly v hello stejná sada výsledků.

Ale pokud odeberete hello nebo můžete nechat hello implicitní a na místě, pak hello následující dotaz nebude vytváří výsledky protože není k dispozici položku protokolu událostí, které patří tooBOTH protokoly. Každá položka protokolu událostí byla zapsána tooonly mezi dva protokoly hello.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Použít další filtry
Hello následující dotaz vrátí položky pro 2 protokoly událostí pro všechny počítače hello, které jste odeslali data.

```
EventLog=Application OR EventLog=System
```

![Výsledky vyhledávání](./media/log-analytics-log-searches/oms-search-results03.png)

Výběrem jedné z pole hello nebo filtry zúží hello dotazu tooa konkrétní počítač, s výjimkou všechny ostatní výsledky. Výsledný dotaz Hello by vypadat podobně jako následující hello.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Ekvivalentní toohello následující příkaz, který je z důvodu hello implicitní operátorem a.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Každý dotaz je vyhodnocován v hello následující explicitní pořadí. Poznámka: hello závorky.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Stejně jako pole hello protokolu událostí, je možné načíst data pouze pro sadu konkrétní počítače přidáním nebo. Například:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Podobně platí, tento hello následující dotaz vrátí **% času procesoru** pro hello vybrané pouze dva počítače.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a>Typy polí
Při vytváření filtrů, byste měli porozumět hello rozdíly při práci s různými typy pole vrácené protokolu hledání.

**Prohledatelná pole** zobrazit modře ve výsledcích hledání.  Prohledatelná pole můžete použít v poli konkrétní toohello podmínek vyhledávání například hello následující:

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

**Uvolněte prohledatelná pole text** jsou uvedeny šedě ve výsledcích hledání.  Je nelze použít s konkrétní toohello pole podmínky hledání jako prohledávatelné pole.  Jejich prohledávání pouze při provádění dotazu ve všech oblastech, jako je například následující hello.

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a>Logické operátory
Číselná pole a data a času, můžete vyhledat pomocí hodnoty *větší než*, *menší než*, a *menší než nebo rovna*. Jednoduché operátory můžete použít jako >, <>, =, < =,! = v panelu vyhledávání dotazu hello.

Pro určité časové období se můžete dotazovat specifickém protokolu událostí. Například hello je vyjádřen posledních 24 hodin s hello následující klávesovými výraz.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="toosearch-using-a-boolean-operator"></a>toosearch pomocí logický operátor
* Do pole vyhledávání dotazu hello zadejte`EventLog=System TimeGenerated>NOW-24HOURS`  
    ![hledání s logická hodnota](./media/log-analytics-log-searches/oms-search-boolean.png)

I když můžete řídit hello časový interval graficky a většinu doby můžete chtít toodo, že existují výhody tooincluding filtr času přímo do dotazu hello. Například to vyhovující postup pomocí řídicích panelů, kde můžete přepsat hello čas pro každou dlaždici, bez ohledu na to hello *globální* selektor čas na stránce řídicího panelu hello. Další informace najdete v tématu [čas záleží na řídicím panelu](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Při filtrování podle času, mějte na paměti, získat výsledky pro hello *průnik* z hello dva časových období: hello uvedené na portálu OMS hello (S1) a hello jeden zadaný v dotazu hello (S2).

![průnik](./media/log-analytics-log-searches/oms-search-intersection.png)

To znamená, pokud hello časových období nekříží, například na portálu OMS hello, kde si zvolíte **tato týdnu** a v dotazu hello, kde můžete definovat **minulého týdne**, nejsou k dispozici žádné průnik a vám nebude Zobrazí všechny výsledky.

Operátory porovnání použitý pro hello TimeGenerated pole jsou také užitečné v jiných situacích. Například s číselná pole.

Například uděleno, že konfigurace Assessment výstrahy mají hello následující hodnoty závažnosti:

* 0 = informace
* 1 = upozornění
* 2 = kritický

Můžete dotazovat na upozornění a kritickou výstrahy a také vyloučit informační ty, které jsou s hello následující dotaz:

```
Type=ConfigurationAlert  Severity>=1
```


Můžete také použít dotazy na rozsah. To znamená, že může poskytovat hello začátek a konec rozsahu hodnot v pořadí. Například pokud chcete zobrazit události z protokolu událostí nástroje Operations Manager hello kde hello ID události je větší než nebo rovna too2100, ale není větší než. 2199, pak hello následující dotaz vrátí je.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> Syntaxe rozsah Hello je nutné použít je oddělovače: hodnota pole hello dvojtečkou (:) a *není* hello rovná se (=). Uzavřete hello horní a dolní konec rozsahu hello do hranatých závorek a je oddělit dvě tečky (.).
>
>

## <a name="manipulate-search-results"></a>Manipulace s výsledky hledání
Pokud chcete vyhledat data, budete má toorefine vyhledávací dotaz a mít funkční úroveň kontroly nad hello výsledky. Pokud jsou načteny výsledků, můžete použít příkazy tootransform je.

Příkazy v analýzy protokolů hledání *musí* podle za znak hello znak svislé čáry (|). Filtr musí být vždy hello první část řetězce dotazu. Definuje hello datové sady, kterou právě pracujete s a následně "prostřednictvím kanálu předá" výsledků do příkazu. Pak můžete použít další příkazy tooadd hello kanálu. Toto je volně podobné toohello zřetězením příkazů Windows Powershellu.

Obecně platí, hello analýzy protokolů hledání jazyk pokusí toofollow prostředí PowerShell styl a pokyny toomake it podobné toohello IT Profesionálové a tooease hello křivku.

Příkazy mají názvy příkazů, takže lze snadno určit, co dělají.  

### <a name="sort"></a>Seřadit
příkaz Hello řazení vám umožní toodefine hello řazení order by – jeden nebo více polí. I když nepoužijete, ve výchozím nastavení, se vynucuje čas sestupném pořadí. výsledky nejnovější Hello jsou vždy v horní části hello výsledků vyhledávání. To znamená, že při spuštění vyhledávání, s `Type=Event EventID=1234` co je pro vás skutečně proveden je:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Je to způsobeno je hello typ možnosti, které jste se seznámili s v protokolech. Například v prohlížeči událostí systému Windows hello.

Můžete použít řazení toochange hello způsob výsledky se vrátí. Hello následující příklady ukazují, jak to funguje.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Hello jednoduché výše uvedených příkladech můžete fungování příkazy – se změní hello tvar hello výsledky, které hello filtr vrátil.

### <a name="limit-and-top"></a>Omezení a nahoře
Jiného méně známé příkazu je omezení. Limit je příkaz prostředí PowerShell jako. Limit je funkčně identické toohello HORNÍM příkazovém. Hello následující dotazy vrátí hello stejné výsledky.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="toosearch-using-top"></a>toosearch pomocí nahoře
* Do pole vyhledávání dotazu hello zadejte`Type=Event EventID=600 | Top 1`   
    ![horní vyhledávání](./media/log-analytics-log-searches/oms-search-top.png)

V obrázku hello výše, jsou 358 tisíc záznamy s ID události = 600. Hello polí, omezující vlastnosti a filtry na hello zbývajících vždy zobrazit informace o hello výsledky vrácené *pomocí filtru část hello* hello dotazu, která je součástí hello před všechny znakem. Hello **výsledky** podokně pouze vrátí výsledek hello poslední 1, protože hello Ukázkový příkaz ve tvaru a transformovat hello výsledky.

### <a name="select"></a>Vyberte
Vyberte příkaz Hello se chová jako Select-Object v prostředí PowerShell. Vrátí filtrované výsledky, které nemají všechny své původní vlastnosti. Místo toho vybere pouze hello vlastnosti, které zadáte.

#### <a name="toorun-a-search-using-hello-select-command"></a>toorun a vyhledávání pomocí příkazu select hello
1. Do pole hledání zadejte `Type=Event` a pak klikněte na **vyhledávání**.
2. Klikněte na tlačítko **+ zobrazit další** v jednom z tooview výsledky hello mají všechny vlastnosti hello hello výsledky.
3. Vyberte některým z nich explicitně a hello změny dotazů příliš`Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Vyberte vyhledávání](./media/log-analytics-log-searches/oms-search-select.png)

Tento příkaz je zvláště užitečné, když chcete toocontrol vyhledávání výstup a vybrat jenom hello části data, která skutečně vás pro vaše zkoumání, což často není úplný záznam hello. To je také užitečné, když mají záznamy různých typů *některé* společných vlastností, ale ne *všechny* jejich vlastnosti jsou společné. Můžete generovat výstup, který může vypadat více přirozeně tabulku nebo fungovat, i když exportovaný soubor CSV tooa a pak massaged v aplikaci Excel.

## <a name="use-hello-measure-command"></a>Použijte příkaz měr hello
MÍRA je jedním z hello nejvíce univerzální příkazy v analýzy protokolů hledání. Umožňuje vám tooapply statistické *funkce* tooyour dat a agregačních výsledků seskupených podle dané pole. Existuje více statistické funkce, které podporuje měr.

### <a name="measure-count"></a>Míra count()
Hello první toowork statistické funkce s a jedním z nejjednodušší toounderstand hello je hello *count()* funkce.

Výsledkem všechny vyhledávací dotaz, jako `Type=Event`, zobrazeny filtry na levé straně výsledků vyhledávání hello zkratka omezující vlastnosti. distribuce hodnoty podle dané pole hello výsledků zobrazí Hello filtry v hledání hello provést.

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Například v hello obrázku výše je zobrazí hello **počítače** pole a uvádí, že v rámci hello téměř 739 tisíc událostí ve výsledcích hello, jsou 68 jedinečné a jiné hodnoty pro hello **počítače** pole v těchto záznamech. dlaždice Hello se zobrazí pouze hello top 5, který jsou hello nejběžnější 5 hodnoty, které jsou napsané v hello **počítače** polí), seřazené podle hello počet dokumentů, které obsahují tuto konkrétní hodnotu v tomto poli. V bitové kopii hello uvidíte, že – mezi tyto události téměř 369 tisíc – 90 tisíc pocházet z hello OpsInsights04.contoso.com počítači 83 tisíc z hello DB03.contoso.com počítače a tak dále.

Co dělat, když chcete toosee všechny hodnoty, protože hello dlaždice se zobrazí pouze pouze hello top 5?

Jaké hello měr se příkaz můžete udělat pomocí funkce count() hello. Tato funkce nepoužívá žádné parametry. Můžete zadat pouze hello pole, podle kterého chcete pomocí – hello toogroup **počítače** pole v tomto případě:

`Type=Event | Measure count() by Computer`

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Ale **počítače** je právě používá pole *v* každá položka dat – nejsou spojené žádné relační databáze a neexistuje žádný samostatné **počítače** objektu kdekoli. Právě hello hodnoty *v* hello popíše dat generované které entitě je a řadu dalších vlastností a aspektů hello dat – proto hello termín *omezující vlastnost*. Však můžete stejně dobře seskupovat podle další pole. Protože hello původní výsledky téměř 739 tisíc události, které jsou přesměrovat do příkazu hello měr také obsahovat pole s názvem **EventID**, můžete použít hello stejným způsobem toogroup podle tohoto pole a získat počet událostí podle ID události:

```
Type=Event | Measure count() by EventID
```

Pokud si nejste zájem o počet hello skutečné záznamů, které obsahují konkrétní hodnotu, ale místo toho pokud chcete pouze seznam hello hodnoty sami, můžete přidat *vyberte* příkaz na konci hello ho a první sloupec právě vyberte hello:

```
Type=Event | Measure count() by EventID | Select EventID
```

Potom můžete získat komplikovanější a předem řazení výsledků hello v dotazu hello nebo klepnutí hello sloupce v mřížce hello příliš.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="toosearch-using-measure-count"></a>toosearch pomocí míru count
* Do pole vyhledávání dotazu hello zadejte`Type=Event | Measure count() by EventID`
* Připojit `| Select EventID` toohello konec hello dotazu.
* Nakonec připojit `| Sort EventID asc` toohello konec hello dotazu.

Existuje několik důležitých bodů toonotice a zdůraznil:

Nejprve hello výsledky, které vidíte nejsou původní nezpracovaná výsledky hello už. Místo toho jsou agregované výsledky – v podstatě skupiny výsledků. Tato akce není problém, ale byste měli vědět, že jste interakci se příliš neliší tvaru dat, která se liší od hello původní nezpracovaná tvar, který se vytvoří v chodu hello v důsledku hello agregace a statistické funkce.

Druhý, **měření počtu** aktuálně vrátí pouze hello nejvyšší 100 odlišné výsledky. Toto omezení neplatí toohello jiných statistických funkcí. Ano obvykle musíte toouse přesnější toosearch první filtr pro konkrétní položky před použitím count() měr.

## <a name="use-hello-max-and-min-functions-with-hello-measure-command"></a>Pomocí funkce max a min hello příkazem měr hello
Existují různé scénáře, kde **měr Max()** a **měr Min()** jsou užitečné. Ale vzhledem k tomu, že jednotlivé funkce je opačné vzájemně, jsme budete ilustrují Max() a můžete experimentovat s Min() sami.

Pokud jste odeslat dotaz pro události zabezpečení, mají **úroveň** vlastnost, která se může lišit. Například:

```
Type=SecurityEvent
```

![Počáteční počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Pokud chcete pro všechny hello zabezpečení tooview hello nejvyšší hodnotu události, které jsou uvedeny běžné počítače, hello Seskupit podle pole, můžete použít

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![hledání měr maximální počítače](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Se zobrazí, který pro hello počítačů, které obsahovaly **úroveň** záznamy, většina z nich má aspoň 8, na úrovni mnoho měl úroveň 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Maximální doba měr vyhledávání vygenerované počítači](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Tato funkce pracuje s čísla, ale spolupracuje také s pole data a času. Je užitečné toocheck pro hello poslední nebo poslední časové razítko pro jakékoliv dat indexované pro každý počítač. Například: Pokud nejnovější událostí zabezpečení hello ohlásil pro každý počítač?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-hello-avg-function-with-hello-measure-command"></a>Pomocí funkce AVG. hello příkazem měr hello
Hello Avg() použít s měr statistické funkce vám umožní toocalculate hello průměrnou hodnotu pro některé pole a skupiny výsledků podle hello stejné nebo jiné pole. To je užitečné v různých případech, například údaje o výkonu.

Začneme s údaje o výkonu. Všimněte si, že OMS aktuálně shromažďuje čítače výkonu pro počítače s Windows a Linux.

toosearch pro *všechny* údaje o výkonu, nejzákladnější dotaz hello je:

```
Type=Perf
```

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg01.png)

Hello první věcí, můžete si všimnout je, že analýzy protokolů ukazuje tři perspektivy: seznam, který ukazuje, který zobrazuje skutečné záznamy hello za hello grafy; Tabulky, který ukazuje tabulkového zobrazení data čítače výkonu; a metriky, které zobrazuje grafy pro hello čítače výkonu.

V obrázku hello výše existují dvě sady pole označené, které indikují hello následující:

* První sada Hello identifikuje název čítače výkonu systému Windows, název objektu a název Instance ve filtru dotazu hello. Jedná se o hello pole, které pravděpodobně budete nejčastěji používat jako omezující vlastnosti nebo filtry
* **Přepočtené** je hello skutečná hodnota čítače hello. V tomto příkladu je hodnota hello *75*.
* **TimeGenerated** je 12:51, ve 24hodinovém formátu.

Zde je zobrazení hello metriky v grafu.

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg02.png)

Po výklad o hello výkonu záznam tvar a nutnosti přečtěte si informace o jinými technikami, hledání můžete míry Avg() tooaggregate číselná data tohoto typu.

Zde je jednoduchý příklad:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![hledání samplevalue průměr](./media/log-analytics-log-searches/oms-search-avg03.png)

V tomto příkladu vyberte čítače výkonu hello celkový čas procesoru a průměr počítačem. Pokud chcete toonarrow dolů vaše výsledky tooonly hello posledních 6 hodin, můžete použít ovládací prvek filtru čas hello nebo zadat v dotazu následujícím způsobem:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="toosearch-using-hello-avg-function-with-hello-measure-command"></a>pomocí funkce AVG. hello hello měr příkaz toosearch
* Hello vyhledávacího dotazu pole, zadejte `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.

Můžete agregovat a chybě korelace dat. *napříč* počítače. Představte si například, že máte na skupinu hostitelů v nějaká farmy, kde každý uzel je rovna tooany jiného a stejně tak všechny hello přibližně rovnoměrně stejný typ pracovní a zatížení. Může dojít, jejich čítače, které vše v jednom přejděte s hello následující dotaz a získat průměry pro celou farmu hello. Můžete spustit výběrem hello počítače s hello následující ukázka:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Teď, když máte hello počítače, také pouze chcete tooselect dvě klíčové ukazatele výkonu (KPI): % využití procesoru a % volného místa na disku. Ano tuto část dotazu hello se změní na:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Teď můžete přidat počítače a čítače s hello následující ukázka:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Protože máte velmi specifických výběrových, hello **měření Avg()** příkaz může vrátit hello průměr není počítačem, ale napříč hello farmy, jednoduše tak, že seskupení podle název_čítače. Například:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

To vám dává užitečné compact zobrazení několik klíčových ukazatelů výkonu vaše prostředí.

![seskupení průměr vyhledávání](./media/log-analytics-log-searches/oms-search-avg04.png)

Na řídicím panelu můžete snadno použít hello vyhledávací dotaz. Můžete například uložit hello vyhledávací dotaz a vytvořit řídicí panel z něj s názvem *webové farmy klíčových ukazatelů výkonu*. toolearn Další informace o použití řídicích panelů, najdete v části [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md).

![řídicí panel vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-hello-sum-function-with-hello-measure-command"></a>Pomocí funkce sum hello příkazem měr hello
Funkce sum Hello je podobné funkce jako tooother hello měr příkazu. Můžete zobrazit příklad o jak toouse hello funkce sum na [W3C IIS protokoly vyhledávání ve statistice provozu Microsoft Azure](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Můžete použít Max() a Min() s textového řetězce, hodnoty data a času a čísla. Pomocí textového řetězce jsou seřazeny podle abecedy a získat první a poslední.

Sum() však nelze použít s jakoukoli jinou hodnotu než číselné pole. To platí také tooAvg().

### <a name="use-hello-percentile-function-with-hello-measure-command"></a>Pomocí funkce percentilu hello příkazem měr hello
Funkce percentilu Hello je podobné tooAvg() a Sum(), můžete ji použít pouze pro číselné pole. Můžete použít všechny percentilu mezi 1 too99 na číselné pole. Můžete také použít obě **percentilu** a **pct** příkazy. Zde je několik příkladů:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-hello-where-command"></a>Pokud je příkaz používejte hello
Dobrý den, kdy příkaz lze použít jako filtr, ale ji jde použít ve filtru toofurther kanálu hello agregován výsledky, které je tvořen příkazem měr – jako názvem na rozdíl od tooraw výsledky, které jsou filtrovány na začátku hello dotazu.

Například:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Můžete přidat další kanálu "|" znak a hello, kde získat příkaz tooonly počítače, jejichž průměrné využití procesoru je nad 80 %, s hello v následujícím příkladu:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Pokud jste obeznámeni s nástrojem Microsoft System Center - Operations Manager si můžete představit hello tam, kde příkazu v podmínkách management pack. Pravidlo kdyby hello příklad hello první část dotazu hello by hello zdroj dat a hello kde by příkaz vypadal detekce stavu hello.

Můžete použít hello dotazu jako dlaždici v **vlastní řídicí panel**, jako monitorování řazení, toosee po přetížené počítače procesory. toolearn Další informace o řídicím panelům, najdete v části [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md). Můžete také vytvořit a používat řídicí panely pomocí mobilní aplikace hello. Další informace najdete v tématu [OMS mobilní aplikace ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). V dlaždicích dolní dva hello hello následující bitové kopie, uvidíte hello monitorování zobrazí seznam, jako číslo. V podstatě vždy chcete hello číslo toobe nula a hello toobe seznam prázdný. Jinak hodnota označuje podmínku výstrahy. V případě potřeby ho můžete tootake, podívejte se na počítače, které jsou přetížena.

![mobilní řídicí panel](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-hello-in-operator"></a>Použití hello v operátoru
Hello *IN* operátor, spolu s *NOT IN* vám umožní toouse subsearches, které jsou hledání, které obsahují další hledání jako argument. Jsou obsaženy v složené závorky {} v rámci jiného *primární* nebo *vnější* vyhledávání. výsledek Hello subsearch, často seznam odlišné výsledky, se pak použije jako argument v jeho primární vyhledávání.

Můžete použít subsearches toomatch podmnožiny dat, nelze popisují přímo v hledaný výraz, ale který se dá vygenerovat z vyhledávání. Například, pokud vás zajímá pomocí jednoho vyhledávání toofind všechny události z *počítače s chybějícími aktualizacemi zabezpečení*, pak je nutné toodesign subsearch, který nejprve identifikuje, že *počítače s chybějícími aktualizacemi zabezpečení*  předtím, než nalezne události patřící toothose hostitele.

Ano, může express *počítače s aktuálně chybějícími požadovanými aktualizacemi zabezpečení* s hello následující dotaz:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Jakmile máte seznam hello, můžete použít vyhledávání hello jako vnitřní vyhledávání toofeed hello seznam počítačů, do vnější (primární) vyhledávání, který bude hledat události pro tyto počítače. Můžete to udělat obklopuje hello vnitřní vyhledávání do složených závorek a napájení své výsledky jako možné hodnoty nebo pole filtru v hello vnější vyhledávání pomocí operátoru IN hello. Hello dotazu by vypadat podobně jako:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Také čas hello upozornění filtru v informacích o vnitřní vyhledávání hello použít, protože hello vyhodnocení aktualizací systému pořídí snímek všech počítačů každých 24 hodin. Vnitřní dotaz hello můžete nastavit víc jednoduchý a přesné tak, že pouze jeden den. vnější vyhledávání Hello místo toho použije hello čas výběr hello uživatelské rozhraní, načítání událostí z hello posledních 7 dnů. V tématu [logické operátory](#boolean-operators) Další informace o času operátory.

Protože jste skutečně pouze výsledky hello použití hello vnitřní hledání jako hodnota filtru pro hello vnější jeden, můžete také použít příkazy ve vnější vyhledávání hello. Například můžete stále skupiny hello výše události s jiného příkazu měr:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in03-revised.png)

Obecně platí budete chtít vaší vnitřní dotaz tooexecute rychle protože analýzy protokolů má straně služby časové limity pro ho a také tooreturn malé množství výsledků. Pokud hello vnitřní dotaz vrátí více výsledků, získá zkrácen hello seznam výsledků, které by mohla způsobit hello vnější vyhledávání tooreturn nesprávné výsledky.

Jiné pravidlo je, že vnitřní hledání hello aktuálně musí tooprovide *agregované* výsledky. Jinými slovy, musí obsahovat *měr* příkaz; je nelze kanálu aktuálně nezpracovaná výsledky do vnějšího vyhledávání.

Navíc může být pouze jeden operátor IN a musí být hello poslední filtru v dotazu hello. Nemůže být více operátorů v nebo by – to v podstatě brání spuštění více subsearches: hello důležité bod je toto pouze jednu odebíraného nebo vnitřní hledání je možné pro každé vnější vyhledávání.

I když tyto limity umožňuje různé druhy korelační hledání v a můžete toodefine něco podobné toogroups třeba počítače, uživatele nebo soubory – ať hello pole ve vašich datech obsahovat. Zde jsou další příklady:

**Všechny aktualizace chybí z počítačů, kde je zakázaný nastavení automatických aktualizací**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Všechny chybové události z počítače se systémem SQL Server (=, kde se má spustit vyhodnocení SQL)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Všechny události zabezpečení z počítačů, které jsou řadiče domény se (=, kde byla spuštěna hodnocení AD)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Které jiné účty přihlášení toohello stejných počítačů, kde má přihlášený účet BACONLAND\jochan?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-hello-distinct-command"></a>Použijte příkaz distinct hello
Jako název hello naznačuje, tento příkaz poskytuje seznam jedinečných hodnot pro pole. Je překvapivě jednoduchý, ale velmi užitečné. Hello stejné bylo možné dosáhnout pomocí příkazu count() měr stejně, jako vidíte níže.

```
Type=Event | Measure count() by Computer
```

![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Ale pokud všechny, které vás zajímají právě seznam jedinečných hodnot a není hello počet dokumentů, které mají této hodnoty, pak DISTINCT může poskytovat čisticí a snadnější tooread výstup a kratší syntaxe, jak je uvedeno níže.

```
Type=Event | Distinct Computer
```
![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-hello-countdistinct-function-with-hello-measure-command"></a>Pomocí funkce countdistinct hello příkazem měr hello
Funkce countdistinct Hello počty hello počet jedinečných hodnot v rámci jednotlivých skupin. Například může být použit toocount hello počet jedinečných počítačů, vytváření sestav pro každý typ:

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-hello-measure-interval-command"></a>Pomocí příkazu interval měr hello
S téměř v reálném čase výkonu shromažďování dat, můžete shromažďovat a vizualizovat všechny čítače výkonu v analýzy protokolů. Jednoduše vstup dotazu hello **typu: výkonu** vrátí tisíce metriky grafy na základě počtu hello čítače a serverů ve vašem prostředí analýzy protokolů. S průběhem metriky na vyžádání, můžete se podívat na hello metriky celkového ve vašem prostředí na vysoké úrovni a podrobné informace na podrobnější data, jako je třeba.

Řekněme, že chcete tooknow co hello průměrné využití procesoru je pro všechny počítače. Prohlížení hello průměrné využití procesoru pro každý počítač nemusí být užitečné, protože může získat vyhlazené výsledky. toolook do další podrobnosti, můžete agregovat sady výsledků v menší časové okno bloky a podívejte se do časové řady mezi různými dimenzemi. Například můžete provádět hello hodinové průměr využití procesoru pro všechny počítače takto:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Průměrný interval měr](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Ve výchozím nastavení tyto výsledky se zobrazí v řádku interaktivního více řad grafu.  Tento graf podporuje řady přepnutím (s změny měřítka osy y), přibližování a ukazatele.  možnost zobrazení tabulky Hello je stále k dispozici pro prohlížení hello nezpracovaných dat v případě potřeby.

Můžete taky Seskupit podle jiných polí. V tomto příkladu dívám se na všech čítačů % hello jeden konkrétní počítač a chcete tooknow co je hello každou hodinu 70 percentily každých čítače:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Jednu věc toonote je, že tyto dotazy nejsou omezené tooperformance čítače. Můžete je použít tooany metriku. V tomto příkladu dívám se na protokoly služby IIS W3C. Chci tooknow, co je hello maximální doba, kterou trvá v intervalu 5 minut pro zpracování každé žádosti:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Použití více agregace v jednom dotazu
Více klauzulí agregační můžete zadat v příkazu měr.  Každé z nich může být alias nezávisle.  Pokud není zadána, hello alias výsledná hello agregační funkci, která byla použita bude název pole (tj. "avg(CounterValue)" pro avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Tady je další příklad:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Další kroky
Další informace o protokolu hledání najdete v tématu:

* Použití [vlastní pole v analýzy protokolů](log-analytics-custom-fields.md) tooextend protokolu hledání.
* Zkontrolujte hello [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md) tooview všechny hello vyhledávání polí a vlastností, které jsou k dispozici v analýzy protokolů.
