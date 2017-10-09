---
title: "aaaCortana Intelligence řešení šablony scénářem pro vyžádání prognózy energie | Microsoft Docs"
description: "Šablona řešení s Microsoft Cortana Intelligence, který pomáhá prognózy vyžádání pro firmu nástroj energie."
services: cortana-analytics
documentationcenter: 
author: ilanr9
manager: ilanr9
editor: yijichen
ms.assetid: 8855dbb9-8543-45b9-b4c6-aa743a04d547
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2016
ms.author: ilanr9;yijichen;garye
ms.openlocfilehash: 32fc6ece7e24ced34282baf107548039694a38b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana Intelligence řešení šablony scénářem pro vyžádání prognózy energie
## <a name="executive-summary"></a>Shrnutí
V hello za několik let, Internet věcí (IoT), mít alternativních zdrojů energie a velké objemy dat sloučené toocreate velká příležitostí v nástroj hello a doméně, energie. V hello stejnou dobu, nástroj hello a hello celý energie sektoru viděli spotřeba vyrovnání se spotřebiteli náročných toocontrol lepší způsoby jejich použití energie. Proto hello nástroj a inteligentní mřížky společností jsou v tooinnovate velmi potřebné a obnovit sami. Kromě toho mnoho power a nástroj mřížky se stávají toomaintain zastaralé a velmi nákladné a spravovat. Během hello poslední rok hello tým pracuje na několika oznámeních podporujících zapojení uživatelů v rámci domény energie hello. Během těchto oznámeních podporujících zapojení uživatelů jsme narazili mnoha případech, ve které hello byla vyhledávání nezávislí dodavatelé softwaru (nezávislí výrobci softwaru) nebo nástroje do prognózy pro budoucí spotřeby energie. Tyto prognózy hraje důležitou roli v jejich aktuálním a budoucím obchodním a staly hello základ pro různé případy použití. Mezi ně patří krátkodobých a dlouhodobých power zatížení prognózy, obchodování, Vyrovnávání zatížení, optimalizace mřížky atd. Velké objemy dat a pokročilé analýzy (AA) metody například Machine Learning (ML) jsou hello klíče předpokladů pro vytvoření prognózy přesné a spolehlivé.  

V této scénářem sestavili jsme hello firmy a analytické pokyny potřebné pro úspěšné vývoj a nasazení spotřeby energie prognózy řešení. Tyto navrhované pokyny vám mohou pomoci nástroje, datových vědců a techniky data v zřízení plně operationalized založené na cloudu, prognózy vyžádání řešení. Pro společnosti, kteří jsou právě spouští jejich velkých objemů dat a cesty pokročilou analýzu můžete toto řešení představují počáteční hello počáteční hodnoty v jejich dlouhodobou strategii inteligentní mřížky.

> [!TIP]
> toodownload diagram, který poskytuje přehled architektury této šablony najdete v části [Cortana Intelligence řešení šablony architektura pro vytváření prognóz vyžádání energie](cortana-analytics-architecture-demand-forecasting-energy.md).  
> 
> 

## <a name="overview"></a>Přehled
Tento dokument popisuje hello firmy, data a technické aspekty pomocí Cortana Intelligence a v konkrétní Azure Machine Learning (AML) pro implementaci hello a nasazení řešení prognózy energie. Hello dokument se skládá ze tří hlavních částí:  

1. Principy podniku  
2. Pochopení dat  
3. Technická implementace

Hello **obchodní principy** část obsahuje přehled hello firmy aspekt jeden potřebám toounderstand a zvažte předchozí toomaking rozhodnutí o investice. Vysvětluje, jak tooqualify hello obchodního problému v dolním tooensure že prediktivní analýzy a machine learningu jsou skutečně efektivní a použít. Další Hello dokumentu se vysvětluje základy hello strojové učení a jak je použité tooaddress prognózy energie problémy. Popisuje ho hello požadavky a kritéria kvalifikace hello případu použití. Některé ukázkové použijte případy a případ obchodní scénáře jsou také uvedeny.

Data jsou hello hlavní složkou pro všechny strojového učení řešení. Hello **pochopení dat** tento dokument popisuje některé důležité aspekty dat hello. Popisuje ho hello druh data, která je potřebná pro prognózy energie, požadavky na kvalitu dat a jaké zdroje dat je obvykle neexistuje. Také popisují, jak hello nezpracovaných dat je funkce použité tooprepare dat, které ve skutečnosti jednotka hello modelování část.

Hello třetí součást hello dokumentu popisuje hello **technickou implementaci** aspekt řešení. Funkce inženýrství a modelování jsou jádrem hello proces vědecké účely hello dat a jsou proto se podrobněji některé. Pokrývá hello konceptu webové služby, které jsou důležité vehicle pro nasazení cloudu řešení prediktivní analýzy. Můžeme také popisují typický Architektura řešení operationalized začátku do konce.

Kromě toho hello dokument obsahuje referenční materiál, který můžete použít toogain další vysvětlení hello domény a technologie.

Je důležité toonote jsme nezamýšlíte toocover v tomto dokumentu hello hlubší data vědecké účely procesu, jeho matematické a technické aspekty. Tyto podrobnosti naleznete v [dokumentace Azure ML](http://azure.microsoft.com/services/machine-learning/) a [blogy](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Cílové skupiny
Hello cílovou skupinu tohoto dokumentu je obchodních a technických pracovníky, kteří chtěli toogain znalosti a pochopení Machine Learning na základě řešení a jak jsou použity konkrétně v rámci domény prognózy energie hello.

Datových vědců mohou také těžit z čtení tohoto dokumentu toogain lepší pochopení hello vysoké úrovně procesu, že jednotky hello nasazení energie prognózy řešení. V tomto kontextu může být také použít tooestablish směrný plán dobrý výchozí bod pro další podrobné a advanced materiálu.

### <a name="industry-trends"></a>Oborových trendů
V hello za několik let mít IoT, alternativních zdrojů energie a velké objemy dat sloučené toocreate velká příležitostí v nástroj hello a místo energie. V hello stejnou dobu, nástroj hello a hello celý energie sektory viděli spotřeba vyrovnání se spotřebiteli náročných toocontrol lepší způsoby jejich použití energie.

Mnoho nástroj a inteligentní energetické společnosti mají byla průkopnické hello [inteligentní mřížky](https://en.wikipedia.org/wiki/Smart_grid) ve nasazení počet použití případů, které používají hello dat vygenerovaných sadami hello mřížky. Mnoho případy použití základem hello vlastností vlastní výroby elektřiny: nemůže být nashromáždila, ani z produkce uložené jako inventáře. Ano co se vytvářejí se musí použít. Nástroje, které mají efektivnější toobecome potřebovat tooforecast spotřebu jednoduše vzhledem k tomu, který bude jim poskytnout lepší možnosti příliš**vyvážit nabídce a poptávce**, proto prevence ztráty energie **snížit skleníkových plynu emisí**a řídit náklady.

Při posuzování nákladů, je dalším důležitým aspektem, který je cena. Nové power tootrade dalo mezi nástroje začnou ve velmi potřebné příliš**předpovídat budoucí vyžádání a budoucí ceny elektřiny**. To může pomoct určit jejich svazky výrobní společnosti.

Když používáme hello slovo 'inteligentní', označujeme ve skutečnosti tooa mřížky, můžete informace a pak proveďte předpovědi. Ho můžete odhadnout sezónní změny energie a také **předvídáte dočasného přetížení situacích a automaticky ji upravit**. Ve vzdáleně regulační spotřeba (pomocí hello Tato inteligentní měřidla), lze provádět lokalizované přetížení situacích. **Nejprve predikci a pak funguje**, hello mřížky umožňuje efektivněji v čase.

Pro hello zbytek tohoto dokumentu se zaměříme na konkrétní řadu případy použití, které se týkají prognózy z budoucí krátkodobé i dlouhodobé na energii na vyžádání. Jsme několik měsíců pracovaly v těchto oblastech a dostalo některé znalosti a dovednosti, která nám umožňují tooproduce odvětví úrovni výsledky. Jiné případy použití se budeme také v dokumentu hello v blízké budoucnosti hello.

## <a name="business-understanding"></a>Obchodní vysvětlení
### <a name="business-goals"></a>Obchodních cílů
Hello **energie ukázku** cílem je toodemonstrate typické prediktivní analýzy a strojového učení řešení, které můžete nasadit ve velmi krátkého časového rámce. Konkrétně je naše aktuální aktivní, aby jeho obchodní hodnotu mohli rychle uvědomili si a využít při povolení energie vyžádání prognózy řešení. Hello informace v této playbook může pomoct zákaznické hello splníte následující cíle hello:

* Krátkou dobu toovalue machine learning na základě řešení
* Možnost tooexpand tooother případu použití pilotní použít případech nebo tooa širší rozsah podle jejich obchodních potřeb
* Rychle získat Cortana Intelligence Suite znalostní báze produktů

S těmito cíli nezapomeňte toto playbook cílem je doručování hello firmy a technické znalosti, která vám pomůže při dosažení těchto cílů.

### <a name="power-load-and-demand-forecasting"></a>Napájení zatížení a vyžádání prognózy
V rámci odvětví hello energie může být mnoha způsoby, které poptávky prognózy lze kritické obchodní problémy vyřešit. Ve skutečnosti vyžádání prognózy lze považovat za hello základ pro mnoho případy použití jader v odvětví hello. Obecně platí, jsme zvážit dva typy předpovědi energetické poptávky: krátkodobé i dlouhodobé. Každé z nich může sloužit k jinému účelu a využívat jiný přístup. Hello hlavní rozdíl mezi hello dva je hello prognózy horizontu, znamená hello rozsah čas do hello budoucí, pro kterou jsme by prognózy.

#### <a name="short-term-load-forecasting"></a>Krátkodobých termín zatížení prognózy
V rámci kontextu hello spotřeby energie krátké termín načíst prognózy (STLF) je definován jako hello agregované zatížení, která je naplánované v blízké budoucnosti na různých částí hello (nebo mřížky hello jako celek) hello. V tomto kontextu je krátkodobá definované toobe časový horizont rozsahu hello hodin too24 1 hodina. V některých případech je také horizon 48 hodin možné. Proto STLF je velmi běžné provozní použití případ hello mřížky. Tady jsou některé příklady STLF řízené případů použití:

* Nabídce a poptávce vyrovnávání
* Podpora obchodním napájení
* Provádění trhu (nastavení napájení cena)
* Provozní optimalizace mřížky
* [Odpověď na vyžádání](https://en.wikipedia.org/wiki/Demand_response)
* Ve špičkách vyžádání prognózy
* Vyžádání straně správy
* Vyrovnávání zatížení a přetížení prevence
* Dlouhodobé zatížení prognózy
* Selhání a anomálií detekce
* Curtailment/vyrovnávání ve špičce 

STLF model jsou většinou založené na hello v blízkosti po (posledního dne nebo týdne) datům o spotřebě a používání naplánované teploty jako důležité předpověď. Získávání přesných teploty prognózy hello příští hodiny a v provozu too24 hodin se stává stále menší výzvu nyní dnů. Tyto modely jsou méně citlivou vzory tooseasonal nebo dlouhodobé trendy spotřeby.

SLTF řešení jsou také velký objem předpovědi volání (žádosti o službu) pravděpodobně toogenerate vzhledem k tomu, že jsou právě vyvolány hodinu a v některých případech i s vyšší frekvence. Je také velmi běžné toosee implantaci, kde každé jednotlivé transformovny nebo transformer je reprezentována jako samostatné model, a proto je ještě lepší hello objem požadavků předpovědi.

#### <a name="long-term-load-forecasting"></a>Dlouhodobé zatížení prognózy
cílem Hello z dlouho termín zatížení prognózy (LTLF) je tooforecast power vyžádání pomocí časový horizont v rozsahu od 1 týden toomultiple měsíců (a v některých případech pro několik let). Tento rozsah horizon je ve většině případů platí pro plánování a investice případy použití.

Pro dlouhodobou scénáře je důležité toohave vysoké kvality dat, které pokrývá rozpětí několik let (minimální 3 roky). Tyto modely obvykle extrahovat sezónnosti vzory z hello historických dat a nutné používat externí predicators například jako počasí a klimatem vzory.

Je důležité tooclarify, který hello delší hello prognózy horizon, může být hello méně přesná hello prognózy. Proto je důležité tooproduce, které některé intervaly spolehlivosti společně s hello skutečné prognózy, který by umožnil člověka toofactor hello možné variace do proces plánování.

Vzhledem k tomu, že spotřeba scénáře hello LTLF je většinou plánování, můžete Očekáváme, že mnohem nižší předpovědi svazky (jako porovnání tooSTLF). Jsme by obvykle najdete v těchto předpovědi vkládat do vizualizace nástroje, například aplikace Excel nebo PowerBI a ručně vyvolat hello uživatele.

### <a name="short-term-vs-long-term-prediction"></a>Krátký termín vs. Dlouhodobé předpovědi
Hello následující tabulka porovnává STLF a LTLF v ohledem toohello nejdůležitější atributy:

| Atribut | Krátkodobá zatížení prognózy | Dlouhodobé zatížení prognózy |
| --- | --- | --- |
| Prognózy Horizon |Hodiny too48, 1 hodina |Z 1 too6 měsíců nebo více |
| Členitost dat |Každou hodinu |Hodinové nebo denní |
| Typické scénáře použití |<ul><li>/ Poptávky vyrovnávání</li><li>Vyberte hodinu prognózy</li><li>Odpověď na vyžádání</li></ul> |<ul><li>Dlouhodobé plánování</li><li>Plánování prostředky mřížky</li><li>Plánování prostředků</li></ul> |
| Typické prognostické |<ul><li>Dne nebo týdne</li><li>Hodiny dne</li><li>Hodinové teploty</li></ul> |<ul><li>Měsíc roku</li><li>Den v měsíci</li><li>Dlouhodobé teploty a klimatem</li></ul> |
| Rozsah historických dat |Data za dva roky toothree |Data za pět let too10 |
| Typické přesnost |MAPE * 5 % nebo nižší |MAPE * 25 % nebo nižší |
| Prognózy frekvence |Vytváří každou hodinu nebo každých 24 hodin |Vytváří jednou měsíčně, čtvrtletně nebo ročně |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – znamenat průměrnou procentuální chyby

Jak je vidět z této tabulky, je velmi důležité toodistinguish mezi hello krátký a dlouhodobé hello prognózy scénáře, protože se jedná představují různých obchodních potřeb a může mít jiné nasazení a vzorce používání.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Příklad použití případ 1: eSmart systémy – přetížení optimalizace
Důležité role [inteligentní mřížky](https://en.wikipedia.org/wiki/Smart_grid) je toodynamically a neustále optimalizovat a upravit hello změna vzorce používání. Spotřebu může být ovlivněno krátkodobé změny, které jsou způsobeny hlavně kolísání teploty (*například*, další power se používá pro podmínku letecké nebo vytápění). Na hello stejný čas, power spotřeba také ovlivněné dlouhodobé trendy. Ty mohou obsahovat sezónnosti účinky, státní svátky, dlouhodobé růstu spotřeby a i hospodářského faktorech, například příjemce index, cena těžba ropy a HDP.

V takovém případě použijte [eSmart](http://www.esmartsystems.com/) chtěli toodeploy cloudové řešení, které umožňuje predikci hello tendenci situace přetížení na jakékoli dané transformovny hello mřížky. Konkrétně eSmart chtěli trakčních tooidentify, které jsou pravděpodobně toooverload v rámci hello příští hodiny, takže okamžitý zásah, by mohl být přijat tooavoid nebo tato situace vyřešit.

Přesný a rychlé provádění předpovědi vyžaduje implementaci tři prediktivní modely:

* Dlouho termín modelu, že umožňuje prognózy z spotřebu energie na každý transformovny během hello další několika týdny nebo měsíce
* Krátkodobá model, který umožňuje předpovědi přetížení situace na danou transformovny během hello příští hodiny
* Model teploty, který poskytuje prognózy z teploty budoucí přes více scénářů

cílem Hello dlouhodobé modelu hello je toorank hello trakčních podle jejich tendenci toooverload (přiděleno jejich kapacita napájení přenosu) během hello příští týden nebo měsíc. To umožňuje vytvoření hello krátké seznam trakčních, které by slouží jako vstup pro krátkodobou předpověď hello. Teploty je důležité předpověď pro dlouhodobé model hello, je nutné tooconstantly produktu více scénář teplotě předpovídá a kanálu je jako vstup do toohello dlouhodobé modelu. krátkodobá Hello prognózy je volána poté toopredict které transformovny je pravděpodobně toooverload nad hello příští hodiny.

modely krátkodobé a dlouhodobé Hello nasazených jednotlivě na každém transformovny. Proto hello praktické provádění těchto modelů vyžaduje rozsáhlé orchestration. toogain vyšší přesnost předpovědi v krátkodobém horizontu hello podrobnější modelu je vyhrazený pro každou hodinu dne hello. Všechny tyto modely jsou vykonány každou hodinu a dokončí provádění v rámci pár minut tooallow dostatečný čas toorespond a přijmout preventivní opatření, v případě potřeby. Tato kolekce modelů je pořád aktuální pomocí pravidelné retraining pomocí hello nejnovější data.

Další informace o tento případ použití naleznete [zde](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Použít kritéria případu kvalifikace – požadavky
hlavní sílu Cortana Intelligence Hello je v jeho toodeploy výkonné možnosti a škálování machine learning zaměřená na řešení. Jde o předpovědi, které jsou spouštěny souběžně navrženou toosupport tisíců. Ji může automaticky škálovat toomeet změny struktury spotřeby. Proto je fokus na řešení na přesnost a výpočetní výkon. Například nástroj společnosti je zájem o vytvoření prognózy hello příští hodiny a pro každou hodinu dne hello poptávky přesné energie. Na hello druhé straně, jsme zájem o méně odpovídání hello otázku, proč vyžádání hello je předpokládaných toobe, jako je (hello přímo pro model se postará o,).

Proto je důležité toorealize, že všechny případy použití a obchodní efektivně vyřešení problémů pomocí machine learning.

Cortana Intelligence a machine learningu může být velice efektivní v řešení daného obchodního problému, pokud jsou splněny hello následující kritéria:

* Hello firmy problém v dolním je **prediktivní** ve své podstatě. V příkladu případu použití prediktivní je nástroj společnost, která chcete toopredict power zatížení na danou transformovny během hello příští hodiny. Na hello druhé straně, analýze a pořadí ovladače historických vyžádání by **popisný** ve své podstatě a proto méně použitelné.
* Je zřejmé **cestu akce** toobe prováděné jednou hello předpovědi je k dispozici. Například predikci přetížení na transformovny během hello příští hodiny můžete aktivovat proaktivní akce snižuje zatížení, který je přidružený tento transformovny a proto potenciálně brání přetížení.
* případ použití Hello představuje **typický typ problému** tak, že když vyřešeny ho hello způsob toosolving pave jiné podobné případy použití.
* můžete nastavit Hello zákazníka **kvantitativní a kvalitativní cíle** toodemonstrate implementace úspěšné řešení. Například dobrý kvantitativní cíl pro energie vyžádání prognózy by hello požadované přesnost prahové hodnoty (*např*, až too5 je povoleno % došlo k chybě) nebo pokud predikci transformovny přetížení pak hello přesnost (počet pozitivních true) a odvolání (rozsah true pozitivních) by měla být s danou prahovou hodnotu. Těchto cílů by měl být odvozen od hello zákazníka obchodních cílů.
* Je zřejmé **scénáře integrace** s pracovním postupem prostředí společnosti hello firmy. Například hello transformovny zatížení prognózy lze integrovat do prevence aktivity přetížení centra tooallow hello mřížky ovládacího prvku.
* Hello zákazník má připravené toouse **dat s dostatečnou kvality** případ použití toosupport hello (v další části hello, další informace naleznete v **Data Quality**, z této playbook).
* Hello zákazníka vyhoví architektuře cloudu zaměřená na data nebo **cloudové machine learning**, včetně Azure ML a další součásti Cortana Intelligence Suite.
* Hello zákazníka je ochotná tooestablish **tok dat tooend end** , zařízení hello doručování dat do cloudu hello průběžně a je ochotná příliš**zprovoznit** hello řešení.
* Hello zákazníka je připraven příliš**vyhradit prostředky** kdo bude mít aktivně zapojení během počáteční pilotní implementace hello tak, aby znalosti a vlastnictví hello řešení může být přenést toohello zákazníka po úspěšné dokončení.
* Hello zákazníka zdroj by měl být **zkušený data professional**, pokud možno vědecký pracovník data.

Kvalifikace případu použití podle hello výše uvedeným kritériím může výrazně zlepšit hello úspěšnost případu použití a vytvořit funkční beachhead pro implementaci hello případů budoucí použití.

### <a name="cloud-based-solutions"></a>Řešení založená na cloudu
Cortana Intelligence Suite v Azure je integrované prostředí, který se nachází v cloudu hello. nasazení Hello pokročilou analýzu řešení v cloudovém prostředí obsahuje významné výhody pro firmy a v hello stejnou dobu může to znamenat velkou změnu pro společnosti, že stále použít místní IT řešeními. V rámci odvětví hello energie je zrušte trend postupné migrace operací do cloudu hello. Tento trend souvisí společně s hello vývoj inteligentní mřížky hello jak je popsáno výše, v **odvětví trendy**. Tato scénářem se zaměřuje na cloudové řešení v doméně hello energie, je důležité tooexplain hello výhody a další důležité informace o nasazení řešení založená na cloudu.

Možná hello největších výhod cloudové řešení je hello náklady. Jako řešení využívá součástí nasazení cloudu, neexistuje žádný předem náklady a náklady na spotřebu (náklady z zboží prodané) komponenty s ním spojená. To znamená, že neexistuje žádné tooinvest nutné v hardwaru, softwaru a údržby IT, a proto je podstatně snížit riziko obchodní.

Další důležité výhodou je struktura průběžnými platbami náklady hello cloudové řešení. Servery založené na cloudu pro computing nebo úložiště můžete nasadit a škálovat na základě právě podle potřeby. To představuje výhody efektivitu nákladů hello cloudové řešení.

Nakonec je pro Investujete do IT údržby nebo vývoj budoucí infrastruktury, jako to vše je součástí hello Cloudová nabídka není nutné. rozsah toothat, Cortana Intelligence Suite zahrnuje hello nejlépe třídy služeb a jeho silniční mapu udržuje vyvíjející se. Nové funkce, součásti a možnosti jsou neustále zavedené a momentální.

Pro společnosti, který je právě spouští jeho přechodu do cloudu hello důrazně doporučujeme tootake postupného implementací mapě cloudu migrace. Věříme, že pro nástroje a společnosti v doméně energie hello, hello případy použití, které jsou popsané v této playbook reprezentuje příležitost vynikající pro pilotní nasazení řešení prediktivní analýzy v cloudu hello.

#### <a name="business-case-justification-considerations"></a>Aspekty obchodní případu při zarovnání do bloku
V mnoha případech může být hello zákazníka zájem o provedení obchodního oprávnění pro danou použití případ, ve kterém jsou cloudové řešení a Machine Learning důležité součásti. Na rozdíl od v případě místních řešení, v případě hello cloudové řešení komponenta předem nákladů hello je minimální a většina elementů hello náklady jsou přidruženy skutečném využití. Pokud jde toodeploying energie prognózy řešení na webu Cortana Intelligence Suite, více služeb lze integrovat s struktura single běžné náklady. Například databáze (*například*, SQL Azure) lze použít toostore hello nezpracovaná data a pak pro hello skutečné předpovídá Azure ML je použité toohost hello prognózy služby. V tomto příkladu můžou zahrnovat hello struktury nákladů, úložiště a transakčních komponent.

Na hello druhé straně, jeden měli rozumět hello obchodní hodnotu operačního vyžádání energie prognózy (krátkodobého nebo dlouhodobého hlediska). Ve skutečnosti je důležité toorealize hello obchodní hodnotu každé prognózy operace. Například přesně prognózy power zatížení pro hello dalších 24 hodin můžete zabránit nadbytečné produkce nebo může zabránit přetížení na hello mřížky a to je možné kvantifikovat z hlediska finanční úspor na každý den.

Základní vzorec pro výpočet hello finanční výhodou vyžádání prognózy řešení by: ![základní vzorec pro výpočet hello finanční výhodou vyžádání prognózy řešení](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Vzhledem k tomu, že Cortana Intelligence Suite poskytuje průběžnými platbami cenový model, není nutné pro by docházelo k vzorec toothis součást pevné náklady. Tento vzorec, lze vypočítat na základě denně, měsíční nebo roční.

Aktuální Cortana Intelligence Suite a Azure ML cenových plánů najdete [zde](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Proces vývoj řešení
cyklus vývoj Hello vyžádání energie prognózy, řešení obvykle zahrnuje 4 fází, všechny z nich provedeme pomocí cloudových technologií a služeb v rámci hello Cortana Intelligence Suite.

To je znázorněno v následujícím diagramu hello:

![Cyklus inteligentní mřížky](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Následující odstavce Hello popisuje tento proces krok 4:

1. **Shromažďování dat** – všechny pokročilé analýzy na základě řešení využívá data (najdete v části **pochopení dat**). Konkrétně pokud jde toopredictive analýzy a vytváření prognóz, spoléháme na probíhající, dynamické tok dat. V případě hello prognózy vyžádání energie tato data můžete použít jako zdroj přímo z inteligentní měřidla nebo již agregovat na místní databázi. Také spoléháme na jiné externí zdroje dat, jako je například počasí a teploty. Tento probíhající tok dat je nutné orchestrovat, naplánovat a uložit. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF) je naše hlavní centrem k provedení této úlohy.
2. **Modelování** – pro přesné a spolehlivé energie prognózy jeden musí vyvíjet (train) a udržovat skvělé model, který umožňuje použít hello historických dat a extrahuje hello smysluplný, a prediktivní vzorců hello data. oblasti Hello Learning počítače (ML) má byl narůstá s více pokročilé algoritmy pravidelně vyvíjených. Azure ML Studio nabízí skvělý uživatelské prostředí, která pomáhá využívat hello nejvíce pokročilé algoritmy ML v rámci dokončení pracovní postup. Tento pracovní postup je znázorněna v intuitivní vývojový diagram a zahrnuje hello data přípravy, funkce extrakce, modelování a vyhodnocení modelu. Hello uživatele můžete stáhnout stovky různých modely, které jsou zahrnuté v tomto prostředí. Konec hello v této fázi vědecký pracovník dat bude mít pracovní model, který je plně vyhodnotí a připravena k nasazení.
   
   Následující diagram Hello je obrázek typické pracovního postupu:
   
   ![Modelování pracovního postupu](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Nasazení** – s modelem pracovní ručně, hello dalším krokem je nasazení. Zde je hello model převeden na webová služba, která vystavuje rozhraní RESTful API, která se může vyvolat souběžně přes Internet hello od různých klientů spotřeby. Azure ML poskytuje jednoduchý způsob nasazení model přímo z hello Azure ML Studio jediným kliknutím na tlačítko. pod pokličkou hello se stane Hello celý proces nasazení. Toto řešení může automaticky škálovat toomeet hello požadované spotřeby.
4. **Spotřeba** – v této fázi se ve skutečnosti uděláme použití hello tooproduce předpovědi modelu prognózy. Spotřeba Hello můžete vycházejí z aplikace uživatele (*například*, řídicí panel) nebo přímo z operačního systému, jako/poptávky vyrovnávání systému nebo řešení optimalizace mřížky. Několik případů použití může vycházejí z jeden model.

## <a name="data-understanding"></a>Pochopení dat
Po pokrývajících hello firmy aspekty (najdete v části **obchodní principy**) vytváření prognóz řešení poptávky energie, Snažíme se teď připravena toodiscuss hello datovou část. Řešení prediktivní analýzy spoléhá na spolehlivé data. U prognózy vyžádání energie, spoléháme na historické spotřeby data pomocí různých úrovní členitosti. Historických dat se používá jako suroviny hello. Určitým pečlivě analýzy, ve které hello vědecký pracovník data určují prognostické (také odkazované tooas funkcí), které můžou být přepnuté do modelu, který nakonec vygeneruje hello požadované prognózy.

V hello zbývající část tohoto oddílu, jsme popíše hello různé postupy a požadavky pro pochopení hello data a jak toobring ho tooa použitelné podoby.

### <a name="hello-model-development-cycle"></a>Hello cyklu vývoje modelu
Vytváření dobrý prognózy modely vyžaduje některé pečlivě přípravu a plánování. Rozdělení hello proces modelování do více kroků a zaměřené na jednom kroku současně může výrazně zlepšit hello výsledek hello celý proces.

Hello následující diagram znázorňuje, jak může hello modelování proces rozdělit do více kroků:

![Cyklu vývoje modelu](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Jak je patrné, že hello cyklus se skládá z šesti kroků:

* Formulování problém
* Přijímání dat a zkoumání dat
* Příprava dat a funkce inženýrství
* Modelování
* Vyhodnocení modelu
* Vývoj

V hello zbytek této části jsme se popisují jednotlivé kroky hello a tooconsider položek v každé fázi.

### <a name="problem-formulation"></a>Formulování problém
Formulování hello problém jsme můžete zvážit hello nejdůležitější kroku jedna stačit předchozí tooimplementing tootake řešení prediktivní analýzy. Zde jsme by transformace hello obchodního problému a jeho rozložit toospecific elementy, které lze vyřešit pomocí data a modelování techniky. Je dobrým zvykem tooformulate hello problém jako sada otázek rádi bychom znali tooanswer. Zde jsou některé možné otázky, které nejsou k dispozici v rámci oboru hello prognózy na energii na vyžádání:

* Co je očekáván hello zatížení jednotlivých transformovny v hello další hodinu nebo den?
* V průběhu dne hello všimnete Moje mřížky poptávky ve špičce?
* Jak pravděpodobně je zátěž ve špičce hello očekává toosustain Moje mřížky?
* Kolik energie měl hello power stanice generovat během každou hodinu dne hello?

Formulování tyto otázky nám umožňuje toofocus na získávání hello správná data a implementace řešení, které je plně v souladu s hello firmy problém. Kromě toho jsme pak můžete nastavit některé klíčové metriky, které nám umožňují tooevaluate hello výkonu hello modelu. Například jak přesný by měl hello prognózy být a jaký je rozsah hello chyby, který bude stále přijatelné podle hello firmy?

### <a name="data-sources"></a>Zdroje dat
Moderní inteligentní mřížky Hello shromažďuje data z různých částí a komponent hello mřížky. Tato data představuje různé aspekty hello provozu a využití hello hello power mřížky. V rámci oboru hello poptávky energie hello prognózy jsme jsou omezení hello zabývat zdrojů dat, které odráží spotřeba hello aktuální požadavek. Jeden zdroj důležité spotřeby energie, jsou inteligentní měřidla. Nástroje kolem hello zeměkouli rychle nasazujete inteligentní měřidla pro jejich příjemce. Inteligentní měřidla zaznamenávat hello skutečné spotřeby energie a neustále předávání společnosti nástroj back toohello tato data. Data se shromažďuje a odesílá zpět v pevných intervalech, od každou hodinu too1 5 minut. Pokročilejší inteligentní měřidla lze naprogramovat vzdáleně toocontrol a vyrovnávat hello skutečné spotřebě v rámci domácnost. Inteligentní měření dat je poměrně spolehlivé a obsahuje časové razítko. Tím je důležité složky pro vyžádání prognózy. Měření dat, se dají agregovat (sečtené) na různých úrovních v rámci topologie mřížky hello: transformer, transformovny, oblast, *atd*. Hello požadované agregace úrovně toobuild jsme můžete vyberte model prognózy pak pro něj. Například pokud hello nástroj společnosti by jako tooforecast budoucí zatížení na každý z jeho mřížky trakčních pak všechny měřidla data může být agregován pro každé jednotlivé transformovny a použít jako vstup pro hello modelu prognózy. Toosmart měřidla označujeme jako interní datový zdroj.

Vyžádání prognózy spolehlivé energie bude také závisí na jiných externích zdrojů dat.. Jeden důležitý faktor, který má vliv na spotřebu energie je hello počasí, nebo přesněji teploty hello. Historická data zobrazují silné korelace mezi mimo teploty a spotřebu energie. Během aktivního letní dnů spotřebitelům při jejich klimatizace a při zapnutí jedná o zimní hello systémů vytápění. Spolehlivý zdroj historických teplot v umístění mřížky hello je proto klíč. Kromě toho také spoléháme na přesná prognóza z teploty jako nástroj pro odhad spotřeby energie.

Další externích zdrojů dat. může také pomoci při vytváření modelů prognózy na energii na vyžádání. Ty mohou obsahovat dlouhodobé klimatem změny, ekonomické indexy (*například*, HDP) a další. V tomto dokumentu jsme nebude obsahovat tyto ostatních zdrojů.

### <a name="data-structure"></a>Datové struktury
Po identifikaci hello požadované zdroje dat, můžeme by jako tooensure, nezpracovaná data, které se shromáždily zahrnuje hello opravte data funkce. model prognózy toobuild spolehlivé vyžádání, potřebujeme by tooensure, který hello shromažďovaných dat zahrnuje datové prvky, které může pomoci předpovídat budoucí vyžádání hello. Tady jsou některé základní požadavky týkající se struktura dat hello (schéma) hello nezpracovaná data.

nezpracovaná data Hello se skládá z řádků a sloupců. Každé měření je reprezentován jako jednoho řádku dat. Každý řádek dat obsahuje více sloupců (také odkazované tooas funkce nebo polí).

1. **Časové razítko** – pole časového razítka hello představuje hello skutečný čas, kdy byla zaznamenána hello měření. Ho by měly splňovat jeden z běžných formátů data a času hello. Datum a čas částí by měly být zahrnuty. Ve většině případů není třeba pro hello čas toobe zaznamenávají do hello druhou úroveň členitosti. Je důležité toospecify hello časové pásmo, ve kterém se zaznamená hello data.
2. **Měřicí ID** – v tomto poli identifikuje hello měření nebo hello měření zařízení. Je kategorií proměnné a může být kombinací číslic a znaků.
3. **Hodnota spotřeby** – jedná se o skutečné spotřebě hello v dané datum a čas. Spotřeba Hello lze měřit v kWh (kilowatt-hour) nebo jakékoliv preferované jednotky. Je důležité, že toonote, který hello Měrná jednotka musí zůstat konzistentní napříč všech měření v datech hello. V některých případech může být spotřeba dodán více než 3 power fáze. V takovém případě by potřebujeme toocollect všechny fáze nezávislé spotřeba hello.
4. **Teplotní** – teploty hello je obvykle shromážděné z nezávislé zdroje. Ale musí být kompatibilní s datům o spotřebě hello. Měl by obsahovat časovým razítkem jak bylo popsáno výše, který vám umožní toobe synchronizována s daty o skutečné spotřebě hello. hodnota teploty Hello lze zadat v stupňů c nebo Fahrenheita ale musí zůstat konzistentní napříč všech měření.
5. **Umístění –** pole umístění hello obvykle souvisí s hello místě, kde jsou shromážděná data teploty hello. Může být reprezentován jako poštovní směrovací číslo nebo ve formátu zeměpisnou šířku a délku (lat nebo long).

Hello následující tabulky uvádí příklady dobrý využívání a teploty formát dat:

| **Datum** | **Čas** | **ID měření** | **Fáze 1** | **Fáze 2** | **Fáze 3** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **Datum** | **Čas** | **Umístění** | **Teplotní** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Jak je vidět výše, tento příklad obsahuje 3 různé hodnoty pro používání přidružené fáze 3 napájení. Všimněte si také, zda text hello datum a čas pole se oddělují, ale mohou také být sdruženy do jednoho sloupce. V takovém případě je sloupec hello umístění reprezentované ve formátu zip kódu 5 číslic a teploty hello ve formátu stupeň c.

### <a name="data-format"></a>Formát dat
Cortana Intelligence Suite může podporovat hello nejběžnější formáty dat sdíleného svazku clusteru, TSV, formát JSON, *atd*. Je důležité, že formát dat hello zůstane konzistentní pro hello celý životní cyklus hello projektu.

### <a name="data-ingestion"></a>Přijímání dat
Vzhledem k tomu, že na energii na vyžádání prognózy je neustále a často předpovědět, jsme musíte zajistit, že hello nezpracovaná data proudí prostřednictvím procesu přijímání dat plnou a spolehlivé. proces přijímání Hello musí zaručit, že hello nezpracovaná data je k dispozici pro hello prognózy procesu v době hello vyžaduje. Která znamená, že četnost přijímání dat hello musí být větší než hello prognózy frekvence.

Například: Pokud naše vyžádání prognózy řešení by vygeneroval novou prognózu v 8:00 AM denně pak potřebujeme tooensure, který hello všechna hello data, která jsou shromážděná během posledních 24 hodin byla plně požity do tohoto bodu a má tooeven, zahrnout hello poslední hodinu dat.

V pořadí tooaccomplish, toho Cortana Intelligence Suite nabízí různé způsoby toosupport proces přijímání spolehlivé data. To dál probereme v hello **nasazení** část tohoto dokumentu.

### <a name="data-quality"></a>Kvality dat.
Hello nezpracovaná data zdroj, který se vyžaduje k provedení prognózy spolehlivé a přesné vyžádání musí splňovat kritéria kvality některé základní data. I když pokročilé statistické metody může být použité toocompensate pro některé možné data quality problém, potřebujeme ještě tooensure, že jsme se při překročení prahové hodnoty některé základní data quality při příjem nová data. Zde je několik aspekty týkající se kvality nezpracovaná data:

* **Chybí hodnota** – vztahuje toohello situace při konkrétní měření nebyla shromážděna. Hello zde základní požadavek je, že tento hello chybí hodnota míry nesmí být větší než 10 % pro jakékoli dané časové období. V případě, že jedna hodnota je chybějící by měl být indikován pomocí předem definované hodnoty (například: '9999') a zda není '0', který může být hodnota je neplatná.
* **Přesnost měření** – hello skutečnou hodnotu spotřeby nebo teploty by měl být přesně zaznamenán. Nesprávné měření způsobí nepřesné prognózy. Chyba měření hello obvykle by měla být nižší než hodnota true relativní toohello 1 %.
* **Čas měření** – je potřeba skutečná razítek hello shromažďovaných dat hello nesmí lišit o více než 10 sekund relativní toohello true čas vlastní měření hello.
* **Synchronizace** – Pokud se používají více zdrojů dat (*například*, využívání a teploty) jsme Ujistěte se, že neexistují žádné synchronizaci času problémy mezi nimi. To znamená, že čas hello rozdíl mezi časové razítko hello shromážděných ze všech dva nezávislé datových zdrojů nesmí překročit více než 10 sekund..
* **Latence** – jak je popsáno výše, v **přijímání dat**, jsme jsou závislé na spolehlivé datového toku a přijímání procesu. toocontrol, že jsme musí zajistit, že jsme řízení hello data latence. To je zadán jako hello časový rozdíl mezi hello čas, který vlastní měření hello pořízení a hello čas, kdy byla načtena do hello Cortana Intelligence Suite úložiště a je připravený k použití. Pro krátkodobou zatížení prognózy nízkou celkovou latenci hello nesmí být větší než 30 minut. Pro dlouhodobé zatížení prognózy nízkou celkovou latenci hello nesmí být větší než 1 den.

### <a name="data-preparation-and-feature-engineering"></a>Příprava dat a funkce inženýrství
Jakmile hello nezpracovaná data byla požity. (najdete v části **přijímání dat**) a byla bezpečně uložena, je připraven toobe zpracovat. Hello fázi přípravy dat je v podstatě trvá hello nezpracovaná data a převodu (transformace, změna tvaru) do formuláře pro modelování fáze hello. Jednoduché operace, například pomocí sloupce hello nezpracovaná data, jako je s jeho skutečná hodnota měřená, standardizované hodnoty, složitějších operací, jako který může zahrnovat [čas obložení](https://en.wikipedia.org/wiki/Lag_operator)a další. Hello nově vytvořený datových sloupců jsou odkazované tooas datových funkcích a hello proces generování tyto odkazované tooas funkce inženýrství. Hello konci tohoto procesu nám nové sady dat, který byl získán z hello nezpracovaná data a lze použít pro modelování. Kromě toho musí fázi přípravy dat hello tootake péče o chybějící hodnoty (viz **Data Quality**) a kompenzovat je. V některých případech také potřebujeme tooensure toonormalize hello data, která všechny hodnoty jsou zobrazeny ve hello stejné měřítko.

V této části jsme některé hello běžné funkce dat, které jsou součástí hello energie seznamu prognózy vyžádání modelů.

**Čas řízené funkce:** tyto funkce jsou odvozeny od hello datum/časové razítko data. Tyto jsou extrahovat a převést do kategorií funkcí, jako je:

* Čas dne – to je hello hodina dne hello, který přebírá hodnoty od 0 too23
* Den v týdnu – to představuje hello den v týdnu hello a přijímá hodnoty z 1 (neděle) too7 (sobota)
* Den v měsíci – to představuje skutečný datum hello a může trvat hodnoty z 1 too31
* Měsíc roku – to představuje hello měsíc a přijímá hodnoty z 1 (leden) too12 (prosinec)
* Víkendu – Toto je funkce binární hodnotu, která přijímá hello hodnoty 0 pro dny v týdnu nebo 1 pro víkendu
* Svátek - Toto je funkce binární hodnotu, která přijímá hello hodnoty 0 pro regulární den nebo 1 pro svátek
* Podmínky Fourierova – hello Fourierova podmínky jsou uvedeny váhu, které jsou odvozeny od hello časové razítko a jsou použité toocapture hello sezónnosti (cykly) v datech hello. Vzhledem k tomu, že jsme může mít několik ročních období v našich dat potřebujeme více Fourierova podmínky. Vyžádání hodnoty může mít například roční, týdenní a denní období nebo cykly které bude mít za následek 3 Fourierova podmínky.

**Funkce nezávislé měření:** hello nezávislé funkce zahrnují všechny hello datové prvky, rádi bychom znali toouse jako prognostické v našem modelu. Tady jsou vyloučeny hello závislé funkci, která by potřebujeme toopredict.

* Funkce opoždění – jedná se o čas zapuštěno hodnoty hello aktuální požadavek. Například bude funkce lag 1 umístit hello vyžádání hodnotu aktuální časové razítko aplikace hello předchozí hodiny (za předpokladu, že po hodinách data) relativní toohello. Jsme podobně přidat prodleva 2, 3, funkce lag *atd*. určuje hello skutečné kombinace prodleva funkce, které se používají během hello modelování fáze hodnocení hello modelu výsledků.
* Dlouhodobé trendů – tato funkce představuje hello lineární rostoucích požadavků mezi let.

**Závislé funkce:** závislé funkce hello je hello sloupce dat, která rádi bychom znali naše toopredict modelu. S [počítač učení se supervizí](https://en.wikipedia.org/wiki/Supervised_learning), musíme nejprve cvičení hello model pomocí závislé součásti hello (což je také odkazované tooas popisky). To umožňuje hello modelu toolearn hello vzory v hello data související s funkcí závislé hello. V energie vyžádání prognózy chceme obvykle toopredict hello aktuální požadavek a proto jsme proč ji používat jako závislé funkce hello.

**Zpracování chybějící hodnoty:** během fáze přípravy dat hello by potřebujeme toodetermine hello nejlepší strategii toohandle chybějící hodnoty. To je ve většině případů s použitím hello různé statistické [data imputace metody](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). V případě hello energie poptávky prognózy jsme obvykle dává chybějící hodnoty pomocí klouzavého průměru z předchozí dostupné datových bodů.

**Data normalizaci:** normalizace dat je jiný typ transformace, což je použité toobring všechny číselná data, jako je například vyžádání prognózy do podobné škálování. To obvykle pomáhá zlepšit přesnost hello modelu. Jsme by obvykle k tomu vydělením skutečná hodnota hello hello rozsah dat hello.
Původní hodnotu hello to bude škálovat do menší oblast, obvykle mezi -1 a 1.

## <a name="modeling"></a>Modelování
Hello modelování fáze je, kde probíhá převod hello hello dat do modelu. V hello základní tohoto procesu existuje pokročilé algoritmy, které kontrola hello historických dat (Cvičná data), rozbalte vzory a sestavení modelu. Tento model můžou být později použít toopredict na nová data, která nebyla použité toobuild hello modelu.

Jakmile spolehlivé funkční modelu jsme ho pak může použít tooscore nová data, která je strukturovaných tooinclude hello požadované funkce (X). Hello procesu určování skóre budou používat hello trvalou modelu (objekt z fáze školení hello) a předvídání hello Cílová proměnná, který je označený jako Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Vyžádání prognózy modelování techniky
V případě hello poptávky prognózy, provedeme pomocí historických dat, který je seřazené podle času. Obecně označujeme toodata, která zahrnuje hello časové dimenze jako [časové řady](https://en.wikipedia.org/wiki/Time_series). cílem Hello na dobu, kterou řady modelování je čas toofind související trendů, sezónnosti, auto korelace (korelace v čase) a formulovali v modelu.

V posledních letech pokročilé algoritmy byly vyvinuté tooaccommodate časové řady prognózy a tooimprove přesnost prognózy. Stručně probereme několik z nich zde.

> [!NOTE]
> Tento oddíl není určený toobe použít jako machine learning a předpovídat přehled, ale spíše jako krátký přehled modelování techniky, které se běžně používají pro vyžádání prognózy. Další informace a vzdělávací materiály o časové řady prognózy, důrazně doporučujeme knihy online hello [vytváření prognóz: Principy a postup](https://www.otexts.org/book/fpp).
> 
> 

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (klouzavého průměru)**](https://www.otexts.org/fpp/6/2)
Klouzavý průměr je jedním z hello první analytické techniky použitého pro předpověď časové řady a je stále jednoho z postupů hello nejčastěji používaná k dnešnímu dni. Je také hello základ pro pokročilejší prognózy techniky. S klouzavý průměr jsme jsou prognózy hello další datového bodu jako průměr prostřednictvím hello K nejnovější bodů, kde K označuje hello pořadí hello klouzavý průměr.

Přesunutí průměrná technika Hello efekt hello vyhlazení hello prognózy a nemusí proto zpracovat dobře velké volatility v datech hello.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (exponenciální vyrovnání)**](https://www.otexts.org/fpp/7/5)
Exponenciální vyhlazování (ETS) je rodina různé metody, které používají váženým průměrem poslední datových bodů v pořadí toopredict hello další datového bodu. je vhodné Hello je vyšší váhou tooassign toomore poslední hodnoty a postupně snížit tento váha pro starší měřené hodnoty. Existuje několik různých metod s této rodině, některé z nich patří například zpracování sezónnosti v datech hello [Holt Winters sezónní metoda](https://www.otexts.org/fpp/7/5).

Některé z těchto metod také zvážit hello sezónnosti dat hello.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatické regrese integrovaný klouzavý průměr)**](https://www.otexts.org/fpp/8)
Automatické regrese integrované přesunutí průměrná (ARIMA) je jiné rodiny metod, které se běžně používá pro předpověď časové řady. Auto-regression metody prakticky kombinuje s klouzavý průměr. Auto-regression metody použití modelů regrese provedením předchozích časových řad hodnoty v pořadí toocompute hello další datum bodu. Metody ARIMA platí také rozdílové metody, které obsahují výpočet hello rozdíl mezi datovými body a jejich namísto hello původní měřené hodnoty použití. Nakonec ARIMA také využívá hello přesunutí průměrná techniky, které jsou popsané výše. Hello kombinace všechny tyto metody v různé způsoby, kterými je konstrukce hello řadu metody ARIMA.

ETS a ARIMA se často používá dnes pro prognózy vyžádání energie a mnoho dalších prognózy problémů. V mnoha případech tyto jsou sloučeny společně toodeliver velmi přesné výsledky.

**Obecné vícenásobná regrese** modely regrese může být hello nejdůležitější modelování přístup v rámci domény hello strojové učení a statistiky. V kontextu hello časové řady použijeme regresní toopredict hello budoucí hodnoty (*například*, poptávky). V regrese jsme trvat lineární kombinaci hello prognostické a další hello váhu (také odkazované tooas koeficienty) tyto prognostické během procesu školení hello. cílem Hello je tooproduce regrese řádek, který bude prognózy naše předpovězené hodnoty. Regrese metody jsou vhodné, když Cílová proměnná hello je číselné a proto také vyhovuje prognózy časové řady. Je širokou škálu regrese metody včetně velmi jednoduché regrese modely, jako například [lineární regrese](https://en.wikipedia.org/wiki/Linear_regression) a další pokročilé těch, které jsou například rozhodovací stromy, [doménových struktur náhodných](https://en.wikipedia.org/wiki/Random_forest), [Neural Sítě](https://en.wikipedia.org/wiki/Artificial_neural_network)a Boosted Decision Trees.

Vytváření energie vyžádání prognózy jako problém regrese nám poskytuje značnou flexibilitu při výběru naší datové funkce, které mohou být kombinovány z data řady čas hello aktuální požadavek a vnějším faktorům, jako je například teploty. Další informace o hello vybrané funkce, které jsou popsané v hello funkce Engineering (najdete v části **Příprava dat a funkce Engineering**) části této scénářem.

Z našich zkušeností s provádění a nasazení na energii na vyžádání prognózy pilotního zjistili jsme, že hello pokročilé regrese modelů, které jsou dostupné v Azure ML mívají tooproduce hello nejlepších výsledků a budeme používat z nich.

## <a name="model-evaluation"></a>Vyhodnocení modelu
Vyhodnocení modelu má zásadní roli v rámci hello **cyklu vývoje modelu**. V tomto kroku podíváme do ověřování hello modelu a jeho výkon s skutečných dat. Během hello modelování krok použijeme pro trénování modelu hello součástí hello dostupná data. Během fáze hodnocení hello jsme trvat hello zbytek hello data tootest hello modelu. Prakticky znamená, že jsme se napájení modelu hello nové se data, která má se změnili a obsahuje hello stejné funkce jako hello školení datovou sadu. Ale během procesu ověřování hello jsme použít hello modelu toopredict hello Cílová proměnná nemusí poskytovat hello k dispozici Cílová proměnná. Často označujeme toothis proces jako vyhodnocování modelu. Používáme by pak hello true cílové hodnoty a porovnejte je s hello těch, které jsou předpovědět. Hello cílem je toomeasure a minimalizovat chyba předpovědi hello, znamená hello rozdíl mezi hello předpovědi a hodnota true hello. Kvantifikace měření chyb hello je klíč, protože jsme by jako toofine tune hello modelu a ověření, zda je ve skutečnosti snížení hello chyby. Upřesnění hello modelu lze provést úpravou modelu parametry, které řídí hello učení procesu nebo přidáním nebo odebráním data funkce (označuje tooas [parametry oblouku](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Prakticky, znamená, že jsme může potřebovat tooiterate mezi hello funkce technikům, modelování a modelu zkušební fáze vícekrát, dokud jsme jsou možné tooreduce hello chyba toohello požadované úrovni.

Je důležité, že tooemphasis, že chyba předpovědi hello nebude nikdy nula, protože to se nikdy model, který lze přesně odhadnout každý výsledek. Je však určitého rozsahu chyby, která je akceptovatelná podle hello firmy. Během procesu ověřování hello rádi bychom znali tooensure, že naše chyba předpovědi modelu je v hello úroveň nebo lépe než hello firmy tolerance úrovně. Proto je důležité tooset hello úroveň hello přípustné chyby na začátku hello hello cyklus během hello **problém formulování** fáze.

### <a name="typical-evaluation-techniques"></a>Techniky typické vyhodnocení
Existují různé způsoby, ve které předpovědi chyba měří a kvantifikovány. V této části se zaměříme hello diskuzi na vyhodnocení techniky relevantní tootime řady a v konkrétní pro vyžádání energie prognózy.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE znamená znamenat absolutní chyba procento. S MAPE jsme jsou computing hello rozdíl mezi každou prognózy bodu a skutečnou hodnotu hello tohoto bodu. Potom jsme vyčíslení hello chyb na bodu pomocí výpočtu hello podíl hello rozdíl relativní toohello skutečnou hodnotu. V posledním kroku průměrná jsme tyto hodnoty. matematický vzorec Hello používá pro MAPE je hello následující:

![Vzorec MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*kde A<sub>t</sub> je skutečnou hodnotu hello F<sub>t</sub> je hello předpovězené hodnoty a n je hello prognózy horizont.*

## <a name="deployment"></a>Nasazení
Jakmile jsme nailed dolů hello modelování fáze a ověření výkonu modelu hello jsme připravené toogo do fáze nasazení hello. V tomto kontextu nasazení znamená povolení hello zákazníka tooconsume hello modelu spuštěním skutečné předpovědi na něm ve velkém měřítku. Hello konceptu nasazení je klíč v Azure ML, protože naším hlavním cílem je tooconstantly vyvolání předpovědi jako názvem na rozdíl od toojust získání hello přehledy z hello data. fáze nasazení Hello je hello část, kde povolíme toobe modelu hello využívat ve velkém měřítku.

V rámci kontextu hello prognózy poptávky energie naše cílem je tooinvoke nepřetržitý a pravidelné prognózy při zajištění, že je k dispozici pro hello model čerstvá data a že hello naplánované, že data se odesílají zpět toohello náročné klienta.

### <a name="web-services-deployment"></a>Nasazení webové služby
Hello hlavní nasadit stavební Azure ML je hello webové služby. Toto je hello co nejúčinnější způsob, jak tooenable spotřeby prediktivního modelu v cloudu hello. zapouzdří hello modelu Hello webové služby a zabalí s [dosáhl standardu RESTful](http://www.restapitutorial.com/) rozhraní API (Application Programming Interface). Hello rozhraní API slouží jako součást žádný kód klienta, jak je znázorněno v následujícím diagramu hello.

![Jsme nasazení služby a spotřeba](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Jak je vidět, hello webové služby je nasazena v hello Cortana Intelligence Suite cloudu a mohou být vyvolány pomocí jeho zveřejněné koncový bod REST API. Jiný typ klientů v různých doménách, můžete současně vyvolat hello služby prostřednictvím hello webového rozhraní API. Hello webové služby můžete škálovat také pro podporu tisíce souběžných volání.

### <a name="a-typical-solution-architecture"></a>Architektura typické řešení
Při nasazování vyžádání energie prognózy řešení, jsme mají zájem o nasazení řešení tooend end, která překročí hello předpovědi webové služby a usnadňuje hello celého datového toku. V době hello jsme vyvolání nové prognózy by potřebujeme toomake se, že tento model hello předány s funkcemi aktuální data. Která znamená, že hello nově shromážděných nezpracovaných dat je neustále požity, zpracování a transformovat na hello požadovaná sada na který hello model byl vytvořený funkcí. Na hello stejný čas, jsme chcete toomake hello naplánované pro hello k dispozici data ukončení náročné klientů. Příklad datového toku cyklus (nebo datovém kanálu) je znázorněno v následujícím diagramu hello:

![Spotřeby energie prognózy End tooEnd toku dat](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Toto jsou hello kroky, které provést jako součást hello energie vyžádání prognózy cyklus:

1. Miliony měřidla nasazené data jsou neustále generování spotřebě energie v reálném čase.
2. Tato data se shromažďují a nahrál do úložiště v cloudu (*například*, objektů Blob v Azure).
3. Před zpracováním, nezpracovaná data hello je agregované tooa transformovny nebo regionální úrovni, jak je definována hello firmy.
4. Hello funkce zpracování (viz **Příprava dat a zpracování funkce**) pak probíhá a vytváří hello data, která je vyžadována pro model cvičení nebo vyhodnocování – hello funkce sady dat je uložená v databázi (*např*, SQL Azure).
5. je volána Hello znovu cvičení služby hello toore train model – aktualizovat verzi hello modelu prognózy je trvalé, takže ji můžete použít hello vyhodnocování webové služby.
6. Hello vyhodnocování webové služby je volána podle plánu, který vyhovuje hello požadované prognózy frekvence.
7. Hello naplánované, že data jsou uložena v databázi, která je přístupná hello end spotřeby klienta.
8. Hello spotřeba klient načte hello prognózy, použije ji zpět do hello mřížky a využívá v souladu s případ použití hello vyžaduje.

Je důležité toonote, který tento celý cyklus je zcela automatizovat a spouští podle plánu. Hello orchestraci celý tento cyklus dat lze provést pomocí nástrojů, jako [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-tooend-deployment-architecture"></a>Ukončení tooEnd architektura nasazení služby
V pořadí toopractically nasadit do energie vyžádání prognózy řešení na webu Cortana Intelligence, potřebujeme tooensure že hello požadované součásti jsou vytvořeno a správně nakonfigurován.

Hello následující diagram znázorňuje typické Cortana Intelligence na základě architektury, který implementuje a orchestruje hello cyklus toku dat, který je popsaný výše:

![Ukončení tooEnd architektura nasazení služby](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Další informace o jednotlivých součástí hello a celý architektura hello naleznete toohello šablona řešení energie.

