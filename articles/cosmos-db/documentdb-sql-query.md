---
title: "aaaSQL dotazy pro rozhraní API služby Azure Cosmos databáze DocumentDB | Microsoft Docs"
description: "Další informace o syntaxi jazyka SQL, databáze koncepty a dotazy SQL pro Azure Cosmos DB. SQL lze použít jako dotazovací jazyk JSON v Azure Cosmos DB."
keywords: "syntaxe SQL, dotaz sql, sql dotazy, json dotazovací jazyk, databázových koncepcí a sql, agregační funkce"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.openlocfilehash: f4db95b87f5796c4e4299aaf016435cb6301bbfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a>Dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB
Microsoft Azure Cosmos DB podporuje dotazování dokumentů pomocí jazyka SQL (Structured Query Language) jako dotazovací jazyk JSON. Cosmos DB je skutečně bez schémat. Na základě jeho závazků toohello datového modelu JSON přímo v rámci hello databázový stroj poskytuje automatické indexování dokumentů JSON bez nutnosti explicitního schématu nebo vytváření sekundárních indexů. 

Při navrhování hello dotazovacího jazyka pro Cosmos DB, jsme měli dva cíle v paměti:

* Místo inventing o nový jazyk dotazů JSON, jsme chtěli toosupport SQL. SQL je jedním z jazyků hello nejvíce známé a oblíbených dotazů. SQL databáze cosmos umožňuje formální programovací model o bohaté dotazy prostřednictvím dokumentů JSON.
* Jako dokument databáze JSON může provést JavaScript přímo v databázovém stroji hello jsme chtěli toouse JavaScript programovací model jako hello foundation pro naše dotazovací jazyk. Hello DocumentDB SQL rozhraní API je integrován do systému typů JavaScript na vyhodnocení výrazu a volání funkce. Tato naopak poskytuje přirozené programovací model pro projekce relačních, hierarchických navigace mezi dokumenty JSON, vlastní spojení, prostorových dotazů a vyvolání uživatelem definovaných funkcí (UDF) vytvořené zcela v JavaScriptu mezi dalších funkcí. 

Věříme, že tyto funkce jsou klíče tooreducing hello tření mezi hello databázové a aplikační hello a jsou zásadní pro produktivita vývojářů.

Doporučujeme začít hello následující video, kde Aravind Ramachandran zobrazí možnosti dotazování Cosmos DB, sledování a navštívíte naše [Query Playground](http://www.documentdb.com/sql/demo), kde můžete vyzkoušet Cosmos DB a spouštět dotazy SQL pro naší datové sadě.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

Pak se vraťte toothis článku, kde Začneme s kurz dotaz SQL, který vás provede některé jednoduché dokumentů JSON a příkazy SQL.

## <a id="GettingStarted"></a>Začínáme s příkazy SQL v databázi systému Cosmos
toosee Cosmos SQL databáze v práci, umožňuje začínat několik jednoduchých dokumentů JSON a provede několik jednoduchých dotazů u ní. Vezměte v úvahu tyto dva dokumenty JSON o dvou řad. S Cosmos DB jsme nemusí toocreate všechny schémata nebo sekundárních indexů explicitně. Můžeme jednoduše potřebovat tooinsert hello JSON dokumentů tooa Cosmos DB kolekce a následně dotazu. Tady bychom měli jednoduché JSON dokumentů pro hello rodinu, hello nadřazených položek, děti (a jejich mazlíčků), adresu a informace o registraci. Hello dokumentu je řetězců, čísel, logické hodnoty, pole a vnořené vlastnosti. 

**Dokument**  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Tady je druhý dokument s jedním jemně rozdílem – `givenName` a `familyName` se používají místo `firstName` a `lastName`.

**Dokument**  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Nyní nyní si vyzkoušíte několik dotazů vůči tato data toounderstand některé hello klíče aspektů DocumentDB SQL rozhraní API. Například hello následující dotaz vrátí hello dokumenty, kde pole id hello odpovídá `AndersenFamily`. Vzhledem k tomu, že je `SELECT *`, výstup hello hello dotazu je hello dokončení dokumentu JSON:

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Teď se podíváme hello případ potřebujeme tooreformat hello výstup JSON v různých obrazce. Tento dotaz, zda projekty nové JSON objektu s dvě vybrané pole název a města, když hello adresa město má hello stejný název jako hello stavu. V tomto případě "NY, NY" odpovídá.

**Dotaz**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Výsledky**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Hello další dotaz vrátí všechny názvy daným hello podřízených prvků řady hello shoduje s id `WakefieldFamily` seřazené podle města hello pobytu.

**Dotaz**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Výsledky**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Rádi bychom znali tooa pozornost toodraw několik pozoruhodné aspektů hello Cosmos DB dotazu jazyka prostřednictvím hello příklady, které jste viděli, pokud:  

* Vzhledem k tomu, že DocumentDB SQL rozhraní API funguje na hodnoty JSON, zabývá stromu ve tvaru entity místo řádků a sloupců. Proto hello jazyk umožňuje naleznete toonodes hello stromu při jakékoli libovolný hloubce, jako je třeba `Node1.Node2.Node3…..Nodem`, podobně jako toorelational SQL odkazující toohello dvě části odkaz `<table>.<column>`.   
* Hello strukturovaná dotazu jazyka funguje s daty bez schématu. Proto dynamicky hello typ systému potřebám toobe hranice. Hello stejný výraz může přinést různých typů na různé dokumenty. Hello výsledek dotazu není platná hodnota JSON, ale není zaručena toobe pevného schématu.  
* Cosmos databáze podporuje pouze striktní dokumentů JSON. To znamená, že systém typů hello a výrazy s omezeným přístupem toodeal jenom s typy JSON. Odkazovat toohello [JSON specifikace](http://www.json.org/) další podrobnosti.  
* Cosmos DB kolekce je kontejner dokumentů JSON bez schémat. Hello vztahy v datových entit v rámci a na dokumentech v kolekci jsou implicitně zaznamenat členství ve skupině a ne primárního a cizího klíče relace. Toto je důležitým aspektem vhodné odkazující na základě spojení intra-document hello probírat později v tomto článku.

## <a id="Indexing"></a>Indexování cosmos DB
Než se nám získat do hello syntaxi DocumentDB SQL rozhraní API, je vhodné využít hello indexování návrhu v Cosmos DB. 

účelem Hello indexy databáze je tooserve dotazy v různých formách a obrazců pomocí spotřeby minimální prostředků (např. využití procesoru a vstup/výstup) současně poskytují dobrý prostupnosti a nízké latence. Často hello Volba správného indexu hello k dotazování databáze vyžaduje mnohem plánování a experimenty. Tento přístup představuje výzvu pro bez schématu databáze, kde hello data neodpovídají schématu striktní tooa a zpracovaní rychle. 

Proto když jsme navržený hello Cosmos DB indexování subsystému, nastaví hello následující cíle:

* Indexování dokumentů bez nutnosti schématu: hello indexování subsystému nevyžaduje žádné informace o schématu ani vytvořit žádný odhad o schématu hello dokumentů. 
* Podpora pro efektivní, bohaté hierarchické a relační dotazy: hello index podporuje hello Cosmos databáze dotazovací jazyk efektivně, včetně podpory pro hierarchické a relační projekce.
* Podpora pro konzistentní dotazy in face of svazek dlouhodobě zápisů: pro zápisu vysokou propustnost úlohy s konzistentní dotazy, hello aktualizace indexu postupně, efektivně a online hello stěně dlouhodobě svazku zápisů. aktualizace konzistentní index Hello je zásadní tooserve hello dotazy na úrovni konzistence hello v které hello uživateli nakonfigurovanému hello dokumentu služby.
* Podpora pro více klientů: zadána hello založené na vyhrazené modelu pro řízení prostředků mezi klienty v rámci rozpočtu hello systémových prostředků (procesoru, paměti a vstupně-výstupních operací za sekundu) přidělený na repliky jsou provedeny aktualizace indexu. 
* Efektivitu úložiště: pro finanční efektivita je hello na disk úložiště režie hello indexu ohraničené a předvídatelné. Toto je velmi důležitý, protože Cosmos DB vývojáře toomake náklady na základě kompromisy mezi režijní náklady na index výkonnosti dotazu toohello vztah umožňuje hello.  

Odkazovat toohello [Azure Cosmos DB – ukázky](https://github.com/Azure/azure-documentdb-net) na webu MSDN ukázek znázorňující, jak tooconfigure hello zásady indexování pro kolekci. Nyní Pojďme do hello podrobnosti o hello syntaxe Azure Cosmos DB SQL.

## <a id="Basics"></a>Základní informace o příkazu jazyka Azure Cosmos DB SQL
Každý dotaz sestává z klauzule SELECT a volitelné FROM a klauzule WHERE za standardy ANSI SQL. Pro každý dotaz, obvykle je výčet hello zdroj v klauzuli FROM hello. Potom hello filtrovat v hello klauzule WHERE se použije na hello zdroj tooretrieve podmnožinu dokumentů JSON. Nakonec se používá klauzuli SELECT hello tooproject hello požadované hodnoty JSON v hello vybrat seznamu.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>FROM – klauzule
Hello `FROM <from_specification>` klauzule je nepovinný, pokud je zdroj hello filtrovat nebo projekci později v dotazu hello. účelem Hello tuto klauzuli je zdroj dat hello toospecify, při které hello musí fungovat dotazu. Běžně hello celé kolekce je zdrojem hello, ale jeden místo toho zadat podmnožinu kolekce hello. 

Dotaz jako `SELECT * FROM Families` označuje, že celou kolekci rodiny hello je přes které tooenumerate hello zdroje. Zvláštní identifikátor KOŘENOVÉ lze použít toorepresent hello kolekce místo použití hello název kolekce. Hello následující seznam obsahuje hello pravidla, které vynucuje na jeden dotaz:

* kolekce Hello je to možné, jako například `SELECT f.id FROM Families AS f` nebo jednoduše `SELECT f.id FROM Families f`. Zde `f` je ekvivalentem hello `Families`. `AS`je identifikátor hello tooalias optional – klíčové slovo.
* Jednou alias, nemůže být vázán hello původního zdroje. Například `SELECT Families.id FROM Families f` je syntakticky neplatný, protože už nelze přeložit identifikátor hello "Rodiny".
* Všechny vlastnosti, které je třeba toobe odkazuje musí být plně kvalifikovaný. V hello chybí dodržování striktní schématu, je to vynucené tooavoid žádné nejednoznačný vazby. Proto `SELECT id FROM Families f` je syntakticky neplatný od hello vlastnost `id` není vázán.

### <a name="subdocuments"></a>Vnořené dokumenty
Hello zdroj může být také snížené tooa menší podmnožinu. Například tooenumerating pouze podstrom v každém dokumentu hello subroot může pak mohou stát hello zdroje, jak ukazuje následující příklad hello:

**Dotaz**

    SELECT * 
    FROM Families.children

**Výsledky**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Při hello výše příklad pole jako zdroj hello, objekt mohou být využity také jako hello zdroj, který je informace zobrazené v hello následující ukázka: žádné platná hodnota JSON (nedefinovaná), můžete najít ve zdroji hello je považován za pro zařazení výsledek hello dotaz Hello. Pokud nemáte některé rodiny `address.state` hodnotu, jsou vyloučeny ve výsledku dotazu hello.

**Dotaz**

    SELECT * 
    FROM Families.address.state

**Výsledky**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>Klauzule WHERE
klauzule WHERE Hello (**`WHERE <filter_condition>`**) je volitelný. Určuje, zda text hello, podmínku (podmínky), musí splňovat dokumentů JSON hello poskytované hello zdroje v pořadí toobe jako součást výsledků hello. Dokumentu JSON se musí vyhodnotit hello zadané podmínky příliš "true" toobe považována za výsledek hello. použití klauzule vrstvou hello indexu v pořadí toodetermine hello absolutní nejmenší podmnožinu dokumentů zdroje, které můžou být součástí hello výsledek Hello. 

Hello následující dotaz požadavků dokumentů, které obsahují název vlastnosti, jehož hodnota je `AndersenFamily`. Jiného dokumentu, který nemá název vlastnosti, nebo kde hello hodnota neodpovídá `AndersenFamily` je vyloučen. 

**Dotaz**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Hello předchozí příklad ukázal dotazu jednoduché rovnosti. DocumentDB SQL rozhraní API také podporuje celou řadu skalární výrazy. Hello nejčastěji používaná jsou binární a unární výrazy. Odkazy na vlastnost z objektu JSON, hello zdroje jsou také platné výrazy. 

Hello následující binární operátory jsou aktuálně podporovány a lze je použít v dotazech, jak je znázorněno v hello následující příklady:  

<table>
<tr>
<td>Aritmetické operace</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitový</td>    
<td>|, &, ^, <<>>,, >>> (nula výplně posunutí doprava)</td>
</tr>
<tr>
<td>Logické</td>
<td>A, NEBO NE</td>
</tr>
<tr>
<td>Porovnání</td>    
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Řetězec</td>    
<td>|| (zřetězení)</td>
</tr>
</table>  


Podívejme se na některé dotazy pomocí binární operátory.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unární operátory Hello +,-, ~ není jsou podporovány také a dá se použít uvnitř dotazy, jak je znázorněno v hello následující ukázka:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Kromě toho toobinary a unární operátory, vlastnost odkazy jsou také povoleny. Například `SELECT * FROM Families f WHERE f.isRegistered` vrátí hello dokumentu JSON obsahující hello vlastnost `isRegistered` kde hodnota vlastnosti hello je rovna toohello JSON `true` hodnotu. Všechny ostatní hodnoty (false, hodnotu null a nedefinovaná, `<number>`, `<string>`, `<object>`, `<array>`atd) vede k vyloučení z výsledku hello toohello zdrojový dokument. 

### <a name="equality-and-comparison-operators"></a>Operátory rovnosti a porovnání
Hello následující tabulka uvádí hello výsledek porovnání rovnosti v DocumentDB API SQL mezi všechny dva typy JSON.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>OP</strong>
         </td>
         <td valign="top">
            <strong>Nedefinovaná</strong>
         </td>
         <td valign="top">
            <strong>Hodnotu Null</strong>
         </td>
         <td valign="top">
            <strong>Logická hodnota</strong>
         </td>
         <td valign="top">
            <strong>Číslo</strong>
         </td>
         <td valign="top">
            <strong>Řetězec</strong>
         </td>
         <td valign="top">
            <strong>Objekt</strong>
         </td>
         <td valign="top">
            <strong>Pole</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nedefinovaná<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Hodnotu Null<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Logická hodnota<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Číslo<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Řetězec<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objekt<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Pole<strong>
         </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
Nedefinovaná </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
      </tr>
   </tbody>
</table>

Pro jiné operátory porovnání jako >, > =,! =, < a < =, hello platí následující pravidla:   

* Výsledkem porovnání mezi typy Undefined.
* Porovnání mezi dvěma objekty nebo dvě maticových má za následek Undefined.   

Pokud je výsledek hello hello skalární výraz, který ve filtru hello Undefined, hello odpovídající dokument není zahrnuta do hello výsledek, protože Undefined není označení rovnosti logicky příliš "true".

### <a name="between-keyword"></a>MEZI klíčové slovo
Můžete taky hello BETWEEN – klíčové slovo tooexpress dotazy na rozsah hodnot jako v ANSI SQL. MEZI můžete použít u řetězců nebo čísla.

Například tento dotaz vrací všechny rodiny dokumenty, ve které hello úrovni prvním podřízeným objektem je mezi 1-5 (obě včetně). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Na rozdíl od v ANSI SQL, můžete taky klauzule BETWEEN hello v klauzuli FROM hello jako hello následující ukázka.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Kratší časy spuštění dotazu mějte na paměti toocreate indexování zásad, která používá typ indexu rozsah proti jakékoli číselné vlastnosti nebo cesty, které jsou filtrovány v klauzuli BETWEEN hello. 

Hello hlavní rozdíl mezi použitím BETWEEN v DocumentDB rozhraní API a ANSI SQL je, že můžete express rozsah dotazy na vlastnosti smíšený typů – například můžete mít "základní" být číslo (5) v některých dokumentů a řetězce v jiné ("grade4"). V těchto případech jako je v jazyce JavaScript, porovnání mezi dva různé typy výsledků v "undefined" a hello dokumentu bude přeskočen.

### <a name="logical-and-or-and-not-operators"></a>Logický (AND, OR a NOT) operátory
Logické operátory pracovat logické hodnoty. Hello logické pravdivosti tabulky pro tyto operátory jsou uvedené v následující tabulky hello.

| NEBO | True | False | Nedefinovaná |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Nedefinovaná |
| Nedefinovaná |True |Nedefinovaná |Nedefinovaná |

| A | True | False | Nedefinovaná |
| --- | --- | --- | --- |
| True |True |False |Nedefinovaná |
| False |False |False |False |
| Nedefinovaná |Nedefinovaná |False |Nedefinovaná |

| NENÍ |  |
| --- | --- |
| True |False |
| False |True |
| Nedefinovaná |Nedefinovaná |

### <a name="in-keyword"></a>IN – klíčové slovo
Hello IN – klíčové slovo lze použít toocheck, zda zadaná hodnota odpovídá žádnou hodnotu v seznamu. Například tento dotaz vrací všechny rodiny dokumenty, kde je hello id "WakefieldFamily" nebo "AndersenFamily". 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Tento příklad vrátí všechny dokumenty, je-li hello stát žádné hello zadané hodnoty.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Unární (?) a operátory Coalesce (?)
operátory Unární a Coalesce Hello se dá použít toobuild podmíněné výrazy, podobně jako toopopular programovacích jazyků, jako je C# a JavaScript. 

operátor unární (?) Hello může být velmi užitečný, pokud fyzicky dostavili vytváření nových vlastností JSON na hello. Například teď můžete napsat dotazy tooclassify hello třída úrovně do lidského čitelné podoby jako Začátečník nebo zprostředkující nebo Upřesnit, jak je uvedeno níže.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Můžete také vnořovat hello operátor volání toohello jako v dotazu hello níže.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Jako s dalšími operátory dotazu, pokud hello odkazované vlastnosti podmíněného výrazu hello chybí v dokumentu, nebo pokud hello typy porovnávané se liší, pak tyto dokumenty jsou vyloučeny ve výsledcích dotazů hello.

Hello Coalesce (?) operátor může být použit tooefficiently zkontrolujte přítomnost hello vlastnost (také známa jako je definován) v dokumentu. To je užitečné při dotazování na částečně strukturovaných nebo data smíšený typů. Tento dotaz vrací například hello "lastName" Pokud existuje, nebo "Přezdívka" hello, pokud není přítomen.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Vlastnost uvozovkách přístupového objektu
Můžete také přístup k vlastnostem pomocí operátoru vlastnost uvozovkách hello `[]`. Například `SELECT c.grade` a `SELECT c["grade"]` odpovídají. Tato syntaxe je užitečné, když potřebujete tooescape vlastnost, která obsahuje mezery, speciální znaky, nebo se stane tooshare hello stejný název jako SQL – klíčové slovo nebo vyhrazené slovo.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>Klauzule SELECT
Klauzule SELECT Hello (**`SELECT <select_list>`**) je povinná a určuje, jaké hodnoty jsou načteny z hello dotaz, podobně jako v ANSI SQL. podmnožina Hello je filtrované nad hello zdroj dokumenty jsou předávány do fáze projekce hello, kde hello zadané hodnoty JSON se načítají a je vytvořený nový objekt JSON, pro každý vstupní předán na něj. 

Hello následující příklad ukazuje typické dotaz SELECT. 

**Dotaz**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Vnořené vlastnosti
V následujícím příkladu hello, jsme jsou projekce dvě vnořené vlastnosti `f.address.state` a `f.address.city`.

**Dotaz**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projekce také podporuje JSON výrazy, jak ukazuje následující příklad hello:

**Dotaz**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Podívejme se na roli hello `$1` sem. Hello `SELECT` klauzule potřebuje toocreate objekt JSON a vzhledem k tomu, že žádný klíč je k dispozici, používáme názvy proměnných implicitní argument počínaje `$1`. Například tento dotaz vrací dvě implicitní argument proměnné s názvem bez přípony `$1` a `$2`.

**Dotaz**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasy
Nyní Pojďme hello příklad rozšířit nad s explicitní aliasy hodnot. Stejně jako se používá pro aliasy – klíčové slovo hello. Zadání je volitelné, jak je znázorněno při plánování hello druhá hodnota, která jako `NameInfo`. 

V případě, že dotaz má dvě vlastnosti se hello stejný název, aliasy musí být použité toorename jedno nebo obě hello vlastnosti tak, aby se jsou od sebe jednoznačně rozlišeny v projekci hello výsledek.

**Dotaz**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalární výrazy
Kromě toho tooproperty odkazuje, klauzule SELECT hello také podporuje skalární výrazy konstanty, aritmetických výrazech, logických výrazů, atd. Tady je příklad jednoduchého dotazu "Hello World".

**Dotaz**

    SELECT "Hello World"

**Výsledky**

    [{
      "$1": "Hello World"
    }]


Zde je ukázka používající skalární výraz.

**Dotaz**

    SELECT ((2 + 11 % 7)-2)/3    

**Výsledky**

    [{
      "$1": 1.33333
    }]


V následujícím příkladu hello hello výsledek hello skalární výraz, který je logická hodnota.

**Dotaz**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Výsledky**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Vytvoření objektu a pole
Další klíčovou funkcí DocumentDB SQL rozhraní API je vytvoření pole nebo objektu. V předchozím příkladu hello Všimněte si, že jsme vytvořili nový objekt JSON. Podobně jeden můžete také vytvořit pole jak je znázorněno v hello následující příklady:

**Dotaz**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Výsledky**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a id="ValueKeyword"></a>VALUE – klíčové slovo
Hello **hodnotu** – klíčové slovo poskytuje hodnotu způsob tooreturn formátu JSON. Například hello dotazu vidíte níže vrátí hello skalární `"Hello World"` místo `{$1: "Hello World"}`.

**Dotaz**

    SELECT VALUE "Hello World"

**Výsledky**

    [
      "Hello World"
    ]


Hello následující dotaz vrátí hodnotu JSON hello bez hello `"address"` popisek ve výsledcích hello.

**Dotaz**

    SELECT VALUE f.address
    FROM Families f    

**Výsledky**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Hello následující příklad rozšiřuje tento tooshow jak tooreturn JSON primitivní hodnoty (hello listové úrovni stromu hello JSON). 

**Dotaz**

    SELECT VALUE f.address.state
    FROM Families f    

**Výsledky**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* – Operátor
Hello speciální – operátor (*) je podporované tooproject hello dokumentu jako-je. Pokud se používá, musí být, že hello k projekci pouze pole. Při dotazu jako `SELECT * FROM Families f` je platný, `SELECT VALUE * FROM Families f ` a `SELECT *, f.id FROM Families f ` nejsou platné.

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <a id="TopKeyword"></a>Operátor TOP
TOP – klíčové slovo Hello lze použít toolimit hello počet hodnot z dotazu. Při horní se používá ve spojení s hello klauzule ORDER by, sadu výsledků hello je omezená toohello první N počet seřazené hodnoty; Funkce hello první N počet výsledků v nedefinované pořadí. Jako osvědčený postup v příkazu SELECT, vždy používejte klauzuli ORDER BY pomocí klauzule TOP hello. To je jedinou možností hello toopredictably označují řádky, které jsou ovlivněné TOP. 

**Dotaz**

    SELECT TOP 1 * 
    FROM Families f 

**Výsledky**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

HORNÍ lze použít s konstantní hodnotou (jak jsme ukázali výše) nebo s hodnotou proměnné použití parametrických dotazů. Další podrobnosti najdete v tématu parametrizované dotazy níže.

### <a id="Aggregates"></a>Agregační funkce
Můžete také provádět agregace v hello `SELECT` klauzule. Agregační funkce provádět výpočet sadu hodnot a vrátí jednu hodnotu. Například hello následující dotaz vrátí počet hello rodiny dokumentů v rámci kolekce hello.

**Dotaz**

    SELECT COUNT(1) 
    FROM Families f 

**Výsledky**

    [{
        "$1": 2
    }]

Můžete se taky vrátit hello skalární hodnota hello agregační pomocí hello `VALUE` – klíčové slovo. Například hello následující dotaz vrátí hello počet hodnot jako jediné číslo:

**Dotaz**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Výsledky**

    [ 2 ]

Můžete také provést agregace v kombinaci s filtry. Například hello následující dotaz vrátí hello počet dokumentů s adresou hello v hello státu Washington.

**Dotaz**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Výsledky**

    [ 1 ]

Hello následující tabulka uvádí hello seznam podporovaných agregační funkce v DocumentDB rozhraní API. `SUM`a `AVG` se provádí přes číselných hodnot, zatímco `COUNT`, `MIN`, a `MAX` lze provést přes čísla, řetězce, logické hodnoty a hodnoty Null. 

| Využití | Popis |
|-------|-------------|
| POČET | Vrátí hello počet položek ve výrazu hello. |
| SOUČET   | Vrátí hello součet všech hodnot hello ve výrazu hello. |
| MIN.   | Vrátí hello minimální hodnotu ve výrazu hello. |
| MAXIMÁLNÍ POČET   | Vrátí hello maximální hodnotu ve výrazu hello. |
| PRŮMĚR   | Vrátí hello průměr hodnot hello ve výrazu hello. |

Agreguje lze také provést přes hello výsledky iterace pole. Další informace najdete v tématu [pole iterace v dotazech](#Iteration).

> [!NOTE]
> Při použití hello Průzkumníka dotazů portálu Azure, Všimněte si, že agregace dotazy může vracet hello částečně agregované výsledky dotazu stránky. Hello sady SDK vytvoří jednu kumulativní hodnotu na všech stránkách. 
> 
> V pořadí tooperform agregace dotazy pomocí kódu, potřebujete .NET SDK 1.12.0, .NET Core SDK 1.1.0 nebo Java SDK 1.9.5 nebo vyšší.    
>

## <a id="OrderByClause"></a>Klauzuli ORDER by
Podobně jako v ANSI SQL, můžete zahrnout volitelné klauzule Order By při dotazování. klauzule Hello může obsahovat volitelné ASC nebo DESC argument toospecify hello pořadí ve kterém musí načíst výsledky.

Zde je například dotaz, který načte rodiny v pořadí podle název hello města trvalé.

**Dotaz**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Výsledky**

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

A tady je dotaz, který načte rodiny v pořadí podle data vytvoření, které je uloženo jako číslo představující hello epoch čas, tj, uplynulý čas od 1. ledna 1970 v sekundách.

**Dotaz**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Výsledky**

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <a id="Advanced"></a>Pokročilé databázových koncepcí a dotazy SQL

### <a id="Iteration"></a>Iterace
Byl přidán nový konstrukce prostřednictvím hello **IN** – klíčové slovo v DocumentDB API SQL tooprovide podpora iterování přes pole JSON. Zdroj FROM Hello poskytuje podporu pro iterací. Začneme hello následující ukázka:

**Dotaz**

    SELECT * 
    FROM Families.children

**Výsledky**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nyní Podíváme se na další dotaz, který provádí iteraci přes podřízené položky v kolekci hello. Všimněte si hello rozdíl v poli výstup hello. Tento příklad rozdělí `children` a vyrovná hello výsledky do jednoho pole.  

**Dotaz**

    SELECT * 
    FROM c IN Families.children

**Výsledky**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

To může být další používané toofilter na každou položku hello pole, jak je znázorněno v hello následující ukázka:

**Dotaz**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Výsledky**  

    [{
      "givenName": "Lisa"
    }]

Můžete také provést agregace přes hello výsledek iterace pole. Například hello následující dotaz vrátí hello počet podřízených prvků mezi všechny řady.

**Dotaz**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Výsledky**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Spojení
V relační databázi je důležité hello nutné toojoin mezi tabulkami. Jeho hello logické corollary toodesigning normalized schémat. Jinak zvláštní toothis, DocumentDB rozhraní API se zabývá hello nenormalizované datový model bez schémat dokumentů. Toto je logický ekvivalent hello a "spojení sama na sebe".

Hello syntaxe, který podporuje jazyk hello je < from_source1 > připojit < from_source2 > připojit... Připojte < from_sourceN >. Celkově platí, tento příkaz vrátí sadu **N**- n-tice (řazené kolekce členů s **N** hodnoty). Každá řazená kolekce členů má vyprodukované všechny aliasy kolekce iterování přes jejich příslušné sady hodnot. Jinými slovy Toto je úplná smíšený produkt sad hello účastní spojení hello.

Hello následující příklady ukazují, jak funguje klauzuli JOIN hello. V následujícím příkladu hello hello výsledek je prázdná, protože hello smíšený produkt každého dokumentu ze zdroje a prázdnou sadou je prázdný.

**Dotaz**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Výsledky**  

    [{
    }]


V následujícím příkladu hello, je hello spojení mezi kořen dokumentu hello a hello `children` subroot. Je smíšený produkt mezi dvěma objekty JSON. Hello skutečnost, že podřízené objekty je pole není platná v hello spojení, protože jsme se zabývají na jednom kořenovou, která je hello podřízených prvků pole. Proto hello výsledek obsahuje pouze dva výsledky, protože hello smíšený produkt každý dokument s polem hello vypočítá přesně pouze jeden dokument.

**Dotaz**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Výsledky**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Hello následující příklad ukazuje konvenční připojení:

**Dotaz**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Výsledky**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Hello nejprve thing toonote je tento hello `from_source` z hello **připojení** klauzule je iterátor. Ano hello tok v takovém případě je následující:  

* Rozbalte každý podřízený element **c** v poli hello.
* Použít smíšený produkt s hello kořen dokumentu hello **f** s každou podřízený element **c** , byl průmětu v prvním kroku hello.
* Nakonec projektu hello kořenový objekt **f** name – vlastnost samostatně. 

první dokument Hello (`AndersenFamily`) obsahuje pouze jeden podřízený element, takže hello sadu výsledků dotazu obsahuje pouze jeden objekt odpovídající toothis dokumentu. druhý dokumentu Hello (`WakefieldFamily`) obsahuje dva podřízené položky. Ano hello smíšený produkt vytváří samostatný objekt pro všechny podřízené, což by vedlo k dva objekty, jednu pro každý dokument podřízené odpovídající toothis. Kořenová Hello pole v obou tyto dokumenty jsou stejné, hello stejně, jako byste očekávali v smíšený produkt.

Hello skutečných nástroj z hello spojení je tooform řazenými kolekcemi členů z hello smíšený produkt ve tvaru, který je jinak tooproject obtížná. Kromě toho, jak vidíte v následujícím příkladu hello, byste mohli vyfiltrovat na kombinaci hello řazené kolekce členů umožňuje hello Uživatel reagoval podmínku celkové uspokojit hello řazené kolekce členů.

**Dotaz**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Výsledky**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Tento příklad představuje přirozené rozšíření Dobrý den předcházející příklad a spojí double. Ano hello smíšený produkt je možné zobrazit jako hello následující pseudo kódu:

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`má jednu podřízenou, který má jednoho nebo více mazlíčků. Ano, hello smíšený produkt vypočítá jeden řádek (1\*1\*1) z této rodiny. WakefieldFamily ale má dva podřízené, ale pouze jednu podřízenou "Jesse" má mazlíčků. Jesse, když má dva mazlíčků. Proto hello smíšený produkt vypočítá 1\*1\*řádků z této rodině, 2 = 2.

V dalším příkladu hello, je další filtr na `pet`. Nevztahuje se na všechny řazených kolekcí členů hello kde hello pet název není "Stínové". Všimněte si, že jsme jsou možné toobuild řazenými kolekcemi členů z pole filtru na všech elementů hello hello řazené kolekce členů a projektu libovolnou kombinaci elementů hello. 

**Dotaz**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Výsledky**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>Integrace jazyka JavaScript
Azure Cosmos DB poskytuje programovací model pro spouštění logiky aplikace založené na jazyce JavaScript přímo na hello kolekce z hlediska uložených procedur a aktivačních událostí. To umožňuje, aby obě:

* Možnost toodo vysoce výkonné transakční operace CRUD a dotazy na dokumenty v kolekci na základě hello těsná integrace běhu programu JavaScript přímo v rámci hello databázového stroje. 
* Fyzická modelování tok řízení, proměnné rozsahu a přiřazení a integrace výjimky zpracování primitiv s databázové transakce. Další informace o podpoře Azure Cosmos DB integrace jazyka JavaScript naleznete v toohello JavaScript na straně serveru programovatelnosti dokumentaci.

### <a id="UserDefinedFunctions"></a>Uživatelem definované funkce (UDF)
Společně s typy hello už definované v tomto článku DocumentDB SQL rozhraní API poskytuje podporu pro uživatele definované funkce (UDF). Skalární funkce UDF zejména, jsou podporovány, kde hello vývojáři můžete předat v počtu nula či více argumentů a vrácení zpět výsledku jeden argument. Každý z těchto argumentů, se kontroluje na právě platné hodnoty na JSON.  

Hello syntaxi DocumentDB SQL rozhraní API je rozšířeno toosupport vlastní logiky aplikace pomocí tyto funkce definované uživatelem. Funkce UDF lze registrovat pomocí rozhraní API DocumentDB a pak odkazuje v rámci dotazu SQL. Ve skutečnosti hello UDF jsou exquisitely určená toobe vyvolané dotazy. Jako volba corollary toothis, funkce UDF nemají objekt kontextu toohello přístup který hello jiných JavaScript mají typy (uložených procedur a aktivačních událostí). Vzhledem k tomu, že dotazy se spustí jen pro čtení, mohou spouštět na primární nebo na sekundární repliky. Proto UDF jsou navrženou toorun na sekundárních replikách na rozdíl od jiných typů jazyka JavaScript.

Níže je příklad, jak můžete registrovat UDF u databáze hello Cosmos DB, konkrétně v rámci kolekce dokumentů.

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

Hello předchozí příklad vytvoří UDF, jehož název je `REGEX_MATCH`. Přijímá dvou řetězcových hodnot JSON `input` a `pattern` a ověří, zda je první odpovídá hello zadat vzor hello v hello druhý pomocí funkce string.match() jazyce JavaScript.

Tato UDF jsme teď můžete použít v dotazu v projekci. Funkce UDF musí být kvalifikovaný pomocí hello malá a velká písmena předponu "udf." Když je volána v rámci dotazů. 

> [!NOTE]
> Předchozí too3/17/2015 Cosmos DB podporované UDF volání bez hello "udf." Předpona jako vyberte REGEX_MATCH(). Tento vzor volání je zastaralá.  
> 
> 

**Dotaz**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Výsledky**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

Hello UDF můžete použít také uvnitř filtr, jak je znázorněno v následujícím příkladu hello, také kvalifikovaný pomocí hello "udf." Předpona:

**Dotaz**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Výsledky**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


V podstatě UDF jsou platné skalární výrazy a mohou být používány projekce a filtry. 

tooexpand na výkon hello UDF, podíváme se na další příklad s podmíněnou logiku:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


Dole je příklad, cvičení hello UDF.

**Dotaz**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

**Výsledky**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Jako hello předchozí příklady představením, funkce UDF integrovat hello sílu jazyka JavaScript tooprovide hello DocumentDB SQL rozhraní API bohaté programovatelný rozhraní toodo komplexní procedurální, podmíněného logiku hello pomoci integrované prostředí JavaScript runtime Možnosti.

DocumentDB SQL rozhraní API poskytuje hello argumenty toohello UDF pro každý dokument ve zdroji hello, v hello aktuální fázi (klauzuli WHERE nebo klauzuli SELECT) zpracování hello UDF. Hello výsledek je obsažena v bezproblémově hello celkové spouštěcí kanál. Pokud hello vlastnosti odkazované tooby hello UDF parametry nejsou k dispozici v hello hodnota JSON hello, že parametr se považuje za není definována a proto hello UDF volání zcela přeskočen. Podobně pokud hello výsledek hello UDF, není součástí hello výsledek. 

V souhrnu funkce UDF jsou skvělý nástroje toodo komplexní obchodní logiky v rámci dotazu hello.

### <a name="operator-evaluation"></a>Vyhodnocení – operátor
Cosmos databáze, hello důsledku způsobená databáze JSON nevykresluje parallels s operátory jazyka JavaScript a jeho sémantiku vyhodnocení. Při Cosmos DB pokusí toopreserve sémantiku JavaScript z hlediska podporu JSON, v některých případech odchylují hello operaci vyhodnocení.

V DocumentDB SQL rozhraní API, na rozdíl od v tradiční SQL hello typů hodnot se často není známý dokud hello hodnoty jsou načteny z databáze. V pořadí tooefficiently spouštět dotazy, většina hello operátory má požadavky na typ strict. 

DocumentDB API SQL neprovede implicitní převody, na rozdíl od jazyka JavaScript. Například dotazu jako `SELECT * FROM Person p WHERE p.Age = 21` odpovídá dokumentů, které obsahují ve vlastnosti stáří, jehož hodnota je 21. Jiného dokumentu, jejichž stáří vlastnost odpovídá řetězec "21" nebo jiných může být nekonečné variace jako "021", "21.0", "0021", "00021", nebude odpovídat atd. Toto je na rozdíl od toohello JavaScript, kde jsou hodnoty řetězce hello implicitně převedena toonumbers (podle operátoru, například: ==). Tato volba je zásadní pro efektivní indexu odpovídající v DocumentDB SQL rozhraní API. 

## <a name="parameterized-sql-queries"></a>Parametrizované dotazy SQL
Cosmos DB podporuje dotazy s parametry vyjádřit s hello známé @ zápis. Parametrizované SQL poskytuje robustní zpracování a uvozovací znaky vstup uživatele brání náhodnou expozici dat prostřednictvím Injektáž SQL. 

Můžete například napsat dotaz, který přebírá příjmení a stav adresy jako parametry a potom spusťte pro různé hodnoty příjmení a stav adresy založené na vstup uživatele.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Tento požadavek potom můžete odeslat tooCosmos DB jako parametrizovaného dotazu JSON, jako vidíte níže.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Hello argument tooTOP se dá nastavit pomocí parametrizované dotazy, jako vidíte níže.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Hodnoty parametru může být libovolný platný kód JSON (řetězce, čísla, logické hodnoty null, dokonce i pole nebo vnořený JSON). Také bez schématu totiž Cosmos DB parametry nejsou ověřovat na libovolného typu.

## <a id="BuiltinFunctions"></a>Integrované funkce
Cosmos DB podporuje také řadu integrovaných funkcí pro běžné operace, které lze použít uvnitř dotazy jako uživatelsky definované funkce (UDF).

| Funkce skupiny          | Operace                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Matematické funkce  | ABS mezní hodnoty, EXP, FLOOR, protokolu, LOG10, POWER, KRUHOVÉ, přihlášení, SQRT, HRANATÉ, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COP, STUPŇŮ, PI, RADIÁNECH, SIN a TAN |
| Typ kontroly funkce | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED a IS_PRIMITIVE                                                           |
| Řetězcové funkce        | CONCAT, obsahuje, ENDSWITH, INDEX_OF, vlevo, délka, nižší, LTRIM, NAHRAĎTE, REPLIKOVAT, zpětného, vpravo, RTRIM, STARTSWITH, SUBSTRING a horní       |
| Funkce pole         | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH a ARRAY_SLICE                                                                                         |
| Prostorové funkce       | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID a ST_ISVALIDDETAILED                                                                           | 

Pokud aktuálně používáte uživatelem definované funkce (UDF) pro kterou integrovaná funkce je nyní k dispozici, byste měli používat hello odpovídající integrovaná funkce, bude rychlejší toorun toobe a další efektivně. 

### <a name="mathematical-functions"></a>Matematické funkce
Hello matematické funkce každé provedení výpočtu, podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu. Zde je tabulku podporovaných předdefinovaných matematické funkce.


| Využití | Popis |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Vrátí hello absolutní (kladné) hello zadána hodnota číselného výrazu. |
| [Horní MEZ (num_expr)](#bk_ceiling) | Vrátí hello nejmenší celé číslo větší než nebo rovno, hello zadaný číselný výraz. |
| [FLOOR (num_expr)](#bk_floor) | Vrátí hello největší celé číslo menší než nebo rovna toohello zadán číselný výraz. |
| [EXP (num_expr)](#bk_exp) | Vrátí exponent hello hello zadán číselný výraz. |
| [PROTOKOL (num_expr [, základní])](#bk_log) | Vrátí hello přirozený logaritmus hello číselného výrazu, nebo pomocí hello logaritmus hello zadán základní |
| [LOG10 (num_expr)](#bk_log10) | Vrátí hello základu 10 logaritmické hello zadána hodnota číselného výrazu. |
| [ZAOKROUHLÍ (num_expr)](#bk_round) | Vrátí číselnou hodnotu, zaokrouhlené toohello nejbližší celé číslo. |
| [TRUNC (num_expr)](#bk_trunc) | Vrátí číselnou hodnotu, zkrácený toohello nejbližší celé číslo. |
| [SQRT (num_expr)](#bk_sqrt) | Vrátí hello druhou odmocninu čísla hello zadán číselný výraz. |
| [HRANATÉ (num_expr)](#bk_square) | Vrátí hello odmocnina z hello zadán číselný výraz. |
| [NAPÁJENÍ (num_expr, num_expr)](#bk_power) | Vrátí hello výkon hello zadán toohello hodnotu číselného výrazu. |
| [PŘIHLÁŠENÍ (num_expr)](#bk_sign) | Vrátí hello přihlašovací (-1, 0, 1) hello zadána hodnota číselného výrazu. |
| [ACOS (num_expr)](#bk_acos) | Vrátí hello úhel v radiánech, jehož kosinus je hello zadaný číselného výrazu; Zkratka Arkus. |
| [ASIN (num_expr)](#bk_asin) | Vrátí hello úhlu, v radiánech, jehož sinus je hello zadán číselný výraz. To je také označován Arkus sinus. |
| [ATAN (num_expr)](#bk_atan) | Vrátí hello úhlu, v radiánech, jehož tangens je hello zadán číselný výraz. To je také označován Arkus. |
| [ATN2 (num_expr)](#bk_atn2) | Vrátí text hello úhel v radiánech, mezi hello kladné osy x a hello ray z hello počátek toohello bodu (y, x), kde x a y jsou hodnoty hello hello dvou výrazů zadaný float. |
| [COS (num_expr)](#bk_cos) | Vrátí hello trigonometrické kosinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz. |
| [COP (num_expr)](#bk_cot) | Vrátí hello trigonometrické kotangens hello úhlu, uvedeného v radiánech, v hello zadán číselný výraz. |
| [STUPŇŮ (num_expr)](#bk_degrees) | Vrátí hello odpovídající úhel ve stupních pro úhlu uvedeného v radiánech. |
| [PI)](#bk_pi) | Vrátí hello konstantní hodnotu čísla PÍ. |
| [RADIÁNECH (num_expr)](#bk_radians) | Vrátí radiánech při zadání číselného výrazu, ve stupních, se. |
| [SIN (num_expr)](#bk_sin) | Vrátí hello trigonometrické sinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz. |
| [TAN (num_expr)](#bk_tan) | Vrátí tangens hello vstupní výraz hello v hello zadaný výraz. |

Například můžete spustit nyní dotazy jako hello následující:

**Dotaz**

    SELECT VALUE ABS(-4)

**Výsledky**

    [4]

Hello hlavní rozdíl mezi tooANSI funkce porovnání Cosmos databáze SQL je, že jsou navrženou toowork i s daty bez schématu a smíšený schématu. Například pokud máte dokument, kdy hello velikost vlastnost chybí, nebo má jiné než číselné hodnoty jako "Neznámý" a potom dokument hello je přeskočil, místo vrátila chybu.

### <a name="type-checking-functions"></a>Typ kontroly funkce
Kontrola, zda Funkce Hello typu Povolit toocheck hello typ výrazu v rámci dotazů SQL. Typ funkce Kontrola, zda lze použít toodetermine hello typ vlastnosti v rámci dokumenty v chodu hello, když je neznámý nebo proměnné. Zde je tabulku kontroluje funkce podporované předdefinovaný typ.

<table>
<tr>
  <td><strong>Využití</strong></td>
  <td><strong>Popis</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty pole.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud hello typ hodnoty hello je logická hodnota.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty number.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud hello typ hodnoty hello je objekt JSON.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty string.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazena hodnota.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (výraz)</a></td>
  <td>Vrátí logickou hodnotu udávající, pokud je typ hello hello hodnoty řetězce, číslo, logickou hodnotu nebo hodnotu null.</td>
</tr>

</table>

Pomocí těchto funkcí, teď můžete spouštět dotazy jako hello následující:

**Dotaz**

    SELECT VALUE IS_NUMBER(-4)

**Výsledky**

    [true]

### <a name="string-functions"></a>Řetězcové funkce
Hello následující skalární funkce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota. Tady je tabulku funkce integrované řetězce:

| Využití | Popis |
| --- | --- |
| [Délka (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Vrátí hello počet znaků, který hello zadán řetězcovým výrazem |
| [CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |Vrátí řetězec, který je výsledkem hello zřetězení dvou nebo více řetězcové hodnoty. |
| [SUBSTRING (str_expr, num_expr num_expr.)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Vrátí část řetězcového výrazu. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý |
| [OBSAHUJE (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu obsahuje hello druhý. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |Vrátí počáteční pozici prvního výskytu hello hello druhý řetězcového výrazu v rámci zadaného řetězcového výrazu první hello nebo -1, pokud není nalezen řetězec hello hello. |
| [LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Vrátí hello levé části řetězce s hello zadaný počet znaků. |
| [RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Vrátí hello právo součástí řetězec s hello zadaný počet znaků. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Vrací výraz řetězce po ho odebere úvodní mezery. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Vrací výraz řetězce po zkracování všechny koncové mezery. |
| [NIŽŠÍ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Vrací výraz řetězce po převodu dat toolowercase velké písmeno. |
| [HORNÍ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Vrací výraz řetězce po převodu dat toouppercase malé písmeno. |
| [NAHRAĎTE (str_expr, str_expr str_expr.)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Nahradí všechny výskyty zadaná řetězcová hodnota s jinou hodnotou řetězce. |
| [REPLIKOVAT (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |Opakuje hodnotu řetězce zadaného počtu opakování. |
| [ZPĚTNÉHO (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Vrátí hello zpětné pořadí řetězcovou hodnotu. |

Pomocí těchto funkcí, můžete nyní spustit dotazy jako následující hello. Například se můžete vrátit název rodiny hello na velká písmena následujícím způsobem:

**Dotaz**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Výsledky**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Nebo zřetězení řetězců jako v tomto příkladu:

**Dotaz**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Výsledky**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Funkce řetězce mohou sloužit také v hello kde klauzule toofilter výsledky, stejně jako v nástroji hello následující ukázka:

**Dotaz**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Výsledky**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Funkce pole
Hello následující skalární funkce provedení operace hodnota vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota. Zde je také tabulka předdefinovaných pole funkcí:

| Využití | Popis |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Vrátí číslo hello elementů hello zadaný výraz pole. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |Vrátí pole, které je výsledkem hello zřetězení dvě nebo více hodnot pole. |
| [ARRAY_CONTAINS (arr_expr, výraz [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Vrátí logickou hodnotu udávající, zda text hello pole obsahuje hello zadané hodnotě. Můžete zadat, pokud je shoda hello celé nebo jeho část. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Vrátí část výraz pole. |

Pole funkce se dá použít toomanipulate pole v rámci JSON. Tady je příklad dotaz, který vrátí všechny dokumenty, kde jedním z nadřazené položky hello je "Každý s každým Wakefieldů". 

**Dotaz**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]

Můžete zadat částečné fragment pro odpovídající elementů v rámci pole hello. Hello následující dotaz hledá všechny nadřazené objekty s hello `givenName` z `Robin`.

**Dotaz**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]


Zde je další příklad používající ARRAY_LENGTH tooget hello počet podřízených prvků na rodiny.

**Dotaz**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Výsledky**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Prostorové funkce
Cosmos DB podporuje následující otevřete geoprostorové Consortium (OGC) integrované funkce pro dotazování geoprostorové hello. 

<table>
<tr>
  <td><strong>Využití</strong></td>
  <td><strong>Popis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Vrátí hello vzdálenost mezi hello dva GeoJSON bodu, mnohoúhelníku nebo LineString výrazů.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Vrací výraz logická hodnota určující, zda text hello první GeoJSON objekt (bod, mnohoúhelníku nebo LineString) je v rámci hello druhý GeoJSON objekt (bod, mnohoúhelníku nebo LineString).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Vrátí hodnotu označující, zda text hello dva zadané GeoJSON objekty (bod, mnohoúhelníku nebo LineString) intersect logický výraz.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Vrátí logickou hodnotu udávající, zda zadaný hello GeoJSON bodu, mnohoúhelníku nebo LineString výraz není platný.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello Zadaný bod GeoJSON, mnohoúhelníku nebo LineString výraz je platná a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.</td>
</tr>
</table>

Prostorové funkce můžou být použité tooperform blízkosti dotazy pro prostorová data. Tady je příklad dotaz, který vrátí že všechny rodiny dokumenty, v rámci 30 km hello zadané umístění používáte hello ST_DISTANCE integrovaná funkce. 

**Dotaz**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]

Další informace o podporovaných geoprostorové v Cosmos DB, najdete v tématu [práci s daty geoprostorové v Azure Cosmos DB](geospatial.md). Který zabalí prostorových funkce a hello syntaxe SQL pro Cosmos DB. Nyní Podívejme se na tom, jak LINQ dotazování funguje a jak komunikuje se syntaxí hello jsme viděli dosavadní.

## <a id="Linq"></a>LINQ tooDocumentDB SQL rozhraní API
LINQ je programovací model rozhraní .NET, která vyjadřuje výpočetní jako dotazy na datové proudy objektů. Cosmos DB poskytuje knihovna klienta toointerface s dotazy LINQ usnadněním převod mezi objekty JSON a rozhraní .NET a mapování z určité podmnožiny LINQ dotazy tooCosmos DB dotazy. 

Hello obrázek níže ukazuje architekturu hello podporu dotazů LINQ pomocí Cosmos DB.  Pomocí klienta aplikace hello Cosmos DB vývojáři můžou vytvářet **IQueryable** objektu, že přímo dotazy hello Cosmos DB dotaz na zprostředkovatele, který pak překládá dotaz LINQ hello do dotazu Cosmos DB. Hello dotazu je předána toohello tooretrieve Cosmos DB serveru a sadu výsledků ve formátu JSON. výsledky jsou deserializovat do datového proudu objektů .NET na straně klienta hello vrátit Hello.

![Architektura podporu dotazů LINQ pomocí rozhraní API DocumentDB - syntaxe SQL, JSON dotazovací jazyk, databázových koncepcí a dotazy SQL][1]

### <a name="net-and-json-mapping"></a>Rozhraní .NET a mapování JSON
Hello mapování mezi objekty .NET a dokumenty JSON je přirozené – každé datové pole, člen je mapován objekt JSON tooa, kde je název pole hello namapované část "klíč" toohello objektu hello a část "value" hello je součástí hodnota toohello rekurzivně namapované hello objektu. Vezměte v úvahu hello následující ukázka: objekt rodiny hello vytvořený je namapované toohello dokumentu JSON, jak je uvedeno níže. A naopak a hello dokumentu JSON je namapované back tooa objekt .NET.

**C# – třída**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-toosql-translation"></a>Překlad tooSQL LINQ
Hello Cosmos DB dotaz na zprostředkovatele provede nejlepší úsilí mapování z dotazu LINQ do dotazu Cosmos DB SQL. V následující popis hello předpokládáme, že hello čtečky má základní znalosti o LINQ.

Nejprve hello typ systému, budeme podporovat všechny JSON primitivní typy – číselnými typy, logická hodnota, string a hodnotu null. Jsou podporovány pouze tyto typy JSON. jsou podporovány následující skalární výrazy Hello.

* Konstantní hodnoty – patří konstantní hodnoty hello primitivní datové typy ve hello dobu, kdy je vyhodnocován dotaz hello.
* Vlastnost nebo pole indexu výrazy – tyto výrazy odkazovat toohello vlastnosti objektu nebo k elementu pole.
  
     rodiny. ID;    Family.Children[0].familyName;    Family.Children[0].Grade;    Family.Children[n].Grade; n je proměnná typu int.
* Aritmetických výrazech – jedná se o běžné aritmetických výrazech na číselné a logické hodnoty. Úplný seznam hello najdete v části specifikace toohello SQL.
  
     2 * family.children[0].grade;    x a y;
* Výraz pro porovnání řetězce - patří porovnávání řetězcovou hodnotu toosome konstantní řetězec hodnotu.  
  
     mother.familyName == "Smith";    child.givenName == s; s je proměnná řetězce
* Objekt nebo pole vytvoření výrazu - tyto výrazy návratový typ složené hodnoty nebo anonymní typ objekt nebo pole tyto objekty. Tyto hodnoty mohou být použity.
  
     novou nadřazenou položku {familyName = "Smith", givenName = "Jan"}; nové {nejprve = 1, druhý = 2}; anonymní typ s dvě pole              
     New [] int {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Seznam podporovaných operátory LINQ
Tady je seznam podporovaných LINQ operátory ve zprostředkovateli LINQ hello součástí hello DocumentDB .NET SDK.

* **Vyberte**: projekce převede toohello SQL SELECT, včetně vytváření objektů
* **Kde**: filtry převede toohello kde SQL a podporovat překlad mezi & &, || a! operátory toohello SQL
* **Označit více**: umožňuje unwinding klauzule SQL JOIN toohello pole. Může být toofilter výrazy použité toochain/vnoření na elementy pole
* **OrderBy a OrderByDescending**: překládá tooORDER BY pořadí
* **Počet**, **součet**, **Min**, **maximální**, a **průměrná** operátory pro agregaci a jejich ekvivalenty asynchronní  **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, a **AverageAsync**.
* **CompareTo**: překládá toorange porovnání. Běžně používané pro řetězce vzhledem k tomu, že nejste porovnatelný v rozhraní .NET
* **Trvat**: překládá toohello horní SQL pro omezení výsledků dotazu
* **Matematické funkce**: podporuje překlad z. Asin Abs Acos, je NET, Atan, Ceiling Cos, Exp, Floor, protokolu, Log10, Pow, kruhové, přihlášení, Sin, Sqrt, Tan, zkrátit toohello ekvivalentní SQL integrované funkce.
* **Funkce pro řetězce**: podporuje překlad z. NET na Concat, obsahuje, EndsWith, IndexOf, počet, ToLower, TrimStart, nahraďte, zpětného, TrimEnd, StartsWith, dílčí řetězec, ToUpper toohello ekvivalentní SQL integrované funkce.
* **Pole funkce**: podporuje překlad z. NET na Concat, obsahuje a počet toohello ekvivalentní SQL integrované funkce.
* **Funkce rozšíření geoprostorové**: podporuje překlad z metod se zakázaným inzerováním vzdálenost v rámci IsValid a IsValidDetailed toohello ekvivalentní SQL integrované funkce.
* **Uživatelem definované funkce rozšíření funkce**: podporuje překlad z hello zóny se zakázaným inzerováním metoda UserDefinedFunctionProvider.Invoke toohello odpovídající uživatelem definované funkce.
* **Různé**: podporuje překlad hello sloučení a podmíněné operátory. Může překládat obsahuje tooString obsahuje, ARRAY_CONTAINS nebo hello v SQL v závislosti na kontextu.

### <a name="sql-query-operators"></a>Operátory dotazu SQL
Zde jsou některé příklady, které ilustrují, jak některé hello standardní operátory dotazu LINQ přeložit dolů tooCosmos DB dotazy.

#### <a name="select-operator"></a>Select – operátor
Syntaxe Hello je `input.Select(x => f(x))`, kde `f` je skalární výraz.

**Výraz lambda LINQ**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**Výraz lambda LINQ**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**Výraz lambda LINQ**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>Označit více – operátor
Syntaxe Hello je `input.SelectMany(x => f(x))`, kde `f` se skalární výraz, který vrátí typ kolekce.

**Výraz lambda LINQ**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Kde – operátor
Syntaxe Hello je `input.Where(x => f(x))`, kde `f` je skalární výraz, který vrací logickou hodnotu.

**Výraz lambda LINQ**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**Výraz lambda LINQ**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Složené příkazy jazyka SQL
Hello výše operátory může být složený tooform více výkonných dotazů. Vzhledem k tomu, že Cosmos DB podporuje vnořené kolekcí, hello složení můžete být zřetězen nebo vnořený.

#### <a name="concatenation"></a>Zřetězení
Syntaxe Hello je `input(.|.SelectMany())(.Select()|.Where())*`. Zřetězené dotaz můžete spustit v volitelný `SelectMany` dotazu následuje více `Select` nebo `Where` operátory.

**Výraz lambda LINQ**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**Výraz lambda LINQ**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**Výraz lambda LINQ**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**Výraz lambda LINQ**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Vnoření
Syntaxe Hello je `input.SelectMany(x=>x.Q())` kde Q je `Select`, `SelectMany`, nebo `Where` operátor.

Vnitřní dotaz hello v vnořený dotaz, je použité tooeach elementu hello vnější kolekce. Jednou z důležitou součást je, že takový dotaz vnitřní hello najdete toohello pole hello elementů v kolekci vnější hello jako spojení sama na sebe.

**Výraz lambda LINQ**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**Výraz lambda LINQ**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**Výraz lambda LINQ**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a id="ExecutingSqlQueries"></a>Provádění dotazů SQL
Cosmos DB zveřejňuje prostředky přes rozhraní REST API, kterou lze volat v jakémkoli jazyce schopném zasílat požadavky HTTP/HTTPS. Cosmos DB dále nabízí programovací knihovny pro několik oblíbených jazyků, jako je rozhraní .NET, Node.js, JavaScript a Python. Hello REST API a hello různé knihovny všechny podporovat, dotazování pomocí SQL. Hello .NET SDK podporuje dotazování kromě tooSQL LINQ.

Dobrý den, jak následující příklady zobrazují toocreate dotazu a odešlete ji proti Cosmos DB databázového účtu.

### <a id="RestAPI"></a>ROZHRANÍ REST API
Cosmos DB nabízí otevřete RESTful programovací model přes protokol HTTP. Databáze účtů se dá zřídit pomocí předplatného Azure. model prostředků Cosmos DB Hello obsahuje sadu prostředků v rámci účtu databáze, z nichž každý je adresovatelné logické a stabilní identifikátoru URI. Sadu prostředků je odkazované tooas informačního kanálu v tomto dokumentu. Databázový účet se skládá ze sady databází, každá obsahuje několik kolekcí, každý z které naopak obsahovat dokumenty, funkce UDF a další typy prostředků.

model základní interakce Hello pomocí těchto prostředků je prostřednictvím příkazy hello HTTP GET, PUT, POST a odstranit pomocí jejich standardní překladu. příkaz POST Hello se používá pro vytvoření nového prostředku, pro spuštění uložené procedury nebo pro zadání dotazu Cosmos DB. Dotazy jsou vždy jen pro čtení operací s žádné vedlejší účinky.

Hello následující příklady ukazují POST pro rozhraní API DocumentDB dotaz směřovaný na kolekce obsahující hello dva ukázkové dokumenty, že jsme si přečetli dosavadní práce. Hello dotazu je jednoduchý filtr u hello JSON název vlastnosti. Všimněte si použití hello hello `x-ms-documentdb-isquery` a Content-Type: `application/query+json` toodenote hlavičky, která hello operaci je dotaz.

**Požadavek**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


**Výsledky**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Hello druhý příklad ukazuje komplexnější dotaz, který vrátí více výsledků z hello spojení.

**Požadavek**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Výsledky**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Pokud výsledku dotazu se nemůže vejít do jedné stránky s výsledky, pak hello REST API vrátí token pokračování prostřednictvím hello `x-ms-continuation-token` hlavičky odpovědi. Klienti mohou stránkování výsledků zahrnutím hello hlavičky v následných výsledky. Hello počet výsledků na stránce lze také řídit prostřednictvím hello `x-ms-max-item-count` číslo záhlaví. Pokud zadaný dotaz hello má agregační funkci jako `COUNT`, pak stránku hello dotaz může vrátit hodnotu částečně agregované přes hello stránky s výsledky. Hello klientům musí provést přes tyto výsledky tooproduce hello konečných výsledků, například agregace druhé úrovně, součet přes hello počty vrátil hello jednotlivé stránky tooreturn hello celkový počet.

zásady konzistence dat hello toomanage pro dotazy, použijte hello `x-ms-consistency-level` záhlaví jako všechny požadavky REST API. Konzistence typu relace, je požadovaná tooalso echo hello nejnovější `x-ms-session-token` hello dotazu požadavek obsahoval hlavičku souboru Cookie. Hello zásady indexování dotazované kolekce můžete také ovlivnit hello konzistence výsledků dotazu. Hello výchozí nastavení zásady indexování, pro kolekce hello index je vždy aktuální pomocí obsahu dokumentu hello a dotazů, výsledky odpovídaly hello konzistence zvolené pro data. Pokud hello indexování zásad volný tooLazy, dotazy mohou vracet zastaralé výsledky. Další informace najdete v tématu [úrovně konzistence databáze Azure Cosmos][consistency-levels].

Pokud zásady indexování hello nakonfigurované na kolekci hello nepodporuje zadaný dotaz hello, vrátí server Azure Cosmos DB hello 400 "Chybný požadavek". Se vrátí pro dotazy na rozsah pro cesty, které jsou nakonfigurované pro vyhledávání hodnoty hash (rovnosti) a cesty explicitně vyloučená z indexování. Hello `x-ms-documentdb-query-enable-scan` záhlaví může být zadaný tooallow hello dotazu tooperform kontrolu, když indexu není k dispozici.

Podrobné metriky můžete získat na spuštění dotazu nastavením `x-ms-documentdb-populatequerymetrics` záhlaví příliš`True`. Další informace najdete v tématu [metriky dotazů SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](documentdb-sql-query-metrics.md).

### <a id="DotNetSdk"></a>SADA SDK JAZYKA C# (.NET)
Hello .NET SDK podporuje LINQ i SQL dotazování. Hello následující příklad ukazuje, jak tooperform hello jednoduchého filtru dotazu zavedená dříve v tomto dokumentu.

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Tato ukázka porovná dvě vlastnosti rovnosti v rámci každého dokumentu a používá anonymní projekce. 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Hello další příklad ukazuje spojení, vyjádřit pomocí LINQ označit více.

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



klient .NET Hello automaticky iteruje všechny stránky hello výsledků dotazu v blocích foreach hello jako v příkladu nahoře. Možnosti zavedené v hello REST API části jsou také k dispozici v dotazu Hello hello .NET SDK pomocí hello `FeedOptions` a `FeedResponse` třídy v hello CreateDocumentQuery metoda. Dobrý den, kolik stránek se dá řídit pomocí hello `MaxItemCount` nastavení. 

Můžete také explicitně řídit stránkování tak, že vytvoříte `IDocumentQueryable` pomocí hello `IQueryable` objekt, pak načtením` ResponseContinuationToken` hodnot a jejich předání zpět jako `RequestContinuationToken` v `FeedOptions`. `EnableScanInQuery`může být sada tooenable kontrol, když hello dotazu nemůže být nepodporuje hello nakonfigurované zásady indexování. Pro dělené kolekce, můžete použít `PartitionKey` toorun hello dotaz jednoho oddílu (i když Cosmos DB může automaticky extrahovat to z textu hello dotazu), a `EnableCrossPartitionQuery` toorun dotazy, které může být nutné toobe spouštění více oddílů. 

Odkazovat příliš[Azure Cosmos DB .NET ukázky](https://github.com/Azure/azure-documentdb-net) pro další ukázky obsahující dotazy. 

### <a id="JavaScriptServerSideApi"></a>Rozhraní API jazyka JavaScript na straně serveru
Cosmos DB poskytuje programovací model pro spouštění logiky aplikace založené na jazyce JavaScript přímo na kolekce hello pomocí uložených procedur a aktivačních událostí. logiky Javascriptové Hello registrované na úrovni kolekce potom můžou provádět databázové operace na operace hello na dokumentech hello hello zadané kolekce. Tyto operace jsou zabalená vedlejším ACID transakcí.

Hello následující příklad ukazuje, jak se dotazuje dokumenty toouse hello dotazu v toomake hello rozhraní API serveru JavaScript z uvnitř uložené procedury a triggery.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a id="References"></a>Odkazy
1. [Úvod tooAzure Cosmos DB][introduction]
2. [Azure Cosmos DB SQL specifikace](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Ukázek Azure DB Cosmos rozhraní .NET](https://github.com/Azure/azure-documentdb-net)
4. [Úrovně konzistence databáze Azure Cosmos][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. Specifikace jazyka JavaScript [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Dotaz na vyhodnocení techniky pro velké databáze [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Zpracování v systémech paralelní relační databáze, stiskněte společnosti IEEE počítače, 1994 dotazů
11. Logická jednotka, Ooi, Tan, zpracování v systémech paralelní relační databáze, stiskněte společnosti IEEE počítače, 1994 dotazů.
12. Olston Kryštof, Robert Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: není tak cizí jazyk pro zpracování dat, SIGMOD 2008.
13. G. Graefe. Hello Cascades architektura pro optimalizaci dotazu. Eng. IEEE dat Bull., 18(3): 1995.

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md