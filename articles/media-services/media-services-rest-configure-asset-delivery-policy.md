---
title: "zásady doručení mediálního aaaConfiguring pomocí Media Services REST API | Microsoft Docs"
description: "Toto téma ukazuje, jak pomocí Media Services REST API zásady pro doručení tooconfigure jiný prostředek."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 8203230d570935e17382c598820dbfe42f83f8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-asset-delivery-policies"></a>Konfigurace zásad doručení assetu
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Pokud máte v plánu prostředky toodeliver dynamicky šifrovat, jeden z hello kroků v hello Media Services obsahu doručení pracovního postupu je konfigurace zásad doručení pro prostředky. zásady doručení assetu Hello informuje Media Services, jak chcete použít pro váš asset toobe doručit: do streamování protokol, který by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete toodynamically váš asset šifrovat a jak (obálky nebo common encryption).

Toto téma popisuje, proč a jak toocreate a nakonfigurujte zásady doručení assetu.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 
>
>Navíc toobe možné toouse dynamické balením a dynamickým šifrováním váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.

Je možné aplikovat různé zásady toohello stejné asset. Například může použít PlayReady šifrování tooSmooth Streaming a pomocí standardu AES Envelope šifrování tooMPEG DASH a HLS. Veškeré protokoly, které nejsou v zásadách doručení definovány (například přidáte jedinou zásadu, která jako hello protokol určuje pouze HLS) budou při streamování blokovány. Výjimka toothis Hello je, pokud máte definovány vůbec žádné zásady doručení assetu. Potom bude možné v hello zrušte všechny protokoly.

Pokud chcete toodeliver asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello. Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení. Například toodeliver asset šifrován Advanced Encryption (Standard AES) obálky šifrovací klíč, nastavte typ zásad hello příliš**DynamicEnvelopeEncryption**. šifrování úložiště tooremove a asset hello datový proud v hello jasné, nastavte typ zásad hello příliš**NoDynamicEncryption**. Příklady, které ukazují, jak tooconfigure tyto typy zásad podle.

V závislosti na tom, jak nakonfigurovat zásady doručení assetu hello je bude možné toodynamically balíčku, dynamicky šifrovat a stream hello následujících protokolů datových proudů: technologie Smooth Streaming, HLS, datové proudy MPEG DASH.

Hello následující seznam ukazuje hello formáty, že používáte toostream Smooth, HLS, DASH.

Technologie Smooth Streaming:

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Pokyny, jak toopublish prostředek a sestavení adresu URL pro streamování, najdete v části [sestavit adresu URL pro streamování](media-services-deliver-streaming-content.md).

## <a name="considerations"></a>Požadavky
* Nelze odstranit AssetDeliveryPolicy přidružený prostředek při Lokátor OnDemand (streaming) existuje pro tento prostředek. Hello doporučení je zásada hello tooremove z hello asset před odstraněním hello zásad.
* Šifrované majetku úložiště nelze vytvořit lokátor streamování, nastavena žádné zásady doručení assetu.  Není-li hello Asset šifrování úložiště, hello systému vám umožní vytvoření prostředku hello Lokátor a datový proud v hello zrušte bez zásady pro doručení assetu.
* Může mít několik zásady doručení mediálního přidružené jednoho datového zdroje, ale můžete zadat jenom jeden ze způsobů toohandle daného AssetDeliveryProtocol.  Znamená, pokud se pokusíte toolink dvě zásady doručení určující hello AssetDeliveryProtocol.SmoothStreaming protokol, který bude výsledkem chyba, protože systém hello nebude vědět, které z nich chcete tooapply když klient zadává žádost technologie Smooth Streaming.
* Pokud máte prostředek s stávající Lokátor streamování, nelze propojit nový prostředek toohello zásady, odpojit existující zásady z hello asset nebo aktualizujete zásady pro doručení přidružený hello asset.  Můžete nejprve mít Lokátor streamování hello tooremove, upravte hello zásady a potom je znovu vytvořit lokátor streamování hello.  Můžete použít hello, které by měly stejné locatorId při vytvoření hello streamování Lokátor ale můžete zajistit, který nezpůsobí problémy pro klienty, protože do mezipaměti obsah hello původ nebo příjem dat CDN.

>[!NOTE]

>Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP. Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Připojení služby tooMedia

Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI.

## <a name="clear-asset-delivery-policy"></a>Zásady doručení assetu vymazat
### <a id="create_asset_delivery_policy"></a>Vytvoření zásady doručení assetu
Hello následující požadavek HTTP vytvoří zásady doručení assetu, která určuje toonot použít dynamické šifrování a protokoly datového proudu hello toodeliver v některém z následujících hello: protokoly MPEG DASH, HLS nebo technologie Smooth Streaming. 

Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.   

Žádost:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Odpověď:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <a id="link_asset_with_asset_delivery_policy"></a>Odkaz asset zásady doručení assetu
Hello následující hello odkazy požadavku HTTP zadat zásady doručení pro asset toohello assetu.

Žádost:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Odpověď:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Zásady doručení assetu DynamicEnvelopeEncryption
### <a name="create-content-key-of-hello-envelopeencryption-type-and-link-it-toohello-asset"></a>Vytvořte klíč obsahu typu EnvelopeEncryption hello a propojit jej toohello asset
Při zadávání zásad doručení DynamicEnvelopeEncryption, musíte se toolink toomake váš asset tooa obsahu klíč hello EnvelopeEncryption typu. Další informace najdete v tématu: [vytváření klíč obsahu](media-services-rest-create-contentkey.md)).

### <a id="get_delivery_url"></a>Získat adresu URL pro doručení
Adresa URL doručení hello GET pro hello zadat metodu doručení obsahu klíče hello vytvořili v předchozím kroku hello. Klient používá hello vrátila adresa URL toorequest AES klíč nebo licence PlayReady v pořadí tooplayback hello chráněného obsahu.

Zadejte typ hello tooget hello adresy URL v textu hello hello požadavku protokolu HTTP. Pokud chráníte svůj obsah pomocí technologie PlayReady, žádosti o adresu URL získání licence Media Services PlayReady pomocí 1 pro hello keyDeliveryType: {"keyDeliveryType": 1}. Pokud chráníte svůj obsah pomocí šifrování hello obálky, požadavků adresu URL pro získání klíče zadáním 2 pro keyDeliveryType: {"keyDeliveryType": 2}.

Žádost:

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

Odpověď:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a>Vytvoření zásady doručení assetu
Hello následující požadavek HTTP vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply dynamické obálky šifrování (**DynamicEnvelopeEncryption**) toohello **HLS**protokolu (v tomto příkladu jiné protokoly budou při streamování blokovány). 

Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.   

Žádost:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


Odpověď:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a>Odkaz asset zásady doručení assetu
V tématu [odkaz asset zásady doručení assetu](#link_asset_with_asset_delivery_policy)

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Zásady doručení assetu DynamicCommonEncryption
### <a name="create-content-key-of-hello-commonencryption-type-and-link-it-toohello-asset"></a>Vytvořte klíč obsahu typu CommonEncryption hello a propojit jej toohello asset
Při zadávání zásad doručení DynamicCommonEncryption, musíte se toolink toomake váš asset tooa obsahu klíč hello CommonEncryption typu. Další informace najdete v tématu: [vytváření klíč obsahu](media-services-rest-create-contentkey.md)).

### <a name="get-delivery-url"></a>Získat adresu URL pro doručení
Získáte hello doručení URL pro metodu doručení PlayReady hello hello obsahu klíče vytvořené v předchozím kroku hello. Klient používá hello vrátil toorequest adresu URL licence PlayReady v pořadí tooplayback hello chráněný obsah. Další informace najdete v tématu [získat adresu URL doručení](#get_delivery_url).

### <a name="create-asset-delivery-policy"></a>Vytvoření zásady doručení assetu
Hello následující požadavek HTTP vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply běžného dynamického šifrování (**DynamicCommonEncryption**) toohello **technologie Smooth Streaming**  protokolu (v tomto příkladu jiné protokoly budou při streamování blokovány). 

Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.   

Žádost:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Pokud chcete tooprotect svůj obsah pomocí Widevine DRM, aktualizujte hello AssetDeliveryConfiguration hodnoty toouse WidevineLicenseAcquisitionUrl (což je hodnota hello 7) a zadejte adresu URL hello služby doručování licencí. Můžete použít následující toohelp partneři AMS doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Například: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Při šifrování s technologií Widevine, by být pouze možnost toodeliver pomocí čárka. Ujistěte se, že toospecify DASH (2) v hello doručovací protokol assetu.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Odkaz asset zásady doručení assetu
V tématu [odkaz asset zásady doručení assetu](#link_asset_with_asset_delivery_policy)

## <a id="types"></a>Typy používané při definování AssetDeliveryPolicy

### <a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol

Hello následující výčtu popisuje hodnoty, které lze nastavit pro doručovací protokol assetu hello.

    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

### <a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

Hello následující výčtu popisuje hodnoty, které můžete zadat pro typ zásady doručení assetu hello.  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// hello Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption toohello asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

### <a name="contentkeydeliverytype"></a>ContentKeyDeliveryType

Hello následující výčtu popisuje hodnoty, které můžete použít metodu doručení hello tooconfigure hello obsahu toohello klíče klienta.
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


### <a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

Následující výčet Hello popisuje hodnoty můžete nastavit určité konfigurace tooget tooconfigure klíčů používaných pro zásady pro doručení assetu.

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// hello PlayReady License Acquisition Url toouse for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// hello PlayReady Custom Attributes tooadd toohello PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

