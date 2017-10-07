---
title: "na základě aaaURL obsahu směrování přehled | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello na základě adresy URL aplikace brány obsahu směrování, konfigurace UrlPathMap a PathBasedRouting pravidlo."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: gwallace
ms.openlocfilehash: 5094b42625baffeb395beace68db0d269e46080c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="url-path-based-routing-overview"></a><span data-ttu-id="8faae-103">Přehled směrování na základě cest URL</span><span class="sxs-lookup"><span data-stu-id="8faae-103">URL Path Based Routing overview</span></span>

<span data-ttu-id="8faae-104">Směrování na základě cesty adres URL umožňuje vám tooroute provoz tooback-end serveru fondy podle cest URL požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="8faae-104">URL Path Based Routing allows you tooroute traffic tooback-end server pools based on URL Paths of hello request.</span></span> 

<span data-ttu-id="8faae-105">Jedním z hello scénáře je tooroute požadavky pro různé typy obsahu toodifferent back-end serveru fondy.</span><span class="sxs-lookup"><span data-stu-id="8faae-105">One of hello scenarios is tooroute requests for different content types toodifferent backend server pools.</span></span>

<span data-ttu-id="8faae-106">V následujícím příkladu hello, Application Gateway obsluhuje přenosy dat pro contoso.com ze tří fondů back-end serverů například: VideoServerPool, ImageServerPool a DefaultServerPool.</span><span class="sxs-lookup"><span data-stu-id="8faae-106">In hello following example, Application Gateway is serving traffic for contoso.com from three back-end server pools for example: VideoServerPool, ImageServerPool, and DefaultServerPool.</span></span>

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

<span data-ttu-id="8faae-108">Požadavky pro http://contoso.com/video * jsou směrované tooVideoServerPool a http://contoso.com/images * jsou směrované tooImageServerPool.</span><span class="sxs-lookup"><span data-stu-id="8faae-108">Requests for http://contoso.com/video* are routed tooVideoServerPool, and http://contoso.com/images* are routed tooImageServerPool.</span></span> <span data-ttu-id="8faae-109">DefaultServerPool je vybraná, pokud cesta vzory hello neodpovídají.</span><span class="sxs-lookup"><span data-stu-id="8faae-109">DefaultServerPool is selected if none of hello path patterns match.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8faae-110">Pravidla se zpracovávají v pořadí hello, jsou uvedeny v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="8faae-110">Rules are processed in hello order they are listed in hello portal.</span></span> <span data-ttu-id="8faae-111">Je první předchozí tooconfiguring velmi doporučené tooconfigure naslouchací procesy více lokalit základní naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="8faae-111">It is highly recommended tooconfigure multi-site listeners first prior tooconfiguring a basic listener.</span></span>  <span data-ttu-id="8faae-112">Tím se zajistí ukončení tento provoz získá směrované toohello přímo zpět.</span><span class="sxs-lookup"><span data-stu-id="8faae-112">This ensures that traffic gets routed toohello right back end.</span></span> <span data-ttu-id="8faae-113">Pokud je základní naslouchací proces uveden jako první a odpovídá příchozímu požadavku, požadavek se zpracuje tímto naslouchacím procesem.</span><span class="sxs-lookup"><span data-stu-id="8faae-113">If a basic listener is listed first and matches an incoming request, it gets processed by that listener.</span></span>

## <a name="urlpathmap-configuration-element"></a><span data-ttu-id="8faae-114">Konfigurační prvek UrlPathMap</span><span class="sxs-lookup"><span data-stu-id="8faae-114">UrlPathMap configuration element</span></span>

<span data-ttu-id="8faae-115">Hello urlPathMap element je použité toospecify cesta vzory tooback-end serveru fondu mapování.</span><span class="sxs-lookup"><span data-stu-id="8faae-115">hello urlPathMap element is used toospecify Path patterns tooback-end server pool mappings.</span></span> <span data-ttu-id="8faae-116">Hello následující ukázka kódu je fragment kódu hello elementu urlPathMap ze souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="8faae-116">hello following code example is hello snippet of urlPathMap element from template file.</span></span>

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> <span data-ttu-id="8faae-117">PathPattern: Toto nastavení je seznam vzorů toomatch cesta.</span><span class="sxs-lookup"><span data-stu-id="8faae-117">PathPattern: This setting is a list of path patterns toomatch.</span></span> <span data-ttu-id="8faae-118">Každý musí začínat znakem / a jediným místem hello "*" je povolena, je v hello end následující "/".</span><span class="sxs-lookup"><span data-stu-id="8faae-118">Each must start with / and hello only place a "*" is allowed is at hello end following a "/."</span></span> <span data-ttu-id="8faae-119">Hello řetězec dodáni toohello cesta objekt přiřazení vzorce nezahrnuje jakýkoli text po hello nejprve? nebo # a tyto znaky nejsou povoleny v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="8faae-119">hello string fed toohello path matcher does not include any text after hello first? or #, and those chars are not allowed here.</span></span>

<span data-ttu-id="8faae-120">Více informací najdete v dokumentu [Šablona Resource Manageru používající směrování na základě adresy URL](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing).</span><span class="sxs-lookup"><span data-stu-id="8faae-120">You can check out a [Resource Manager template using URL-based routing](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) for more information.</span></span>

## <a name="pathbasedrouting-rule"></a><span data-ttu-id="8faae-121">Pravidlo PathBasedRouting</span><span class="sxs-lookup"><span data-stu-id="8faae-121">PathBasedRouting rule</span></span>

<span data-ttu-id="8faae-122">RequestRoutingRule typu PathBasedRouting je použité toobind urlPathMap tooa naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="8faae-122">RequestRoutingRule of type PathBasedRouting is used toobind a listener tooa urlPathMap.</span></span> <span data-ttu-id="8faae-123">Všechny požadavky přijaté tímto naslouchacím procesem jsou směrovány na základě zásad zadaných v UrlPathMap.</span><span class="sxs-lookup"><span data-stu-id="8faae-123">All requests that are received for this listener are routed based on policy specified in urlPathMap.</span></span>
<span data-ttu-id="8faae-124">Fragment pravidla PathBasedRouting:</span><span class="sxs-lookup"><span data-stu-id="8faae-124">Snippet of PathBasedRouting rule:</span></span>

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a><span data-ttu-id="8faae-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8faae-125">Next steps</span></span>

<span data-ttu-id="8faae-126">Po seznamovat založené na adrese URL obsahu směrování, přejděte příliš[vytvoření služby application gateway pomocí směrování na základě adresy URL](application-gateway-create-url-route-portal.md) toocreate aplikační brány s pravidel směrování adres URL.</span><span class="sxs-lookup"><span data-stu-id="8faae-126">After learning about URL-based content routing, go too[create an application gateway using URL-based routing](application-gateway-create-url-route-portal.md) toocreate an application gateway with URL routing rules.</span></span>
