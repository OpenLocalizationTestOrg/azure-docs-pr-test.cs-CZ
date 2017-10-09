---
title: "AAA \"Vytvoření indexu (rozhraní REST - API Azure Search) | Microsoft Docs\""
description: "Vytvořte index v kódu pomocí hello HTTP REST API služby Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: ac6c5fba-ad59-492d-b715-d25a7a7ae051
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 117ab64a9874a443351a8a02a9b959b8f7beb7c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-rest-api"></a>Vytvoření indexu Azure Search pomocí REST API hello
> [!div class="op_single_selector"]
>
> * [Přehled](search-what-is-an-index.md)
> * [Azure Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
>
>

Tento článek vás provede procesem vytvoření Azure Search hello [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) pomocí hello REST API služby Azure Search.

Předtím, než podle těchto pokynů vytvoříte index, byste už měli mít [vytvořenou službu Azure Search](search-create-service-portal.md).

toocreate indexu Azure Search pomocí hello REST API, vydáte koncový bod adresy URL jednu HTTP POST požadavek tooyour služby Azure Search. Definice indexu bude obsažená v textu hello žádosti jako obsah JSON ve správném.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Identifikace klíče rozhraní API správce služby Azure Search
Teď, když máte zřízenou službu Azure Search, můžete vydávat žádosti HTTP na koncový bod adresy URL služby pomocí hello REST API. *Všechny* žádostí o rozhraní API musí zahrnovat hello klíč api-key generovaný pro hello službu vyhledávání jste zřídili. Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.

1. toofind vaší služby klíče api Key musíte se přihlásit hello [portálu Azure](https://portal.azure.com/)
2. Okno služby Azure Search přejděte tooyour
3. Klikněte na hello ikonu klíčů."

Vaše služba bude mít *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat.. Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.
* Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.

Pro účely vytvoření indexu hello, můžete použít buď vaší primární nebo sekundární klíč správce.

## <a name="define-your-azure-search-index-using-well-formed-json"></a>Definování indexu Azure Search pomocí správného formátu JSON
Jediná služba tooyour požadavek HTTP POST vytvoří váš index. Hello text žádosti HTTP POST bude obsahovat jeden objekt JSON, který definuje index Azure Search.

1. Hello první vlastností tohoto objektu JSON je hello název indexu.
2. Hello druhou vlastností tohoto objektu JSON je pole JSON s názvem `fields` , který obsahuje samostatný objekt JSON pro každé pole v indexu. Každý z těchto objektů JSON obsahuje více dvojic název hodnota pro každou hello pole atributů, včetně "name" "type" atd.

Je důležité, aby byl vyhledávání uživatelské prostředí a obchodní potřeby v úvahu při navrhování indexu, protože každé pole musí mít přiřazen hello [správné atributy](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Tyto atributy určují, které funkce vyhledávání (filtrování, používání faset, řazení fulltextového vyhledávání atd.) použít toowhich pole. Kterýkoli atribut nezadáte bude výchozí hello tooenable hello odpovídající funkce hledání, pokud ji výslovně zakážete.

V našem příkladu jsme nazvali index „hotels“ a pole jsme definovali takto:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Pečlivě zvolili hello atributy indexu pro každé pole v závislosti na tom, jak myslíme si, že se pravděpodobně použijí v aplikaci. Například `hotelId` je jedinečný klíč, který lidé hledající hotely nejspíš neznají, takže jsme zakázat fulltextové vyhledávání pro toto pole nastavte `searchable` příliš`false`, což šetří místo v indexu hello.

Upozorňujeme, že právě jedno pole v indexu typu `Edm.String` musí být hello určeny jako pole "klíčem" hello.

výše uvedená definice indexu Hello používá analyzátor jazyka pro hello `description_fr` pole, protože je určený toostore francouzského textu. V tématu [tématu jazykové podpory hello](https://docs.microsoft.com/rest/api/searchservice/Language-support) a také odpovídající hello [příspěvku na blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/) Další informace o analyzátorech jazyka.

## <a name="issue-hello-http-request"></a>Problém hello HTTP požadavku
1. Použijte definici indexu jako text žádosti hello, vydejte HTTP POST požadavek tooyour Azure Search service adresu URL koncového bodu. V adrese URL hello, že toouse být název vaší služby jako název hostitele hello a umístí hello správné `api-version` jako parametr řetězce dotazu (aktuální verze rozhraní API hello je `2016-09-01` v době publikování tohoto dokumentu hello).
2. V hlavičkách žádosti hello, zadejte hello `Content-Type` jako `application/json`. Budete také potřebovat tooprovide klíč správce vaší služby, který jste určili v kroku I v hello `api-key` záhlaví.

Tooprovide bude mít vlastní název a rozhraní api klíče tooissue hello žádost o služby níže:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [api-key]


V případě úspěšné žádosti by se měl zobrazit stavový kód 201 (vytvořeno). Další informace o vytvoření indexu prostřednictvím hello REST API, navštivte hello [referenční dokumentace rozhraní API zde](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

Když jste hotovi s toodelete indexu a chcete ho, stačí vydejte žádost HTTP DELETE. Jedná se například jak jsme odstranili index "hotels" hello:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2016-09-01
    api-key: [api-key]


## <a name="next-steps"></a>Další kroky
Po vytvoření indexu Azure Search, budete moci příliš[nahrát obsah do indexu hello](search-what-is-data-import.md) , abyste mohli začít prohledávat data.
