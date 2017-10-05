---
title: "Nakonfigurujte zásady doručení assetu pomocí .NET SDK | Microsoft Docs"
description: "Toto téma ukazuje, jak nakonfigurovat zásady doručení jiný prostředek Azure Media Services .NET SDK."
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
ms.openlocfilehash: 282fd9e24dc147e31613469926128894d48366f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a><span data-ttu-id="cc51e-103">Nakonfigurujte zásady doručení assetu pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="cc51e-103">Configure asset delivery policies with .NET SDK</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a><span data-ttu-id="cc51e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="cc51e-104">Overview</span></span>
<span data-ttu-id="cc51e-105">Pokud budete chtít doručení šifrované prostředky, jeden z kroků v pracovním postupu doručování obsahu Media Services je konfigurace zásad doručení pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="cc51e-105">If you plan to delivery encrypted assets, one of the steps in the Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="cc51e-106">Zásady doručení assetu informuje Media Services, jak chcete použít pro váš asset, který bude doručen: do které protokol pro streamování by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete dynamicky šifrovat. váš asset a jak (obálky nebo common encryption).</span><span class="sxs-lookup"><span data-stu-id="cc51e-106">The asset delivery policy tells Media Services how you want for your asset to be delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want to dynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="cc51e-107">Toto téma popisuje, proč a jak vytvořit a nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-107">This topic discusses why and how to create and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="cc51e-108">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="cc51e-108">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="cc51e-109">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="cc51e-109">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="cc51e-110">Abyste mohli používat dynamické balením a dynamickým šifrováním také váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="cc51e-110">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>


<span data-ttu-id="cc51e-111">Různé zásady je možné aplikovat na stejný asset.</span><span class="sxs-lookup"><span data-stu-id="cc51e-111">You could apply different policies to the same asset.</span></span> <span data-ttu-id="cc51e-112">Můžete například použít šifrování PlayReady na technologie Smooth Streaming a pomocí standardu AES Envelope šifrování a MPEG DASH, HLS.</span><span class="sxs-lookup"><span data-stu-id="cc51e-112">For example, you could apply PlayReady encryption to Smooth Streaming and AES Envelope encryption to MPEG DASH and HLS.</span></span> <span data-ttu-id="cc51e-113">Veškeré protokoly, které nejsou v zásadách doručení definovány (například když přidáte jedinou zásadu, která jako protokol určuje pouze HLS), budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="cc51e-113">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="cc51e-114">Výjimkou je, pokud nemáte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-114">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="cc51e-115">Pak budou všechny protokoly povolené v nešifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="cc51e-115">Then, all protocols will be allowed in the clear.</span></span>

<span data-ttu-id="cc51e-116">Pokud chcete doručovat šifrované asset úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-116">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="cc51e-117">Před asset Streamovat, server datových proudů odebere šifrování úložiště a datové proudy svůj obsah pomocí zadaného doručování zásad.</span><span class="sxs-lookup"><span data-stu-id="cc51e-117">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="cc51e-118">Například k poskytování asset šifrován Advanced Encryption (Standard AES) obálky šifrovací klíč, nastavte typ zásad na **DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="cc51e-118">For example, to deliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set the policy type to **DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="cc51e-119">Pokud chcete odebrat šifrování úložiště a Streamovat prostředek v nešifrované podobě, nastavte typ zásad na **NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="cc51e-119">To remove storage encryption and stream the asset in the clear, set the policy type to **NoDynamicEncryption**.</span></span> <span data-ttu-id="cc51e-120">Postupujte podle příklady, které ukazují, jak konfigurovat tyto typy zásad.</span><span class="sxs-lookup"><span data-stu-id="cc51e-120">Examples that show how to configure these policy types follow.</span></span>

<span data-ttu-id="cc51e-121">V závislosti na tom, jak nakonfigurovat zásady doručení assetu by nebudete moct dynamicky balíčku, dynamicky šifrovat a stream u následujících protokolů streamování: datové proudy technologie Smooth Streaming, HLS a MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="cc51e-121">Depending on how you configure the asset delivery policy you would be able to dynamically package, dynamically encrypt, and stream the following streaming protocols: Smooth Streaming, HLS, and MPEG DASH streams.</span></span>

<span data-ttu-id="cc51e-122">V následujícím seznamu jsou formáty, které umožňují stream Smooth, HLS a POMLČKY.</span><span class="sxs-lookup"><span data-stu-id="cc51e-122">The following list shows the formats that you use to stream Smooth, HLS, and DASH.</span></span>

<span data-ttu-id="cc51e-123">Technologie Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="cc51e-123">Smooth Streaming:</span></span>

<span data-ttu-id="cc51e-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="cc51e-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="cc51e-125">HLS:</span><span class="sxs-lookup"><span data-stu-id="cc51e-125">HLS:</span></span>

<span data-ttu-id="cc51e-126">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="cc51e-126">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="cc51e-127">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="cc51e-127">MPEG DASH</span></span>

<span data-ttu-id="cc51e-128">{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="cc51e-128">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


## <a name="considerations"></a><span data-ttu-id="cc51e-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc51e-129">Considerations</span></span>
* <span data-ttu-id="cc51e-130">Nelze odstranit AssetDeliveryPolicy přidružený prostředek při Lokátor OnDemand (streaming) existuje pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="cc51e-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="cc51e-131">Doporučuje se před odstraněním zásady odeberte zásady z prostředku.</span><span class="sxs-lookup"><span data-stu-id="cc51e-131">The recommendation is to remove the policy from the asset before deleting the policy.</span></span>
* <span data-ttu-id="cc51e-132">Šifrované majetku úložiště nelze vytvořit lokátor streamování, nastavena žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="cc51e-133">Není-li Asset šifrování úložiště, systém vám umožní vytvořit Lokátor a Streamovat prostředek v nešifrované podobě bez zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-133">If the Asset isn’t storage encrypted, the system will let you create a locator and stream the asset in the clear without an asset delivery policy.</span></span>
* <span data-ttu-id="cc51e-134">Můžete mít více zásady doručení mediálního přidružené jednoho datového zdroje, ale můžete určit pouze jeden způsob, jak zpracovávat dané AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="cc51e-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way to handle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="cc51e-135">Znamená, pokud se pokusíte propojit dvě zásady doručení, které zadat AssetDeliveryProtocol.SmoothStreaming protokol, který bude výsledkem chyba, protože systém nebude vědět, který jeden se má použít, když klient odešle požadavek technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="cc51e-135">Meaning if you try to link two delivery policies that specify the AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because the system does not know which one you want it to apply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="cc51e-136">Pokud máte prostředek s stávající Lokátor streamování, nemůže propojit nové zásady pro daný prostředek (můžete odpojit existující zásady z prostředku, nebo aktualizujete zásady pro doručení přidružený asset).</span><span class="sxs-lookup"><span data-stu-id="cc51e-136">If you have an asset with an existing streaming locator, you cannot link a new policy to the asset (you can either unlink an existing policy from the asset, or update a delivery policy associated with the asset).</span></span>  <span data-ttu-id="cc51e-137">Nejdřív musíte odstraňte Lokátor streamování, upravit zásady a potom je znovu vytvořit lokátor streamování.</span><span class="sxs-lookup"><span data-stu-id="cc51e-137">You first have to remove the streaming locator, adjust the policies, and then re-create the streaming locator.</span></span>  <span data-ttu-id="cc51e-138">Stejné locatorId můžete použít, když je znovu vytvořit lokátor streamování, ale měli byste zajistit, že vzhledem k tomu, že do mezipaměti obsah počátek nebo podřízené CDN, který nebude způsobovat problémy pro klienty.</span><span class="sxs-lookup"><span data-stu-id="cc51e-138">You can use the same locatorId when you recreate the streaming locator but you should ensure that won’t cause issues for clients since content can be cached by the origin or a downstream CDN.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="cc51e-139">Zásady doručení assetu vymazat</span><span class="sxs-lookup"><span data-stu-id="cc51e-139">Clear asset delivery policy</span></span>

<span data-ttu-id="cc51e-140">Následující **ConfigureClearAssetDeliveryPolicy** metoda určuje nelze použít dynamické šifrování a poskytovat datový proud v některém z těchto protokolů: protokoly MPEG DASH, HLS nebo technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="cc51e-140">The following **ConfigureClearAssetDeliveryPolicy** method specifies to not apply dynamic encryption and to deliver the stream in any of the following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> <span data-ttu-id="cc51e-141">Můžete chtít tuto zásadu použít pro vaše prostředky šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="cc51e-141">You might want to apply this policy to your storage encrypted assets.</span></span>

<span data-ttu-id="cc51e-142">Informace na hodnotách, které můžete zadat při vytváření AssetDeliveryPolicy najdete v tématu [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="cc51e-142">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="cc51e-143">Zásady doručení assetu DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="cc51e-143">DynamicCommonEncryption asset delivery policy</span></span>

<span data-ttu-id="cc51e-144">Následující **CreateAssetDeliveryPolicy** metoda vytvoří **AssetDeliveryPolicy** nakonfigurovaný pro použití běžného dynamického šifrování (**DynamicCommonEncryption**) smooth streamování protokolu (jiné protokoly, bude zablokován streamování).</span><span class="sxs-lookup"><span data-stu-id="cc51e-144">The following **CreateAssetDeliveryPolicy** method creates the **AssetDeliveryPolicy** that is configured to apply dynamic common encryption (**DynamicCommonEncryption**) to a smooth streaming protocol (other protocols will be blocked from streaming).</span></span> <span data-ttu-id="cc51e-145">Metoda přebírá dva parametry: **Asset** (asset, do které chcete použít zásady doručení) a **IContentKey** (klíč obsahu **CommonEncryption** typu, pro Další informace najdete v tématu: [vytváření klíč obsahu](media-services-dotnet-create-contentkey.md#common_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="cc51e-145">The method takes two parameters : **Asset** (the asset to which you want to apply the delivery policy) and **IContentKey** (the content key of the **CommonEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#common_contentkey)).</span></span>

<span data-ttu-id="cc51e-146">Informace na hodnotách, které můžete zadat při vytváření AssetDeliveryPolicy najdete v tématu [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="cc51e-146">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

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
    
            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }

<span data-ttu-id="cc51e-147">Azure Media Services můžete také přidat Widevine šifrování.</span><span class="sxs-lookup"><span data-stu-id="cc51e-147">Azure Media Services also enables you to add Widevine encryption.</span></span> <span data-ttu-id="cc51e-148">Následující příklad ukazuje technologie PlayReady a Widevine, který se přidává do zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-148">The following example demonstrates both PlayReady and Widevine being added to the asset delivery policy.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

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


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

> [!NOTE]
> <span data-ttu-id="cc51e-149">Při šifrování s technologií Widevine, by pouze možné doručíte pomocí čárka.</span><span class="sxs-lookup"><span data-stu-id="cc51e-149">When encrypting with Widevine, you would only be able to deliver using DASH.</span></span> <span data-ttu-id="cc51e-150">Nezapomeňte zadat DASH v doručovací protokol assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-150">Make sure to specify DASH in the asset delivery protocol.</span></span>
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="cc51e-151">Zásady doručení assetu DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="cc51e-151">DynamicEnvelopeEncryption asset delivery policy</span></span>
<span data-ttu-id="cc51e-152">Následující **CreateAssetDeliveryPolicy** metoda vytvoří **AssetDeliveryPolicy** nakonfigurovaný pro použití dynamické obálky šifrování (**DynamicEnvelopeEncryption**) pro protokoly technologie Smooth Streaming, HLS a DASH (Pokud se rozhodnete není uveden některé protokoly, že budou Blokovaní z streamování).</span><span class="sxs-lookup"><span data-stu-id="cc51e-152">The following **CreateAssetDeliveryPolicy** method creates the **AssetDeliveryPolicy** that is configured to apply dynamic envelope encryption (**DynamicEnvelopeEncryption**) to Smooth Streaming, HLS, and DASH protocols (if you decide to not specify some protocols, they will be blocked from streaming).</span></span> <span data-ttu-id="cc51e-153">Metoda přebírá dva parametry: **Asset** (asset, do které chcete použít zásady doručení) a **IContentKey** (klíč obsahu **EnvelopeEncryption** typu Další informace najdete v tématu: [vytváření klíč obsahu](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="cc51e-153">The method takes two parameters : **Asset** (the asset to which you want to apply the delivery policy) and **IContentKey** (the content key of the **EnvelopeEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span></span>

<span data-ttu-id="cc51e-154">Informace na hodnotách, které můžete zadat při vytváření AssetDeliveryPolicy najdete v tématu [typy používané při definování AssetDeliveryPolicy](#types) části.</span><span class="sxs-lookup"><span data-stu-id="cc51e-154">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
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

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


## <span data-ttu-id="cc51e-155"><a id="types"></a>Typy používané při definování AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="cc51e-155"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <span data-ttu-id="cc51e-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="cc51e-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span></span>

<span data-ttu-id="cc51e-157">Následující výčet popisuje hodnoty, které lze nastavit pro doručovací protokol assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-157">The following enum describes values you can set for the asset delivery protocol.</span></span>

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

### <span data-ttu-id="cc51e-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="cc51e-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span></span>

<span data-ttu-id="cc51e-159">Následující výčet popisuje hodnoty, které můžete zadat pro typ zásad doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-159">The following enum describes values you can set for the asset delivery policy type.</span></span>  

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

### <span data-ttu-id="cc51e-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="cc51e-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>

<span data-ttu-id="cc51e-161">Následující výčet popisuje hodnoty, které můžete použít ke konfiguraci metodu doručení obsahu klíče, který se klient.</span><span class="sxs-lookup"><span data-stu-id="cc51e-161">The following enum describes values you can use to configure the delivery method of the content key to the client.</span></span>
    
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

### <span data-ttu-id="cc51e-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="cc51e-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="cc51e-163">Následující výčet popisuje hodnoty, které můžete nastavit, aby konfigurace klíče, které slouží k získání konkrétní konfigurace pro zásady pro doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cc51e-163">The following enum describes values you can set to configure keys used to get specific configuration for an asset delivery policy.</span></span>

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="cc51e-164">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="cc51e-164">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="cc51e-165">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="cc51e-165">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

