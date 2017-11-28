---
title: "aaaCreate indexu Azure Search | Microsoft Azure | Hostovaná Cloudová vyhledávací služba"
description: "Co je index ve službě Azure Search a jak se používá?"
services: search
documentationcenter: 
author: ashmaka
ms.assetid: a395e166-bf2e-4fca-8bfc-116a46c5f7b1
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: c01cc654ff91427c8f1569b2f5b060a0a0f044c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index"></a>Vytvoření indexu Azure Search
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-an-index.md)
> * [Azure Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

## <a name="what-is-an-index"></a>Co je index?
*Index* je trvalé úložiště *dokumentů* a jiných objektů, které používá služba Azure Search. Dokument je jednou jednotkou prohledávatelných dat v indexu. Například internetový prodejce může mít dokument pro každou prodanou položku, zatímco tisková agentura může mít dokument pro každý článek atd. Mapování tyto koncepty toomore známé databázových ekvivalentů: *index* je koncepčně podobá tooa *tabulky*, a *dokumenty* jsou příliš zhruba odpovídá*řádky* v tabulce.

Pokud jste přidáváte nebo nahráváte dokumenty a odeslat tooAzure dotazy vyhledávání, hledání, odeslat vaše požadavky tooa konkrétního indexu ve vyhledávací službě.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Typy a atributy polí v indexu Azure Search
Při definování schématu musíte zadat hello název, typ a atributy každého pole v indexu. Hello typ pole klasifikuje hello data, která je uložená v tomto poli. Atributy se nastavují u jednotlivých polí toospecify použití pole hello. Hello následující tabulky poskytují výčet hello typy a atributy, které můžete zadat.

### <a name="field-types"></a>Typy polí
| Typ | Popis |
| --- | --- |
| *Edm.String* |Text, který jde volitelně tokenizovat k fulltextovému hledání (dělení slov, rozklad atd). |
| *Collection(Edm.String)* |Seznam řetězců, které jde volitelně tokenizovat k fulltextovému hledání. Neexistuje žádné teoretické omezení na hello počet položek v kolekci, ale hello 16 MB omezení velikosti datové části platí toocollections. |
| *Edm.Boolean* |Obsahuje hodnoty true nebo false. |
| *Edm.Int32* |32bitové celočíselné hodnoty. |
| *Edm.Int64* |64bitové celočíselné hodnoty. |
| *Edm.Double* |Číselné údaje s dvojitou přesností. |
| *Edm.DateTimeOffset* |Hodnoty data a času ve formátu hello OData V4 (například `yyyy-MM-ddTHH:mm:ss.fffZ` nebo `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Bod představující geografické umístění na zeměkouli hello. |

Podrobnější informace o [datových typech podporovaných službou Azure Search najdete tady](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="field-attributes"></a>Atributy polí
| Atribut | Popis |
| --- | --- |
| *Klíč* |Řetězec, který poskytuje hello jedinečné ID jednotlivých dokumentů, které slouží k vyhledávání dokumentů. Každý index musí mít jeden klíč. Pouze jedno pole může být hello klíč a jeho typ musí být nastavený tooEdm.String. |
| *Retrievable* |Určuje, jestli může být pole vrácené ve výsledku hledání. |
| *Filterable* |Umožňuje toobe hello pole použít ve filtrovacích dotazech. |
| *Sortable* |Umožňuje dotazu toosort výsledky hledání podle tohoto pole. |
| *Facetable* |Umožňuje pole toobe použít v [Fasetové navigace](search-faceted-navigation.md) strukturu pro uživatele řízené samotným filtrování. Obvykle obsahují opakované hodnoty, které můžete použít toogroup pole více dokumentů společně (například více dokumentů, které spadají pod jednu značku nebo kategorii služeb) nejlépe fungovat jako omezující vlastnosti. |
| *Searchable* |Značky hello pole jako fulltextově prohledávatelné. |

Podrobnější informace o [atributech indexu služby Azure Search najdete tady](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="guidance-for-defining-an-index-schema"></a>Pokyny pro definování schématu indexu
Při navrhování indexu, přepněte doby hello plánování fáze toothink promyslete každé rozhodnutí. Je důležité, aby byl vyhledávání uživatelské prostředí a obchodní potřeby v úvahu při navrhování indexu, protože každé pole musí mít přiřazen hello [správné atributy](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Při změně indexu po nasazení zahrnuje znovu sestavit a načíst hello data.

Pokud se v průběhu času změní požadavky na úložiště dat, můžete zvýšit nebo snížit kapacitu přidáním nebo odebráním oddílů. Podrobnosti najdete v tématu [Správa služby Search v Azure](search-manage.md) nebo [Omezení služby](search-limits-quotas-capacity.md).

