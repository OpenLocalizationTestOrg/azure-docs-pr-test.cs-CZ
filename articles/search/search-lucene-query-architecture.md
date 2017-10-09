---
title: "aaaFull text search engine (Lucene) architektura ve službě Azure Search | Microsoft Docs"
description: "Vysvětlení Lucene dotaz zpracování a dokumentu načtení koncepty pro fulltextové vyhledávání, jako související tooAzure vyhledávání."
services: search
manager: jhubbard
author: yahnoosh
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/06/2017
ms.author: jlembicz
ms.openlocfilehash: c6d1bea8d40154fd9237b9e44584cdfcd193cbd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a>Jak úplné textové vyhledávání funguje ve službě Azure Search

Tento článek je pro vývojáře, kteří potřebují lépe pochopili, jak funguje Lucene fulltextové vyhledávání ve službě Azure Search. Pro dotazy text Azure Search ve většině scénářů bezproblémově dodá očekávané výsledky, ale někdy může získat výsledek, který se zdá, že "off" nějakým způsobem. V těchto situacích s pozadí ve hello čtyři fáze provádění dotazů Lucene (dotaz analýzy, lexikální analýzy, dokumentů párování, vyhodnocování) vám může pomoct identifikovat konkrétní změny tooquery parametry nebo index konfigurace, která bude poskytovat hello požadovaný výsledek. 

> [!Note] 
> Služba Azure Search používá Lucene pro fulltextové vyhledávání, ale Lucene integrace není vyčerpávající. Jsme selektivně vystavení a rozšířit Lucene funkce tooenable hello scénáře důležité tooAzure vyhledávání. 

## <a name="architecture-overview-and-diagram"></a>Přehled architektury a diagram

Zpracování textu v plném znění vyhledávací dotaz začíná analýza hello dotazu text tooextract hledaným výrazům. Vyhledávací Hello používá indexování dokumentů tooretrieve s odpovídající podmínky. Konkrétní dotaz podmínky jsou někdy rozdělit a obnovených do nového formuláře toocast širší net přes co může považovat za potenciální shody. Je sada výsledků dotazu je pak seřazené podle relevance skóre přiřazené tooeach jednotlivých odpovídající dokumentu. Ty hello horní části hello seznam hodnocení jsou vráceny toohello volající aplikace.

Revidovat, provádění dotazů má čtyři fáze: 

1. Analýza dotazu 
2. Lexikální analýzy 
3. Načtení dokumentu 
4. Vyhodnocování 

Hello obrázek níže znázorňuje hello komponenty používané tooprocess žádost o vyhledávání. 

 ![Diagram architektury dotazů Lucene ve službě Azure Search][1]


| Klíčové komponenty | Funkční popis | 
|----------------|------------------------|
|**Dotaz analyzátory** | Operátory dotazu nezávislá na infrastruktuře vyhledávacích dotazů a vytvořte hello dotazu struktury (stromové dotazu) toobe odeslané toohello vyhledávací web. |
|**Analyzátory** | Lexikální analýzám na vyhledávacích dotazů. Tento proces může zahrnovat transformace, odebrání nebo rozšiřování vyhledávacích dotazů. |
|**Index** | Strukturu efektivní dat používá toostore a uspořádat prohledávatelné podmínky extrahovat z indexovaného dokumentů. |
|**Vyhledávací web** | Načítá a skóre dokumentů na základě obsahu hello hello odpovídající obrácený index. |

## <a name="anatomy-of-a-search-request"></a>Anatomie žádost o vyhledávání

Žádost o vyhledávání je dokončení specifikaci co má být vrácen sadě výsledků dotazu. V nejjednodušší podobě je prázdný dotaz s žádná kritéria jakéhokoli druhu. Realističtější příklad obsahuje parametry, několik dotazu podmínky, případně vymezená toocertain pole, může být výraz filtru a řazení pravidla.  

Hello následující příklad je žádost o vyhledávání může odeslat tooAzure vyhledávání pomocí hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

~~~~
POST /indexes/hotels/docs/search?api-version=2016-09-01 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

Pro tento požadavek hello vyhledávacího webu hello následující:

1. Filtry dokumentů, kde je cena hello alespoň $60 a menší než 300 USD.
2. Provede dotaz hello. V tomto příkladu hello vyhledávací dotaz se skládá z frází a podmínky: `"Spacious, air-condition* +\"Ocean view\""` (uživatelé obvykle nezadávejte interpunkční znaménka, ale včetně v příkladu hello umožňuje tooexplain jak ji zpracovat analyzátorů). Pro tento dotaz hello vyhledávacího webu vyhledá hello popis a název pole zadaný v `searchFields` pro dokumenty, které obsahují "Oceánu zobrazení" a dále na hello termín "velké" nebo na podmínky, které začínají předponou hello "air-condition". Hello `searchMode` parametr je použité toomatch na všechny termín (výchozí) nebo všechny z nich pro případy, kdy není explicitně požadované termín (`+`).
3. Objednávky hello výslednou sadu hotels pomocí bezkontaktní tooa zadané geografické umístění a poté vrácen toohello volající aplikace. 

Hello většina Tento článek se týká zpracování hello *vyhledávací dotaz*: `"Spacious, air-condition* +\"Ocean view\""`. Filtrování a řazení jsou mimo rozsah. Další informace najdete v tématu hello [referenční dokumentace rozhraní API pro vyhledávání](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>Fáze 1: Analýza dotazu 

Jak jsme uvedli, řetězec dotazu hello je první řádek hello hello žádosti: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

Analyzátor odděluje operátory dotazu Hello (například `*` a `+` v příkladu hello) z hledání podmínky a deconstructs hello vyhledávací dotaz do *poddotazy* podporovaného typu: 

+ *Termín dotazu* pro samostatné podmínky (třeba velké)
+ *dotaz frázi* uvozovkách podmínek (podobně jako zobrazení oceánu)
+ *Předpona dotazu* podmínek, za nímž následuje operátor předponu `*` (jako jsou air-condition)

Úplný seznam podporovaných dotazu typy najdete v části [sytnax dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Operátory přidružené poddotazu určit, zda dotaz hello "musí být" nebo "by měla být" splněna v pořadí pro dokument toobe považovány za shodné. Například `+"Ocean view"` je "musí" kvůli toohello `+` operátor. 

Analyzátor dotazu Hello ke změně struktury hello poddotazy do *dotazu stromu* (interní strukturu představující dotazu hello) předá v toohello vyhledávací web. V první fázi hello dotazu analýza hello dotazu stromu vypadat třeba takto.  

 ![Logická hodnota dotazu searchmode všechny][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Podporované analyzátory: jednoduchý a úplné Lucene 

 Služba Azure Search zpřístupní dvou různých dotazu jazyků, `simple` (výchozí) a `full`. Pomocí nastavení hello `queryType` parametr s vaši žádost o vyhledávání se zjistit analyzátor dotazu hello jazyk dotazu, který zvolíte, aby věděl, že může jak toointerpret hello operátory a syntaxe. Hello [jednoduchý dotaz jazyka](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) je intuitivní a robustní, často vhodný toointerpret uživatelský vstup jako-je bez zpracování na straně klienta. Podporuje operátory dotazu známých z webové vyhledávacích webů. Hello [úplné Lucene dotazovací jazyk](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), které dostanete tak, že nastavení `queryType=full`, rozšiřuje hello výchozí jednoduchý dotaz jazyk přidáním podpory pro další operátory a typy dotazů jako zástupný znak, přibližné, regulární výraz a dotazy v rámci pole. Například by odeslaných za jednoduchá syntaxe dotazů regulární výraz vyhodnocen jako řetězec dotazu a není výraz. žádost o Hello příklad v tomto článku používá hello úplné Lucene dotazovací jazyk.

### <a name="impact-of-searchmode-on-hello-parser"></a>Dopad searchMode hello analyzátoru 

Jiný parametr žádost o vyhledávání, který ovlivňuje analýza je hello `searchMode` parametr. Ovládá hello výchozí operátor pro logickou dotazy: všechny (výchozí) nebo všechny.  

Když `searchMode=any`, který je hello výchozím hello místo odděleny velké a air-condition je nebo (`||`), provedení ekvivalentní hello ukázkový dotaz text: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Explicitní operátory, jako například `+` v `+"Ocean view"`, jsou jednoznačné v dotazu Logická konstrukce (hello termín *musí* odpovídat). Jak toointerpret hello zbývající podmínky je méně zřejmé: velké a air-condition. Měli hello vyhledávacího webu najít odpovídá na zobrazení oceánu *a* velké *a* air-condition? Nebo by měl zjistit oceánu zobrazení plus *buď jeden* z hello zbývající podmínky? 

Ve výchozím nastavení (`searchMode=any`), vyhledávací hello předpokládá hello širší interpretace. Buď pole *by měl* odpovídat, které odráží sémantiku "nebo". Hello počátečního dotazu stromu zobrazené dříve, s hello dvě operace "by", ukazuje hello výchozí.  

Předpokládejme, že teď nastavujeme `searchMode=all`. V takovém případě hello místo interpretována jako "a" operaci. Každý zbývající podmínky hello musí být součástí hello dokumentu tooqualify jako shoda. Ukázkový dotaz výsledné Hello by interpretovat následujícím způsobem: 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

Strom upravený dotaz pro tento dotaz bude následující, kde odpovídající dokument je hello průnik všechny tři poddotazy: 

 ![Logická hodnota dotazu searchmode všechny][3]

> [!Note] 
> Výběr `searchMode=any` přes `searchMode=all` je nejlepší rozhodnutí byly přijaty spuštěním reprezentativní dotazy. Uživatelé, kteří jsou pravděpodobně tooinclude operátory (společný při vyhledávání dokument uloží) může intuitivnější výsledky pokud `searchMode=all` informuje konstrukce boolean dotazu. Další informace o tom hello vztahu mezi `searchMode` a operátory, najdete v části [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>Fáze 2: Lexikální analysis 

Lexikální analyzátorů proces *termín dotazy* a *fráze dotazy* po strukturovaná hello dotazu stromu. Analyzátor přijme hello tooit zadané vstupy textu hello analyzátor, procesy hello text a pak zasílá zpět tokenizovaného podmínky, které toobe začleněná do hello stromu dotazu. 

Nejběžnější formulář Hello lexikální analýzy *lingvistické analysis* který transformuje vyhledávacích dotazů podle pravidla konkrétní tooa zadané jazyka: 

* Snižuje formuláře dotazu termín toohello kořenové slova 
* Odebrání nepotřebných slova (stopslova, jako je například "na" nebo "a" v angličtině) 
* Rozdělení složené slovo do součásti 
* Nižší velká slova velká a malá písmena 

Všechny tyto operace jsou obvykle tooerase rozdíly mezi zadávání textu hello poskytované hello uživatele a hello podmínky uložené v indexu hello. Tyto operace nad rámec zpracování textu a vyžaduje zevrubnou znalost jazyka hello sám sebe. tooadd podporuje tuto vrstvu lingvistické sledování, Azure vyhledávání dlouhý seznam [analyzátory jazyka](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene a společnosti Microsoft.

> [!Note]
> Požadavky na Analysis rozsahu minimální tooelaborate v závislosti na vašem scénáři. Můžete řídit složitost lexikální analýzy hello výběrem jedné z analyzátorů hello předdefinované nebo vytvoření vlastních [vlastní analyzátor](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Analyzátory nejsou vymezená toosearchable pole a jsou zadané jako součást definice pole. To vám umožní toovary lexikální analýzy na základě za pole. Tento parametr nezadáte, hello *standardní* Lucene analyzer je použít.

V našem příkladu předchozí tooanalysis hello počátečního dotazu stromu má hello termín "Spacious" s velká písmena "S" a čárku hello analyzátor dotazu interpretuje jako součást termín dotazu hello (čárkou nepovažuje operátor jazyk dotazu).  

Když analyzátor výchozí hello zpracovává hello termín, bude malá písmena "zobrazení oceánu" a "velké" a odeberte hello čárku. Hello upravený dotaz stromu bude vypadat takto: 

 ![Logická hodnota dotaz s analyzovaných podmínky][4]

### <a name="testing-analyzer-behaviors"></a>Testování Analyzátor chování 

chování Hello analyzátor lze otestovat pomocí hello [analyzovat rozhraní API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Zadejte text hello, který budete chtít tooanalyze toosee jaké podmínky zadané analyzátor vygeneruje. Například toosee, jak by zpracovat standardní analyzátor hello hello text "air-condition", můžete vydat hello následující požadavek:

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

Standardní analyzátor Hello dělí do následujících dvou tokeny, zadávání poznámek o nich s atributy, jako je počáteční a koncové posunutí (používá se pro přístupů zvýraznění), jakož i jejich polohu (používá se pro párování frázi) hello vstupního textu hello:

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

### <a name="exceptions-toolexical-analysis"></a>Analýza toolexical výjimky 

Lexikální analýzy se vztahuje pouze tooquery typy, které vyžadují úplný podmínky – dotazu termín nebo frázi dotazu. Netýkají tooquery typů s neúplné podmínky – předpona dotazu, dotaz zástupný znak, regulární výraz dotazu – nebo tooa přibližné dotazu. Ty typy, včetně hello předponu dotazu s termín dotazů *air-condition\**  v našem příkladu se přidají přímo stromu dotazu toohello obcházení hello analysis fáze. Hello pouze transformaci u vyhledávacích dotazů těchto typů provést, je předpoklady.

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a>Fáze 3: Načtení dokumentu 

Načtení dokumentu odkazuje toofinding dokumentů v indexu hello s odpovídajícími položkami. Tato fáze odhalíte nejlépe v příkladu. Začněme s indexem hotels s hello následující jednoduché schématu: 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

Další Předpokládejme, že tento index obsahuje následující čtyři dokumenty hello: 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance toohello beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on hello north shore of hello island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

**Jak jsou indexované podmínky**

načtení toounderstand pomáhá tooknow několik základní informace o indexování. jednotka Hello úložiště je obráceným index, jeden pro každé prohledávatelné pole. V rámci obráceným index je seřazený seznam všechny podmínky ze všech dokumentů. Každému termínu mapuje toohello seznam dokumentů, v nichž se vyskytuje, zřejmá jako v následujícím příkladu hello.

tooproduce hello podmínky v indexu obráceným hello vyhledávacího webu provede lexikální analýzu přes hello obsah dokumentů, podobně jako toowhat probíhá při zpracování dotazu. Vstupní textové hodnoty jsou předávány tooan analyzátor, používají formát nižší, odstraní interpunkční znaménka, a tak dále, v závislosti na konfiguraci analyzátor hello. Je běžné, ale nejsou vyžadovány, toouse hello stejné analyzátory pro vyhledávání a indexování operations tak, aby vypadal jako podmínky uvnitř hello index další podmínky dotazu.

> [!Note]
> Služba Azure Search umožňuje určit různé analyzátory pro indexování a hledání prostřednictvím další `indexAnalyzer` a `searchAnalyzer` pole parametrů. Pokud tento parametr nezadáte, hello analyzátor s hello `analyzer` vlastnost se používá pro indexování a vyhledávání.  

**Převedený index pro příklad dokumenty**

Vrácení tooour třeba pro hello **název** pole, index hello obrácený vypadat třeba takto:

| Označení | Seznam dokumentů |
|------|---------------|
| atman | 1 |
| plážový | 2 |
| hotelů | 1, 3 |
| oceánu | 4  |
| playa | 3 |
| možnost | 3 |
| Retreat | 4 |

V hello pole název, pouze *hotelů* se zobrazí v dva dokumenty: 1, 3.

Pro hello **popis** pole hello index je následujícím způsobem:

| Označení | Seznam dokumentů |
|------|---------------|
| letecké | 3
| a | 4
| plážový | 1
| záleží | 3
| možnost | 3
| Vzdálenost | 1
| Island | 2
| kauaʻi | 2
| nachází | 2
| – sever | 2
| oceánu | 1, 2, 3
| z | 2
| na |2
| quiet | 4
| místnosti  | 1, 3
| secluded | 4
| pobřeží | 2
| Velké | 1
| Dobrý den | 1, 2
| příliš| 1
| zobrazit | 1, 2, 3
| procházení | 1
| S | 3


**Odpovídající vyhledávacích dotazů vůči indexované podmínky**

Indexy hello obrácený výše, umožňuje vrátit toohello ukázkový dotaz a v tématu Jak odpovídající dokumenty pro náš příklad dotazu nebyly nalezeny. Odvolat že stromové struktuře poslední dotaz hello vypadá takto: 

 ![Logická hodnota dotaz s analyzovaných podmínky][4]

Během provádění dotazu jednotlivé dotazy jsou u spustit prohledatelná pole hello nezávisle. 

+ TermQuery Vítejte "velké", odpovídá dokumentu 1 (hotelů Atman). 

+ Hello PrefixQuery, "air-condition *", se neshoduje se žádné dokumenty. 

  Toto je chování, které někdy confuses vývojáři. I když hello termín klimatizovaným existuje v dokumentu hello, je rozdělený do dvou podmínky podle hello výchozí analyzátor. Odvolat, že předpona dotazy, které obsahují částečnou podmínky, nejsou analýza. Proto podmínky s předponu "air-condition" jsou vyhledávány v hello obrácený index a nebyl nalezen.

+ PhraseQuery Hello "oceánu zobrazení", vyhledá podmínky hello "oceánu" a "Zobrazit" a zkontroluje hello blízko podmínkami hello původního dokumentu. Dokumenty, 1, 2 a 3 odpovídat tento dotaz do pole Popis hello. Všimněte si dokument 4 má hello oceánu termín v hlavě hello ale není považovány za shodné, jako jsme to potřebné pro frázi "zobrazení oceánu" hello, nikoli jednotlivých slov. 

> [!Note]
> Vyhledávací dotaz je spustit nezávisle pro všechna prohledatelná pole v indexu Azure Search, není-li omezit hello pole s hello hello `searchFields` parametr, jak je ukázáno v požadavku hello Příklad hledání. Dokumenty, které odpovídají v některém z hello vybrané pole jsou vráceny. 

Hello dokumenty, které odpovídají na hello celou pro dotaz hello Nejistá, jsou 1, 2, 3. 

## <a name="stage-4-scoring"></a>Fáze 4: vyhodnocování  

Každému dokumentu v sadě výsledků hledání je přiřazen relevance skóre. Funkce Hello skóre relevance hello je toorank vyšší tyto dokumenty, odpovězte osvědčených uživatel otázka jako vyjádřená hello vyhledávací dotaz. výpočet skóre Hello je založen na statistické vlastnosti podmínky, které odpovídá. V hello je základní hello vyhodnocování vzorec [TF/IDF (termín frekvence inverzní dokumentu frekvenci)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). V dotazech obsahující výjimečná a běžné podmínky TF/IDF zvýší úroveň výsledky obsahující hello výjimečných podmínek. Například v hypotetický index s všechny články Wikipedia z dokumentů takový dotaz odpovídající hello *hello ředitel*, dokumentů, které vyhovují na *ředitel* jsou považovány za relevantní více než dokumenty na **.


### <a name="scoring-example"></a>Příklad vyhodnocování

Odvolat hello tři dokumenty, které odpovídá náš příklad dotazu:
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance toohello beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on hello north shore of hello island of Kauai. Ocean view."
    }
  ]
}
~~~~

Nejlépe dokumentu 1 odpovídající hello dotazu, protože obě hello termín *velké* a požadované frázi hello *oceánu zobrazení* dojít do pole Popis hello. Následující dva dokumenty Hello odpovídat jenom hello frázi *oceánu zobrazení*. Ho může být překvapení této hello relevance skóre pro dokument 2 a 3 je jiné, i když se shodoval hello dotaz hello stejným způsobem. Je to proto hello vyhodnocování vzorec obsahuje více součástí než právě TF/IDF. V takovém případě dokumentu 3 byl přiřazen mírně vyšší skóre, protože jeho popis je kratší. Další informace o [Lucene je praktické vyhodnocování vzorec](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand, jak můžete ovlivnit délka pole a dalších faktorů hello relevance skóre.

Některé typy (zástupný znak, předpony, regulární výraz) dotazů vždy přispívat konstantní skóre dokumentů toohello celkové skóre. To umožňuje shodami prostřednictvím toobe rozšíření dotazu zahrnuté ve výsledcích hello, ale bez ovlivnění hello hodnocení. 

Příklad ukazuje, proč to záleží. Vyhledávání pomocí zástupných znaků, včetně předpony hledání, jsou nejednoznačné podle definice, protože vstup hello je částečné řetězce s potenciální odpovídá na velký počet různorodých podmínky (zvažte vstup "prohlídka *", s odpovídá na "kurzy", "tourettes" nalezena, a " tourmaline"). Zadané hello povahu těchto výsledků, neexistuje žádný způsob odvození tooreasonably podmínky, které jsou vyšší hodnotu než jiné. Z tohoto důvodu jsme ignorovat termín frekvence, při vyhodnocování výsledky v dotazech typy zástupný znak, předpony a regulární výraz. V žádosti o více částech hledání obsahující částečným i úplným podmínky, jsou součástí výsledků z částečné vstup hello s konstantou, stanovení skóre tooavoid odchylka směrem potenciálně neočekávané odpovídá.

### <a name="score-tuning"></a>Skóre ladění

Existují dva způsoby tootune relevance skóre ve službě Azure Search:

1. **Vyhodnocování profily** povýšit dokumenty v hello seřazeny seznam výsledků na základě sady pravidel. V našem příkladu jsme zvažte dokumenty, které odpovídá v poli Název hello relevantnější než dokumenty, které odpovídá v poli Popis hello. Kromě toho Pokud index měli pole ceny pro každý hotelů, jsme může zvýšit úroveň dokumenty s nižší cenou. Další informace jak příliš[přidat index vyhledávání tooa profily vyhodnocování.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Zvýšení skóre termínu** (k dispozici pouze v syntaxe dotazů Lucene úplné hello) poskytuje zvýšení skóre operátor `^` , může být použita tooany části stromu hello dotazu. V našem příkladu, namísto hledání na předponě hello *air-condition*\*, jeden může vyhledat přesnou termín buď hello *air-condition* nebo hello předponu, ale dokumenty, které odpovídají na hello přesný termín, jsou seřazeny vyšší použitím nárůst toohello termín dotazu: *letecké podmínku ^ 2 || AIR-condition**. Další informace o [zvýšení skóre termínu](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Vyhodnocování v distribuované indexu

Všechny indexy ve službě Azure Search jsou automaticky rozdělí na několik horizontálních oddílů, což nám tooquickly distribuovat hello index mezi několika uzly při škálování služby registrace nebo snížit. Když je vydaný žádost o vyhledávání, jeho vydání před každou horizontálního oddílu nezávisle. Hello výsledky z každé horizontálního oddílu jsou pak sloučit a seřazené podle skóre (Pokud není definováno žádné jiné pořadí). Je důležité tooknow, který není mezi všechny horizontálních oddílů hello vyhodnocování funkce váhu dotazu termín frekvence před jeho frekvence inverzní dokumentu v všechny dokumenty v rámci hello horizontálních!

To znamená, relevance skóre *může* být různé pro identické dokumenty, pokud budou umístěny v různých horizontálních oddílů. Tyto rozdíly naštěstí zpravidla toodisappear růstem hello počet dokumentů v indexu hello kvůli toomore i termín distribuce. Není možné tooassume, na které horizontálního oddílu se umístí všechny daného dokumentu. Ale za předpokladu, že klíč dokumentu nemění, bude vždy přiřazena toohello stejné ID horizontálního oddílu.

Obecně platí skóre dokumentů není hello nejlepší atribut pro řazení dokumenty, pokud stabilitu pořadí je důležité. Například zadány dvě dokument s identické skóre, není zaručeno, která jako první se objeví v při dalším spuštění hello stejný dotaz. Dokument skóre pouze získat obecný přehled o dokument relevance relativní tooother dokumenty v sadě výsledků hello.

## <a name="conclusion"></a>Závěr

Úspěch Hello internet vyhledávací weby vyvolalo očekávání pro fulltextové vyhledávání přes osobní data. Pro téměř k libovolnému druhu vyhledávání nyní Očekáváme, že modul toounderstand hello naše záměr, i když jsou nesprávně zadaných podmínek nebo jsou neúplné. Může i Očekáváme, že shody na základě téměř ekvivalentní termíny nebo synonyma, které jsme zadali ve skutečnosti.

Z hlediska technické fulltextové vyhledávání je vysoce komplexní, vyžadují pokročilé analýzy lingvistické a systematicky tooprocessing způsoby, které generovat, rozbalte a transformace dotazu podmínky toodeliver výsledku relevantní. Zadané hello vyplývajících složité kroky, je celá řada faktorů, které můžou ovlivnit hello výsledek dotazu. Z tohoto důvodu Investujete hello čas toounderstand hello mechanismů fulltextové vyhledávání nabízí konkrétní výhody, při pokusu o toowork prostřednictvím neočekávané výsledky.  

Tento článek prozkoumali fulltextového vyhledávání v kontextu hello Azure Search. Věříme, že nabízí dostatečná pozadí toorecognize možné příčiny a řešení pro řešení běžných problémů s dotazu. 

## <a name="next-steps"></a>Další kroky

+ Sestavení indexu ukázkový text hello, vyzkoušejte různé dotazy a zkontrolovat výsledky. Pokyny najdete v tématu [sestavení a dotazování indexu portálu hello](search-get-started-portal.md#query-index).

+ Zkuste syntaxe další dotaz z hello [vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) příklad části nebo z [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) v Průzkumník služby Search na portálu hello.

+ Zkontrolujte [vyhodnocování profily](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Pokud chcete, aby tootune hodnocení ve vyhledávací aplikaci.

+ Zjistěte, jak tooapply [lexikální analyzátory jazyka](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Konfigurace vlastní analyzátorů](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) pro minimální zpracování nebo speciální zpracování na konkrétních polí.

+ [Porovnání standardní a anglické analyzátorů](http://alice.unearth.ai/))-souběžného na tento ukázkový web. 

## <a name="see-also"></a>Viz také

[Hledání dokumentů rozhraní REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[Jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[Úplná syntaxe dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[Zpracování výsledků vyhledávání](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
