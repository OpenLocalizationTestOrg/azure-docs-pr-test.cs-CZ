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
# <a name="url-path-based-routing-overview"></a>Přehled směrování na základě cest URL

Směrování na základě cesty adres URL umožňuje vám tooroute provoz tooback-end serveru fondy podle cest URL požadavku hello. 

Jedním z hello scénáře je tooroute požadavky pro různé typy obsahu toodifferent back-end serveru fondy.

V následujícím příkladu hello, Application Gateway obsluhuje přenosy dat pro contoso.com ze tří fondů back-end serverů například: VideoServerPool, ImageServerPool a DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Požadavky pro http://contoso.com/video * jsou směrované tooVideoServerPool a http://contoso.com/images * jsou směrované tooImageServerPool. DefaultServerPool je vybraná, pokud cesta vzory hello neodpovídají.

> [!IMPORTANT]
> Pravidla se zpracovávají v pořadí hello, jsou uvedeny v portálu hello. Je první předchozí tooconfiguring velmi doporučené tooconfigure naslouchací procesy více lokalit základní naslouchací proces.  Tím se zajistí ukončení tento provoz získá směrované toohello přímo zpět. Pokud je základní naslouchací proces uveden jako první a odpovídá příchozímu požadavku, požadavek se zpracuje tímto naslouchacím procesem.

## <a name="urlpathmap-configuration-element"></a>Konfigurační prvek UrlPathMap

Hello urlPathMap element je použité toospecify cesta vzory tooback-end serveru fondu mapování. Hello následující ukázka kódu je fragment kódu hello elementu urlPathMap ze souboru šablony.

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
> PathPattern: Toto nastavení je seznam vzorů toomatch cesta. Každý musí začínat znakem / a jediným místem hello "*" je povolena, je v hello end následující "/". Hello řetězec dodáni toohello cesta objekt přiřazení vzorce nezahrnuje jakýkoli text po hello nejprve? nebo # a tyto znaky nejsou povoleny v tomto poli.

Více informací najdete v dokumentu [Šablona Resource Manageru používající směrování na základě adresy URL](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing).

## <a name="pathbasedrouting-rule"></a>Pravidlo PathBasedRouting

RequestRoutingRule typu PathBasedRouting je použité toobind urlPathMap tooa naslouchací proces. Všechny požadavky přijaté tímto naslouchacím procesem jsou směrovány na základě zásad zadaných v UrlPathMap.
Fragment pravidla PathBasedRouting:

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

## <a name="next-steps"></a>Další kroky

Po seznamovat založené na adrese URL obsahu směrování, přejděte příliš[vytvoření služby application gateway pomocí směrování na základě adresy URL](application-gateway-create-url-route-portal.md) toocreate aplikační brány s pravidel směrování adres URL.
