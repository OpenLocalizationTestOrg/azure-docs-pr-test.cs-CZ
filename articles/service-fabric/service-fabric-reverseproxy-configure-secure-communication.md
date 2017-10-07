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
# <a name="connect-tooa-secure-service-with-hello-reverse-proxy"></a>Připojení ke službám zabezpečené tooa reverzní proxy server hello

Tento článek vysvětluje, jak tooestablish zabezpečené připojení mezi hello reverzní proxy server a služby, a tak umožňuje end tooend zabezpečený kanál.

Připojení služby toosecure je podporována pouze v případě reverzní proxy server je nakonfigurovaný toolisten na HTTPS. Zbytek dokumentu hello předpokládá, že se že jedná o případ hello.
Odkazovat příliš[reverzní proxy server v Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello reverzní proxy server v Service Fabric.

## <a name="secure-connection-establishment-between-hello-reverse-proxy-and-services"></a>Navázání zabezpečeného připojení mezi hello reverzní proxy server a služby 

### <a name="reverse-proxy-authenticating-tooservices"></a>Reverse tooservices ověřování proxy serveru:
Hello reverzní proxy server se identifikuje tooservices pomocí jeho certifikát zadaný u ***reverseProxyCertificate*** vlastnost hello **clusteru** [části Typ prostředku](../azure-resource-manager/resource-group-authoring-templates.md). Služby můžete implementovat hello logiku tooverify hello certifikát předložený hello reverzní proxy server. Hello služby můžete zadat podrobnosti o certifikátu klienta hello přijata jako nastavení konfigurace v hello konfigurační balíček. To může číst za běhu a použít toovalidate hello certifikát předložený hello reverzní proxy server. Odkazovat příliš[spravovat aplikace parametry](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello konfigurační nastavení. 

### <a name="reverse-proxy-verifying-hello-services-identity-via-hello-certificate-presented-by-hello-service"></a>Reverse proxy server ověření identity hello služby prostřednictvím hello certifikát předložený hello služby:
ověřování certifikátu serveru tooperform hello certifikátů předložený hello služby, reverzní proxy server podporuje některý z hello následující možnosti: None, ServiceCommonNameAndIssuer a ServiceCertificateThumbprints.
tooselect jednu z těchto tří možností, zadejte hello **ApplicationCertificateValidationPolicy** v části Parametry hello ApplicationGateway/Http prvek v rámci [fabricSettings](service-fabric-cluster-fabric-settings.md).

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

Podrobnosti o další konfiguraci pro každý z těchto možností najdete v další části toohello.

### <a name="service-certificate-validation-options"></a>Možnosti ověřování certifikátu služby 

- **Žádný**: reverzní proxy server přeskočí ověření certifikátu hello směrovány přes proxy server služby a navázat zabezpečené připojení hello. Toto je výchozí chování hello.
Zadejte hello **ApplicationCertificateValidationPolicy** s hodnotou **žádné** v části Parametry hello elementu ApplicationGateway/Http.

- **ServiceCommonNameAndIssuer**: reverzní proxy server ověří hello certifikát předložený hello služby založené na běžný název certifikátu a kryptografický otisk vystavitelů okamžitou: Zadejte hello **ApplicationCertificateValidationPolicy**  s hodnotou **ServiceCommonNameAndIssuer** v části Parametry hello elementu ApplicationGateway/Http.

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

toospecify hello seznam běžný název služby a vystavitele kryptografické otisky, přidejte **ApplicationGateway/Http/ServiceCommonNameAndIssuer** prvek v rámci fabricSettings, jak je uvedeno níže. V elementu pole hello parametry lze přidat více párů kryptografický otisk vystavitelů a běžný název certifikátu. 

Pokud reverzní proxy server hello koncový bod se připojuje toopresents certifikát, který je běžné jméno a vystavitele kryptografický otisk odpovídá některému z hello hodnoty zadané v tomto poli, je vytvořit kanál SSL. Při toomatch hello certifikát podrobnosti o chybě se nezdaří reverzní proxy server požadavek klienta hello se 502 stavový kód (chybný brána). Hello řádku stav protokolu HTTP bude také obsahovat hello frázi "Neplatný certifikát SSL." 

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


- **ServiceCertificateThumbprints**: reverzní proxy server ověří podle jeho kryptografický otisk certifikátu směrovány přes proxy server služby hello. Můžete zvolit toogo tato trasa při hello služby jsou nakonfigurovány s certifikáty podepsané svými držiteli: Zadejte hello **ApplicationCertificateValidationPolicy** s hodnotou **ServiceCertificateThumbprints**v části Parametry hello elementu ApplicationGateway/Http.

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

Také zadat kryptografické otisky hello s **ServiceCertificateThumbprints** položky v části parametry elementu ApplicationGateway/Http. Více kryptografické otisky lze zadat jako seznam oddělený čárkami v poli hodnota hello, jak je uvedeno níže:

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

Pokud hello kryptografický otisk certifikátu serveru hello je uveden v této položce Konfigurace, bude úspěšná reverzní proxy server hello připojení SSL. Jinak ukončí hello připojení a selže hello požadavek klienta se 502 (chybný brána). Hello řádku stav protokolu HTTP bude také obsahovat hello frázi "Neplatný certifikát SSL."

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Koncový bod výběr logika při vystavení zabezpečené, jakož i zabezpečená koncové body služby
Služba fabric podporuje konfiguraci více koncových bodů pro službu. Viz [Určení prostředků v manifestu služby](service-fabric-service-manifest-resources.md).

Reverzní proxy server vybere jeden hello koncové body tooforward hello žádosti podle hello **ListenerName** parametr dotazu. Pokud není zadáno, můžete vybrat libovolný koncový bod, ze seznamu koncových bodů hello. Teď to může být koncový bod HTTP nebo HTTPS. Mohou existovat scénáře nebo požadavky na místo, kam chcete toooperate reverzní proxy server hello v "zabezpečený pouze režim", jednofaktorovému nechcete, aby hello zabezpečené reverzní proxy server tooforward požadavky toounsecured koncové body. Toho lze dosáhnout zadáním hello **SecureOnlyMode** položku konfigurace s hodnotou **true** v části Parametry hello elementu ApplicationGateway/Http.   

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
> Při provozu v **SecureOnlyMode**, pokud má zadanou klienta **ListenerName** odpovídající tooan HTTP(unsecured) koncový bod, reverzní proxy server selže hello žádost s 404 stavový kód HTTP (není nalezena).

## <a name="setting-up-client-certificate-authentication-through-hello-reverse-proxy"></a>Nastavení ověřování pomocí certifikátu klienta přes reverzní proxy server hello
Probíhá ukončení protokolu SSL na reverzní proxy server hello a dojde ke ztrátě všech dat certifikát klienta hello. Pro ověření hello služby tooperform klientského certifikátu, nastavte hello **ForwardClientCertificate** nastavení v části Parametry hello elementu ApplicationGateway/Http.

1. Při **ForwardClientCertificate** je nastaven příliš**false**, reverzní proxy server nebude vyžadovat certifikátu klienta se hello během jeho metody handshake SSL s klientem hello.
Toto je výchozí chování hello.

2. Když **ForwardClientCertificate** je nastaven příliš**true**, reverse proxy žádosti o certifikát klienta hello během jeho metody handshake SSL s klientem hello.
Bude poté předávat hello klienta data certifikátu v vlastní hlavičku HTTP s názvem **certifikát klienta X**. Hodnota hlavičky Hello je řetězec s kódováním base64 PEM formátu hello hello klientského certifikátu. Hello služby můžete být úspěšné, nebo selhání hello žádost s odpovídající stavový kód po kontrole data certifikátu hello.
Pokud hello klienta nemá k dispozici certifikát, reverzní proxy server předává hlavičku prázdný a nechat hello služby popisovač hello případu.

> Reverzní proxy server je pouhé předávání. Ho nebude provádět žádné ověření certifikátu klienta hello.


## <a name="next-steps"></a>Další kroky
* Odkazovat příliš[konfigurovat reverzní proxy server tooconnect toosecure služby](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) pro Azure Resource Manager šablony ukázky tooconfigure zabezpečené reverzní proxy server s možností ověřování certifikátu hello jinou službu.
* Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkového projektu na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)
* [Webové rozhraní API, která používá OWIN v spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)
* [Správa certifikátů clusteru](service-fabric-cluster-security-update-certs-azure.md)
