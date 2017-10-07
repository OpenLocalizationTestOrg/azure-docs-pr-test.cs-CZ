---
title: "aaaAzure Service Fabric reverse zabezpečené komunikace proxy | Microsoft Docs"
description: "Nakonfigurujte reverzní proxy server tooenable zabezpečené komunikace začátku do konce."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: e1248dffe2c324373ad0d09d3f5f094db74480d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-secure-service-with-hello-reverse-proxy"></a><span data-ttu-id="6ab33-103">Připojení ke službám zabezpečené tooa reverzní proxy server hello</span><span class="sxs-lookup"><span data-stu-id="6ab33-103">Connect tooa secure service with hello reverse proxy</span></span>

<span data-ttu-id="6ab33-104">Tento článek vysvětluje, jak tooestablish zabezpečené připojení mezi hello reverzní proxy server a služby, a tak umožňuje end tooend zabezpečený kanál.</span><span class="sxs-lookup"><span data-stu-id="6ab33-104">This article explains how tooestablish secure connection between hello reverse proxy and services, thus enabling an end tooend secure channel.</span></span>

<span data-ttu-id="6ab33-105">Připojení služby toosecure je podporována pouze v případě reverzní proxy server je nakonfigurovaný toolisten na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6ab33-105">Connecting toosecure services is supported only when reverse proxy is configured toolisten on HTTPS.</span></span> <span data-ttu-id="6ab33-106">Zbytek dokumentu hello předpokládá, že se že jedná o případ hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-106">Rest of hello document assumes this is hello case.</span></span>
<span data-ttu-id="6ab33-107">Odkazovat příliš[reverzní proxy server v Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello reverzní proxy server v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6ab33-107">Refer too[Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello reverse proxy in Service Fabric.</span></span>

## <a name="secure-connection-establishment-between-hello-reverse-proxy-and-services"></a><span data-ttu-id="6ab33-108">Navázání zabezpečeného připojení mezi hello reverzní proxy server a služby</span><span class="sxs-lookup"><span data-stu-id="6ab33-108">Secure connection establishment between hello reverse proxy and services</span></span> 

### <a name="reverse-proxy-authenticating-tooservices"></a><span data-ttu-id="6ab33-109">Reverse tooservices ověřování proxy serveru:</span><span class="sxs-lookup"><span data-stu-id="6ab33-109">Reverse proxy authenticating tooservices:</span></span>
<span data-ttu-id="6ab33-110">Hello reverzní proxy server se identifikuje tooservices pomocí jeho certifikát zadaný u ***reverseProxyCertificate*** vlastnost hello **clusteru** [části Typ prostředku](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6ab33-110">hello reverse proxy identifies itself tooservices using its certificate, specified with ***reverseProxyCertificate*** property in hello **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="6ab33-111">Služby můžete implementovat hello logiku tooverify hello certifikát předložený hello reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="6ab33-111">Services can implement hello logic tooverify hello certificate presented by hello reverse proxy.</span></span> <span data-ttu-id="6ab33-112">Hello služby můžete zadat podrobnosti o certifikátu klienta hello přijata jako nastavení konfigurace v hello konfigurační balíček.</span><span class="sxs-lookup"><span data-stu-id="6ab33-112">hello services can specify hello accepted client certificate details as configuration settings in hello configuration package.</span></span> <span data-ttu-id="6ab33-113">To může číst za běhu a použít toovalidate hello certifikát předložený hello reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="6ab33-113">This can be read at runtime and used toovalidate hello certificate presented by hello reverse proxy.</span></span> <span data-ttu-id="6ab33-114">Odkazovat příliš[spravovat aplikace parametry](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello konfigurační nastavení.</span><span class="sxs-lookup"><span data-stu-id="6ab33-114">Refer too[Manage application parameters](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello configuration settings.</span></span> 

### <a name="reverse-proxy-verifying-hello-services-identity-via-hello-certificate-presented-by-hello-service"></a><span data-ttu-id="6ab33-115">Reverse proxy server ověření identity hello služby prostřednictvím hello certifikát předložený hello služby:</span><span class="sxs-lookup"><span data-stu-id="6ab33-115">Reverse proxy verifying hello service's identity via hello certificate presented by hello service:</span></span>
<span data-ttu-id="6ab33-116">ověřování certifikátu serveru tooperform hello certifikátů předložený hello služby, reverzní proxy server podporuje některý z hello následující možnosti: None, ServiceCommonNameAndIssuer a ServiceCertificateThumbprints.</span><span class="sxs-lookup"><span data-stu-id="6ab33-116">tooperform server certificate validation of hello certificates presented by hello services, reverse proxy supports one of hello following options: None, ServiceCommonNameAndIssuer, and ServiceCertificateThumbprints.</span></span>
<span data-ttu-id="6ab33-117">tooselect jednu z těchto tří možností, zadejte hello **ApplicationCertificateValidationPolicy** v části Parametry hello ApplicationGateway/Http prvek v rámci [fabricSettings](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="6ab33-117">tooselect one of these three options, specify hello **ApplicationCertificateValidationPolicy** in hello parameters section of ApplicationGateway/Http element under [fabricSettings](service-fabric-cluster-fabric-settings.md).</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "None"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="6ab33-118">Podrobnosti o další konfiguraci pro každý z těchto možností najdete v další části toohello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-118">Refer toohello next section for details about additional configuration for each of these options.</span></span>

### <a name="service-certificate-validation-options"></a><span data-ttu-id="6ab33-119">Možnosti ověřování certifikátu služby</span><span class="sxs-lookup"><span data-stu-id="6ab33-119">Service certificate validation options</span></span> 

- <span data-ttu-id="6ab33-120">**Žádný**: reverzní proxy server přeskočí ověření certifikátu hello směrovány přes proxy server služby a navázat zabezpečené připojení hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-120">**None**: Reverse proxy skips verification of hello proxied service certificate and establishes hello secure connection.</span></span> <span data-ttu-id="6ab33-121">Toto je výchozí chování hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-121">This is hello default behavior.</span></span>
<span data-ttu-id="6ab33-122">Zadejte hello **ApplicationCertificateValidationPolicy** s hodnotou **žádné** v části Parametry hello elementu ApplicationGateway/Http.</span><span class="sxs-lookup"><span data-stu-id="6ab33-122">Specify hello **ApplicationCertificateValidationPolicy** with value **None** in hello parameters section of ApplicationGateway/Http element.</span></span>

- <span data-ttu-id="6ab33-123">**ServiceCommonNameAndIssuer**: reverzní proxy server ověří hello certifikát předložený hello služby založené na běžný název certifikátu a kryptografický otisk vystavitelů okamžitou: Zadejte hello **ApplicationCertificateValidationPolicy**  s hodnotou **ServiceCommonNameAndIssuer** v části Parametry hello elementu ApplicationGateway/Http.</span><span class="sxs-lookup"><span data-stu-id="6ab33-123">**ServiceCommonNameAndIssuer**: Reverse proxy verifies hello certificate presented by hello service based on certificate's common name and immediate issuer's thumbprint: Specify hello **ApplicationCertificateValidationPolicy** with value **ServiceCommonNameAndIssuer** in hello parameters section of ApplicationGateway/Http element.</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCommonNameAndIssuer"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="6ab33-124">toospecify hello seznam běžný název služby a vystavitele kryptografické otisky, přidejte **ApplicationGateway/Http/ServiceCommonNameAndIssuer** prvek v rámci fabricSettings, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6ab33-124">toospecify hello list of service common name and issuer thumbprints, add a **ApplicationGateway/Http/ServiceCommonNameAndIssuer** element under fabricSettings, as shown below.</span></span> <span data-ttu-id="6ab33-125">V elementu pole hello parametry lze přidat více párů kryptografický otisk vystavitelů a běžný název certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6ab33-125">Multiple certificate common name and issuer thumbprint pairs can be added in hello parameters array element.</span></span> 

<span data-ttu-id="6ab33-126">Pokud reverzní proxy server hello koncový bod se připojuje toopresents certifikát, který je běžné jméno a vystavitele kryptografický otisk odpovídá některému z hello hodnoty zadané v tomto poli, je vytvořit kanál SSL.</span><span class="sxs-lookup"><span data-stu-id="6ab33-126">If hello endpoint reverse proxy is connecting toopresents a certificate who's common name and  issuer thumbprint matches any of hello values specified here, SSL channel is established.</span></span> <span data-ttu-id="6ab33-127">Při toomatch hello certifikát podrobnosti o chybě se nezdaří reverzní proxy server požadavek klienta hello se 502 stavový kód (chybný brána).</span><span class="sxs-lookup"><span data-stu-id="6ab33-127">Upon failure toomatch hello certificate details, reverse proxy fails hello client's request with a 502 (Bad Gateway) status code.</span></span> <span data-ttu-id="6ab33-128">Hello řádku stav protokolu HTTP bude také obsahovat hello frázi "Neplatný certifikát SSL."</span><span class="sxs-lookup"><span data-stu-id="6ab33-128">hello HTTP status line will also contain hello phrase "Invalid SSL Certificate."</span></span> 

```json
{
"fabricSettings": [
          ...
          {
             "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
            "parameters": [
              {
                "name": "WinFabric-Test-Certificate-CN1",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
              },
              {
                "name": "WinFabric-Test-Certificate-CN2",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
              }
            ]
          }
        ],
        ...
}
```


- <span data-ttu-id="6ab33-129">**ServiceCertificateThumbprints**: reverzní proxy server ověří podle jeho kryptografický otisk certifikátu směrovány přes proxy server služby hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-129">**ServiceCertificateThumbprints**: Reverse proxy will verify hello proxied service certificate based on its thumbprint.</span></span> <span data-ttu-id="6ab33-130">Můžete zvolit toogo tato trasa při hello služby jsou nakonfigurovány s certifikáty podepsané svými držiteli: Zadejte hello **ApplicationCertificateValidationPolicy** s hodnotou **ServiceCertificateThumbprints**v části Parametry hello elementu ApplicationGateway/Http.</span><span class="sxs-lookup"><span data-stu-id="6ab33-130">You can choose toogo this route when hello services are configured with self signed certificates: Specify hello **ApplicationCertificateValidationPolicy** with value **ServiceCertificateThumbprints** in hello parameters section of ApplicationGateway/Http element.</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCertificateThumbprints"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="6ab33-131">Také zadat kryptografické otisky hello s **ServiceCertificateThumbprints** položky v části parametry elementu ApplicationGateway/Http.</span><span class="sxs-lookup"><span data-stu-id="6ab33-131">Also specify hello thumbprints with a **ServiceCertificateThumbprints** entry under parameters section of ApplicationGateway/Http element.</span></span> <span data-ttu-id="6ab33-132">Více kryptografické otisky lze zadat jako seznam oddělený čárkami v poli hodnota hello, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6ab33-132">Multiple thumbprints can be specified as a comma-separated list in hello value field, as shown below:</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "ServiceCertificateThumbprints",
                "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="6ab33-133">Pokud hello kryptografický otisk certifikátu serveru hello je uveden v této položce Konfigurace, bude úspěšná reverzní proxy server hello připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="6ab33-133">If hello thumbprint of hello server certificate is listed in this config entry, reverse proxy succeeds hello SSL connection.</span></span> <span data-ttu-id="6ab33-134">Jinak ukončí hello připojení a selže hello požadavek klienta se 502 (chybný brána).</span><span class="sxs-lookup"><span data-stu-id="6ab33-134">Otherwise, it terminates hello connection and fails hello client's request with a 502 (Bad Gateway).</span></span> <span data-ttu-id="6ab33-135">Hello řádku stav protokolu HTTP bude také obsahovat hello frázi "Neplatný certifikát SSL."</span><span class="sxs-lookup"><span data-stu-id="6ab33-135">hello HTTP status line will also contain hello phrase "Invalid SSL Certificate."</span></span>

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a><span data-ttu-id="6ab33-136">Koncový bod výběr logika při vystavení zabezpečené, jakož i zabezpečená koncové body služby</span><span class="sxs-lookup"><span data-stu-id="6ab33-136">Endpoint selection logic when services expose secure as well as unsecured endpoints</span></span>
<span data-ttu-id="6ab33-137">Služba fabric podporuje konfiguraci více koncových bodů pro službu.</span><span class="sxs-lookup"><span data-stu-id="6ab33-137">Service fabric supports configuring  multiple endpoints for a service.</span></span> <span data-ttu-id="6ab33-138">Viz [Určení prostředků v manifestu služby](service-fabric-service-manifest-resources.md).</span><span class="sxs-lookup"><span data-stu-id="6ab33-138">See [Specify resources in a service manifest](service-fabric-service-manifest-resources.md).</span></span>

<span data-ttu-id="6ab33-139">Reverzní proxy server vybere jeden hello koncové body tooforward hello žádosti podle hello **ListenerName** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="6ab33-139">Reverse proxy selects one of hello endpoints tooforward hello request based on hello  **ListenerName** query parameter.</span></span> <span data-ttu-id="6ab33-140">Pokud není zadáno, můžete vybrat libovolný koncový bod, ze seznamu koncových bodů hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-140">If this is not specified, it can pick any endpoint from hello endpoints list.</span></span> <span data-ttu-id="6ab33-141">Teď to může být koncový bod HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6ab33-141">Now this can be an HTTP or HTTPS endpoint.</span></span> <span data-ttu-id="6ab33-142">Mohou existovat scénáře nebo požadavky na místo, kam chcete toooperate reverzní proxy server hello v "zabezpečený pouze režim", jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="6ab33-142">There might be scenarios/requirements where you want hello reverse proxy toooperate in a "secure only mode", i.e</span></span> <span data-ttu-id="6ab33-143">nechcete, aby hello zabezpečené reverzní proxy server tooforward požadavky toounsecured koncové body.</span><span class="sxs-lookup"><span data-stu-id="6ab33-143">you don't want hello secure reverse proxy tooforward requests toounsecured endpoints.</span></span> <span data-ttu-id="6ab33-144">Toho lze dosáhnout zadáním hello **SecureOnlyMode** položku konfigurace s hodnotou **true** v části Parametry hello elementu ApplicationGateway/Http.</span><span class="sxs-lookup"><span data-stu-id="6ab33-144">This can be achieved by specifying hello **SecureOnlyMode** configuration entry with value **true** in hello parameters section of ApplicationGateway/Http element.</span></span>   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> 
> <span data-ttu-id="6ab33-145">Při provozu v **SecureOnlyMode**, pokud má zadanou klienta **ListenerName** odpovídající tooan HTTP(unsecured) koncový bod, reverzní proxy server selže hello žádost s 404 stavový kód HTTP (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="6ab33-145">When operating in **SecureOnlyMode**, if client has specified a **ListenerName** corresponding tooan HTTP(unsecured) endpoint, reverse proxy fails hello request with a 404 (Not Found) HTTP status code.</span></span>

## <a name="setting-up-client-certificate-authentication-through-hello-reverse-proxy"></a><span data-ttu-id="6ab33-146">Nastavení ověřování pomocí certifikátu klienta přes reverzní proxy server hello</span><span class="sxs-lookup"><span data-stu-id="6ab33-146">Setting up client certificate authentication through hello reverse proxy</span></span>
<span data-ttu-id="6ab33-147">Probíhá ukončení protokolu SSL na reverzní proxy server hello a dojde ke ztrátě všech dat certifikát klienta hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-147">SSL termination happens at hello reverse proxy and all hello client certificate data is lost.</span></span> <span data-ttu-id="6ab33-148">Pro ověření hello služby tooperform klientského certifikátu, nastavte hello **ForwardClientCertificate** nastavení v části Parametry hello elementu ApplicationGateway/Http.</span><span class="sxs-lookup"><span data-stu-id="6ab33-148">For hello services tooperform client certificate authentication, set hello **ForwardClientCertificate** setting in hello parameters section of ApplicationGateway/Http element.</span></span>

1. <span data-ttu-id="6ab33-149">Při **ForwardClientCertificate** je nastaven příliš**false**, reverzní proxy server nebude vyžadovat certifikátu klienta se hello během jeho metody handshake SSL s klientem hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-149">When **ForwardClientCertificate** is set too**false**, reverse proxy will not request for hello client certificate during its SSL handshake with hello client.</span></span>
<span data-ttu-id="6ab33-150">Toto je výchozí chování hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-150">This is hello default behavior.</span></span>

2. <span data-ttu-id="6ab33-151">Když **ForwardClientCertificate** je nastaven příliš**true**, reverse proxy žádosti o certifikát klienta hello během jeho metody handshake SSL s klientem hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-151">When **ForwardClientCertificate** is set too**true**, reverse proxy requests for hello client's certificate during its SSL handshake with hello client.</span></span>
<span data-ttu-id="6ab33-152">Bude poté předávat hello klienta data certifikátu v vlastní hlavičku HTTP s názvem **certifikát klienta X**.</span><span class="sxs-lookup"><span data-stu-id="6ab33-152">It will then forward hello client certificate data in a custom HTTP header named **X-Client-Certificate**.</span></span> <span data-ttu-id="6ab33-153">Hodnota hlavičky Hello je řetězec s kódováním base64 PEM formátu hello hello klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6ab33-153">hello header value is hello base64 encoded PEM format string of hello client's certificate.</span></span> <span data-ttu-id="6ab33-154">Hello služby můžete být úspěšné, nebo selhání hello žádost s odpovídající stavový kód po kontrole data certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-154">hello service can succeed/fail hello request with appropriate status code after inspecting hello certificate data.</span></span>
<span data-ttu-id="6ab33-155">Pokud hello klienta nemá k dispozici certifikát, reverzní proxy server předává hlavičku prázdný a nechat hello služby popisovač hello případu.</span><span class="sxs-lookup"><span data-stu-id="6ab33-155">If hello client does not present a certificate, reverse proxy forwards an empty header and let hello service handle hello case.</span></span>

> <span data-ttu-id="6ab33-156">Reverzní proxy server je pouhé předávání.</span><span class="sxs-lookup"><span data-stu-id="6ab33-156">Reverse proxy is a mere forwarder.</span></span> <span data-ttu-id="6ab33-157">Ho nebude provádět žádné ověření certifikátu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="6ab33-157">It will not perform any validation of hello client's certificate.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6ab33-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ab33-158">Next steps</span></span>
* <span data-ttu-id="6ab33-159">Odkazovat příliš[konfigurovat reverzní proxy server tooconnect toosecure služby](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) pro Azure Resource Manager šablony ukázky tooconfigure zabezpečené reverzní proxy server s možností ověřování certifikátu hello jinou službu.</span><span class="sxs-lookup"><span data-stu-id="6ab33-159">Refer too[Configure reverse proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples tooconfigure secure reverse proxy with hello different service certificate validation options.</span></span>
* <span data-ttu-id="6ab33-160">Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkového projektu na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="6ab33-160">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="6ab33-161">Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="6ab33-161">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="6ab33-162">Webové rozhraní API, která používá OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="6ab33-162">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="6ab33-163">Správa certifikátů clusteru</span><span class="sxs-lookup"><span data-stu-id="6ab33-163">Manage cluster certificates</span></span>](service-fabric-cluster-security-update-certs-azure.md)
