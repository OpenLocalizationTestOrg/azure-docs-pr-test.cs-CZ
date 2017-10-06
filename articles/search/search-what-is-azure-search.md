---
title: aaaWhat je Azure Search | Microsoft Docs
description: "Služba Azure Search je plně spravovaná hostované cloudové vyhledávací službu. Další informace v přehledu této funkce."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/26/2017
ms.author: ashmaka
ms.openlocfilehash: b38a7e93a7f0e0d5f8a3779858032271b3883778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-search"></a>Co je Azure Search?
Služba Azure Search je řešení vyhledávání jako služby cloud, které poskytuje vývojářům rozhraní API a nástroje pro přidání bohaté vyhledáváním přes data v aplikacích pro webové, mobilní a enterprise.

Funkce je zveřejněna prostřednictvím jednoduchou [REST API](/rest/api/searchservice/) nebo [.NET SDK](search-howto-dotnet-sdk.md) který zakrývá složitost vyplývajících hello technologie vyhledávání. V přidání tooAPIs hello portál Azure poskytuje správy a podpory při vytváření prototypu. Microsoft spravuje infrastruktury a dostupnost.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Souhrn funkcí

| Kategorie | Funkce |
|----------|----------|
|Fulltextové vyhledávání a analýza textu | [**Fulltextové vyhledávání** ](search-lucene-query-architecture.md) je primární případ použití pro většinu aplikací na základě hledání. Dotazy můžete formulovali pomocí podporovaných syntaxe: <br/><br/>[**Jednoduchá syntaxe dotazů**](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), který nabízí logické operátory, operátory hledání fráze, přípony operátory, operátorů.<br/><br/>[**Syntaxe dotazů Lucene** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) nabízí všechny jednoduchý dotaz podporu plus přibližné vyhledávání, vyhledávání blízkých výrazů podmínek zvýšení skóre a regulární výrazy.| 
| Integrace dat | Indexů Azure Search přijímat data z jakéhokoli zdroje, zadaný je odeslán jako datovou strukturu JSON. <br/><br/> Volitelně můžete pro podporované zdroje dat v Azure, můžete použít [ **indexery** ](search-indexer-overview.md) tooautomatically procházení [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-documentdb.md), nebo [úložiště objektů Azure Blob](search-howto-indexing-azure-blob-storage.md) toosync indexu vyhledávání je obsahu s primární data store. Můžete provádět Azure Blob indexery *hádání dokumentu toho* pro [indexování formáty souborů hlavní](search-howto-indexing-azure-blob-storage.md), včetně dokumentů Microsoft Office, PDF a HTML. |
| Analýza vyhledávání | [**Vlastní lexikální analyzátorů** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) jsou k dispozici pro komplexní vyhledávací dotazy výslovnosti porovnáváním a regulární výrazy. |
| Podpora jazyků | [**Analyzátory jazyka** ](https://docs.microsoft.com/rest/api/searchservice/language-support) z Lucene, jakož i Microsoft je přirozeného jazyka procesorů, které jsou k dispozici v různých jazyků 56 toointelligently popisovač pro specifický jazyk linguistics včetně příkaz časy, pohlaví, nestandardní množném čísle podstatná jména (například "myši' oproti 'myši'), zrušte vícesložkovém word, dělení slov (pro jazyky, bez mezer) a další. |
| Geograficky vyhledávání | Služba Azure Search inteligentně zpracovává, filtrů a zobrazí zeměpisné umístění. Umožňuje uživatelům tooexplore dat podle hello blízko fyzické umístění tooa výsledků hledání. [Přehrát toto video](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) nebo [Zkontrolujte tato ukázka](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) toolearn Další. |
| Funkce uživatelského rozhraní | [**Hledání návrhy** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) lze povolit pro našeptávání dotazů v panelu vyhledávání. Skutečné dokumenty v indexu jsou navrhované jako uživatelé zadání částečné vyhledávání. <br/><br/>[**Fasetové navigace** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) je povolená prostřednictvím parametru jeden dotaz. Služba Azure Search vrací strukturu Fasetové navigace, který můžete použít jako hello kódu seznam kategorií pro řízené samotným filtrování (například toofilter položky katalogu cena range nebo brand). <br/><br/> [**Filtry** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) lze použít tooincorporate Fasetové navigace do uživatelského rozhraní aplikace, zvýšit formulování dotazu a filtru na základě zadaného uživatelem nebo vývojáře kritérií. Vytvoření filtrů pomocí syntaxe hello OData.<br/><br/> [**Zvýrazňování** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) platí tooa atting visual formuláře odpovídající – klíčové slovo ve výsledcích hledání. Můžete vybrat pole, která vrátí zvýrazněná fragmenty.<br/><br/>[**Řazení** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) je poskytovat několik polí prostřednictvím hello Indexujte schéma a poté přepínat stav v době dotazů s parametrem hledání jednoduchého.<br/><br/> [**Stránkování** ](search-pagination-page-layout.md) a omezení výsledky hledání je jednoduchá s ovládacím prvkem hello jemně přizpůsobená Azure Search nabízí než výsledky hledání.  
| Důležitost | [**Jednoduché vyhodnocování** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) je klíčovou výhodou Azure Search. Vyhodnocování profily jsou použité toomodel relevance jako funkce hodnot v hello dokumenty sami. Například může být vhodné novější produkty nebo slevou produkty tooappear vyšší ve výsledcích hledání hello. Můžete také vytvořit vyhodnocování profilů pomocí značek pro individuální vyhodnocování podle preferencí vyhledávání zákazníka jste sledovat a uloženy odděleně. |
| Monitorování a vytváření sestav | [**Analýza provozu vyhledávání** ](search-traffic-analytics.md) jsou shromážděná a analyzovaných toounlock přehledy z co uživatelé psaní do vyhledávacího pole hello. <br/><br/>Metriky pro dotazy na za sekundu, latence a omezení zachycení a uvedený v portálu stránky s bez další nezbytné konfigurace. Můžete také snadno monitorování index a počty dokumentů, aby kapacity můžete upravit podle potřeby. Další informace najdete v tématu [služby správy](search-manage.md) |
| Nástroje pro vytváření prototypů a kontroly | Hello portálu, můžete použít hello [ **Průvodce importem dat** ](search-import-data-portal.md) tooconfigure indexery indexu návrháře toostand až indexu, a [ **Průzkumník služby Search** ](search-explorer.md) tootest dotazy a upřesněte profily vyhodnocování. Můžete také otevřít všechny index tooview jeho schématu. |
| Infrastruktura | Hello **vysokou dostupností platformy** zajistí prostředí služby velmi spolehlivé vyhledávání. Při změně měřítka správně, [Azure Search nabízí SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Plně spravované a škálovatelné** jako řešení začátku do konce, vyžaduje Azure Search absolutně žádné správu infrastruktury. Vaše služba může být šité na míru tooyour potřebám podle škálování v dvěma rozměry toohandle další úložiště dokumentů, vyšší zatížení dotazu nebo obojí.

## <a name="how-it-works"></a>Jak to funguje
### <a name="step-1-provision-service"></a>Krok 1: Zřizování služby
Můžete číselníku si služby Azure Search v hello [portál Azure](https://portal.azure.com/) nebo prostřednictvím hello [rozhraní API pro správu prostředků Azure](/rest/api/searchmanagement/). Můžete buď hello bezplatná služba sdílet s jiným odběratelům nebo [placené vrstvy](https://azure.microsoft.com/pricing/details/search/) , vyhrazuje vlastní prostředky, které jsou používány pouze služby. U placených úrovní je možné škálovat službu v dvě dimenze: 

- Přidejte toogrow repliky, které dotazu velkou kapacitu toohandle načte.   
- Přidejte úložiště toogrow oddíly pro další dokumenty. 

Podle dokumentu propustnost úložiště a dotaz zpracování samostatně, můžete nakalibrovat financování na základě požadavků produkční.

### <a name="step-2-create-index"></a>Krok 2: Vytvoření indexu
Předtím, než můžete nahrát vyhledávat obsah, je nutné zadat indexu Azure Search. Index je jako tabulku databáze, která obsahuje vaše data a může přijmout vyhledávací dotazy. Můžete definovat hello index schématu toomap tooreflect hello struktura hello dokumentů chcete toosearch, podobně jako toofields v databázi.

Schéma se dají vytvářet v hello Azure portálu nebo programově pomocí hello [.NET SDK](search-howto-dotnet-sdk.md) nebo [REST API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>Krok 3: Index dat
Po definování indexu, jste tooupload připravený obsah. Můžete použít push nebo pull modelu.

Hello pull model načítá data z externích zdrojů. Je podporován prostřednictvím *indexery* , zjednodušit a automatizovat aspektů přijímání dat, jako je například připojení k čtení a serializaci dat. [Indexery](/rest/api/searchservice/Indexer-operations) jsou k dispozici pro databázi Cosmos Azure, Azure SQL Database, úložiště objektů Blob Azure a SQL Server hostované ve virtuálním počítači Azure. Indexer pro můžete nakonfigurovat na vyžádání nebo naplánované aktualizace.

Hello push model je zajišťováno prostřednictvím hello SDK nebo rozhraní REST API, používá k odeslání aktualizované dokumenty tooan index. Nabízet data z prakticky libovolný datovou sadu hello formátu JSON. V tématu [přidání, aktualizace nebo odstranění dokumentů](/rest/api/searchservice/addupdate-or-delete-documents) nebo [jak toouse hello .NET SDK)](search-howto-dotnet-sdk.md) pokyny k načítání dat.

### <a name="step-4-search"></a>Krok 4: vyhledávání
Po naplnění indexu, můžete [vydávat vyhledávací dotazy](/rest/api/searchservice/Search-Documents) požadavky koncový bod služby tooyour pomocí jednoduchého HTTP pomocí rozhraní REST API nebo hello .NET SDK.

## <a name="how-it-compares"></a>Jak porovná

Zákazníci často požádejte jak [fulltextové vyhledávání ve službě Azure Search](search-lucene-query-architecture.md) porovná s [fulltextové vyhledávání](https://en.wikipedia.org/wiki/Full_text_search) ve svých produktech databáze. Naše odpovědi je, že jsou možnosti jazyka Azure Search bohatší a flexibilnější, s podporou dotazů Lucene, analyzátory jazyka Lucene a společnosti Microsoft, vlastní analyzátory pro výslovnosti nebo jiné speciální vstupy a hello možnost toomerge dat z víc zdrojů v indexu vyhledávání hello. 

Jiný bod důraz je, že hledání řešení adres hello celý vyhledáváním. Například můžete vlastní vyhodnocování výsledků, Fasetové navigace řízené samotným filtrování, počtu zvýraznění a návrhy typeahead dotazu. 

Nástroje pro monitorování a pochopení aktivit dotaz můžete také dvoufaktorového do rozhodnutí řešení vyhledávání. Například Azure Search podporuje [Analýza provozu vyhledávání](search-traffic-analytics.md) pro metriky pro interaktivní kurz, horní vyhledávání, hledání bez kliknutí a tak dále. V produktech, kde vyhledávání je doplněk, budete pravděpodobně toofind tyto funkce.

Využití prostředků je potřeba vzít v úvahu. Vyhledávání v přirozeném jazyce, je často výpočetně náročné. Některé z našich zákazníků se sníženou zátěží tooAzure operace vyhledávání vyhledávání způsob toopreserve počítač prostředky pro zpracování transakcí. Externího vyhledáváním můžete snadno upravit škálování toomatch dotazu svazku.

Jakmile jste se rozhodli toogo s vyhrazenou search, vaše další rozhodnutí je mezi cloudovou službu nebo místní server. Cloudová služba je hello správnou volbou, pokud chcete [řešení na klíč s minimální režii a údržby a upravit škálování](#cloud-service-advantage).

V rámci cloudu zlepší hello několik poskytovatelů nabízejí funkce porovnatelný z hlediska směrný plán s fulltextové vyhledávání, hledání geograficky a hello možnost toohandle určitá úroveň nejednoznačnosti v hledání vstupy. Obvykle je [specializované funkce](#feature-drilldown), nebo hello snadné a celkové jednoduchost rozhraní API, nástrojů a správy, který určuje, co nejlépe vyhovovaly hello.

Mezi poskytovatelů cloudu Azure Search je nejsilnější pro fulltextové vyhledávání úlohy nad obsahu úložiště a databáze v Azure, pro aplikace, které závisí hlavně na hledání pro načítání informací o i obsahu navigace. Klíče síly patří:

+ Integrace dat Azure (pro indexaci) ve vrstvě indexování hello
+ Portál Azure pro centrální správy
+ Azure škálování, spolehlivosti a dostupnosti špičkových
+ Jazykové a vlastní analýzy s analyzátory pro plnou fulltextového vyhledávání v 56 jazycích
+ [Základní funkce běžné toosearch zaměřená na aplikace](#feature-drilldown): vyhodnocování, používání faset, návrhy, synonyma, geograficky vyhledávání a další.

> [!Note]
> zdroje dat mimo Azure, zrušte zaškrtnutí toobe jsou plně podporované, ale závisí na metodika nabízené vyšší nároky na kód, nikoli indexery. Pomocí rozhraní API, můžete předat indexu Azure Search kolekce tooan žádné JSON dokumentů.

Mezi naše zákazníky zahrnují tyto možné tooleverage hello nejširší řadu funkcí ve službě Azure Search online katalogů, -obchodní programy a dokumentu zjišťování aplikací.

## <a name="rest-api--net-sdk"></a>Rozhraní REST API | .net SDK

Při portálu hello provádět mnoho úloh, Azure Search je určena pro vývojáře, kteří chtějí funkce vyhledávání toointegrate do stávající aplikace. Hello následující programovací rozhraní jsou k dispozici.

|Platforma |Popis |
|-----|------------|
|[REST](/rest/api/searchservice/) | Příkazy HTTP nepodporuje žádné programovací platformy a jazyk, včetně Xamarin, Java a JavaScript|
|[.NET SDK](search-howto-dotnet-sdk.md) | Obálka .NET pro hello REST API nabízí efektivní kódování v jazyce C# a hello ostatních jazyků spravovaného kódu cílení na rozhraní .NET Framework |

## <a name="free-trial"></a>Bezplatná zkušební verze
Předplatitelé služby Azure můžete [zřídit službu v úrovni Free hello](search-create-service-portal.md).

Pokud nejste odběratele, můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Získáte kredity pro vyzkoušení placených služeb Azure. Až se vyčerpáte, můžete zachovat hello účtu a použít [bezplatné služby Azure](https://azure.microsoft.com/free/). Platební karty se nikdy účtovat Pokud explicitně změnit nastavení a požádejte toobe účtovat.

Alternativně můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure. 

## <a name="how-tooget-started"></a>Způsob spuštění tooget

1. Vytvoření služby v hello [úroveň Free](search-create-service-portal.md).

2. Krok prostřednictvím jednoho nebo více následujících kurzech hello. 

  + [Jak toouse hello .NET SDK](search-howto-dotnet-sdk.md) ukazuje hello hlavní kroky ve spravovaném kódu.  
  + [Začínáme s hello REST API](https://github.com/Azure-Samples/search-rest-api-getting-started) ukazuje hello stejné kroky pomocí hello REST API.  
  + [Vytvoření první indexu hello portálu](search-get-started-portal.md) ukazuje integrované funkce indexování a prototypu na portál hello.   

Vyhledávací weby jsou běžné ovladače hello načítání informace v mobilních aplikacích, na webu hello a v úložištích firemní data. Služba Azure Search nabízí nástroje pro vytváření hledání prostředí podobné toothose na velké komerčních webů.

V tomto videu 9 minutu z manažer programu Cavanagh počítači zjistěte, jak integrace vyhledávací může hodit vaší aplikace. Krátký ukázky se týkají klíčových funkcí v Azure Search a jak vypadá obvyklý pracovní postup. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0 až 3 minut obsahuje klíčové funkce a případy použití.
+ 3 4 minut popisuje zřizování služby. 
+ 4 až 6 minut popisuje Import dat použít Průvodce toocreate indexu pomocí hello nemovitosti předdefinované datové sady.
+ 6 9 minut popisuje Průzkumník služby Search a různé dotazy.


