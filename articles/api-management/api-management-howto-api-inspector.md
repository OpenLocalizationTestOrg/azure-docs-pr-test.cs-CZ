---
title: "volání aaaTrace pomocí rozhraní API Inspector - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak pomocí volání tootrace hello Inspector rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: b0c401caa8da1b789f6cfe5edf97a5f118d78f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-api-inspector-tootrace-calls-in-azure-api-management"></a>Jakým způsobem volá toouse hello tootrace Inspector rozhraní API v Azure API Management
Služba API Management nabízí rozhraní API Inspector toohelp nástroj k ladění a řešení potíží s vaše rozhraní API. Hello Inspector rozhraní API je možné programově a můžete taky použít přímo z portálu pro vývojáře hello. 

Kromě toho tootracing operace, nástroj Inspector rozhraní API také sleduje [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx) hodnocení. Ukázka, najdete v části [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too21:00.

Tato příručka poskytuje návod pomocí rozhraní API Inspector.

> [!NOTE]
> Trasování API Inspector pouze generované a k dispozici pro požadavků, které obsahují klíče předplatné, které patří toohello [správce](api-management-howto-create-groups.md) účtu.
> 
> 

## <a name="trace-call"></a> Použití rozhraní API Inspector tootrace volání
toouse Inspector rozhraní API, přidejte **ocp-apim trasování: true** žádosti o volání operace tooyour záhlaví a pak stáhnout a prohlédnout hello trasování pomocí adresy URL hello indikován hello **ocp apim trasování umístění** hlavička odpovědi. To lze provést programově a můžete také provést přímo z portálu pro vývojáře hello.

Tento kurz ukazuje, jak toouse hello API Inspector tootrace operace pomocí hello základní rozhraní API kalkulačky, který je nakonfigurovaný v hello [Správa vašeho prvního rozhraní API](api-management-get-started.md) kurz Začínáme. Pokud jste nedokončili tohoto kurzu trvá jenom pár chvil tooimport hello rozhraní API základní kalkulačky, nebo můžete použít jiné API dle vlastního výběru, jako je například hello Echo API. Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které je možné použít tooexperiment s a další informace o službě API Management. Hello Echo API vrátí zpět ať vstup je odeslán tooit. toouse, můžete vyvolat všechny akce HTTP a hello návratová hodnota bude jednoduše jste poslali. 

tooget začít, klikněte na tlačítko **portál pro vývojáře** v hello portál Azure pro služby API Management. Operace lze volat přímo z portálu pro vývojáře hello, který nabízí pohodlný způsob tooview a otestovat hello operace rozhraní API.

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

![Portál pro vývojáře API Management][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** z hello horní nabídce a pak klikněte na tlačítko **Basic Calculator**.

![Echo API][api-management-api]

Klikněte na tlačítko **vyzkoušet** tootry hello **přidat dvě celá čísla** operaci.

![Vyzkoušet][api-management-open-console]

Ponechat hello výchozí hodnoty parametrů a vyberte hello klíč předplatného pro produkt hello chcete toouse z hello **klíč předplatného** rozevíracího seznamu.

Ve výchozím nastavení v portálu hello hello vývojáře **Ocp-Apim-trasování** záhlaví je již nastaven příliš**true**. Tuto hlavičku konfiguruje, zda se vygeneruje trasování.

![Odeslat][api-management-http-get]

Klikněte na tlačítko **odeslat** tooinvoke hello operaci.

![Odeslat][api-management-send-results]

V odpovědi hello hlavičky bude **ocp apim trasování umístění** s hodnotou podobné toohello následující ukázka.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

trasování Hello si můžete stáhnout z hello zadané umístění a zkontrolovat, jak je předvedeno v dalším kroku hello. Upozorňujeme, že pouze hello poslední 100 záznamů protokolu se ukládají a jsou v otočení opakovaně umístění protokolu. Pokud provedete více než 100 volání s povoleným sledováním nakonec začnete přepsal hello první trasování na místě.

## <a name="inspect-trace"></a>Zkontrolovat hello trasování
tooreview hello hodnoty v hello trasování, stáhněte si soubor trasování hello ze hello **ocp apim trasování umístění** adresy URL. Je textový soubor ve formátu JSON a obsahuje položky podobné toohello následující ukázka.

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded toohello backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent toohello caller. Starting toostream hello response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming toohello caller is complete."
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Další kroky
* Podívejte se na ukázku trasování výrazy zásad v [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Rychlé převíjení vpřed too21:00 toosee hello ukázku.

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector tootrace a call]: #trace-call
[Inspect hello trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




