---
title: "Údržba aaaPredictive letecký s Azure - Cortana Intelligence řešení šablony | Microsoft Docs"
description: "Šablona řešení s Microsoft Cortana Intelligence pro prediktivní údržby v letecký, nástrojů a Transport."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: da863a89d2409a8b56b23066f15b4da13e1cd3d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Cortana Intelligence řešení šablony scénářem pro prediktivní údržby v letecký a jiné firmy
## <a name="executive-summary"></a>Shrnutí
Prediktivní údržby je jedním z hello nejvíce vyžadované aplikací prediktivní analýzy unarguable výhod, jako třeba obrovské množství úsporu nákladů. Cílem této scénářem je poskytuje odkaz pro řešení prediktivní údržby pomocí hello důraz na případy použití hlavní.
Je připravené toogive hello čtečky představu o hello nejběžnější obchodní scénáře prediktivní údržby, výzvy opravňujících pro takové řešení obchodních problémů, potřeba dat toosolve těchto obchodních problémů, prediktivního modelování Tyto údaje a osvědčených postupů pomocí ukázkové architektury řešení řešení toobuild techniky.
Také popisuje hello specifika hello prediktivní modely vyvinutý jako je například funkce technikům, model vyhodnocení vývoj a výkonu. V podstatě této playbook spojuje hello firmy a analytické pokyny potřebné pro úspěšné vývoj a nasazení řešení prediktivní údržby. Tyto pokyny jsou připravené toohelp hello cílovou skupinu vytvoření počáteční řešení pomocí Cortana Intelligence Suite a konkrétně Azure Machine Learning jako počáteční bod v jejich dlouhodobou strategii prediktivní údržby. Hello dokumentaci týkající se Cortana Intelligence Suite a Azure Machine Learning najdete v [Cortana Analytics](http://www.microsoft.com/server-cloud/cortana-analytics-suite/overview.aspx) a [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) stránky.

> [!TIP]
> Technického průvodce tooimplementing Tato šablona řešení, najdete v části [technické příručce toohello Cortana Intelligence řešení šablony pro prediktivní údržby](cortana-analytics-technical-guide-predictive-maintenance.md).
> toodownload diagram, který poskytuje přehled architektury této šablony najdete v části [architektura hello Cortana Intelligence řešení šablony pro prediktivní údržby](cortana-analytics-architecture-predictive-maintenance.md).
> 
> 

## <a name="playbook-overview-and-target-audience"></a>Cílová skupina playbook přehled a cíle
Tato scénářem je uspořádány toobenefit technická a Netechnická cílové skupiny s různými pozadí a zájmů v prostoru prediktivní údržby. Hello playbook zahrnuje obě nejdůležitější aspekty hello různé typy řešení prediktivní údržby a podrobnosti o tom, tooimplement je. obsah Hello jsou rovnoměrně nahrazovat obou toohello cílové skupiny, kdo jsou pouze chtějí porozumět řešení místa a hello typ aplikace, stejně jako ty, kteří hledají tooimplement těchto řešení a jsou proto zájem o technické podrobnosti.

Většina hello obsah v této playbook nepředpokládá předchozí data vědecké účely vědomí nebo odborných znalostí. Některé části hello playbook však bude poněkud vyžadovat znalost datové vědy koncepty toobe sledovat podrobnosti implementace. Vědecké účely úvodní dat na úrovni dovednosti jsou požadované benefit toofully z hello materiály v těchto částech.

první polovinu Hello hello playbook vztahuje Úvod toopredictive údržby aplikace, jak tooqualify řešení prediktivní údržby, kolekce běžně používá případů hello podrobné informace o této obchodního problému, hello dat, které obaluje tyto použití případy a hello firmy výhody implementace těchto řešení prediktivní údržby. Tyto části nevyžadují žádné technické znalosti v doméně hello prediktivní analýzy.

V hello druhé polovině tématu hello scénářem nabídneme hello typy techniky prediktivního modelování pro prediktivní údržby aplikace a jak implementovat tyto modely prostřednictvím příklady z uvedených v první polovině hello hello playbook případy použití hello. Tento koncept je znázorněn přechodem provede kroky pro předzpracování dat, jako je označování dat a funkce technici, model výběr, školení nebo testování a výkonu vyhodnocení osvědčené postupy. Tyto části jsou vhodné pro technické cílovou skupinu.

## <a name="predictive-maintenance-in-iot"></a>Prediktivní údržby v IoT
dopad Hello výpadku neplánovanou zařízení může být velmi destruktivní pro firmy. Je důležité tookeep pole zařízení s v pořadí toomaximize využití a výkonu a současně minimalizujete její nákladná, neplánované výpadky. Čeká na toooccur selhání hello jednoduše, není dostupný v dnešních obchodní operace scény. tooremain produktivní, společností vyhledejte nové způsoby toomaximize asset výkonu tím, že použití hello údajů shromážděných z různých kanálů. Jeden způsob, jak důležité tooanalyze tyto informace je tooutilize prediktivní analýzy techniky využívající historických vzory toopredict budoucí výsledky. Jeden z těchto řešení nejoblíbenější hello se nazývá prediktivní údržby, který může být obvykle definováno jako ale mimo jiné toopredicting možnosti selhání majetku v blízké budoucnosti tak, že hello, který může být prostředky monitorovat tooproactively hello identifikovat chyby a proveďte akce předtím, než dojde k selhání hello. Tato řešení detekovat selhání vzory toodetermine prostředky, které jsou v hello největší riziko selhání. Tuto časná identifikaci problémů pomáhá nasadit prostředky omezené údržby cenově výhodnější způsobem a vylepšení kvality a zadat řetězec procesy.

S hello zvýšení hello Internet věcí (IoT) aplikací, prediktivní údržby má byla získání roste pozornost v odvětví hello jako hello shromažďování dat a zpracování technologie používá dostatek toogenerate přenášet, ukládat a analyzovat všechna druhy dat v dávkách nebo v reálném čase. Tyto technologie umožňují snadný vývoj a nasazení řešení pro kompletní s advanced řešení pro analýzu, se pravděpodobně poskytuje největší benefit hello řešení prediktivní údržby.

Obchodní problémy v rozsahu domény prediktivní údržby hello od vysoce rizikové provozní z důvodu selhání toounexpected a omezené přehledy o hello hlavní příčinu problémů ve složitém firemním prostředí. Hello většina těchto problémů může být zařazené do kategorií toofall pod hello následující obchodní otázky:

* Co je hello pravděpodobnost, že v blízké budoucnosti hello selže zařízení?
* Co je hello zbývající dobu životnosti hello zařízení?
* Jaké jsou hello příčiny chyby a jaké akce údržby musí být provádět toofix tyto problémy?

S využitím tooanswer prediktivní údržby tyto otázky, firmy můžete:

* Snížit provozní riziko a sledování chyb, než k nim došlo zvýšit návratovou hodnotu na prostředky
* Omezit operace nepotřebné založené na čase údržby a řídit náklady na údržbu
* Zvýšit celkový image značky, eliminovat chybný osobnosti a výsledný ztraceny prodejní z otěr zákazníka.
* Nižší poplatky za inventáře snížením úrovně inventáře Odhadnutím toho, jaká bod
* Zjistit problémy při údržbě připojené toovarious vzory

Řešení prediktivní údržby může podnikům s klíčových ukazatelů výkonu, jako je stav skóre toomonitor v reálném čase asset podmínky, odhad zbývající životnost prostředků, doporučení pro proaktivní údržby aktivity hello a Odhadované pořadí data určena k nahrazení částí.

## <a name="qualification-criteria-for-predictive-maintenance"></a>Kvalifikace kritéria pro prediktivní údržby
Je důležité tooemphasize, že všechny případy použití nebo lze prediktivní údržby efektivně vyřešit obchodní problémy. Důležité je, že kritéria kvalifikace zahrnují hello problém je prediktivní ve své podstatě, který již existuje zrušte cestu akce pořadí tooprevent selhání zjištěných předem a co je nejdůležitější – data s dostatečnou hello toosupport kvality používat případu je k dispozici. Zde zaměříme na hello dat požadavky pro vytváření řešení prediktivní údržby úspěšné.

Při vytváření prediktivních modelů, používáme historická data tootrain hello model, který pak rozpoznají skrytá vzory a dále určit tyto vzory v datech budoucí hello. Tyto modely jsou cvičení s příklady popsaného jejich funkcí a hello cíl předpovědi. Hello trained model je očekávané toomake předpovědi na cíli hello pouze na základě funkcí hello nové příklady hello. Je velmi důležitý, že hello modelu zachycení hello vztah mezi funkcí a hello cíl předpovědi. V pořadí tootrain efektivní strojového učení modelu, potřebujeme Cvičná data, která obsahuje funkce, které mají ve skutečnosti prediktivní power směrem hello cíl předpovědi význam hello data by měla být relevantní toohello předpovědi cílem tooexpect přesný předpovědi.

Například pokud hello cíl je selhání toopredict train kol, hello cvičení dat by mělo obsahovat funkce související s wheel (např. telemetrie odrážející hello stav souborů Wheel, hello vzdálenost, car zatížení, atd.). Ale pokud hello cíl je selhání modulu train toopredict, pravděpodobně potřebujeme další sadu Cvičná data, která má funkce související se modul. Před vytvořením prediktivní modely, očekávané hello firmy odborné toounderstand hello data důležitosti požadavek a poskytují znalostní báze hello domény, který je potřebný tooselect relevantní podmnožiny dat pro analýzu hello.

Existují tři základní data zdroje, které vyhledáme při určení toobe problém firmy, která je vhodná pro řešení prediktivní údržby:

1. Selhání historie: V aplikacích prediktivní údržby událostí selhání jsou obvykle velmi výjimečných. Však při sestavování prediktivní modely, které předpovědi selhání, hello algoritmus musí vzoru normálního provozu hello toolearn, jakož i hello selhání vzor procesem hello školení. Proto je nezbytné, aby hello Cvičná data obsahuje dostatečný počet příklady v obou kategorií v pořadí toolearn tyto dvě různé vzorce. Z tohoto důvodu je nutné, aby data mají dostatečný počet událostí selhání. Události chyb naleznete v záznamy údržby a historie nahrazení částí nebo anomálie v hello školení, data lze také použít jako selhání označenou odborníky hello domény.
2. Historie údržby nebo opravit: Základní zdroj dat pro řešení prediktivní údržby je hello údržby podrobné historie hello asset obsahující informace o komponentách hello nahradit, preventivní údržbě aktivuje provádět atd. Je velmi důležité, že tyto události tyto vzory snížení vlivu hello a bez těchto informací způsobí, že oklamání toocapture výsledků.
3. Počítač podmínky: V pořadí toopredict kolik dnů (hodiny, paliva, transakce atd.) na počítač, trvá po předtím, než selže, předpokládáme, stav počítač hello snižuje časem při své činnosti. Proto očekáváme hello data toocontain čas různých funkcí, které zaznamenat tento vzor stárnutí a všechny anomálií, které vede toodegradation. V aplikace či aplikace IoT hello telemetrická data z různých snímače představují jeden dobrým příkladem. V pořadí toopredict Pokud je počítač se bude toofail v časovém rámci, v ideálním případě hello data by měla zachytit ponižující trend během tohoto časového rámce před událostí skutečné selhání hello.

Kromě toho je nutné, data, která je přímo související toohello provozních podmínek hello cílový prostředek předpovědí. rozhodnutí Hello cíle vychází z obou obchodních potřeb a data dostupnosti. Pořízení hello cvičení wheel selhání předpovědi jako příklad může předpovídat jsme "Pokud hello wheel probíhající toohave selhání" nebo "Pokud celý train hello budete mít k chybě". Hello nejdřív jednu cílí více konkrétní součást zatímco druhý cílem selhání hello train. Hello druhá je zadat další obecné otázku, která vyžaduje mnohem víc rozmístěné datové prvky než hello první z nich, což těžší toobuild modelu. Při selhání wheel toopredict podle vzhledu data podmínky vysoké úrovně train hello naopak nemusí být vhodný, protože neobsahuje informace na úrovni součástí hello. Obecně platí je více rozumný události konkrétní chyby toopredict než ty, které další obecné jsou.

Jeden běžné otázku, která se obvykle zobrazí výzva, o data historie selhání je "jak mnoho událostí selhání jsou požadované tootrain modelu a kolik se považuje za"dostatek"? Neexistuje žádná otázka toothat zrušte odpovědí stejně jako mnoho scénářů prediktivní analýzy, je obvykle hello kvalitu hello data, která určuje, jaké jsou přijatelné. Pokud hello datová sada neobsahuje funkce, které jsou relevantní toofailure předpovědi, pak i v případě, že existuje mnoho událostí selhání vytváření správný model nemusí být možné. Však hello pravidlem je, že hello další hello selhání události hello lepší hello je model a kolik selhání příklady jsou požadovány odhad je velmi kontextu a měr závislé na data. Tento problém je popsané v hello části pro zpracování imbalanced datové sady, kde navrhujeme toocope metody s problém hello nemá dostatek selhání.

## <a name="sample-use-cases"></a>Ukázka případy použití
Tato část se zaměřuje na kolekci případy použití prediktivní údržby z několika odvětví, jako je například letecký, nástroje a Transport. Každou podrobněji část do případy použití hello shromážděné z těchto oblastí a popisuje obchodních problémů, hello data okolního hello obchodního problému a hello výhod řešení prediktivní údržby.

### <a name="aerospace"></a>Letecký
#### <a name="use-case-1-flight-delay-and-cancellations"></a>Případ použití 1: Zpoždění letu a zrušení
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Jeden z hello hlavní obchodní problémy, airlines čelí, je hello výrazné náklady, které jsou spojené s letů je zpožděno z důvodu toomechanical problémy. Pokud hello mechanických selhání nelze opravit, může i lety zrušit. To je velmi nákladné, jako je vytváření zpoždění problémy v plánování a operací, chybný pověsti příčiny a nespokojenosti zákazníka spolu s mnoha jiných problémů. Airlines zájem o zvlášť predikci takových mechanických selhání předem, aby se může snížit zpoždění letů nebo zrušení. cílem Hello hello řešení prediktivní údržby pro tyto případy je toopredict hello pravděpodobnost letadla se odložené nebo zrušit, podle příslušné zdroje dat například informace o postupu údržby historie a letu. Hello dvou zdrojů hlavní data pro tento případ použití jsou hello letu nožičky a protokoly stránky. Data pohybující se větev zahrnuje data o podrobnosti o postupu letu hello například hello datum a čas odeslání a přijetí, odeslání a přijetí letištích atd. Data protokolu stránky obsahuje řadu chyb a údržbě kódy, které se zaznamenávají hello údržby pracovníky.

##### <a name="business-value-of-hello-predictive-model"></a>*Obchodní hodnotu hello prediktivního modelu*
Pomocí hello historická data k dispozici, prediktivního modelu bylo vytvořeno prostřednictvím několika klasifikační algoritmus toopredict hello typ mechanických problém, což vede zpoždění nebo zrušení letu v rámci hello dalších 24 hodin. Tím, že tento předpovědi, akce potřebné údržby děláte toomitigate hello riziko při prováděn servis letadel a tím zabránit možné zpoždění nebo zrušení. Pomocí webové služby Azure Machine Learning, prediktivní modely hello lze bez problémů a snadno integrovat do airlines' existující operační platformy. 

#### <a name="use-case-2-aircraft-component-failure"></a>Použití případ 2: Selhání součásti letadla
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Letecké motory jsou velmi citlivé a nákladné kusy zařízení a modul část náhrady mezi hello většiny běžných úloh údržby v nástroji hello letecká společnost odvětví. Řešení údržby pro airlines vyžadují pečlivě správu uložených dostupnost součásti, doručení a plánování. Je možné toogather intelligence na spolehlivost součást vede toosubstantial snížení na investice náklady. Zdroj Hello hlavních dat pro tento případ použití je telemetrická data shromážděná z počet senzory v hello letadla informacemi na hello podmínku letadla hello. Záznamy údržby byly také použít tooidentify při selhání komponent došlo k chybě a bylo provedeno nahrazení.

##### <a name="business-value-of-hello-predictive-model"></a>Obchodní hodnotu hello prediktivního modelu
Klasifikace více třídy modelu bylo vytvořeno který předpovídá pravděpodobnost selhání kvůli tooa určité součásti v rámci hello příští měsíc. Díky využití architektury těchto řešení, airlines může snížit náklady na opravu součásti, zvýšit dostupnost součásti uložené, snížit inventáře úrovně související prostředky a zlepšit plánování údržby.

### <a name="utilities"></a>Nástroje
#### <a name="use-case-1-atm-cash-dispense-failure"></a>Případ použití 1: Selhání obejít bez peněžních ATM
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Vedení v asset náročné odvětví často stavu, je primární provozní riziko tootheir podnikům neočekávané selhání jejich prostředků. Jako příklad selhání stroje například peněžními automaty v bankovnictví odvětví velmi problém je běžný, ke kterému dochází často. Těchto typů problémů vytvořit řešení prediktivní údržby velmi žádoucí pro operátory takové stroje. V tomto případě použít je problém předpovědi toocalculate hello pravděpodobnost, že získá ATM peněžních odvolání transakce přerušena z důvodu selhání tooa v enumenátor peněžních hello například uvíznutí papíru nebo selhání část. Zdroje hlavních dat pro tento případ jsou odečty snímačů, která shromažďují měření při peněžních poznámky jsou právě distribuován a také záznamy údržby shromážděných v čase. Data snímače zahrnuty odečty snímačů na každou transakci byla dokončena a také odečty snímačů za každou Poznámka distribuován. Hello senzor odečty poskytuje měření například mezery mezi poznámky, tloušťka, mějte na paměti příchodem vzdálenost atd. Data údržby zahrnuty kódy chyb a opravit informace. Tyto byly použité tooidentify selhání případech.

##### <a name="business-value-of-hello-predictive-model"></a>*Obchodní hodnotu hello prediktivního modelu*
Dva prediktivní modely byly vytvořené toopredict selhání v hello peněžních odvolání transakce a počet selhání v jednotlivých poznámek hello distribuován během transakce. Tím, že je schopný toopredict selhání transakce předem, může být peněžními automaty servis proaktivně tooprevent selhání výskytu. Navíc s Poznámka předpovědi selhání, pokud transakce je pravděpodobně toofail před jeho dokončením kvůli tooa Poznámka obejít bez chyby, může být nejlepší toostop hello proces a upozornit zákazníka hello neúplné transakce nikoli čekajících na službu údržby hello tooarrive po výskytu chyby hello, což může způsobit nespokojenosti toolarger zákazníka.

#### <a name="use-case-2-wind-turbine-failures"></a>Použití případ 2: Selhání turbína větru
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
S hello vyvolat prostředí povědomí o větru turbín se staly jedním z hlavních zdrojů energie generování hello a jejich obvykle náklady milionů dolarů. Jedním z hello klíčové komponenty větru turbín je generátor motorových hello, což je vybavený s mnoha snímače, které pomáhá toomonitor turbína podmínky a stav. Hello odečty snímačů obsahovat cenné informace, které lze použít toobuild toopredict prediktivního modelu kritické klíčových ukazatelů výkonu (KPI) například toofailure střední čas pro součásti turbína větru hello. Data pro tento případ použití pocházejí z více turbín větru, které se nacházejí ve třech umístěních různé farmy. Měření z zavřete tooa, které set senzorů z každé turbína nebyly zaznamenány každých 10 sekund jeden rok. Tyto odečty zahrnují měření například teploty, generátor rychlost, turbína energie a generátor zrušení.

##### <a name="business-value-of-hello-predictive-model"></a>*Obchodní hodnotu hello prediktivního modelu*
Prediktivní modely byly vytvořené tooestimate generátory a senzory teploty zbývající dobu životnosti. Odhadnutím toho, jaká hello pravděpodobnost selhání údržby, které se technici můžou zaměřit na podezřelé turbín, které jsou pravděpodobně toofail brzy toocomplement založené na čase údržby režimy.
Kromě toho prediktivní modely přepněte přehledy toohello úroveň příspěvku na různé faktory toohello pravděpodobnost selhání, která pomáhá obchodní toohave lépe porozumět hello kořenové způsobit problémy.

#### <a name="use-case-3-circuit-breaker-failures"></a>Případ použití 3: selhání jistič
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Elektrické energie a plynu operace, které zahrnují výrobou, distribucí a prodej elektrické energie vyžadovat značné množství údržby zajistit power řádky provozní vůbec časového tooguarantee doručování toohouseholds energie. Selhání těchto operací je velmi důležité, protože téměř každé entity se provádí power problémy v oblasti hello, které k nim dojde. Moduly okruh dělení jsou důležité pro takové operace, jako jsou zařízení, která vyjmout elektrický aktuální v případě problémů a okruhy krátké tooprevent žádné čáry toopower poškození nedocházelo. Pro tento případ použití obchodního problému je toopredict jistič selhání daného údržby protokoly, historie příkazů a technických specifikací.

Tři zdroje hlavních dat pro tento případ jsou protokoly údržby, které zahrnují preventivní a systematické opravné akce, provozních dat, které obsahuje příkazy pro automatickou a ruční odeslání toocircuit funkce, jako rozdělování pro akce Otevřít a zavřít a technical Specifikace data o vlastnostech každé jistič například roku vytvořit, umístění, modelu, atd.

##### <a name="business-value-of-hello-predictive-model"></a>*Obchodní hodnotu hello prediktivního modelu*
Řešení prediktivní údržby pomáhají omezit náklady opravit a zvýšit životní cyklus hello vybavení, jako jsou moduly okruh dělení. Tyto modely taky pomoct zlepšit kvalitu hello hello power sítě, protože modely poskytování upozornění dopředu, která snižují neočekávané selhání, které vedly toofewer přerušení toohello služby.

#### <a name="use-case-4-elevator-door-failures"></a>Případ použití 4: Hodnocení dveře selhání
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Většina velkým organizacím hodnocení obvykle mají miliony výtahy systémem kolem hello, world. toogain konkurenční výhody, se zaměřit na spolehlivost, což je to důležité většina tootheir zákazníků. Kreslení na hello potenciálně hello Internet věcí připojením jejich výtahy toohello cloudu a shromažďování dat ze senzorů hodnocení a systémů, jsou možné tootransform data do cenné business intelligence, což významně zvyšuje operací nabízí prediktivní a preemptivní údržby, není něco, který je k dispozici toohello konkurenci ještě. Hello firmy požadavek pro tento případ je tooprovide, které způsobí, že znalostní báze knowledge base prediktivní aplikace, který bude předpovídat hello potenciální dveře chyb. Hello vyžaduje data pro tuto implementaci se skládá ze tří částí, které jsou statické funkce hodnocení (například identifikátory smlouvy frekvence údržby, typ sestavení, atd.), informace o využití (například počet cyklů dveře, průměrná dveře zavřít čas atd.) a selhání historie (tj. záznamů historických selhání a jejich příčin).

Více třídami logistic regresní model byl sestaven s Azure Machine Learning toosolve hello předpovědi problém, s hello integrované statické funkce a data o využití jako funkce a hello příčiny záznamů historických selhání jako třída popisky. Tento model prediktivní obsazením aplikací na mobilním zařízení, který je používán pole technici toohelp zlepšení efektivity práce. Když připravuje technik a zadává přejde na web toorepair hodnocení, si mohou odkazovat toothis aplikace doporučené příčiny a doporučené kurzy údržby akce toofix hello hodnocení dveří tak rychlý jako možné.

### <a name="transportation-and-logistics"></a>Transport a logistiky
#### <a name="use-case-1-brake-disc-failures"></a>Případ použití 1: Selhání disku brzdy
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Zásady údržby typické pro vozidel obsahují opravné a preventivní údržby. Opravné údržby znamená, že tento vehicle hello je opravit po došlo k selhání, což může vyvolat ovladač toohello závažné potíže v důsledku neočekávané selhání a hello času ke znehodnocení části na návštěvu toomechanic. Většina vozidel jsou také subjektu tooa preventivní údržbě zásad, které vyžaduje provedení některé kontroly podle plánu, který nevyžaduje do účtu hello skutečný stav subsystémy car hello. Žádný z těchto postupů se úspěšná ve plně odstraňuje problémy. Zde případ konkrétní použití Hello je brzdy disk selhání předpovědi podle údaje shromážděné prostřednictvím nainstalovaných v systému můžete zadat hello Auto, která uchovává informace o historických řízení vzory a jiných podmínek, které hello car zpřístupněn. Hello nejdůležitější zdroj dat pro tento případ je hello senzor data, která měřit, například zrychlení, brzdění vzory, řídí vzdálenosti, rychlosti atd. Tyto informace vázány jiné statické informace, jako je funkce car nápovědy sestavení dobrou sada prognostické, které mohou být používány prediktivního modelu. Další sadu základní informace, které je hello selhání data, která je odvodit z databáze pořadí část (používá tookeep hello k výměně za chodu část pořadí data a počty se jako aut jsou probíhá údržba v dealerům hello).

##### <a name="business-value-of-hello-predictive-model"></a>*Obchodní hodnotu hello prediktivního modelu*
Hello firmy hodnota prediktivní přístupu je podstatné. Prediktivní údržby systému můžete naplánovat prodejce toohello návštěvu podle prediktivního modelu. Hello modelu může být založen na senzorických informace, které je představující aktuální stav hello hello car a hello řídí historie. Tuto metodu můžete minimalizovat riziko hello neočekávaných chyb, které může také dojít před hello další periodické údržby.
Může také snížit množství hello nepotřebné preventivní údržby. Ovladač můžete proaktivně informováni, že v několika týdny a zadejte hello obchodník s touto informací může být nezbytné změnu částí. Hello obchodník poté může předem připravit balíček jednotlivých údržby pro ovladač hello.

#### <a name="use-case-2-subway-train-door-failures"></a>Případ použití 2: Podzemní dráha train dveře selhání
##### <a name="business-problem-and-data-sources"></a>*Obchodní problém a zdroje dat.*
Jedním z hlavních důvodů hello zpoždění a problémy na operace podzemní dráha je selhání dveře train automobilů. Predikci Pokud automobilu train může mít selhání dveře nebo se může tooforecast hello počet dnů do hello další door nezdaří, je velmi důležité předvídání. Poskytuje hello možnost toooptimize train dveře údržby a snížit hello train výpadek.

#### <a name="data-sources"></a>Zdroje dat
Jsou tři zdroje dat v tomto případě použijte 

* **cvičení data události**, což je hello staré záznamy train událostí 
* **data údržby** například údržby typy, typy pořadí pracovních a kódy s prioritou  
* **zaznamenává chyby**.

##### <a name="business-value-of-hello-predictive-model"></a>*Obchodní hodnotu hello prediktivního modelu*
Dva modely byly vytvořené toopredict další den selhání pravděpodobnosti pomocí binární klasifikace a počet dnů do selhání pomocí regrese. Podobná hello starší případech hello modely vytvořte strávíte možnost tooimprove kvality služeb a vzájemně doplňuje hello pravidelné údržby režimů zvýšit spokojenost zákazníků.

## <a name="data-preparation"></a>Příprava dat
### <a name="data-sources"></a>Zdroje dat
Hello běžné datové prvky pro prediktivní údržby problémy lze shrnout takto:

* Selhání historie: hello historie selhání počítače nebo součást v rámci hello počítače.
* Historie údržby: hello oprava historie počítače, např. kódy chyb, předchozí aktivity údržby nebo součást nahrazení.
* Počítače podmínky a využití: hello provozních podmínek počítače např dat shromážděných ze senzorů.
* Počítač funkce: funkce hello počítače, například modul velikost, značka a model, umístění.
* Operátor funkce: hello funkce operátoru hello, např. pohlaví, zkušenosti.

Je možné, obvykle hello případ selhání historie je součástí historie údržby, jako v podobě hello speciální chybové kódy nebo pořadí data pro náhradní díly. V takových případech můžete z dat údržby hello extrahovat selhání. Kromě toho mohou mít jiné organizační domény různých dalších zdrojů dat, které ovlivňují selhání vzorců, které zde nejsou uvedeny úplné. Tyto mělo by být určené v součinnosti s odborníky hello domény při vytváření prediktivních modelů.

Některé příklady výše datové prvky z případy použití jsou:

Selhání historie: boje zpoždění data, letadla součást selhání data a typy, selhání transakce za stažení peněžních ATM, selhání dveře train nebo hodnocení, brzdy disku nahrazení pořadí data, větru turbína selhání data a jistič příkaz selhání.

Historie údržby: protokoly chyb letu, chyba ATM transakcích cvičení údržby záznamů, včetně typu údržby, krátký popis atd. a jistič údržby záznamy.

Počítače podmínky a využití: letu tras a časy, data shromážděná z letecké motory odečty snímačů z transakcí ATM cvičení data událostí, odečty snímačů z větru turbín, silech a připojené aut.

Počítač funkce: jistič technických specifikací například napětí úrovně, informace o zeměpisné poloze nebo car funkce jako je například Ujistěte se, modelu, modul velikost, můžete zadat typy zařízení produkční atd.

Hello uvedených zdrojů dat, jsou hello dva hlavní typy dat, které v doméně prediktivní údržby se sledují dočasná data a statických dat.
Selhání historie, počítač podmínky, opravte historie, téměř vždy historie využití přijde s časovými razítky označující hello čas kolekce pro jednotlivá data. Funkce počítačů a operátor funkce jsou obecně statické vzhledem k tomu obvykle popisují hello technických specifikací počítače nebo operátor na vlastnosti. Je možné pro tyto funkce toochange přes čas a pokud tak jejich by měl být považován za časovým razítkem zdroje dat.

### <a name="merging-data-sources"></a>Slučování zdroje dat
Před získáním do libovolného typu funkce inženýrství nebo označování proces, potřebujeme toofirst připravit naše data v funkcí toocreate formuláře požadované hello. Hello konečným cílem je toogenerate záznam pro každou časovou jednotku pro každý prostředek s její funkce a popisky toobe předány do hello algoritmu strojového učení. V pořadí tooprepare, který odstranit závěrečné sady dat třeba vzít některé kroky předběžného zpracování. Prvním krokem je toodivide hello trvání shromažďování dat do jednotky doby, kdy každý záznam patří tooa časovou jednotku pro určitý prostředek. Shromažďování dat je možné také rozdělit do jiné jednotky jako je například akce, ale pro jednoduchost používáme jednotky doby pro hello zbytek vysvětlení hello.

může být Hello měrné jednotky pro dobu v sekundách, minut, hodiny, dny, měsíce, cykly, miles nebo transakce v závislosti na hello efektivitu Příprava dat a změn hello zjištěnými v hello podmínky hello asset z toohello jednotky a čas další či jiné faktory konkrétní toohello domény. Jinými slovy hello časovou jednotku nemá toobe hello stejná jako četnost hello shromažďování dat jako v mnoha případech data nemusí zobrazit žádný rozdíl z jedné jednotky toohello jiné. Například pokud teploty hodnoty byly shromažďovaných každých 10 sekund, výběr časovou jednotku 10 sekund pro analýzu celého hello zvýšení kapacity hello počet příklady bez zadání jakýchkoli dalších informací. Lepší strategie by toouse průměr přes hodinu jako příklad.

Příklad schémata obecných datových zdrojů dat. hello podrobně hello jsou výše uvedené části:

Zaznamenává údržby: Toto jsou záznamy hello akce údržby provést. nezpracovaná data údržby obvykle dodává s ID prostředku a časové razítko s informacemi o jaké aktivity údržby prováděly v daném čase Hello. V případě takové nezpracovaná data třeba údržby aktivity toobe přeložit do kategorií sloupců s každou kategorii odpovídající tooa údržby typ akce. schéma Hello základní data pro záznamy údržby bude zahrnovat sloupce asset ID, čas a údržby akce.

Selhání záznamů: Toto jsou hello záznamy, které patří toohello cíl předpovědi, který nazýváme selhání nebo důvod selhání. To mohou být specifické chybové kódy nebo události chyb definované konkrétní obchodní podmínky. V některých případech data obsahují více kódy chyb některé z nich odpovídají toofailures, které vás zajímají. Ne všechny chyby je, že cíl předpovědi tak další chyby jsou obvykle používá tooconstruct funkce, které mohou souviset s chybami. Hello základní data schématu pro selhání záznamy by mělo zahrnovat asset ID, čas a selhání nebo sloupce Důvod selhání, pokud důvod je k dispozici.

Počítač podmínky: Jedná se o ideálně sledování v reálném čase data o hello provozních podmínek dat hello. Například selhání dveře otevírání dveří a uzavírací časy jsou dobrou indikátory o aktuální podmínce hello dveří. schéma Hello základní data pro počítače podmínek bude zahrnovat asset ID, čas a podmínky hodnoty sloupců.

Data počítače a operátor: počítač a operátor data mohou být sloučeny do tooidentify jedno schéma, které prostředek byl provozují operátoru společně s vlastností prostředku a operátor. Například automobilu obvykle vlastníkem je ovladač s atributy, jako je stáří, řídí prostředí atd. Pokud se tato data se změní v čase, tato data musí rovněž zahrnovat sloupec čas a považovat za čas různých dat pro funkci generování. Hello základní data schématu pro počítač podmínky by zahrnují ID prostředku, funkce asset, operátor ID a operátor funkce.

poslední tabulku Hello před označování a funkce generování může být generována vlevo propojení počítače podmínky tabulky se záznamy selhání na ID prostředku a čas. Tato tabulka pak lze propojit se záznamy údržby na ID prostředku a čas polí a nakonec se počítač a operátor funkce na ID prostředku. první levé spojení Hello opustí hodnoty null pro sloupec selhání, když je počítač v normálním provozu, tyto můžete nebude splněn hodnotou indikátor pro běžné operace. V tomto sloupci selhání je použité toocreate popisky pro prediktivní model hello.

### <a name="feature-engineering"></a>Návrh funkcí
Hello prvním krokem při modelování je funkce inženýrství. Rada Hello funkce generace je tooconceptually popisují a abstraktní podmínka stavu počítače a v daném okamžiku pomocí historických dat, která nebyla shromážděna až toothat bodu v čase. V další části hello poskytujeme přehled hello typu technik, které lze použít pro prediktivní údržby a označování hello jak probíhá pro každý postup. Hello přesný postup, který se má použít, závisí na hello dat a obchodních problémů. Metody engineering hello funkce popsané níže však může být použit jako účaří pro vytváření funkcí. Níže probereme prodleva funkce, které musí zkonstruovat z datových zdrojů, které obsahují časová razítka a také statické funkce, které jsou vytvořené z statických dat zdroje a poskytují příklady z hello případy použití.

#### <a name="lag-features"></a>Funkce Lag funkce
Jak už bylo zmíněno dříve, v prediktivní údržby, historická data obvykle dodává s časová razítka označující hello čas kolekce pro jednotlivá data. Vytvoření funkce z hello dat, která se dodává s označen časovým razítkem data mnoha způsoby. V této části probereme některé z těchto metod pro prediktivní údržby. Ale jsme nejsou omezeny tyto metody samostatně. Vzhledem k tomu, že funkce inženýrství považuje za toobe mezi největší kreativitu oblasti hello prediktivního modelování, může být řadu dalších funkcí toocreate způsoby. Zde jsou některé obecné postupy.

##### <a name="rolling-aggregates"></a>*Vrácení agregace*
Pro každý záznam prostředek jsme vyberte okno postupného velikosti "W", což je hello počet jednotek času, který rádi bychom znali toocompute historických agregace pro. Potom výpočetní vrácení agregační funkcí s použitím hello W období před datem hello daného záznamu. Několik příkladů vrácení agregace můžete vrácení, počty, znamená, standardních odchylek, extrémních podle standardních odchylek, měří CUSUMŮV, minimální a maximální hodnoty pro okno. Jiné zajímavé technika je toocapture trend změny, špičky a úrovně změny pomocí algoritmů, které zjišťovat anomálie v dat pomocí algoritmů detekce anomálií.

Pro demonstrační, viz obrázek 1 kde jsme představují hodnoty čidel zaznamenány pro určitý prostředek pro každou jednotku času s hello modré čáry a označit hello vrácení průměrná funkci výpočtu pro W = 3 hello záznamů v t<sub>1</sub> a t<sub>2 </sub> které jsou označeny oranžová a zelená seskupení v uvedeném pořadí.

![Obrázek 1. Vrácení agregační funkce](media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png)

Obrázek 1. Vrácení agregační funkce

Hodnoty čidel z poslední týden, poslední tři dny a poslední den jako příklady pro letadla selhání součásti, byly použité toocreate vrácení znamená, směrodatná odchylka a funkce sum. Podobně pro znamená ATM selhání, nezpracovaná senzor hodnoty a vrací medián, rozsah, standardních odchylek počet extrémních nad rámec tři standardních odchylek, horní a dolní CUMSUM funkce byly použity.

Pro předpověď zpoždění letu počet kódů chyb z poslední týden byly použité toocreate funkce. Selhání train dveří počty hello událostí na hello poslední den počítá událostí přes hello předchozí 2 týdny a odchylku počtu událostí hello předchozích 15 dnů byly použité toocreate prodleva funkce. Stejné počítání byl použit pro události související s údržbou.

Kromě toho výběrem W je moc velké (např.) letopočty), je možné zaznamenává toolook na celou historii hello prostředek, jako je například počítání všechny údržby, selhání atd. až se hello čas hello záznamu. Tato metoda se používá pro počítání jistič chybami hello poslední tři roky. Pro selhání train také všechny události údržby měla počítají toocreate toocapture funkce hello dlouhodobé údržby účinky.

##### <a name="tumbling-aggregates"></a>*Přeskakující agregace*
Pro každý s popiskem záznam majetku vybereme okno velikosti "W -<sub>tisíc</sub>" kde tisíc je číslo hello nebo windows velikost "W", že má být toocreate funkce lag funkce. jako velký počet toocapture dlouhodobé snížení vzory nebo malý počet krátkodobé důsledky toocapture mohou být zachyceny "k". Používáme k přeskakující windows W -<sub>tisíc</sub> , W -<sub>(k-1)</sub>,..., W -<sub>2</sub> , W -<sub>1</sub> toocreate agregační funkce dobu hello před hello záznam Datum a čas (viz obrázek 2). Tyto jsou také vrácení windows na úrovni hello záznamů pro časovou jednotku, která není zaznamenaná na obrázku 2, ale hello Rada je stejný jako obrázek 1 hello kde t<sub>2</sub> je také použít toodemonstrate hello vrácení vliv.

![Obrázek 2. Přeskakujícího agregační funkce](media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png)

Obrázek 2. Přeskakujícího agregační funkce

Jako příklad větru turbín, W = 1 a k = 3 měsíce byly funkce lag toocreate použité pro každou hello posledních 3 měsíců pomocí horní a dolní odlehlé hodnoty.

#### <a name="static-features"></a>Statické funkce
Jedná se o technických specifikací hello vybavení, jako je například datum výroby, číslo modelu, umístění atd. Funkce opoždění jsou většinou číselné ve své podstatě, bude statické funkcí obvykle kategorií proměnných v modelech hello. Jako příklad jistič vlastnosti, například napětí, aktuální a specifikace power společně s typy transformer používaly zdrojů energie atd. Brzdy selhání disku hello typ souborů Wheel můžete zadat takový jako, pokud jsou slitiny nebo oceli používaly jako některé z funkcí statické hello.

Během generování funkce je třeba provést některé důležité kroky jako je například zpracování chybějící hodnoty a normalizace. Chybí hodnota imputace a také normalizaci data, která není tady popisovaných mnoha způsoby. Je však výhodné tootry různé metody toosee Pokud je možné zvýšit výkonnost předpovědi.

Tabulka konečné Funkce Hello po funkce technici kroky popsané v hello výše uvedené části by měla vypadat přibližně hello následující příklad schéma dat po časovou jednotku za den:

| ID prostředku | Čas | Funkce sloupců | Štítek |
| --- | --- | --- | --- |
| 1 |1 den | | |
| 1 |Den 2 | | |
| Tlačítka ... |Tlačítka ... | | |
| 2 |1 den | | |
| 2 |Den 2 | | |
| Tlačítka ... |Tlačítka ... | | |

## <a name="modeling-techniques"></a>Techniky modelování
Prediktivní údržby je velmi bohaté doména často nasazení obchodních otázek, které může použijí z mnoha různých úhlů hello prediktivní modelování perspektivy. V dalších částech hello poskytujeme hlavní technik, které jsou používané toomodel různých obchodních otázek, které můžete zodpovězeny s řešení prediktivní údržby. I když existují podobnosti, každý model má svou vlastní způsob vytváření popisky, které jsou podrobně popsány. Jako prostředek doprovodné najdete toohello šablona prediktivní údržby, který je součástí hello ukázkových experimentů uvedené v Azure Machine Learning. online materiálu toohello Hello odkazy pro tuto šablonu jsou uvedeny v části prostředky hello. Uvidíte, jak se používají některé funkce hello technici techniky popsané výše a hello modelování techniku, která je popsána v následujících částech hello toopredict letadla modul selhání pomocí Azure Machine Learning.

### <a name="binary-classification-for-predictive-maintenance"></a>Binární klasifikace pro prediktivní údržby
Binární klasifikace pro prediktivní údržby je použité toopredict hello pravděpodobnost, že zařízení selže a v budoucích časovém intervalu. Hello časové období je dáno a na základě obchodní pravidla a hello dat po ruce. Některé běžné časových období jsou minimální realizace čas potřebný toopurchase náhradních dílů tooreplace pravděpodobně toodamage součásti nebo čas požadované toodeploy údržby prostředky tooperform údržby rutiny toofix hello problém, který je pravděpodobně toooccur v rámci které časové období. Říkáme tohoto období budoucí horizon "X".

V pořadí toouse binární klasifikaci potřebujeme dva typy tooidentify příklady, které říkáme kladné a záporné. Každý příkladem je záznam, který patří tooa časovou jednotku pro určitý prostředek koncepčně popisující a poskytuje abstrakci jeho provozních podmínek se toothat časovou jednotku prostřednictvím funkce inženýrství použití zdrojů historických a dalších dat popsané výše. V kontextu hello binární klasifikace pro prediktivní údržby, kladné typ označuje selhání (popisek 1) a záporná typ označuje normální provozní podmínky (Popisek = 0) kde popisky jsou typu kategorií. cílem Hello je toofind model, který identifikuje každý nový příklad jako pravděpodobně toofail a fungovat normálně v rámci hello vedle X jednotkami času.

#### <a name="label-construction"></a>Popisek konstrukce
V pořadí toocreate prediktivního modelu tooanswer hello otázka "Jaký je hello pravděpodobnost, že hello asset selže v hello vedle X jednotkami času?", označování se provádí pořízení X záznamů před toohello selhání prostředek a označování je jako "o toofail" (popisek = 1) Při označování všechny záznamy jako "normální" (Popisek = 0). Tato metoda popisky jsou kategorií proměnné (viz obrázek 3).

![Obrázek 3. Označování pro binární klasifikaci](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png)

Obrázek 3. Označování pro binární klasifikaci

Zpoždění letů a zrušení X vydán jako jeden den toopredict zpoždění při hello dalších 24 hodin. Všechny lety, které jsou v rámci 24 hodin, než selhání byly označeny jako hodnotami 1. Pro ATM peněžních obejít bez chyb, dva modely binární klasifikace byly vytvořené toopredict hello pravděpodobnost selhání transakce v hello příštích 10 minutách a také toopredict hello pravděpodobnost selhání v hello další 100 poznámky distribuován. Všechny transakce, které bylo provedeno v rámci hello posledních 10 minut hello selhání jsou označeny jako 1 pro první model hello. A všechny poznámky distribuován v rámci hello poslední 100 poznámky k selhání byly označený jako 1 pro model druhý hello. Pro jistič selhání úkolů hello je toopredict hello pravděpodobnost, že hello další jistič příkaz selže, v takovém případě X je zvolen budoucí příkaz toobe jeden. Pro selhání train dveří byl hello binární klasifikace model vytvořený toopredict selhání v rámci hello příštích 7 dní. Selhání turbína větru jste vybrali X jako 3 měsíce.

Větru turbína a train dveře případech se také používají pro použití zbývající dobu životnosti toopredict regrese analysis server hello stejná data, ale s využitím různých označování strategie, který je vysvětlen v další části hello.

### <a name="regression-for-predictive-maintenance"></a>Regrese pro prediktivní údržby
Modely Regrese v prediktivní údržby jsou použité toocompute zbývající dobu životnosti (RUL) prostředek, který je definován jako předtím, než dojde k další chybě hello je funkční hello množství času, který hello asset. Stejné jako binární klasifikace, každý příkladem je záznam, který patří tooa časovou jednotku "Y" pro určitý prostředek. V kontextu hello regrese, je cílem hello toofind model, který vypočítá hello zbývající dobu životnosti každý nový příklad jako průběžné číslo, které je hello dobu zbývající před selháním hello. Říkáme tohoto období některé více Y. Každý příklad má také zbývající dobu životnosti, což může být vypočítána hello množství času, například před selháním další hello zbývající.

#### <a name="label-construction"></a>Popisek konstrukce
Zadané hello otázka "Jaký je hello zbývající dobu životnosti hello zařízení?
", popisků pro regresní model hello konstruovat tak, že každý záznam předchozí toohello selhání označování je pomocí výpočtu, kolik jednotek času zůstat před selháním další hello. Tato metoda popisky jsou průběžné proměnné (viz obrázek 4).

![Obrázek 4. Označování pro regresní](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png)

Obrázek 4. Označování pro regresní

Jiné než binární klasifikace pro regresní, prostředky bez jakýchkoliv potíží v datech hello nelze použít pro modelování označování se provádí v referenční bod selhání tooa a jeho výpočet není možné, aniž by věděly, jak dlouho asset hello v zůstal naživu před došlo k chybě. Tento problém je nejlépe řešit jiné statistické technika nazývají základními analýzy.
Nebudete jsme, že toodiscuss základními analýzy v této scénářem z důvodu hello potenciální komplikace, které mohou nastat při použití hello technika toopredictive údržby použít případů, které se týkají různých čas dat pomocí pravidelných intervalech.

### <a name="multi-class-classification-for-predictive-maintenance"></a>Klasifikace více tříd pro prediktivní údržby
Klasifikace více tříd pro prediktivní údržby můžete použít k předvídání dvě budoucí výsledky. Hello nejprve jeden je tooassign tooone asset Dobrý den několik možných období čas toogive řadu toofailure čas pro každý prostředek. Hello druhá je tooidentify hello pravděpodobnost selhání budoucí období včas tooone hello více hlavní příčiny. Umožňuje údržby pracovníky, kteří jsou vybaveny tento znalostní báze pro zpracování hello problémy předem. Jiné technika modelování více tříd se zaměřuje na určení hello nejpravděpodobnější hlavní příčinu základě selhání. To umožňuje toobe doporučení pro hello nejvyšší údržby akce toobe prováděné v pořadí toofix selhání zadané.
Tak, že seřazený seznam kořenové způsobí, že a přidružené akce opravy  
Technici může být efektivnější v trvá jejich první akce opravit po selhání.

#### <a name="label-construction"></a>Popisek konstrukce
Zadané hello dva dotazy, které jsou "co je hello pravděpodobnost, že prostředek v hello další"aZ"jednotkami času selže tam, kde"a"je hello počet období" a "co je hello pravděpodobnost, že hello asset v hello vedle X jednotkami času selže kvůli tooproblem" P<sub>i </sub>"kde"i"je hello počet možných příčinách označování se provádí v následujících způsob, jak tyto tootechniques hello.

Pro první otázku hello, označování se provádí tak, že záznamy aZ před selháním hello majetku označování je pomocí sad času (3Z, 2Z, Z) jako jejich popisky při označování všechny záznamy jako "normální" (Popisek = 0). Tato metoda je popisek kategorií proměnná (viz obrázek 5).

![Obrázek 5. Označování pro více třídami klasifikaci pro předpověď časové selhání](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png)

Obrázek 5. Označování pro více třídami klasifikaci pro předpověď časové selhání

Pro druhou otázku hello, označování se provádí pořízení X záznamů před selháním hello prostředek a označování je jako "o toofail z důvodu problému P<sub>i</sub>" (popisek = P<sub>i</sub>) při označování všechny záznamy jako " Normální"(Popisek = 0). Tato metoda popisky jsou kategorií proměnné (viz obrázek 6).

![Obrázek 6. Označování pro více třídami klasifikaci pro kořenové příčina předpovědi](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png)

Obrázek 6. Označování pro více třídami klasifikaci pro kořenové příčina předpovědi

Hello model přiřadí pravděpodobnost selhání kvůli tooeach P<sub>i</sub> a také hello pravděpodobnost bez chyby. Tyto pravděpodobnostech lze provést řazení podle předpovědi tooallow rozsahem hello problémů, které jsou pravděpodobně toooccur v budoucnu hello. Případ použití selhání součásti letadla se strukturovaná jako více třídami klasifikace problému. To umožňuje hello předpovědi hello pravděpodobnostech selhání kvůli tootwo různých přetížení ventil součásti vyskytující se v hello příští měsíc.

U akce údržby doporučujeme po selhání, označování nevyžaduje, aby budoucí horizon toobe zaznamenání. To je proto hello modelu není predikci selhání v hello budoucí ale ho je právě předpovídá nejpravděpodobnější příčiny hello po selhání hello bylo provedeno. Hodnocení dveře selhání spadají do hello třetí případě, kdy cílem hello toopredict příčinu chyby hello zadané historická data týkající se provozních podmínek. Tento model se pak použije toopredict hello nejpravděpodobnější hlavní příčiny po došlo k selhání. Klíčovou výhodou tohoto modelu je, že pomáhá tooeasily používáním technici diagnostikovat a opravit problémy, které byste jinak potřebovali za roky prostředí.

## <a name="training-validation-and-testing-methods-in-predictive-maintenance"></a>Školení, ověřování a testování metody v prediktivní údržby
V prediktivní údržby, podobně jako tooany jiné řešení místo obsahující označen časovým razítkem data, hello typické trénování a testování běžné potřeby tootake účet hello čas různých aspektů toobetter generalize neviditelný budoucí data.

### <a name="cross-validation"></a>Křížové ověření
Mnoho algoritmy strojového učení závisí na několika hyperparameters, který může významně změní výkonu modelu. Hello optimální hodnoty těchto hyperparameters nejsou počítaný automaticky při tréninku modelu hello, ale musí být určena vědecký pracovník data. Nalezení správné hodnoty hyperparameters několika způsoby. Hello nejběžnější jeden je "tisíc násobek křížové ověření" což rozdělí hello příklady náhodně na složení "k". Pro každou sadu hodnot hyperparameters je algoritmus učení časy spuštění kB. Při každé iteraci hello příklady v aktuální násobek hello jsou použity jako sada ověření, hello zbytek hello příklady jsou použity jako sada školení. algoritmus Hello soupravy přes příklady školení a metriky výkonu hello se vypočítávají v průběhu ověřování příklady. Na konci hello této smyčky pro každou sadu hodnot hyperparameter jsme výpočetní průměr hodnot metriky výkonu hello tisíc a zvolte hyperparameter hodnoty, které mají nejlepší výkon průměrná hello.

Jak je uvedeno nahoře, v prediktivní údržby problémy, data se zaznamenává jako časové řady událostí, které pocházejí z několika zdrojů dat. Tyto záznamy lze provést řazení podle času toohello označování záznam nebo příklad. Proto pokud jsme rozdělit hello datovou sadu náhodně na školení a ověření, některé příklady školení hello jsou později v čase než některé příklady ověření. Výsledkem je odhad budoucích výkonu hyperparameter hodnot na základě hello dat, které byly přijaty předtím, než byla cvičení modelu. Tyto odhady může být příliš optimistickou metodu, zejména v případě, že časové řady nejsou stojící a jejich chování časem změnit. V důsledku toho může být vybraný hyperparameter hodnoty optimální.

Lepší způsob hledání správné hodnoty hyperparameters je toosplit hello příklady do školení a ověření způsobem závislá na čase, nastavte tak, že jsou všechny příklady ověření v čase pozdější, než všechny příklady školení. Pro každou sadu hodnot hyperparameters jsme pak cvičení hello algoritmus přes trénovací sada, měření výkonu modelu přes hello stejné ověření nastavit a zvolte hyperparameter hodnoty, které se zobrazí hello nejlepší výkon. Pokud data časové řady není stojící a vyvíjí v průběhu času, hello hyperparameter hodnoty zvolené podle train a ověření rozdělení realizace tooa lepší budoucí "modelu výkon než hodnotami hello náhodně vybere podle křížové ověření.

poslední model Hello je generován cvičení algoritmu učení přes celou dat pomocí hello nejlepší hyperparameter hodnoty, které se nacházejí pomocí školení a ověření rozdělené nebo křížové ověření.

### <a name="testing-for-model-performance"></a>Testování výkonu modelu
Po sestavení modelu potřebujeme tooestimate jeho budoucí výkon na nová data. Nejjednodušší odhad Hello může být hello výkonu hello modelu přes hello Cvičná data. Ale tento odhad příliš optimistickou metodu, protože model je šité na míru toohello data, která jsou použité tooestimate výkonu. Lepší odhad může být metriky výkonu hodnot hyperparameter vypočítán na sadu ověření hello nebo metriku výkonu průměrná vypočítaný z křížové ověření. Ale pro hello stejné důvodů, proč jak je uvedeno, jsou tyto odhady stále příliš optimistickou metodu. Potřebujeme realističtější přístupy pro měření výkonu modelu.

Jedním ze způsobů je toosplit hello data náhodně do školení, ověřování a testování sad. Hello školení a ověřování, jsou hodnoty používané tooselect hyperparameters a train model hello s nimi. výkon Hello modelu se měří v hello testovací sady.

Dalším způsobem, který je relevantní toopredictive údržby, je toosplit příklady do cvičení, ověřování a testování sad způsobem závislá na čase, tak, aby všechny testovací příklady jsou v čase pozdější, než všechny příklady školení a ověření. Po rozdělení hello, se provádějí měření generování a výkonu v modelu hello stejná jak bylo popsáno výše.

Po časové řady na místě a snadno toopredict obou přístupů generovat podobné Odhady budoucí výkonu. Ale po časové řady-stojícího vozidla nebo pevné toopredict se druhý přístup hello bude generovat realističtější odhad budoucích výkonu než hello první z nich.

### <a name="time-dependent-split"></a>Rozdělení závislá na čase
Jako osvědčený postup v této části jsme trvat bližší pohled na tom, jak implementovat rozdělení závislá na čase. Jsme popisují obousměrný rozdělení závislá na čase mezi učení a testovací sady, ale přesně hello stejné logiky, která má být použita pro závislá na čase rozdělení pro školení a ověření sady.

Předpokládejme, že máme označen časovým razítkem události například měření z různých snímače datového proudu. Funkce školení a příklady test, jakož i jejich popisky jsou definované ve stanovených časových rámců, které obsahují více událostí.
Například pro binární klasifikaci, jak je popsáno v části funkce techniků a modelování techniky, funkce se vytvářejí na základě posledních hello události a popisky se vytvářejí na základě budoucí události v rámci "X" jednotkami času v hello budoucí. Proto hello označování časový rámec příkladu dodává později pak hello časový rámec jeho funkcí. Pro rozdělení závislá na čase jsme vyberte bod v čase, kdy jsme trénování modelu s ujít hyperparameters pomocí historických dat si toothat bodu. tooprevent úniku budoucí popisky, které jsou nad rámec hello školení oříznutím do Cvičná data, vybereme možnost hello nejnovější časový rámec toolabel školení příklady toobe X jednotky před datem oříznutím hello školení. Na obrázku 7 každý plný kroužek představuje řádek v hello konečné funkce datové sady, pro které hello funkce a popisky se vypočítávají podle toothe metody popsané výše. Který zadána hello obrázku hello záznamy, které patří do trénování a testování sad při implementaci závislá na čase rozdělení pro X = 2 a W = 3:

![Na obrázku 7. Závislá na čase rozdělení pro binární klasifikaci](media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png)

Na obrázku 7. Závislá na čase rozdělení pro binární klasifikaci

Hello zelená čtverce představují záznamy hello patřící toohello časové jednotky, které lze použít pro školení. Jak je popsáno výše, každý školení příkladu hello konečné funkce tabulky je generován prohlížení po 3 období pro funkci generace a 2 budoucí období pro označování před oříznutím den školení. Jsme příklady nepoužívejte v školení hello nastavená, pokud libovolná součást hello 2 budoucí období, například je nad rámec hello školení oříznutím vzhledem k tomu, že předpokládáme, že jsme nemají viditelnost nad rámec oříznutím školení. Z důvodu omezení toothat černým příklady představují záznamy hello hello konečné s názvem bez přípony datovou sadu, která se nesmí použít v hello trénovací datové sady. Tyto záznamy se nesmí použít data, která buď protože jsou před oříznutím hello školení a jejich označování stanovených časových rámců částečně závisí na časovém rámci hello školení, který by neměl být případ hello jako rádi bychom znali toocompletely oddělit označování časové rámce pro testování trénování a testování úniku informací tooprevent popisek.

Tento postup umožňuje překrývají v hello data pro funkce vytváření mezi trénování a testování příklady, které jsou oříznutím zavřít toothe školení. V závislosti na dostupnost dat lze provést i závažnější oddělení pomocí není některé příklady hello v testovací sadě, které jsou v rámci jednotky doby W světelného hello školení.

Z našich pracovních jsme zjištěno, že problém úniku hello více vážně týká regrese modely použité pro predikci zbývající dobu životnosti a pomocí náhodného rozdělení vede tooextreme overfitting. Podobně regrese problémy hello rozdělení musí být tak, aby záznamy patřící k prostředkům s chybami před školení oříznutí slouží pro sadu školení a prostředky, s chybami po oříznutím hello se mají použít pro testování sady.

Jako obecné metody, je další důležité osvědčených postupů pro rozdělení dat pro trénování a testování toouse rozdělení podle ID prostředku tak, aby žádný z hello prostředky, které byly používány v školení se používají pro testování, protože toomake, zda je na nápad testování, kdy se používá nový prostředek  toomake předpovědi na, hello modelu poskytuje realistické výsledky.

### <a name="handling-imbalanced-data"></a>Zpracování imbalanced dat
V klasifikaci problémy Pokud existují další příklady jednu třídu než hello ostatní hello dat je uvedená toobe imbalanced. V ideálním případě by rádi bychom znali toohave dostatek zástupců každá třída v hello školení data toobe možné toodifferentiate mezi různými třídami. Pokud jedna třída je menší než 10 % hello dat, budeme moct že hello dat je imbalanced a říkáme hello underrepresented dataset menšinových – třída. Výrazně v řadě případů se nám najít imbalanced datové sady kde jednu třídu je vážně underrepresented porovnání tooothers například pouze tvořící 0,001 % hello datových bodů. Třída nevyváženosti je problém v mnoha doménách včetně odhalování podvodů, vniknutí sítě a prediktivní údržby, kde selhání jsou obvykle výjimečných výskytů v hello životnost hello prostředky, které tvoří hello menšinových – příklady tříd.

V případě třída nevyváženosti výkon většinu standardních algoritmů učení dojde k ohrožení bezpečnosti jako jejich cílem je toominimize hello celkové míra chyb. Například pro datovou sadu s příklady 99 % záporné tříd a příklady kladné tříd 1 %, jsme se připojíte 99 % přesnost jednoduše označování všechny instance jako záporné. To však misclassifies všechny kladné příklady v tak hello algoritmus není užitečné sice velmi vysokou přesnost metrika hello. V důsledku toho nejsou dostatečná v případě imbalanced learning konvenční vyhodnocení metriky, například celkový přesnost na míra chyb. Další metriky, například přesnost, odvolání, F1 skóre a náklady na upravenou křivek ROC se používají pro hodnocení v případě imbalanced datové sady, která je popsána v hello části vyhodnocení metriky.

Existují však některé metody, které pomáhají nápravě třída nevyváženosti problém. Hello dva hlavní ty jsou vzorkování techniky a náklady citlivé učení.

#### <a name="sampling-methods"></a>Metody vzorkování
použití Hello vzorkování metody v imbalanced učení se skládá z úpravy datovou sadu hello některé mechanismy v pořadí tooprovide datovou sadu s vyrovnáváním. I když existují spoustu různých odběry vzorků, většina z nich splněny následující jsou náhodných oversampling a v části vzorkování.

Jednoduše stanovené, náhodné oversampling je výběr z náhodného vzorku z menšinových třídy, replikuje tyto příklady a jejich přidáním tootraining datové sady. To zvyšuje počet hello celkový příklady v třídě menšinových a nakonec vyvážit hello počet příklady různých tříd. Jeden nebezpečí oversampling je, že více instancí některé příklady může způsobit hello třídění toobecome příliš konkrétní úvodní toooverfitting.
To by způsobilo školení vysokou přesnost, ale může být velmi nízký výkon na neviditelný testování data. Naopak náhodných pod vzorkování je vyberete z náhodného vzorku většina třídy a odebrání těchto příkladů z trénovací datové sady. Odebrání příklady z třídy většina může způsobit hello třídění toomiss důležité koncepty toohello většinu třída, která se týkají. Hybridní vzorkování, kde je třída menšinových převzorkované a většina třída je v části vzorků na hello současně je další přijatelná přístup. Existuje mnoho dalších sofistikovanější techniky vzorkování jsou k dispozici a efektivní vzorkování metody pro třídu nevyváženosti je oblast oblíbených research příjem konstantní pozornost a příspěvky z mnoha kanály. Použití různých technik při rozhodování o hello co nejúčinnější ty, které je obvykle levé toohello data vědecký pracovník tooresearch a experimentovat a jsou vysoce závisí na vlastnosti dat hello. Kromě toho je důležité se, že jsou metody vzorkování použité pouze školení toohello toomake nastavit, ale není hello testovací sada.

#### <a name="cost-sensitive-learning"></a>Náklady citlivé učení
V prediktivní údržby, chyb, které tvoří hello menšinových třídy jsou důležitější sledovat než normální příklady a proto hello zaměřuje se na výkon hello algoritmus na selhání je obvykle hello fokus. To se obvykle označuje jako nerovné ztráta nebo asymetrický náklady misclassifying elementy různých tříd, ve kterém může nesprávně predikci kladnou jako záporné náklady více než naopak. Hello potřeby třídění musí být schopný toogive předpovědi vysokou přesnost přes hello menšinových – třída bez vážně kompromisů v hello přesnost pro třídu většina hello.

Existuje několik způsobů, které jde tohoto dosáhnout. Přiřazením vysoké náklady chybnou hello menšinových třídy a snažíme toominimize, které na celkové náklady, hello problém různé ztratí můžete efektivně řešit. Některé algoritmy strojového učení použít tento nápad ze své podstaty například SVMs (Support Vector počítače), kde lze začlenit náklady kladné a záporné příklady během doby školení. Podobně zvýšení skóre metody se používají a obvykle zobrazit dobrý výkon v případě imbalanced data, jako například boosted rozhodovací strom algoritmy.

## <a name="evaluation-metrics"></a>Vyhodnocení metriky
Jak už bylo zmíněno dříve, třída nevyváženosti způsobí, že nízký výkon jako algoritmy mívají tooclassify většinu – příklady tříd lepší v výdajů minoritního třída případů chyba celkový chybnou hello je mnohem vyšší, pokud třída většina správně označené. To způsobí, že sazby nízkou odvolání a větší problém se změní, pokud je velmi vysoké náklady hello firmy toohello falešných poplachů. Přesnost je nejoblíbenější metrika hello používá k popisu třídění výkonu. Ale jak jsme vysvětlili výše přesnost je neefektivní a nezahrnují hello skutečné výkon třídění funkce, jako je velmi citlivé toodata distribuce. Místo toho další metriky vyhodnocení jsou použité tooassess imbalanced learning problémy. V takových případech přesnost, odvolání a F1 skóre musí být hello počátečních metrik toolook na při hodnocení výkonu modelu prediktivní údržby. V prediktivní údržby označují kolik hello selhání v testovací sadě hello byly identifikovány správně modelem hello odvolání sazby. Vyšší rychlosti pro vyvolání to znamenat, že je hello model při zachytávání hello true selhání úspěšný. Metrika přesnost má vztah k hello počet falešných poplachů kde nižší sazby přesnost odpovídají vyšší falešných poplachů. F1 skóre zvažuje přesnost i pro vyvolání sazby s nejlepší hodnota je 1 a nejhorších se 0.

Kromě toho pro binární klasifikaci, decile tabulky a navýšení grafy jsou velmi informativní při hodnocení výkonu. Budou se zaměříte jenom na kladné – třída (počet selhání) a zadejte složitější přehled o výkonu algoritmus než co je vidět na základě právě pevného operační bodu na hello: křivka ROC (příjemce operační vlastnosti).
Decile tabulky se získají řazení hello testovací příklady podle jejich předpokládaných pravděpodobnosti selhání počítaný modelem hello před prahování toodecide na poslední popisek hello. Hello seřazené, že ukázky jsou pak seskupené v deciles (tj. hello 10 % ukázky s největší pravděpodobností a pak 20 %, 30 % a tak dále). Podle computing hello poměr mezi true kladné počet jednotlivých decile a jeho náhodné směrného plánu (tj 0.1, 0.2..) jeden odhadnout, jak hello algoritmus výkonu změny v každé decile. Navýšení grafy jsou použité tooplot decile hodnoty true kladné míra decile versus páry náhodných true kladné rychlost pro všechny deciles vykreslení. První deciles hello obvykle představují hello fokus hello výsledky, protože tady vidíte největší zvýšení. První deciles se také dají považovat za reprezentativní pro "na riziko" Pokud se používá pro prediktivní údržby.

## <a name="sample-solution-architecture"></a>Ukázkové architektury řešení
Při nasazení řešení prediktivní údržby, jsme zajímá v řešení end tooend poskytující průběžné cyklus přijímání dat, úložiště dat pro trénování modelu, funkce generování, předpovědi a vizualizace výsledků hello spolu s Výstraha generování mechanismus například prostředek řídicí panel monitorování. Chceme datový kanál, který poskytuje budoucí Statistika toohello uživatele průběžné automatizované způsobem. Příklad prediktivní údržby architekturu pro takové IoT datový kanál je znázorněno na obrázku 8 níže. V architektuře se shromažďují v reálném čase telemetrie do centra událostí, která ukládá data streamování. Tato data se požita stream analytics v reálném čase zpracování dat, jako je například funkce generování. Hello funkce se pak používají toocall hello prediktivního modelu webové služby a výsledky se zobrazují na řídicím panelu hello. Na hello stejný čas, ingestovaný data jsou také uloženy v databázi historických a sloučit s externími zdroji dat, jako je místní data základny toocreate příklady školení pro modelování.
Stejné datové sklady lze použít pro dávkové vyhodnocování hello příklady a ukládání hello výsledků, které lze znovu použít tooprovide prediktivní sestavy na řídicím panelu hello.

![Obrázek 8. Příklad Architektura řešení prediktivní údržby](media/cortana-analytics-playbook-predictive-maintenance/example-solution-architecture-for-predictive-maintenance.png)

Obrázek 8. Příklad Architektura řešení prediktivní údržby

Další informace o jednotlivých součástí hello architektury hello naleznete příliš[Azure](https://azure.microsoft.com/) dokumentaci.

