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
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>Nakonfigurujte zásady doručení assetu pomocí .NET SDK
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Přehled
Pokud máte v plánu toodelivery šifrované prostředky, jeden z hello kroků v hello Media Services obsahu doručení pracovního postupu je konfigurace zásad doručení pro prostředky. zásady doručení assetu Hello informuje Media Services, jak chcete použít pro váš asset toobe doručit: do streamování protokol, který by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete toodynamically váš asset šifrovat a jak (obálky nebo common encryption).

Toto téma popisuje, proč a jak toocreate a nakonfigurujte zásady doručení assetu.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 
>
>Navíc toobe možné toouse dynamické balením a dynamickým šifrováním váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.


Je možné aplikovat různé zásady toohello stejné asset. Například může použít PlayReady šifrování tooSmooth Streaming a pomocí standardu AES Envelope šifrování tooMPEG DASH a HLS. Veškeré protokoly, které nejsou v zásadách doručení definovány (například přidáte jedinou zásadu, která jako hello protokol určuje pouze HLS) budou při streamování blokovány. Výjimka toothis Hello je, pokud máte definovány vůbec žádné zásady doručení assetu. Potom bude možné v hello zrušte všechny protokoly.

Pokud chcete toodeliver asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello. Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení. Například toodeliver asset šifrován Advanced Encryption (Standard AES) obálky šifrovací klíč, nastavte typ zásad hello příliš**DynamicEnvelopeEncryption**. šifrování úložiště tooremove a asset hello datový proud v hello jasné, nastavte typ zásad hello příliš**NoDynamicEncryption**. Příklady, které ukazují, jak tooconfigure tyto typy zásad podle.

V závislosti na tom, jak nakonfigurovat zásady doručení assetu hello je bude možné toodynamically balíčku, dynamicky šifrovat a stream hello následujících protokolů datových proudů: datové proudy technologie Smooth Streaming, HLS a MPEG DASH.

Hello následujícím seznamu jsou formáty hello použijete toostream Smooth, HLS a POMLČKY.

Technologie Smooth Streaming:

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


## <a name="considerations"></a>Požadavky
* Nelze odstranit AssetDeliveryPolicy přidružený prostředek při Lokátor OnDemand (streaming) existuje pro tento prostředek. Hello doporučení je zásada hello tooremove z hello asset před odstraněním hello zásad.
* Šifrované majetku úložiště nelze vytvořit lokátor streamování, nastavena žádné zásady doručení assetu.  Není-li hello Asset šifrování úložiště, hello systému vám umožní vytvoření prostředku hello Lokátor a datový proud v hello zrušte bez zásady pro doručení assetu.
* Může mít několik zásady doručení mediálního přidružené jednoho datového zdroje, ale můžete zadat jenom jeden ze způsobů toohandle daného AssetDeliveryProtocol.  Znamená, pokud se pokusíte toolink dvě zásady doručení určující hello AssetDeliveryProtocol.SmoothStreaming protokol, který bude výsledkem chyba, protože systém hello nebude vědět, které z nich chcete tooapply když klient zadává žádost technologie Smooth Streaming.
* Pokud máte prostředek s stávající Lokátor streamování, nelze propojit nový prostředek zásad toohello (můžete odpojit existující zásady z hello asset, nebo aktualizujete zásady pro doručení přidružené hello asset).  Můžete nejprve mít Lokátor streamování hello tooremove, upravte hello zásady a potom je znovu vytvořit lokátor streamování hello.  Můžete použít hello, které by měly stejné locatorId při vytvoření hello streamování Lokátor ale můžete zajistit, který nezpůsobí problémy pro klienty, protože do mezipaměti obsah hello původ nebo příjem dat CDN.

## <a name="clear-asset-delivery-policy"></a>Zásady doručení assetu vymazat

Následující Hello **ConfigureClearAssetDeliveryPolicy** metoda určuje toonot použít dynamické šifrování a protokoly datového proudu hello toodeliver v některém z následujících hello: protokoly MPEG DASH, HLS nebo technologie Smooth Streaming. Můžete chtít tooapply této zásady tooyour šifrování úložiště prostředky.

Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Zásady doručení assetu DynamicCommonEncryption

Následující Hello **CreateAssetDeliveryPolicy** metoda vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply běžného dynamického šifrování (**DynamicCommonEncryption**) tooa technologie smooth streaming protocol (jiné protokoly, bude zablokován streamování). Hello metoda přebírá dva parametry: **Asset** (hello asset toowhich chcete zásady doručení hello tooapply) a **IContentKey** (hello obsahu klíč hello **CommonEncryption**typu, další informace najdete v tématu: [vytváření klíč obsahu](media-services-dotnet-create-contentkey.md#common_contentkey)).

Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.

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

Azure Media Services také umožňuje tooadd Widevine šifrování. Hello následující příklad ukazuje technologie PlayReady a Widevine přidávané zásady doručení assetu toohello.

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
> Při šifrování s technologií Widevine, by být pouze možnost toodeliver pomocí čárka. Ujistěte se, že toospecify DASH v doručovací protokol assetu hello.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Zásady doručení assetu DynamicEnvelopeEncryption
Následující Hello **CreateAssetDeliveryPolicy** metoda vytvoří hello **AssetDeliveryPolicy** který je nakonfigurovaný tooapply dynamické obálky šifrování (**DynamicEnvelopeEncryption** ) tooSmooth protokoly Streaming, HLS a DASH (Pokud se rozhodnete toonot zadejte některé protokoly, že budou Blokovaní z streamování). Hello metoda přebírá dva parametry: **Asset** (hello asset toowhich chcete zásady doručení hello tooapply) a **IContentKey** (hello obsahu klíč hello **EnvelopeEncryption**typu, další informace najdete v tématu: [vytváření klíč obsahu](media-services-dotnet-create-contentkey.md#envelope_contentkey)).

Informace o co hodnoty, které můžete zadat při vytváření AssetDeliveryPolicy, najdete v části hello [typy používané při definování AssetDeliveryPolicy](#types) části.   

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


## <a id="types"></a>Typy používané při definování AssetDeliveryPolicy

### <a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol

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

### <a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

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

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

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

### <a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

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

