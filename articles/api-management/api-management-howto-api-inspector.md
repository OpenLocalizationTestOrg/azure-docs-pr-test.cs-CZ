---
title: "Trasování volání pomocí rozhraní API Inspector - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak pro trasování volání s využitím Inspector rozhraní API v Azure API Management."
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
ms.openlocfilehash: a9d4d3be7f046af975f6dc25670070204848588c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a><span data-ttu-id="8c7a3-103">Jak používat nástroj Inspector rozhraní API pro trasování volání v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="8c7a3-103">How to use the API Inspector to trace calls in Azure API Management</span></span>
<span data-ttu-id="8c7a3-104">API Management představuje nástroj Inspector rozhraní API vám pomohou při ladění a řešení potíží s vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-104">API Management provides an API Inspector tool to help you with debugging and troubleshooting your APIs.</span></span> <span data-ttu-id="8c7a3-105">Rozhraní API Inspector je možné programově a můžete taky použít přímo z portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-105">The API Inspector can be used programmatically and can also be used directly from the developer portal.</span></span> 

<span data-ttu-id="8c7a3-106">Kromě trasování operací, nástroj Inspector rozhraní API také sleduje [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx) hodnocení.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-106">In addition to tracing operations, API Inspector also traces [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx) evaluations.</span></span> <span data-ttu-id="8c7a3-107">Ukázka, najdete v části [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 21:00.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-107">For a demonstration, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 21:00.</span></span>

<span data-ttu-id="8c7a3-108">Tato příručka poskytuje návod pomocí rozhraní API Inspector.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-108">This guide provides a walk-through of using API Inspector.</span></span>

> [!NOTE]
> <span data-ttu-id="8c7a3-109">Trasování API Inspector pouze generované a k dispozici pro požadavků, které obsahují klíče předplatné, které patří do [správce](api-management-howto-create-groups.md) účtu.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-109">API Inspector traces are only generated and made available for requests containing subscription keys that belong to the [administrator](api-management-howto-create-groups.md) account.</span></span>
> 
> 

## <span data-ttu-id="8c7a3-110"><a name="trace-call"></a> Inspector pomocí rozhraní API pro trasování volání</span><span class="sxs-lookup"><span data-stu-id="8c7a3-110"><a name="trace-call"> </a> Use API Inspector to trace a call</span></span>
<span data-ttu-id="8c7a3-111">Použít kontrola rozhraní API, přidejte **ocp-apim trasování: true** žádosti hlavičky k volání operace a pak stáhnout a prohlédnout trasování pomocí adresy URL uvedené **ocp apim trasování umístění** odpovědi hlavička.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-111">To use API Inspector, add an **ocp-apim-trace: true** request header to your operation call, and then download and inspect the trace using the URL indicated by the **ocp-apim-trace-location** response header.</span></span> <span data-ttu-id="8c7a3-112">To lze provést programově a můžete také provést přímo z portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-112">This can be done programatically, and can also be done directly from the developer portal.</span></span>

<span data-ttu-id="8c7a3-113">Tento kurz ukazuje, jak použít kontrola rozhraní API pro trasování operací pomocí základní rozhraní API kalkulačky, který je nakonfigurovaný v [Správa vašeho prvního rozhraní API](api-management-get-started.md) kurz Začínáme.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-113">This tutorial shows how to use the API Inspector to trace operations using the Basic Calculator API that is configured in the [Manage your first API](api-management-get-started.md) getting started tutorial.</span></span> <span data-ttu-id="8c7a3-114">Pokud jste nedokončili tohoto kurzu trvá jenom pár chvil importovat rozhraní API základní kalkulačky, nebo můžete použít jiné API dle vlastního výběru, jako je například Echo API.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-114">If you haven't completed that tutorial it only takes a few moments to import the Basic Calculator API, or you can use another API of your choosing such as the Echo API.</span></span> <span data-ttu-id="8c7a3-115">Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním API programu Echo, které můžete použít k experimentování a seznámení se službou API Management.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-115">Each API Management service instance comes pre-configured with an Echo API that can be used to experiment with and learn about API Management.</span></span> <span data-ttu-id="8c7a3-116">Echo API vrátí zpět ať vstup je do něj odeslané.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-116">The Echo API returns back whatever input is sent to it.</span></span> <span data-ttu-id="8c7a3-117">Pokud chcete použít, můžete vyvolat všechny akce HTTP a návratová hodnota bude jednoduše jste poslali.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-117">To use it, you can invoke any HTTP verb, and the return value will simply be what you sent.</span></span> 

<span data-ttu-id="8c7a3-118">Chcete-li začít, klikněte na tlačítko **portál pro vývojáře** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-118">To get started, click **Developer portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="8c7a3-119">Operace lze volat přímo z portálu pro vývojáře, která představuje pohodlný způsob pro zobrazení a testování operací v rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-119">Operations can be called directly from the developer portal which provides a convenient way to view and test the operations of an API.</span></span>

> <span data-ttu-id="8c7a3-120">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-120">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

![Portál pro vývojáře API Management][api-management-developer-portal-menu]

<span data-ttu-id="8c7a3-122">Klikněte na tlačítko **rozhraní API** z horní nabídce a pak klikněte na tlačítko **Basic Calculator**.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-122">Click **APIs** from the top menu, and then click **Basic Calculator**.</span></span>

![Echo API][api-management-api]

<span data-ttu-id="8c7a3-124">Klikněte na tlačítko **vyzkoušet** pokusit **přidat dvě celá čísla** operaci.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-124">Click **Try it** to try the **Add two integers** operation.</span></span>

![Vyzkoušet][api-management-open-console]

<span data-ttu-id="8c7a3-126">Ponechte výchozí hodnoty parametrů a vyberte klíč předplatného pro produkt, kterou chcete použít z **klíč předplatného** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-126">Keep the default parameter values, and select the subscription key for the product you want to use from the **subscription-key** drop-down.</span></span>

<span data-ttu-id="8c7a3-127">Ve výchozím nastavení v portálu pro vývojáře **Ocp-Apim-trasování** záhlaví je již nastaven na **true**.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-127">By default in the developer portal the **Ocp-Apim-Trace** header is already set to **true**.</span></span> <span data-ttu-id="8c7a3-128">Tuto hlavičku konfiguruje, zda se vygeneruje trasování.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-128">This header configures whether or not a trace is generated.</span></span>

![Odeslat][api-management-http-get]

<span data-ttu-id="8c7a3-130">Klikněte na tlačítko **odeslat** k vyvolání operace.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-130">Click **Send** to invoke the operation.</span></span>

![Odeslat][api-management-send-results]

<span data-ttu-id="8c7a3-132">V odpovědi bude hlavičky **ocp apim trasování umístění** s hodnotou podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-132">In the response headers will be an **ocp-apim-trace-location** with a value similar to the following example.</span></span>

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

<span data-ttu-id="8c7a3-133">Trasování je možné stáhnout ze zadaného umístění a zkontrolovat, jak je předvedeno v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-133">The trace can be downloaded from the specified location and reviewed as demonstrated in the next step.</span></span> <span data-ttu-id="8c7a3-134">Upozorňujeme, že pouze poslední položky 100 protokolu se ukládají a jsou v otočení opakovaně umístění protokolu.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-134">Note that only the last 100 log entries are stored and log locations are reused in rotation.</span></span> <span data-ttu-id="8c7a3-135">Pokud provedete více než 100 volání s povoleným sledováním nakonec začnete přepsal první trasování na místě.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-135">So if you make more than 100 calls with tracing enabled you will eventually start overwriting the first traces in place.</span></span>

## <span data-ttu-id="8c7a3-136"><a name="inspect-trace"></a>Zkontrolovat trasování</span><span class="sxs-lookup"><span data-stu-id="8c7a3-136"><a name="inspect-trace"> </a>Inspect the trace</span></span>
<span data-ttu-id="8c7a3-137">Chcete-li zkontrolovat hodnoty v trasování, stáhněte soubor trasování z **ocp apim trasování umístění** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-137">To review the values in the trace, download the trace file from the **ocp-apim-trace-location** URL.</span></span> <span data-ttu-id="8c7a3-138">Je textový soubor ve formátu JSON a obsahuje položky, podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-138">It is a text file in JSON format, and contains entries similar to the following example.</span></span>

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
                    "message": "Request is being forwarded to the backend service.",
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
                    "message": "Response headers have been sent to the caller. Starting to stream the response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming to the caller is complete."
                }
            }
        ]
    }
}
```

## <span data-ttu-id="8c7a3-139"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c7a3-139"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="8c7a3-140">Podívejte se na ukázku trasování výrazy zásad v [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span><span class="sxs-lookup"><span data-stu-id="8c7a3-140">Watch a demo of tracing policy expressions in [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span></span> <span data-ttu-id="8c7a3-141">Převinutí souboru 21:00 zobrazíte ukázku.</span><span class="sxs-lookup"><span data-stu-id="8c7a3-141">Fast-forward to 21:00 to see the demo.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
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




