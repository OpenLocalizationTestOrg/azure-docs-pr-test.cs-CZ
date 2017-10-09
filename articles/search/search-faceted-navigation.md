---
title: "aaaHow tooimplement Fasetové navigace ve službě Azure Search | Microsoft Docs"
description: "Přidejte tooapplications Fasetové navigace, který integraci se službou Azure Search, hostované cloudové vyhledávací službu v Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: cdf98fd4-63fd-4b50-b0b0-835cb08ad4d3
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 3/10/2017
ms.author: heidist
ms.openlocfilehash: c1e6bf9dc55d0044525db79e37d35a50ded5a736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-faceted-navigation-in-azure-search"></a>Jak tooimplement Fasetové navigace ve službě Azure Search
Fasetové navigace je filtrační mechanismus, který poskytuje řízené samotným přejít k podrobnostem navigace ve vyhledávání aplikací. Hello termín "Fasetové navigace, může být obeznámeni, ale pravděpodobně ho před jste použili. Fasetové navigace jako hello následující příklad ukazuje, není nic jiného než hello kategorií používá toofilter výsledky.

 ![Portál ukázkové úlohy vyhledávání systému Azure][1]

Fasetové navigace je toosearch alternativní vstupní bod. Nabízí pohodlný alternativní tootyping komplexní vyhledávacích výrazech ručně. Omezující vlastnosti můžete najít, co hledáte, a zajistit, že neobdržíte nulové výsledky. Jako vývojář omezující vlastnosti vám umožní vystavit hello nejužitečnější vyhledávací kritéria pro navigace vaše vyhledávání souhrnu. V aplikacích online prodejní Fasetové navigace je často sestavena značky, oddělení (dětský vcítit), velikost, ceny, oblíbenosti a hodnocení. 

Implementace Fasetové navigace se liší mezi technologie vyhledávání. Ve službě Azure Search je Fasetové navigace vytvořená v době dotazů s využitím pole, které jste dříve s atributy ve schématu.

-   V hello dotazy, které vaše aplikace vytvoří, musíte odeslat dotaz *parametry dotazu omezující vlastnost* sada výsledků tooget hello k dispozici omezující vlastnosti hodnoty filtru pro tento dokument.

-   tooactually trim sadu výsledků hello dokumentů, musíte také použít aplikaci hello `$filter` výraz.

Při vývoji vaší aplikace psaní kódu, který vytvoří dotazy se považuje za hello hromadné hello práce. Řadu hello chování aplikace, které by uživatel očekával od Fasetové navigace jsou poskytovány hello služby, včetně integrovanou podporu pro definování rozsahy a získávání počty omezující vlastnost výsledků. Hello služby zahrnuje také rozumný výchozí hodnoty, které vám pomůžou vyhnout nepraktické navigační struktury. 

## <a name="sample-code-and-demo"></a>Ukázkový kód a demo
Tento článek používá portál úlohy vyhledávání jako příklad. Příklad Hello je implementovaný jako aplikace ASP.NET MVC.

-   Zobrazit a otestovat hello pracovní ukázkový online v [Azure Search úlohy portál ukázku](http://azjobsdemo.azurewebsites.net/).

-   Stáhnout hello kód z hello [Azure-Samples úložišti na Githubu](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>Začínáme
Pokud jste vývoj nových toosearch, hello nejlepší způsob, jak toothink Fasetové navigace je zobrazuje hello možnosti řízené samotným vyhledávání. Je typ procházení hledání založené na předdefinovaných filtrů, použít pro rychlé zužující dolů výsledky hledání prostřednictvím akce bodu a kliknutím. 

### <a name="interaction-model"></a>Interakce modelu

Možnosti hledání Hello Fasetové navigace je iterativní, proto budeme, začněte tím, že vysvětlení jako posloupnost dotazy, které unfold v odpovědi toouser akce.

výchozí bod Hello je stránku aplikace, která poskytuje Fasetové navigace, obvykle umístěny na obvodu hello. Fasetové navigace je často stromová struktura, s zaškrtávací políčka pro každou hodnotu, nebo můžete kliknout text. 

1. Dotaz odeslané tooAzure vyhledávání určuje struktuře Fasetové navigace hello přes jeden nebo více parametrů dotazu omezující vlastnosti. Dotaz hello může zahrnovat `facet=Rating`, případně s `:values` nebo `:sort` toofurther možnost Upřesnit prezentace hello.
2. Prezentační vrstva Hello vykreslí stránku vyhledávání poskytující Fasetové navigace, pomocí hello omezujícími vlastnostmi zadanými na vyžádání hello.
3. Zadané strukturu Fasetové navigace, která zahrnuje hodnocení, kliknutí tooindicate "4", která má být zobrazena pouze produkty s hodnocení 4 nebo vyšší. 
4. V odpovědi hello aplikace odešle dotaz, který zahrnuje`$filter=Rating ge 4` 
5. Hello prezentační vrstvy aktualizace hello stránky, zobrazující snížené výslednou sadu, obsahující pouze tyto položky, které splňují kritéria nové hello (v tomto případě produkty hodnocení 4 a vyšší).

Omezující vlastnost je parametr dotazu, ale Nezaměňujte se vstupem dotazu. Se nikdy nepoužívá jako kritéria pro výběr v dotazu. Místo toho představit omezující vlastnost parametry dotazu jako vstupy toohello navigační strukturu, která se dodává zpět v odpovědi hello. Pro každý parametr dotazu omezující vlastnosti, které poskytnete vyhodnotí Azure Search, kolik dokumenty jsou v hello částečné výsledky pro každou hodnotu omezující vlastnosti.

Všimněte si hello `$filter` v kroku 4. Filtr Hello je důležitým aspektem Fasetové navigace. I když omezující vlastnosti a filtry jsou nezávislé v hello rozhraní API, musíte obou toodeliver hello prostředí, které máte v plánu. 

### <a name="app-design-pattern"></a>Vzor návrhu aplikace

V kódu aplikace je vzor hello toouse omezující vlastnosti dotazu parametry tooreturn hello Fasetové navigace struktura spolu s výsledky omezující vlastnost plus výraz $filter.  hello popisovače výraz filtru Hello click – událost na hodnota omezující vlastnosti hello. Představte si, že hello `$filter` výraz hello kódu skutečné oříznutí hello výsledků vyhledávání vrátil toohello prezentační vrstvy. Zadána omezující vlastnost barvy, klikněte na barvu hello Red se implementuje pomocí `$filter` výraz, který vybere jen ty položky, které mají barva red. 

### <a name="query-basics"></a>Základy dotazů

Ve službě Azure Search, požadavek se specifikuje prostřednictvím jeden nebo více parametrů dotazu (viz [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798927.aspx) popis každé z nich). Žádný z parametrů dotazu hello jsou požadovány, ale musí mít alespoň jeden v pořadí pro dotaz toobe, který je platný.

Přesnost, porozuměl jsem jim jako hello toofilter možnost si důležité přístupů je dosaženo pomocí jednoho nebo obou těchto výrazů:

-   **hledání =**  
    Hello hodnota tohoto parametru se považuje za hello hledaný výraz. Může být jediný text nebo výraz komplexní vyhledávání, který obsahuje více podmínek a operátory. Na serveru hello hledaný výraz slouží pro fulltextové vyhledávání, dotazování prohledávatelné pole v indexu hello k porovnání podmínky, vrátí výsledky v rank pořadí. Pokud nastavíte `search` toonull, provádění dotazů je nad celý index hello (tedy `search=*`). In this Case, další prvky hello dotazu, jako například `$filter` nebo vyhodnocování profil, jsou hello primární faktory, které mají vliv na dokumenty, které jsou vráceny `($filter`) a pořadí, v jakém (`scoringProfile` nebo `$orderby`).

-   **$filter =**  
    Filtr je výkonný mechanismus pro omezení velikosti hello výsledků vyhledávání na základě hello hodnot atributů určitého dokumentu. A `$filter` je nejdřív vyhodnotit, za nímž následuje používání omezujících vlastností logiky, která generuje hello dostupné hodnoty a odpovídající počty pro každou hodnotu

Komplexní vyhledávacích výrazech snížit výkon hello hello dotazu. Kde je to možné, použijte filtr dobře sestavený výrazy tooincrease přesnost a zlepšit výkon dotazů.

toobetter pochopit, jak filtr přidá další přesnost, porovnání tooone komplexní vyhledávání výraz, který obsahuje výraz filtru:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Obě dotazy jsou platné, ale hello druhou je vyšší, pokud hledáte bez motely s parkovací v Praze.
-   první dotaz Hello závisí na těchto určitá slova se uvedených nebo není uveden v polí s řetězcem jako název, popis a žádné jiné pole obsahující prohledávatelná data.
-   Hello druhý dotaz hledá přesné shody na strukturovaných dat a je pravděpodobně toobe mnohem přesnější.

V aplikacích, které zahrnují Fasetové navigace, ujistěte se, že je přiložena akce každého uživatele v struktuře Fasetové navigace zúžení výsledků vyhledávání. toonarrow výsledky, použijte výraz filtru.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Sestavení aplikace Fasetové navigace
V kódu aplikace, vytvoří žádost o vyhledávání hello implementujete Fasetové navigace s Azure Search. Hello Fasetové navigace spoléhá na elementy ve vašem schématu, které jste definovali dříve.

Předdefinované v indexu vyhledávání je hello `Facetable [true|false]` indexu atribut, nastavení na vybrané pole tooenable nebo zakázat jejich použití ve struktuře Fasetové navigace. Bez `"Facetable" = true`, pole nelze použít v omezující vlastnost navigace.

Hello prezentační vrstvy do vašeho kódu nabízí hello uživatelské prostředí. By měl zobrazit seznam hello základní částí hello Fasetové navigace, například hello štítek, hodnoty, zaškrtněte políčka a počet hello. Hello REST API služby Azure Search je nezávislá na platformě, takže použijte jakoukoli jazyk a platformy, které chcete. Hello důležité je, že tooinclude prvky uživatelského rozhraní, které podporují přírůstkové aktualizace, s aktualizovaný stav uživatelského rozhraní je vybrán každý další omezující vlastnosti. 

V době dotazů kódu aplikace vytvoří žádost, která zahrnuje `facet=[string]`, parametr požadavek, který poskytuje hello pole toofacet podle. Dotaz může mít více omezující, jako například `&facet=color&facet=category&facet=rating`, každé z nich oddělených ampersand (&) znaků.

Kód aplikace také musí vytvořit `$filter` výraz toohandle hello klikněte na události v Fasetové navigace. A `$filter` snižuje hello výsledky hledání, pomocí hodnoty omezující vlastnost hello jako kritéria filtru.

Služba Azure Search vrátí výsledky hledání hello, založené na jeden nebo více výrazů, které zadáte, společně s struktuře Fasetové navigace toohello aktualizace. Ve službě Azure Search Fasetové navigace je konstrukce jedna úroveň, s hodnotami omezující vlastnost, počty a kolik výsledků, které se nacházejí pro každé z nich.

V následujících částech hello, můžeme provést podrobnější o tom, jak toobuild každou část.

<a name="buildindex"></a>

## <a name="build-hello-index"></a>Sestavení indexu hello
Používání omezujících vlastností je povolena na základě pole pole v indexu hello prostřednictvím tohoto atributu indexu: `"Facetable": true`.  
Všechny typy pole, které by mohly být použity pravděpodobně v Fasetové navigace jsou `Facetable` ve výchozím nastavení. Zahrnout tyto typy polí `Edm.String`, `Edm.DateTimeOffset`, a všechny hello číselné pole typů (v podstatě všechny typy polí jsou facetable (kategorizovatelné) s výjimkou `Edm.GeographyPoint`, který nelze použít v Fasetové navigace). 

Při vytváření indexu, je vhodné, Fasetové navigace pro pole, která má být nikdy použit jako omezující vlastnost tooexplicitly zapněte používání omezujících vlastností vypnuté.  Konkrétně polí s řetězcem hodnoty typu singleton, jako je například ID nebo produktu, název, musí být nastavená příliš`"Facetable": false` tooprevent jejich náhodnému (a neúčinná) použít v Fasetové navigace. Zapnutí používání omezujících vlastností vypnout kde nepotřebujete ho pomáhá udržet co nejmenší velikost hello hello indexu a obvykle zvyšuje výkon.

Toto je součástí hello schéma pro hello ukázkové úlohy portál ukázkovou aplikaci, oříznut některé atributy tooreduce hello velikosti:

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { “name”: "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { “name”: "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { “name”: "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

Jak je vidět ve schématu ukázkový text hello, `Facetable` je vypnuté pro pole řetězce, které by se neměla používat jako omezující, jako je třeba ID hodnoty. Zapnutí používání omezujících vlastností vypnout kde nepotřebujete ho pomáhá udržet co nejmenší velikost hello hello indexu a obvykle zvyšuje výkon.

> [!TIP]
> Jako osvědčený postup zahrnují hello úplnou sadu atributů index pro každé pole. I když `Facetable` je ve výchozím skoro všech polí, účelově nastavení každý atribut vám může pomoct pečlivě promyslete hello důsledků každé rozhodnutí schématu. 

<a name="checkdata"></a>

## <a name="check-hello-data"></a>Zkontrolujte, zda text hello data
Hello kvality dat. má přímý vliv na tom, jestli bude realizována hello Fasetové navigace struktura podle očekávání. Ovlivní také hello snadné vytváření filtrů tooreduce hello výslednou sadu.

Pokud chcete toofacet Brand nebo ceny, musí každý dokument obsahovat hodnoty pro *značka* a *ProductPrice* které jsou platné, konzistentní a produktivitu jako možnost filter.

Tady je několik připomenutí co tooscrub pro:

* Pro každé pole, které chcete toofacet podle položte si zda obsahuje hodnoty, které jsou vhodné jako filtry řízené samotným vyhledávání. Hello hodnoty by měly být krátké, popisný a dostatečně odlišný toooffer zrušte výběr mezi konkurenční možnosti.
* Pravopisné chyby nebo téměř odpovídající hodnoty. Pokud jste omezující vlastnost barvy a hodnoty polí obsahovat oranžová a Ornage (chybné), obě by vyzvednutí omezující vlastnost podle pole barev hello.
* Smíšená případu textu můžete také způsobí zmatek v Fasetové navigace s oranžová a oranžová zobrazí jako dvě různé hodnoty. 
* Verze jednoho a množném čísle stejnou hodnotu může mít za následek samostatné omezující vlastnosti pro každou hello.

Si lze představit, péči k přípravě hello dat je zásadní aspekt efektivní Fasetové navigace.

<a name="presentationlayer"></a>

## <a name="build-hello-ui"></a>Sestavení hello uživatelského rozhraní
Práce zpět z hello prezentační vrstvy můžete nápovědy odkrýt požadavky, které může být načteni jinak a zjistěte, jaké funkce jsou nezbytné toohello možností vyhledávání.

Z hlediska Fasetové navigace zobrazí hello Fasetové navigace struktura stránku web nebo aplikaci, zjistí vstupu uživatele na stránku hello a vloží elementy hello změnit. 

Pro webové aplikace se AJAX obvykle používá v prezentační vrstvě hello protože umožňuje toorefresh přírůstkové změny. Můžete také použít rozhraní ASP.NET MVC nebo jakoukoli jinou vizualizaci platformu, zda se může připojit tooan služby Azure Search pomocí protokolu HTTP. Ukázková aplikace Hello odkazuje v tomto článku – hello **Azure Search úlohy portál ukázku** – se stane toobe aplikaci ASP.NET MVC.

V ukázce hello Fasetové navigace je integrovaná hello stránky s výsledky hledání. v následujícím příkladu, které jsou převzaty z hello Hello `index.cshtml` stránka s výsledky souboru hello ukázkové aplikace, zobrazí hello statické HTML struktury pro zobrazení Fasetové navigace na hello vyhledávání. Hello seznam omezující vlastnosti je sestavit nebo znovu sestavit dynamicky při odeslání hledaný termín, nebo zaškrtněte nebo zrušte omezující vlastnost.

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

Následující fragment kódu z hello Hello `index.cshtml` stránky dynamicky vytvoří hello HTML toodisplay hello první podmínka, název firmy. Podobné funkce jako dynamicky sestavení hello HTML pro hello jiné omezující vlastnosti. Každý omezující vlastnosti má popisek a počet, který zobrazí hello počet položek pro tento výsledek omezující vlastnost nalezena.

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> Při návrhu hello stránky s výsledky hledání, mějte na paměti tooadd mechanismus pro vymazání omezující vlastnosti. Pokud přidáte zaškrtávací políčka, snadno uvidíte jak tooclear hello filtry. Pro jiné rozložení může být nutné vzor s popisem cesty nebo další tvůrčí přístup. Například v hello úlohy vyhledávání portál ukázkovou aplikaci, můžete kliknout na hello `[X]` po omezující vlastnost vybrané omezující vlastnost tooclear hello.

<a name="buildquery"></a>

## <a name="build-hello-query"></a>Vytvoření dotazu hello
Hello kód, který můžete psát pro tvorbu dotazů musí určovat, že všechny části platný dotaz, včetně vyhledávacích výrazech, omezující vlastnosti, filtrů, vyhodnocování profilů – nic používá tooformulate žádost. V této části nám prozkoumat, kde omezující vlastnosti začlenit do dotazu, a jak se používají filtry s omezujícími vlastnostmi toodeliver snížené sada výsledků.

Všimněte si, že omezující vlastnosti jsou nedílnou součástí v této ukázkové aplikaci. Hello vyhledáváním v hello portál ukázkové úlohy je navržen na Fasetové navigace a filtry. Hello viditelného umístění Fasetové navigace na stránce hello ukazuje její význam. 

Příkladem je často toobegin vhodné místo. v následujícím příkladu, které jsou převzaty z hello Hello `JobsSearch.cs` souboru sestavení požadavek, který vytvoří omezující vlastnosti navigace na základě obchodní název, umístění, typ účtování a minimální mzda. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Parametr dotazu omezující vlastnosti nastavena tooa pole a v závislosti na typu dat hello, lze nastavit další parametry podle seznamu odděleného čárkami, který zahrnuje `count:<integer>`, `sort:<>`, `interval:<integer>`, a `values:<list>`. Seznam hodnot je podporována pro číselná data při nastavování rozsahů. V tématu [vyhledávání dokumentů (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798927.aspx) podrobnosti o použití.

Společně s omezující má požadavek hello formulovali aplikací také sestavit filtry toonarrow dolů hello sadu candidate dokumentů na základě výběru hodnota omezující vlastnosti. Pro úložiště kolo Fasetové navigace poskytuje různá vodítka tooquestions jako *jaké typy kol, výrobců a barvy jsou k dispozici?*. Filtrování odpovědi na dotazy jako *red které přesný kol se Horská kola, v tomto cena rozsah?*. Po kliknutí na tlačítko "Red" tooindicate, která má být zobrazena pouze Red produktů, hello další dotaz hello aplikace odešle zahrnuje `$filter=Color eq ‘Red’`.

Následující fragment kódu z hello Hello `JobsSearch.cs` stránky přidá hello vybraný název firmy toohello filtr, pokud vyberete hodnotu z omezující hello firmy název vlastnosti.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>Tipy a osvědčené postupy

### <a name="indexing-tips"></a>Tipy pro indexování
**Pokud nepoužijete vyhledávací pole zlepšit efektivitu indexu**

Pokud vaše aplikace používá výhradně Fasetové navigace (tedy bez vyhledávacího pole), můžete označit hello pole jako `searchable=false`, `facetable=true` tooproduce kompaktnější index. Kromě toho indexování proběhne pouze celý omezující vlastnosti hodnoty, ovšem se žádné dělením slov nebo indexování hello součásti aplikace word hodnoty.

**Zadejte pole, která lze použít jako omezující vlastnosti**

Odvolat tuto hello schéma indexu hello určuje pole, která jsou k dispozici toouse jako omezující vlastnost. Za předpokladu, že je pole facetable (kategorizovatelné), hello dotazu určuje, které pole toofacet podle. Hello pole, podle kterého jsou používání omezujících vlastností poskytuje hello hodnoty, které se zobrazí pod popiskem hello. 

Hello hodnoty, které se zobrazí pod každý popisek se načítají z indexu hello. Například pokud hello omezující vlastnost pole je *barva*, hello hodnoty, které jsou k dispozici pro další filtrování jsou hello hodnot pro toto pole - červená, černé a tak dále.

Číselné a data a času pouze hodnoty, je možné explicitně nastavit hodnoty na hello omezující vlastnosti pole (například `facet=Rating,values:1|2|3|4|5`). Seznam hodnot je povolena pro tyto pole typů toosimplify hello oddělení omezující vlastnost výsledky do sousedních rozsahy (buď na základě číselných hodnot nebo časových období rozsahy). 

**Ve výchozím nastavení může mít pouze jednu úroveň Fasetové navigace** 

Jak jsme uvedli, neexistuje žádné přímé podpory pro vnoření omezující vlastnosti v hierarchii. Ve výchozím nastavení Fasetové navigace ve službě Azure Search podporuje pouze jednu úroveň filtry. Ale existuje alternativní řešení. Můžete kódovat omezující vlastnost hierarchická struktura v `Collection(Edm.String)` v jedné položce bodu na hierarchii. Implementace tohoto řešení je nad rámec hello rámec tohoto článku. 

### <a name="querying-tips"></a>Dotazování tipy
**Ověřte pole**

Pokud vytvoříte hello seznam omezující vlastnosti dynamicky podle nedůvěryhodné uživatelský vstup, ověřte, zda jsou platná hello názvy polí Fasetové hello. Nebo řídicí názvy hello při vytváření adres URL pomocí `Uri.EscapeDataString()` v rozhraní .NET, nebo ekvivalentní v vaši platformu volba hello.

### <a name="filtering-tips"></a>Filtrování tipy
**Zvýšit přesnost vyhledávání s filtry**

Použití filtrů. Pokud byste tedy spoléhat na vrátí právě vyhledávacích výrazech samostatně, rozklad by mohlo způsobit toobe dokumentu, který nemá hodnotu přesné omezující vlastnost hello v některém z jeho polí.

**Zvýšení výkonu vyhledávání s filtry**

Filtry zúžit hello sadu dokumenty kandidáta pro hledání a vyloučit z hodnocení. Pokud máte velké sady dokumentů, často pomocí selektivní omezující vlastnost procházení poskytuje lepší výkon.
  
**Filtrovat jenom hello Fasetové pole**

V Fasetové procházení, obvykle je vhodné tooonly obsahovat dokumenty, které obsahují hodnota omezující vlastnosti hello v určitém poli (Fasetové), není kdekoli napříč všechna prohledatelná pole. Přidání filtru posiluje hello cílové pole přesměrováním hello služby toosearch hello Fasetové pole pro odpovídající hodnotu.

**Trim omezující vlastnost výsledků s další filtry**

Omezující vlastnost výsledky jsou dokumenty nalezen ve výsledcích hledání hello, které odpovídají termín omezující vlastnosti. V následující ukázka ve výsledcích hledání hello *cloud computing*, 254 položky mají také *interní specifikace* jako typ obsahu. Položky nejsou nezbytně vzájemně vylučují. Pokud položku splňuje hello kritéria obou filtrů, se počítá v každé z nich. Tato duplikace je možné při používání omezujících vlastností na `Collection(Edm.String)` pole, které jsou často používá tooimplement označování dokumentu.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Obecně platí Pokud zjistíte, že výsledky omezující vlastnost konzistentně příliš velký, doporučujeme přidat další filtry toogive uživatelé další možnosti pro zúžení hello vyhledávání.

### <a name="tips-about-result-count"></a>Tipy k počet výsledků

**Limit hello počet položek v hello omezující vlastnosti navigace**

Pro každé Fasetové pole v hello navigačním stromu je výchozí limit 10 hodnot. Toto výchozí nastavení má smysl pro navigační struktury, protože udržuje hello hodnoty seznamu tooa spravovat velikost. Můžete přepsat výchozí hello přiřazením toocount hodnotu.

* `&facet=city,count:5`Určuje, že pouze hello prvních pět města najít v horní části hello seřazeny výsledky se vrátí jako výsledek omezující vlastnosti. Vezměte v úvahu ukázkový dotaz s hledaný termín "letiště" a odpovídá 32. Pokud dotaz hello Určuje `&facet=city,count:5`jen měst prvních pět jedinečný hello s hello jsou součástí většina dokumenty ve výsledcích hledání hello hello omezující vlastnost výsledků.

Všimněte si hello odlišit omezující vlastnost výsledky a výsledky hledání. Výsledky hledání jsou všechny hello dokumenty, které odpovídají dotazu hello. Omezující vlastnost výsledky jsou hello odpovídá pro každou hodnotu omezující vlastnosti. V příkladu hello výsledky hledání obsahují města názvy, které nejsou v seznamu klasifikace omezující vlastnost hello (v našem příkladu 5). Po vymazání omezující vlastnosti, nebo zvolte jiné omezující vlastnosti kromě města se vidět výsledky, které jsou odfiltrována prostřednictvím Fasetové navigace. 

> [!NOTE]
> Hovoříte o `count` při více než jeden typ může být matoucí. Hello následující tabulka nabízí stručné souhrnné informace o použití hello termín v rozhraní API služby Azure Search, ukázky kódu a dokumentaci. 

* `@colorFacet.count`<br/>
  V kódu prezentace měli byste vidět počet parametrů na omezující vlastnost hello, použité toodisplay hello počet výsledků omezující vlastnosti. Ve výsledcích omezující vlastnost určuje počet hello počet dokumentů, které odpovídají na hello omezující vlastnost termín nebo rozsah.
* `&facet=City,count:12`<br/>
  V dotazu omezující vlastnosti můžete nastavit počet tooa hodnotu.  Hello výchozí hodnota je 10, ale můžete nastavit vyšší nebo nižší. Nastavení `count:12` získá hello nejvyšší 12 odpovídá ve výsledcích omezující vlastnosti podle počtu dokumentu.
* "`@odata.count`"<br/>
  V odpovědi na dotaz hello tato hodnota označuje hello počet odpovídajících položek ve výsledcích hledání hello. V průměru je větší než součet hello výsledků omezující vlastnost kombinaci z důvodu přítomnosti toohello položky, které odpovídají hello hledaný termín, ale obsahuje žádné odpovídající hodnota omezující vlastnosti.

**Získat ve výsledcích omezující vlastnost**

Když přidáte Fasetové dotazu filter tooa, můžete příkaz omezující vlastnost hello tooretain (například `facet=Rating&$filter=Rating ge 4`). Technicky omezující vlastnost = hodnocení není vyžadována, ale bude vrací hello počty hodnoty omezující vlastnosti pro hodnocení, 4 a vyšší. Například pokud jste na tlačítko "4" a hello dotaz obsahuje filtr pro větší nebo rovna příliš "4", počty se vrátí pro každou hodnocení, která je 4 a vyšší.  

**Ujistěte se, že můžete získat přesné omezující vlastnost**

Za určitých okolností, můžete zjistit, že počty omezující vlastnost neodpovídají hello sad výsledků dotazu (viz [Fasetové navigace ve službě Azure Search (příspěvek na fóru)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Počty omezující vlastnost může být nesprávné kvůli architektura toohello horizontálního dělení. Každý index vyhledávání má víc horizontálních oddílů a každý horizontálního oddílu sestavy omezující vlastnosti hlavních hello podle počtu dokumentu, který je pak zkombinované do jednoho výsledku. Pokud některé horizontálních oddílů mnoho odpovídající hodnoty, zatímco ostatní uživatelé mají méně, můžete zjistit, zda některé hodnoty omezující vlastnost chybí a v části započítány ve výsledcích hello.

I když toto chování změnit kdykoli, pokud dojde k tomuto chování dnes, můžete alternativně vyřešit ji pomocí uměle nafouknutí počet hello:<number> tooa velký počet tooenforce Úplné generování sestav z každé horizontálního oddílu. Pokud hello hodnota počtu: je větší než nebo rovna toohello počet jedinečných hodnot v poli hello, že je zaručeno přesné výsledky. Ale pokud jsou vysoké počty dokumentů, je snížení výkonu, proto tuto možnost použijte uvážlivě.

### <a name="user-interface-tips"></a>Tipy pro uživatelské rozhraní
**Přidání popisky pro každé pole v omezující vlastnosti navigace**

Popisky jsou obvykle definovány v hello HTML nebo formátu (`index.cshtml` v ukázkové aplikaci hello). Neexistuje žádné rozhraní API ve službě Azure Search pro omezující vlastnosti navigace popisky nebo další metadata.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Filtr na základě rozsahu
Používání faset přes rozsah hodnot je běžné aplikace požadavek vyhledávání. Rozsahy jsou podporovány pro číselná data a hodnoty DateTime. Další informace o jednotlivých přístupů v [vyhledávání dokumentů (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Vyhledávání systému Azure zjednodušuje vytváření rozsahu tím, že poskytuje dva přístupy pro výpočty rozsah. Pro oba přístupy vytvoří Azure Search hello příslušné rozsahy zadané vstupy hello, které jste zadali. Například pokud zadáte hodnoty rozsahu 10 | 20 | 30, automaticky vytvoří rozsahy 0-10, 10 20, 20-30. Aplikace můžete volitelně odebrat žádné intervaly, které jsou prázdné. 

**Způsob 1: Pomocí parametru interval hello**  
tooset omezující cenu v přírůstcích po 10, zadali byste:`&facet=price,interval:10`

**Způsob 2: Použijte seznam hodnot**  
Číselná data můžete seznam hodnot.  Vezměte v úvahu rozsah hello omezující vlastnosti pro `listPrice` pole, vykresluje následujícím způsobem:

  ![Seznam ukázek hodnoty][5]

toospecify rozsah omezující vlastnosti, jako v předchozím snímku obrazovky hello hello použijte seznam hodnot:

    facet=listPrice,values:10|25|100|500|1000|2500

Každý rozsah je sestaven pomocí 0 jako počáteční bod, hodnotu ze seznamu hello jako koncový bod a pak oříznut hello předchozí rozsah toocreate diskrétní intervalů. Vyhledávání systému Azure se provádí tyto věci jako součást Fasetové navigace. Nemáte toowrite kód pro vytvoření struktury v každém intervalu.

### <a name="build-a-filter-for-a-range"></a>Vytvořit filtr pro rozsah
na základě rozsahu vyberete toofilter dokumenty, můžete použít hello `"ge"` a `"lt"` filtrovat operátory ve dvou částech výraz, který definuje koncové body hello hello rozsahu. Například pokud se rozhodnete hello rozsah 10 až 25 `listPrice` pole, bude filtr hello `$filter=listPrice ge 10 and listPrice lt 25`. V ukázkovém kódu hello, výrazu filtru hello používá **priceFrom** a **priceTo** parametry tooset hello koncové body. 

  ![Dotazy na rozsah hodnot][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Filtrovat na základě vzdálenosti
Běžné toosee filtry, které pomáhají zvolit úložiště, restaurace nebo cíl v závislosti na jeho blízkosti tooyour aktuálního umístění. Tento typ filtru může vypadat například Fasetové navigace, je právě filtru. Jsme zmínili ji sem pro těch, které jste, který hledáte konkrétně implementace Rady, jak tento problém konkrétního návrhu.

Ve službě Azure Search jsou dvě funkce geoprostorové **geo.distance** a **geo.intersects**.

* Hello **geo.distance** funkce vrátí hello vzdálenost v kilometrech mezi dva body. Jeden bod je pole a druhá je konstanta předanou v rámci hello filtru. 
* Hello **geo.intersects** funkce vrátí hodnotu true, pokud k danému bodu je v rámci dané mnohoúhelníku. bod Hello je pole a mnohoúhelníku hello je zadán jako seznam souřadnice předanou v rámci hello filtru konstantní.

Můžete najít příklady filtru v [syntaxe výrazu OData (Azure Search)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>

## <a name="try-hello-demo"></a>Zkuste ukázku hello
Hello Azure Search úlohy portál ukázku obsahuje příklady hello odkazuje v tomto článku.

-   Zobrazit a otestovat hello pracovní ukázkový online v [Azure Search úlohy portál ukázku](http://azjobsdemo.azurewebsites.net/).

-   Stáhnout hello kód z hello [Azure-Samples úložišti na Githubu](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Při práci s výsledky hledání, podívejte se na adresu URL hello změny v dotazu konstrukce. Tato aplikace se stane tooappend omezující vlastnosti toohello identifikátoru URI při výběru každé z nich.

1. mapování funkce toouse hello hello ukázkovou aplikaci, získat klíč mapy Bing z hello [Dev Center pro mapy Bing](https://www.bingmapsportal.com/). Vložení prostřednictvím hello existující klíč v hello `index.cshtml` stránky. Hello `BingApiKey` nastavení v hello `Web.config` soubor se nepoužije. 

2. Spusťte aplikaci hello. Volitelné prohlídku hello nebo zavřít dialogové okno hello.
   
3. Zadejte hledaný termín, jako je například "analytik" a klikněte na ikonu hledání hello. Hello dotazu se provede rychle.
   
   Struktura Fasetové navigace, vrátí se výsledky hledání hello. Ve výsledcích hello hledání hello Fasetové navigace struktura zahrnuje počty pro každý výsledek omezující vlastnosti. Žádné omezující vlastnosti jsou vybrané, takže jsou vráceny všechny odpovídající výsledky.
   
   ![Výsledky hledání před výběrem omezující vlastnosti][11]

4. Klikněte na název firmy, umístění nebo minimální mzda. Omezující vlastnost měla hodnotu null u hello počáteční hledání, ale přijmou na hodnoty jsou výsledky hledání hello oříznut položek, které už neodpovídá.
   
   ![Výsledky hledání po výběru omezující vlastnosti][12]

5. tooclear hello Fasetové dotaz tak, aby můžete zkusit jiný dotaz chování, klikněte na tlačítko hello `[X]` po výběru hello omezující vlastnosti tooclear hello omezující vlastnosti.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Další informace
Kukátko [Deep Dive informace o službě Azure Search](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). V 45:25, je ukázku na postupy tooimplement omezující vlastnosti.

Pro další statistiky na zásadách návrhu pro fasetovou navigaci doporučujeme hello následující odkazy:

* [Návrh a Fasetové vyhledávání](http://www.uie.com/articles/faceted_search/)
* [Vzory návrhu: Fasetové navigace](http://alistapart.com/article/design-patterns-faceted-navigation)


<!--Anchors-->
[How toobuild it]: #howtobuildit
[Build hello presentation layer]: #presentationlayer
[Build hello index]: #buildindex
[Check for data quality]: #checkdata
[Build hello query]: #buildquery
[Tips on how toocontrol faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/azure-search-faceting-example.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png
[11]: ./media/search-faceted-navigation/faceted-search-before-facets.png
[12]: ./media/search-faceted-navigation/faceted-search-after-facets.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

