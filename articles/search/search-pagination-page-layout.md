---
title: "výsledky hledání toopage aaaHow ve službě Azure Search | Microsoft Docs"
description: "Stránkování ve službě Azure Search, hostované cloudové vyhledávací službě v Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: e3abc1ca4d5994b0a77955379081a4fcfa5a7fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopage-search-results-in-azure-search"></a>Jak výsledky hledání toopage ve službě Azure Search
Tento článek obsahuje pokyny k jak stránky, například celkový počet, načtení dokumentu, pořadí řazení a navigační výsledků toouse hello rozhraní REST API služby Azure Search tooimplement standardních prvků hledání.

V každém případě dál uvedených souvisejících se stránkami možnosti, které přispívají data nebo informace o tooyour stránky s výsledky hledání jsou určeny pomocí hello [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798927.aspx) požadavky odeslané tooyour služby Azure Search. Žádosti o zahrnovat příkaz GET, cestu a parametry dotazu, které informovat o tom, co je požadované hello služby a jak tooformulate hello odpovědi.

> [!NOTE]
> Počet elementů, například adresa URL služby a cesta, příkaz HTTP, zahrnuje žádost platná `api-version`a tak dále. Jako stručný výtah jsme oříznut hello příklady toohighlight jenom hello syntaxi, která je relevantní toopagination. Najdete v tématu hello [rozhraní REST API služby Azure Search](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentace podrobnosti o žádosti o syntaxi.
> 
> 

## <a name="total-hits-and-page-counts"></a>Celkový počet přístupů a počty stránky
Zobrazuje hello celkový počet výsledků vrácených dotazem a ty pak vrácení výsledků v menší bloky dat, je základní toovirtually všechny stránky vyhledávání.

![][1]

Ve službě Azure Search, můžete použít hello `$count`, `$top`, a `$skip` parametry tooreturn tyto hodnoty. Hello následující příklad ukazuje ukázková žádost pro celkový počet přístupů, vrátí jako `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Dokumenty ve skupinách 15 načíst a také se zobrazují celkový počet přístupů hello začínající na první stránku hello:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Přestránkování výsledky vyžaduje obě `$top` a `$skip`, kde `$top` Určuje, kolik tooreturn položek v dávce, a `$skip` Určuje, kolik položek tooskip. V hello následující ukázka, každé stránce zobrazuje hello vedle 15 položky indikován hello přírůstkové přeskočí v hello `$skip` parametr.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Rozložení
Na stránce výsledky hledání můžete chtít tooshow miniaturu, podmnožinu pole a stránku odkaz tooa plnou verzi produktu.

 ![][2]

Ve službě Azure Search, byste použili `$select` a vyhledávací příkaz tooimplement toto prostředí.

tooreturn podmnožinu pole vedle sebe rozložení:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Bitové kopie a mediálních souborů nejsou přímo s možností vyhledávání a by měly být uložené v jiné platformy úložiště, jako je například úložiště objektů Azure Blob, tooreduce náklady. V indexu hello a dokumenty zadejte pole, které obsahuje adresu URL hello hello externí obsah. Pole hello pak můžete použít jako odkaz na obrázek. bitové kopie toohello Hello adresa URL musí být v dokumentu hello.

tooretrieve produktu popis stránky pro **onClick** událostí, použijte [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass v klíči dokumentu tooretrieve hello hello. datový typ Hello hello klíče je `Edm.String`. V tomto příkladu je *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Řazení podle relevance, hodnocení a ceny
Výchozí toorelevance, ale je běžné toomake alternativní pořadí řazení snadno dostupné, aby zákazníci můžete rychle změnit pořadí existující výsledky do různých rank pořadí se často pořadí řazení.

 ![][3]

Ve službě Azure Search, řazení podle hello `$orderby` výrazu pro všechna pole, které jsou uloženy jako`"Sortable": true.`

Relevance důrazně souvisí s profily vyhodnocování. Můžete použít hello vyhodnocování výchozí, což závisí na text analýzy a statistiky toorank pořadí všechny výsledky, s vyšší skóre toodocuments s více nebo silnější odpovídá děje hledaný termín.

Alternativní pořadí řazení obvykle souvisejí s **onClick** události, které zpětné volání metoda tooa které sestavení hello pořadí řazení. Například uděleno tento element stránky:

 ![][4]

Metoda, která přijme hello vybrali možnost řazení jako vstup a vrátí seřazený seznam pro kritéria hello přidružené k této možnosti vytvoříte.

 ![][5]

> [!NOTE]
> Při vyhodnocování výchozí hello stačí pro mnoho scénářů, doporučujeme místo vytvoření relevance na základě vlastní profil vyhodnocování. Vlastní profil vyhodnocování vám dává položky tooboost způsobem, které jsou více výhodné tooyour firmy. V tématu [přidat profil vyhodnocování](http://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace. 
> 
> 

## <a name="faceted-navigation"></a>Fasetová navigace
Navigace vyhledávání je běžné na stránka s výsledky, často nacházející se v horní části stránky a hello straně. Ve službě Azure Search poskytuje Fasetové navigace řízené samotným hledání podle předdefinovaných filtrů. V tématu [Fasetové navigace ve službě Azure Search](search-faceted-navigation.md) podrobnosti.

## <a name="filters-at-hello-page-level"></a>Filtry na úrovni stránky hello
Pokud návrh vašeho řešení zahrnuty vyhrazené vyhledávání stránky pro určité typy obsahu (například prodejní online aplikace, která má oddělení uvedené v hello horní části stránky hello), můžete vložit výraz filtru společně **onClick** tooopen událostí na stránce v odkazující stavu. 

Můžete odeslat filtr s nebo bez výrazu vyhledávání. Například hello následující požadavek bude filtrovat název značky, vrácení pouze dokumenty, které odpovídají ho.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

V tématu [vyhledávání dokumentů (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798927.aspx) Další informace o `$filter` výrazy.

## <a name="see-also"></a>Viz také
* [Rozhraní API REST služby vyhledávání systému Azure](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [Operace indexu](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [Operace dokumentu](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [Kurzy o službě Azure Search a video](search-video-demo-tutorial-list.md)
* [Fasetové navigace ve službě Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
