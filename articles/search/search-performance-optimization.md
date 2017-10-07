---
title: "aspekty výkonu a optimalizace vyhledávání aaaAzure | Microsoft Docs"
description: "Vyladění výkonu Azure Search a nakonfigurujte optimálního škálování"
services: search
documentationcenter: 
author: LiamCavanagh
manager: pablocas
editor: 
ms.assetid: 4d3cd864-29d2-4921-be0d-a3f1a819de46
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: 725149ba32773ab6b4c9d4b441424aba78db7c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Aspekty výkonu a optimalizace Azure Search
Skvělé vyhledáváním je klíče toosuccess pro mnoho mobilních a webových aplikací. Z nemovitosti ovlivní tooused car tržišť tooonline katalogů, rychlé vyhledávání a relevantní výsledky zkušeností zákazníků se hello. Tento dokument je určený toohelp zjistit osvědčené postupy pro jak tooget hello naplno Azure Search, zejména pro pokročilé scénáře s pokročilé požadavky pro škálovatelnost, podpora více jazyků nebo vlastní hodnocení.  Kromě toho tento dokument popisuje internals a popisuje přístupy, které efektivně fungoval v aplikacích reálného zákazníka.

## <a name="performance-and-scale-tuning-for-search-services"></a>Výkonu a škálování optimalizace pro vyhledávací služby
Snažíme se všechny moduly používané toosearch například Bing a Google a hello vysoký výkon, které nabízejí.  Výsledkem je když zákazníci používají webovým hledání nebo mobilních aplikací, bude očekávané podobné výkonové charakteristiky.  Při optimalizaci výkonu vyhledávání, jeden z přístupů nejlepší hello je toofocus na latenci, což je hello doba, jakou dotazu trvat toocomplete a vrátí výsledky.  Při optimalizaci pro vyhledávací latence je důležité pro:

1. Vyberte cílovou latencí (nebo maximální množství času), aby typické vyhledávací žádost neměla překročit toocomplete.
2. Vytvoření a otestování skutečné pracovní zátěže proti služby vyhledávání prostřednictvím realistické datovou sadu toomeasure tyto rychlosti latence.
3. Spustit s nízkou počet dotazů za sekundu (QPS) a pokračovat číslo hello tooincrease provést v hello testu, dokud latence klesne pod hello dotazů hello definované cílovou latencí.  Toto je toohelp důležité srovnávacího testu, které plánujete pro škálování využití růstem vaší aplikace.
4. Pokud je to možné, opakovaně používat připojení prostřednictvím protokolu HTTP.  Pokud používáte hello .NET SDK služby Azure Search, znamená to, by měl znovu použít instanci nebo [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) instance a pokud používáte hello REST API, by měl znovu použít jeden HttpClient.

Při vytváření těchto testovací úlohy, nejsou některé vlastnosti tookeep Azure Search v paměti:

1. Je možné toopush mnoho vyhledávací dotazy najednou, že bude přetížena hello prostředky, které jsou k dispozici ve službě Azure Search.  Pokud k tomu dojde, zobrazí se kódy odpovědí HTTP 503.  Z tohoto důvodu je nejlepší toostart pomocí různých rozsahů vyhledávání požadavky toosee hello rozdíly v latence sazby jako přidáte další požadavky hledání.
2. Odesílání obsahu tooAzure, které ovlivní vyhledávání hello celkový výkon a latence hello službu Azure Search.  Pokud očekáváte toosend dat při provádění hledání uživatele, je důležité tootake tímto zatížením do účtu v testy.
3. Ne každý vyhledávací dotaz provede na hello stejné úrovně výkonu.  Například dokument vyhledávání nebo vyhledávání návrhu bude obvykle rychlejší než dotaz s velký počet omezující vlastnosti a filtry.  Je nejlepší hello tootake různé dotazy, které očekáváte, že toosee do účet při vytváření testů.  
4. Varianta požadavky search je důležité, protože pokud průběžně provést hello stejné vyhledávat požadavky, ukládání do mezipaměti dat bude počáteční toomake výkonu vypadat lepší, než je může s dotazem více různorodých nastavit.

> [!NOTE]
> [Visual Studio zátěžové testování](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) je skutečně dobře tooperform vaše srovnávacího testu testuje jako umožňuje požadavky tooexecute HTTP jako potřebovali byste pro provádění dotazy pro Azure Search a umožňuje paralelního zpracování požadavků.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Škálování Azure Search sazby vysoké dotazu a omezení požadavků
Když obdrželi příliš mnoho požadavků omezenému nebo překročit sazby latence cílový z dotazu zvýšené zatížení, můžete si prohlédnout toodecrease latence sazby jedním ze dvou způsobů:

1. **Zvýšení replik:** replika je jako kopii dat umožňuje Azure Search tooload vyrovnávat požadavky proti hello více kopií.  Všechny Vyrovnávání zatížení a replikace dat mezi replik spravuje Azure Search a upravíte hello počet replik přidělené pro vaši službu kdykoli.  Můžete přidělit too12 replik ve standardní službu vyhledávání a 3 replik v základní vyhledávací službu. Repliky mohou být upravena buď z hello [portál Azure](search-create-service-portal.md) nebo [prostředí PowerShell](search-manage-powershell.md).
2. **Zvýšení úrovně vyhledávání:** vyhledávání systému Azure se dodává [počet vrstev](https://azure.microsoft.com/pricing/details/search/) a každý z těchto úrovní nabízí různé úrovně výkonu.  V některých případech může mít mnoho dotazy této hello vrstvě, jsou na neposkytuje počty dostatečně nízkou latencí, i v případě, že repliky podle toho se limit.  V takovém případě můžete tooconsider využití jeden z vyšší úrovně vyhledávání hello například hello Azure Search S3 vrstvy, který skvěle hodí pro scénáře s velkým počtem dokumentů a velmi vysoké dotazu úlohy.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Škálování Azure Search pomalé jednotlivých dotazů
Z jednoho dotazu trvá příliš dlouho toocomplete je další důvod, proč může být pomalé latence sazby.  V takovém případě přidání repliky nebude zlepšit latenci sazby.  Pro tento případ existují dvě možnosti k dispozici:

1. **Zvýšit oddíly** oddíl je mechanismus pro rozdělení dat mezi další prostředky.  Z tohoto důvodu při přidat druhý oddíl získá vaše data rozdělit na dvě.  Třetí oddíl rozdělí indexu na tři atd.  To má také hello účinek, že v některých případech pomalé dotazy bude rychlejší kvůli toohello paralelizace výpočtu.  Existuje několik příkladů kde jsme viděli tento paralelizace velmi dobře fungují s dotazy, které mají nízkou selektivitu dotazy.  Tento postup se skládá z dotazy, které odpovídají mnoho dokumenty nebo při používání omezujících vlastností musí tooprovide počty přes velké počty dokumentů.  Vzhledem k tomu, že je, že mnoho výpočtu tooscore hello důležitosti hello dokumentů nebo toocount hello počty dokumentů, přidání další oddíly může pomoci tooprovide další výpočty.  
   
   Může být maximálně 12 oddílů ve standardní službu vyhledávání a 1 oddíl v hello základní vyhledávací službu.  Oddíly mohou být upravena buď z hello [portál Azure](search-create-service-portal.md) nebo [prostředí PowerShell](search-manage-powershell.md).
2. **Limit vysokou kardinalitou pole:** vysokou kardinalitou pole se skládá z facetable (kategorizovatelné) nebo filtrování pole, které má velký počet jedinečných hodnot a v důsledku toho převezme velké množství prostředků toocompute výsledky.   Zkontrolujte například, nastavení produktu ID nebo popisu pole jako facetable (kategorizovatelné) nebo filtrování by pro vysokou kardinalitou protože většina hello hodnoty z toodocument dokumentu jsou jedinečné. Pokud je to možné, omezte počet hello pole vysokou kardinalitou.
3. **Zvýšení úrovně vyhledávání:** přesunutí nahoru tooa vyšší úroveň Azure Search může být jiný způsob tooimprove výkonu pomalé dotazů.  Každý vyšší úroveň také poskytuje rychlejší procesoru a víc paměti, což může mít kladný dopad na výkon dotazů.

## <a name="scaling-for-availability"></a>Škálování dostupnosti
Repliky nejen snížit latenci dotazu, ale můžete povolit také pro vysokou dostupnost.  S jednu repliku byste měli očekávat pravidelné výpadek z důvodu restartování tooserver po aktualizace softwaru nebo pro další události údržby, které se vyskytnou.  V důsledku toho je důležité tooconsider Pokud vaše aplikace vyžaduje vysokou dostupnost hledání (dotazy) a také zápisy (indexování událostí).  Služba Azure Search nabízí možnosti SLA na všech hello nabídky placeného vyhledávání s hello následující atributy:

* 2 replik pro vysokou dostupnost úloh jen pro čtení (dotazy)
* 3 nebo více replik pro vysokou dostupnost úloh pro čtení a zápis (dotazy a indexování)

Další informace o to, navštivte hello [smlouvy o úrovni služeb Azure Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Vzhledem k tomu, že repliky jsou kopií vašich dat s více replik umožňuje restartování počítače toodo Azure Search a údržby na jednu repliku současně současně dotazuje toocontinue toobe prováděné vůči hello další repliky.  Z tohoto důvodu je také nutné tooconsider, jak tento výpadek může mít vliv na hello dotazy, které teď máte toobe spustit pro jednu menší kopii dat hello.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Škálování zeměpisné polohy úloh a zadejte geografická redundance
Pro úlohy, zeměpisné polohy zjistíte, že uživatelé nacházející se daleko od hello datového centra je hostitelem služby Azure Search, budou mít vyšší latence sazby.  Z tohoto důvodu je často důležité toohave více hledání služeb v oblastech, které jsou v blíže uživatelé toothese blízkosti.  Služba Azure Search v oblastech aktuálně neposkytuje metoda automatické geografická replikace indexů Azure Search, ale existují způsoby, které lze použít pomocí kterých můžete nastavit jednoduché tooimplement tento proces a spravovat. Tyto jsou uvedeny v hello několik dalších částech.

Hello cílem zeměpisné polohy sadu služby vyhledávání je toohave dva nebo více indexů, k dispozici ve dvou nebo více oblastech, kde bude uživatel směrován toohello služby Azure Search, která poskytuje nejnižší latenci hello, jak je vidět v tomto příkladu:

   ![Mezi – karta služeb podle oblasti][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Udržování synchronizace dat mezi několika služby vyhledávání systému Azure
Existují dvě možnosti pro zachování vaší distribuované služby vyhledávání synchronizované které se skládají z buď pomocí hello [indexeru pro hledání Azure](search-indexer-overview.md) nebo hello Push rozhraní API (nazývaná také jen tooas hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Indexery vyhledávání systému Azure
Pokud používáte hello Azure Search Indexer, již importujete změny dat z centrální úložiště dat jako databázi SQL Azure nebo Azure Cosmos DB. Při vytváření nového hledání služby, můžete jednoduše taky vytvořit nové Indexer vyhledávání Azure pro tuto službu této toothis body stejné úložiště. Tímto způsobem vždy, když nové změny začalo hello úložiště dat, potom budou indexovat pomocí hello různé indexery.  

Tady je příklad, jak by vypadat této architektuře.

   ![Single – datový zdroj s kombinací služby a distribuované indexeru][2]

### <a name="push-api"></a>Push rozhraní API
Pokud používáte hello Push API služby Azure Search příliš[aktualizovat obsah v indexu Azure Search](https://docs.microsoft.com/rest/api/searchservice/update-index), můžete ponechat vaší různé služby vyhledávání synchronizované vynucením změny tooall vyhledávací služby vždy, když je nutná aktualizace.  Pokud to je důležité toomake zda toohandle případy, kdy služby search tooone aktualizace se nezdaří a jednu nebo více aktualizací úspěšné.

## <a name="leveraging-azure-traffic-manager"></a>Využívání Azure Traffic Manager
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) vám umožní tooroute požadavky toomultiple geograficky umístěné weby, které jsou pak zajišťované více služby vyhledávání systému Azure.  Jednou z výhod hello Traffic Manager je, že můžete testu tooensure Azure Search, že je k dispozici a směrování služby vyhledávání tooalternate uživatele v případě hello výpadku.  Kromě toho pokud jsou směrování požadavky search prostřednictvím webů Azure, Azure Traffic Manageru můžete tooload vyrovnávání případech, kdy hello web si ale není Azure Search.  Tady je příklad jaké hello architekturu, která využívá Traffic Manager.

   ![Mezi – karta služeb podle oblasti, s centrální Traffic Manager][3]

## <a name="monitoring-performance"></a>Sledování výkonu
Služba Azure Search nabízí hello možnost tooanalyze a monitorování hello výkon vaší služby prostřednictvím [Analýza provozu vyhledávání (STA)](search-traffic-analytics.md). Prostřednictvím STA můžete volitelně protokolovat operace hello jednotlivých hledání, jakož i účet úložiště Azure tooan agregovaných metrik, která se dá pak zpracována pro analýzu nebo vizualizována ve službě Power BI.  STA metriky můžete zkontrolovat statistiku výkonu tak, jako je například průměrný počet dotazů nebo dobu odezvy na dotazy.  Kromě toho hello operaci protokolování vám umožní toodrill do podrobnosti operace konkrétní hledání.

STA je cenným nástrojem toounderstand latence sazby z tohoto hlediska Azure Search.  Vzhledem k tomu, že metriky výkonu dotazu hello protokolována jsou založené na hello čas dotazu trvá toobe plně zpracovány ve službě Azure Search (od času hello je požadovaný toowhen bude odeslaná), budete moct toouse tento toodetermine Pokud problémy s latencí ze hello služby Azure Search na straně nebo mimo hello služby, například z latence sítě.  

## <a name="next-steps"></a>Další kroky
toolearn Další informace o hello cenové úrovně a omezení služby pro každé z nich, najdete v části [omezení ve službě Azure Search služby](search-limits-quotas-capacity.md).

Navštivte [plánování kapacity](search-capacity-planning.md) toolearn Další informace o oddílu a repliky kombinace.

Pro další přejít k podrobnostem na výkon a toosee některé ukázky jak tooimplement hello optimalizace popsané v tomto článku podívejte se na následující videa hello:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
