---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Předběžná dokumentace pro funkci hello synonyma (preview), v hello REST API služby Azure Search."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 2695139d2b298fa2e7c1814715fdf96729f594ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="synonyms-in-azure-search-preview"></a>Synonyma ve službě Azure Search (preview)

Synonyma na vyhledávacích webech přidružit ekvivalentní podmínky, které implicitně rozbalte hello obor dotazu, bez nutnosti tooactually zadejte termín hello uživatele hello. Například dané hello termín "pes" a přidružení synonymum "PSA" a "štěněte", všechny dokumenty obsahující "pes", "PSA" nebo "štěněte" bude spadat do rozsahu hello hello dotazu.

Ve službě Azure Search synonymum rozšíření se provádí v době dotazu. Můžete přidat synonymum mapy tooa služby s žádné přerušení tooexisting operace. Můžete přidat **synonymMaps** definici pole vlastnosti tooa bez nutnosti toorebuild hello index. Další informace najdete v tématu [aktualizace indexu](https://docs.microsoft.com/rest/api/searchservice/update-index).

## <a name="feature-availability"></a>Dostupnost funkcí

Hello synonyma funkce je aktuálně ve verzi preview a podporována v pouze hello nejnovější verzi preview rozhraní api-version (verze api-version = 2016-09-01-Preview). Podpora webu Azure Portal se v současnosti neposkytuje. Protože hello rozhraní API verze je zadán u hello požadavku, je možné toocombine všeobecně dostupná (GA) a preview rozhraní API v hello stejná aplikace. Však může změnit preview, které nejsou v rámci smlouvy o úrovni služeb a funkcí rozhraní API, tak nedoporučujeme jejich používání v produkční aplikace.

## <a name="how-toouse-synonyms-in-azure-search"></a>Jak vyhledávat synonyma toouse v Azure

Ve službě Azure Search podpory synonymum vychází synonymum mapy definovat a nahrát tooyour služby. Tyto mapy tvoří nezávislý prostředek (např. indexy nebo zdroje dat) a mohou být využívána žádné prohledávatelné pole v jakékoli indexu ve vyhledávací službě.

Mapuje synonymum a indexy jsou zachována nezávisle. Po definování mapu synonymum a nahrajte ho tooyour služby, můžete povolit funkci synonymum hello na pole tak, že přidáte novou vlastnost s názvem **synonymMaps** v definici pole hello. Vytvoření, aktualizace a odstranění mapu synonymum, je vždy celé dokumentu operaci, což znamená, že se nedají vytvořit, aktualizovat nebo odstranit části hello synonymum mapy postupně. Aktualizace i jeden záznam nevyžaduje znovu načíst.

Začlenění synonyma do vyhledávací aplikaci je dvoustupňový proces:

1.  Přidáte službu vyhledávání synonymum mapy tooyour prostřednictvím hello rozhraní API níže.  

2.  Konfigurovat mapu synonymum hello toouse prohledávatelné pole v definici indexu hello.

### <a name="synonymmaps-resource-apis"></a>Rozhraní API SynonymMaps prostředků

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Přidat nebo aktualizovat mapu synonymum v rámci služby, pomocí POST nebo BLOKOVAT.

Synonymum maps se odešlou toohello service pomocí Metoda POST nebo PUT. Každé pravidlo musí být odděleny hello znak nového řádku (\n). Můžete definovat too5, 000 pravidel na synonymum mapy ve bezplatné služby a 10 000 pravidel ve všech dalších skladových položek. Každé pravidlo může obsahovat maximálně too20 rozšíření.

V této verzi preview musí být synonymum mapy ve formátu hello Apache Solr, který je popsáno níže. Pokud máte existující slovník synonymum v jiném formátu a chcete toouse ji přímo, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

Můžete vytvořit nové mapování synonymum pomocí HTTP POST, stejně jako hello následující ukázka:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternativně můžete použít PUT a zadejte název mapy synonymum hello na hello identifikátor URI. Pokud hello synonymum mapy neexistuje, bude vytvořen.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr synonymum formátu

Formát Solr Hello podporuje ekvivalentní a explicitní synonymum mapování. Pravidla mapování splňovat toohello s otevřeným zdrojem synonymum filtru specifikaci Apache Solr, popsané v tomto dokumentu: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Zde je ukázka pravidla pro ekvivalentní synonyma.
```
              USA, United States, United States of America
```

Pomocí výše uvedené pravidlo hello, vyhledávací dotaz "USA" bude rozšiřovat příliš "USA" nebo "USA" nebo "USA".

Šipka je označený jako explicitní mapování "= >". -Li zadána, termín posloupnost vyhledávací dotaz, který odpovídá hello levé straně "= >" bude nahrazena adresou hello alternativy na pravé straně hello. Zadané pravidlo hello níže, vyhledávací dotazy "Washington", "Wash." nebo "WA" budou všechny možné přepsaná příliš "WA". Explicitní mapování platí v určeném směru hello a není přepisování příliš hello dotazu "WA" "Washington" v tomto případě.
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Seznam synonymum mapuje v rámci služby.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Načíst synonymum mapu v rámci služby.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Odstranění mapy synonyma v rámci služby.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-toouse-hello-synonym-map-in-hello-index-definition"></a>Konfigurovat mapu synonymum hello toouse prohledávatelné pole v definici indexu hello.

Nové vlastnosti pole **synonymMaps** lze použít toospecify toouse synonymum mapy pro prohledávatelné pole. Synonymum mapy prostředků úrovně služby a lze odkazovat pomocí libovolného pole index v rámci služby hello.

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**synonymMaps** lze zadat pro prohledatelná pole typu hello 'Edm.String' nebo 'Collection(Edm.String)'.

> [!NOTE]
> V této verzi preview může mít pouze jeden synonymum mapovat na pole. Pokud chcete toouse více mapami synonymum, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Dopad synonyma na jiných vyhledávacích funkcí

Funkce synonyma Hello přepíše původní dotaz hello s synonyma s hello operátor OR. Z tohoto důvodu přístupů zvýrazňování a vyhodnocování profily zachází s původní termín hello a synonyma jako ekvivalentní.

Funkce synonymum platí toosearch dotazy a nepoužije toofilters nebo omezující vlastnosti. Podobně návrhy založen pouze na původní termín hello; synonymum shoduje se nezobrazí v odpovědi hello.

Synonymum rozšíření se nevztahují toowildcard hledaných termínů; Předpona, přibližné a podmínky regex nejsou rozšířit.

## <a name="tips-for-building-a-synonym-map"></a>Tipy pro sestavování synonymum mapy

- Mapu stručný a dobře navrženou synonymum je efektivnější než vyčerpávající seznam možných shod. Nadměrně velké nebo složité slovník trvat déle tooparse a ovlivnit latence dotazu hello, pokud dotaz hello rozšíří toomany synonyma. Místo odhad, při které by mohly používat podmínky, můžete získat skutečný podmínky hello prostřednictvím [hledání sestava analýzy provozu](search-traffic-analytics.md).

- Jako předběžná verze i ověřování cvičení, povolit a pak bude pomocí této sestavy tooprecisely určit, jaké podmínky těžit z synonymum shoda a pak pokračujte toouse jej jako ověření, že je mapu synonymum generovala lepší výsledek. V sestavě hello předdefinované hello dlaždice "Nejčastější dotazy vyhledávání" a "nula výsledek vyhledávací dotazy" získáte hello potřebné informace.

- Můžete vytvořit více mapami synonymum pro vyhledávací aplikaci (například podle jazyka, pokud vaše aplikace podporuje základní vícejazyčnou zákazníka). V současné době pole lze použít pouze jeden z nich. Vlastnosti synonymMaps můžete kdykoli aktualizovat.

## <a name="next-steps"></a>Další kroky

- Pokud máte existující index ve vývojovém prostředí (mimo produkční), Experimentujte s toosee malé slovník jak hello přidání synonyma změní hello vyhledávání prostředí, včetně dopad na vyhodnocování profily, počtu zvýraznění a návrhy.

- [Povolit Analýza provozu vyhledávání](search-traffic-analytics.md) a hello pomocí předdefinované sestavy Power BI toolearn podmínky, které se používají většinu a které těch, které jsou návratový hello nulové dokumenty. Díky tyto přehledy, zkontrolovat, jestli hello slovník tooinclude synonyma pro neproduktivní dotazy, které by měl být řešení toodocuments v indexu.
