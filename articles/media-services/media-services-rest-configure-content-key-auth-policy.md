---
title: "zásady autorizace klíče obsahu aaaConfigure se zbytkem - Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure zásad autorizace pro klíč obsahu pomocí Media Services REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7af5f9e2-8ed8-43f2-843b-580ce8759fd4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: c058b7682bcbfb736faba18ec7fce33f2f2acb49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a><span data-ttu-id="fbcad-103">Dynamické šifrování: Konfigurace zásad autorizace klíče obsahu</span><span class="sxs-lookup"><span data-stu-id="fbcad-103">Dynamic encryption: Configure Content Key Authorization Policy</span></span>
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a><span data-ttu-id="fbcad-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="fbcad-104">Overview</span></span>
<span data-ttu-id="fbcad-105">Microsoft Azure Media Services umožňuje toodeliver můžete obsah (dynamicky) šifrován Standard AES (Advanced Encryption) (s použitím 128bitové šifrování klíče) a PlayReady nebo Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="fbcad-105">Microsoft Azure Media Services enables you toodeliver your content encrypted (dynamically) with Advanced Encryption Standard (AES) (using 128-bit encryption keys) and PlayReady or Widevine DRM.</span></span> <span data-ttu-id="fbcad-106">Služba Media Services také poskytuje službu k doručování klíče a licence PlayReady nebo Widevine tooauthorized klientů.</span><span class="sxs-lookup"><span data-stu-id="fbcad-106">Media Services also provides a service for delivering keys and PlayReady/Widevine licenses tooauthorized clients.</span></span>

<span data-ttu-id="fbcad-107">Pokud chcete pro Media Services tooencrypt prostředek, je nutné tooassociate šifrovací klíč (**CommonEncryption** nebo **EnvelopeEncryption**) s hello asset (jak je popsáno [sem](media-services-rest-create-contentkey.md)) a také nakonfigurovat zásady autorizace pro klíč hello (jak je popsáno v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="fbcad-107">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key (**CommonEncryption** or **EnvelopeEncryption**) with hello asset (as described [here](media-services-rest-create-contentkey.md)) and also configure authorization policies for hello key (as described in this article).</span></span>

<span data-ttu-id="fbcad-108">Když přehrávač vyžádá datový proud, služba Media Services použije hello zadané klíče toodynamically šifrování svůj obsah pomocí šifrování AES nebo PlayReady.</span><span class="sxs-lookup"><span data-stu-id="fbcad-108">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES or PlayReady encryption.</span></span> <span data-ttu-id="fbcad-109">datový proud hello toodecrypt, hello player bude požadovat hello klíč z hello doručení klíče služby.</span><span class="sxs-lookup"><span data-stu-id="fbcad-109">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="fbcad-110">toodecide, jestli je uživatel hello autorizovaný tooget hello klíč, hello služba vyhodnocuje hello zásady autorizace, které jste zadali pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="fbcad-110">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="fbcad-111">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="fbcad-111">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="fbcad-112">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.</span><span class="sxs-lookup"><span data-stu-id="fbcad-112">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span> <span data-ttu-id="fbcad-113">zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS).</span><span class="sxs-lookup"><span data-stu-id="fbcad-113">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="fbcad-114">Služba Media Services podporuje tokeny ve hello **jednoduchých webových tokenů** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátu a ** webových tokenů JSON **(JWT) formátu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-114">Media Services supports tokens in hello **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format and **JSON Web Token **(JWT) format.</span></span>

<span data-ttu-id="fbcad-115">Služba Media Services neposkytuje zabezpečení tokenu služby.</span><span class="sxs-lookup"><span data-stu-id="fbcad-115">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="fbcad-116">Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny.</span><span class="sxs-lookup"><span data-stu-id="fbcad-116">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="fbcad-117">Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a deklarace identity vystavují (jak je popsáno v tomto článku) zadaný v konfiguraci omezení s tokenem hello.</span><span class="sxs-lookup"><span data-stu-id="fbcad-117">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration (as described in this article).</span></span> <span data-ttu-id="fbcad-118">Hello doručení klíče služby Media Services vrátí hello šifrovací klíče toohello klienta Pokud hello token je platný a hello deklarace v tokenu hello odpovídají nakonfigurovaným pro klíč obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="fbcad-118">hello Media Services key delivery service will return hello encryption key toohello client if hello token is valid and hello claims in hello token match those configured for hello content key.</span></span>

<span data-ttu-id="fbcad-119">Další informace najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="fbcad-119">For more information, see</span></span>

[<span data-ttu-id="fbcad-120">Ověření pomocí tokenu JWT</span><span class="sxs-lookup"><span data-stu-id="fbcad-120">JWT token authentication</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="fbcad-121">[Integrace aplikace Azure Media Services OWIN MVC se službou Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="fbcad-121">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="fbcad-122">[Použití Azure ACS tooissue tokeny](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="fbcad-122">[Use Azure ACS tooissue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

### <a name="some-considerations-apply"></a><span data-ttu-id="fbcad-123">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="fbcad-123">Some considerations apply:</span></span>
* <span data-ttu-id="fbcad-124">toobe možné toouse dynamické balení a dynamické šifrování, zkontrolujte, zda je hello koncový bod, ze kterého mají být toostream streamování vašeho obsahu v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-124">toobe able toouse dynamic packaging and dynamic encryption, make sure hello streaming endpoint from which you want toostream  your content is in hello **Running** state.</span></span>
* <span data-ttu-id="fbcad-125">Váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="fbcad-125">Your asset must contain a set of adaptive bitrate MP4s or  adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="fbcad-126">Další informace najdete v tématu [kódovat asset](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="fbcad-126">For more information, see [Encode an asset](media-services-encode-asset.md).</span></span>
* <span data-ttu-id="fbcad-127">Nahrání a kódování vaše prostředky pomocí **AssetCreationOptions.StorageEncrypted** možnost.</span><span class="sxs-lookup"><span data-stu-id="fbcad-127">Upload and encode your assets using **AssetCreationOptions.StorageEncrypted** option.</span></span>
* <span data-ttu-id="fbcad-128">Pokud máte v plánu toohave více obsahu klíčů, které vyžadují hello stejné konfigurace zásad, se důrazně doporučuje toocreate zásady jeden autorizace a opakovaně ji používat s více klíčů k obsahu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-128">If you plan toohave multiple content keys that require hello same policy configuration, it is strongly recommended toocreate a single authorization policy and reuse it with multiple content keys.</span></span>
* <span data-ttu-id="fbcad-129">Hello služba pro přenos klíče ukládá do mezipaměti ContentKeyAuthorizationPolicy a související objekty (Možnosti zásad a omezení) pro 15 minut.</span><span class="sxs-lookup"><span data-stu-id="fbcad-129">hello Key Delivery service caches ContentKeyAuthorizationPolicy and its related objects (policy options and restrictions) for 15 minutes.</span></span>  <span data-ttu-id="fbcad-130">Pokud vytvoříte ContentKeyAuthorizationPolicy a zadejte toouse "Token" omezení, pak otestovat ji a potom aktualizovat zásady hello příliš "Otevřít" omezení, bude trvat přibližně 15 minut před hello zásad přepínače toohello "Otevřené" verze hello zásad.</span><span class="sxs-lookup"><span data-stu-id="fbcad-130">If you create a ContentKeyAuthorizationPolicy and specify toouse a “Token” restriction, then test it, and then update hello policy too“Open” restriction, it will take roughly 15 minutes before hello policy switches toohello “Open” version of hello policy.</span></span>
* <span data-ttu-id="fbcad-131">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="fbcad-131">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
* <span data-ttu-id="fbcad-132">V současné době nelze zašifrovat progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="fbcad-132">Currently, you cannot encrypt progressive downloads.</span></span>

## <a name="aes-128-dynamic-encryption"></a><span data-ttu-id="fbcad-133">Dynamické šifrování AES-128</span><span class="sxs-lookup"><span data-stu-id="fbcad-133">AES-128 Dynamic Encryption</span></span>
> [!NOTE]
> <span data-ttu-id="fbcad-134">Při práci s hello Media Services REST API hello platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="fbcad-134">When working with hello Media Services REST API, hello following considerations apply:</span></span>
> 
> <span data-ttu-id="fbcad-135">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbcad-135">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="fbcad-136">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="fbcad-136">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 
> <span data-ttu-id="fbcad-137">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="fbcad-137">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="fbcad-138">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="fbcad-138">You must make subsequent calls toohello new URI.</span></span> <span data-ttu-id="fbcad-139">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="fbcad-139">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
> 
> 

### <a name="open-restriction"></a><span data-ttu-id="fbcad-140">Otevřete omezení</span><span class="sxs-lookup"><span data-stu-id="fbcad-140">Open Restriction</span></span>
<span data-ttu-id="fbcad-141">Otevřete omezení znamená, že systém hello dodá hello tooanyone klíče, který vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="fbcad-141">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="fbcad-142">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="fbcad-142">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="fbcad-143">Hello následující ukázka vytvoří zásadu otevřete autorizace a přidá ho toohello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-143">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

#### <span data-ttu-id="fbcad-144"><a id="ContentKeyAuthorizationPolicies"></a>Vytvoření ContentKeyAuthorizationPolicies</span><span class="sxs-lookup"><span data-stu-id="fbcad-144"><a id="ContentKeyAuthorizationPolicies"></a>Create ContentKeyAuthorizationPolicies</span></span>
<span data-ttu-id="fbcad-145">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-145">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbbef702-e769-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423578086&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lZlyQ2%2bvH73qtJsb42%2fH3xF7r7EvQFR3UXyezuDENFU%3d
    x-ms-version: 2.11
    x-ms-client-request-id: d732dbfa-54fc-474c-99d6-9b46a006f389
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 36

    {"Name":"Open Authorization Policy"}

<span data-ttu-id="fbcad-146">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-146">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 211
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3Adb4593da-f4d1-4cc5-a92a-d20eacbabee4')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d732dbfa-54fc-474c-99d6-9b46a006f389
    request-id: aabfa731-e884-4bf3-8314-492b04747ac4
    x-ms-request-id: aabfa731-e884-4bf3-8314-492b04747ac4
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 08:25:56 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicies/@Element","Id":"nb:ckpid:UUID:db4593da-f4d1-4cc5-a92a-d20eacbabee4","Name":"Open Authorization Policy"}

#### <span data-ttu-id="fbcad-147"><a id="ContentKeyAuthorizationPolicyOptions"></a>Vytvoření ContentKeyAuthorizationPolicyOptions</span><span class="sxs-lookup"><span data-stu-id="fbcad-147"><a id="ContentKeyAuthorizationPolicyOptions"></a>Create ContentKeyAuthorizationPolicyOptions</span></span>
<span data-ttu-id="fbcad-148">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-148">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbbef702-e769-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423580006&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Ref3EsonGF7fUKCwGwGgiMnZitzIzsDOvvMTeVrVVPg%3d
    x-ms-version: 2.11
    x-ms-client-request-id: d225e357-e60e-4f42-add8-9d93aba1409a
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 168

    {"Name":"policy","KeyDeliveryType":2,"KeyDeliveryConfiguration":"","Restrictions":[{"Name":"HLS Open Authorization Policy","KeyRestrictionType":0,"Requirements":null}]}

<span data-ttu-id="fbcad-149">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-149">Response:</span></span>    

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 349
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A57829b17-1101-4797-919b-f816f4a007b7')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d225e357-e60e-4f42-add8-9d93aba1409a
    request-id: 81bcad37-295b-431f-972f-b23f2e4172c9
    x-ms-request-id: 81bcad37-295b-431f-972f-b23f2e4172c9
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 08:56:40 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:57829b17-1101-4797-919b-f816f4a007b7","Name":"policy","KeyDeliveryType":2,"KeyDeliveryConfiguration":"","Restrictions":[{"Name":"HLS Open Authorization Policy","KeyRestrictionType":0,"Requirements":null}]}

#### <span data-ttu-id="fbcad-150"><a id="LinkContentKeyAuthorizationPoliciesWithOptions"></a>ContentKeyAuthorizationPolicies odkaz s možností</span><span class="sxs-lookup"><span data-stu-id="fbcad-150"><a id="LinkContentKeyAuthorizationPoliciesWithOptions"></a>Link ContentKeyAuthorizationPolicies with Options</span></span>
<span data-ttu-id="fbcad-151">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-151">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3A0baa438b-8ac2-4c40-a53c-4d4722b78715')/$links/Options HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423580006&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Ref3EsonGF7fUKCwGwGgiMnZitzIzsDOvvMTeVrVVPg%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 9847f705-f2ca-4e95-a478-8f823dbbaa29
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 154

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A57829b17-1101-4797-919b-f816f4a007b7')"}

<span data-ttu-id="fbcad-152">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-152">Response:</span></span>

    HTTP/1.1 204 No Content

#### <span data-ttu-id="fbcad-153"><a id="AddAuthorizationPolicyToKey"></a>Přidejte klíč obsahu toohello zásad autorizace</span><span class="sxs-lookup"><span data-stu-id="fbcad-153"><a id="AddAuthorizationPolicyToKey"></a>Add authorization policy toohello content key</span></span>
<span data-ttu-id="fbcad-154">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-154">Request:</span></span>

    PUT https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A2e6d36a7-a17c-4e9a-830d-eca23ad1a6f9') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423581565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=JiNSG3w6r2C0nIyfKvTZj1uPJGjuitD%2b0sbfZ%2b2JDZI%3d
    x-ms-version: 2.11
    x-ms-client-request-id: e613efff-cb6a-41b4-984a-f4f8fb6e76a4
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 78

    {"AuthorizationPolicyId":"nb:ckpid:UUID:c06cebb8-c4f0-4d1a-ba00-3273fb2bc3ad"}

<span data-ttu-id="fbcad-155">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-155">Response:</span></span>

    HTTP/1.1 204 No Content

### <a name="token-restriction"></a><span data-ttu-id="fbcad-156">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="fbcad-156">Token Restriction</span></span>
<span data-ttu-id="fbcad-157">Tato část popisuje, jak toocreate obsahu klíče zásad autorizace a přidružte ji k hello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-157">This section describes how toocreate a content key authorization policy and associate it with hello content key.</span></span> <span data-ttu-id="fbcad-158">zásady autorizace Hello popisuje, jaké požadavky na ověřování musí být nesplnění toodetermine, pokud je uživatel hello autorizovaný tooreceive hello klíč (například seznamu "ověřovací klíč" hello obsahovat hello klíč byl podepsaný token tuto hello).</span><span class="sxs-lookup"><span data-stu-id="fbcad-158">hello authorization policy describes what authorization requirements must be met toodetermine if hello user is authorized tooreceive hello key (for example, does hello “verification key” list contain hello key that hello token was signed with).</span></span>

<span data-ttu-id="fbcad-159">možnost omezení s tokenem hello tooconfigure, je nutné toouse XML tokenu hello toodescribe autorizace požadavky.</span><span class="sxs-lookup"><span data-stu-id="fbcad-159">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="fbcad-160">Konfigurace omezení s tokenem Hello XML musí odpovídat toohello následující schématu XML.</span><span class="sxs-lookup"><span data-stu-id="fbcad-160">hello token restriction configuration XML must conform toohello following XML schema.</span></span>

#### <span data-ttu-id="fbcad-161"><a id="schema"></a>Omezení s tokenem schématu</span><span class="sxs-lookup"><span data-stu-id="fbcad-161"><a id="schema"></a>Token restriction schema</span></span>
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

<span data-ttu-id="fbcad-162">Při konfiguraci hello **tokenu** omezený zásady, je nutné zadat hello primární ** ověřovací klíč **, **vystavitele** a **cílovou skupinu** parametry.</span><span class="sxs-lookup"><span data-stu-id="fbcad-162">When configuring hello **token** restricted policy, you must specify hello primary** verification key**, **issuer** and **audience** parameters.</span></span> <span data-ttu-id="fbcad-163">Hello ** primární ověření klíč ** obsahuje hello klíč, který hello token byl podepsán, **vystavitele** služby tokenů zabezpečení hello je tento token hello problémy.</span><span class="sxs-lookup"><span data-stu-id="fbcad-163">hello **primary verification key **contains hello key that hello token was signed with, **issuer** is hello secure token service that issues hello token.</span></span> <span data-ttu-id="fbcad-164">Hello **cílovou skupinu** (někdy nazývané **oboru**) popisuje hello záměr hello tokenu nebo hello prostředků hello token povolí přístup k.</span><span class="sxs-lookup"><span data-stu-id="fbcad-164">hello **audience** (sometimes called **scope**) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="fbcad-165">Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="fbcad-165">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span> 

<span data-ttu-id="fbcad-166">Hello následující ukázka vytvoří token omezení zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="fbcad-166">hello following example creates an authorization policy with a token restriction.</span></span> <span data-ttu-id="fbcad-167">V tomto příkladu hello by klient musel předem toopresent token, který obsahuje: podpisový klíč (VerificationKey), vydavatel tokenu a požadované deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="fbcad-167">In this example, hello client would have toopresent a token that contains: signing key (VerificationKey), a token issuer, and required claims.</span></span>

### <a name="create-contentkeyauthorizationpolicies"></a><span data-ttu-id="fbcad-168">Vytvoření ContentKeyAuthorizationPolicies</span><span class="sxs-lookup"><span data-stu-id="fbcad-168">Create ContentKeyAuthorizationPolicies</span></span>
<span data-ttu-id="fbcad-169">Vytvořte hello "Token omezení zásady", jak je znázorněno [zde](#ContentKeyAuthorizationPolicies).</span><span class="sxs-lookup"><span data-stu-id="fbcad-169">Create hello "Token Restriction Policy" as shown [here](#ContentKeyAuthorizationPolicies).</span></span>

### <a name="create-contentkeyauthorizationpolicyoptions"></a><span data-ttu-id="fbcad-170">Vytvoření ContentKeyAuthorizationPolicyOptions</span><span class="sxs-lookup"><span data-stu-id="fbcad-170">Create ContentKeyAuthorizationPolicyOptions</span></span>
<span data-ttu-id="fbcad-171">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-171">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbbef702-e769-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423580720&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=5LsNu%2b0D4eD3UOP3BviTLDkUjaErdUx0ekJ8402xidQ%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 2643d836-bfe7-438e-9ba2-bc6ff28e4a53
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 1079

    {"Name":"Token option for HLS","KeyDeliveryType":2,"KeyDeliveryConfiguration":null,"Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>BklyAFiPTQsuJNKriQJBZHYaKM2CkCTDQX2bw9sMYuvEC9sjW0W7GUIBygQL/+POEeUqCYPnmEU2g0o1GW2Oqg==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>E5BUHiN4vBdzUzdP0IWaHFMMU3D1uRZgF16TOhSfwwHGSw+Kbf0XqsHzEIYk11M372viB9vbiacsdcQksA0ftw==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

<span data-ttu-id="fbcad-172">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-172">Response:</span></span>    

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1260
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3Ae1ef6145-46e8-4ee6-9756-b1cf96328c23')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 2643d836-bfe7-438e-9ba2-bc6ff28e4a53
    request-id: 2310b716-aeaa-421e-913e-3ce2f6f685ca
    x-ms-request-id: 2310b716-aeaa-421e-913e-3ce2f6f685ca
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:10:37 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:e1ef6145-46e8-4ee6-9756-b1cf96328c23","Name":"Token option for HLS","KeyDeliveryType":2,"KeyDeliveryConfiguration":null,"Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>BklyAFiPTQsuJNKriQJBZHYaKM2CkCTDQX2bw9sMYuvEC9sjW0W7GUIBygQL/+POEeUqCYPnmEU2g0o1GW2Oqg==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>E5BUHiN4vBdzUzdP0IWaHFMMU3D1uRZgF16TOhSfwwHGSw+Kbf0XqsHzEIYk11M372viB9vbiacsdcQksA0ftw==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a><span data-ttu-id="fbcad-173">ContentKeyAuthorizationPolicies odkaz s možností</span><span class="sxs-lookup"><span data-stu-id="fbcad-173">Link ContentKeyAuthorizationPolicies with Options</span></span>
<span data-ttu-id="fbcad-174">Odkaz ContentKeyAuthorizationPolicies s možností, jak je znázorněno [zde](#ContentKeyAuthorizationPolicies).</span><span class="sxs-lookup"><span data-stu-id="fbcad-174">Link ContentKeyAuthorizationPolicies with Options as shown [here](#ContentKeyAuthorizationPolicies).</span></span>

#### <a name="add-authorization-policy-toohello-content-key"></a><span data-ttu-id="fbcad-175">Přidejte klíč obsahu toohello zásad autorizace</span><span class="sxs-lookup"><span data-stu-id="fbcad-175">Add authorization policy toohello content key</span></span>
<span data-ttu-id="fbcad-176">Přidejte AuthorizationPolicy toohello ContentKey, jak je znázorněno [zde](#AddAuthorizationPolicyToKey).</span><span class="sxs-lookup"><span data-stu-id="fbcad-176">Add AuthorizationPolicy toohello ContentKey as shown [here](#AddAuthorizationPolicyToKey).</span></span>

## <a name="playready-dynamic-encryption"></a><span data-ttu-id="fbcad-177">Šifrování PlayReady dynamický</span><span class="sxs-lookup"><span data-stu-id="fbcad-177">PlayReady Dynamic Encryption</span></span>
<span data-ttu-id="fbcad-178">Služba Media Services umožňuje tooconfigure hello práva a omezení, že chcete pro hello tooenforce runtime PlayReady DRM při uživatele, že tooplay zpět chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-178">Media Services enables you tooconfigure hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplay back protected content.</span></span> 

<span data-ttu-id="fbcad-179">Při ochraně obsahu pomocí technologie PlayReady, jednou z věcí hello potřebujete toospecify ve vaší zásady autorizace je řetězec XML, který definuje hello [šablona licence PlayReady](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fbcad-179">When protecting your content with PlayReady, one of hello things you need toospecify in your authorization policy is an XML string that defines hello [PlayReady license template](media-services-playready-license-template-overview.md).</span></span> 

### <a name="open-restriction"></a><span data-ttu-id="fbcad-180">Otevřete omezení</span><span class="sxs-lookup"><span data-stu-id="fbcad-180">Open Restriction</span></span>
<span data-ttu-id="fbcad-181">Otevřete omezení znamená, že systém hello dodá hello tooanyone klíče, který vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="fbcad-181">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="fbcad-182">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="fbcad-182">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="fbcad-183">Hello následující ukázka vytvoří zásadu otevřete autorizace a přidá ho toohello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-183">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

#### <span data-ttu-id="fbcad-184"><a id="ContentKeyAuthorizationPolicies2"></a>Vytvoření ContentKeyAuthorizationPolicies</span><span class="sxs-lookup"><span data-stu-id="fbcad-184"><a id="ContentKeyAuthorizationPolicies2"></a>Create ContentKeyAuthorizationPolicies</span></span>
<span data-ttu-id="fbcad-185">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-185">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423581565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=JiNSG3w6r2C0nIyfKvTZj1uPJGjuitD%2b0sbfZ%2b2JDZI%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 9e7fa407-f84e-43aa-8f05-9790b46e279b
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 58

    {"Name":"Deliver Common Content Key"}

<span data-ttu-id="fbcad-186">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-186">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 233
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3Acc3c64a8-e2fc-4e09-bf60-ac954251a387')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 9e7fa407-f84e-43aa-8f05-9790b46e279b
    request-id: b3d33c1b-a9cb-4120-ac0c-18f64846c147
    x-ms-request-id: b3d33c1b-a9cb-4120-ac0c-18f64846c147
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:26:00 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicies/@Element","Id":"nb:ckpid:UUID:cc3c64a8-e2fc-4e09-bf60-ac954251a387","Name":"Deliver Common Content Key"}


#### <a name="create-contentkeyauthorizationpolicyoptions"></a><span data-ttu-id="fbcad-187">Vytvoření ContentKeyAuthorizationPolicyOptions</span><span class="sxs-lookup"><span data-stu-id="fbcad-187">Create ContentKeyAuthorizationPolicyOptions</span></span>
<span data-ttu-id="fbcad-188">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-188">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423581565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=JiNSG3w6r2C0nIyfKvTZj1uPJGjuitD%2b0sbfZ%2b2JDZI%3d
    x-ms-version: 2.11
    x-ms-client-request-id: f160ad25-b457-4bc6-8197-315604c5e585
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 593

    {"Name":"","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Open","KeyRestrictionType":0,"Requirements":null}]}

<span data-ttu-id="fbcad-189">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-189">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 774
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A1052308c-4df7-4fdb-8d21-4d2141fc2be0')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: f160ad25-b457-4bc6-8197-315604c5e585
    request-id: 563f5a42-50a4-4c4a-add8-a833f8364231
    x-ms-request-id: 563f5a42-50a4-4c4a-add8-a833f8364231
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:23:24 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:1052308c-4df7-4fdb-8d21-4d2141fc2be0","Name":"","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Open","KeyRestrictionType":0,"Requirements":null}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a><span data-ttu-id="fbcad-190">ContentKeyAuthorizationPolicies odkaz s možností</span><span class="sxs-lookup"><span data-stu-id="fbcad-190">Link ContentKeyAuthorizationPolicies with Options</span></span>
<span data-ttu-id="fbcad-191">Odkaz ContentKeyAuthorizationPolicies s možností, jak je znázorněno [zde](#ContentKeyAuthorizationPolicies).</span><span class="sxs-lookup"><span data-stu-id="fbcad-191">Link ContentKeyAuthorizationPolicies with Options as shown [here](#ContentKeyAuthorizationPolicies).</span></span>

#### <a name="add-authorization-policy-toohello-content-key"></a><span data-ttu-id="fbcad-192">Přidejte klíč obsahu toohello zásad autorizace</span><span class="sxs-lookup"><span data-stu-id="fbcad-192">Add authorization policy toohello content key</span></span>
<span data-ttu-id="fbcad-193">Přidejte AuthorizationPolicy toohello ContentKey, jak je znázorněno [zde](#AddAuthorizationPolicyToKey).</span><span class="sxs-lookup"><span data-stu-id="fbcad-193">Add AuthorizationPolicy toohello ContentKey as shown [here](#AddAuthorizationPolicyToKey).</span></span>

### <a name="token-restriction"></a><span data-ttu-id="fbcad-194">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="fbcad-194">Token Restriction</span></span>
<span data-ttu-id="fbcad-195">možnost omezení s tokenem hello tooconfigure, je nutné toouse XML tokenu hello toodescribe autorizace požadavky.</span><span class="sxs-lookup"><span data-stu-id="fbcad-195">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="fbcad-196">Konfigurace omezení s tokenem Hello XML musí odpovídat schématu XML toohello ukazuje [to](#schema) části.</span><span class="sxs-lookup"><span data-stu-id="fbcad-196">hello token restriction configuration XML must conform toohello XML schema shown in [this](#schema) section.</span></span>

#### <a name="create-contentkeyauthorizationpolicies"></a><span data-ttu-id="fbcad-197">Vytvoření ContentKeyAuthorizationPolicies</span><span class="sxs-lookup"><span data-stu-id="fbcad-197">Create ContentKeyAuthorizationPolicies</span></span>
<span data-ttu-id="fbcad-198">Vytvořte ContentKeyAuthorizationPolicies, jak je znázorněno [zde](#ContentKeyAuthorizationPolicies2).</span><span class="sxs-lookup"><span data-stu-id="fbcad-198">Create ContentKeyAuthorizationPolicies as shown [here](#ContentKeyAuthorizationPolicies2).</span></span>

#### <a name="create-contentkeyauthorizationpolicyoptions"></a><span data-ttu-id="fbcad-199">Vytvoření ContentKeyAuthorizationPolicyOptions</span><span class="sxs-lookup"><span data-stu-id="fbcad-199">Create ContentKeyAuthorizationPolicyOptions</span></span>
<span data-ttu-id="fbcad-200">Žádost:</span><span class="sxs-lookup"><span data-stu-id="fbcad-200">Request:</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423583561&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=5eZnkOsSv%2fLLEKmS%2bWObBlsNYyee8BQlp%2bUYbjugcJg%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ab079b0e-2ba9-4cf1-b549-a97bfa6cd2d3
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 1525

    {"Name":"Token option","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>w52OyHVqXT8aaupGxuJ3NGt8M6opHDOtx132p4r6q4hLI6ffnLusgEGie1kedUewVoIe1tqDkVE6xsIV7O91KA==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>dYwLKIEMBljLeY9VM7vWdlhps31Fbt0XXhqP5VyjQa33bJXleBtkzQ6dF5AtwI9gDcdM2dV2TvYNhCilBKjMCg==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

<span data-ttu-id="fbcad-201">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="fbcad-201">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1706
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3Ae42bbeae-de42-4077-90e9-a844f297ef70')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: ab079b0e-2ba9-4cf1-b549-a97bfa6cd2d3
    request-id: ccf8a4ba-731e-4124-8192-079592c251cc
    x-ms-request-id: ccf8a4ba-731e-4124-8192-079592c251cc
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:58:47 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:e42bbeae-de42-4077-90e9-a844f297ef70","Name":"Token option","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>w52OyHVqXT8aaupGxuJ3NGt8M6opHDOtx132p4r6q4hLI6ffnLusgEGie1kedUewVoIe1tqDkVE6xsIV7O91KA==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>dYwLKIEMBljLeY9VM7vWdlhps31Fbt0XXhqP5VyjQa33bJXleBtkzQ6dF5AtwI9gDcdM2dV2TvYNhCilBKjMCg==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

#### <a name="link-contentkeyauthorizationpolicies-with-options"></a><span data-ttu-id="fbcad-202">ContentKeyAuthorizationPolicies odkaz s možností</span><span class="sxs-lookup"><span data-stu-id="fbcad-202">Link ContentKeyAuthorizationPolicies with Options</span></span>
<span data-ttu-id="fbcad-203">Odkaz ContentKeyAuthorizationPolicies s možností, jak je znázorněno [zde](#ContentKeyAuthorizationPolicies).</span><span class="sxs-lookup"><span data-stu-id="fbcad-203">Link ContentKeyAuthorizationPolicies with Options as shown [here](#ContentKeyAuthorizationPolicies).</span></span>

#### <a name="add-authorization-policy-toohello-content-key"></a><span data-ttu-id="fbcad-204">Přidejte klíč obsahu toohello zásad autorizace</span><span class="sxs-lookup"><span data-stu-id="fbcad-204">Add authorization policy toohello content key</span></span>
<span data-ttu-id="fbcad-205">Přidejte AuthorizationPolicy toohello ContentKey, jak je znázorněno [zde](#AddAuthorizationPolicyToKey).</span><span class="sxs-lookup"><span data-stu-id="fbcad-205">Add AuthorizationPolicy toohello ContentKey as shown [here](#AddAuthorizationPolicyToKey).</span></span>

## <span data-ttu-id="fbcad-206"><a id="types"></a>Typy používané při definování ContentKeyAuthorizationPolicy</span><span class="sxs-lookup"><span data-stu-id="fbcad-206"><a id="types"></a>Types used when defining ContentKeyAuthorizationPolicy</span></span>
### <span data-ttu-id="fbcad-207"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span><span class="sxs-lookup"><span data-stu-id="fbcad-207"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span></span>
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <span data-ttu-id="fbcad-208"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="fbcad-208"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>
    public enum ContentKeyDeliveryType
    {
        None = 0,
        PlayReadyLicense = 1,
        BaselineHttp = 2,
        Widevine = 3
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="fbcad-209">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="fbcad-209">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="fbcad-210">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="fbcad-210">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="fbcad-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbcad-211">Next Steps</span></span>
<span data-ttu-id="fbcad-212">Teď, když jste nakonfigurovali zásady autorizace klíče obsahu, přejděte toohello [jak zásady doručení assetu tooconfigure](media-services-rest-configure-asset-delivery-policy.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="fbcad-212">Now that you have configured content key's authorization policy, go toohello [How tooconfigure asset delivery policy](media-services-rest-configure-asset-delivery-policy.md) topic.</span></span>

