---
title: "přehledu srovnávacího testu aaaAzure databáze SQL"
description: "Toto téma popisuje hello Azure SQL Database srovnávacího testu měření výkonu hello databáze SQL Azure."
services: sql-database
documentationcenter: na
author: jan-eng
manager: jhubbard
editor: monicar
ms.assetid: e26f8a66-2c12-49d7-8297-45b4d48a5c01
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/21/2016
ms.author: janeng
ms.openlocfilehash: 1024e9ada511935f911cb1345b4dc5508997c702
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-benchmark-overview"></a>Přehled služby Azure SQL Database srovnávacího testu
## <a name="overview"></a>Přehled
Microsoft Azure SQL Database nabízí tři [úrovních služeb](sql-database-service-tiers.md) s více úrovněmi výkonu. Každou úroveň výkonu poskytuje zvýšení propustnosti stále větší toodeliver sadu prostředků, nebo "power", určené.

Je důležité toobe možné tooquantify jak roste power hello každé úrovně výkonu překládá do databáze vyšší výkon. toodo, které tuto společnost Microsoft vyvinula hello Azure SQL Database srovnávacího testu (ASDB). srovnávací test Hello vykonává směs základní operace v všechny úlohy OLTP nalezen. Jsme měření propustnosti hello dá dosáhnout databáze spuštěné v každé úrovni výkonu.

Hello prostředků a výkonu každou úroveň a výkonu služby jsou vyjádřeny z hlediska [jednotky transakcí databáze (Dtu)](sql-database-what-is-a-dtu.md). Počet jednotek Dtu poskytují způsob toodescribe hello relativní kapacitu úrovně výkonu založené na kombinaci měření procesoru, paměti a čtení a zápis nabízené jednotlivými úrovněmi výkonu výše. Zvýší hello hodnocení DTU databáze znamená zároveň toodoubling hello databáze napájení. srovnávací test Hello umožňuje nám tooassess hello dopad na výkon databáze hello zvýšení spotřeby nabízí jednotlivými úrovněmi výkonu výkonem skutečné databázové operace, při škálování velikost databáze, počet uživatelů a transakcí míry v poměru toohello prostředky k dispozici toohello databáze.

Podle vyjádření hello propustnost vrstvu služby na úrovni Basic hello pomocí transakce za hodinu, se týkají vrstvy služby na úrovni Standard hello pomocí transakce za minutu a úroveň služeb Premium hello použití transakcí za sekundu, umožňuje snazší tooquickly hello potenciální výkon jednotlivých požadavků na toohello služby vrstvy aplikace.

## <a name="correlating-benchmark-results-tooreal-world-database-performance"></a>Korelace výkonu databáze world tooreal výsledků srovnávacího testu
Je důležité toounderstand této ASDB, jako jsou všechny srovnávacích testů se reprezentativní a naznačuje výslednou jenom. Hello transakce sazby dosáhnout pomocí aplikace hello srovnávacího testu nebude hello stejné jako ty, které může dosáhnout s jinými aplikacemi. Hello srovnávacího testu se skládá z kolekce jinou transakci, které typy spouštění schéma obsahující rozsah tabulky a datové typy. Během cvičení srovnávacího testu hello hello stejné základní operace, které jsou běžné úlohy tooall OLTP, nepředstavuje žádné konkrétní třídě databáze nebo aplikace. cílem Hello srovnávacího testu hello je tooprovide relativní výkon přiměřené Průvodce toohello databáze, která může být očekávána při škálování nahoru nebo dolů mezi úrovněmi výkonu. Ve skutečnosti databáze jsou různé velikosti a složitost, dojde k jiné mix úloh a bude odpovídat různými způsoby. Například aplikace náročné na vstupně-výstupní operace narazit dříve vstupně-výstupní operace prahové hodnoty nebo narazit aplikace náročná na prostředky procesoru CPU omezení dříve. Není zaručeno, že všechny konkrétní databáze bude škálovat v hello stejný způsobem jako hello srovnávací test v rámci zvýšení zatížení.

Hello srovnávacího testu a její metody jsou podrobněji popsané v níže.

## <a name="benchmark-summary"></a>Souhrn srovnávacího testu
ASDB měří výkon hello směs základní databázových operací, které se vyskytují nejčastěji v online transakcí (OLTP) úlohy zpracování. Přestože srovnávacího testu hello je navržený s cloud computing pamatovat, hello schématu databáze, pro naplnění dat a transakce byla navrženou toobe široce reprezentativní základní elementů hello nejčastěji používaná v úlohách OLTP.

## <a name="schema"></a>Schéma
schéma Hello je navrženou toohave dostatek různých a složitost toosupport širokou škálu operace. Hello srovnávacího testu spouští skládá z šesti tabulky databáze. tabulky Hello spadají do tří kategorií: pevné velikosti, škálování a rozšiřujících se. Existují dvě tabulky pevné velikosti; tři škálování tabulky; a rostoucí jedna tabulka. Pevné velikosti tabulky obsahovat konstantní počet řádků. Škálování tabulky obsahovat mohutnost, která je přímo úměrná toodatabase výkon, ale nemění během hello srovnávacího testu. je velikost Hello narůstají tabulku jako tabulku škálování na počáteční zatížení, ale poté změní hello mohutnost v hello během spuštění hello srovnávacího testu, jako jsou vloženy a odstranit řádky.

Hello schéma obsahuje směs datových typů, včetně typu integer, číselný znak a datum a čas. schéma Hello zahrnuje primární a sekundární klíče, ale nejsou žádné cizí klíče – to znamená, že jsou žádné omezení referenční integrity mezi tabulkami.

Program generování dat generuje hello dat pro databázi počáteční hello. Celé číslo a číselná data se generují s různými strategie. V některých případech se náhodně distribuují hodnoty v rozsahu. V ostatních případech je sada hodnot náhodně permutovanou funkci tooensure, že se zachová konkrétní distribuční. Textová pole jsou generovány z vyvážené seznam slova tooproduce realistické vypadající data.

Hello databáze je velikost podle "měřítko." Hello měřítko (zkratka jako SF) určuje hello Kardinalita hello škálování a rozšiřujících se tabulky. Jak je popsáno níže v hello části uživatelů a Pacing, velikost databáze hello, počet uživatelů a všechny škálování v poměru tooeach další maximální výkon.

## <a name="transactions"></a>Transakce
zatížení Hello se skládá z typy devět transakcí, jak ukazuje následující tabulka hello. Každou transakci je navrženou toohighlight konkrétní sadu systému charakteristiky v hello databáze modul a systému hardwaru, s vysokým kontrastem z hello dalších transakcí. Tento přístup umožňuje snazší dopad hello tooassess toooverall výkonu různých součástí. Například "Pro čtení velkou" hello transakce vytváří velký počet operací čtení z disku.

| Typ transakce | Popis |
| --- | --- |
| Přečtěte si Lite |VYBRAT; v paměti; jen pro čtení |
| Střední pro čtení |VYBRAT; většinou v paměti; jen pro čtení |
| Těžký pro čtení |VYBRAT; většinou není v paměti; jen pro čtení |
| Aktualizace Lite |AKTUALIZACE; v paměti; čtení a zápis |
| Těžký aktualizace |AKTUALIZACE; většinou není v paměti; čtení a zápis |
| Vložení Lite |VLOŽIT; v paměti; čtení a zápis |
| Vložit těžký |VLOŽIT; většinou není v paměti; čtení a zápis |
| Odstranění |ODSTRANIT; směs v paměti a není v paměti; čtení a zápis |
| Těžký procesoru |VYBRAT; v paměti; relativně velké zatížení procesoru; jen pro čtení |

## <a name="workload-mix"></a>Kombinace úloh
Transakce jsou náhodně vybrané ze vyvážené distribuce s hello následující celkové kombinaci. Hello celkové kombinace má pro čtení a zápis poměr přibližně 2:1.

| Typ transakce | % Kombinaci |
| --- | --- |
| Přečtěte si Lite |35 |
| Střední pro čtení |20 |
| Těžký pro čtení |5 |
| Aktualizace Lite |20 |
| Těžký aktualizace |3 |
| Vložení Lite |3 |
| Vložit těžký |2 |
| Odstranění |2 |
| Těžký procesoru |10 |

## <a name="users-and-pacing"></a>Uživatelé a interval
Hello srovnávacího testu zatížení vycházejí z nástroj, který odešle transakce mezi sadu připojení toosimulate hello chování počet souběžných uživatelů. I když jsou všechny hello připojení a transakce generované počítače, pro jednoduchost označujeme toothese připojení jako "uživatelé." I když každý uživatel pracuje nezávisle na jiných uživatelů, všichni uživatelé provést hello stejné cyklus následující kroky:

1. Navázání připojení k databázi.
2. Opakujte až signalizovaného tooexit:
   * Vyberte transakce (z náhodně vyvážené distribuce).
   * Proveďte hello vybrané transakce a doby odezvy hello měr.
   * Počkejte intervalu zpoždění.
3. Připojení k databázi zavřít hello.
4. Ukončení.

je náhodně vybrané Hello interval zpoždění (v kroku 2c), ale s distribučním, který má v průměru 1.0 sekundu. Každý uživatel může, proto v průměru generovat maximálně jednu transakci za sekundu.

## <a name="scaling-rules"></a>Škálování pravidla
Hello počet uživatelů, je dáno hello velikost databáze (v jednotkách měřítko). Je jeden uživatel pro každých pět jednotky měřítko. Z důvodu hello interval zpoždění může jeden uživatel generovat maximálně jednu transakci za sekundu v průměru.

Například-měřítko 500 (SF = 500) databáze bude mít 100 uživatelů a můžete dosáhnout maximálně 100 TPS. toodrive vyšší míra TPS vyžaduje více uživatelů a větší databázi.

Následující tabulka Hello ukazuje hello počet uživatelů ve skutečnosti trvalejší pro každou úroveň a výkonu služby.

| Úroveň služby (úroveň výkonu) | Uživatelé | Velikost databáze |
| --- | --- | --- |
| Basic |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6/P3) |800 |114 GB |

## <a name="measurement-duration"></a>Doba trvání měření
Platný spuštění testu výkonnosti vyžaduje stabilního stavu měření doby trvání alespoň jednu hodinu.

## <a name="metrics"></a>Metriky
Hello klíčové metriky v srovnávacího testu hello jsou propustnost a dobu odezvy.

* Propustnost je hello základní výkon měr v hello srovnávacího testu. Propustnost je uvedená v transakce za jednotku předčasné, počítání všechny typy transakcí.
* Doba odezvy se rozumí míra výkonu předvídatelnost. omezení času odezvy Hello se liší podle třídy služeb s vyšší třídy služeb s přísnější požadavky na dobu odezvy, jak je uvedeno níže.

| Třída služby | Propustnost měr | Požadavky na dobu odezvy |
| --- | --- | --- |
| Premium |Transakce za sekundu |95. percentil v 0,5 sekund |
| Standard |Transakce za minutu |90. percentil v sekundách 1.0 |
| Basic |Transakce za hodinu |80. percentil v sekundách 2.0 |

## <a name="conclusion"></a>Závěr
Hello srovnávacího testu databáze SQL Azure měří hello relativní výkon spuštění pro řadu hello úrovně dostupných služeb a úrovně výkonu databáze SQL Azure. srovnávací test Hello vykonává směs základní databázových operací, které se vyskytují nejčastěji v online transakcí (OLTP) úlohy zpracování. Podle měření skutečným výkonem, poskytuje srovnávacího testu hello smysluplnější vyhodnocení účinku hello na propustnost změna hello úroveň výkonu než je možné pomocí právě výpis hello prostředky poskytované jednotlivé úrovně, jako je například rychlosti procesoru, velikosti paměti a IOPS . V budoucích hello jsme tooevolve hello srovnávacího testu toobroaden pokračovat v jeho oboru a rozbalte hello data poskytnutá.

## <a name="resources"></a>Zdroje
[Úvod tooSQL databáze](sql-database-technical-overview.md)

[Úrovně služeb a úrovně výkonu](sql-database-service-tiers.md)

[Pokyny výkonu pro izolované databáze](sql-database-performance-guidance.md)
