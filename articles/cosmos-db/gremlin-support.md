---
title: "Podpora systému Cosmos DB Gremlin aaaAzure | Microsoft Docs"
description: "Další informace o hello Gremlin jazyka z Apache TinkerPop, která funkcí a kroků a dostupné v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 6016ccba-0fb9-4218-892e-8f32a1bcc590
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/10/2017
ms.author: denlee
ms.openlocfilehash: f8c2ce50c6946e971f56fe1f3838b0899cb2ad8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Graf podporu Azure Cosmos DB Gremlin
Podporuje Azure Cosmos DB [Apache Tinkerpop](http://tinkerpop.apache.org) graf traversal jazyk [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), což je rozhraní Graph API pro vytváření entit grafu a provádění operace dotazů grafu. Můžete použít hello Gremlin jazyk toocreate grafu entit (vrcholy a okraje), upravte vlastnosti v rámci těchto entit, provádět dotazy a traversals a odstranit entity. 

Azure Cosmos DB přináší funkce připravené pro podniky toograph databáze. To zahrnuje globální distribuční nezávislé škálování úložiště a propustnosti, latenci předvídatelný jednociferné milisekundu, automatické indexování a SLA 99,99 %. Protože Azure Cosmos DB podporuje TinkerPop/Gremlin, můžete snadno migrovat aplikace napsané v jiné databáze grafu bez nutnosti změny toomake kódu. Kromě toho na základě Gremlin podpory Azure Cosmos DB zajišťuje bezproblémovou integraci s povoleným TinkerPop analytics architektury, jako je [Apache Spark GraphX](http://spark.apache.org/graphx/). 

V tomto článku jsme zadejte rychlé návod Gremlin a výčet hello Gremlin funkcí a kroků, které jsou podporovány ve verzi preview hello podpory rozhraní Graph API.

## <a name="gremlin-by-example"></a>Gremlin příklad
Použijeme toounderstand grafu ukázka, jak dotazy mohou být vyjádřeny v Gremlin. Hello následující obrázek znázorňuje obchodní aplikace, která spravuje data o uživatelích, zájmů a zařízení ve formě hello graf.  

![Zobrazuje osob, zařízení a zájmů ukázkové databáze](./media/gremlin-support/sample-graph.png) 

Tento graf má hello následující typy vrchol (nazývané "Popis" v Gremlin):

- Lidé: hello graf má tři osoby, každý s každým Thomas a Ben
- Zájmů: jejich zájmů, v tomto příkladu hello herní fotbalové
- Zařízení: hello zařízení, která používají osoby
- Operační systémy: hello operační systémy, hello zařízení se systémem

Jsme představují hello vztahy mezi tyto entity prostřednictvím hello edge typy/popisky následující:

- Zná: například "Thomas ví, každý s každým"
- Také zajímat: zájmů hello toorepresent hello uživatelů v našem grafu, například "Ben má zájem o fotbalu"
- RunsOS: Přenosný počítač spustí hello operačního systému Windows
- Používá: toorepresent zařízení, které si uživatel používá. Například každý s každým používá Motorola telefon se sériovým číslem 77

Umožňuje spouštění tohoto grafu. pomocí hello některé operace [Gremlin konzoly](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console). Můžete také provést tyto operace pomocí Gremlin ovladačů v platformě hello podle vašeho výběru (Java, Node.js, Python nebo .NET).  Předtím, než se podíváme na co je podporováno v Azure Cosmos DB, podíváme se na několik příkladů tooget seznámit se syntaxí hello.

První Podíváme se na CRUD. Hello následující příkaz Gremlin vloží hello "Thomas" vrchol do grafu hello:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

V dalším kroku hello následující příkaz Gremlin vloží "ví" okraje mezi Thomas a každý s každým.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Hello následující dotaz vrátí vrcholy hello "osoba" v sestupném pořadí podle jejich názvy první:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Kde grafy nám je, když potřebujete tooanswer otázky jako "Jaké operační systémy se přátelích Thomas použít?". Tento jednoduchý traversal tooget Gremlin můžete spustit z grafu hello tyto informace:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Nyní Podíváme se na databázi Cosmos Azure poskytuje vývojářům Gremlin.

## <a name="gremlin-features"></a>Funkce gremlin
TinkerPop je standard, který obsahuje širokou škálu technologie grafu. Proto má toodescribe standardní terminologie jaké funkce jsou poskytované poskytovatelem grafu. Azure Cosmos DB poskytuje souběžnosti trvalé, vysoká, databázi zapisovatelné grafu, která může být rozdělený napříč více servery nebo clustery. 

Hello následující tabulka uvádí hello TinkerPop funkce, které implementují Cosmos databázi Azure: 

| Kategorie | Azure Cosmos DB implementace |  Poznámky | 
| --- | --- | --- |
| Funkce grafu | Poskytuje trvalosti a ConcurrentAccess ve verzi preview. Navrženou toosupport transakce | Metody počítač se dá implementovat prostřednictvím konektoru Spark hello. |
| Proměnné funkce | Podporuje logická hodnota, celé číslo, bajtů, dvakrát, Float, celé číslo, Long, řetězec | Podporuje primitivní typy, není kompatibilní s komplexní typy prostřednictvím datového modelu |
| Vrchol funkce | Podporuje RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty –, RemoveProperty  | Podporuje vytváření, úpravu a odstranění vrcholy |
| Vrchol vlastnost funkce | StringIds, UserSuppliedIds, AddProperty –, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Podporuje vytváření, úpravy a odstraňování vrchol vlastnosti |
| Funkce edge | AddEges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty –, RemoveProperty | Podporuje vytváření, úpravu a odstranění okrajů |
| Funkce vlastnost Edge | Vlastnosti, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Podporuje vytváření, úpravy a odstraňování vlastnosti edge |

## <a name="gremlin-wire-format-graphson"></a>Gremlin přenosový formát: GraphSON

Azure Cosmos DB používá hello [GraphSON formát](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) při vrácení výsledků z Gremlin operace. GraphSON se standardní formát hello Gremlin představující vrcholy, okraje a vlastností (jeden a více hodnot vlastnosti) pomocí JSON. 

Například hello následující fragment kódu ukazuje znázornění GraphSON vrcholu *vrátil toohello klienta* z Azure Cosmos DB. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

Vlastnosti Hello používá GraphSON pro vrcholy jsou hello následující:

| Vlastnost | Popis |
| --- | --- |
| id | ID Hello vrchol hello. Musí být jedinečné (v kombinaci s hodnotou hello _partition, pokud je k dispozici) |
| Popisek | Popisek Hello vrcholu hello. Toto je volitelné a slouží toodescribe typ entity hello. |
| type | Použít toodistinguish vrcholy z jiných grafu dokumentů |
| properties | Kontejner uživatelem definované vlastnosti související s vrchol hello. Každá vlastnost může mít více hodnot. |
| _partition (Konfigurovat) | klíč oddílu Hello vrcholu hello. Můžou být použité tooscale out grafy toomultiple servery |
| outE | Tato položka obsahuje seznam se okraje z vrchol. Ukládání informací o sousedství hello s vrchol umožňuje rychlé spuštění traversals. Okraje jsou seskupené podle jejich popisky. |

A hraniční hello obsahuje následující informace toohelp s částmi tooother navigační hello grafu hello.

| Vlastnost | Popis |
| --- | --- |
| id | ID Hello hello okraj. Musí být jedinečné (v kombinaci s hodnotou hello _partition, pokud je k dispozici) |
| Popisek | Popisek Hello hello okraj. Tato vlastnost je volitelné a slouží toodescribe typ vztahu hello. |
| inventáře | Tato položka obsahuje seznam v vrcholy pro okraj. Ukládání informací o sousedství hello s hranou hello umožňuje rychlé spuštění traversals. Vrcholy jsou seskupené podle jejich popisky. |
| properties | Kontejner uživatelem definované vlastnosti související s hello hranou. Každá vlastnost může mít více hodnot. |

Každou vlastnost můžete ukládat víc hodnot v rámci pole. 

| Vlastnost | Popis |
| --- | --- |
| hodnota | Hello hodnotu vlastnosti hello

## <a name="gremlin-partitioning"></a>Vytváření oddílů gremlin

V Azure Cosmos DB, grafy ukládají v rámci kontejnerů, které je možné škálovat nezávisle z hlediska úložiště a propustnost (vyjádřeno v normalizovaný požadavků za sekundu). Každý kontejner musí definovat volitelný, ale doporučuje vlastnost klíče oddílu, která určuje hranici logický oddíl pro související data. Každý vrchol okraj musí mít `id` vlastnosti, které jsou jedinečné pro entity v rámci této hodnotu klíče oddílu. Hello podrobnosti jsou popsané v [vytváření oddílů v Azure Cosmos DB](partition-data.md).

Operace gremlin fungují bezproblémově napříč daty grafu, které jsou v rozsahu více oddílů v Azure Cosmos DB. Je však doporučeno toochoose klíč oddílu pro vaše grafy, které se často používá jako filtr ve funkci dotazy, obsahuje mnoho různých hodnot a podobné frekvenci přístup tyto hodnoty. 

## <a name="gremlin-steps"></a>Kroky gremlin
Nyní Podíváme se na hello Gremlin kroky nepodporuje Azure Cosmos DB. Úplný odkaz na Gremlin, najdete v části [TinkerPop odkaz](http://tinkerpop.apache.org/docs/current/reference).

| Krok | Popis | TinkerPop 3.2 dokumentace | Poznámky |
| --- | --- | --- | --- |
| `addE` | Přidá okraj mezi dvěma vrcholy | [Krok addE](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) | |
| `addV` | Přidá vrchol toohello graf | [Krok addV](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) | |
| `and` | Ensurest, že všechny traversals hello vrátit hodnotu | [a krok](http://tinkerpop.apache.org/docs/current/reference/#and-step) | |
| `as` | Krok jedno tooassign výstup proměnné toohello krok | [jako krok](http://tinkerpop.apache.org/docs/current/reference/#as-step) | |
| `by` | Jedno krok použít s `group` a`order` | [Tímto krokem](http://tinkerpop.apache.org/docs/current/reference/#by-step) | |
| `coalesce` | Vrátí první traversal hello, který vrací výsledek | [sloučení krok](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) | |
| `constant` | Vrátí konstantní hodnotu. Použít s`coalesce`| [konstantní krok](http://tinkerpop.apache.org/docs/current/reference/#constant-step) | |
| `count` | Vrátí počet hello z hello traversal | [počet krok](http://tinkerpop.apache.org/docs/current/reference/#count-step) | |
| `dedup` | Vrátí hello hodnot s odebranými duplikáty hello | [krok odstraňování duplicitních dat](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) | |
| `drop` | Vyřazuje hello hodnoty (vrchol nebo edge) | [Přetáhněte krok.](http://tinkerpop.apache.org/docs/current/reference/#drop-step) | |
| `fold` | Jednání jako bariéry, která vypočítá hello agregace výsledků| [fold – krok](http://tinkerpop.apache.org/docs/current/reference/#fold-step) | |
| `group` | Skupiny hello podle hello popisků zadané hodnoty| [Krok skupiny](http://tinkerpop.apache.org/docs/current/reference/#group-step) | |
| `has` | Použít vlastnosti toofilter, vrcholy a okrajů. Podporuje `hasLabel`, `hasId`, `hasNot`, a `has` variant. | [má krok](http://tinkerpop.apache.org/docs/current/reference/#has-step) | |
| `inject` | Vložení hodnoty do datového proudu| [Vložit krok](http://tinkerpop.apache.org/docs/current/reference/#inject-step) | |
| `is` | Použít tooperform filtr pomocí logický výraz | [je krok](http://tinkerpop.apache.org/docs/current/reference/#is-step) | |
| `limit` | Použít toolimit počet položek v hello traversal| [limit krok](http://tinkerpop.apache.org/docs/current/reference/#limit-step) | |
| `local` | Místní zabalí oddíl poddotazu procházení, podobně jako tooa | [místní krok](http://tinkerpop.apache.org/docs/current/reference/#local-step) | |
| `not` | Použít tooproduce hello negace filtru | [není krok](http://tinkerpop.apache.org/docs/current/reference/#not-step) | |
| `optional` | Vrátí hello výsledek hello zadaný traversal Pokud dává else výsledek vrátí hello volání – element | [volitelný krok](http://tinkerpop.apache.org/docs/current/reference/#optional-step) | |
| `or` | Zajišťuje, aby se nejméně jedna z hello traversals vrací hodnotu | [nebo krok](http://tinkerpop.apache.org/docs/current/reference/#or-step) | |
| `order` | Vrátí výsledky v hello zadané pořadí řazení | [Krok pořadí](http://tinkerpop.apache.org/docs/current/reference/#order-step) | |
| `path` | Vrátí hello úplnou cestu hello traversal | [Krok cesty](http://tinkerpop.apache.org/docs/current/reference/#path-step) | |
| `project` | Projekty hello vlastnosti jako mapování | [Krok projektu](http://tinkerpop.apache.org/docs/current/reference/#project-step) | |
| `properties` | Vrátí hello vlastností hello určit popisky | [Krok vlastnosti](http://tinkerpop.apache.org/docs/current/reference/#properties-step) | |
| `range` | Filtry toohello zadaný rozsah hodnot| [Krok rozsahu](http://tinkerpop.apache.org/docs/current/reference/#range-step) | |
| `repeat` | Krok hello opakuje pro hello zadat počet opakování. Použít pro opakování ve smyčce | [Opakujte krok](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) | |
| `sample` | Použít toosample výsledky z hello traversal | [Ukázka krok](http://tinkerpop.apache.org/docs/current/reference/#sample-step) | |
| `select` | Použít tooproject výsledky z hello traversal |  [Vyberte krok](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Použít pro neblokující agregace z hello traversal | [Krok úložiště](http://tinkerpop.apache.org/docs/current/reference/#store-step) | |
| `tree` | Agregační cesty z vrchol do stromu | [Krok stromu](http://tinkerpop.apache.org/docs/current/reference/#tree-step) | |
| `unfold` | Nezobrazovaly iterovat jako krok| [unfold – krok](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) | |
| `union` | Sloučení výsledky z více traversals| [Union krok](http://tinkerpop.apache.org/docs/current/reference/#union-step) | |
| `V` | Zahrnuje hello kroky potřebné pro traversals mezi vrcholy a okrajů `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV`, a `otherV` pro | [vrchol kroky](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) | |
| `where` | Použít toofilter výsledky z hello traversal. Podporuje `eq`, `neq`, `lt`, `lte`, `gt`, `gte`, a `between` operátory  | [kde krok](http://tinkerpop.apache.org/docs/current/reference/#where-step) | |

Modul optimalizované zápisu Azure Cosmos DB podporuje automatické indexování všech vlastností v rámci vrcholy a okrajů ve výchozím nastavení. Proto dotazy s filtry, rozsahem dotazy, řazení, nebo agregace na žádnou vlastnost jsou zpracovány od hello indexu a efektivně obsluhovat. Další informace o tom, jak indexování funguje v Azure Cosmos DB najdete na našem dokumentu [vázané na schéma indexu](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Další kroky
* Začínáme vytvoření grafu aplikace [pomocí naší sady SDK](create-graph-dotnet.md) 
* Další informace o [podpory Azure Cosmos DB grafu](graph-introduction.md)
