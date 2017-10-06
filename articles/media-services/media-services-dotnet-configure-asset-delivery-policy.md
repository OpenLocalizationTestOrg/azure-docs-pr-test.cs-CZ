---
title: "zásady doručení mediálního aaaConfigure pomocí .NET SDK | Microsoft Docs"
description: "Toto téma ukazuje, jak zásady doručení tooconfigure jiný prostředek s Azure Media Services .NET SDK."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/13/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: a6f2644d639cd36d4cdc269b6f01fd4acdf7160b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a><span data-ttu-id="23a7f-103">Nakonfigurujte zásady doručení assetu pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="23a7f-103">Configure asset delivery policies with .NET SDK</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a><span data-ttu-id="23a7f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="23a7f-104">Overview</span></span>
<span data-ttu-id="23a7f-105">Pokud máte v plánu toodelivery šifrované prostředky, jeden z hello kroků v hello Media Services obsahu doručení pracovního postupu je konfigurace zásad doručení pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="23a7f-105">If you plan toodelivery encrypted assets, one of hello steps in hello Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="23a7f-106">zásady doručení assetu Hello informuje Media Services, jak chcete použít pro váš asset toobe doručit: do streamování protokol, který by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete toodynamically váš asset šifrovat a jak (obálky nebo common encryption).</span><span class="sxs-lookup"><span data-stu-id="23a7f-106">hello asset delivery policy tells Media Services how you want for your asset toobe delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want toodynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="23a7f-107">Toto téma popisuje, proč a jak toocreate a nakonfigurujte zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-107">This topic discusses why and how toocreate and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="23a7f-108">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-108">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="23a7f-109">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-109">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="23a7f-110">Navíc toobe možné toouse dynamické balením a dynamickým šifrováním váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="23a7f-110">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>


<span data-ttu-id="23a7f-111">Je možné aplikovat různé zásady toohello stejné asset.</span><span class="sxs-lookup"><span data-stu-id="23a7f-111">You could apply different policies toohello same asset.</span></span> <span data-ttu-id="23a7f-112">Například může použít PlayReady šifrování tooSmooth Streaming a pomocí standardu AES Envelope šifrování tooMPEG DASH a HLS.</span><span class="sxs-lookup"><span data-stu-id="23a7f-112">For example, you could apply PlayReady encryption tooSmooth Streaming and AES Envelope encryption tooMPEG DASH and HLS.</span></span> <span data-ttu-id="23a7f-113">Veškeré protokoly, které nejsou v zásadách doručení definovány (například přidáte jedinou zásadu, která jako hello protokol určuje pouze HLS) budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="23a7f-113">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="23a7f-114">Výjimka toothis Hello je, pokud máte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-114">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="23a7f-115">Potom bude možné v hello zrušte všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="23a7f-115">Then, all protocols will be allowed in hello clear.</span></span>

<span data-ttu-id="23a7f-116">Pokud chcete toodeliver asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello.</span><span class="sxs-lookup"><span data-stu-id="23a7f-116">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="23a7f-117">Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení.</span><span class="sxs-lookup"><span data-stu-id="23a7f-117">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="23a7f-118">Například toodeliver asset šifrován Advanced Encryption (Standard AES) obálky šifrovací klíč, nastavte typ zásad hello příliš**DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="23a7f-118">For example, toodeliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set hello policy type too**DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="23a7f-119">šifrování úložiště tooremove a asset hello datový proud v hello jasné, nastavte typ zásad hello příliš**NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="23a7f-119">tooremove storage encryption and stream hello asset in hello clear, set hello policy type too**NoDynamicEncryption**.</span></span> <span data-ttu-id="23a7f-120">Příklady, které ukazují, jak tooconfigure tyto typy zásad podle.</span><span class="sxs-lookup"><span data-stu-id="23a7f-120">Examples that show how tooconfigure these policy types follow.</span></span>

<span data-ttu-id="23a7f-121">V závislosti na tom, jak nakonfigurovat zásady doručení assetu hello je bude možné toodynamically balíčku, dynamicky šifrovat a stream hello následujících protokolů datových proudů: datové proudy technologie Smooth Streaming, HLS a MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="23a7f-121">Depending on how you configure hello asset delivery policy you would be able toodynamically package, dynamically encrypt, and stream hello following streaming protocols: Smooth Streaming, HLS, and MPEG DASH streams.</span></span>

<span data-ttu-id="23a7f-122">Hello následujícím seznamu jsou formáty hello použijete toostream Smooth, HLS a POMLČKY.</span><span class="sxs-lookup"><span data-stu-id="23a7f-122">hello following list shows hello formats that you use toostream Smooth, HLS, and DASH.</span></span>

<span data-ttu-id="23a7f-123">Technologie Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="23a7f-123">Smooth Streaming:</span></span>

<span data-ttu-id="23a7f-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="23a7f-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="23a7f-125">HLS:</span><span class="sxs-lookup"><span data-stu-id="23a7f-125">HLS:</span></span>

<span data-ttu-id="23a7f-126">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="23a7f-126">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="23a7f-127">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="23a7f-127">MPEG DASH</span></span>

<span data-ttu-id="23a7f-128">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="23a7f-128">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


## <a name="considerations"></a><span data-ttu-id="23a7f-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="23a7f-129">Considerations</span></span>
* <span data-ttu-id="23a7f-130">Nelze odstranit AssetDeliveryPolicy přidružený prostředek při Lokátor OnDemand (streaming) existuje pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="23a7f-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="23a7f-131">Hello doporučení je zásada hello tooremove z hello asset před odstraněním hello zásad.</span><span class="sxs-lookup"><span data-stu-id="23a7f-131">hello recommendation is tooremove hello policy from hello asset before deleting hello policy.</span></span>
* <span data-ttu-id="23a7f-132">Šifrované majetku úložiště nelze vytvořit lokátor streamování, nastavena žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="23a7f-133">Není-li hello Asset šifrování úložiště, hello systému vám umožní vytvoření prostředku hello Lokátor a datový proud v hello zrušte bez zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-133">If hello Asset isn’t storage encrypted, hello system will let you create a locator and stream hello asset in hello clear without an asset delivery policy.</span></span>
* <span data-ttu-id="23a7f-134">Může mít několik zásady doručení mediálního přidružené jednoho datového zdroje, ale můžete zadat jenom jeden ze způsobů toohandle daného AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="23a7f-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way toohandle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="23a7f-135">Znamená, pokud se pokusíte toolink dvě zásady doručení určující hello AssetDeliveryProtocol.SmoothStreaming protokol, který bude výsledkem chyba, protože systém hello nebude vědět, které z nich chcete tooapply když klient zadává žádost technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="23a7f-135">Meaning if you try toolink two delivery policies that specify hello AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because hello system does not know which one you want it tooapply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="23a7f-136">Pokud máte prostředek s stávající Lokátor streamování, nelze propojit nový prostředek zásad toohello (můžete odpojit existující zásady z hello asset, nebo aktualizujete zásady pro doručení přidružené hello asset).</span><span class="sxs-lookup"><span data-stu-id="23a7f-136">If you have an asset with an existing streaming locator, you cannot link a new policy toohello asset (you can either unlink an existing policy from hello asset, or update a delivery policy associated with hello asset).</span></span>  <span data-ttu-id="23a7f-137">Můžete nejprve mít Lokátor streamování hello tooremove, upravte hello zásady a potom je znovu vytvořit lokátor streamování hello.</span><span class="sxs-lookup"><span data-stu-id="23a7f-137">You first have tooremove hello streaming locator, adjust hello policies, and then re-create hello streaming locator.</span></span>  <span data-ttu-id="23a7f-138">Můžete použít hello, které by měly stejné locatorId při vytvoření hello streamování Lokátor ale můžete zajistit, který nezpůsobí problémy pro klienty, protože do mezipaměti obsah hello původ nebo příjem dat CDN.</span><span class="sxs-lookup"><span data-stu-id="23a7f-138">You can use hello same locatorId when you recreate hello streaming locator but you should ensure that won’t cause issues for clients since content can be cached by hello origin or a downstream CDN.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="23a7f-139">Zásady doručení assetu vymazat</span><span class="sxs-lookup"><span data-stu-id="23a7f-139">Clear asset delivery policy</span></span>

<span data-ttu-id="23a7f-140">Následující Hello **ConfigureClearAssetDeliveryPolicy** metoda určuje toonot použít dynamické šifrování a protokoly datového proudu hello toodeliver v některém z následujících hello: protokoly MPEG DASH, HLS nebo technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="23a7f-140">hello following **ConfigureClearAssetDeliveryPolicy** method specifies toonot apply dynamic encryption and toodeliver hello stream in any of hello following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> <span data-ttu-id="23a7f-141">Můžete chtít tooapply této zásady tooyour šifrování úložiště prostředky.</span><span class="sxs-lookup"><span data-stu-id="23a7f-141">You might want tooapply this policy tooyour storage encrypted assets.</span></span>

<span data-ttu-id="23a7f-142">Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="23a7f-142">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="23a7f-143">Zásady doručení assetu DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="23a7f-143">DynamicCommonEncryption asset delivery policy</span></span>

<span data-ttu-id="23a7f-144">Následující Hello **CreateAssetDeliveryPolicy** metoda vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply běžného dynamického šifrování (**DynamicCommonEncryption**) tooa technologie smooth streaming protocol (jiné protokoly, bude zablokován streamování).</span><span class="sxs-lookup"><span data-stu-id="23a7f-144">hello following **CreateAssetDeliveryPolicy** method creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic common encryption (**DynamicCommonEncryption**) tooa smooth streaming protocol (other protocols will be blocked from streaming).</span></span> <span data-ttu-id="23a7f-145">Hello metoda přebírá dva parametry: **Asset** (hello asset toowhich chcete zásady doručení hello tooapply) a **IContentKey** (hello obsahu klíč hello **CommonEncryption**typu, další informace najdete v tématu: [vytváření klíč obsahu](media-services-dotnet-create-contentkey.md#common_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="23a7f-145">hello method takes two parameters : **Asset** (hello asset toowhich you want tooapply hello delivery policy) and **IContentKey** (hello content key of hello **CommonEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#common_contentkey)).</span></span>

<span data-ttu-id="23a7f-146">Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="23a7f-146">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            };
    
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                    "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicCommonEncryption,
                AssetDeliveryProtocol.SmoothStreaming,
                assetDeliveryPolicyConfiguration);
    
            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }

<span data-ttu-id="23a7f-147">Azure Media Services také umožňuje tooadd Widevine šifrování.</span><span class="sxs-lookup"><span data-stu-id="23a7f-147">Azure Media Services also enables you tooadd Widevine encryption.</span></span> <span data-ttu-id="23a7f-148">Hello následující příklad ukazuje technologie PlayReady a Widevine přidávané zásady doručení assetu toohello.</span><span class="sxs-lookup"><span data-stu-id="23a7f-148">hello following example demonstrates both PlayReady and Widevine being added toohello asset delivery policy.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get hello PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy toohello asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

> [!NOTE]
> <span data-ttu-id="23a7f-149">Při šifrování s technologií Widevine, by být pouze možnost toodeliver pomocí čárka.</span><span class="sxs-lookup"><span data-stu-id="23a7f-149">When encrypting with Widevine, you would only be able toodeliver using DASH.</span></span> <span data-ttu-id="23a7f-150">Ujistěte se, že toospecify DASH v doručovací protokol assetu hello.</span><span class="sxs-lookup"><span data-stu-id="23a7f-150">Make sure toospecify DASH in hello asset delivery protocol.</span></span>
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="23a7f-151">Zásady doručení assetu DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="23a7f-151">DynamicEnvelopeEncryption asset delivery policy</span></span>
<span data-ttu-id="23a7f-152">Následující Hello **CreateAssetDeliveryPolicy** metoda vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply dynamické obálky šifrování (**DynamicEnvelopeEncryption** ) tooSmooth protokoly Streaming, HLS a DASH (Pokud se rozhodnete toonot zadejte některé protokoly, že budou Blokovaní z streamování).</span><span class="sxs-lookup"><span data-stu-id="23a7f-152">hello following **CreateAssetDeliveryPolicy** method creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic envelope encryption (**DynamicEnvelopeEncryption**) tooSmooth Streaming, HLS, and DASH protocols (if you decide toonot specify some protocols, they will be blocked from streaming).</span></span> <span data-ttu-id="23a7f-153">Hello metoda přebírá dva parametry: **Asset** (hello asset toowhich chcete zásady doručení hello tooapply) a **IContentKey** (hello obsahu klíč hello **EnvelopeEncryption**typu, další informace najdete v tématu: [vytváření klíč obsahu](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="23a7f-153">hello method takes two parameters : **Asset** (hello asset toowhich you want tooapply hello delivery policy) and **IContentKey** (hello content key of hello **EnvelopeEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span></span>

<span data-ttu-id="23a7f-154">Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="23a7f-154">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get hello Key Delivery Base Url by removing hello Query parameter.  hello Dynamic Encryption service will
        //  automatically add hello correct key identifier toohello url when it generates hello Envelope encrypted content
        //  manifest.  Omitting hello IV will also cause hello Dynamice Encryption service toogenerate a deterministic
        //  IV for hello content automatically.  By using hello EnvelopeBaseKeyAcquisitionUrl and omitting hello IV, this
        //  allows hello AssetDelivery policy toobe reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // hello following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended toohello envelope and
        //   hello Initialization Vector (IV) toouse for hello envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy toohello asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


## <span data-ttu-id="23a7f-155"><a id="types"></a>Typy používané při definování AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="23a7f-155"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <span data-ttu-id="23a7f-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="23a7f-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span></span>

<span data-ttu-id="23a7f-157">Hello následující výčtu popisuje hodnoty, které lze nastavit pro doručovací protokol assetu hello.</span><span class="sxs-lookup"><span data-stu-id="23a7f-157">hello following enum describes values you can set for hello asset delivery protocol.</span></span>

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

### <span data-ttu-id="23a7f-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="23a7f-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span></span>

<span data-ttu-id="23a7f-159">Hello následující výčtu popisuje hodnoty, které můžete zadat pro typ zásady doručení assetu hello.</span><span class="sxs-lookup"><span data-stu-id="23a7f-159">hello following enum describes values you can set for hello asset delivery policy type.</span></span>  

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

### <span data-ttu-id="23a7f-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="23a7f-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>

<span data-ttu-id="23a7f-161">Hello následující výčtu popisuje hodnoty, které můžete použít metodu doručení hello tooconfigure hello obsahu toohello klíče klienta.</span><span class="sxs-lookup"><span data-stu-id="23a7f-161">hello following enum describes values you can use tooconfigure hello delivery method of hello content key toohello client.</span></span>
    
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

### <span data-ttu-id="23a7f-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="23a7f-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="23a7f-163">Následující výčet Hello popisuje hodnoty můžete nastavit určité konfigurace tooget tooconfigure klíčů používaných pro zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="23a7f-163">hello following enum describes values you can set tooconfigure keys used tooget specific configuration for an asset delivery policy.</span></span>

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="23a7f-164">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="23a7f-164">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="23a7f-165">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="23a7f-165">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

