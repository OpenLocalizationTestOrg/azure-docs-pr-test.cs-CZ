---
title: "aaaAzure analýzy protokolů hledání odkaz | Microsoft Docs"
description: "Hello analýzy protokolů hledání odkaz popisuje hello vyhledávání jazyk a poskytuje hello obecné dotazu syntaxe možnosti, které můžete použít při vyhledávání pro data a filtrování výrazy toohelp zužte své hledání."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7478a1139b88a1ce76ebb7b76027a6ccd66f4f27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-search-reference"></a>Referenční dokumentace vyhledávání analýzy protokolů

>[!NOTE]
> Tento článek popisuje vyhledávání protokolu pomocí dotazovacího jazyka pro aktuální hello v analýzy protokolů.  Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak by měl odkazovat příliš[hello referenční příručka jazyka pro nový jazyk hello](https://go.microsoft.com/fwlink/?linkid=856079).

Hello následujícím referenčním oddílu o vyhledávání jazyk popisuje hello obecné dotazu syntaxe možnosti, které můžete použít při vyhledávání pro data a filtrování výrazy toohelp zužte své hledání. Popisuje také příkazy, které můžete tootake akce na hello data načtená.

Další informace o hello pole, vrátí se v hledání a hello omezující vlastnosti, které vám pomohou zjistit informace o podobné kategorie dat do hello [pole hledání a omezující vlastnost odkazovat části](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Syntaxe dotazu obecné
Obecné dotazování Hello syntaxe vypadá takto:

```
filterExpression | command1 | command2 …
```

výraz filtru Hello (`filterExpression`) definuje hello "kde" podmínky pro hello dotazu. příkazy Hello použít toohello výsledků vrácených dotazem hello. Více příkazů musí být odděleny znakem hello (|).

### <a name="general-syntax-examples"></a>Příklady syntaxe obecné
Příklady:

```
system
```

Tento dotaz vrací výsledky, které obsahují hello word *systému* v každé pole, které pro fulltextové indexování nebo podmínek vyhledávání.

> [!NOTE]
> Ne všechna pole jsou indexované tímto způsobem, ale jsou nejběžnější textové pole (například názvy a popisy), obvykle hello.
>
>

```
system error
```

Tento dotaz vrací výsledky, které obsahují hello slova *systému* a *chyba*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Tento dotaz vrací výsledky, které obsahují hello slova *systému* a *chyba*. Potom seřadí hello výsledky podle hello *ManagementGroupName* pole (ve vzestupném pořadí) a potom podle hello *TimeGenerated* pole (v sestupném pořadí). Trvá hello pouze prvních 10 výsledky.

> [!IMPORTANT]
> Všechny hello názvy polí a hello hodnoty pro pole řetězce a textu hello jsou velká a malá písmena.
>
>

## <a name="filter-expressions"></a>Výrazy filtru
Hello následující témata popisují hello filtru výrazů.

### <a name="string-literals"></a>Textové literály
Řetězcový literál je řetězec, který nelze rozpoznat analyzátorem hello jako klíčové slovo nebo předdefinované datový typ (například číslo nebo datum).

Příklady:

```
These all are string literals
```

Tento dotaz vyhledává výsledky, které obsahují výskyty všechna pět slova. tooperform komplexní řetězec hledání, uzavřete hello řetězcový literál v uvozovkách. Například:

```
"Windows Server"
```

Tento příkaz vrátí jenom výsledky s přesné shody pro *systému Windows Server*.

### <a name="numbers"></a>Čísla
Hello analyzátor podporuje hello desítkové celé číslo a číslo s plovoucí desetinnou čárkou syntaxe pro číselné pole.

Příklady:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Data a časy
Každá část data v systému hello má *TimeGenerated* vlastnosti, která představuje hello původní datum a čas záznamu hello. Některé typy dat může mít další datum a čas pole (například *změněno*).

Časová osa Hello **graf a času** selektor v Azure Log Analytics ukazuje distribuční výsledků v čase (podle toohello aktuální dotaz spuštěn). To je založené na hello *TimeGenerated* pole. Datum a čas mají konkrétní řetězec formátu, který lze použít v dotazech toorestrict hello dotazu tooa určitý časový rámec. Můžete také použít syntaxi toorefer toorelative časové intervaly (například "mezi před 3 dny a 2 hodinami").

Hello následují platné formuláře syntaxe kalendářních dat a časů:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Například:

```
TimeGenerated:2013-10-01T12:20
```

Hello předchozí příkaz vrátí jenom záznamy se *TimeGenerated* hodnota přesně 12:20 na 1. října 2013.

Analyzátor Hello také podporuje hello klávesovými hodnotu data a času, teď. (Nepravděpodobné, že to předá výsledky, protože data doesn't make prostřednictvím systému hello to rychlé.)

Tyto příklady jsou toouse stavební bloky pro relativní a absolutní datum. V hello následující tři témata, uvidíte, jak toouse je v rozšířené filtry s příklady, které používají rozsahy relativní datum.

### <a name="datetime-math"></a>Matematické datum a čas
Použít hello datum a čas matematické operátory toooffset nebo zaokrouhlit hello hodnotě Datum a čas, a to pomocí jednoduché výpočty datum a čas.

Syntaxe:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operátor | Popis |
| --- | --- |
| / |Datum a čas toohello zaokrouhlí zadat jednotku. Například nyní / den zaokrouhlí hello aktuální datum a čas toomidnight Dobrý den aktuálního dne. |
| + nebo - |Posuny data a času podle hello zadat počet jednotek. Například nyní + 1 hodina posune hello aktuální datum a čas o jednu hodinu dopředu. 2013-10-01T12:00-10 dní posune hodnoty Date hello zpět o 10 dní. |

Matematické operátory hello datum a čas můžete zřetězené společně. Například:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Hello následující tabulka uvádí jednotky hello podporované datum a čas.

| Datum a čas jednotky | Popis |
| --- | --- |
| ROK, LET. |Zaokrouhlí toocurrent rok nebo posuny podle hello zadaný počet roků. |
| MĚSÍC, MĚSÍCŮ |Zaokrouhlí toocurrent měsíci, nebo posuny podle hello zadaný počet měsíců. |
| DEN, DNŮ, DATUM |Zaokrouhlí toocurrent den měsíce hello nebo posuny podle hello zadaný počet dnů. |
| HODINA, ČAS |Zaokrouhlí toocurrent hodinu nebo posuny podle hello zadaný počet hodin. |
| MINUTA, MINUT |Zaokrouhlí toocurrent minutu nebo posuny podle hello zadaný počet minut. |
| SECOND, SECONDS. |Zaokrouhlí toocurrent druhý nebo posune o hello zadat počet sekund. |
| MILISEKUND, POČET MILISEKUND, MILLI, MILLIS |Počet milisekund, po zadána zaokrouhlí toocurrent milisekundu nebo posuny podle hello. |

### <a name="field-facets"></a>Omezující vlastnosti pole
Pomocí pole omezující vlastnosti můžete zadat podmínky hello vyhledávání konkrétních polí a jejich přesné hodnoty. To se liší od zápis "bez textu" dotazů pro různé podmínky v celém indexu hello. Jste viděli již tato technika v několika předchozích příkladech. Hello Následují příklady složitější.

**Syntaxe**

```
field:value
```

```
field=value
```

**Popis**

Hledání hello pole pro konkrétní hodnotu hello. Hello hodnota může být řetězcový literál, číslo nebo datum a čas.

Například:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntaxe**

*pole > hodnota*

*pole < hodnota*

*pole > = hodnota*

*pole < = hodnota*

*pole! = hodnota*

**Popis**

Poskytuje porovnání.

Například:

```
TimeGenerated>NOW+2HOURS
```

**Syntaxe**

```
field:[from..to]
```

**Popis**

Poskytuje rozsah používání omezujících vlastností.

Například:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>V
Hello **IN** – klíčové slovo vám umožní tooselect ze seznamu hodnot. V závislosti na hello syntaxe, které používáte může se jednat jednoduchý seznam hodnot, které poskytnete, nebo seznam hodnot z agregace.

Syntaxe 1:

```
field IN {value1,value2,value3,...}
```

Tuto syntaxi umožňuje tooinclude všechny hodnoty v jednoduchých seznamů.



Příklady:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Syntaxe 2:

```
(Outer query) (Field toouse with inner query results) IN {Inner query | measure count() by (Field toosend tooouter query)} (rest  of outer query)  
```

Tuto syntaxi umožňuje toocreate agregace. Z tohoto agregace do jiné vnější search (primární), která vypadá pro události se tyto hodnoty můžete pak informačního kanálu hello seznamu hodnot. To provedete obklopuje hello vnitřní vyhledávání do složených závorek a napájení své výsledky jako možných hodnot pro pole v hello vnější vyhledávání pomocí operátoru IN hello.

Vnitřní dotaz příklad: *počítače aktuálně chybějící aktualizace zabezpečení* s hello následující agregace dotazu:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Hello poslední dotaz, který vyhledá *všechny události systému Windows pro počítače, které jsou aktuálně chybějící aktualizace zabezpečení* podobá hello následující:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
Hello **obsahuje** – klíčové slovo vám umožní toofilter pro záznamy s polem, které obsahuje zadaný řetězec. To je malá a velká písmena, lze použít pouze u polí s řetězcem a nesmí obsahovat žádné řídicí znaky.

Syntaxe:

```
field:contains("string")
```

Příklad:

```
Type:contains("Event")
```

Vrátí záznamy s typem, který obsahuje řetězec hello "Událost". Mezi příklady patří **událostí**, **SecurityEvent**, a **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Regulární výrazy
Můžete určit podmínku vyhledávání pro pole s regulárním výrazem, pomocí hello **Regex** – klíčové slovo. Úplný popis syntaxe hello můžete použít v regulárních výrazech naleznete v tématu [v analýzy protokolů pomocí regulárních výrazů toofilter protokolu hledání](log-analytics-log-searches-regex.md).

Syntaxe:

```
field:Regex("Regular Expression")
```

Příklad:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Logické operátory
dotaz Hello jazyky podporují hello logické operátory (*a*, *nebo*, a *není*) a jejich aliasy stylu jazyka C (*&&*,  *||* , a *!*, v uvedeném pořadí). Závorky toogroup můžete použít tyto operátory.

Příklady:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Logický operátor hello argumenty hello filtru nejvyšší úrovně, můžete vynechat. V takovém případě se předpokládá hello operátor AND.

| Výraz filtru | Ekvivalentní příliš|
| --- | --- |
| Systémová chyba |systém a chyb |
| systému "Windows Server" nebo závažnosti: 1 |systém a ("Windows Server" nebo závažnosti: 1) |

### <a name="wildcarding"></a>Použití zástupných znaků
Hello dotazovací jazyk podporuje používání hello ( \* ) znak příliš představují jeden nebo více znaků pro hodnotu v dotazu.

Příklad:

 Vyhledá všechny počítače s "SQL" v hello názvem, například "Redmond-SQL".

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> V tuto chvíli nelze použít zástupné znaky v rámci uvozovky. Například uvítací zprávu `"*This text*"` zvažuje hello (\*) použít jako literál (\*) znaků.


## <a name="commands"></a>Příkazy


příkazy Hello použít toohello výsledky, které jsou vrácených dotazem hello. Použití hello kanálu znak (|) tooapply toohello příkaz načíst výsledky. Více příkazů musí být odděleny znakem hello.

> [!NOTE]
> Názvy příkazů může být napsán v velká nebo malá písmena, na rozdíl od hello názvy polí a hello data.
>
>

### <a name="sort"></a>Seřadit
Syntaxe:

    sort field1 asc|desc, field2 asc|desc, …

Řazení výsledků hello podle určitého pole. Hello asc nebo desc příponu toosort hello výsledky ve vzestupném nebo sestupném pořadí je volitelný. Pokud je vynechaný, hello *asc* se předpokládá, že pořadí řazení. Pro hello **TimeGenerated** pole, *desc* pořadí řazení se předpokládá, tak, aby vracel nejnovější výsledky hello nejprve ve výchozím nastavení.

### <a name="toplimit"></a>Horní/Limit
Syntaxe:

    top number


    limit number
Omezení hello odpovědi toohello top N výsledky.

Příklad:

    Type:Alert errors detected | top 10

Vrátí hello top 10 odpovídajících výsledků.

### <a name="skip"></a>Přeskočit
Syntaxe:

    skip number

Přeskočí hello počet výsledků.

Příklad:

    Type:Alert errors detected | top 10 | skip 200

Vrátí top 10 odpovídající výsledky začínající na výsledek 200.

### <a name="select"></a>Vyberte
Syntaxe:

    select field1, field2, ...

Omezí výsledky toohello pole, které zvolíte.

Příklad:

    Type:Alert errors detected | select Name, Severity

Omezení hello vrácených výsledků pole příliš*název* a *závažnost*.

### <a name="measure"></a>Míra
Hello *měr* příkaz je použité tooapply statistických funkcí toohello nezpracovaná výsledků. To je velmi užitečná tooget *Seskupit podle* zobrazení nad daty hello. Při použití příkazu hello měr, analýzy protokolů hledání zobrazí tabulku s agregované výsledky.

**Syntaxe:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Agreguje hello výsledků podle *skupinové pole*a vypočítá hello agregaci hodnot měr pomocí *aggregatedField*.

| Míra statistické funkce | Popis |
| --- | --- |
| *Vlastnost aggregateFunction* |Název Hello hello agregační funkce (rozlišování malých a velkých písmen). jsou podporovány následující agregační funkce Hello: počet, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, PERCENTILU ## nebo PCT ## (## je jakékoli číslo mezi 1 a 99). |
| *aggregatedField* |Hello pole, které je agregaci. Toto pole je volitelné pro hello počet agregační funkce, ale má toobe existující číselné pole pro SUM, MAX, MIN, AVG, STDDEV, PERCENTILU ## nebo PCT ## (## je jakékoli číslo mezi 1 a 99). Některé z hello může být také Hello aggregatedField **rozšíření** podporované funkce. |
| *fieldAlias* |Hodnota aliasu pro hello vypočítat agregovat Hello (volitelné). Pokud není zadáno, je název pole hello **AggregatedValue**. |
| *skupinové pole* |Název Hello hello pole, který sada výsledků hello se seskupují po. |
| *Interval* |Hello časový interval, ve formátu hello:**nnnNAME**. **nnn**je hello kladné celé číslo. **NÁZEV** je název interval hello. Názvy podporovaný interval jsou velká a malá písmena a zahrnují: MILISEKUNDU [S], [S] SEKUNDU MINUTU [S], [S] HODINU dne [S], [S] měsíce a roku [S]. |

Hello interval možnost lze použít pouze v polích časových skupinu (například *TimeGenerated* a *TimeCreated*). V současné době to nevynucuje hello služby, ale pole bez datum a čas, který je předán toohello back-end způsobí, že chyba v běhu. Pokud se implementuje ověření schématu hello, rozhraní API služby hello odmítne dotazy, které používají pole bez datum a čas pro interval agregace. Hello aktuální *měr* implementace podporuje interval seskupení pro všechny agregační funkci.

Pokud je vynechaná klauzule BY hello, ale není zadaný interval (jako druhý syntaxe), hello *TimeGenerated* pole se předpokládá, že ve výchozím nastavení.

Příklady:

**Příklad 1**

    Type:Alert | measure count() as Count by ObjectId

Skupiny hello výstrahy podle *ObjectID*a vypočítá hello počet výstrah pro každou skupinu. Hello agregovaná hodnota vrácena jako hello *počet* pole (alias).

**Příklad 2**

    Type:Alert | measure count() interval 1HOUR

Skupiny hello výstrahy podle intervalu 1 hodin pomocí hello *TimeGenerated* pole a vrátí hello počet výstrah v každém intervalu.

**Příklad 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Stejné jako hello předchozí příklad, ale s aliasem agregované pole (*AlertsPerHour*).

**Příklad 4**

    * | míra count() podle 5DAYS TimeCreated intervalu

Výsledky hello seskupeny podle intervaly 5 dnů pomocí hello *TimeCreated* pole a vrátí hello počet výsledků v každém intervalu.

**Příklad 5**

    Type:Alert | measure max(Severity) by WorkflowName

Skupiny hello výstrahy podle názvu úlohy, a vrátí hello hodnotu maximální závažnost výstrahy pro každý pracovní postup.

**Příklad 6**

    Type:Alert | measure min(Severity) by WorkflowName

Stejné jako hello předchozí příklad, ale s hello *min* agregovat funkce.

**Příklad 7**

    Type:Perf | measure avg(CounterValue) by Computer

Skupiny výkonu počítačem a vypočítá průměr hello (průměr).

**Příklad 8**

    Type:Perf | measure sum(CounterValue) by Computer

Stejné jako hello předchozí příklad, ale používá *součet*.

**Příklad 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Stejné jako hello předchozí příklad, ale používá *stddev*.

**Příklad 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

Stejné jako hello předchozí příklad, ale používá *percentile70*.

**Příklad 11**

    Type:Perf | measure pct70(CounterValue) by Computer

Stejné jako hello předchozí příklad, ale používá *pct70*. Všimněte si, že *PCT ##* je pouze alias *PERCENTILU ##* funkce.

**Příklad 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

Skupiny výkonu nejdřív počítač a potom CounterName a vypočítá průměr hello (průměr).

**Příklad 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

Získá hello nejvyšší pět pracovních s hello maximální počet výstrah.

**Příklad 14**

    * | míra countdistinct(Computer) podle typu

Spočítá hello počet jedinečných počítačů, vytváření sestav pro každého typu.

**Příklad 15**

    * | míra countdistinct(Computer) intervalu 1 hodina

Spočítá hello počet jedinečných počítačů, vytváření sestav pro každou hodinu.

**Příklad 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

% Času procesoru počítačem skupiny a vrátí hello průměr pro každou hodinu.

**Příklad 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

Skupiny W3CIISLog metodou a vrátí hello maximální pro každých 5 minut.

**Příklad 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

Skupiny % času procesoru počítače a vrátí hello minimální, průměr, 75 percentilu a maximum pro každou hodinu.

**Příklad 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

Skupiny % času procesoru nejprve podle počítače a poté podle Instance název a vrátí hello minimální, průměrná, 75. percentil a maximální pro každou hodinu.

**Příklad 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Vypočítá hello maximálně zápisy na disk za minutu pro každý disk ve vašem počítači.

### <a name="where"></a>kde
Syntaxe:

```
**where** AggregatedValue>20
```

Lze použít pouze po *měr* příkaz toofurther filtru hello agregovat výsledky této hello *měr* má vytvořeného agregační funkce.

Příklady:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>Odstranění duplicitních dat
Syntaxe:

    Dedup FieldName

Vrátí první dokument hello najít pro každou jedinečnou hodnotu hello zadané pole.

Příklad:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

Tento příklad vrátí jedna událost (hello nejnovější událost) na ID události.

### <a name="join"></a>Spojit
Spojení hello výsledky dva dotazy tooform jedné sada výsledků.  Podporuje několik typů join popsané v hello podle tabulky.

| Typ spojení | Popis |
|:--|:--|
| vnitřní | Vrátí pouze záznamy s odpovídající hodnotou v obou dotazy. |
| vnější | Vrátí všechny záznamy z obou dotazů.  |
| Vlevo  | Vrátí všechny záznamy z levé dotazu a odpovídající záznamy z pravé dotazu. |


- Spojení aktuálně nepodporují dotazy, které zahrnují hello **IN** – klíčové slovo, hello **měr** příkaz nebo hello **rozšíření** příkaz, pokud je cílem pole pravé dotazu hello.
- Můžete zahrnout aktuálně pouze jednoho pole ke spojení.
- Hledání jednoduchého nesmí obsahovat více než jedno připojení.

**Syntaxe**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Příklady**

tooillustrate hello spojení různé typy, zvažte připojení typu dat shromážděných z vlastního protokolu volána MyBackup_CL s hello prezenčního signálu pro jednotlivé počítače.  Tyto datové typy mít hello následující data.

`Type = MyBackup_CL`

| TimeGenerated | Počítač | LastBackupStatus |
|:---|:---|:---|
| 4/20/2017 01:26:32.137 AM | Srv01.contoso.com | Úspěch |
| 4/20/2017 02:13:12.381 AM | SRV02.contoso.com | Úspěch |
| 4/20/2017 02:13:12.381 AM | srv03.contoso.com | Selhání |

`Type = Hearbeat`(Jenom podmnožinu zobrazená pole)

| TimeGenerated | Počítač | ComputerIP |
|:---|:---|:---|
| 4/21/2017 12:01:34.482 PM | Srv01.contoso.com | 10.10.100.1 |
| 4/21/2017 12:02:21.916 PM | SRV02.contoso.com | 10.10.100.2 |
| 4/21/2017 12:01:47.373 PM | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>vnitřní spojení

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

Vrátí hello následující záznamy, kde pole hello počítače odpovídá pro oba datové typy.

| Počítač| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Úspěch | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Prezenční signál |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Úspěch | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Prezenční signál |


#### <a name="outer-join"></a>vnější spojení

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

Vrátí hello následující záznamy pro oba datové typy.

| Počítač| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Úspěch  | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Prezenční signál |
| SRV02.contoso.com | 4/20/2017 02:14:12.381 AM | Úspěch  | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Prezenční signál |
| srv03.contoso.com | 4/20/2017 01:33:35.974 AM | Selhání  | 4/21/2017 12:01:47.373 PM | | |
| srv04.contoso.com |                           |          | 4/21/2017 12:01:47.373 PM | 10.10.100.2 | Prezenční signál |



#### <a name="left-join"></a>Levé spojení

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

Vrátí hello následující záznamy z MyBackup_CL se žádné odpovídající pole z prezenčního signálu.

| Počítač| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Úspěch | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Prezenční signál |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Úspěch | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Prezenční signál |
| srv03.contoso.com | 4/20/2017 02:13:12.381 AM | Selhání | | | |


### <a name="extend"></a>Rozšíření
Umožňuje vám toocreate běhu pole v dotazech. Všimněte si, že spuštění pole nelze používat s hello měr příkaz tooperform agregace.

**Příklad 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Ukazuje váha skóre doporučení pro doporučení vyhodnocení SQL.

**Příklad 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Zobrazí hodnotu čítače v články znalostní báze místo bajtů.

**Příklad 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Měřítka hello hodnotu WireData TotalBytes tak, aby všechny výsledky jsou v rozmezí od 0 do 100.

**Příklad 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Značky hodnoty čítače výkonu menší než 50 procent jako nízká, jiné jako Vysoká.

**Příklad 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Vypočítá hello maximálně zápisy na disk za minutu pro každý disk ve vašem počítači.

**Podporované funkce**

| Funkce | Popis | Příklady syntaxe |
| --- | --- | --- |
| Abs |Vrátí hello absolutní hodnotu hello zadaná hodnota nebo funkce. |`abs(x)` <br> `abs(-5)` |
| ACOS |Vrací kosinus oblouk hodnotu, nebo funkci. |`acos(x)` |
| a |Vrátí hodnotu true, pokud všechny jeho operandy vyhodnotit tootrue. |`and(not(exists(popularity)),exists(price))` |
| ASIN |Vrací sinus oblouk hodnotu, nebo funkci. |`asin(x)` |
| Atan |Vrací tangens oblouk hodnotu, nebo funkci. |`atan(x)` |
| ATAN2 |Vrací úhel hello vyplývající z hello převod hello obdélníkový souřadnice x, y toopolar souřadnice. |`atan2(x,y)` |
| cbrt – |Kořenové datové krychle. |`cbrt(x)` |
| ceil |Zaokrouhlí číslo nahoru tooan celé číslo. |`ceil(x)`  <br> `ceil(5.6)`Vrátí hodnotu 6 |
| Cos |Vrátí kosinus úhlu. |`cos(x)` |
| COSH |Vrací hyperbolický kosinus úhlu. |`cosh(x)` |
| DEF |Zkratka pro výchozí. Vrátí hello hodnotu pole "pole". Pokud hello pole neexistuje, vrátí hodnotu výchozí hello zadaný a vypočítá hello první hodnota kde: `exists()==true`. |`def(rating,5)`. Tato funkce def() vrátí hello hodnocení nebo pokud není zadán žádný hodnocení v dokumentu hello, vrátí 5. <br> `def(myfield, 1.0)`je ekvivalentní příliš`if(exists(myfield),myfield,1.0)`. |
| stupňů |Převede toodegrees radiánech. |`deg(x)` |
| div |`div(x,y)`rozdělí x, y. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| DIST |Vrátí hello vzdálenost mezi dvěma vektorů, (body) v n dimenzí místa. Přebírá hello power plus dva nebo více instancí ValueSource a vypočítá hello vzdálenosti mezi dvěma vektory hello. Každý ValueSource musí být číslo. Musí být sudé číslo instancí ValueSource předaná a hello metoda předpokládá, že hello první polovinu představují první vektoru hello a hello druhou polovinu představují druhý vektoru hello. |`dist(2, x, y, 0, 0)`Vypočítá hello Euclidean vzdálenost mezi (0,0) a (x, y) pro každý dokument. <br> `dist(1, x, y, 0, 0)`Vypočítá hello Manhattan (taxicab) vzdálenost mezi (0,0) a (x, y) pro každý dokument. <br> `dist(2,,x,y,z,0,0,0)`Euclidean vzdálenost mezi (0,0,0) a (x, y, z) pro každý dokument.<br>`dist(1,x,y,z,e,f,g)`Manhattan vzdálenost mezi (x, y, z) a (e, f, g), kde každý znak je název pole. |
| existuje |Vrátí hodnotu TRUE, pokud žádné členem hello pole existuje. |`exists(author)`Vrátí hodnotu TRUE pro každý dokument, který má hodnotu v poli "Autor" hello.<br>`exists(query(price:5.00))`Vrátí hodnotu TRUE, pokud "cena" odpovídá, "5.00". |
| Exp |Eulerova vrátí číslo vyvolá toopower x. |`exp(x)` |
| Floor |Zaokrouhlí číslo dolů tooan celé číslo. |`floor(x)`  <br> `floor(5.6)`Vrátí hodnotu 5 |
| hypo |Vrátí sqrt(sum(pow(x,2),pow(y,2))) bez zprostředkující přetečení nebo podtečení. |`hypo(x,y)`  <br> ` |
| Pokud |Umožňuje podmíněného funkce dotazy. V `if(test,value1,value2)`, test se nebo odkazuje tooa logická hodnota nebo výraz, který vrací logickou hodnotu (TRUE nebo FALSE). `value1`je hodnota hello vrácené funkcí hello Pokud test vypočítá hodnotu TRUE. `value2`je hodnota hello vrácené funkcí hello Pokud test vypočítá FALSE. Výraz může být žádné funkce, které výstupy logické hodnoty. Může být také funkce vracení číselných hodnot, ve kterých se interpretuje jako false hodnota 0, nebo vrácení řetězce, ve které případu prázdný řetězec interpretována jako false. |`if(termfreq(cat,'electronics'),popularity,42)`Tato funkce zkontroluje každý dokument toosee pokud obsahuje hello termín "electronics" v poli cat hello. Pokud, pak hello je vrácena hodnota pole oblíbenosti hello. Jinak je vrácena hodnota hello 42. |
| lineární |Implementuje `m*x+c`, kde m a c jsou konstanty a x je libovolný funkce. Jde o ekvivalent příliš`sum(product(m,x),c)`, ale poněkud efektivnější, jak jsou implementované jako jedinou funkci. |`linear(x,m,c) linear(x,2,4)`Vrátí`2*x+4` |
| ln |Vrátí hello přirozené protokolu hello zadat funkce. |`ln(x)` |
| Protokolu |Vrátí hello protokolu základní 10 hello zadané funkce. |`log(x)   log(sum(x,100))` |
| mapy |Mapuje hodnoty vstupní funkce x, které spadají do min a max, včetně toohello zadaný cíl. Hello argumenty min a max musí být konstanty. Hello argumenty cíle a výchozí může být konstanty nebo funkce. Pokud mezi minimální a maximální nespadá hello hodnotu x, pak je vrácena hodnota x buď hello nebo výchozí hodnota je vrácena v případě, že zadaný jako 5. argument. |`map(x,min,max,target) map(x,0,0,1)`Změní všechny hodnoty 0 too1. To může být užitečné při zpracování 0 výchozí hodnoty.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`Změní všechny hodnoty mezi 0 a 100 too1 a všechny ostatní hodnoty příliš-1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Změní hodnoty od 0 do 100 toox + 599 a všechny ostatní hodnoty toofrequency hello podmínek, solr, pole textu hello. |
| maximální počet |Vrátí hello maximální číselná hodnota více vnořené funkce nebo konstanty, které jsou zadané jako argumenty: `max(x,y,...)`. max – Funkce Hello také může být užitečná pro "bottoming out" jinou funkci nebo pole v některé zadaný konstanta.  Použití hello `field(myfield,max)` syntaxe pro výběr hello maximální hodnota je jediné pole s více hodnotami. |`max(myfield,myotherfield,0)` |
| min |Vrátí hello minimální číselnou hodnotu více vnořené funkce konstanty, které jsou zadané jako argumenty: `min(x,y,...)`. Funkce min Hello může být také užitečná při "horní mez" na funkce pomocí konstanta. Použití hello `field(myfield,min)` syntaxe pro výběr hello minimální hodnota jednoho pole s více hodnotami. |`min(myfield,myotherfield,0)` |
| MOD |Vypočítá numerického zbytku hello hello funkce x hello funkce y. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Vrátí rozdíl mezi její argumenty v milisekundách. Data jsou relativní toohello systémem Unix nebo POSIX epoch čas, půlnoc, 1. ledna 1970 UTC. Argumenty může být název hello indexované TrieDateField, nebo na základě konstantní data matematické datum nebo teď. `ms()`je ekvivalentní příliš`ms(NOW)`, počet milisekund od hello epoch. `ms(a)`Vrátí hello počet milisekund od hello epoch, který představuje hello argument. `ms(a,b)`Vrátí hello počet milisekund, po tomto b dojde před a, což je `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| není |Hodnota Hello logicky Negované hello zabalená funkce. |`not(exists(author))`Hodnota TRUE, pouze pokud `exists(author)` je false. |
| nebo |Logická disjunkce. |`or(value1,value2)`Hodnota TRUE, pokud buď value1 nebo value2 platí. |
| Pow |Vyvolá hello zadat základní toohello zadaný napájení. `pow(x,y)`Vyvolá x toohello power y. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`Hello stejné jako sqrt. |
| Produktu |Vrátí hello produktu více hodnot nebo funkce, které jsou určené v seznamu odděleném čárkami. `mul(...)`může také použít jako alias pro tuto funkci. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Provede vzájemných funkce s `recip(x,m,a,b)` implementace `a/(m*x+b)`, kde m, a, b jsou konstanty a x je žádné libovolně komplexní funkce. Když a b stejné a je x > = 0, tato funkce má maximální hodnota je 1, který zahodí jako x zvyšuje. Hello hodnotu zvýšit a b společně má za následek pohyb hello celý funkce tooa plošší součástí křivky hello. Tyto vlastnosti můžete vytvořit ideální funkce pro zvyšovat skóre novější dokumenty, když je x `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad |Převede tooradians stupňů. |`rad(x)` |
| Tisknout |Zaokrouhlí číslo toohello nejbližší celé číslo. |`rint(x)`  <br> `rint(5.6)`Vrátí hodnotu 6 |
| Sin |Vrátí sinus úhlu. |`sin(x)` |
| SINH |Vrací hyperbolický sinus úhlu. |`sinh(x)` |
| Škálování |Udává hodnoty hello funkce x, tak, aby se patří mezi hello zadané minTarget a maxTarget (včetně). aktuální implementace Hello prochází všechny hello funkce hodnoty tooobtain hello min a max, takže ho můžete vybrat hello správné škálování. aktuální implementace Hello nerozlišuje po odstranění dokumentů, nebo dokumenty, které nemají žádnou hodnotu. Použije 0.0 hodnoty pro tyto případy: To znamená, že pokud jsou obvykle všechny větší hodnotu než 0,0 hodnoty, jeden můžete nadále binárními 0,0 jako hello toomap minimální hodnota z. V těchto případech odpovídající `map()` funkce by se použil jako alternativní řešení toochange 0,0 tooa hodnotu v rozsahu skutečné hello, jak je vidět tady:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`Měřítka hello hodnoty x, tak, aby všechny hodnoty jsou mezi 1 a 2 (včetně). |
| Sqrt |Vrátí hello druhou odmocninu čísla hello zadaná hodnota nebo funkce. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Vypočítá hello vzdálenost mezi dva řetězce. Používá hello Lucene pravopisu kontrolu StringDistance rozhraní a podporuje všechny hello implementace, které jsou k dispozici v tomto balíčku. Kromě toho umožňuje tooplug aplikace ve své vlastní prostřednictvím prostředků na Solr možnosti načítání. provede strdist `(string1, string2, distance measure)`. Možné hodnoty pro míru vzdálenost jsou:<ul><li>jw: Jaro Winkler</li><li>Upravit: Levenstein nebo upravit vzdálenost</li><li>ngram: hello NGramDistance,-li zadána, můžete volitelně předávat hello ngram velikost příliš. Výchozí hodnota je 2.</li><li>FQN: Plně kvalifikovaný název pro implementaci rozhraní StringDistance hello – třída. Musí mít konstruktor bez arg.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub – |Vrátí x-y z `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Součet |Vrátí hello součet více hodnot nebo funkce, které jsou určené v seznamu odděleném čárkami. `add(...)`může se použít jako alias pro tuto funkci. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Vrátí číslo hello kolikrát hello termín se zobrazí v hello pole pro tento dokument. |termfreq(text,'memory') |
| Tan |Vrátí tangens úhlu. |`tan(x)` |
| TANH |Vrací hyperbolický tangens úhlu. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Odkaz na pole a omezující vlastnost vyhledávání
Při použití hledání protokolů toofind data zobrazit výsledky různé pole a omezující vlastnosti. Některé z informací hello se nemusí zobrazit velmi popisný. Použijte následující informace toohelp pochopit hello výsledky hello.

| Pole | Typ vyhledávání | Popis |
| --- | --- | --- |
| TenantId |Všechny |Použít toopartition data. |
| TimeGenerated |Všechny |Použít časové osy hello toodrive, timeselectors (ve vyhledávání a na dalších obrazovkách). Reprezentuje při generování hello část dat (obvykle na hello agent). čas Hello je vyjádřen ve formátu ISO a je vždycky UTC. V případě hello typy, které jsou založeny na existující instrumentace (to znamená, události v protokolu) je to obvykle hello reálném čas, že hello položku nebo řádek nebo záznam protokolu. Pro některé hello hello jiné typy, které vytváří prostřednictvím sad management Pack nebo v cloudu hello (například doporučení nebo výstrahy), doba představuje něco jiný. Toto je čas hello tato nová data se snímkem konfigurací nějaká nebyla shromážděna, nebo bylo vytvořeno ho podle doporučení nebo výstrahy. |
| ID události |Událost |ID události v protokolu událostí systému Windows hello. |
| Protokol událostí |Událost |Protokol událostí, kde hello událost byla zaznamenána v systému Windows. |
| EventLevelName |Událost |Kritický nebo upozornění nebo informace nebo úspěch |
| eventLevel |Událost |Číselnou hodnotu pro kritický nebo upozornění nebo informace nebo úspěch (použijte EventLevelName místo pro snazší/srozumitelnější dotazy). |
| SourceSystem |Všechny |Kde hello data pocházejí z (z hlediska připojení režimu toohello služby). Mezi příklady patří Microsoft System Center Operations Manager a Azure Storage. |
| Název objektu |PerfHourly |Název objektu výkonu systému Windows. |
| InstanceName |PerfHourly |Název instance čítače výkonu systému Windows. |
| CounteName |PerfHourly |Název čítače výkonu systému Windows. |
| ObjectDisplayName |PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject, |Zobrazovaný název objektu hello cílem pravidlo kolekce výkonu v nástroji Operations Manager. Mohou být také hello zobrazovaný název objektu hello zjištěných Statistika provozu nebo na které hello byla vygenerována výstraha. |
| RootObjectName |PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject, |Zobrazovaný název nadřazené hello hello nadřazeného objektu hello cílem pravidlo kolekce výkonu v nástroji Operations Manager (v dvojité hostitelský vztah). Mohou být také hello zobrazovaný název objektu hello zjištěných Statistika provozu nebo na které hello byla vygenerována výstraha. |
| Počítač |Většina typů |Název počítače, které hello dat patří. |
| Název zařízení |ProtectionStatus |Data hello název počítače patří příliš (stejné jako "Počítač"). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |Pořadí stav Threat je číselné vyjádření stavu threat hello. Kódy odpovědí podobné tooHTTP, hodnocení hello má mezery mezi hello čísla (proto žádné hrozeb je 150 a není 100 nebo 0), a místo tooadd nové stavy. Záměr hello souhrnu threat stav a stav ochrany, je, že tooshow hello nejhorší stav hello počítače byl v průběhu hello vybrané časové období. čísla Hello rank hello, můžete vyhledat hello záznam s nejvyšší číslo hello různých stavů. |
| ThreatStatus |ProtectionStatus |Popis ThreatStatus, mapování 1:1 s ThreatStatusRank. |
| TypeofProtection |ProtectionStatus |Antimalwarových produktu, který je zjištěn v počítači hello: none, nástroje pro odebrání Microsoft Malware, Forefront a tak dále. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus RequiredUpdate |ID služby stavu pro tento počítač agenta. |
| HealthServiceId |Většina typů |ID služby stavu pro tento počítač agenta. |
| ManagementGroupName |Většina typů |Název skupiny pro správu pro agenty nástroje Operations Manager připojit. Jinak má hodnotu null nebo prázdný. |
| objectType |ConfigurationObject |Zadejte pro tento objekt zjištěný assessment konfigurace analýzy protokolů (jako sada management pack Operations Manager typu nebo třídy). |
| UpdateTitle |RequiredUpdate |Název hello aktualizace, která byla nalezena není nainstalovaná. |
| PublishDate |RequiredUpdate |Pokud byla aktualizace hello publikována ve službě Microsoft Update. |
| Server |RequiredUpdate |Data hello název počítače patří příliš (stejné jako "Počítač"). |
| Produkt |RequiredUpdate |Produkt, který hello aktualizace se vztahuje na. |
| UpdateClassification |RequiredUpdate |Typ aktualizace (například kumulativní aktualizace nebo service pack). |
| KBID |RequiredUpdate |ID článku KB, který popisuje tento osvědčený postup nebo aktualizace. |
| WorkflowName |ConfigurationAlert |Název hello pravidlo nebo monitorování, které vytváří výstrahy hello. |
| Závažnost |ConfigurationAlert |Závažnost výstrahy hello. |
| Priorita |ConfigurationAlert |Priorita výstrahy hello. |
| IsMonitorAlert |ConfigurationAlert |Je tato výstraha vygeneruje, monitorování (true) nebo pravidlem (false)? |
| AlertParameters |ConfigurationAlert |Soubor XML s parametry hello hello analýzy protokolů výstrahy. |
| Kontext |ConfigurationAlert |Soubor XML s kontextem hello hello analýzy protokolů výstrahy. |
| Úloha |ConfigurationAlert |Označuje technologie nebo úlohy, které hello výstrahy. |
| AdvisorWorkload |Doporučení |Označuje technologie nebo úlohy, které hello doporučení. |
| Popis |ConfigurationAlert |Popis výstrahy (krátký). |
| DaysSinceLastUpdate |UpdateAgent |Počet dní před (relativní tooTimeGenerated tento záznam) tohoto agenta nainstalovat v žádné aktualizace ze služby Windows Server Update Service (WSUS) nebo Microsoft Update? |
| DaysSinceLastUpdateBucket |UpdateAgent |Podle DaysSinceLastUpdate kategorizaci v čas kbelíků z dobu počítač poslední nainstalované všechny aktualizace z webu služby WSUS nebo Microsoft Update. |
| AutomaticUpdateEnabled |UpdateAgent |Kontrola automatických aktualizací povolená nebo zakázaná na tento agent? |
| AutomaticUpdateValue |UpdateAgent |Je automatickou aktualizaci pro vracení se změnami sadu tooautomatically stažení a instalace, stahují jenom nebo jenom zkontrolujte? |
| WindowsUpdateAgentVersion |UpdateAgent |Číslo verze agenta hello Microsoft Update. |
| WSUSServer |UpdateAgent |Který WSUS server tohoto agenta aktualizace cílí? |
| OSVersion |UpdateAgent |Verze operačního systému hello tohoto agenta aktualizace běží na. |
| Name (Název) |Doporučení, ConfigurationObjectProperty |Název nebo název hello doporučení nebo název vlastnosti hello z hodnocení konfigurace analýzy protokolů. |
| Hodnota |ConfigurationObjectProperty |Hodnota vlastnosti z hodnocení konfigurace analýzy protokolů. |
| KBLink |Doporučení |Adresa URL toohello KB článek, který popisuje tento osvědčený postup nebo aktualizace. |
| RecommendationStatus |Doporučení |Doporučení jsou mezi hello několik typů, jejichž záznamy získat aktualizované, právě přidanou toohello indexu vyhledávání. Tento stav se změní, zda hello doporučení je aktivní/otevřít, nebo pokud analýzy protokolů zjistí, že ho byl vyřešen. |
| RenderedDescription |Událost |Vykreslené popis (znovu používaný text se vyplněná parametry) událost systému Windows. |
| ParameterXml |Událost |Soubor XML s parametry hello hello datové části události systému Windows (jak je vidět v prohlížeči událostí). |
| EventData |Událost |Soubor XML s hello celou datová část události systému Windows (jak je vidět v prohlížeči událostí). |
| Zdroj |Událost |Zdroj protokolu událostí, které vygenerovalo událost hello. |
| EventCategory |Událost |Kategorie události hello přímo z protokolu událostí systému Windows hello. |
| Uživatelské jméno |Událost |Uživatelské jméno událostí systému Windows hello (obvykle AUTHORITY\LOCALSYSTEM NT). |
| SampleValue |PerfHourly |Průměrná hodnota hello hodinové agregace čítače výkonu. |
| Min. |PerfHourly |Minimální hodnota v hello hodinový interval agregace hodinové čítače výkonu. |
| Max. |PerfHourly |Maximální hodnota v hello hodinový interval agregace hodinové čítače výkonu. |
| Percentile95 |PerfHourly |Hello 95. hodnota percentilu pro hello hodinový interval agregace hodinové čítače výkonu. |
| SampleCount |PerfHourly |Kolik nezpracovaná vzorků byly použité tooproduce tomto hodinové agregace záznamu. |
| Hrozby |ProtectionStatus |Název malwaru byl nalezen. |
| StorageAccount |W3CIISLog |Azure hello protokol účtu úložiště byl načten z. |
| AzureDeploymentID |W3CIISLog |ID nasazení Azure hello cloudové služby hello protokolu patří. |
| Role |W3CIISLog |Role hello Azure cloud service hello protokolu patří. |
| RoleInstance |W3CIISLog |RoleInstance hello Azure role, která hello protokolu patří. |
| sSiteName |W3CIISLog |Web služby IIS, která hello protokolu patří too(metabase notation); pole s-sitename Hello v původní protokolu hello. |
| sComputerName |W3CIISLog |pole s-computername Hello v původní protokolu hello. |
| sIP |W3CIISLog |Požadavek HTTP hello adresu IP serveru byla určena. pole s-ip Hello v původní protokolu hello. |
| csMethod |W3CIISLog |Metoda HTTP (např. GET nebo POST) používaná klientem hello v hello HTTP žádosti. Hello cs-method v původní protokolu hello. |
| cIP |W3CIISLog |Požadavek HTTP hello adresa IP klienta pochází. Hello c-ip pole v původní protokolu hello. |
| csUserAgent |W3CIISLog |HTTP User-Agent deklarovaná hello klienta (prohlížeče nebo jinak). Hello cs-user-agent v původní protokolu hello. |
| scStatus |W3CIISLog |Stav protokolu HTTP (například 200/403/500) vrátil kód klienta toohello server hello. Hello cs stav v původní protokolu hello. |
| timeTaken |W3CIISLog |Jak dlouho (v milisekundách) trvalo této žádosti hello toocomplete. pole timetaken Hello v původní protokolu hello. |
| csUriStem |W3CIISLog |Relativní identifikátor URI (bez adresa hostitele, který je, a vyhledávání), byl požadován. Hello cs-modul uristem pole v původní protokolu hello. |
| csUriQuery |W3CIISLog |Dotaz v identifikátoru URI. Identifikátor URI dotazy jsou potřebný jenom u dynamických stránek, například stránky ASP, takže toto pole obvykle obsahuje pomlčka pro statické stránky. |
| sPort |W3CIISLog |Port serveru, který hello požadavek HTTP byl odeslán příliš (a zda služba IIS naslouchá, protože je převzata). |
| csUserName |W3CIISLog |Ověřit uživatelské jméno, pokud je požadavek hello ověřený a není anonymní. |
| csVersion |W3CIISLog |Verzi protokolu HTTP použitou v žádosti o hello (například HTTP/1.1). |
| csCookie |W3CIISLog |Informace o souboru cookie. |
| csReferer |W3CIISLog |Webový server tento hello uživatel navštívil jako poslední. Tento web poskytl odkaz toohello aktuální web. |
| csHost |W3CIISLog |Hlavička hostitele (například www.mysite.com) požadovanou. |
| scSubStatus |W3CIISLog |Druhotný stavový kód chyby. |
| scWin32Status |W3CIISLog |Stavový kód Windows. |
| csBytes |W3CIISLog |Bajtů odeslaných v požadavku hello ze serveru toohello client hello. |
| scBytes |W3CIISLog |Bajtů vrátila zpět v odpovědi hello z klienta toohello server hello. |
| ConfigChangeType |Změnakonfigurace |Typ změny (například WindowsServices nebo softwaru). |
| ChangeCategory |Změnakonfigurace |Kategorie změn hello (Added/upravené nebo odebrané). |
| SoftwareType |Změnakonfigurace |Typ softwaru (aplikace nebo aktualizace). |
| SoftwareName |Změnakonfigurace |Název softwaru hello (pouze změny použít toosoftware). |
| Vydavatel |Změnakonfigurace |Dodavatele, který publikuje hello softwaru (pouze změny použít toosoftware). |
| SvcChangeType |Změnakonfigurace |Typ změny, které bylo použito na služby systému Windows (stavu/StartupType/cesta/Účet_služby). Toto je pouze změny tooWindows příslušné služby. |
| SvcDisplayName |Změnakonfigurace |Zobrazovaný název služby hello, která byla změněna. |
| SvcName |Změnakonfigurace |Název služby hello, která byla změněna. |
| SvcState |Změnakonfigurace |Nový stav (aktuální) služby hello. |
| SvcPreviousState |Změnakonfigurace |Předchozí označuje stav služby hello (platí jenom Pokud se změnila stav služby). |
| SvcStartupType |Změnakonfigurace |Typ spuštění služby. |
| SvcPreviousStartupType |Změnakonfigurace |Předchozí typ spuštění v služby (platí pouze je-li změnit typ spuštění služby). |
| SvcAccount |Změnakonfigurace |Účet služby. |
| SvcPreviousAccount |Změnakonfigurace |Účet služby předchozí (platí jenom Pokud změněn účet služby). |
| SvcPath |Změnakonfigurace |Cesta toohello spustitelný soubor služby systému Windows hello. |
| SvcPreviousPath |Změnakonfigurace |Předchozí cesta hello spustitelný soubor pro hello služba systému Windows (platí jenom pokud ho změnit). |
| SvcDescription |Změnakonfigurace |Popis služby hello. |
| Předchozí |Změnakonfigurace |Předchozí stav tohoto softwaru (nainstalováno nebo není nainstalovaná nebo předchozí verze). |
| Aktuální |Změnakonfigurace |Nejnovější stav tohoto softwaru (nainstalováno nebo není nainstalovaná nebo aktuální verze). |

## <a name="next-steps"></a>Další kroky
Další informace o protokolu hledání:

* Seznamte se s [protokolu hledání](log-analytics-log-searches.md) tooview podrobné informace shromážděné řešení.
* Použití [vlastních polí ve analýzy protokolů](log-analytics-custom-fields.md) tooextend protokolu hledání.
