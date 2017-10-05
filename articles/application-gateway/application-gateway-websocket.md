---
title: Podpora protokolu WebSocket v Azure Application Gateway | Microsoft Docs
description: "Tato stránka obsahuje přehled podpory protokolu WebSocket brány aplikace."
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
ms.openlocfilehash: 75b06ddd02da231b7813c609c848c75e42116da5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a><span data-ttu-id="99b50-103">Přehled podpory protokolu WebSocket v aplikační brány</span><span class="sxs-lookup"><span data-stu-id="99b50-103">Overview of WebSocket support in Application Gateway</span></span>

<span data-ttu-id="99b50-104">Application Gateway poskytuje nativní podporu pro WebSocket napříč všech velikostí brány.</span><span class="sxs-lookup"><span data-stu-id="99b50-104">Application Gateway provides native support for WebSocket across all gateway sizes.</span></span> <span data-ttu-id="99b50-105">Neexistuje žádné uživatelsky konfigurovatelného nastavení selektivně povolení nebo zakázání podpory protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99b50-105">There is no user-configurable setting to selectively enable or disable WebSocket support.</span></span> 

<span data-ttu-id="99b50-106">Protokol WebSocket standardizované v [RFC6455](https://tools.ietf.org/html/rfc6455) umožňuje plně duplexní komunikace mezi serverem a klientem přes dlouhotrvající připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="99b50-106">WebSocket protocol standardized in [RFC6455](https://tools.ietf.org/html/rfc6455) enables a full duplex communication between a server and a client over a long running TCP connection.</span></span> <span data-ttu-id="99b50-107">Tato funkce umožňuje více interaktivní komunikace mezi webového serveru a klienta, které může být obousměrné bez nutnosti dotazování jako požadavků ve implementace založené na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="99b50-107">This feature allows for a more interactive communication between the web server and the client, which can be bidirectional without the need for polling as required in HTTP-based implementations.</span></span> <span data-ttu-id="99b50-108">WebSocket má nedostatek nároky na rozdíl od protokolu HTTP a můžete znovu použít stejné připojení protokolu TCP pro více požadavku nebo odpovědi, výsledkem je efektivnější využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="99b50-108">WebSocket has low overhead unlike HTTP and can reuse the same TCP connection for multiple request/responses resulting in a more efficient utilization of resources.</span></span> <span data-ttu-id="99b50-109">Protokol WebSocket protokoly jsou navrženy pro práci přes tradiční HTTP porty 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="99b50-109">WebSocket protocols are designed to work over traditional HTTP ports of 80 and 443.</span></span>

<span data-ttu-id="99b50-110">Můžete nadále používat standardní naslouchací proces protokolu HTTP na portu 80 nebo 443 přijímat provoz protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99b50-110">You can continue using a standard HTTP listener on port 80 or 443 to receive WebSocket traffic.</span></span> <span data-ttu-id="99b50-111">Provoz protokolu WebSocket je pak přesměrované na serveru povoleno back-end protokolu WebSocket pomocí příslušné back-endový fond uvedeného v pravidlech brány aplikace.</span><span class="sxs-lookup"><span data-stu-id="99b50-111">WebSocket traffic is then directed to the WebSocket enabled backend server using the appropriate backend pool as specified in application gateway rules.</span></span> <span data-ttu-id="99b50-112">Back-end serveru musí reagovat na sondy brány aplikace, které jsou popsány v [přehled test stavu](application-gateway-probe-overview.md) části.</span><span class="sxs-lookup"><span data-stu-id="99b50-112">The backend server must respond to the application gateway probes, which are described in the [health probe overview](application-gateway-probe-overview.md) section.</span></span> <span data-ttu-id="99b50-113">Sondy stavu služby Application gateway jsou pouze prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="99b50-113">Application gateway health probes are HTTP/HTTPS only.</span></span> <span data-ttu-id="99b50-114">Každý server back-end musí odpovědět na sondy HTTP pro službu application gateway pro směrování provozu protokolu WebSocket k serveru.</span><span class="sxs-lookup"><span data-stu-id="99b50-114">Each backend server must respond to HTTP probes for application gateway to route WebSocket traffic to the server.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="99b50-115">Konfigurační prvek naslouchacího procesu</span><span class="sxs-lookup"><span data-stu-id="99b50-115">Listener configuration element</span></span>

<span data-ttu-id="99b50-116">Existující naslouchací proces protokolu HTTP lze použít pro podporu přenosů protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99b50-116">An existing HTTP listener can be used to support WebSocket traffic.</span></span> <span data-ttu-id="99b50-117">Zde je fragment element httpListeners z ukázkového souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="99b50-117">The following is a snippet of an httpListeners element from a sample template file.</span></span> <span data-ttu-id="99b50-118">Potřebovali byste naslouchací procesy HTTP a HTTPS pro podporu protokolu WebSocket a zabezpečení přenosů protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99b50-118">You would need both HTTP and HTTPS listeners to support WebSocket and secure WebSocket traffic.</span></span> <span data-ttu-id="99b50-119">Podobně můžete použít [portál](application-gateway-create-gateway-portal.md) nebo [prostředí PowerShell](application-gateway-create-gateway-arm.md) k vytvoření aplikační brány s moduly pro naslouchání na portu 80/443 pro podporu přenosů protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99b50-119">Similarly you can use the [portal](application-gateway-create-gateway-portal.md) or [PowerShell](application-gateway-create-gateway-arm.md) to create an application gateway with listeners on port 80/443 to support WebSocket traffic.</span></span>

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a><span data-ttu-id="99b50-120">Konfigurace pravidla BackendAddressPool, BackendHttpSetting a směrování</span><span class="sxs-lookup"><span data-stu-id="99b50-120">BackendAddressPool, BackendHttpSetting, and Routing rule configuration</span></span>

<span data-ttu-id="99b50-121">BackendAddressPool se používá k definování fond back-end servery protokolu WebSocket povoleno.</span><span class="sxs-lookup"><span data-stu-id="99b50-121">A BackendAddressPool is used to define a backend pool with WebSocket enabled servers.</span></span> <span data-ttu-id="99b50-122">BackendHttpSetting je definována pomocí back-end port 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="99b50-122">The backendHttpSetting is defined with a backend port 80 and 443.</span></span> <span data-ttu-id="99b50-123">Vlastnosti pro spřažení na základě souborů cookie a requestTimeouts nejsou relevantní pro přenosy protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="99b50-123">The properties for cookie-based affinity and requestTimeouts are not relevant to WebSocket traffic.</span></span> <span data-ttu-id="99b50-124">Neexistuje žádná změna v pravidlo směrování, "Základní" se používá ke svázání odpovídající naslouchací proces a odpovídající fondu adres back-end.</span><span class="sxs-lookup"><span data-stu-id="99b50-124">There is no change required in the routing rule, 'Basic' is used to tie the appropriate listener to the corresponding backend address pool.</span></span> 

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

## <a name="websocket-enabled-backend"></a><span data-ttu-id="99b50-125">Back-end WebSocket povoleno</span><span class="sxs-lookup"><span data-stu-id="99b50-125">WebSocket enabled backend</span></span>

<span data-ttu-id="99b50-126">Váš back-end musí mít protokolu HTTP nebo HTTPS webový server spuštěný ve konfigurovaného portu pro protokolu WebSocket pro práci (obvykle 80/443).</span><span class="sxs-lookup"><span data-stu-id="99b50-126">Your backend must have a HTTP/HTTPS web server running on the configured port (usually 80/443) for WebSocket to work.</span></span> <span data-ttu-id="99b50-127">Tento požadavek je, protože protokol WebSocket vyžaduje počáteční handshake být upgradu protokolu WebSocket jako pole hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="99b50-127">This requirement is because WebSocket protocol requires the initial handshake to be HTTP with upgrade to WebSocket protocol as a header field.</span></span> <span data-ttu-id="99b50-128">Následuje příklad hlavičky:</span><span class="sxs-lookup"><span data-stu-id="99b50-128">The following is an example of a header:</span></span>

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

<span data-ttu-id="99b50-129">Dalším důvodem je, že tento test stavu back-end brány aplikace podporuje pouze protokoly HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="99b50-129">Another reason for this is that application gateway backend health probe supports HTTP and HTTPS protocols only.</span></span> <span data-ttu-id="99b50-130">Pokud back-end serveru neodpovídá na protokolu HTTP nebo HTTPS sondy, se dostala mimo fond back-end.</span><span class="sxs-lookup"><span data-stu-id="99b50-130">If the backend server does not respond to HTTP or HTTPS probes, it is taken out of backend pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99b50-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99b50-131">Next steps</span></span>

<span data-ttu-id="99b50-132">Po získání informací o podporu protokolu WebSocket, přejděte na [vytvoření služby application gateway](application-gateway-create-gateway.md) začít pracovat s protokolu WebSocket webové aplikace s podporou.</span><span class="sxs-lookup"><span data-stu-id="99b50-132">After learning about WebSocket support, go to [create an application gateway](application-gateway-create-gateway.md) to get started with a WebSocket enabled web application.</span></span>

