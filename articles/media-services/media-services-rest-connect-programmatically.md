---
title: "Připojení k účtu Media Services pomocí rozhraní REST API | Microsoft Docs"
description: "Toto téma ukazuje, jak se připojit ke službě Media Services pomocí rozhraní REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 79dc64f1-15d8-4a81-b9d9-3d3c44d2e1e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 4feb0eb81823835e8e0b701463d85b27f5598019
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Připojení k účtu Media Services pomocí Media Services REST API
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-connect-programmatically.md)
> * [REST](media-services-rest-connect-programmatically.md)
> 
> 

Toto téma popisuje, jak získat programové připojení k Microsoft Azure Media Services, pokud jsou programování s Media Services REST API.

Při přístupu k Microsoft Azure Media Services jsou požadovány dvě věci: přístupový token poskytovaný služby Řízení přístupu Azure (ACS) a identifikátor URI Media Services sám sebe. Můžete použít jakýmkoli způsobem, který má být při vytváření tyto požadavky, dokud, zadejte hodnoty hlavičky správné a předejte v tokenu přístupu správně při volání do služby Media Services.

Následující kroky popisují nejběžnější pracovního postupu při použití Media Services REST API pro připojení ke službě Media Services:

1. Získání tokenu přístupu 
2. Připojení ke službě Media Services identifikátor URI 
   
   > [!NOTE]
   > Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání nový identifikátor URI.
   > Také můžete obdržet odpovědi HTTP/1.1 200, která obsahuje popis metadat rozhraní API ODATA.
   > 
   > 
3. POST následující volání rozhraní API nové adrese URL. 
   
    Například pokud po pokusu o připojení, vy máte následující:
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    Publikujte následující volání rozhraní API https://wamsbayclus001rest-hs.cloudapp.net/api/.

    >[!NOTE]
    >Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad. Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

## <a name="access-control-address"></a>Adresy MAC
Služba Media Services adresy MAC je https://wamsprodglobal001acs.accesscontrol.windows.net, s výjimkou oblast Severní Čína, kde je https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

## <a name="getting-an-access-token"></a>Získání tokenu přístupu
Přístup k Media Services přímo prostřednictvím REST API, načtou přístupový token ze služby ACS a použití při každé žádosti protokolu HTTP, které provedete ve službě. Tento token je podobná dalších tokenů poskytované služby ACS na základě deklarací přístup zadaná v hlavičce požadavku HTTP a pomocí protokolu OAuth v2. Nepotřebujete žádné další nezbytné součásti před přímým připojením ke službě Media Services.

Následující příklad ukazuje hlavičku požadavku HTTP a text, na které se používá k načtení tokenu.

**Záhlaví**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


**Text**:

Budete potřebovat prokázat, že hodnoty client_id a tajný klíč client_secret v těle této žádosti; client_id a tajný klíč client_secret odpovídají hodnoty AccountName a AccountKey. Tyto hodnoty jsou které jste získali od Media Services při nastavování účtu. 

Všimněte si, že AccountKey pro váš účet Media Services musí být kódovaná adresou URL (najdete v části [kódování procent](http://tools.ietf.org/html/rfc3986#section-2.1) při použití jako hodnota tajný klíč client_secret v žádosti o token přístupu.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Například: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Následující příklad ukazuje odpověď HTTP, který obsahuje přístup v textu odpovědi tokenu.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670

    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }


> [!NOTE]
> Doporučuje se pro ukládání do mezipaměti hodnoty "access_token" a "expires_in" na externí úložiště. Token dat může později načíst z úložiště a opětovně využít v voláními rozhraní REST API Media Services. To je obzvláště užitečná pro scénáře, kde token lze bezpečně sdílet mezi více procesy nebo počítače.
> 
> 

Zajistěte, aby ke sledování hodnotu "expires_in" přístupový token a podle potřeby aktualizujte voláními rozhraní REST API s nové tokeny.

### <a name="connecting-to-the-media-services-uri"></a>Připojení ke službě Media Services identifikátor URI
Kořenová identifikátor URI pro Media Services je https://media.windows.net/. Měli nejdřív připojit k tento identifikátor URI, a pokud jste zpátky v odpovědi 301 přesměrování, byste měli následující volání nový identifikátor URI. Kromě toho nepoužívejte veškeré auto přesměrování nebo postupujte podle logiky své žádosti. Příkazy HTTP a těla požadavku nebudou předávány na nový identifikátor URI.

Všimněte si, že kořenový identifikátor URI pro nahrávání a stahování souborů Asset https://yourstorageaccount.blob.core.windows.net/ kde název účtu úložiště je stejný jako ten, který jste použili při nastavení vašeho účtu Media Services.

Následující příklad ukazuje, požadavek HTTP do kořenového adresáře Media Services identifikátor URI (https://media.windows.net/). Požadavek získá zpět v odpovědi 301 přesměrování. Další požadavek používá nový identifikátor URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Požadavek HTTP**:

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**Odpověď HTTP**:

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164

    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**Požadavek HTTP** (pomocí nový identifikátor URI):

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**Odpověď HTTP**:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}



> [!NOTE]
> Jakmile se zobrazí nový identifikátor URI, který je identifikátor URI, který se má použít pro komunikaci pomocí služby Media Services. 
> 
> 

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

