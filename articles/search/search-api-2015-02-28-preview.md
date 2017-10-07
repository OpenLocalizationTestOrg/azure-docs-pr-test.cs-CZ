---
title: "aaaAzure vyhledávání REST API verze 2015-02-28-verze Preview služby | Microsoft Docs"
description: "Azure Search služby REST API verze 2015-02-28-Preview zahrnuje povolenými experimentálními funkcemi, jako je například analyzátory přirozeného jazyka a moreLikeThis hledání."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: 63cb30b7f43a42552c6744ea087afea947857224
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Rozhraní API REST služby vyhledávání systému Azure: Verze 2015-02-28-Preview
Tento článek je hello referenční dokumentaci k nástroji pro `api-version=2015-02-28-Preview`. Tato verze preview rozšiřuje hello aktuální verzi všeobecně dostupná, [rozhraní api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), tím, že poskytuje hello následující povolenými experimentálními funkcemi:

* `moreLikeThis`parametr v hello dotazu [vyhledávání dokumentů](#SearchDocs) rozhraní API. Najde jiné dokumenty, které jsou relevantní tooanother určitého dokumentu.

Několik dalších částí hello `2015-02-28-Preview` REST API popsané samostatně. Mezi ně patří:

* [Vyhodnocování profily](search-api-scoring-profiles-2015-02-28-preview.md)
* [Indexery](search-api-indexers-2015-02-28-preview.md)

Služba Azure Search je k dispozici v několika verzích. Naleznete příliš[verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti.

## <a name="apis-in-this-document"></a>Rozhraní API v tomto dokumentu
Rozhraní API služby Azure Search podporuje dva syntaxe adresy URL pro operace rozhraní API: jednoduchý a OData (viz [podporu pro OData (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798932.aspx) podrobnosti). Hello následující seznam obsahuje jednoduchý syntaktický hello.

[Vytvoření indexu](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Aktualizovat Index](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Získat Index](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Výpis indexy](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Získat statistiku indexu](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Analyzátor testu](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Odstranění indexu](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Přidat, odstranit a aktualizovat Data v indexu](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Vyhledávání dokumentů](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Vyhledávání dokumentů](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Počet dokumentů](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Návrhy](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a>Operace indexu
Můžete vytvořit a spravovat indexy ve službě Azure Search pomocí jednoduchých požadavků HTTP (POST, GET, PUT, DELETE) s daným indexem prostředků. toocreate na index, nejprve POST dokument JSON, který popisuje schéma indexu hello. Hello schéma definuje pole hello hello index, jejich datové typy a jejich použití (například v fulltextové vyhledávání, řazení a filtrů používání omezujících vlastností). Definuje také vyhodnocování profily, trochu a další atributy tooconfigure hello chování hello indexu.

Hello následující příklad uvádí ukázku schématu, používá pro vyhledávání na hotelů informace s pole s popisem hello definované ve dvou jazycích. Všimněte si, jak atributy řídit, jak se používá pole hello. Například hello `hotelId` slouží jako klíč dokumentu hello (`"key": true`) a je vyloučen z fulltextové vyhledávání (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Po vytvoření indexu hello budete odesílat dokumenty, které naplnit hello index. V tématu [přidat nebo aktualizovat dokumenty](#AddOrUpdateDocuments) pro tento další krok.

Úvodní video tooindexing ve službě Azure Search, najdete v části hello [Channel 9 cloudu zahrnují díl na Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).

<a name="CreateIndex"></a>

## <a name="create-index"></a>Vytvoření indexu
Index je primární prostředek hello uspořádání a vyhledávání dokumentů ve službě Azure Search, podobně jako toohow tabulku organizuje záznamů v databázi. Každý index obsahuje kolekci dokumentů, že všechny odpovídat schéma indexu toohello (názvy polí, datové typy a vlastnosti), ale indexy určují také další konstrukce (trochu, vyhodnocování profily a možnosti CORS), které definují jiného chování vyhledávání.

Můžete vytvořit nový index v rámci služby Azure Search pomocí požadavek HTTP POST nebo PUT. text Hello hello požadavku je schématu JSON, který určuje hello index a informace o konfiguraci.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativně můžete použít PUT a zadejte název indexu hello na hello identifikátor URI. Pokud hello indexu neexistuje, bude vytvořen.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Vytvoření indexu určuje hello struktura hello dokumenty uložené a použít v operace hledání. Naplnění indexu hello je samostatný operace. Pro tento krok, můžete použít [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (k dispozici pro podporované zdroje dat) nebo [přidání, aktualizace nebo odstranění dokumentů](https://msdn.microsoft.com/library/azure/dn798930.aspx) operaci. index Hello obrácený se vygeneruje, když jsou odeslány hello dokumenty.

**Poznámka:**: maximální počet indexů povolené hello se liší podle cenové úrovně. bezplatné služby Hello umožňuje až too3 indexy. Standardní služby umožňuje 50 indexy jednu službu vyhledávání. V tématu [limity a omezení](http://msdn.microsoft.com/library/azure/dn798934.aspx) podrobnosti.

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. Hello **Create Index** požadavek lze sestavit pomocí Metoda POST nebo PUT metody. Pokud používáte POST, je nutné zadat název indexu v textu žádosti hello společně s definice schématu indexu hello. Pomocí PUT název indexu hello je součástí adresy URL hello. Pokud hello indexu neexistuje, vytvoří se. Pokud již existuje, je aktualizovaná toohello novou definici.

Název indexu Hello musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků. Po spuštění název indexu hello s písmenem nebo číslicí, může obsahovat hello zbytek hello název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky hello.

Hello `api-version` je vyžadován. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) seznam dostupných verzí.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte příliš`application/json`
* `api-key`: Vyžaduje se. Hello `api-key` se používá ke
* ověřit službu vyhledávání tooyour hello požadavku. Je řetězcová hodnota, jedinečné tooyour služby. Hello **Create Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat i hello název služby a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

<a name="RequestData"></a>
**Syntaxe požadavku textu**

text Hello hello žádosti obsahuje definici schématu, která zahrnuje hello seznam datových polí v rámci dokumenty, kteří budou dodáni do tohoto indexu, datové typy, atributy a také volitelný seznam vyhodnocování profily, které jsou používané tooscore odpovídající dokumenty v Doba dotazu.

Všimněte si, že pro požadavek POST musíte zadat název indexu hello v textu žádosti hello.

Může existovat pouze jedno pole s klíčem v indexu hello. Má toobe pole řetězce. Toto pole představuje jedinečný identifikátor každého dokumentu uloženého v rámci hello index hello.

hlavní části Hello indexu zahrnout hello následující:

* `name`
* `fields`které budou dodáni do tohoto indexu, včetně názvu, datový typ a vlastnosti, které definují povolené akce na tomto poli.
* `suggesters`použít pro automatické dokončování nebo našeptávání dotazů.
* `scoringProfiles`používá pro určení rozsahu skóre vlastní vyhledávání. V tématu [přidejte vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) podrobnosti.
* `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` používá toodefine, jak jsou dokumenty či dotazy rozdělen do indexovanou/prohledávatelné tokeny. V tématu [Analysis ve službě Azure Search](https://aka.ms//azsanalysis) podrobnosti.
* `defaultScoringProfile`použít výchozí hello toooverwrite vyhodnocování chování.
* `corsOptions`dotazy cross-origin tooallow oproti indexu.

Syntaxe Hello strukturování datová část požadavku hello je následující. Ukázková žádost je k dispozici další na v tomto tématu.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Atributy indexu**

Hello následující atributy lze nastavit při vytváření indexu. Podrobnosti o vyhodnocování a vyhodnocování profilů najdete v tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Nastaví hello název pole hello.

`type`-Nastaví hello datový typ pro pole hello.

`searchable`-Označí pole hello fulltextového vyhledávání možné. To znamená, že ho určitým analysis například dělení slov během indexování. Pokud nastavíte `searchable` tooa hodnota pole jako "slunečného dne", interně je rozdělit na jednotlivé tokeny hello "slunečného" a "den". To umožňuje fulltextové vyhledávání pro tyto podmínky. Pole typu `Edm.String` nebo `Collection(Edm.String)` jsou `searchable` ve výchozím nastavení. Pole dalších typů nelze `searchable`.

* **Poznámka:**: `searchable` pole využívat místo navíc v indexu, protože Azure Search budou ukládat další tokenizovaná verzi hello hodnotu pole pro fulltextové vyhledávání. Pokud chcete místo toosave indexu a není nutné pole toobe, zahrnuté ve vyhledávání, nastavte `searchable` příliš`false`.

`filterable`– Umožňuje toobe hello pole odkazovaná v `$filter` dotazy. `filterable`se liší od `searchable` v tom, jak jsou zpracovávány řetězce. Pole typu `Edm.String` nebo `Collection(Edm.String)` , které jsou `filterable` nejsou prováděny dělení slov, tak porovnání jsou pro jenom přesné shody. Například pokud nastavíte toto pole `f` příliš "slunečného dne" `$filter=f eq 'sunny'` se najít žádné shody, ale `$filter=f eq 'sunny day'` bude. Všechna pole jsou `filterable` ve výchozím nastavení.

`sortable`-Ve výchozím nastavení systému hello řadí výsledky podle skóre, ale v mnoha prostředí budou uživatelé chtít toosort pole v dokumentech hello. Pole typu `Collection(Edm.String)` nemůže být `sortable`. Všechna ostatní pole jsou `sortable` ve výchozím nastavení.

`facetable`-Obvykle používaných v prezentace výsledky hledání, který obsahuje počet přístupů podle kategorie (pro příklad, vyhledejte digitální fotoaparáty a přístupů viz značku, megapixely, ceny, atd.). Tuto možnost nelze použít s pole typu `Edm.GeographyPoint`. Všechna ostatní pole jsou `facetable` ve výchozím nastavení.

* **Poznámka:**: pole typu `Edm.String` , které jsou `filterable`, `sortable`, nebo `facetable` může být maximálně 32 KB délku. Je to proto, že tato pole jsou považovány za jeden hledaný termín a hello maximální délka termín, který se ve službě Azure Search je 32KB. Pokud potřebujete další text než tento jeden řetězec pole toostore, budete potřebovat tooexplicitly nastavit `filterable`, `sortable`, a `facetable` příliš`false` v definici indexu.
* **Poznámka:**: Pokud má pole žádná z výše uvedených hello atributy nastavit příliš`true` (`searchable`, `filterable`, `sortable`, nebo`facetable`) pole hello je efektivně vyloučen z indexu hello obrácený. Tato možnost je užitečná pro pole, které nejsou používány v dotazech, ale je potřeba mít ve výsledcích hledání. Kromě těchto polí z indexu hello zvyšuje výkon.

`key`-Značky hello pole jako obsahující jedinečné identifikátory pro dokumenty v indexu hello. Právě jedno pole musí být zvolena jako hello `key` pole a musí být typu `Edm.String`. Klíčové pole můžou být použité toolook dokumentů přímo prostřednictvím hello [rozhraní API pro vyhledávání](#LookupAPI).

`retrievable`-Nastaví, zda může být hello pole vrácené ve výsledku hledání.  To je užitečné, když chcete toouse pole (například okraje) jako filtr, řazení, nebo vyhodnocování mechanismus, ale nechcete, aby hello pole toobe viditelné toohello koncového uživatele. Tento atribut musí být `true` pro `key` pole.

`analyzer`-Nastaví hello název toouse hello analyzátor pro pole hello v čas při vyhledávání a indexování čas. Hello je parametr povoleno nastaven hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost lze použít pouze s `searchable` pole a nelze nastavit společně s buď `searchAnalyzer` nebo `indexAnalyzer`.  Jakmile je zvoleno hello analyzátor, nelze změnit pro pole hello.

`searchAnalyzer`-Nastaví hello název hello analyzátor používá během hledání pro pole hello. Hello je parametr povoleno nastaven hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost lze použít pouze s `searchable` pole. Je nutné ji nastavit společně s `indexAnalyzer` a nelze ji nastavit společně s hello `analyzer` možnost. Tento analyzátor mohou být aktualizovány na stávající pole.

`indexAnalyzer`-Nastaví hello název hello analyzátor používá během indexování pro pole hello. Hello je parametr povoleno nastaven hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost lze použít pouze s `searchable` pole. Je nutné ji nastavit společně s `searchAnalyzer` a nelze ji nastavit společně s hello `analyzer` možnost. Jakmile je zvoleno hello analyzátor, nelze změnit pro pole hello.

`suggesters`-Nastaví hello režim hledání a pole, která jsou hello zdroj obsahu hello návrhy. V tématu [trochu](#Suggesters) podrobnosti.

`scoringProfiles`-Definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání. Vyhodnocování profily jsou tvořeny váhu pole a funkce. V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace o atributech hello používá v profil vyhodnocování.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Jazyková podpora**

Prohledatelná pole podstoupit analysis která nejvíc často zahrnuje dělení slov, normalizaci text a filtrování podmínky. Ve výchozím nastavení jsou prohledatelná pole ve službě Azure Search analyzovaný se hello [Apache Lucene standardní analyzátor](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) která dělí text do elementů následující["Segmentace Unicode Text"](http://unicode.org/reports/tr29/) pravidla. Kromě toho standardní analyzátor hello převede všechny znaky tootheir malá formuláře. Indexované dokumenty a hledaných termínů projít hello analysis během indexování a zpracování dotazu.

Služba Azure Search podporuje různé jazyky. Každý jazyk vyžaduje analyzer nestandardní text, který účty pro charakteristiky daného jazyka. Služba Azure Search nabízí dva typy analyzátorů:

* 35 analyzátorů zajišťoval Lucene.
* 50 analyzátorů zajišťoval proprietární přirozeného jazyka Microsoft zpracování technologie používané v kanceláři a Bing.

Někteří vývojáři preferovat hello známější, jednoduchý a open-source řešení Lucene. Lucene analyzátorů je rychlejší, ale analyzátorů Microsoft hello mít rozšířené možnosti, jako je například Lematizace, decompounding (v jazycích jako němčina, dánština, holandština, švédština, norština, estonština, dokončit, maďarština, slovenština) a entity rozpoznávání (adresy URL aplikace word e-mailů, čísla a kalendářní data). Pokud je to možné byste měli spustit porovnání obou hello společnosti Microsoft a Lucene analyzátorů toodecide které z nich je lepší přizpůsobit.

***Jejich porovnání***

Hello Lucene analyzátor pro angličtinu rozšiřuje standardní analyzátor hello. Se odebere z slova Přivlastňovací pád (koncové společnosti), platí rozklad dle [silné rozklad algoritmus](http://tartarus.org/~martin/PorterStemmer/)a také odebere Angličtina [zastavit slova](http://en.wikipedia.org/wiki/Stop_words).

Porovnání provede analyzátor Microsoft hello Lematizace místo rozklad. Znamená to, se může zpracovat tvary a nestandardní word forms mnohem lepší co má za následek více relevantní výsledky hledání (sledovat modul 7 [Azure Search MVA prezentace](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) podrobnosti).

Indexování s Microsoft analyzátorů je v průměru toothree dvakrát nižší než jejich ekvivalenty Lucene, v závislosti na jazyk hello. Pro dotazy Průměrná velikost nesmí být výrazně ovlivněn výkon vyhledávání.

***Konfigurace***

Pro každé pole v definici indexu hello můžete nastavit hello `analyzer` název vlastnosti tooan analyzátor, který určuje, které jazyk a dodavatele. Hello stejné Analyzátor se použijí při indexování a hledání toto pole.
Například můžete mít samostatné pole pro angličtinu, francouzštinu a španělština hotelů popisy, které existují vedle sebe v hello stejný index. Použití hello [parametr dotazu 'searchFields'](#SearchQueryParameters) toospecify které pole pro specifický jazyk toosearch proti v dotazech. Můžete zkontrolovat dotazu příklady, které zahrnují hello `analyzer` vlastnost [vyhledávání dokumentů](#SearchDocs). 

***Analyzátor seznamu***

Níže je seznam podporovaných jazyků společně s názvy analyzátor Lucene a Microsoft hello.

<table style="font-size:12">
    <tr>
        <th>Jazyk</th>
        <th>Název analyzátor Microsoft</th>
        <th>Název analyzátor Lucene</th>
    </tr>
    <tr>
        <td>arabština</td>
        <td>ar.microsoft</td>
        <td>ar.lucene</td>        
    </tr>
    <tr>
        <td>Arménština</td>
        <td></td>
        <td>hy.lucene</td>
      </tr>
    <tr>
        <td>Bengálském</td>
        <td>Bn.microsoft</td>
        <td></td>
    </tr>
      <tr>
        <td>Baskičtina</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
      <tr>
         <td>Bulharština</td>
        <td>BG.microsoft</td>
        <td>BG.lucene</td>
      </tr>
      <tr>
        <td>Katalánština</td>
        <td>CA.microsoft</td>
        <td>CA.lucene</td>          
      </tr>
    <tr>
        <td>Zjednodušená čínština</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>        
    </tr>
    <tr>
        <td>Tradiční čínština</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>        
    <tr>
    <tr>
        <td>Chorvatština</td>
        <td>hr.microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>čeština</td>
        <td>cs.microsoft</td>
        <td>cs.lucene</td>        
    </tr>    
    <tr>
        <td>dánština</td>
        <td>da.microsoft</td>
        <td>da.lucene</td>        
    </tr>    
    <tr>
        <td>holandština</td>
        <td>NL.microsoft</td>
        <td>NL.lucene</td>    
    </tr>    
    <tr>
        <td>Angličtina</td>        
        <td>en.microsoft</td>
        <td>en.lucene</td>        
    </tr>
    <tr>
        <td>Estonština</td>
        <td>et.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>finština</td>
        <td>Fi.microsoft</td>
        <td>Fi.lucene</td>        
    </tr>    
    <tr>
        <td>francouzština</td>
        <td>FR.microsoft</td>
        <td>FR.lucene</td>        
    </tr>
    <tr>
        <td>Galicijština</td>
        <td></td>
        <td>GL.lucene</td>        
      </tr>
    <tr>
        <td>němčina</td>
        <td>de.microsoft</td>
        <td>de.lucene</td>        
    </tr>
    <tr>
        <td>řečtina</td>
        <td>El.microsoft</td>
        <td>El.lucene</td>        
    </tr>
    <tr>
        <td>Gudžarátština</td>
        <td>gu.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebrejština</td>
        <td>he.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindština</td>
        <td>Hi.microsoft</td>
        <td>Hi.lucene</td>        
    </tr>
    <tr>
        <td>maďarština</td>        
        <td>HU.microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>Islandština</td>
        <td>is.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonéština (Bahasa)</td>
        <td>ID.microsoft</td>
        <td>ID.lucene</td>        
    </tr>
    <tr>
        <td>Irština</td>
        <td></td>
          <td>GA.lucene</td>
    </tr>
    <tr>
        <td>italština</td>
        <td>IT.microsoft</td>
        <td>IT.lucene</td>        
    </tr>
    <tr>
        <td>japonština</td>
        <td>ja.microsoft</td>
        <td>ja.lucene</td>

    </tr>
    <tr>
        <td>Kannadština</td>
        <td>Ka.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>korejština</td>
        <td>Ko.microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lotyština</td>        
        <td>LV.microsoft</td>
        <td>LV.lucene</td>    
    </tr>
    <tr>
        <td>Litevština</td>
        <td>lt.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalámština</td>
        <td>ml.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajština (Latina)</td>
        <td>MS.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maráthština</td>
        <td>MR.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>norština</td>
        <td>NB.microsoft</td>
        <td>No.lucene</td>        
    </tr>
      <tr>
        <td>Perština</td>
        <td></td>
        <td>fa.lucene</td>        
      </tr>
    <tr>
        <td>polština</td>
        <td>PL.microsoft</td>
        <td>PL.lucene</td>        
    </tr>
    <tr>
        <td>Portugalština (Brazílie)</td>
        <td>PT-Br.microsoft</td>
        <td>PT-Br.lucene</td>        
    </tr>
    <tr>
        <td>Portugalština (Portugalsko)</td>
        <td>PT-Pt.microsoft</td>        
        <td>PT-Pt.lucene</td>
    </tr>
    <tr>
        <td>Paňdžábština</td>
        <td>Pa.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>rumunština</td>
        <td>ro.microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>ruština</td>
        <td>RU.microsoft</td>
        <td>RU.lucene</td>    
    </tr>
    <tr>
        <td>Srbština (cyrilice)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Srbština (Latina)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovenština</td>
        <td>Sk.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovinština</td>
        <td>SL.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>španělština</td>
        <td>ES.microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>švédština</td>
        <td>Sv.microsoft</td>
        <td>Sv.lucene</td>
    </tr>

    <tr>
        <td>Tamilština</td>
        <td>ta.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugština</td>
        <td>te.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thajština</td>
        <td>TH.microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>turečtina</td>
        <td>TR.microsoft</td>
        <td>TR.lucene</td>        
    </tr>
    <tr>
        <td>Ukrajinština</td>
        <td>UK.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdština</td>
        <td>Your.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamština</td>
        <td>VI.microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Kromě toho poskytuje Azure Search bez ohledu na jazyk analyzátor konfigurace</td>
    <tr>
        <td>Skládání standardní ASCII</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Segmentace text Unicode (standardní Tokenizátor)</li>
            <li>Skládání filtru ASCII – převede znaky znakové sady Unicode, které nepatří do jejich ekvivalenty ASCII toohello sadu nejprve 127 znaků ASCII. To je užitečné pro odebrání znaky s diakritikou.</li>
        </ul>
        </td>
    </tr>
</table>

Všechny analyzátory s názvy opatřen poznámkou <i>lucene</i> se používá technologii [analyzátory jazyka Apache Lucene](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Další informace o hello ASCII skládání filtru najdete [zde](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Moduly pro návrhy**

A `suggester` definuje, která pole v indexu jsou použité toosupport automatického dokončování v hledání. Obvykle jsou částečné vyhledávání řetězců odeslána toohello [návrhy API](#Suggestions) při hello uživatele je zadáním vyhledávací dotaz a hello rozhraní API vrací sadu navrhované frází. Modulu pro návrhy, kterou definujete v indexu hello určuje pole, která jsou použité toobuild hello našeptávání hledaný text. V tématu [trochu](#Suggesters) podrobnosti o konfiguraci.

**Profily skórování**

A `scoringProfile` definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání hello. Vyhodnocování profily jsou tvořeny váhu pole a funkce. toouse je zadat profil v řetězci dotazu hello podle názvu.

Výchozí profil vyhodnocování funguje za hello scény toocompute skóre vyhledávání pro každou položku v sadě výsledků. Můžete použít hello interní, nepojmenované profil vyhodnocování. Alternativně nastavte `defaultScoringProfile` toouse vlastní profil jako výchozí hello, vyvolá se vždy, když není zadaný vlastního profilu v řetězci dotazu hello.

V tématu [přidat vyhodnocování profily indexu vyhledávání tooa (služby REST API Azure Search)](search-api-scoring-profiles-2015-02-28-preview.md) podrobnosti.

**Možnosti CORS**

Javascript na straně klienta nelze volat všechny rozhraní API ve výchozím nastavení, protože prohlížeč hello zabrání všech žádostí napříč zdroji. Povolte CORS (sdílení prostředků různého původu) tak, že nastavení hello `corsOptions` atribut tooallow cross-origin dotazy tooyour index. Všimněte si, že pouze dotaz rozhraní API pro podporu CORS z bezpečnostních důvodů. pro CORS lze nastavit Hello následující možnosti:

* `allowedOrigins`(povinné): Toto je seznam zdroje, kterým se udělí přístup tooyour index. To znamená, že žádný kód Javascript z těchto zdroje bude povoleno tooquery indexu (za předpokladu, že poskytuje správný klíč API hello). Každý původ je obvykle ve formátu hello `protocol://fully-qualified-domain-name:port` i když hello port je často vynechán. V tématu [v tomto článku](http://go.microsoft.com/fwlink/?LinkId=330822) další podrobnosti.
  * Pokud chcete zdroje tooall přístup tooallow, zahrnout `*` jako jedna položka v hello `allowedOrigins` pole. Všimněte si, že **to není doporučeno, postup pro produkční vyhledávací služby.** Však může být užitečné pro vývoj nebo účely ladění.
* `maxAgeInSeconds`(volitelné): v prohlížečích se používá tato hodnota toodetermine hello doba trvání (v sekundách) toocache CORS předběžných odpovědí. Toto musí být nezáporné celé číslo. Hello větší má hodnotu, bude hello lepší výkon, ale hello déle bude taky trvat vliv tootake změny zásad CORS. Pokud není nastavena, použije se výchozí hodnota 5 minut.

<a name="CreateUpdateIndexExample"></a>
**Příklad text žádosti**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Odpověď**

Pro úspěšné žádosti: "201 – vytvořeno".

Ve výchozím nastavení bude obsahovat text odpovědi hello hello JSON pro definici indexu hello, který byl vytvořen. Pokud hello `Prefer` hlavička požadavku je nastaven příliš`return=minimal`, bude text odpovědi hello prázdný a hello úspěch stavový kód bude "204 žádný obsah" místo "201 – vytvořeno". To platí bez ohledu na to, jestli byl PUT nebo POST použité toocreate hello index.

**Poznámky**

V současné době je omezená podpora aktualizace schématu indexu. Všechny aktualizace schémat, které by vyžadovaly přeindexování například změny typu polí nejsou aktuálně podporovány. Ale existující pole nemůžete změnit ani odstranit, můžete kdykoli přidat nová pole existující index tooan kdykoli. Při přidání nové pole mít všechny stávající dokumenty v indexu hello automaticky pro toto pole hodnotu null. Žádné další úložný prostor využijí dokud nové dokumenty jsou přidána toohello index.

<a name="Suggesters"></a>

## <a name="suggesters"></a>Moduly pro návrhy
Funkce návrhy Hello ve službě Azure Search je funkce našeptávání nebo automatické dokončování dotazů, které poskytuje seznam potenciální hledaný text v odpovědi toopartial řetězec vstupy do vyhledávacího pole. Pravděpodobně jste zaznamenali dotazů při používání komerční vyhledávací stroje: zadáním ".NET" v Bing vytvoří seznam podmínek pro ".NET 4.5", "rozhraní .NET Framework 3,5", a tak dále. Pokud používáte službu vyhledávání hello REST API, implementace návrhy ve vlastní aplikaci Azure Search vyžaduje hello následující:

* Povolit návrhy přidáním **modulu pro návrhy** konstrukce v indexu, udělíte hello název, režim hledání a seznam polí, pro kterou našeptávání je volána. Například pokud zadáte "mesto" jako pole zdroje, zadáte řetězec částečné vyhledávání "Sea" bude mít za následek "Seattle", "Pobřežního" a "Seatac" (všechny tři, jsou názvy skutečné města) nabízí jako uživatel toohello návrhy dotazu.
* Vyvolat návrhy, volání hello [API návrhy](#Suggestions) v kódu aplikace. Částečné řetězce se obvykle odesílají toohello služby při hello uživatele je zadáním vyhledávací dotaz a toto rozhraní API vrací sadu navrhované frází.

Tento článek vysvětluje, jak tooconfigure **modulu pro návrhy**. Také byste měli revidovat hello [návrhy API](#Suggestions) podrobnosti o tom, jak se používá modulu pro návrhy.

**Využití**

`Suggesters`jsou vytvořené v indexu hello a fungují lépe, když používá toosuggest konkrétní dokumenty místo přijít podmínky či fráze. Hello nejlepší candidate pole jsou produktů, názvů a jiné poměrně krátké věty, které můžete identifikovat položku. Méně účinné jsou opakovaných pole, kategorie a značky, nebo velmi dlouhé pole jako pole popisy nebo komentáře.

Jako součást definice indexu hello, můžete přidat jeden modulu pro návrhy toohello `suggesters` kolekce. Vlastnosti, které definují modulu pro návrhy zahrnout hello následující:

* `name`: hello název modulu pro návrhy hello. Při volání metody hello použijete hello název modulu pro návrhy hello `suggest` rozhraní API.
* `searchMode`: hello toosearch strategie použitá pro kandidátských frází. je technologie Hello jediný momentálně podporovaný režim `analyzingInfixMatching`, který provádí flexibilní porovnávání frází na začátku hello nebo uprostřed vět hello.
* `sourceFields`: Seznam jeden nebo více polí, které jsou hello zdroj obsahu hello návrhy. Pouze pole typu `Edm.String` a `Collection(Edm.String)` může být zdrojů pro návrhy. Lze použít pouze pole, které nemají vlastní analyzátor jazyka nastavit.

**Příklad modulu pro návrhy**

Modulu pro návrhy je součástí hello index. V hello může existovat pouze jedna modulu pro návrhy `suggesters` kolekce v aktuální verzi hello vedle hello polí kolekce a `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> Pokud jste použili hello verze verzi public preview služby Azure Search, `suggesters` nahrazuje starší logická vlastnost (`"suggestions": false`), předpona návrhy podporována pouze pro krátké řetězce (3-25 znaků). Čím jsou nahrazené, `suggesters`, podporuje infix odpovídající, který vyhledá odpovídající podmínky od hello začátku nebo uprostřed hello pole obsahu, lepší toleranci chyb v řetězci pro vyhledávání. Od verze hello všeobecně dostupná, to je nyní hello pouze implementace hello návrhů rozhraní API. Hello starší `suggestions` vlastnost, která byla zavedena v `api-version=2014-07-31-Preview` toowork pokračuje v této verzi, ale není v provozu v hello `2015-02-28` nebo novější verze Azure Search.
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a>Aktualizovat Index
Můžete aktualizovat existující index v rámci služby Azure Search pomocí požadavek HTTP PUT. Aktualizace můžou zahrnovat přidáte nové pole toohello existující schéma, úprava možnosti CORS a úprava profily vyhodnocování. V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace. Zadáte název hello hello index tooupdate v identifikátoru URI žádosti hello:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Důležité:** podpora pro aktualizace schématu indexu je omezená toooperations, který není zapotřebí nové sestavení indexu vyhledávání hello. Všechny aktualizace schémat, které by vyžadovaly přeindexování, například změny typu polí nejsou aktuálně podporovány. Kdykoli můžete přidat nová pole, ale existující pole nemůžete změnit ani odstranit. Hello totéž platí i příliš`suggesters`. Může kdykoli přidat nová pole, jsou přidány tooa modulu pro návrhy na hello čas hello pole, ale pole nelze odebrat z `suggesters` a příliš nelze přidat stávající pole`suggesters`.

Při přidávání nového indexu pole tooan, všechny stávající dokumenty v indexu hello bude mít automaticky pro toto pole hodnotu null. Žádné další úložný prostor využijí dokud nové dokumenty jsou přidána toohello index.

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. Hello **aktualizace indexu** požadavku je vytvořený pomocí HTTP PUT. Pomocí PUT název indexu hello je součástí adresy URL hello. Pokud hello indexu neexistuje, vytvoří se. Pokud už existuje hello index, je aktualizovaná toohello novou definici.

Název indexu Hello musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků. Po spuštění název indexu hello s písmenem nebo číslicí, může obsahovat hello zbytek hello název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky hello.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte příliš`application/json`
* `api-key`: Vyžaduje se. Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **aktualizace indexu** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Syntaxe požadavku textu**

Při aktualizaci existujícího indexu, textu hello musí zahrnovat hello původní definici schématu, plus hello nová pole, který chcete přidat, jakož i hello upravit vyhodnocování profily, trochu a možnosti CORS, pokud existuje. Pokud nejsou změny hello vyhodnocování profily a možnosti CORS, je nutné zahrnout hello původních z vytvoření indexu hello. Obecně je hello nejlepší toouse vzor pro aktualizace definice indexu hello tooretrieve s GET, upravte ho a pak jej aktualizovat s PUT.

Syntaxe schématu Hello používá toocreate, který je zde uveden index pro práci v aplikaci. V tématu [Create Index](#CreateIndex) další podrobnosti.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Odpověď**

Pro úspěšné žádosti: "204 žádný obsah".

Ve výchozím nastavení bude text odpovědi hello prázdný. Nicméně, pokud hello `Prefer` hlavička požadavku je nastaven příliš`return=representation`, hello odpovědi bude obsahovat hello JSON pro hello definici indexu, která byla aktualizována. V takovém případě bude kód stavu úspěch hello "200 OK".

**Aktualizace definice indexu s vlastní analyzátory**

Jakmile je definovány analyzátor, tokenizátor, token filtru nebo filtr char, nemůže být upraven. Nové lze přidat existující index tooan pouze v případě hello `allowIndexDowntime` příznak nastaven tootrue v žádosti o aktualizaci indexu hello: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Všimněte si, že bude tuto operaci put indexu v režimu offline pro alespoň několik sekund vaší indexování a dotaz požadavků toofail. Výkon a zápisu dostupnost hello index může být zasažená po dobu několika minut po aktualizaci hello indexu nebo delší dobu velmi velký indexy.

<a name="ListIndexes"></a>

## <a name="list-indexes"></a>Seznam indexů
Hello **seznamu indexy** operace vrátí seznam hodnot hello indexy aktuálně ve službě Azure Search.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. Hello **seznamu indexy** požadavek se dá vytvořit pomocí metody GET hello.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: Vyžaduje se. Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **seznamu indexy** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Tady je odpovědi na příkladu:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Všimněte si, že můžete filtrovat hello odpovědi dolů toojust hello vlastnosti, které vás zajímají. Například pokud chcete pouze seznam názvů index, použijte hello OData `$select` dotazu možnost:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

V takovém případě hello odpověď z hello výše příklad by měly vypadat následovně:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Toto je užitečné toosave šířky pásma, pokud máte spoustu indexy ve vyhledávací službě.

<a name="GetIndex"></a>

## <a name="get-index"></a>Získat Index
Hello **získat Index** operaci získá definici indexu hello z Azure Search.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. Hello **získat Index** požadavek se dá vytvořit pomocí metody GET hello.

v identifikátoru URI žádosti hello Hello [název indexu] Určuje, které tooreturn indexu z kolekce indexů hello.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **získat Index** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Viz příklad hello JSON v [vytvářet a aktualizovat Index](#CreateUpdateIndexExample) příklad datové části odpovědi hello.

<a name="DeleteIndex"></a>

## <a name="delete-index"></a>Odstranit Index
Hello **odstranit Index** operace odebere index a související dokumenty ze služby Azure Search. Název indexu hello můžete získat z řídicího panelu služby hello ve hello portál Azure nebo z hello rozhraní API. V tématu [seznamu indexy](#ListIndexes) podrobnosti.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. Hello **odstranit Index** požadavek se dá vytvořit pomocí hello metodu DELETE.

v identifikátoru URI žádosti hello Hello [název indexu] Určuje, které toodelete indexu z kolekce indexů hello.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: Vyžaduje se. Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour. Hello **odstranit Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a>Získat statistiku indexu
Hello **získat statistiku Index** operace z Azure Search vrátí počet dokumentů pro aktuální index hello plus využití úložiště.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Statistika na dokument počet a velikost úložiště se shromažďují každých několik minut, ne v reálném čase. Statistiky hello vrácený toto rozhraní API proto nemusí odrážet změny způsobené poslední operace indexování.
> 
> 

**Požadavek**

Je požadován pro všechny požadavky služby protokol HTTPS. Hello **získat statistiku Index** požadavek se dá vytvořit pomocí metody GET hello.

Hello [název indexu] v identifikátoru URI žádosti hello informuje hello služby tooreturn index statistiku pro hello zadaným indexem.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **získat statistiku Index** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

text odpovědi Hello je ve formátu hello:

    {
      "documentCount": number,
      "storageSize": number (size of hello index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a>Analyzátor testu
Hello **analyzovat rozhraní API** ukazuje, jak analyzátor dělí text do tokenů.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Požadavek**

Je požadován pro všechny požadavky služby protokol HTTPS. Hello **analyzovat rozhraní API** požadavek se dá vytvořit pomocí metody POST hello.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **analyzovat rozhraní API** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello název indexu a hello název tooconstruct hello žádost o adresu URL služby. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

    {
      "text": "Text tooanalyze",
      "analyzer": "analyzer_name"
    }

nebo

    {
      "text": "Text tooanalyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

Hello `analyzer_name`, `tokenizer_name`, `token_filter_name` a `char_filter_name` nutnost toobe platné názvy předdefinované nebo vlastní analyzátorů, tokenizers, tokenu filtry a filtry znakový hello index. informace o procesu hello lexikální analýzy najdete toolearn [Analysis ve službě Azure Search](https://aka.ms/azsanalysis).

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

text odpovědi Hello je ve formátu hello:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of hello first character of hello token),
          "endOffset": number (index of hello last character of hello token),
          "position": number (position of hello token in hello input text)
        },
        ...
      ]
    }

**Analýza příklad rozhraní API**

**Požadavek**

    {
      "text": "Text tooanalyze",
      "analyzer": "standard"
    }

**Odpověď**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a>Operace dokumentu
Ve službě Azure Search index je uložená v cloudu hello a vyplněny dokumentů JSON, můžete nahrávat na server služby toohello. Všechny hello dokumenty, které můžete nahrávat na server tvoří hello souhrnu dat hledání. Dokumenty obsahují pole, z nichž některé jsou tokenizovaného do hledaných termínů jako jejich uložení. Hello `/docs` segment adresy URL v hello rozhraní API služby Azure Search představuje kolekci hello dokumentů v indexu. Všechny operace provést na kolekci hello, například odeslání, sloučení, odstranění nebo dotazování dokumentů proběhla v kontextu hello jeden index, takže hello adresy URL pro tyto operace bude vždy začínat `/indexes/[index name]/docs` pro název daného indexu.

Kód aplikace musíte vygenerovat buď tooAzure tooupload dokumenty JSON vyhledávání nebo můžete použít [indexer](https://msdn.microsoft.com/library/dn946891.aspx) tooload dokumentů. Pokud je zdroj dat hello Azure SQL Database nebo Azure Cosmos DB. Obvykle indexy budou naplněny z jedné datové sady, které zadáte.

Měli byste mít jeden dokument pro každou položku, které chcete toosearch. Film pronájem aplikace může mít jeden dokument na film, storefront aplikace může mít jeden dokument za SKU, online výukových kurzů aplikace může mít jeden dokument za kurzu, firma research může mít jeden dokument pro každý academic dokumentu v jejich úložiště a tak dále.

Dokumenty obsahovat jeden či více polí. Pole může obsahovat text, který je tokenizovaného do podmínek vyhledávání ve službě Azure Search, jakož i bez tokenizovaného nebo jiné než textové hodnoty, které mohou být používány filtry, nebo profily vyhodnocování. Hello názvy, datové typy a funkce vyhledávání, které jsou podporovány pro každé pole určuje schéma indexu hello. Jedno z polí hello ve schématu každý index musí být určeny jako ID a každý dokument musí mít hodnotu pro hello ID pole, která jednoznačně identifikuje tento dokument v indexu hello. Všechna ostatní pole dokumentu jsou volitelné a bude použita výchozí hodnota null tooa Pokud nezadaný. Všimněte si, že hodnoty null nemusí provádět žádné místo v indexu vyhledávání hello.

Předtím, než můžete odesílat dokumenty, musí již jste vytvořili hello index hello služby. V tématu [Create Index](#CreateIndex) podrobnosti o tento první krok.

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a>Přidání, aktualizace nebo odstranění dokumentů
Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST. Pro velké množství aktualizací se doporučuje dávkování dokumentů (too1000 dokumenty na jednu dávku) nebo o 16 MB na jednu dávku.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST.

[název indexu] obsahuje identifikátor URI požadavku Hello zadání které dokumenty toopost index. Najednou můžete účtovat pouze index tooone dokumenty.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte příliš`application/json`
* `api-key`: Vyžaduje se. Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcová hodnota, jedinečné tooyour služby. Hello **přidat dokumenty** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Hello textu hello požadavku obsahuje jeden nebo více dokumentů toobe indexované. Dokumenty jsou identifikovány jedinečný klíč. Každý dokument je přidružené akci: odeslání, sloučení, mergeOrUpload nebo odstranit. Odeslání požadavků musí obsahovat data dokumentu hello jako sada párů klíč/hodnota.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> Dokument klíče může obsahovat pouze písmena, číslice, pomlčky ("-"), podtržítka ("_") a symboly rovná se ("="). Další podrobnosti najdete v tématu [pravidla po pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx).
> 
> 

**Akce dokumentu**

* `upload`: Nahrávání akci je podobný tooan "upsert", kde hello dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje. Všimněte si, že všechna pole jsou nahrazena v případě aktualizace hello.
* `merge`: Aktualizace sloučení existující dokumentů s hello zadaná pole. Pokud hello dokument neexistuje, sloučení hello selže. Každé pole zadané ve sloučení nahradí stávající pole hello v dokumentu hello. To zahrnuje i pole typu `Collection(Edm.String)`. Například pokud hello dokument obsahuje pole "značky" s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro "značky" hello konečná hodnota pole značky"hello" bude `["economy", "pool"]`. Zruší **není** být `["budget", "economy", "pool"]`.
* `mergeOrUpload`: chová jako `merge` Pokud dokument s hello zadaný klíč již existuje v indexu hello. Pokud hello dokument neexistuje, chová se jako `upload` s novým dokumentem.
* `delete`: Odstranění odebere hello indexu zadaný dokument hello. Všimněte si, že všechna zadaná pole v `delete` operaci než hello pole klíče budou ignorovány. Pokud chcete tooremove z dokumentu jednotlivá pole, použijte `merge` místo a jednoduše nastavte hello pole explicitně příliš`null`.

**Odpověď**

Pro úspěšné odpovědi, což znamená, že všechny položky byly úspěšně indexované je vrátit stavovým kódem 200 (OK). To vyplývá z hello `status` vlastnost Probíhá nastavení tootrue pro všechny položky, jakož i hello `statusCode` vlastnost Probíhá nastavení tooeither 201 (pro nově nahraném dokumenty) nebo 200 (pro sloučené nebo odstraněné dokumenty):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Stavový kód 207 (více Status) je vrácena, pokud nebyla alespoň jedna položka úspěšně indexovaná. Položky, které nebyly indexovány mají hello `status` pole nastaveno toofalse. Hello `errorMessage` a `statusCode` vlastnosti označí hello důvod hello indexování Chyba:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "hello search service is too busy tooprocess this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Hello následující tabulka vysvětluje různé každý dokument stavové kódy, které mohou být vráceny v odpovědi hello hello. Všimněte si, že některé naznačují potíže s hello požádat samostatně, zatímco ostatní označují dočasné chybový stav. Hello pozdější opakujte po prodlevě.

<table style="font-size:12">
    <tr>
        <th>Stavový kód</th>
        <th>Význam</th>
        <th>Opakovatelná</th>
        <th>Poznámky</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument byl úspěšně upravit nebo odstranit.</td>
        <td>neuvedeno</td>
        <td>Operace odstranění se <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. To znamená i v případě, že klíč dokumentu neexistuje v indexu hello, pokusu o operaci odstranění s tímto klíčem bude mít za následek 200 stavový kód.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument byl úspěšně vytvořen.</td>
        <td>neuvedeno</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Došlo k chybě v hello dokumentu, která zabránila z indexování.</td>
        <td>Ne</td>
        <td>Hello chybovou zprávu ve hello odpovědi bude určení toho, co se hello dokumentu.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Hello dokument nelze sloučit, protože neexistuje hello zadaný klíč v indexu hello.</td>
        <td>Ne</td>
        <td>Této chybě nedochází pro nahrávání vzhledem k tomu, že se vytváření nových dokumentů a nespustí se pro odstranění vzhledem k tomu, že jsou <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Při pokusu o tooindex dokument byl zjištěn konflikt verzí.</td>
        <td>Ano</td>
        <td>Tomu může dojít, když se pokoušíte tooindex hello stejné dokumentu více než jednou současně.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Hello index je dočasně nedostupný, protože byla aktualizována s hello allowIndexDowntime příznak sadu too'true'.</td>
        <td>Ano</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Vyhledávací služba je dočasně nedostupná, pravděpodobně z důvodu tooheavy zatížení.</td>
        <td>Ano</td>
        <td>Váš kód čekat, než v tomto případě nebo riziko prodloužit nedostupnost služby hello.</td>
    </tr>
</table> 

**Poznámka:**: Pokud váš klientský kód často zaznamená 207 odpověď, jedním z možných důvodů je, že systém hello je zatížení. Můžete to ověřit kontrolou hello `statusCode` vlastnost 503. Pokud se jedná o hello případ, doporučujeme ***omezení indexování požadavky***. Jinak hodnota pokud není subside indexování provoz, hello systém může spustit odmítat všechny požadavky s 503 chyby.

Kód stavu 429 označuje, že jste překročili kvótu hello počet dokumentů na index. Musíte vytvořit nový index nebo upgrade na vyšší limity kapacity.

**Příklad:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a>Vyhledávání dokumentů
A **vyhledávání** operace se objeví jako požadavek GET nebo POST a určuje parametry, které poskytují hello kritéria pro výběr odpovídající dokumenty.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Když toouse POST místo GET**

Při použití metody GET protokolu HTTP hello toocall **vyhledávání** rozhraní API, musíte vědět, že nesmí překročit délka hello adresy URL žádosti hello 8 KB toobe. Je to obvykle dostatečně pro většinu aplikací. Některé aplikace však vytvořit velmi velké dotazy nebo výrazy filtru OData. Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry a dotazy, než metoda GET. S POST, počet hello termíny nebo klauzule v dotazu je hello omezení faktoru, není hello velikost hello nezpracovaná dotazu vzhledem k tomu, že omezení velikosti hello požadavku pro metodu POST je přibližně 16 MB.

> [!NOTE]
> I když limit velikosti požadavek POST hello je moc velká, nemůže být libovolně komplexní vyhledávací dotazy a výrazy filtru. V tématu [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) a [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost vyhledávání dotazu a filtr.
> 
> 

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. Hello **vyhledávání** požadavek se dá vytvořit pomocí hello GET nebo POST metody.

identifikátor URI požadavku Hello Určuje, které tooquery indexu, pro všechny dokumenty, které odpovídají hello parametry. Parametry jsou zadány na hello řetězec dotazu v případě hello požadavků GET a v žádosti o hello textu v případě hello POST požadavky.

Jako osvědčený postup při vytváření požadavky GET, mějte na paměti, příliš[kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifického dotazu parametry při volání metody hello REST API přímo. Pro **vyhledávání** operace, to zahrnuje:

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

Kódování URL se doporučuje jenom na hello výše parametry dotazu. Pokud jste omylem kódování URL hello řetězec dotazu celý, (vše za hello?), dojde k přerušení požadavky.

Navíc kódování URL je nutné pouze při volání metody hello získat přímo pomocí rozhraní REST API. Žádné kódování URL je nezbytné při volání metody **vyhledávání** pomocí POST, nebo při použití hello [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), která zpracovává kódování URL za vás.

<a name="SearchQueryParameters"></a>
**Parametry dotazu**

**Hledání** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání. Zadejte tyto parametry v adrese URL hello řetězec dotazu při volání metody **vyhledávání** prostřednictvím GET a jako vlastnosti JSON v textu žádosti hello při volání metody **vyhledávání** přes POST. Hello syntaxe pro některé parametry se mírně liší mezi GET a POST. Tyto rozdíly jsou popsány podle vhodnosti níže:

`search=[string]`(volitelné) – hello toosearch text pro. Všechny `searchable` prohledány jsou ve výchozím nastavení není-li `searchFields` je zadán. Při hledání `searchable` tokenizovaného polí a hledaný text hello, sám sebe, tak více podmínek je možné oddělit prázdné znaky (například: `search=hello world`). použít všechny termín toomatch `*` (může to být užitečné pro dotazy Booleovský filtr). Tento parametr vynechá má stejné jako jeho nastavení příliš ovlivňuje hello`*`. V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) pro konkrétní na syntaxe vyhledávání hello.

* **Poznámka:**: hello výsledky mohou být někdy překvapivé při dotazování přes `searchable` pole. tokenizátor Hello obsahuje logiku toohandle případech běžné tooEnglish text jako apostrofy, čárky v čísel atd. Například `search=123,456` bude shodovat s jeden termín 123,456 místo jednotlivých podmínky hello 123 a 456, protože čárkami jsou použity jako oddělovače tisíc pro velké počty v angličtině. Z tohoto důvodu doporučujeme používat mezer spíše než interpunkce tooseparate podmínky v hello `search` parametr.

`searchMode=any|all`(volitelné, použije se výchozí hodnota příliš`any`) – ať některého nebo všech hello hledaných termínů musí odpovídat pořadí toocount hello dokumentu jako shoda.

`searchFields=[string]`(volitelné) – seznam hello toosearch názvy polí oddělených čárkou pro hello zadaný text. Cílová pole. musí být označen jako `searchable`.

`queryType=simple|full`(volitelné, použije se výchozí hodnota příliš`simple`) – Pokud je sada příliš "jednoduchý" hledaný text interpretovat pomocí jednoduchého dotazovací jazyk, který umožňuje symbolů, jako +, * a "". Dotazy jsou vyhodnocovány napříč všechna prohledatelná pole (nebo pole uvedené v `searchFields`) v každém dokumentu ve výchozím nastavení. Pokud je typ dotazu hello nastavená příliš`full` hledaný text interpretována pomocí jazyka dotazů Lucene hello, což umožňuje specifické pole a vyvážené hledání. V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) a [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) pro konkrétní na syntaxe vyhledávání hello. 

> [!NOTE]
> Rozsah vyhledávání v hello Lucene dotazovací jazyk nepodporuje považuje $filter který nabízí podobné funkce.
> 
> 

`moreLikeThis=[key]`(volitelné) **Důležité:** tato funkce je k dispozici v `2015-02-28-Preview`. Tuto možnost nelze použít v dotazu, který obsahuje parametr hledání textu hello `search=[string]`. Hello `moreLikeThis` parametr Vyhledá dokumenty, které jsou podobné toohello dokumentu určeného klíč dokumentu hello. Při hledání požadavku s `moreLikeThis`, vygeneruje se seznam podmínek vyhledávání, na základě frekvence hello a vzácnost podmínkami hello zdrojový dokument. Tyto podmínky jsou pak použít toomake hello požadavku. Ve výchozím nastavení, hello obsah všech `searchable` pole jsou považovány za Pokud `searchFields` je použité toorestrict pole, která vyhledávají.  

`$skip=#`(volitelné) – hello počet vyhledávání výsledků tooskip; Nemůže být vyšší než 100 000. Pokud potřebujete tooscan dokumentů v pořadí, ale nemůžete použít `$skip` z důvodu omezení toothis, zvažte použití `$orderby` na klíč zcela seřazené a `$filter` s rozsahem dotazu místo.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `skip` místo `$skip`.
> 
> 

`$top=#`(volitelné) – hello počet vyhledávání výsledků tooretrieve. To lze použít ve spojení s `$skip` tooimplement klienta stránkování výsledků vyhledávání.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `top` místo `$top`.
> 
> 

`$count=true|false`(volitelné, použije se výchozí hodnota příliš`false`)-určuje, zda toofetch hello celkový počet výsledků. Toto je počet hello všechny dokumenty, které odpovídají hello `search` a `$filter` parametry, ignoruje `$top` a `$skip`. Nastavení této hodnoty příliš`true` může mít dopad na výkon. Všimněte si, že vrácený počet hello je sblížení.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `count` místo `$count`.
> 
> 

`$orderby=[string]`(volitelné) – seznam oddělený čárkami výrazy toosort hello výsledků podle. Každý výraz může být název pole nebo volání toohello `geo.distance()` funkce. Každý výraz může následovat `asc` tooindicated vzestupné řazení a `desc` tooindicate sestupně. Výchozí Hello je vzestupné pořadí. TIES bude rozděleno podle hello shodu skóre dokumentů. Pokud žádné `$orderby` je zadána výchozí pořadí hello sestupné podle skóre shodu dokumentu. Existuje limit 32 klauzule pro `$orderby`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.
> 
> 

`$select=[string]`(volitelné) – seznam tooretrieve polí oddělených čárkami. Pokud tento parametr zadán, jsou zahrnuty všechna pole označeno získat ve schématu hello. Můžete také explicitní žádost o všechna pole nastavením tento parametr příliš`*`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `select` místo `$select`.
> 
> 

`facet=[string]`(nula nebo více) – pole toofacet podle. Řetězec hello může volitelně obsahovat parametry toocustomize hello používání faset vyjádřený jako textový soubor s oddělovači `name:value` páry. Jsou platné parametry:

* `count`(maximální počet omezující vlastnosti podmínky; výchozí hodnota je 10). Neexistuje žádné maximální, ale vyšší hodnoty zpoplatněná odpovídající snížení výkonu, zejména pokud Fasetové pole hello obsahuje velký počet jedinečných podmínky.
  * Příklad: `facet=category,count:5` získá hello nejvyšší pěti kategorií ve výsledcích omezující vlastnosti.  
  * **Poznámka:**: Pokud hello `count` parametru je menší než počet hello jedinečný podmínky, nemusí být přesné výsledky hello. Toto je z důvodu toohello způsob používání omezujících vlastností dotazy jsou rozmístěny v horizontálních oddílů. Zvýšení `count` obvykle zvyšuje hello přesnost počtů hello termín, ale v snížený výkon.
* `sort`(mezi `count` toosort *sestupném* podle počtu, `-count` toosort *vzestupném* podle počtu, `value` toosort *vzestupném* podle hodnoty, nebo `-value` toosort *sestupném* podle hodnoty)
  * Příklad: `facet=category,count:3,sort:count` získá hello nejvyšší tří kategorií ve výsledcích omezující vlastnosti v sestupném pořadí podle počtu hello dokumentů s každý název města. Například pokud hello nejvyšší tří kategorií jsou rozpočtu, Motel a možnost a nároky má 5 přístupů, Motel má 6 a možnost má 4, pak hello kbelíků bude v pořadí hello Motel, rozpočtu, možnost.
  * Příklad: `facet=rating,sort:-value` vytváří kbelíků pro všechny možné hodnocení v sestupném pořadí podle hodnoty. Například pokud hello hodnocení ze 1 too5, kbelíků hello bude objednáno 5, 4, 3, 2, 1, bez ohledu na to, kolik dokumenty odpovídají každé hodnocení.
* `values`(oddělený kanálu číselný nebo `Edm.DateTimeOffset` hodnoty určující dynamickou sadu hodnot omezující vlastnosti položky)
  * Příklad: `facet=baseRate,values:10|20` vytvoří tři kbelíků: jeden pro základní míra 0 až toobut není včetně 10, jeden pro 10 až toobut bez zahrnutí 20 a jeden pro 20 nebo vyšší.
  * Příklad: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` vytvoří dvě kbelíků: jeden pro hotels renovovanou před. února 2010 a jeden pro hotels renovovanou února 1. 2010 nebo novější.
* `interval`(interval celé číslo větší než 0. v případě čísel nebo `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` pro hodnoty času datum)
  * Příklad: `facet=baseRate,interval:100` vytváří kbelíků založené na základní míra rozsahy velikost 100. Například pokud základní sazby všechny až 60 $ $600, budou existovat intervaly 0-100, 100 200, 200 300, 300 400, 400-500 a 500 600.
  * Příklad: `facet=lastRenovationDate,interval:year` vytváří jeden sady pro každý rok po hotels byly renovovanou.
* `timeoffset`([+-] hh: mm, [+-] hh: mm, nebo [+-] hh) `timeoffset` je volitelný. Můžete sloučit pouze s hello `interval` možnost a pouze tehdy, když použité tooa pole typu `Edm.DateTimeOffset`. Hodnota Hello určuje hello UTC Čas posunutí tooaccount pro nastavení času hranice.
  * Příklad: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` používá hello den hranice, která začíná na UTC 01:00:00 (půlnoc v hello cílové časové pásmo)
* **Poznámka:**: `count` a `sort` lze spojit do hello stejné omezující vlastnost specifikace, ale nelze kombinovat s `interval` nebo `values`, a `interval` a `values` nemůže být společně kombinovány.
* **Poznámka:**: omezující vlastnosti Interval na datum a čas se vypočítávají podle času UTC, pokud `timeoffset` není zadán. Příklad: pro `facet=lastRenovationDate,interval:day`, hello den hranic začíná na 00:00:00 UTC. 

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `facets` místo `facet`. Také můžete zadat jako pole JSON řetězců, kde každý řetězec je výraz samostatné omezující vlastnosti.
> 
> 

`$filter=[string]`(volitelné) – výraz strukturovaných vyhledávání v standardní syntaxi OData.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.
> 
> 

`highlight=[string]`(volitelné) – označuje sadu názvů polí oddělených čárkami, které používá pro stiskněte klávesu. Pouze `searchable` pole lze použít pro přístupů zvýraznění.

`highlightPreTag=[string]`(volitelné, použije se výchozí hodnota příliš`<em>`) – řetězec značky, která přidá toohit označuje. Musí být nastavena s `highlightPostTag`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).
> 
> 

`highlightPostTag=[string]`(volitelné, použije se výchozí hodnota příliš`</em>`)-značku řetězec, který připojí toohit označuje. Musí být nastavena s `highlightPreTag`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).
> 
> 

`scoringProfile=[string]`(volitelné) – název hello vyhodnocování tooevaluate profil odpovídat skóre pro odpovídající dokumenty ve výsledcích hello toosort pořadí.

`scoringParameter=[string]`(nula nebo více) – označuje hello hodnoty pro každý parametr definovaný ve funkci vyhodnocování (například `referencePointParameter`) formátu hello `name-value1,value2,...`.

* Například pokud hello vyhodnocování profil definuje funkci s parametr s názvem "mylocation" hello zadat parametr řetězce dotazu by `&scoringParameter=mylocation--122.2,44.8`. první dash Hello hello název odděluje od hello seznam hodnot, zatímco druhý dash hello je součástí hello první hodnota (zeměpisné délky v tomto příkladu).
* Pro výpočet skóre parametry, jako je značky zvyšovat skóre, který může obsahovat čárky můžete vyhnuli tyto hodnoty v seznamu hello pomocí jednoduchých uvozovek a být. Pokud hello hodnoty, sami obsahovat jednoduché uvozovky, můžete je vyhnuli předchozího.
  * Například pokud máte značku zvyšovat skóre parametr s názvem "mytag" a chcete tooboost na hello značky hodnoty "Hello, O'Brien" a "Smith", hello dotazu by možnost řetězce `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Všimněte si, že uvozovky jsou pouze požadované hodnoty, které obsahují čárkami.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `scoringParameters` místo `scoringParameter`. Navíc je zadat jako pole JSON řetězců, kde je každý řetězec samostatné `name-values` pár.
> 
> 

`minimumCoverage`(volitelné, použije se výchozí hodnota too100) - číslo v rozsahu od 0 do 100 určující procento hello hello index, který musí být pokryté komponentami vyhledávací dotaz v pořadí pro hello dotazu toobe hlášené jako úspěšné. Ve výchozím nastavení, musí být k dispozici celý index hello nebo `Search` vrátí stavový kód protokolu HTTP 503. Pokud nastavíte `minimumCoverage` a `Search` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi hello označující hello procento hello index, který je zahrnutý v dotazu hello.

> [!NOTE]
> Nastavení této hodnoty parametru tooa menší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku. Ne všechny odpovídající dokumenty jsou však zaručena toobe existuje ve výsledcích hledání hello. Pokud hledání odvolání je důležitější tooyour aplikace než dostupnosti, pak je nejlepší tooleave `minimumCoverage` na výchozí hodnotě 100.
> 
> 

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

Poznámka: Pro tuto operaci hello `api-version` je zadána jako parametr dotazu v URL hello bez ohledu na to, jestli volání **vyhledávání** s GET nebo POST.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour. Hello **vyhledávání** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

U metody GET: žádné.

Pro metodu POST:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Pokračování částečné vyhledávání odpovědí**

Někdy Azure Search nemůže vrátit, že všechny hello požadovaný má za následek odpověď o jedné vyhledávání. K tomu může dojít různých důvodů, například kdy hello dotazu vyžadovali příliš mnoho dokumentů není zadáním `$top` nebo zadáte hodnotu `$top` , protože je příliš velký. V takových případech bude obsahovat Azure Search hello `@odata.nextLink` poznámky v textu hello odpověď a také `@search.nextPageParameters` Pokud byl požadavek POST. Můžete hello hodnoty těchto poznámek tooformulate jiné vyhledávání požadavku tooget hello další části hello hledání odpovědi. Tento postup se nazývá ***pokračování*** hello původní žádost vyhledávání a hello poznámky se obvykle označují jako ***pokračování tokeny***. V tématu [hello příklad](#SearchResponse) podrobnosti o syntaxi hello těchto poznámek a jejich umístění v textu odpovědi hello. 

Hello důvodů, proč Azure Search může vrátit pokračování tokeny jsou toochange závisí na implementaci a předmět. Robustní klientů musí být vždy připraven toohandle případy, kdy jsou vráceny méně dokumenty, než se očekává a je zahrnuté toocontinue načítání dokumenty token pokračování. Také Upozorňujeme, že je nutné použít hello stejnou metodu HTTP jako původní žádost hello v toocontinue pořadí. Například pokud jste odeslali požadavek GET, všechny žádosti pokračování odeslání musíte taky použít GET (a stejně tak pro metodu POST).

<a name="SearchResponse"></a>
**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

    {
      "@odata.count": # (if $count=true was provided in hello query),
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "@search.facets": { (if faceting was specified in hello query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body toofetch hello next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL toofetch hello next page of results if not all results could be returned in this response; Applies tooboth GET and POST)
    }

**Příklady:**

Další příklady můžete najít na hello [syntaxe výrazu OData pro službu Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) stránky.

1)    Hledání hello Index seřazeny sestupně podle data.

    GET /indexes/hotels/docs? hledání = * & $orderby = lastRenovationDate desc & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "orderby": "lastRenovationDate desc"}

2)    V rámci Fasetové vyhledávání v rejstříku hello a načíst omezující vlastnosti pro kategorií, hodnocení, značky, jakož i položky s baseRate v konkrétních oblastech:

    GET /indexes/hotels/docs? hledání = test & omezující vlastnost = kategorie & omezující vlastnost = hodnocení & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["kategorie", "hodnocení", "značky", "baseRate, hodnoty: 80 | 150 | 220"]}

3)    Pomocí filtru, hello předchozí Fasetové dotazu výsledky zúžit po hello uživatel klikne na hodnocení 3 a kategorie "Motel":

    GET /indexes/hotels/docs? hledání = test & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & $filter = hodnocení eq 3 a kategorie eq "Motel" & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["značky", "baseRate, hodnoty: 80 | 150 | 220"], "filtr": "hodnocení eq 3 a kategorie eq"Motel""}

4) V rámci Fasetové vyhledávání nastavte horní limit na jedinečný podmínky vrácených dotazem. Hello výchozí hodnota je 10, ale můžete zvýšit nebo snížit tuto hodnotu pomocí hello `count` parametr na hello `facet` atribut:

    GET /indexes/hotels/docs? hledání = test & omezující vlastnost = města, počet: 5 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["města, počet: 5"]}

5)    Hledání hello Index v rámci konkrétních polí; Například pro specifický jazyk pole:

    GET /indexes/hotels/docs? hledání = hôtel & searchFields = description_fr & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hôtel", "searchFields": "description_fr"}

6) Hledání hello Index napříč více polí. Například můžete ukládat a dotaz prohledatelná pole v několika jazycích, vše v rámci hello stejný index.  Pokud angličtinu a francouzštinu popisy současně existovat ve hello stejné dokumentu, můžete se vrátit any nebo all v hello výsledků dotazu:

    GET /indexes/hotels/docs? hledání = hotelů & searchFields = popis, description_fr & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hotelů", "searchFields": "popis, description_fr"}

Všimněte si, že dotaz lze použít pouze jeden index najednou. Nevytvářejte více indexů pro jednotlivé jazyky, pokud máte v plánu tooquery, jeden v čase.

7)    Stránkování - Get hello 1 stránku položek (velikost stránky je 10):

    GET /indexes/hotels/docs? hledání = * & $skip = 0 & $top = 10 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": 0, "top": 10}

8)    Stránkování - Get hello 2. stránka položky (velikost stránky je 10):

    GET /indexes/hotels/docs? hledání = * & $skip = 10 & $top = 10 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": "top" 10: 10}

9)    Načtěte konkrétní sadu pole:

    GET /indexes/hotels/docs? hledání = * & $select = hotelName, popis a rozhraní api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "Vyberte": "hotelName, popis"}

10)  Načtení dokumenty odpovídající výraz konkrétní filtru:

    GET /indexes/hotels/docs? $filter = (baseRate ge 60 a baseRate lt 300) nebo hotelName eq 'Zvláštní zůstat' & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"filtr": "(baseRate ge 60 a baseRate lt 300) nebo hotelName eq"Zvláštní zůstat""}

11) Hello index a nevrací fragmenty se přístupů označuje

    GET /indexes/hotels/docs? hledání = něco & zvýrazněte = Popis & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "highlight": "Popis"}

12) Hledání hello indexu a vrátit dokumenty seřadit z blíže toofarther směrem od referenčního místa

    GET /indexes/hotels/docs? vyhledávání něco = & $orderby=geo.distance (umístění, geography'POINT(-122.12315 47.88121)') & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "orderby": "geo.distance (umístění, geography'POINT(-122.12315 47.88121)')"}

13) Hledání hello index za předpokladu, že je profil vyhodnocování názvem "geo" s dvě funkce pro výpočet skóre podle vzdálenosti, jeden definování parametr s názvem "currentLocation" a jeden definování parametr s názvem "lastLocation"

    GET /indexes/hotels/docs? vyhledávání něco = & scoringProfile = geograficky & scoringParameter = currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "scoringProfile": "geo", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}

14) Najít dokumenty v indexu pomocí hello [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx). Tento dotaz vrací hotels kde prohledatelná pole obsahovat podmínky hello "pohodlí" a "umístění", ale ne "motel":

    GET /indexes/hotels/docs? hledání = pohodlí + umístění-motel & searchMode = all & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "pohodlí + umístění-motel", "searchMode": "vše"}

Všimněte si použití hello `searchMode=all` výše. Tento parametr včetně přepíše výchozí hello `searchMode=any`, zajistit který `-motel` znamená "A není" místo "Nebo není". Bez `searchMode=all`, získáte "Nebo není" který rozbalí než omezuje výsledky hledání, a to může být counter-intuitive toosome uživatelé.

15) Najít dokumenty v indexu pomocí hello [syntaxe dotazů lucene](https://msdn.microsoft.com/library/mt589323.aspx). Tento dotaz vrací hotels kde hello pole kategorie obsahuje hello termín "rozpočet" a všechna prohledatelná pole obsahující hello frázi "nedávno renovovanou". Dokumenty obsahující hello frázi "nedávno renovovanou" jsou řazeny výše v důsledku hello termín nárůst hodnotu (3)

    GET /indexes/hotels/docs? hledání = kategorie: nároky a \"nedávno renovovanou\"^ 3 & searchMode = all & verze api-version = 2015-02-28-Preview & Typ úplné =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "kategorie: nároky a \"nedávno renovovanou\"^ 3", "typ": "úplná", "searchMode": "vše"}

<a name="LookupAPI"></a>

## <a name="lookup-document"></a>Vyhledávání dokumentů
Hello **vyhledávání dokumentů** operace dokumentu načte z Azure Search. To je užitečné, když uživatel klikne na konkrétní výsledek a chcete, aby toolook si konkrétní podrobnosti o tomto dokumentu.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. Hello **vyhledávání dokumentů** požadavek konstruovat následujícím způsobem.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternativně můžete hello tradiční OData syntaxe vyhledávání klíčů:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

žádost o Hello URI obsahuje [název indexu] a [klíče], které tooretrieve dokumentu z index, který zadáte. Najednou můžete získat pouze jeden dokument. Použití **vyhledávání** tooget více dokumentů v jedné žádosti.

**Parametry dotazu**

`$select=[string]`(volitelné) – seznam tooretrieve polí oddělených čárkami. Pokud tento parametr nezadáte, nebo nastavte příliš`*`, všechna pole, které jsou označeny jako získat ve schématu hello jsou součástí hello projekce.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

Poznámka: Pro tuto operaci hello `api-version` je zadána jako parametr dotazu.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour. Hello **vyhledávání dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

    {
      field_name: field_value (fields matching hello default or specified projection)
    }

**Příklad**

Vyhledávání hello dokument, který má klíč: 2.

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Dokument hello vyhledávání, který má klíč pomocí syntaxe OData 3:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a>Počet dokumentů
Hello **počet dokumentů** operace načte počet hello počet dokumentů v indexu vyhledávání. Hello `$count` syntaxe je součástí hello protokolu OData.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. Hello **počet dokumentů** požadavek se dá vytvořit pomocí metody GET hello.

Hello [název indexu] v identifikátoru URI žádosti hello informuje hello služby tooreturn počet všechny položky v kolekci dokumenty hello hello zadaným indexem.

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.

* `Accept`: Tato hodnota musí být nastavena příliš`text/plain`.
* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour. Hello **počet dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

text odpovědi Hello obsahuje hodnotu počtu hello jako celé číslo ve formátu v prostém textu.

<a name="Suggestions"></a>

## <a name="suggestions"></a>Návrhy
Hello **návrhy** operace načte návrhy založené na vstupu částečné vyhledávání. Ho se obvykle používá v našeptávání návrhy vyhledávání polí tooprovide jako uživatelé zadávají hledaný text.

Požadavky návrhu zaměřena na cíl dokumenty návrhy, takže hello navrhované, že text může být opakována v případě více dokumentů candidate odpovídat hello stejné vyhledávání vstup. Můžete použít `$select` tooretrieve jiných dokumentů pole (včetně klíč dokumentu hello) tak, aby můžete zjistit, který dokument je hello zdroj pro každý návrhu.

A **návrhy** operaci se objeví jako požadavek GET nebo POST.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Když toouse POST místo GET**

Při použití metody GET protokolu HTTP hello toocall **návrhy** rozhraní API, musíte vědět, že nesmí překročit délka hello adresy URL žádosti hello 8 KB toobe. Je to obvykle dostatečně pro většinu aplikací. Některé aplikace však vytvořit velmi velké dotazy, konkrétně výrazy filtru OData. Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry než metoda GET. S POST hello počet klauzulí ve filtru je hello omezující faktor, není hello velikost řetězec nezpracovaná filtru hello vzhledem k tomu, že omezení velikosti hello požadavku pro metodu POST je přibližně 16 MB.

> [!NOTE]
> I když limit velikosti požadavek POST hello je moc velká, nemůže být libovolně komplexní výrazech filtru. V tématu [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost filtru.
> 
> 

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. Hello **návrhy** požadavek se dá vytvořit pomocí hello GET nebo POST metody.

identifikátor URI požadavku Hello Určuje název hello hello index tooquery. Parametry, jako je například hello částečně vstupní hledaný termín, jsou zadány na hello řetězec dotazu v případě hello požadavků GET a v žádosti o hello textu v případě hello POST požadavky.

Jako osvědčený postup při vytváření požadavky GET, mějte na paměti, příliš[kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifického dotazu parametry při volání metody hello REST API přímo. Pro **návrhy** operace, to zahrnuje:

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

Kódování URL se doporučuje jenom na hello výše parametry dotazu. Pokud jste omylem kódování URL hello řetězec dotazu celý, (vše za hello?), dojde k přerušení požadavky.

Navíc kódování URL je nutné pouze při volání metody hello získat přímo pomocí rozhraní REST API. Žádné kódování URL je nezbytné při volání metody **návrhy** pomocí POST, nebo při použití hello [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), která zpracovává kódování URL za vás.

**Parametry dotazu**

**Návrhy** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání. Zadejte tyto parametry v adrese URL hello řetězec dotazu při volání metody **návrhy** prostřednictvím GET a jako vlastnosti JSON v textu žádosti hello při volání metody **návrhy** přes POST. Hello syntaxe pro některé parametry se mírně liší mezi GET a POST. Tyto rozdíly jsou popsány podle vhodnosti níže:

`search=[string]`-hello vyhledávací text toouse toosuggest dotazy. Musí být minimálně 1 znak a maximálně 100 znaků.

`highlightPreTag=[string]`(volitelné) – značku řetězec, který přidá toosearch přístupů. Musí být nastavena s `highlightPostTag`.

> [!NOTE]
> Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).
> 
> 

`highlightPostTag=[string]`(volitelné) – značku řetězec, který připojí toosearch přístupů. Musí být nastavena s `highlightPreTag`.

> [!NOTE]
> Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).
> 
> 

`suggesterName=[string]`-hello název modulu pro návrhy hello jako zadaný v hello `suggesters` kolekce, která je součástí definice indexu hello. A `suggester` určuje pole, která hledat podmínky navrhované dotazu. V tématu [trochu](#Suggesters) podrobnosti.

`fuzzy=[boolean]`(volitelné, výchozí = false) – Pokud nastavíte tootrue toto rozhraní API najdete návrhy i tehdy, je nahrazovanou nebo chybí znak v hello hledaný text. Zatímco to poskytuje lepší podmínky v některých scénářích kompromisnímu výkon, protože vyhledávání s fuzzy logikou návrhu pomalejší a spotřebovávat více prostředků.

`searchFields=[string]`(volitelné) – seznam hello toosearch názvy polí oddělených čárkou pro hello zadaný hledaný text. Cílová pole. musí být povolen pro návrhy.

`$top=#`(volitelné, výchozí = 5)-hello počet tooretrieve návrhy. Musí být číslo v rozsahu od 1 do 100.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `top` místo `$top`.
> 
> 

`$filter=[string]`(volitelné) – výraz, který filtruje hello dokumenty za návrhy.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.
> 
> 

`$orderby=[string]`(volitelné) – seznam oddělený čárkami výrazy toosort hello výsledků podle. Každý výraz může být název pole nebo volání toohello `geo.distance()` funkce. Každý výraz může následovat `asc` tooindicated vzestupné řazení a `desc` tooindicate sestupně. Výchozí Hello je vzestupné pořadí. Existuje limit 32 klauzule pro `$orderby`.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.
> 
> 

`$select=[string]`(volitelné) – seznam tooretrieve polí oddělených čárkami. Pokud tento parametr nezadáte, hello pouze klíč dokumentu a návrhu je vrácen. Všechna pole můžete explicitně žádostí nastavením tento parametr příliš`*`.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `select` místo `$select`.
> 
> 

`minimumCoverage`(volitelné, použije se výchozí hodnota too80) - číslo v rozsahu od 0 do 100 určující procento hello hello index, který musí být předmětem dotazu návrhy aby hello dotazu toobe hlášené jako úspěšné. Ve výchozím nastavení, musí být k dispozici alespoň 80 % hello index nebo `Suggest` vrátí stavový kód protokolu HTTP 503. Pokud nastavíte `minimumCoverage` a `Suggest` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi hello označující hello procento hello index, který je zahrnutý v dotazu hello.

> [!NOTE]
> Nastavení této hodnoty parametru tooa menší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku. Ne všechny odpovídající návrh je však zaručena toobe součástí hello výsledky. Pokud odvolání je důležitější tooyour aplikace než dostupnost, je jeho osvědčené není toolower `minimumCoverage` pod jeho výchozí hodnota 80.
> 
> 

`api-version=[string]`(povinné). verze preview Hello je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

Poznámka: Pro tuto operaci hello `api-version` je zadána jako parametr dotazu v URL hello bez ohledu na to, jestli volání **návrhy** s GET nebo POST.

**Hlavičky požadavku**

Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi

* `api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání. Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour. Hello **návrhy** žádosti můžete zadat buď klíč správce, nebo klíč dotazu jako hello `api-key`.

Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti. Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

U metody GET: žádné.

Pro metodu POST:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Pokud je možnost projekce hello použité tooretrieve pole, která jsou součástí každý element pole hello:

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Příklad**

Načtení 5 návrhy, kde je hello částečné vyhledávání vstup 'lux.

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
