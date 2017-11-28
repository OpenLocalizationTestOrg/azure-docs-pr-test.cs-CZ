---
title: "Konfigurace zásad doručení assetu pomocí Media Services REST API | Microsoft Docs"
description: "Toto téma ukazuje, jak nakonfigurovat zásady doručení jiný prostředek pomocí Media Services REST API."
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
ms.openlocfilehash: 7ffbde11b943961dd3a3b5edebd0cfd52429e845
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-asset-delivery-policies"></a><span data-ttu-id="3e114-103">Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-103">Configuring asset delivery policies</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

<span data-ttu-id="3e114-104">Pokud máte v plánu pro doručování dynamicky šifrovaných prostředky, jeden z kroků v pracovním postupu doručování obsahu Media Services je konfigurace zásad doručení pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="3e114-104">If you plan to deliver dynamically encrypted assets, one of the steps in the Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="3e114-105">Zásady doručení assetu informuje Media Services, jak chcete použít pro váš asset, který bude doručen: do které protokol pro streamování by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete dynamicky šifrovat. váš asset a jak (obálky nebo common encryption).</span><span class="sxs-lookup"><span data-stu-id="3e114-105">The asset delivery policy tells Media Services how you want for your asset to be delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want to dynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="3e114-106">Toto téma popisuje, proč a jak vytvořit a nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-106">This topic discusses why and how to create and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="3e114-107">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="3e114-107">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="3e114-108">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="3e114-108">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="3e114-109">Abyste mohli používat dynamické balením a dynamickým šifrováním také váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="3e114-109">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="3e114-110">Různé zásady je možné aplikovat na stejný asset.</span><span class="sxs-lookup"><span data-stu-id="3e114-110">You could apply different policies to the same asset.</span></span> <span data-ttu-id="3e114-111">Můžete například použít šifrování PlayReady na technologie Smooth Streaming a pomocí standardu AES Envelope šifrování a MPEG DASH, HLS.</span><span class="sxs-lookup"><span data-stu-id="3e114-111">For example, you could apply PlayReady encryption to Smooth Streaming and AES Envelope encryption to MPEG DASH and HLS.</span></span> <span data-ttu-id="3e114-112">Veškeré protokoly, které nejsou v zásadách doručení definovány (například když přidáte jedinou zásadu, která jako protokol určuje pouze HLS), budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="3e114-112">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="3e114-113">Výjimkou je, pokud nemáte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-113">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="3e114-114">Pak budou všechny protokoly povolené v nešifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="3e114-114">Then, all protocols will be allowed in the clear.</span></span>

<span data-ttu-id="3e114-115">Pokud chcete doručovat šifrované asset úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-115">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="3e114-116">Před asset Streamovat, server datových proudů odebere šifrování úložiště a datové proudy svůj obsah pomocí zadaného doručování zásad.</span><span class="sxs-lookup"><span data-stu-id="3e114-116">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="3e114-117">Například k poskytování asset šifrován Advanced Encryption (Standard AES) obálky šifrovací klíč, nastavte typ zásad na **DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="3e114-117">For example, to deliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set the policy type to **DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="3e114-118">Pokud chcete odebrat šifrování úložiště a Streamovat prostředek v nešifrované podobě, nastavte typ zásad na **NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="3e114-118">To remove storage encryption and stream the asset in the clear, set the policy type to **NoDynamicEncryption**.</span></span> <span data-ttu-id="3e114-119">Postupujte podle příklady, které ukazují, jak konfigurovat tyto typy zásad.</span><span class="sxs-lookup"><span data-stu-id="3e114-119">Examples that show how to configure these policy types follow.</span></span>

<span data-ttu-id="3e114-120">V závislosti na tom, jak nakonfigurovat zásady doručení assetu by nebudete moct dynamicky balíčku, dynamicky šifrovat a stream u následujících protokolů streamování: technologie Smooth Streaming, HLS, datové proudy MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="3e114-120">Depending on how you configure the asset delivery policy you would be able to dynamically package, dynamically encrypt, and stream the following streaming protocols: Smooth Streaming, HLS, MPEG DASH streams.</span></span>

<span data-ttu-id="3e114-121">V následujícím seznamu jsou formáty, který používáte pro datový proud Smooth, HLS, DASH.</span><span class="sxs-lookup"><span data-stu-id="3e114-121">The following list shows the formats that you use to stream Smooth, HLS, DASH.</span></span>

<span data-ttu-id="3e114-122">Technologie Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="3e114-122">Smooth Streaming:</span></span>

<span data-ttu-id="3e114-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="3e114-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="3e114-124">HLS:</span><span class="sxs-lookup"><span data-stu-id="3e114-124">HLS:</span></span>

<span data-ttu-id="3e114-125">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="3e114-125">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="3e114-126">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="3e114-126">MPEG DASH</span></span>

<span data-ttu-id="3e114-127">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="3e114-127">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


<span data-ttu-id="3e114-128">Pokyny k publikování assetu a vytvoření adresy URL streamování najdete v článku o [vytvoření adresy URL streamování](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="3e114-128">For instructions on how to publish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="3e114-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e114-129">Considerations</span></span>
* <span data-ttu-id="3e114-130">Nelze odstranit AssetDeliveryPolicy přidružený prostředek při Lokátor OnDemand (streaming) existuje pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="3e114-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="3e114-131">Doporučuje se před odstraněním zásady odeberte zásady z prostředku.</span><span class="sxs-lookup"><span data-stu-id="3e114-131">The recommendation is to remove the policy from the asset before deleting the policy.</span></span>
* <span data-ttu-id="3e114-132">Šifrované majetku úložiště nelze vytvořit lokátor streamování, nastavena žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="3e114-133">Není-li Asset šifrování úložiště, systém vám umožní vytvořit Lokátor a Streamovat prostředek v nešifrované podobě bez zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-133">If the Asset isn’t storage encrypted, the system will let you create a locator and stream the asset in the clear without an asset delivery policy.</span></span>
* <span data-ttu-id="3e114-134">Můžete mít více zásady doručení mediálního přidružené jednoho datového zdroje, ale můžete určit pouze jeden způsob, jak zpracovávat dané AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="3e114-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way to handle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="3e114-135">Znamená, pokud se pokusíte propojit dvě zásady doručení, které zadat AssetDeliveryProtocol.SmoothStreaming protokol, který bude výsledkem chyba, protože systém nebude vědět, který jeden se má použít, když klient odešle požadavek technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="3e114-135">Meaning if you try to link two delivery policies that specify the AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because the system does not know which one you want it to apply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="3e114-136">Pokud máte prostředek s stávající Lokátor streamování, nelze propojit nové zásady pro daný prostředek, odpojit existující zásady z prostředku nebo aktualizujete zásady pro doručení přidružený asset.</span><span class="sxs-lookup"><span data-stu-id="3e114-136">If you have an asset with an existing streaming locator, you cannot link a new policy to the asset, unlink an existing policy from the asset, or update a delivery policy associated with the asset.</span></span>  <span data-ttu-id="3e114-137">Nejdřív musíte odstraňte Lokátor streamování, upravit zásady a potom je znovu vytvořit lokátor streamování.</span><span class="sxs-lookup"><span data-stu-id="3e114-137">You first have to remove the streaming locator, adjust the policies, and then re-create the streaming locator.</span></span>  <span data-ttu-id="3e114-138">Stejné locatorId můžete použít, když je znovu vytvořit lokátor streamování, ale měli byste zajistit, že vzhledem k tomu, že do mezipaměti obsah počátek nebo podřízené CDN, který nebude způsobovat problémy pro klienty.</span><span class="sxs-lookup"><span data-stu-id="3e114-138">You can use the same locatorId when you recreate the streaming locator but you should ensure that won’t cause issues for clients since content can be cached by the origin or a downstream CDN.</span></span>

>[!NOTE]

><span data-ttu-id="3e114-139">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e114-139">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="3e114-140">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="3e114-140">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="3e114-141">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="3e114-141">Connect to Media Services</span></span>

<span data-ttu-id="3e114-142">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="3e114-142">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="3e114-143">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="3e114-143">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="3e114-144">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="3e114-144">You must make subsequent calls to the new URI.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="3e114-145">Zásady doručení assetu vymazat</span><span class="sxs-lookup"><span data-stu-id="3e114-145">Clear asset delivery policy</span></span>
### <span data-ttu-id="3e114-146"><a id="create_asset_delivery_policy"></a>Vytvoření zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-146"><a id="create_asset_delivery_policy"></a>Create asset delivery policy</span></span>
<span data-ttu-id="3e114-147">Následující požadavek HTTP vytvoří zásady doručení assetu, který určuje nelze použít dynamické šifrování a poskytovat datový proud v některém z těchto protokolů: protokoly MPEG DASH, HLS nebo technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="3e114-147">The following HTTP request creates an asset delivery policy that specifies to not apply dynamic encryption and to deliver the stream in any of the following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> 

<span data-ttu-id="3e114-148">Informace na hodnotách, které můžete zadat při vytváření AssetDeliveryPolicy najdete v tématu [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="3e114-148">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="3e114-149">Žádost:</span><span class="sxs-lookup"><span data-stu-id="3e114-149">Request:</span></span>

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

<span data-ttu-id="3e114-150">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="3e114-150">Response:</span></span>

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

### <span data-ttu-id="3e114-151"><a id="link_asset_with_asset_delivery_policy"></a>Odkaz asset zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-151"><a id="link_asset_with_asset_delivery_policy"></a>Link asset with asset delivery policy</span></span>
<span data-ttu-id="3e114-152">Následující požadavek HTTP odkazuje na zásady doručení assetu pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="3e114-152">The following HTTP request links the specified asset to the asset delivery policy to.</span></span>

<span data-ttu-id="3e114-153">Žádost:</span><span class="sxs-lookup"><span data-stu-id="3e114-153">Request:</span></span>

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

<span data-ttu-id="3e114-154">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="3e114-154">Response:</span></span>

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="3e114-155">Zásady doručení assetu DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="3e114-155">DynamicEnvelopeEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a><span data-ttu-id="3e114-156">Vytvořte klíč obsahu typu EnvelopeEncryption a tu propojit na prostředku</span><span class="sxs-lookup"><span data-stu-id="3e114-156">Create content key of the EnvelopeEncryption type and link it to the asset</span></span>
<span data-ttu-id="3e114-157">Při zadávání zásad doručení DynamicEnvelopeEncryption, budete muset nezapomeňte propojit klíč obsahu typu EnvelopeEncryption asset.</span><span class="sxs-lookup"><span data-stu-id="3e114-157">When specifying DynamicEnvelopeEncryption delivery policy, you need to make sure to link your asset to a content key of the EnvelopeEncryption type.</span></span> <span data-ttu-id="3e114-158">Další informace najdete v tématu: [vytváření klíč obsahu](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="3e114-158">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <span data-ttu-id="3e114-159"><a id="get_delivery_url"></a>Získat adresu URL pro doručení</span><span class="sxs-lookup"><span data-stu-id="3e114-159"><a id="get_delivery_url"></a>Get delivery URL</span></span>
<span data-ttu-id="3e114-160">Získáte adresu URL doručení pro metodu zadaný doručení obsahu klíče vytvořeného v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3e114-160">Get the delivery URL for the specified delivery method of the content key created in the previous step.</span></span> <span data-ttu-id="3e114-161">Klient používá vrácené adresu URL k vyžádání AES klíč nebo licence PlayReady, aby přehrávání chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="3e114-161">A client uses the returned URL to request an AES key or a PlayReady license in order to playback the protected content.</span></span>

<span data-ttu-id="3e114-162">Zadejte typ adresy URL získat v textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e114-162">Specify the type of the URL to get in the body of the HTTP request.</span></span> <span data-ttu-id="3e114-163">Pokud chráníte svůj obsah pomocí technologie PlayReady, žádosti o adresu URL získání licence Media Services PlayReady pomocí 1 pro keyDeliveryType: {"keyDeliveryType": 1}.</span><span class="sxs-lookup"><span data-stu-id="3e114-163">If you are protecting your content with PlayReady, request a Media Services PlayReady license acquisition URL, using 1 for the keyDeliveryType: {"keyDeliveryType":1}.</span></span> <span data-ttu-id="3e114-164">Pokud chráníte svůj obsah pomocí šifrování obálky, požadavků adresu URL pro získání klíče zadáním 2 pro keyDeliveryType: {"keyDeliveryType": 2}.</span><span class="sxs-lookup"><span data-stu-id="3e114-164">If you are protecting your content with the envelope encryption, request a key acquisition URL by specifying 2 for keyDeliveryType: {"keyDeliveryType":2}.</span></span>

<span data-ttu-id="3e114-165">Žádost:</span><span class="sxs-lookup"><span data-stu-id="3e114-165">Request:</span></span>

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

<span data-ttu-id="3e114-166">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="3e114-166">Response:</span></span>

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


### <a name="create-asset-delivery-policy"></a><span data-ttu-id="3e114-167">Vytvoření zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-167">Create asset delivery policy</span></span>
<span data-ttu-id="3e114-168">Vytvoří následující požadavek HTTP **AssetDeliveryPolicy** nakonfigurovaný pro použití dynamické obálky šifrování (**DynamicEnvelopeEncryption**) k **HLS** protokol (v tomto příkladu jiné protokoly budou při streamování blokovány).</span><span class="sxs-lookup"><span data-stu-id="3e114-168">The following HTTP request creates the **AssetDeliveryPolicy** that is configured to apply dynamic envelope encryption (**DynamicEnvelopeEncryption**) to the **HLS** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="3e114-169">Informace na hodnotách, které můžete zadat při vytváření AssetDeliveryPolicy najdete v tématu [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="3e114-169">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="3e114-170">Žádost:</span><span class="sxs-lookup"><span data-stu-id="3e114-170">Request:</span></span>

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


<span data-ttu-id="3e114-171">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="3e114-171">Response:</span></span>

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


### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="3e114-172">Odkaz asset zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-172">Link asset with asset delivery policy</span></span>
<span data-ttu-id="3e114-173">V tématu [odkaz asset zásady doručení assetu](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="3e114-173">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="3e114-174">Zásady doručení assetu DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="3e114-174">DynamicCommonEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a><span data-ttu-id="3e114-175">Vytvořte klíč obsahu typu CommonEncryption a tu propojit na prostředku</span><span class="sxs-lookup"><span data-stu-id="3e114-175">Create content key of the CommonEncryption type and link it to the asset</span></span>
<span data-ttu-id="3e114-176">Při zadávání zásad doručení DynamicCommonEncryption, budete muset nezapomeňte propojit klíč obsahu typu CommonEncryption asset.</span><span class="sxs-lookup"><span data-stu-id="3e114-176">When specifying DynamicCommonEncryption delivery policy, you need to make sure to link your asset to a content key of the CommonEncryption type.</span></span> <span data-ttu-id="3e114-177">Další informace najdete v tématu: [vytváření klíč obsahu](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="3e114-177">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <a name="get-delivery-url"></a><span data-ttu-id="3e114-178">Získat adresu URL pro doručení</span><span class="sxs-lookup"><span data-stu-id="3e114-178">Get Delivery URL</span></span>
<span data-ttu-id="3e114-179">Získáte adresu URL doručení pro metodu doručení PlayReady obsahu klíče vytvořeného v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3e114-179">Get the delivery URL for the PlayReady delivery method of the content key created in the previous step.</span></span> <span data-ttu-id="3e114-180">Klient používá vrácené adresu URL k vyžádání licence PlayReady, aby přehrávání chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="3e114-180">A client uses the returned URL to request a PlayReady license in order to playback the protected content.</span></span> <span data-ttu-id="3e114-181">Další informace najdete v tématu [získat adresu URL doručení](#get_delivery_url).</span><span class="sxs-lookup"><span data-stu-id="3e114-181">For more information, see [Get Delivery URL](#get_delivery_url).</span></span>

### <a name="create-asset-delivery-policy"></a><span data-ttu-id="3e114-182">Vytvoření zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-182">Create asset delivery policy</span></span>
<span data-ttu-id="3e114-183">Vytvoří následující požadavek HTTP **AssetDeliveryPolicy** nakonfigurovaný pro použití běžného dynamického šifrování (**DynamicCommonEncryption**) do **technologie Smooth Streaming**protokolu (v tomto příkladu jiné protokoly budou při streamování blokovány).</span><span class="sxs-lookup"><span data-stu-id="3e114-183">The following HTTP request creates the **AssetDeliveryPolicy** that is configured to apply dynamic common encryption (**DynamicCommonEncryption**) to the **Smooth Streaming** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="3e114-184">Informace na hodnotách, které můžete zadat při vytváření AssetDeliveryPolicy najdete v tématu [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="3e114-184">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="3e114-185">Žádost:</span><span class="sxs-lookup"><span data-stu-id="3e114-185">Request:</span></span>

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


<span data-ttu-id="3e114-186">Pokud chcete chránit obsah pomocí Widevine DRM, aktualizujte hodnoty AssetDeliveryConfiguration používat WidevineLicenseAcquisitionUrl (což je hodnota 7) a zadejte adresu URL služby doručování licencí.</span><span class="sxs-lookup"><span data-stu-id="3e114-186">If you want to protect your content using Widevine DRM, update the AssetDeliveryConfiguration values to use WidevineLicenseAcquisitionUrl (which has the value of 7) and specify the URL of a license delivery service.</span></span> <span data-ttu-id="3e114-187">Můžete použít následující partneři AMS doručit licence na Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="3e114-187">You can use the following AMS partners to help you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="3e114-188">Například:</span><span class="sxs-lookup"><span data-stu-id="3e114-188">For example:</span></span> 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> <span data-ttu-id="3e114-189">Při šifrování s technologií Widevine, by pouze možné doručíte pomocí čárka.</span><span class="sxs-lookup"><span data-stu-id="3e114-189">When encrypting with Widevine, you would only be able to deliver using DASH.</span></span> <span data-ttu-id="3e114-190">Nezapomeňte zadat DASH (2) v doručovací protokol assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-190">Make sure to specify DASH (2) in the asset delivery protocol.</span></span>
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="3e114-191">Odkaz asset zásady doručení assetu</span><span class="sxs-lookup"><span data-stu-id="3e114-191">Link asset with asset delivery policy</span></span>
<span data-ttu-id="3e114-192">V tématu [odkaz asset zásady doručení assetu](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="3e114-192">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <span data-ttu-id="3e114-193"><a id="types"></a>Typy používané při definování AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="3e114-193"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <a name="assetdeliveryprotocol"></a><span data-ttu-id="3e114-194">AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="3e114-194">AssetDeliveryProtocol</span></span>

<span data-ttu-id="3e114-195">Následující výčet popisuje hodnoty, které lze nastavit pro doručovací protokol assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-195">The following enum describes values you can set for the asset delivery protocol.</span></span>

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

### <a name="assetdeliverypolicytype"></a><span data-ttu-id="3e114-196">AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="3e114-196">AssetDeliveryPolicyType</span></span>

<span data-ttu-id="3e114-197">Následující výčet popisuje hodnoty, které můžete zadat pro typ zásad doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-197">The following enum describes values you can set for the asset delivery policy type.</span></span>  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
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

### <a name="contentkeydeliverytype"></a><span data-ttu-id="3e114-198">ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="3e114-198">ContentKeyDeliveryType</span></span>

<span data-ttu-id="3e114-199">Následující výčet popisuje hodnoty, které můžete použít ke konfiguraci metodu doručení obsahu klíče, který se klient.</span><span class="sxs-lookup"><span data-stu-id="3e114-199">The following enum describes values you can use to configure the delivery method of the content key to the client.</span></span>
    
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


### <a name="assetdeliverypolicyconfigurationkey"></a><span data-ttu-id="3e114-200">AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="3e114-200">AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="3e114-201">Následující výčet popisuje hodnoty, které můžete nastavit, aby konfigurace klíče, které slouží k získání konkrétní konfigurace pro zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="3e114-201">The following enum describes values you can set to configure keys used to get specific configuration for an asset delivery policy.</span></span>

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
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="3e114-202">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="3e114-202">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3e114-203">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="3e114-203">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

