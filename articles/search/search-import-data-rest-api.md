---
title: "AAA \"nahrání dat (rozhraní REST - API Azure Search) | Microsoft Docs\""
description: "Zjistěte, jak hello tooupload data tooan index Azure Search pomocí REST API."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 6ba1336012d1f0f6d6d6c933e16aa879afb9b824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-rest-api"></a>Nahrávání dat tooAzure vyhledávání pomocí hello REST API
> [!div class="op_single_selector"]
>
> * [Přehled](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
>
>

Tento článek vám ukáže, jak toouse hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/) tooimport data do indexu Azure Search.

Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md).

V pořadí toopush dokumentů do indexu pomocí hello REST API vydáte HTTP POST požadavek tooyour indexu na URL koncového bodu. text Hello hello požadavek HTTP, že text je objekt JSON obsahující dokumenty hello toobe přidat, upravit nebo odstranit.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Identifikace klíče rozhraní API správce služby Azure Search
Při odesílání požadavků HTTP na vaši službu pomocí REST API hello *každý* požadavku API musí zahrnovat hello klíč api-key generovaný pro hello službu vyhledávání jste zřídili. Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.

1. toofind klíče služby api Key, se můžete přihlásit toohello [portálu Azure](https://portal.azure.com/)
2. Okno služby Azure Search přejděte tooyour
3. Klikněte na hello ikonu klíčů."

Vaše služba bude mít *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat.. Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.
* Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.

Pro účely hello importování dat do indexu, můžete použít buď vaší primární nebo sekundární klíč správce.

## <a name="decide-which-indexing-action-toouse"></a>Rozhodněte, které indexování toouse akce
Pokud používáte hello REST API, vydáte požadavky HTTP POST s JSON požadavek těla tooyour indexu Azure Search na adresu URL koncového bodu. Hello objekt JSON v požadavku HTTP bude obsahovat jedno pole JSON s názvem "value" s objekty JSON reprezentujícími dokumenty, které byste chtěli tooadd tooyour indexu, aktualizovat nebo odstranit.

Každý objekt JSON v poli "value" hello představuje dokumentu toobe, indexovat. Každý z těchto objektů obsahuje klíč dokumentu hello a určuje akci hello potřeby indexování (odeslání, sloučení, odstranění atd.). Podle toho, která hello níže akce, který zvolíte musí být objekt pro každý dokument obsahovat pouze určitá pole:

| @search.action | Popis | Potřebná pole pro každý dokument | Poznámky |
| --- | --- | --- | --- |
| `upload` |`upload` Akci je podobný tooan "upsert", kde hello dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje. |klíč a další pole chcete toodefine |Pokud aktualizujete nebo nahrazujete stávající dokument, bude každé pole, které není určený v požadavku hello mít nastavené příliš`null`. K tomu dojde i v případě, že bylo hello pole dříve nastavené tooa jinou hodnotu než null. |
| `merge` |Aktualizace stávající dokumentů s hello zadaná pole. Pokud hello dokument v indexu hello neexistuje, sloučení hello se nezdaří. |klíč a další pole chcete toodefine |Každé pole zadané ve sloučení nahradí stávající pole hello v dokumentu hello. To zahrnuje i pole typu `Collection(Edm.String)`. Například pokud hello dokument obsahuje pole `tags` s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro `tags`, hello konečná hodnota hello `tags` pole bude `["economy", "pool"]`. Hodnota nebude `["budget", "economy", "pool"]`. |
| `mergeOrUpload` |Tato akce se chová jako `merge` Pokud dokument s hello zadaný klíč již existuje v indexu hello. Pokud hello dokument neexistuje, chová se jako `upload` s novým dokumentem. |klíč a další pole chcete toodefine |- |
| `delete` |Odebere zadaný dokument hello hello index. |pouze klíč |Všechna zadaná pole kromě pole klíče hello budou ignorovány. Pokud chcete tooremove z dokumentu jednotlivá pole, použijte `merge` místo a jednoduše nastavte pole hello explicitně toonull. |

## <a name="construct-your-http-request-and-request-body"></a>Konstrukce požadavku HTTP a textu žádosti
Teď, když jste shromáždili hello potřebné hodnoty polí pro akce indexu, jsou připravené tooconstruct hello vlastní požadavek HTTP a JSON vaše data žádosti tooimport textu.

#### <a name="request-and-request-headers"></a>Požadavek a hlavičky požadavku
V adrese URL hello, budete potřebovat tooprovide váš název služby, název indexu ("hotels" v tomto případě), a také hello správnou verzi rozhraní API (aktuální verze rozhraní API hello je `2016-09-01` v době publikování tohoto dokumentu hello). Budete potřebovat toodefine hello `Content-Type` a `api-key` hlavičky požadavku. Pro pozdější hello použijte jeden z klíčů správce vaší služby.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Text žádosti
```JSON
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close tootown hall and hello river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

V tomto případě používáme jako akce hledání `upload`, `mergeOrUpload`, a `delete`.

Předpokládejme, že je ukázkový index „hotels“ již naplněný řadou dokumentů. Všimněte si, jak nebylo nutné toospecify všechny hello možná pole dokumentu při použití `mergeOrUpload` a jak jsme zadali pouze klíč dokumentu hello (`hotelId`) při použití `delete`.

Také Upozorňujeme, že jste může obsahovat jenom too1000 dokumentů (nebo 16 MB) v jedné žádosti indexování.

## <a name="understand-your-http-response-code"></a>Pochopení kódu odpovědi HTTP
#### <a name="200"></a>200
Po odeslání úspěšné žádosti indexování obdržíte odpověď protokolu HTTP se stavovým kódem `200 OK`. Hello text JSON hello odpověď HTTP bude vypadat takto:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Stavový kód `207` se vrátí, pokud nebyla alespoň jedna položka úspěšně indexovaná. Hello text JSON hello odpověď HTTP bude obsahovat informace o neúspěšných dokumentech hello.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "hello search service is too busy tooprocess this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> Často to znamená, že hello zatížení na hledání služby dosahuje bodu, kdy začnou požadavky indexování tooreturn `503` odpovědi. V tom případě důrazně doporučujeme, aby se váš klientský kód stáhnul a chvíli počkal před tím, než to zkusí znovu. To vám poskytne hello systému některé toorecover čas, zvýšit hello šanci na úspěšné provedení dalších požadavků. Rychlé opakování požadavků pouze prodlouží situaci hello.
>
>

#### <a name="429"></a>429
Stavový kód `429` bude vrácen, pokud jste překročili kvótu hello počet dokumentů na index.

#### <a name="503"></a>503
Stavový kód `503` bude vrácen, pokud žádná z položek hello v požadavku hello byly úspěšně indexovat. Tato chyba znamená, že hello systém je velmi zatížen a váš požadavek nelze zpracovat v tuto chvíli.

> [!NOTE]
> V tom případě důrazně doporučujeme, aby se váš klientský kód stáhnul a chvíli počkal před tím, než to zkusí znovu. To vám poskytne hello systému některé toorecover čas, zvýšit hello šanci na úspěšné provedení dalších požadavků. Rychlé opakování požadavků pouze prodlouží situaci hello.
>
>

Další informace o akcích dokumentu a úspěšných/neúspěšných odpovědích naleznete v tématu [Přidání, aktualizování nebo odstranění dokumentů](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents). Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

## <a name="next-steps"></a>Další kroky
Po naplnění indexu Azure Search, bude připravená toostart vystavování toosearch dotazy pro dokumenty. Podrobnosti naleznete v tématu [Dotazování indexu Azure Search](search-query-overview.md).
