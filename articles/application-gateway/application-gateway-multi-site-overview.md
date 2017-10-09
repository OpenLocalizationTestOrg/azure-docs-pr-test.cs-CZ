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
# <a name="application-gateway-multiple-site-hosting"></a><span data-ttu-id="70baa-103">Hostování více webů ve službě Application Gateway</span><span class="sxs-lookup"><span data-stu-id="70baa-103">Application Gateway multiple site hosting</span></span>

<span data-ttu-id="70baa-104">Hostování více lokality umožňuje tooconfigure více než jednu webovou aplikaci na hello stejnou instanci brány aplikace.</span><span class="sxs-lookup"><span data-stu-id="70baa-104">Multiple site hosting enables you tooconfigure more than one web application on hello same application gateway instance.</span></span> <span data-ttu-id="70baa-105">Tato funkce umožňuje tooconfigure efektivnější topologie nasazení přidáním až too20 weby tooone aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="70baa-105">This feature allows you tooconfigure a more efficient topology for your deployments by adding up too20 websites tooone application gateway.</span></span> <span data-ttu-id="70baa-106">Každý web může přesměrovat tooits vlastní fond back-end.</span><span class="sxs-lookup"><span data-stu-id="70baa-106">Each website can be directed tooits own backend pool.</span></span> <span data-ttu-id="70baa-107">V následujícím příkladu hello Aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com ze dvou fondů back-end serverů názvem ContosoServerPool a FabrikamServerPool.</span><span class="sxs-lookup"><span data-stu-id="70baa-107">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com from two back-end server pools called ContosoServerPool and FabrikamServerPool.</span></span>

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

> [!IMPORTANT]
> <span data-ttu-id="70baa-109">Pravidla se zpracovávají v pořadí hello, jsou uvedeny v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="70baa-109">Rules are processed in hello order they are listed in hello portal.</span></span> <span data-ttu-id="70baa-110">Je první předchozí tooconfiguring velmi doporučené tooconfigure naslouchací procesy více lokalit základní naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="70baa-110">It is highly recommended tooconfigure multi-site listeners first prior tooconfiguring a basic listener.</span></span>  <span data-ttu-id="70baa-111">Tím bude zajištěno ukončení tento provoz získá směrované toohello přímo zpět.</span><span class="sxs-lookup"><span data-stu-id="70baa-111">This will ensure that traffic gets routed toohello right back end.</span></span> <span data-ttu-id="70baa-112">Pokud je základní naslouchací proces uveden jako první a odpovídá příchozímu požadavku, požadavek se zpracuje tímto naslouchacím procesem.</span><span class="sxs-lookup"><span data-stu-id="70baa-112">If a basic listener is listed first and matches an incoming request, it gets processed by that listener.</span></span>

<span data-ttu-id="70baa-113">Požadavky pro http://contoso.com jsou směrované tooContosoServerPool a http://fabrikam.com směrované tooFabrikamServerPool.</span><span class="sxs-lookup"><span data-stu-id="70baa-113">Requests for http://contoso.com are routed tooContosoServerPool, and http://fabrikam.com are routed tooFabrikamServerPool.</span></span>

<span data-ttu-id="70baa-114">Podobně jako dvě subdomény hello stejné nadřazené domény může být hostitelem hello stejné brány nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="70baa-114">Similarly two subdomains of hello same parent domain can be hosted on hello same application gateway deployment.</span></span> <span data-ttu-id="70baa-115">Příklady použití poddomén mohou zahrnovat http://blog.contoso.com a http://app.contoso.com hostované v jednom nasazení služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="70baa-115">Examples of using subdomains could include http://blog.contoso.com and http://app.contoso.com hosted on a single application gateway deployment.</span></span>

## <a name="host-headers-and-server-name-indication-sni"></a><span data-ttu-id="70baa-116">Hlavičky hostitele a Identifikace názvu serveru (SNI)</span><span class="sxs-lookup"><span data-stu-id="70baa-116">Host headers and Server Name Indication (SNI)</span></span>

<span data-ttu-id="70baa-117">Existují tři běžné mechanismy pro povolení více lokality hostování na hello stejné infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="70baa-117">There are three common mechanisms for enabling multiple site hosting on hello same infrastructure.</span></span>

1. <span data-ttu-id="70baa-118">Hostování více webových aplikací, z nichž každá je na jedinečné IP adrese.</span><span class="sxs-lookup"><span data-stu-id="70baa-118">Host multiple web applications each on a unique IP address.</span></span>
2. <span data-ttu-id="70baa-119">Název hostitele použít toohost několika webových aplikací na hello stejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="70baa-119">Use host name toohost multiple web applications on hello same IP address.</span></span>
3. <span data-ttu-id="70baa-120">Použití jiné porty toohost několika webových aplikací na hello stejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="70baa-120">Use different ports toohost multiple web applications on hello same IP address.</span></span>

<span data-ttu-id="70baa-121">V současné době získá služba Application Gateway jednu veřejnou IP adresu, na které naslouchá provozu.</span><span class="sxs-lookup"><span data-stu-id="70baa-121">Currently an application gateway gets a single public IP address on which it listens for traffic.</span></span> <span data-ttu-id="70baa-122">Proto v současné době podpora více aplikací, z nichž každá má vlastní IP adresu, není podporována.</span><span class="sxs-lookup"><span data-stu-id="70baa-122">Therefore supporting multiple applications, each with its own IP address, is currently not supported.</span></span> <span data-ttu-id="70baa-123">Aplikační brána podporuje hostování více aplikací každý naslouchá na jiné porty, ale v tomto scénáři by vyžadovaly hello aplikace tooaccept provoz na nestandardní porty a není často požadovaná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="70baa-123">Application Gateway supports hosting multiple applications each listening on different ports but this scenario would require hello applications tooaccept traffic on non-standard ports and is often not a desired configuration.</span></span> <span data-ttu-id="70baa-124">Aplikační brána spoléhá na HTTP 1.1 toohost hlavičky hostitele více než jeden web na hello stejnou veřejnou IP adresu a port.</span><span class="sxs-lookup"><span data-stu-id="70baa-124">Application Gateway relies on HTTP 1.1 host headers toohost more than one website on hello same public IP address and port.</span></span> <span data-ttu-id="70baa-125">Hello webů hostovaných na aplikační bránu můžete taky podporu snižování zátěže protokolu SSL s příponou TLS indikace názvu serveru (SNI).</span><span class="sxs-lookup"><span data-stu-id="70baa-125">hello sites hosted on application gateway can also support SSL offload with Server Name Indication (SNI) TLS extension.</span></span> <span data-ttu-id="70baa-126">Tento scénář znamená, že tento hello klienta prohlížeče a back-endové webové farmy musí podporovat protokol HTTP/1.1 a TLS rozšíření, jak jsou definovány v dokumentu RFC 6066.</span><span class="sxs-lookup"><span data-stu-id="70baa-126">This scenario means that hello client browser and backend web farm must support HTTP/1.1 and TLS extension as defined in RFC 6066.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="70baa-127">Konfigurační prvek naslouchacího procesu</span><span class="sxs-lookup"><span data-stu-id="70baa-127">Listener configuration element</span></span>

<span data-ttu-id="70baa-128">Existující HTTPListener konfigurační prvek je lepší toosupport hostitelský název serveru název indikace elementy a, který je využíván jiným aplikace brány tooroute provoz tooappropriate back-end fondu.</span><span class="sxs-lookup"><span data-stu-id="70baa-128">Existing HTTPListener configuration element is enhanced toosupport host name and server name indication elements, which is used by application gateway tooroute traffic tooappropriate backend pool.</span></span> <span data-ttu-id="70baa-129">Hello následující ukázka kódu je fragment kódu hello elementu HttpListeners ze souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="70baa-129">hello following code example is hello snippet of HttpListeners element from template file.</span></span>

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

<span data-ttu-id="70baa-130">Můžete navštívit [šablony Resource Manageru pomocí více hostování lokality](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) nasazení založené na šablonách tooend end.</span><span class="sxs-lookup"><span data-stu-id="70baa-130">You can visit [Resource Manager template using multiple site hosting](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) for an end tooend template-based deployment.</span></span>

## <a name="routing-rule"></a><span data-ttu-id="70baa-131">Pravidlo směrování</span><span class="sxs-lookup"><span data-stu-id="70baa-131">Routing rule</span></span>

<span data-ttu-id="70baa-132">Neexistuje žádná změna v pravidlo směrování hello.</span><span class="sxs-lookup"><span data-stu-id="70baa-132">There is no change required in hello routing rule.</span></span> <span data-ttu-id="70baa-133">Hello pravidlo směrování, že "Základní" by měly pokračovat toobe vybrali tootie hello příslušnou lokalitu naslouchací proces toohello odpovídající back-end fondu adres.</span><span class="sxs-lookup"><span data-stu-id="70baa-133">hello routing rule 'Basic' should continue toobe chosen tootie hello appropriate site listener toohello corresponding backend address pool.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="70baa-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70baa-134">Next steps</span></span>

<span data-ttu-id="70baa-135">Po seznamovat více lokality hostování, přejít příliš[vytvoření služby application gateway pomocí více hostování lokality](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate aplikační brány s možnost toosupport více než jednu webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70baa-135">After learning about multiple site hosting, go too[create an application gateway using multiple site hosting](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate an application gateway with ability toosupport more than one web application.</span></span>

