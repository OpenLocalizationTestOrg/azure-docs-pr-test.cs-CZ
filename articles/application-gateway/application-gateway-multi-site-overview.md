---
title: "aaaHosting více lokalit na Azure Application Gateway | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello podporu více lokalit Application Gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: 
ms.assetid: 49993fd2-87e5-4a66-b386-8d22056a616d
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: 4ab6faa97f1891d7525affdaa36463681bf99e9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-multiple-site-hosting"></a>Hostování více webů ve službě Application Gateway

Hostování více lokality umožňuje tooconfigure více než jednu webovou aplikaci na hello stejnou instanci brány aplikace. Tato funkce umožňuje tooconfigure efektivnější topologie nasazení přidáním až too20 weby tooone aplikační brány. Každý web může přesměrovat tooits vlastní fond back-end. V následujícím příkladu hello Aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com ze dvou fondů back-end serverů názvem ContosoServerPool a FabrikamServerPool.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

> [!IMPORTANT]
> Pravidla se zpracovávají v pořadí hello, jsou uvedeny v portálu hello. Je první předchozí tooconfiguring velmi doporučené tooconfigure naslouchací procesy více lokalit základní naslouchací proces.  Tím bude zajištěno ukončení tento provoz získá směrované toohello přímo zpět. Pokud je základní naslouchací proces uveden jako první a odpovídá příchozímu požadavku, požadavek se zpracuje tímto naslouchacím procesem.

Požadavky pro http://contoso.com jsou směrované tooContosoServerPool a http://fabrikam.com směrované tooFabrikamServerPool.

Podobně jako dvě subdomény hello stejné nadřazené domény může být hostitelem hello stejné brány nasazení aplikace. Příklady použití poddomén mohou zahrnovat http://blog.contoso.com a http://app.contoso.com hostované v jednom nasazení služby Application Gateway.

## <a name="host-headers-and-server-name-indication-sni"></a>Hlavičky hostitele a Identifikace názvu serveru (SNI)

Existují tři běžné mechanismy pro povolení více lokality hostování na hello stejné infrastruktury.

1. Hostování více webových aplikací, z nichž každá je na jedinečné IP adrese.
2. Název hostitele použít toohost několika webových aplikací na hello stejnou IP adresu.
3. Použití jiné porty toohost několika webových aplikací na hello stejnou IP adresu.

V současné době získá služba Application Gateway jednu veřejnou IP adresu, na které naslouchá provozu. Proto v současné době podpora více aplikací, z nichž každá má vlastní IP adresu, není podporována. Aplikační brána podporuje hostování více aplikací každý naslouchá na jiné porty, ale v tomto scénáři by vyžadovaly hello aplikace tooaccept provoz na nestandardní porty a není často požadovaná konfigurace. Aplikační brána spoléhá na HTTP 1.1 toohost hlavičky hostitele více než jeden web na hello stejnou veřejnou IP adresu a port. Hello webů hostovaných na aplikační bránu můžete taky podporu snižování zátěže protokolu SSL s příponou TLS indikace názvu serveru (SNI). Tento scénář znamená, že tento hello klienta prohlížeče a back-endové webové farmy musí podporovat protokol HTTP/1.1 a TLS rozšíření, jak jsou definovány v dokumentu RFC 6066.

## <a name="listener-configuration-element"></a>Konfigurační prvek naslouchacího procesu

Existující HTTPListener konfigurační prvek je lepší toosupport hostitelský název serveru název indikace elementy a, který je využíván jiným aplikace brány tooroute provoz tooappropriate back-end fondu. Hello následující ukázka kódu je fragment kódu hello elementu HttpListeners ze souboru šablony.

```json
"httpListeners": [
    {
        "name": "appGatewayHttpsListener1",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
            },
            "Protocol": "Https",
            "SslCertificate": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
            },
            "HostName": "contoso.com",
            "RequireServerNameIndication": "true"
        }
    },
    {
        "name": "appGatewayHttpListener2",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
            },
            "Protocol": "Http",
            "HostName": "fabrikam.com",
            "RequireServerNameIndication": "false"
        }
    }
],
```

Můžete navštívit [šablony Resource Manageru pomocí více hostování lokality](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) nasazení založené na šablonách tooend end.

## <a name="routing-rule"></a>Pravidlo směrování

Neexistuje žádná změna v pravidlo směrování hello. Hello pravidlo směrování, že "Základní" by měly pokračovat toobe vybrali tootie hello příslušnou lokalitu naslouchací proces toohello odpovídající back-end fondu adres.

```json
"requestRoutingRules": [
{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

},
{
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}
]
```

## <a name="next-steps"></a>Další kroky

Po seznamovat více lokality hostování, přejít příliš[vytvoření služby application gateway pomocí více hostování lokality](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate aplikační brány s možnost toosupport více než jednu webovou aplikaci.

