---
title: "aaaHow tooBuild komplexní plány a pokročilé opakování ve službě Azure Scheduler"
description: "Jak tooBuild komplexní plány a pokročilé opakování ve službě Azure Scheduler"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 02172791978b12be0ccb3078125d057b2efe8523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobuild-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Jak tooBuild komplexní plány a pokročilé opakování ve službě Azure Scheduler
## <a name="overview"></a>Přehled
V hello srdcem Azure Scheduler je úloha hello *plán*. plán Hello Určuje, kdy a jak hello Scheduler provede úlohu hello.

Azure Scheduler vám umožní toospecify různé jednorázové a opakovaného plány pro úlohy. *Jednorázové* plány fire jednou v zadanou dobu – efektivně, jsou *opakovaného* plány, které jsou spouštěny pouze jednou. Opakující se plány fire na předem určený frekvenci.

Azure Scheduler umožňuje s tuto flexibilitu potřebují podporuje celou řadu obchodních scénářů:

* Vyčištění periodických dat. – například každý den, odstranit všechny tweetů starší než 3 měsíce
* Archivace – například každý měsíc, nabízenou faktury historie toobackup služby
* Požadavky na externí data – například každých 15 minut, načítat nové sestavy počasí identifikátor klíče subjektu z NOAA
* Bitovou kopii zpracování – třeba každý den v týdnu, hodiny mimo špičku, použít cloudové výpočetní Image toocompress nahrán daný den

V tomto článku jsme provede Příklad úloh, které můžete vytvořit s plánovačem Azure. Poskytujeme hello data JSON, který popisuje každý plán. Pokud používáte hello [REST API Scheduleru](https://msdn.microsoft.com/library/mt629143.aspx), můžete použít tento stejný formát JSON pro [vytváření úlohu služby Azure Scheduler](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Podporované scénáře
Hello mnoho příklady v tomto tématu ilustrují hello spektra scénáře, které podporuje Azure Scheduler. Tyto příklady širokém smyslu ilustruje, jak toocreate plány pro mnoha způsoby chování, včetně hello těch, které jsou níže:

* Spustit jednou v konkrétní datum a čas
* Spuštění a opakovat explicitní několikrát
* Spustit okamžitě a opakovat
* Spuštění a opakovat každých  *n*  minuty, hodiny, dny, týdny nebo měsíce, spouštění v určitém čase
* Spuštění a opakovat týdenní nebo měsíční četností, ale jenom na konkrétní dny, konkrétní dny v týdnu nebo konkrétní dny v měsíci
* Spuštění a opakovat na více než jednou. v době – například poslední pátek a pondělí v každém měsíci, nebo na 5:15:00 a 17:15:00 každý den

## <a name="dates-and-datetimes"></a>Data a času
Data v Azure Plánovač úloh, postupujte podle hello [specifikace formátu ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) a obsahovat pouze hello datum.

Odkazy na datum a čas ve službě Azure Scheduler úlohy podle hello [specifikace formátu ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) a obsahovat části data a času. Datum a čas, která neurčuje časový posun se předpokládá toobe UTC.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Postupy: Použití formátu JSON a rozhraní REST API pro vytváření plánů
hello toocreate jednoduchého plánu pomocí [REST API služby Azure Scheduler](https://msdn.microsoft.com/library/mt629143), první [zaregistrovat předplatné zprostředkovatele prostředků](https://msdn.microsoft.com/library/azure/dn790548.aspx) (hello název zprostředkovatele pro Scheduler je  *Microsoft.Scheduler*), pak [vytvořit kolekci úloh](https://msdn.microsoft.com/library/mt629159.aspx)a v neposlední řadě [vytvořit úlohu](https://msdn.microsoft.com/library/mt629145.aspx). Když vytvoříte úlohu, můžete určit plánování a opakování pomocí JSON jako hello jeden převzaty webových stránek níže:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often toofire (default too1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default toorecur infinitely)
            "endTime": "2012-11-04",      // optional (default toorecur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Přehled: Úloha schématu základy
Hello následující tabulka poskytuje podrobný přehled hello důležité elementy související toorecurrence a plánování v rámci úlohy:

| **Název JSON** | **Popis** |
|:--- |:--- |
| ***čas spuštění*** |*startTime* je datum a čas. Pro jednoduché plány *startTime* je první výskyt hello a pro komplexní plány hello úloha spustí dřív než *startTime*. |
| ***opakování*** |Hello *opakování* objektu určuje opakování pravidla pro hello úlohy a úlohy hello opakování hello bude vykonán. objekt opakování Hello podporuje hello prvky *frekvenci, interval, endTime, count,* a *plán*. Pokud *opakování* je definován *frekvence* je vyžadován; hello další prvky *opakování* jsou volitelné. |
| ***frekvence*** |Hello *frekvence* řetězec představující hello frekvence jednotky, na které hello úlohy opakovat. Podporované hodnoty jsou *"minutu", "hodina", "dne", "týden"* nebo *"měsíc".* |
| ***interval*** |Hello *interval* je kladné celé číslo a označuje hello interval hello *frekvence* který určuje, jak často hello bude úloha spuštěna. Například pokud *interval* 3 a *frekvence* je "týden" hello úloha opakuje každé 3 týdny. Azure Scheduler podporuje maximálně *interval* 18 měsíců měsíční frekvence 78 týdny frekvence týdenní nebo 548 dnů pro denní četnost. Pro hodinu a minutu četnost hello podporované rozsah je 1 < = *interval* < = 1 000. |
| ***čas ukončení*** |Hello *endTime* řetězec Určuje hello data a času, po které hello nesmí spuštění úlohy. Není platný toohave *endTime* v posledních hello. Pokud žádné *endTime* nebo je zadaný počet, hello úloha spustí nekonečnou. Obě *endTime* a *počet* nelze vložit pro hello stejnou úlohu. |
| ***počet*** |<p>Hello *počet* je kladné celé (větší než nula) určuje hello počet, kolikrát má tato úloha spuštěna před dokončením.</p><p>Hello *počet* představuje hello stanovený počet spuštění úlohy hello před určuje byla dokončena. Například pro úlohu, která se spouští každý den *počet* 5 a počáteční datum pondělí, dokončení hello úlohy po spuštění v pátek. Pokud spuštění hello datum je v minulosti hello, první spuštění hello se počítá z čas vytvoření hello.</p><p>Pokud žádné *endTime* nebo *počet* není zadaný, hello úloha spustí nekonečnou. Obě *endTime* a *počet* nelze vložit pro hello stejnou úlohu.</p> |
| ***plán*** |Úlohy v zadaném intervalu mění jeho opakování podle plánu opakování. A *plán* obsahuje změny na základě minuty, hodiny, dny v týdnu, dny v měsíci a číslo týdne. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Přehled: Úloha schématu výchozí hodnoty, omezení a příklady
Po tomto přehledu probereme každý z těchto prvků podrobně.

| **Název JSON** | **Typ hodnoty** | **Vyžaduje?** | **Výchozí hodnota** | **Platné hodnoty** | **Příklad** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***čas spuštění*** |Řetězec |Ne |Žádný |Data a časy podle normy ISO 8601 |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***opakování*** |Objekt |Ne |Žádný |Objekt opakování |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***frekvence*** |Řetězec |Ano |Žádný |"minutu", "hodina", "dne", "týden", "měsíc" |<code>"frequency" : "hour"</code> |
| ***interval*** |Číslo |Ne |1 |1 too1000. |<code>"interval":10</code> |
| ***čas ukončení*** |Řetězec |Ne |Žádný |Hodnota data a času představující čas v budoucnu hello |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***počet*** |Číslo |Ne |Žádný |>= 1 |<code>"count": 5</code> |
| ***plán*** |Objekt |Ne |Žádný |Objekt plánu |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Podrobné informace: *startTime*
Hello následující tabulka zachycení jak *startTime* Určuje, jak spustit úlohu.

| **hodnoty startTime** | **Bez opakování** | **Opakování. Žádný plán** | **Opakování s plánem** |
|:--- |:--- |:--- |:--- |
| **Žádný čas spuštění** |Spustit jednou okamžitě |Spusťte jednou okamžitě. Spouštění dalších spuštěních podle výpočet z čas posledního spuštění |<p>Spustit jednou okamžitě</p><p>Zahájí další spuštění na základě plánu opakování.</p> |
| **Počáteční čas v minulosti.** |Spustit jednou okamžitě |<p>Výpočet budoucí prvním spuštění po spuštění a v daném čase spustit</p><p>Spouštění dalších spuštěních na základě oncalculating z čas posledního spuštění</p><p>Najdete v příkladu za touto tabulkou další vysvětlení</p> |<p>Úloha spustí *ne sooner než* hello zadaný počáteční čas. první výskyt Hello se podle plánu hello vypočítaných z čas zahájení hello</p><p>Zahájí další spuštění na základě plánu opakování.</p> |
| **Počáteční čas v budoucnosti nebo v současné době** |Spustit jednou v zadaný čas spuštění |<p>Spustit jednou v zadaný čas spuštění</p><p>Spouštění dalších spuštěních podle výpočet z čas posledního spuštění</p> |<p>Úloha spustí *ne sooner než* hello zadaný počáteční čas. první výskyt Hello se podle plánu hello vypočítaných z čas zahájení hello</p><p>Zahájí další spuštění na základě plánu opakování.</p> |

Umožňuje zobrazit příklad co se stane, kde *startTime* je v minulosti, hello s *opakování* , ale žádné *plán*.  Předpokládejme, že hello aktuální čas je 2015-04-08 13:00, *startTime* je 2015-04-07 14:00, a *opakování* je 2 dní (definované s *frekvence*: den a *interval*: 2.) Všimněte si, že hello *startTime* provede před hello aktuální čas a je v minulosti hello

Za těchto podmínek hello *první spuštění* bude 2015-04-09 v 14:00\. modul Scheduler Hello vypočítá výskytů provádění z hello počáteční čas.  Všechny instance v posledních hello se zahodí. modul Hello používá hello další instance, ke kterému dochází v budoucnu hello.  Proto v tomto případě *startTime* je 2015-04-07 na 2:00 pm, hello další instance je dvou dnů od té doby je 2015-04-09 na 2:00 pm.

Upozorňujeme, že první spuštění hello by být hello stejné i když hello startTime 2015-04-05 14:00 nebo 14:00\ 2015-04-01. Po prvním spuštění hello, dalších spuštěních se počítají pomocí hello naplánované – tak nebudou v 2015-04-11 na 2:00 pm, pak 2015-04-13 na 2:00 pm, pak 2015-04-15 na 2:00 pm, atd.

Nakonec, když má plánu, nastaveného hodin a minut nejsou v plánu hello, že výchozí toohello hodin a minut hello první spuštění, v uvedeném pořadí.

## <a name="deep-dive-schedule"></a>Podrobné informace: *plán*
Na jedné straně *plán* můžete *limit* hello počet spuštění úlohy.  Například pokud úlohy s četností "měsíc" má *plán* spouští se v den pouze 31, hello úloha běží pouze měsících, které mají 31<sup>st</sup> den.

Na druhé straně hello *plán* může taky *rozbalte* hello počet spuštění úlohy. Například pokud úlohy s četností "měsíc" má *plán* , spustí na 1 a 2 dny v měsíci, hello úlohy běží na hello 1<sup>st</sup> a 2<sup>nd</sup> dny v měsíci hello místo pouze jednou měsíc.

Pokud jsou zadány více elementů plán, hello pořadí vyhodnocení je z hello největší toosmallest – číslo týden, měsíc, den, den v týdnu, hodin a minut.

Hello následující tabulka popisuje *plán* elementy podrobně.

| **Název JSON** | **Popis** | **Platné hodnoty** |
|:--- |:--- |:--- |
| **minut** |Počet minut hello hodinu, ve které hello bude spuštěna úloha |<ul><li>Celé číslo, nebo</li><li>Pole celých čísel</li></ul> |
| **hodiny** |Hodiny dne hello, na které hello bude spuštěna úloha |<ul><li>Celé číslo, nebo</li><li>Pole celých čísel</li></ul> |
| **dny v týdnu** |Počet dnů hello týden hello úlohy se spustí. Tuto položku je možné zadat jenom při týdenní frekvenci. |<ul><li>"Pondělí", "Úterý", "Středa", "Čtvrtek", "Pátek", "Sobota" a "Neděle"</li><li>Pole všech hello nad hodnotu (maximální pole velikost 7)</li></ul>*Není* malá a velká písmena |
| **monthlyOccurrences** |Určuje, které dny hello měsíc hello úlohy se spustí. Tuto položku je možné zadat jenom při měsíční frekvenci. |<ul><li>Pole objektů monthlyOccurrence:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *den* je den hello hello týden hello úlohy se spustí, například {neděle} je každou neděli měsíci hello. Povinná hodnota.</p><p>Výskyt je *výskyt* hello den v měsíci hello, například {neděle, -1} je hello poslední neděle měsíci hello. Volitelné.</p> |
| **Prescribed** |Den hello měsíc hello úlohy se spustí. Tuto položku je možné zadat jenom při měsíční frekvenci. |<ul><li>Libovolná hodnota < = -1 a > =-31.</li><li>Libovolná hodnota > = 1 a < = 31.</li><li>Pole výše uvedených hodnot</li></ul> |

## <a name="examples-recurrence-schedules"></a>Příklady: Opakování plány
Hello Následují příklady různých plány opakování – zaměřením na objekt hello plánu a jeho dílčí prvky.

Hello plány níže všechny předpokládá, že hello *interval* nastavena too1\. Navíc jeden musí předpokládat, hello správné frekvence v souladu toowhat je v hello *plán* – například nelze použít frekvence "dne" a mají "Prescribed" Změna v plánu hello. Takové omezení jsou popsané výše.

| **Příklad** | **Popis** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |Spustit na 5: 00 každý den. Azure Scheduler odpovídá až každé hodnoty v "hours" jednotlivé hodnoty jsou "minutes", po jednom toocreate seznam všechny časy hello úlohy, na které hello jsou toobe spustit. |
| <code>{"minutes":[15], "hours":[5]}</code> |Spustit na 5:15:00 každý den |
| <code>{"minutes":[15], "hours":[5,17]}</code> |Spustit na 5:15:00 a 17:15:00 každý den |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 každý den |
| <code>{"minutes":[0,15,30,45]}</code> |Spustit každých 15 minut |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Spouštět každou hodinu. Tato úloha spouští každou hodinu. řídí Hello minutu hello *startTime*, pokud je zadaná nebo pokud není zadaný žádný, podle času vytvoření hello. Například pokud hello časem spuštění a čas vytvoření (podle toho, co platí) je 12:25 hodin, hello úlohy se budou spouštět v 00:25, 01:25 02:25,..., 23:25. Hello plán je ekvivalentní toohaving úlohu s *frekvence* "hodina" *interval* 1, a ne *plán*. Hello rozdílem je, že tento plán může používat jiné *frekvence* a *interval* toocreate jiné úlohy příliš. Například, pokud hello *frekvence* byly "měsíc", hello plán by spustit jenom jednou za měsíc místo každý den Pokud *frekvence* byly "den" |
| <code>{minutes:[0]}</code> |Spusťte každých hodinu na hello hodinu. Tato úloha také spouští každou hodinu, ale na hello hodinu (například 12: 00, 1: 00, 2: 00, atd.) Toto je ekvivalentní tooa úlohy s četností "hodina", čas spuštění s nulový počet minut a žádný plán, pokud frekvence hello byly "dne", ale pokud frekvence hello "týden" nebo "měsíc", hello plán by spustit pouze jeden den, týden nebo jednoho dne a měsíce, v uvedeném pořadí. |
| <code>{"minutes":[15]}</code> |Spustit na 15 minut po hodině každou hodinu. Spouští se každou hodinu od 00:15, tedy v 1:15, 2:15 atd., a končí ve 22:15 a 23:15. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |Spustit v 17: 00 v sobotu každý týden |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit v 17: 00 v pondělí, středu a pátek každý týden |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit v 17:15:00 a 17:45:00 v pondělí, středu a pátek každý týden |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit na 5: 00 a 17: 00 v pondělí, středu a pátek každý týden |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 v pondělí, středu a pátek každý týden |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Spustí se ve všední dny každých 15 minut. |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Spustit každých 15 minut mezi 9: 00 a 16:45:00 ve všední dny |
| <code>{"weekDays":["sunday"]}</code> |Spustit v neděli při spuštění |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |Na úterý a každý čtvrtek při spuštění |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |Spustit v 6: 00 na hello 28th den z každých měsíc (za předpokladu frekvenci měsíc) |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Spustili hello hello měsíc poslední den v 6: 00. Pokud chcete toorun úlohu na hello poslední den v měsíci, použijte místo den 28, 29, 30 a 31 -1. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |Spustit v 6: 00 hello první a poslední den z každých měsíc |
| <code>{monthDays":[1,-1]}</code> |Spustit na hello první a poslední den z každých měsíce na čas spuštění |
| <code>{monthDays":[1,14]}</code> |Spustit na hello první a Fourteenth den každých měsíce na čas spuštění |
| <code>{monthDays":[2]}</code> |Spusťte na hello druhý den hello měsíci v čas spuštění |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |Spustit na prvním každý pátek v měsíci v 5: 00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: Spusťte na první každý pátek v měsíci při spuštění |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Spustit na třetí pátek od konce měsíce, každý měsíc, při spuštění |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Spustit na první a poslední pátek v každém měsíci v 5:15:00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Spustit na první a poslední pátek v každém měsíci v čas spuštění |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Spusťte na páté každý pátek v měsíci v čase zahájení. Pokud není žádná páté pátek v měsíci, tento nemá nejde spustit, protože je naplánované toorun na pouze páté pátek. Zvažte, pokud má úloha hello toorun na hello posledního výskytu pátek v měsíci hello pomocí -1 namísto 5 pro výskyt hello. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Spustit každých 15 minut na poslední pátek v měsíci hello |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 na hello 3. středa z každých měsíc |

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Koncepty, terminologie a hierarchie entit Azure Scheduleru](scheduler-concepts-terms.md)

 [Začněte používat plánovače v hello portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Azure Scheduleru](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

