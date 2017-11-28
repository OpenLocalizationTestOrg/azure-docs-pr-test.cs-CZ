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
# <a name="overview-of-websocket-support-in-application-gateway"></a><span data-ttu-id="dd35e-103">Přehled podpory protokolu WebSocket v aplikační brány</span><span class="sxs-lookup"><span data-stu-id="dd35e-103">Overview of WebSocket support in Application Gateway</span></span>

<span data-ttu-id="dd35e-104">Application Gateway poskytuje nativní podporu pro WebSocket napříč všech velikostí brány.</span><span class="sxs-lookup"><span data-stu-id="dd35e-104">Application Gateway provides native support for WebSocket across all gateway sizes.</span></span> <span data-ttu-id="dd35e-105">Neexistuje žádné uživatelsky konfigurovatelného nastavení tooselectively povolit nebo zakázat podporu protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-105">There is no user-configurable setting tooselectively enable or disable WebSocket support.</span></span> 

<span data-ttu-id="dd35e-106">Protokol WebSocket standardizované v [RFC6455](https://tools.ietf.org/html/rfc6455) umožňuje plně duplexní komunikace mezi serverem a klientem přes dlouhotrvající připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="dd35e-106">WebSocket protocol standardized in [RFC6455](https://tools.ietf.org/html/rfc6455) enables a full duplex communication between a server and a client over a long running TCP connection.</span></span> <span data-ttu-id="dd35e-107">Tato funkce umožňuje více interaktivní komunikace mezi hello webového serveru a klienta hello, což může být obousměrné bez nutnosti hello dotazování jako požadavků ve implementace založené na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd35e-107">This feature allows for a more interactive communication between hello web server and hello client, which can be bidirectional without hello need for polling as required in HTTP-based implementations.</span></span> <span data-ttu-id="dd35e-108">WebSocket má nedostatek nároky na rozdíl od protokolu HTTP a můžete opakované použití hello stejné připojení TCP pro více požadavku nebo odpovědi, výsledkem je efektivnější využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="dd35e-108">WebSocket has low overhead unlike HTTP and can reuse hello same TCP connection for multiple request/responses resulting in a more efficient utilization of resources.</span></span> <span data-ttu-id="dd35e-109">Protokol WebSocket protokoly jsou navrženou toowork přes tradiční HTTP porty 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="dd35e-109">WebSocket protocols are designed toowork over traditional HTTP ports of 80 and 443.</span></span>

<span data-ttu-id="dd35e-110">Můžete nadále používat standardní naslouchací proces protokolu HTTP na portu 80 nebo 443 tooreceive provoz protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-110">You can continue using a standard HTTP listener on port 80 or 443 tooreceive WebSocket traffic.</span></span> <span data-ttu-id="dd35e-111">Provoz protokolu WebSocket je povoleno back-end serveru pomocí hello odpovídající back-endový fond uvedených v pravidlech brány aplikace řízené toohello protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-111">WebSocket traffic is then directed toohello WebSocket enabled backend server using hello appropriate backend pool as specified in application gateway rules.</span></span> <span data-ttu-id="dd35e-112">Hello back-end serveru musí odpovídat toohello aplikace brány sondy, které jsou popsány v hello [přehled test stavu](application-gateway-probe-overview.md) části.</span><span class="sxs-lookup"><span data-stu-id="dd35e-112">hello backend server must respond toohello application gateway probes, which are described in hello [health probe overview](application-gateway-probe-overview.md) section.</span></span> <span data-ttu-id="dd35e-113">Sondy stavu služby Application gateway jsou pouze prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dd35e-113">Application gateway health probes are HTTP/HTTPS only.</span></span> <span data-ttu-id="dd35e-114">Každý server back-end musí odpovědět tooHTTP sondy pro aplikační bránu tooroute protokolu WebSocket provoz toohello server.</span><span class="sxs-lookup"><span data-stu-id="dd35e-114">Each backend server must respond tooHTTP probes for application gateway tooroute WebSocket traffic toohello server.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="dd35e-115">Konfigurační prvek naslouchacího procesu</span><span class="sxs-lookup"><span data-stu-id="dd35e-115">Listener configuration element</span></span>

<span data-ttu-id="dd35e-116">Existující naslouchací proces protokolu HTTP lze použít toosupport provoz protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-116">An existing HTTP listener can be used toosupport WebSocket traffic.</span></span> <span data-ttu-id="dd35e-117">Následující Hello je fragment elementu httpListeners z ukázkového souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="dd35e-117">hello following is a snippet of an httpListeners element from a sample template file.</span></span> <span data-ttu-id="dd35e-118">Bude potřebovat HTTP a HTTPS toosupport naslouchací procesy protokolu WebSocket a zabezpečení přenosů protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-118">You would need both HTTP and HTTPS listeners toosupport WebSocket and secure WebSocket traffic.</span></span> <span data-ttu-id="dd35e-119">Podobně můžete použít hello [portál](application-gateway-create-gateway-portal.md) nebo [prostředí PowerShell](application-gateway-create-gateway-arm.md) toocreate aplikační brány s moduly pro naslouchání na portu 80/443 toosupport protokolu WebSocket provoz.</span><span class="sxs-lookup"><span data-stu-id="dd35e-119">Similarly you can use hello [portal](application-gateway-create-gateway-portal.md) or [PowerShell](application-gateway-create-gateway-arm.md) toocreate an application gateway with listeners on port 80/443 toosupport WebSocket traffic.</span></span>

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a><span data-ttu-id="dd35e-120">Konfigurace pravidla BackendAddressPool, BackendHttpSetting a směrování</span><span class="sxs-lookup"><span data-stu-id="dd35e-120">BackendAddressPool, BackendHttpSetting, and Routing rule configuration</span></span>

<span data-ttu-id="dd35e-121">BackendAddressPool je použité toodefine fond back-end servery protokolu WebSocket povoleno.</span><span class="sxs-lookup"><span data-stu-id="dd35e-121">A BackendAddressPool is used toodefine a backend pool with WebSocket enabled servers.</span></span> <span data-ttu-id="dd35e-122">Hello backendHttpSetting je definována pomocí back-end port 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="dd35e-122">hello backendHttpSetting is defined with a backend port 80 and 443.</span></span> <span data-ttu-id="dd35e-123">Hello vlastnosti pro spřažení na základě souborů cookie a requestTimeouts nejsou relevantní tooWebSocket provoz.</span><span class="sxs-lookup"><span data-stu-id="dd35e-123">hello properties for cookie-based affinity and requestTimeouts are not relevant tooWebSocket traffic.</span></span> <span data-ttu-id="dd35e-124">Neexistuje žádná změna v pravidlo směrování hello požadován, "Základní" je použité tootie hello odpovídající naslouchací proces toohello odpovídající fondu adres back-end.</span><span class="sxs-lookup"><span data-stu-id="dd35e-124">There is no change required in hello routing rule, 'Basic' is used tootie hello appropriate listener toohello corresponding backend address pool.</span></span> 

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

## <a name="websocket-enabled-backend"></a><span data-ttu-id="dd35e-125">Back-end WebSocket povoleno</span><span class="sxs-lookup"><span data-stu-id="dd35e-125">WebSocket enabled backend</span></span>

<span data-ttu-id="dd35e-126">Váš back-end musí mít protokolu HTTP nebo HTTPS webový server spuštěný ve hello nakonfigurovat port (obvykle 80/443) pro toowork protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-126">Your backend must have a HTTP/HTTPS web server running on hello configured port (usually 80/443) for WebSocket toowork.</span></span> <span data-ttu-id="dd35e-127">Tento požadavek je, protože protokol WebSocket vyžaduje hello počáteční handshake toobe HTTP s protokolem upgradu tooWebSocket jako pole hlavičky.</span><span class="sxs-lookup"><span data-stu-id="dd35e-127">This requirement is because WebSocket protocol requires hello initial handshake toobe HTTP with upgrade tooWebSocket protocol as a header field.</span></span> <span data-ttu-id="dd35e-128">Hello tady je příklad hlavičky:</span><span class="sxs-lookup"><span data-stu-id="dd35e-128">hello following is an example of a header:</span></span>

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

<span data-ttu-id="dd35e-129">Dalším důvodem je, že tento test stavu back-end brány aplikace podporuje pouze protokoly HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dd35e-129">Another reason for this is that application gateway backend health probe supports HTTP and HTTPS protocols only.</span></span> <span data-ttu-id="dd35e-130">Pokud hello back-end serveru neodpovídá tooHTTP nebo sondy HTTPS, je vyjmuta z fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="dd35e-130">If hello backend server does not respond tooHTTP or HTTPS probes, it is taken out of backend pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd35e-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd35e-131">Next steps</span></span>

<span data-ttu-id="dd35e-132">Po získání informací o podporu protokolu WebSocket, přejděte příliš[vytvoření služby application gateway](application-gateway-create-gateway.md) webové aplikace s podporou tooget začít s protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dd35e-132">After learning about WebSocket support, go too[create an application gateway](application-gateway-create-gateway.md) tooget started with a WebSocket enabled web application.</span></span>

