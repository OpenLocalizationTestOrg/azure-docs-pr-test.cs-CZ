---
title: Podpora aaaWebSocket v Azure Application Gateway | Microsoft Docs
description: "Tato stránka obsahuje přehled hello podpory protokolu WebSocket brány aplikace."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: amsriva
ms.openlocfilehash: 3776117803e8559ad243c2d4c3dd661199c1e48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Přehled podpory protokolu WebSocket v aplikační brány

Application Gateway poskytuje nativní podporu pro WebSocket napříč všech velikostí brány. Neexistuje žádné uživatelsky konfigurovatelného nastavení tooselectively povolit nebo zakázat podporu protokolu WebSocket. 

Protokol WebSocket standardizované v [RFC6455](https://tools.ietf.org/html/rfc6455) umožňuje plně duplexní komunikace mezi serverem a klientem přes dlouhotrvající připojení TCP. Tato funkce umožňuje více interaktivní komunikace mezi hello webového serveru a klienta hello, což může být obousměrné bez nutnosti hello dotazování jako požadavků ve implementace založené na protokolu HTTP. WebSocket má nedostatek nároky na rozdíl od protokolu HTTP a můžete opakované použití hello stejné připojení TCP pro více požadavku nebo odpovědi, výsledkem je efektivnější využití prostředků. Protokol WebSocket protokoly jsou navrženou toowork přes tradiční HTTP porty 80 a 443.

Můžete nadále používat standardní naslouchací proces protokolu HTTP na portu 80 nebo 443 tooreceive provoz protokolu WebSocket. Provoz protokolu WebSocket je povoleno back-end serveru pomocí hello odpovídající back-endový fond uvedených v pravidlech brány aplikace řízené toohello protokolu WebSocket. Hello back-end serveru musí odpovídat toohello aplikace brány sondy, které jsou popsány v hello [přehled test stavu](application-gateway-probe-overview.md) části. Sondy stavu služby Application gateway jsou pouze prostřednictvím protokolu HTTP nebo HTTPS. Každý server back-end musí odpovědět tooHTTP sondy pro aplikační bránu tooroute protokolu WebSocket provoz toohello server.

## <a name="listener-configuration-element"></a>Konfigurační prvek naslouchacího procesu

Existující naslouchací proces protokolu HTTP lze použít toosupport provoz protokolu WebSocket. Následující Hello je fragment elementu httpListeners z ukázkového souboru šablony. Bude potřebovat HTTP a HTTPS toosupport naslouchací procesy protokolu WebSocket a zabezpečení přenosů protokolu WebSocket. Podobně můžete použít hello [portál](application-gateway-create-gateway-portal.md) nebo [prostředí PowerShell](application-gateway-create-gateway-arm.md) toocreate aplikační brány s moduly pro naslouchání na portu 80/443 toosupport protokolu WebSocket provoz.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Konfigurace pravidla BackendAddressPool, BackendHttpSetting a směrování

BackendAddressPool je použité toodefine fond back-end servery protokolu WebSocket povoleno. Hello backendHttpSetting je definována pomocí back-end port 80 a 443. Hello vlastnosti pro spřažení na základě souborů cookie a requestTimeouts nejsou relevantní tooWebSocket provoz. Neexistuje žádná změna v pravidlo směrování hello požadován, "Základní" je použité tootie hello odpovídající naslouchací proces toohello odpovídající fondu adres back-end. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>Back-end WebSocket povoleno

Váš back-end musí mít protokolu HTTP nebo HTTPS webový server spuštěný ve hello nakonfigurovat port (obvykle 80/443) pro toowork protokolu WebSocket. Tento požadavek je, protože protokol WebSocket vyžaduje hello počáteční handshake toobe HTTP s protokolem upgradu tooWebSocket jako pole hlavičky. Hello tady je příklad hlavičky:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Dalším důvodem je, že tento test stavu back-end brány aplikace podporuje pouze protokoly HTTP a HTTPS. Pokud hello back-end serveru neodpovídá tooHTTP nebo sondy HTTPS, je vyjmuta z fondu back-end.

## <a name="next-steps"></a>Další kroky

Po získání informací o podporu protokolu WebSocket, přejděte příliš[vytvoření služby application gateway](application-gateway-create-gateway.md) webové aplikace s podporou tooget začít s protokolu WebSocket.

