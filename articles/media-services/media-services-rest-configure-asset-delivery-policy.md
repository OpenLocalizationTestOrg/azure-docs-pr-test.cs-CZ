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
# <a name="configuring-asset-delivery-policies"></a><span data-ttu-id="430da-103">Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-103">Configuring asset delivery policies</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

<span data-ttu-id="430da-104">Pokud máte v plánu prostředky toodeliver dynamicky šifrovat, jeden z hello kroků v hello Media Services obsahu doručení pracovního postupu je konfigurace zásad doručení pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="430da-104">If you plan toodeliver dynamically encrypted assets, one of hello steps in hello Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="430da-105">zásady doručení assetu Hello informuje Media Services, jak chcete použít pro váš asset toobe doručit: do streamování protokol, který by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete toodynamically váš asset šifrovat a jak (obálky nebo common encryption).</span><span class="sxs-lookup"><span data-stu-id="430da-105">hello asset delivery policy tells Media Services how you want for your asset toobe delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want toodynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="430da-106">Toto téma popisuje, proč a jak toocreate a nakonfigurujte zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-106">This topic discusses why and how toocreate and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="430da-107">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="430da-107">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="430da-108">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="430da-108">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="430da-109">Navíc toobe možné toouse dynamické balením a dynamickým šifrováním váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="430da-109">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="430da-110">Je možné aplikovat různé zásady toohello stejné asset.</span><span class="sxs-lookup"><span data-stu-id="430da-110">You could apply different policies toohello same asset.</span></span> <span data-ttu-id="430da-111">Například může použít PlayReady šifrování tooSmooth Streaming a pomocí standardu AES Envelope šifrování tooMPEG DASH a HLS.</span><span class="sxs-lookup"><span data-stu-id="430da-111">For example, you could apply PlayReady encryption tooSmooth Streaming and AES Envelope encryption tooMPEG DASH and HLS.</span></span> <span data-ttu-id="430da-112">Veškeré protokoly, které nejsou v zásadách doručení definovány (například přidáte jedinou zásadu, která jako hello protokol určuje pouze HLS) budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="430da-112">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="430da-113">Výjimka toothis Hello je, pokud máte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-113">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="430da-114">Potom bude možné v hello zrušte všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="430da-114">Then, all protocols will be allowed in hello clear.</span></span>

<span data-ttu-id="430da-115">Pokud chcete toodeliver asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello.</span><span class="sxs-lookup"><span data-stu-id="430da-115">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="430da-116">Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení.</span><span class="sxs-lookup"><span data-stu-id="430da-116">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="430da-117">Například toodeliver asset šifrován Advanced Encryption (Standard AES) obálky šifrovací klíč, nastavte typ zásad hello příliš**DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="430da-117">For example, toodeliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set hello policy type too**DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="430da-118">šifrování úložiště tooremove a asset hello datový proud v hello jasné, nastavte typ zásad hello příliš**NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="430da-118">tooremove storage encryption and stream hello asset in hello clear, set hello policy type too**NoDynamicEncryption**.</span></span> <span data-ttu-id="430da-119">Příklady, které ukazují, jak tooconfigure tyto typy zásad podle.</span><span class="sxs-lookup"><span data-stu-id="430da-119">Examples that show how tooconfigure these policy types follow.</span></span>

<span data-ttu-id="430da-120">V závislosti na tom, jak nakonfigurovat zásady doručení assetu hello je bude možné toodynamically balíčku, dynamicky šifrovat a stream hello následujících protokolů datových proudů: technologie Smooth Streaming, HLS, datové proudy MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="430da-120">Depending on how you configure hello asset delivery policy you would be able toodynamically package, dynamically encrypt, and stream hello following streaming protocols: Smooth Streaming, HLS, MPEG DASH streams.</span></span>

<span data-ttu-id="430da-121">Hello následující seznam ukazuje hello formáty, že používáte toostream Smooth, HLS, DASH.</span><span class="sxs-lookup"><span data-stu-id="430da-121">hello following list shows hello formats that you use toostream Smooth, HLS, DASH.</span></span>

<span data-ttu-id="430da-122">Technologie Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="430da-122">Smooth Streaming:</span></span>

<span data-ttu-id="430da-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="430da-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="430da-124">HLS:</span><span class="sxs-lookup"><span data-stu-id="430da-124">HLS:</span></span>

<span data-ttu-id="430da-125">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="430da-125">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="430da-126">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="430da-126">MPEG DASH</span></span>

<span data-ttu-id="430da-127">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="430da-127">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


<span data-ttu-id="430da-128">Pokyny, jak toopublish prostředek a sestavení adresu URL pro streamování, najdete v části [sestavit adresu URL pro streamování](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="430da-128">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="430da-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="430da-129">Considerations</span></span>
* <span data-ttu-id="430da-130">Nelze odstranit AssetDeliveryPolicy přidružený prostředek při Lokátor OnDemand (streaming) existuje pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="430da-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="430da-131">Hello doporučení je zásada hello tooremove z hello asset před odstraněním hello zásad.</span><span class="sxs-lookup"><span data-stu-id="430da-131">hello recommendation is tooremove hello policy from hello asset before deleting hello policy.</span></span>
* <span data-ttu-id="430da-132">Šifrované majetku úložiště nelze vytvořit lokátor streamování, nastavena žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="430da-133">Není-li hello Asset šifrování úložiště, hello systému vám umožní vytvoření prostředku hello Lokátor a datový proud v hello zrušte bez zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-133">If hello Asset isn’t storage encrypted, hello system will let you create a locator and stream hello asset in hello clear without an asset delivery policy.</span></span>
* <span data-ttu-id="430da-134">Může mít několik zásady doručení mediálního přidružené jednoho datového zdroje, ale můžete zadat jenom jeden ze způsobů toohandle daného AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="430da-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way toohandle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="430da-135">Znamená, pokud se pokusíte toolink dvě zásady doručení určující hello AssetDeliveryProtocol.SmoothStreaming protokol, který bude výsledkem chyba, protože systém hello nebude vědět, které z nich chcete tooapply když klient zadává žádost technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="430da-135">Meaning if you try toolink two delivery policies that specify hello AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because hello system does not know which one you want it tooapply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="430da-136">Pokud máte prostředek s stávající Lokátor streamování, nelze propojit nový prostředek toohello zásady, odpojit existující zásady z hello asset nebo aktualizujete zásady pro doručení přidružený hello asset.</span><span class="sxs-lookup"><span data-stu-id="430da-136">If you have an asset with an existing streaming locator, you cannot link a new policy toohello asset, unlink an existing policy from hello asset, or update a delivery policy associated with hello asset.</span></span>  <span data-ttu-id="430da-137">Můžete nejprve mít Lokátor streamování hello tooremove, upravte hello zásady a potom je znovu vytvořit lokátor streamování hello.</span><span class="sxs-lookup"><span data-stu-id="430da-137">You first have tooremove hello streaming locator, adjust hello policies, and then re-create hello streaming locator.</span></span>  <span data-ttu-id="430da-138">Můžete použít hello, které by měly stejné locatorId při vytvoření hello streamování Lokátor ale můžete zajistit, který nezpůsobí problémy pro klienty, protože do mezipaměti obsah hello původ nebo příjem dat CDN.</span><span class="sxs-lookup"><span data-stu-id="430da-138">You can use hello same locatorId when you recreate hello streaming locator but you should ensure that won’t cause issues for clients since content can be cached by hello origin or a downstream CDN.</span></span>

>[!NOTE]

><span data-ttu-id="430da-139">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="430da-139">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="430da-140">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="430da-140">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="430da-141">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="430da-141">Connect tooMedia Services</span></span>

<span data-ttu-id="430da-142">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="430da-142">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="430da-143">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="430da-143">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="430da-144">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="430da-144">You must make subsequent calls toohello new URI.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="430da-145">Zásady doručení assetu vymazat</span><span class="sxs-lookup"><span data-stu-id="430da-145">Clear asset delivery policy</span></span>
### <span data-ttu-id="430da-146"><a id="create_asset_delivery_policy"></a>Vytvoření zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-146"><a id="create_asset_delivery_policy"></a>Create asset delivery policy</span></span>
<span data-ttu-id="430da-147">Hello následující požadavek HTTP vytvoří zásady doručení assetu, která určuje toonot použít dynamické šifrování a protokoly datového proudu hello toodeliver v některém z následujících hello: protokoly MPEG DASH, HLS nebo technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="430da-147">hello following HTTP request creates an asset delivery policy that specifies toonot apply dynamic encryption and toodeliver hello stream in any of hello following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> 

<span data-ttu-id="430da-148">Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="430da-148">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="430da-149">Žádost:</span><span class="sxs-lookup"><span data-stu-id="430da-149">Request:</span></span>

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

<span data-ttu-id="430da-150">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="430da-150">Response:</span></span>

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

### <span data-ttu-id="430da-151"><a id="link_asset_with_asset_delivery_policy"></a>Odkaz asset zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-151"><a id="link_asset_with_asset_delivery_policy"></a>Link asset with asset delivery policy</span></span>
<span data-ttu-id="430da-152">Hello následující hello odkazy požadavku HTTP zadat zásady doručení pro asset toohello assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-152">hello following HTTP request links hello specified asset toohello asset delivery policy to.</span></span>

<span data-ttu-id="430da-153">Žádost:</span><span class="sxs-lookup"><span data-stu-id="430da-153">Request:</span></span>

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

<span data-ttu-id="430da-154">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="430da-154">Response:</span></span>

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="430da-155">Zásady doručení assetu DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="430da-155">DynamicEnvelopeEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-hello-envelopeencryption-type-and-link-it-toohello-asset"></a><span data-ttu-id="430da-156">Vytvořte klíč obsahu typu EnvelopeEncryption hello a propojit jej toohello asset</span><span class="sxs-lookup"><span data-stu-id="430da-156">Create content key of hello EnvelopeEncryption type and link it toohello asset</span></span>
<span data-ttu-id="430da-157">Při zadávání zásad doručení DynamicEnvelopeEncryption, musíte se toolink toomake váš asset tooa obsahu klíč hello EnvelopeEncryption typu.</span><span class="sxs-lookup"><span data-stu-id="430da-157">When specifying DynamicEnvelopeEncryption delivery policy, you need toomake sure toolink your asset tooa content key of hello EnvelopeEncryption type.</span></span> <span data-ttu-id="430da-158">Další informace najdete v tématu: [vytváření klíč obsahu](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="430da-158">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <span data-ttu-id="430da-159"><a id="get_delivery_url"></a>Získat adresu URL pro doručení</span><span class="sxs-lookup"><span data-stu-id="430da-159"><a id="get_delivery_url"></a>Get delivery URL</span></span>
<span data-ttu-id="430da-160">Adresa URL doručení hello GET pro hello zadat metodu doručení obsahu klíče hello vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="430da-160">Get hello delivery URL for hello specified delivery method of hello content key created in hello previous step.</span></span> <span data-ttu-id="430da-161">Klient používá hello vrátila adresa URL toorequest AES klíč nebo licence PlayReady v pořadí tooplayback hello chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="430da-161">A client uses hello returned URL toorequest an AES key or a PlayReady license in order tooplayback hello protected content.</span></span>

<span data-ttu-id="430da-162">Zadejte typ hello tooget hello adresy URL v textu hello hello požadavku protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="430da-162">Specify hello type of hello URL tooget in hello body of hello HTTP request.</span></span> <span data-ttu-id="430da-163">Pokud chráníte svůj obsah pomocí technologie PlayReady, žádosti o adresu URL získání licence Media Services PlayReady pomocí 1 pro hello keyDeliveryType: {"keyDeliveryType": 1}.</span><span class="sxs-lookup"><span data-stu-id="430da-163">If you are protecting your content with PlayReady, request a Media Services PlayReady license acquisition URL, using 1 for hello keyDeliveryType: {"keyDeliveryType":1}.</span></span> <span data-ttu-id="430da-164">Pokud chráníte svůj obsah pomocí šifrování hello obálky, požadavků adresu URL pro získání klíče zadáním 2 pro keyDeliveryType: {"keyDeliveryType": 2}.</span><span class="sxs-lookup"><span data-stu-id="430da-164">If you are protecting your content with hello envelope encryption, request a key acquisition URL by specifying 2 for keyDeliveryType: {"keyDeliveryType":2}.</span></span>

<span data-ttu-id="430da-165">Žádost:</span><span class="sxs-lookup"><span data-stu-id="430da-165">Request:</span></span>

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

<span data-ttu-id="430da-166">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="430da-166">Response:</span></span>

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


### <a name="create-asset-delivery-policy"></a><span data-ttu-id="430da-167">Vytvoření zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-167">Create asset delivery policy</span></span>
<span data-ttu-id="430da-168">Hello následující požadavek HTTP vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply dynamické obálky šifrování (**DynamicEnvelopeEncryption**) toohello **HLS**protokolu (v tomto příkladu jiné protokoly budou při streamování blokovány).</span><span class="sxs-lookup"><span data-stu-id="430da-168">hello following HTTP request creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic envelope encryption (**DynamicEnvelopeEncryption**) toohello **HLS** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="430da-169">Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="430da-169">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="430da-170">Žádost:</span><span class="sxs-lookup"><span data-stu-id="430da-170">Request:</span></span>

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


<span data-ttu-id="430da-171">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="430da-171">Response:</span></span>

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


### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="430da-172">Odkaz asset zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-172">Link asset with asset delivery policy</span></span>
<span data-ttu-id="430da-173">V tématu [odkaz asset zásady doručení assetu](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="430da-173">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="430da-174">Zásady doručení assetu DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="430da-174">DynamicCommonEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-hello-commonencryption-type-and-link-it-toohello-asset"></a><span data-ttu-id="430da-175">Vytvořte klíč obsahu typu CommonEncryption hello a propojit jej toohello asset</span><span class="sxs-lookup"><span data-stu-id="430da-175">Create content key of hello CommonEncryption type and link it toohello asset</span></span>
<span data-ttu-id="430da-176">Při zadávání zásad doručení DynamicCommonEncryption, musíte se toolink toomake váš asset tooa obsahu klíč hello CommonEncryption typu.</span><span class="sxs-lookup"><span data-stu-id="430da-176">When specifying DynamicCommonEncryption delivery policy, you need toomake sure toolink your asset tooa content key of hello CommonEncryption type.</span></span> <span data-ttu-id="430da-177">Další informace najdete v tématu: [vytváření klíč obsahu](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="430da-177">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <a name="get-delivery-url"></a><span data-ttu-id="430da-178">Získat adresu URL pro doručení</span><span class="sxs-lookup"><span data-stu-id="430da-178">Get Delivery URL</span></span>
<span data-ttu-id="430da-179">Získáte hello doručení URL pro metodu doručení PlayReady hello hello obsahu klíče vytvořené v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="430da-179">Get hello delivery URL for hello PlayReady delivery method of hello content key created in hello previous step.</span></span> <span data-ttu-id="430da-180">Klient používá hello vrátil toorequest adresu URL licence PlayReady v pořadí tooplayback hello chráněný obsah.</span><span class="sxs-lookup"><span data-stu-id="430da-180">A client uses hello returned URL toorequest a PlayReady license in order tooplayback hello protected content.</span></span> <span data-ttu-id="430da-181">Další informace najdete v tématu [získat adresu URL doručení](#get_delivery_url).</span><span class="sxs-lookup"><span data-stu-id="430da-181">For more information, see [Get Delivery URL](#get_delivery_url).</span></span>

### <a name="create-asset-delivery-policy"></a><span data-ttu-id="430da-182">Vytvoření zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-182">Create asset delivery policy</span></span>
<span data-ttu-id="430da-183">Hello následující požadavek HTTP vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply běžného dynamického šifrování (**DynamicCommonEncryption**) toohello **technologie Smooth Streaming**  protokolu (v tomto příkladu jiné protokoly budou při streamování blokovány).</span><span class="sxs-lookup"><span data-stu-id="430da-183">hello following HTTP request creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic common encryption (**DynamicCommonEncryption**) toohello **Smooth Streaming** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="430da-184">Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="430da-184">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="430da-185">Žádost:</span><span class="sxs-lookup"><span data-stu-id="430da-185">Request:</span></span>

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


<span data-ttu-id="430da-186">Pokud chcete tooprotect svůj obsah pomocí Widevine DRM, aktualizujte hello AssetDeliveryConfiguration hodnoty toouse WidevineLicenseAcquisitionUrl (což je hodnota hello 7) a zadejte adresu URL hello služby doručování licencí.</span><span class="sxs-lookup"><span data-stu-id="430da-186">If you want tooprotect your content using Widevine DRM, update hello AssetDeliveryConfiguration values toouse WidevineLicenseAcquisitionUrl (which has hello value of 7) and specify hello URL of a license delivery service.</span></span> <span data-ttu-id="430da-187">Můžete použít následující toohelp partneři AMS doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="430da-187">You can use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="430da-188">Například:</span><span class="sxs-lookup"><span data-stu-id="430da-188">For example:</span></span> 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> <span data-ttu-id="430da-189">Při šifrování s technologií Widevine, by být pouze možnost toodeliver pomocí čárka.</span><span class="sxs-lookup"><span data-stu-id="430da-189">When encrypting with Widevine, you would only be able toodeliver using DASH.</span></span> <span data-ttu-id="430da-190">Ujistěte se, že toospecify DASH (2) v hello doručovací protokol assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-190">Make sure toospecify DASH (2) in hello asset delivery protocol.</span></span>
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="430da-191">Odkaz asset zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="430da-191">Link asset with asset delivery policy</span></span>
<span data-ttu-id="430da-192">V tématu [odkaz asset zásady doručení assetu](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="430da-192">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <span data-ttu-id="430da-193"><a id="types"></a>Typy používané při definování AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="430da-193"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <a name="assetdeliveryprotocol"></a><span data-ttu-id="430da-194">AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="430da-194">AssetDeliveryProtocol</span></span>

<span data-ttu-id="430da-195">Hello následující výčtu popisuje hodnoty, které lze nastavit pro doručovací protokol assetu hello.</span><span class="sxs-lookup"><span data-stu-id="430da-195">hello following enum describes values you can set for hello asset delivery protocol.</span></span>

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

### <a name="assetdeliverypolicytype"></a><span data-ttu-id="430da-196">AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="430da-196">AssetDeliveryPolicyType</span></span>

<span data-ttu-id="430da-197">Hello následující výčtu popisuje hodnoty, které můžete zadat pro typ zásady doručení assetu hello.</span><span class="sxs-lookup"><span data-stu-id="430da-197">hello following enum describes values you can set for hello asset delivery policy type.</span></span>  

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

### <a name="contentkeydeliverytype"></a><span data-ttu-id="430da-198">ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="430da-198">ContentKeyDeliveryType</span></span>

<span data-ttu-id="430da-199">Hello následující výčtu popisuje hodnoty, které můžete použít metodu doručení hello tooconfigure hello obsahu toohello klíče klienta.</span><span class="sxs-lookup"><span data-stu-id="430da-199">hello following enum describes values you can use tooconfigure hello delivery method of hello content key toohello client.</span></span>
    
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


### <a name="assetdeliverypolicyconfigurationkey"></a><span data-ttu-id="430da-200">AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="430da-200">AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="430da-201">Následující výčet Hello popisuje hodnoty můžete nastavit určité konfigurace tooget tooconfigure klíčů používaných pro zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="430da-201">hello following enum describes values you can set tooconfigure keys used tooget specific configuration for an asset delivery policy.</span></span>

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="430da-202">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="430da-202">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="430da-203">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="430da-203">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

