---
title: profily aaaScoring (Azure Search REST API verze 2015-02-28-Preview) | Microsoft Docs
description: "Služba Azure Search je hostované cloudové vyhledávací službě, která podporuje ladění výsledků seřazený podle uživatelské profily vyhodnocování."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: bfd62649-13d7-40b3-a8fa-85521a15084d
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.author: heidist
ms.date: 10/27/2016
ms.openlocfilehash: 17f83fdf6818dc6ffcc3e04f5d0185c6f646b63d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Vyhodnocování profilů (Azure Search REST API verze 2015-02-28-Preview)
> [!NOTE]
> Tento článek popisuje vyhodnocování profily v hello [2015-02-28-Preview](search-api-2015-02-28-preview.md). Aktuálně není žádný rozdíl mezi hello `2016-09-01` verze popsané na [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) a hello `2015-02-28-Preview` zde popsané verze, ale nabízíme tento dokument přesto v pořadí tooprovide dokumentu pokrytí napříč hello celý rozhraní API.
>
>

## <a name="overview"></a>Přehled
Vyhodnocování odkazuje toohello výpočet skóre vyhledávání pro každou položku vrátila ve výsledcích hledání. skóre Hello je slouží jako ukazatel relevance položky v kontextu hello hello aktuální operace vyhledávání. Hello vyšší hello skóre, hello relevantnější hello položky. Ve výsledcích hledání jsou položky seřazené z vysoké toolow, podle skóre vyhledávání hello vypočítat pro každou položku pořadí.

Služba Azure Search používá výchozí vyhodnocování toocompute počáteční skóre, ale můžete přizpůsobit hello výpočtu prostřednictvím profil vyhodnocování. Vyhodnocování profily získáte větší kontrolu nad hello hodnocení položek ve výsledcích hledání. Může například chcete tooboost položky na základě jejich potenciální výnosů, zvýšit úroveň novější položky nebo možná zvýšení položky, které byly v inventáři příliš dlouhý.

Profil vyhodnocování je součástí definice indexu hello, skládá z pole, funkce a parametry.

toogive jste představu o co profil vyhodnocování vypadá to, hello následující příklad ukazuje jednoduchý profil s názvem 'geograficky'. Tato zvyšuje položky, které mají hello hledaný termín v hello `hotelName` pole. Používá také hello `distance` funkce toofavor položky, které se nacházejí v rámci deset kilometrech hello aktuální umístění. Pokud někdo hledá hello termín 'DIČ' a 'DIČ, se stane toobe součástí názvu hotelů hello, bude zobrazovat na vyšších pozicích ve výsledcích hledání hello dokumenty, které zahrnují hotels s 'DIČ'.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

toouse, je tento profil vyhodnocování, dotaz formulovali toospecify hello profilu v řetězci dotazu hello. V dotazu hello níže, Všimněte si hello parametr dotazu, `scoringProfile=geo` v žádosti o hello.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Tento dotaz hledá hello termín 'DIČ' a předá hello aktuální umístění. Všimněte si, že tento dotaz obsahuje dalších parametrů, jako například `scoringParameter`. Parametry dotazu jsou popsané v [vyhledávání dokumentů (API služby Azure Search)](search-api-2015-02-28-preview.md#SearchDocs).

Klikněte na tlačítko [příklad](#example) tooreview podrobnější příklad profil vyhodnocování.

## <a name="what-is-default-scoring"></a>Co je vyhodnocování výchozí?
Vyhodnocování vypočítá skóre vyhledávání pro každou položku v sadě rank seřazený výsledek. Každá položka v sadě výsledků hledání je přiřazen skóre vyhledávání a potom seřazeny nejvyšší toolowest. Položky s vyšší skóre hello se vrátí toohello aplikace. Ve výchozím nastavení, hello top 50 se vrátí, ale můžete použít hello `$top` parametr tooreturn menší nebo větší počet položek (až too1000 jedinou odpověď).

Ve výchozím nastavení se vypočítá skóre vyhledávání podle statistické vlastnosti hello dat a dotazu hello. Služba Azure Search Vyhledá dokumenty, které zahrnují hello hledaná slova v řetězci dotazu hello (některé nebo všechny, v závislosti na `searchMode`), upřednostňuje dokumentů, které obsahují velký počet instancí hello hledaný termín. skóre vyhledávání Hello se zvyšuje i vyšší Pokud hello termín je taková situace vzácná přes svátek hello data, ale běžné v rámci hello dokumentu. Hello základ pro relevance toocomputing tento postup se označuje jako TF-IDF nebo (termín frekvence inverzní dokumentu frekvenci).

Za předpokladu, že neexistuje žádné vlastní řazení, výsledky, jsou pak seřazeny podle skóre vyhledávání předtím, než jsou vrácena toohello volající aplikace. Pokud `$top` není zadán, 50 položek, které mají nejvyšší vyhledávání hello jsou vráceny skóre.

Hledání skóre hodnoty můžete opakovat po dobu je sada výsledků dotazu. Například můžete mít 10 položek se skóre 1.2, 20 položky se skóre 1.0 a 20 položky se skóre 0,5. Když více přístupů do mají hello stejné skóre vyhledávání, řazení hello stejné scored položek není definována a není stabilní. Znovu spustit dotaz hello a může se zobrazit položky posunutí pozice. Zadána dvě položky s identické skóre není zaručeno, která se zobrazí jako první.

## <a name="when-toouse-custom-scoring"></a>Při vyhodnocování vlastní toouse
Měli byste vytvořit jeden nebo více profily vyhodnocování hello výchozí řazení chování není přejděte dostatečně v pro splnění cílů vaší firmy. Například můžete rozhodnout, že závažnost hledání by měl upřednostnit nově přidané položky. Podobně můžete mít pole, které obsahuje zisku nebo některé jiné pole označující potenciál. Zvyšovat skóre přístupů, které přinášejí tooyour obchodní výhody, může být důležitým faktorem při rozhodování o toouse profily vyhodnocování.

Na základě důležitosti řazení se také implementuje pomocí profily vyhodnocování. Vezměte v úvahu stránky, které jste použili v minulosti hello, které vám umožní seřadit podle ceny, data, hodnocení nebo relevance výsledků vyhledávání. Ve službě Azure Search jednotka vyhodnocování profily možnost, relevance"hello. definice Hello relevance spravujete, zabezpečuje pomocí vhodné obchodních cílů a hello typ vyhledávání, že který má toodeliver.

<a name="example"></a>

## <a name="example"></a>Příklad
Jak jsme uvedli, přizpůsobené vyhodnocování se implementuje pomocí vyhodnocování profily definice ve schématu indexu.

Tento příklad ukazuje hello schéma indexu s dva profily vyhodnocování (`boostGenre`, `newAndHighlyRated`). Jakýkoli dotaz proti tento index, který zahrnuje buď profil jako parametr dotazu použije sadu výsledků hello profil tooscore hello.

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Pracovní postup
vlastní tooimplement vyhodnocování chování, přidejte vyhodnocování schématu toohello profil, který definuje hello index. Může mít až too16 profily vyhodnocování v rámci indexu (najdete v části [omezení služby](search-limits-quotas-capacity.md)), ale v době v jakékoli dané dotazu lze určit pouze jeden profil.

Začněte s hello [šablony](#bkmk_template) uvedené v tomto tématu.

Zadejte název. Vyhodnocování profily jsou volitelné, ale pokud přidáte jednu, je nutné zadat název hello. Být jisti toofollow hello zásady vytváření názvů pro pole (začíná písmenem, zabraňuje speciální znaky a vyhrazená slova). V tématu [pravidla pojmenování](http://msdn.microsoft.com/library/azure/dn857353.aspx) Další informace.

Hello textu hello vyhodnocování profil sestavený ze vyvážené pole a funkce.

### <a name="weights"></a>Váhu
Hello `weights` vlastnost profil vyhodnocování Určuje dvojice název hodnota, které přiřadit tooa relativní váhu pole. V hello [příklad](#example), hello albumTitle genre, jsou a artistName boosted 1.5, 5, 2, v uvedeném pořadí. Proč je genre boosted mnohem vyšší než ostatní hello? Pokud se provádí vyhledávání přes data, která je poněkud homogenní (stejně jako hello případě 'genre' v hello `musicstoreindex`), může být nutné větší odchylky v relativní váhu hello. Například v hello `musicstoreindex`, 'rock' zobrazí jako obou genre a v popisech genre stejně jako obsahuje jiné spojení. Pokud chcete, aby genre toooutweigh genre popis, bude nutné hello genre pole mnohem vyšší relativní váhu.

### <a name="functions"></a>Funkce
Funkce se používají, pokud je potřeba pro konkrétní kontexty další výpočty. Funkce platné typy jsou `freshness`, `magnitude`, `distance` a `tag`. Jednotlivé funkce obsahuje parametry, které jsou jedinečné tooit.

* `freshness`by měl použít, pokud chcete, že je tooboost jak starý, nebo nový u položky. Tuto funkci lze použít pouze s pole data a času (`Edm.DataTimeOffset`). Poznámka: hello `boostingDuration` atribut se používá jenom s hello aktuálnosti funkce.
* `magnitude`by měl použít, pokud chcete, že je tooboost podle způsobu, jakým vysoké a nízké číselnou hodnotu. Scénáře, které volají pro tuto funkci zahrnují zvyšovat skóre marže, nejvyšší cenu, nejnižší cena nebo počet souborů ke stažení. Můžete nechat provést zpětnou hello rozsah, vysoké toolow, pokud chcete, aby inverzní vzor hello (například tooboost nižší cenou položky víc než vyšší za cenu položky). Zadaný rozsah ceny ze 100 USD příliš$ 1, byste měli nastavit `boostingRangeStart` na 100 a `boostingRangeEnd` na 1 tooboost hello nižší cenou položky. Tato funkce slouží pouze s dvojitou a celé číslo pole.
* `distance`má být používána když chcete, aby tooboost blízkosti nebo zeměpisné umístění. Tuto funkci lze použít pouze s `Edm.GeographyPoint` pole.
* `tag`má být používána když chcete, aby tooboost značky společné mezi dokumenty a vyhledávací dotazy. Tuto funkci lze použít pouze s `Edm.String` a `Collection(Edm.String)` pole.

#### <a name="rules-for-using-functions"></a>Pravidla pro používání funkce
* Typ funkce (aktuálnosti, rozsahem, vzdálenost, značka) musí být malá písmena.
* Funkce nesmí obsahovat hodnoty null nebo prázdný. Konkrétně, pokud zahrnete název pole, máte tooset ho toosomething.
* Funkce lze pouze pole použité toofilterable. V tématu [Create Index](search-api-2015-02-28-preview.md#CreateIndex) Další informace o filtrování pole.
* Funkce může být pouze použité toofields, která jsou definována v hello kolekce polí indexu.

Po definování indexu hello sestavení indexu hello tím, že nahrajete schéma indexu hello, za nímž následuje dokumenty. V tématu [Create Index](search-api-2015-02-28-preview.md#CreateIndex) a [přidat nebo aktualizovat dokumenty](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) pokyny týkající se těchto operací. Po hello index, byste měli mít funkční vyhodnocování profil, který pracuje s daty vyhledávání.

<a name="bkmk_template"></a>

## <a name="template"></a>Šablona
Tato část uvádí hello syntaxe a šablony pro vyhodnocování profily. Odkazovat příliš[indexu reference na atribut](#bkmk_indexref) v další části hello popisy hello atributů.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies toosearchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location)
              "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>

## <a name="scoring-profile-property-reference"></a>Odkaz na vlastnost profilu vyhodnocování
> [!NOTE]
> Vyhodnocování funkce může být pouze použité toofields, které jsou filtrování.
>
>

| Vlastnost | Popis |
| --- | --- |
| `name` |Povinná hodnota. Toto je název profilu vyhodnocování hello hello. Se stejnými zásadami vytváření názvů pole hello způsobem. Ho musí začínat písmenem, nemůže obsahovat tečky, dvojtečky nebo @ symboly a nesmí začínat hello frázi "azureSearch" (malá a velká písmena). |
| `text` |Obsahuje vlastnost váhu hello. |
| `weights` |Volitelné. Dvojice název hodnota, která určuje název pole a relativní váhy. Relativní váhu musí být kladné celé číslo nebo číslo s plovoucí desetinnou čárkou. Můžete zadat název pole hello bez odpovídající váhu. Váhu jsou použité tooindicate hello význam relativní tooanother jedno pole. |
| `functions` |Volitelné. Všimněte si, že vyhodnocování funkce lze pouze použité toofields, které jsou filtrování. |
| `type` |Požaduje se pro funkce vyhodnocování. Určuje typ funkce toouse hello. Platné hodnoty patří `magnitude`, `freshness`, `distance` a `tag`. Můžete zahrnout více než jednu funkci každý profil vyhodnocování. název funkce Hello musí být malá písmena. |
| `boost` |Požaduje se pro funkce vyhodnocování. Kladné číslo použije jako násobitel základního skóre. Nemůže být stejná too1. |
| `fieldName` |Požaduje se pro funkce vyhodnocování. Vyhodnocování funkce může být pouze použité toofields, které jsou součástí kolekce pole hello hello indexu a které jsou filtrování. Kromě toho každý typ funkce zavádí další omezení (podle aktuálnosti se používá s pole data a času, podle magnitudy se celé číslo nebo double poli, vzdálenost vložením polí, umístění a značky s řetězec nebo řetězec kolekci polí). Zadávat lze pouze jedno pole za definici funkce. Pro příklad, toouse rozsahem dvakrát v hello stejný profil, potřebovali byste rozsahem definice tooinclude dva, jeden pro každé pole. |
| `interpolation` |Požaduje se pro funkce vyhodnocování. Definuje sklon hello, pro které hello zvyšování skóre od začátku hello toohello konec rozsahu hello hello rozsahu. Platné hodnoty patří `linear` (výchozí), `constant`, `quadratic`, a `logarithmic`. V tématu [nastavit interpolace](#bkmk_interpolation) podrobnosti. |
| `magnitude` |Hello rozsahem vyhodnocování funkce je použité tooalter pořadí na základě hello rozsahu hodnot pro číselné pole. Mezi nejběžnější příklady použití hello tohoto patří:<ul><li>Z hodnocení hvězdičkami: Alter hello vyhodnocování podle hello hodnotu v poli "Hvězdičky hodnocení" hello. Při dvou položek jsou relevantní, zobrazí se nejprve hello položku s vyšší hodnocení hello.</li><li>Okraj: Pokud dva dokumenty jsou relevantní, může přát prodejce tooboost dokumenty, které mají vyšší okraje nejdřív.</li><li>Klikněte na tlačítko počty: aplikace, které sledují kliknutím prostřednictvím akce tooproducts nebo stránky, můžete použít rozsahem tooboost položky, které jsou obvykle tooget hello většina provozu.</li><li>Stáhněte si počty: pro aplikace, které sledují stahování, hello rozsahem funkce umožňuje můžete zvýšit položky, které mají hello většina souborů ke stažení.</li></ul> |
| `magnitude:boostingRangeStart` |Nastaví hello spustit hodnotu hello rozsahu, který stanoví skóre pro magnitudu. Hello hodnota musí být celé číslo nebo číslo s plovoucí desetinnou čárkou. U hodnocení hvězdičkami 1 až 4 by to bylo 1. Pro rozsahy nad 50 % by to bylo 50. |
| `magnitude:boostingRangeEnd` |Nastaví koncovou hodnotu hello hello rozsahu, který stanoví skóre pro magnitudu. Hello hodnota musí být celé číslo nebo číslo s plovoucí desetinnou čárkou. U hodnocení hvězdičkami 1 až 4 by to bylo 4. |
| `magnitude:constantBoostBeyondRange` |Platné hodnoty jsou true nebo false (výchozí). Pokud nastavíte tootrue, hello skóre bude pokračovat toodocuments tooapply, které mají hodnotu hello cílového pole, která je vyšší než horní mez rozsahu hello hello. Pokud je hodnota false, nebudou hello nárůst této funkce použité toodocuments s hodnotou hello cílového pole, která spadá mimo rozsah hello. |
| `freshness` |Hello aktuálnosti vyhodnocování funkce je použité tooalter pořadí položek na základě hodnot v polích typu DateTimeOffset. Položka s pozdější může být například vyšší než starší položky seřazeny. (Všimněte si, je také možné toorank položky třeba kalendář události s budoucí data tak, že hodnoty vyšší než další položky seřazeny položky blíže toohello přítomen v hello budoucí.) V aktuálním vydání služby hello jeden element end hello rozsahu bude opraven toohello aktuální čas. Hello druhém konci je čas v minulosti hello podle hello `boostingDuration`. tooboost rozsah časů ve hello budoucí použít jako záporné `boostingDuration`. míra Hello, na které hello zvyšovat skóre změny z maximální a minimální rozsah je dáno toohello interpolace použít hello vyhodnocování profilu (viz následující obrázek hello). tooreverse hello zvýšení skóre Multi-Factor použít, zvolte faktor zesílení menší než 1. |
| `freshness:boostingDuration` |Nastaví konec platnosti, po kterém se u konkrétního dokumentu přestane zvyšovat skóre. V tématu [nastavit boostingDuration](#bkmk_boostdur) v následující části Syntaxe a příklady hello. |
| `distance` |vzdálenost Hello vyhodnocování funkce je použité tooaffect hello skóre dokumentů podle toho, jaký zavřete nebo daleko jsou relativní tooa referenční geografické polohy. Hello referenčním umístěním je zadána jako součást dotazu hello parametr (pomocí hello `scoringParameter` parametr dotazu) jako fyzický pevný disk, lat argument. |
| `distance:referencePointParameter` |Parametr toobe předána toouse dotazů jako referenční umístění. scoringParameter je parametr dotazu. V tématu [vyhledávání dokumentů](search-api-2015-02-28-preview.md#SearchDocs) popisy parametry dotazu. |
| `distance:boostingDistance` |Číslo určující hello vzdálenost v kilometrech od hello referenčního místa, ve které končí hello zvyšovat skóre rozsahu. |
| `tag` |Hello značky vyhodnocování funkce se používá tooaffect hello skóre dokumentů podle značek v dokumentech a vyhledávací dotazy. Bude boosted dokumenty, které obsahují značky společné s hello vyhledávací dotaz. Hello značky pro hello vyhledávací dotaz je zadat jako parametr vyhodnocování v každé žádosti o vyhledávání (pomocí hello `scoringParameter` parametr dotazu). |
| `tag:tagsParameter` |Parametr toobe předávat v dotazech toospecify značky pro konkrétní žádost. `scoringParameter`je parametr dotazu. V tématu [vyhledávání dokumentů](search-api-2015-02-28-preview.md#SearchDocs) popisy parametry dotazu. |
| `functionAggregation` |Volitelné. Platí, pouze pokud je zadán funkce. Platné hodnoty patří: `sum` (výchozí), `average`, `minimum`, `maximum`, a `firstMatching`. Skóre vyhledávání je jediná hodnota, která se počítá z více proměnných, včetně víc funkcí. Tento atributy Určuje, jak jsou hello zvyšuje všech funkcí hello zkombinované do jednoho agregační nárůst, který je pak použité toohello základní skóre dokumentů. základní skóre Hello je založená na hodnotě tf-idf hello vypočítaný z dokumentu hello a hello vyhledávací dotaz. |
| `defaultScoringProfile` |Při provádění žádost o vyhledávání, pokud není zadán žádný profil vyhodnocování, vyhodnocování výchozím nastavení je použité (tf-idf pouze). Výchozí název profilu vyhodnocování lze nastavit tady, vyvolá Azure Search toouse tento profil, pokud žádný konkrétní profil je uveden v požadavku hledání hello. |

<a name="bkmk_interpolation"></a>

## <a name="set-interpolations"></a>Sada interpolace
Interpolace povolit toodefine hello sklon, pro které hello zvyšování skóre od začátku hello toohello konec rozsahu hello hello rozsahu. lze použít následující interpolace Hello:

* `Linear`: Pro položky, které jsou v rámci hello maximální a minimální rozsah bude provedeno hello nárůst použít toohello položky v rámci neustále klesá. Lineární je hello výchozí interpolace pro profil vyhodnocování.
* `Constant`: Pro položky, které jsou v rámci hello počáteční a koncová rozsahu bude konstantní nárůst použité toohello rank výsledky.
* `Quadratic`: V porovnání tooa lineární interpolace, který má neustále klesá nárůst Kvadratická sníží původně menší tempem a potom jako se blíží konec rozsahu hello, sníží v mnohem vyšší intervalu. Tato možnost interpolace není povolena ve značce funkce vyhodnocování.
* `Logarithmic`: V porovnání tooa lineární interpolace, který má neustále klesá nárůst toto zobrazení se původně sníží tempem vyšší a potom jako se blíží konec rozsahu hello, sníží v mnohem menší intervalu. Tato možnost interpolace není povolena ve značce funkce vyhodnocování.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>

## <a name="set-boostingduration"></a>Sada boostingDuration
`boostingDuration`je atribut hello aktuálnosti funkce. Použijete jej tooset zastaví platnosti, po které zvyšovat skóre u konkrétního dokumentu. Například tooboost řadu produktů nebo brand propagační dobu 10 dní, zadali byste hello období 10 dnů jako "P10D" pro tyto dokumenty. Nebo zadejte tooboost nadcházející události v hello příští týden "-P7D".

`boostingDuration`musí být naformátovaná jako hodnota "hodnoty doby podle" XSD (omezená podmnožina hodnoty doby trvání ISO 8601). vzor Hello: `[-]P[nD][T[nH][nM][nS]]`.

Hello následující tabulka obsahuje několik příkladů.

| Doba trvání | boostingDuration |
| --- | --- |
| 1 den |"P1D" |
| 2 dny a 12 hodin |"P2DT12H" |
| 15 minut |"PT15M" |
| 30 dní, 5 hodin, 10 minut, a 6.334 sekund |"P30DT5H10M6.334S" |

Další příklady najdete v tématu [schématu XML: datové typy (adrese W3.org webový server)](http://www.w3.org/TR/xmlschema11-2/).

**Viz také**
[rozhraní API REST služby vyhledávání systému Azure](http://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN <br/>
[Vytvoření indexu (Azure Search rozhraní API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) na webu MSDN<br/>
[Přidat vyhodnocování indexu vyhledávání tooa profil](http://msdn.microsoft.com/library/azure/dn798928.aspx) na webu MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
