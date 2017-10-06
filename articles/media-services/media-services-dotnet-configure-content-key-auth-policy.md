---
title: "zásady autorizace klíče obsahu aaaConfigure pomocí sady Media Services .NET SDK | Microsoft Docs"
description: "Zjistěte, jak tooconfigure zásad autorizace pro klíč obsahu pomocí sady Media Services .NET SDK."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: cfcbc5da9819bcec8b163fef183988a8beff9ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a><span data-ttu-id="58818-103">Dynamické šifrování: Nakonfigurujte zásady autorizace klíče obsahu</span><span class="sxs-lookup"><span data-stu-id="58818-103">Dynamic encryption: configure content key authorization policy</span></span>
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a><span data-ttu-id="58818-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="58818-104">Overview</span></span>
<span data-ttu-id="58818-105">Microsoft Azure Media Services umožňuje toodeliver MPEG-DASH, technologie Smooth Streaming a datové proudy HTTP-Live-Streaming (HLS) chráněné pomocí Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="58818-105">Microsoft Azure Media Services enables you toodeliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with Advanced Encryption Standard (AES) (using 128-bit encryption keys) or [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="58818-106">AMS taky umožňuje vám toodeliver datové proudy DASH šifrované pomocí Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="58818-106">AMS also enables you toodeliver DASH streams encrypted with Widevine DRM.</span></span> <span data-ttu-id="58818-107">Technologie PlayReady a Widevine jsou šifrované podle specifikace Common Encryption (ISO/IEC CENC 23001-7) hello.</span><span class="sxs-lookup"><span data-stu-id="58818-107">Both PlayReady and Widevine are encrypted per hello Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span>

<span data-ttu-id="58818-108">Služba Media Services také poskytuje **klíč nebo licenční služby doručení** ze kterých klienti můžete získat klíče AES nebo tooplay licence PlayReady nebo Widevine hello šifrovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="58818-108">Media Services also provides a **Key/License Delivery Service** from which clients can obtain AES keys or PlayReady/Widevine licenses tooplay hello encrypted content.</span></span>

<span data-ttu-id="58818-109">Pokud chcete pro Media Services tooencrypt prostředek, je nutné tooassociate šifrovací klíč (**CommonEncryption** nebo **EnvelopeEncryption**) s hello asset (jak je popsáno [sem](media-services-dotnet-create-contentkey.md)) a také nakonfigurovat zásady autorizace pro klíč hello (jak je popsáno v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="58818-109">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key (**CommonEncryption** or **EnvelopeEncryption**) with hello asset (as described [here](media-services-dotnet-create-contentkey.md)) and also configure authorization policies for hello key (as described in this article).</span></span>

<span data-ttu-id="58818-110">Když přehrávač vyžádá datový proud, služba Media Services použije hello zadané klíče toodynamically šifrování svůj obsah pomocí šifrování AES nebo DRM.</span><span class="sxs-lookup"><span data-stu-id="58818-110">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES or DRM encryption.</span></span> <span data-ttu-id="58818-111">datový proud hello toodecrypt, hello player bude požadovat hello klíč z hello doručení klíče služby.</span><span class="sxs-lookup"><span data-stu-id="58818-111">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="58818-112">toodecide, jestli je uživatel hello autorizovaný tooget hello klíč, hello služba vyhodnocuje hello zásady autorizace, které jste zadali pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="58818-112">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="58818-113">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="58818-113">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="58818-114">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.</span><span class="sxs-lookup"><span data-stu-id="58818-114">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span> <span data-ttu-id="58818-115">zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS).</span><span class="sxs-lookup"><span data-stu-id="58818-115">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="58818-116">Služba Media Services podporuje tokeny ve hello **jednoduchých webových tokenů** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátu a **webových tokenů JSON** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) formátu.</span><span class="sxs-lookup"><span data-stu-id="58818-116">Media Services supports tokens in hello **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format and **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span></span>

<span data-ttu-id="58818-117">Služba Media Services neposkytuje zabezpečení tokenu služby.</span><span class="sxs-lookup"><span data-stu-id="58818-117">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="58818-118">Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny.</span><span class="sxs-lookup"><span data-stu-id="58818-118">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="58818-119">Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a deklarace identity vystavují (jak je popsáno v tomto článku) zadaný v konfiguraci omezení s tokenem hello.</span><span class="sxs-lookup"><span data-stu-id="58818-119">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration (as described in this article).</span></span> <span data-ttu-id="58818-120">Hello doručení klíče služby Media Services vrátí hello šifrovací klíče toohello klienta Pokud hello token je platný a hello deklarace v tokenu hello odpovídají nakonfigurovaným pro klíč obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="58818-120">hello Media Services key delivery service will return hello encryption key toohello client if hello token is valid and hello claims in hello token match those configured for hello content key.</span></span>

<span data-ttu-id="58818-121">Další informace najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="58818-121">For more information, see</span></span>

[<span data-ttu-id="58818-122">Ověření pomocí tokenu JWT</span><span class="sxs-lookup"><span data-stu-id="58818-122">JWT token authentication</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="58818-123">[Integrace aplikace Azure Media Services OWIN MVC se službou Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="58818-123">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="58818-124">[Použití Azure ACS tooissue tokeny](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="58818-124">[Use Azure ACS tooissue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

### <a name="some-considerations-apply"></a><span data-ttu-id="58818-125">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="58818-125">Some considerations apply:</span></span>
* <span data-ttu-id="58818-126">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="58818-126">When your AMS account is created a **default** streaming endpoint is added  tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="58818-127">toostart streamování vašeho obsahu a proveďte výhod dynamického balení a dynamické šifrování, koncový bod streamování má toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="58818-127">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, your streaming endpoint has toobe in hello **Running** state.</span></span> 
* <span data-ttu-id="58818-128">Váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="58818-128">Your asset must contain a set of adaptive bitrate MP4s or  adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="58818-129">Další informace najdete v tématu [kódovat asset](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="58818-129">For more information, see [Encode an asset](media-services-encode-asset.md).</span></span>
* <span data-ttu-id="58818-130">Nahrání a kódování vaše prostředky pomocí **AssetCreationOptions.StorageEncrypted** možnost.</span><span class="sxs-lookup"><span data-stu-id="58818-130">Upload and encode your assets using **AssetCreationOptions.StorageEncrypted** option.</span></span>
* <span data-ttu-id="58818-131">Pokud máte v plánu toohave více obsahu klíčů, které vyžadují hello stejné konfigurace zásad, se důrazně doporučuje toocreate zásady jeden autorizace a opakovaně ji používat s více klíčů k obsahu.</span><span class="sxs-lookup"><span data-stu-id="58818-131">If you plan toohave multiple content keys that require hello same policy configuration, it is strongly recommended toocreate a single authorization policy and reuse it with multiple content keys.</span></span>
* <span data-ttu-id="58818-132">Hello služba pro přenos klíče ukládá do mezipaměti ContentKeyAuthorizationPolicy a související objekty (Možnosti zásad a omezení) pro 15 minut.</span><span class="sxs-lookup"><span data-stu-id="58818-132">hello Key Delivery service caches ContentKeyAuthorizationPolicy and its related objects (policy options and restrictions) for 15 minutes.</span></span>  <span data-ttu-id="58818-133">Pokud vytvoříte ContentKeyAuthorizationPolicy a zadejte toouse "Token" omezení, pak otestovat ji a potom aktualizovat zásady hello příliš "Otevřít" omezení, bude trvat přibližně 15 minut před hello zásad přepínače toohello "Otevřené" verze hello zásad.</span><span class="sxs-lookup"><span data-stu-id="58818-133">If you create a ContentKeyAuthorizationPolicy and specify toouse a “Token” restriction, then test it, and then update hello policy too“Open” restriction, it will take roughly 15 minutes before hello policy switches toohello “Open” version of hello policy.</span></span>
* <span data-ttu-id="58818-134">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="58818-134">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
* <span data-ttu-id="58818-135">V současné době nelze zašifrovat progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="58818-135">Currently, you cannot encrypt progressive downloads.</span></span>

## <a name="aes-128-dynamic-encryption"></a><span data-ttu-id="58818-136">Dynamické šifrování AES-128</span><span class="sxs-lookup"><span data-stu-id="58818-136">AES-128 Dynamic Encryption</span></span>
### <a name="open-restriction"></a><span data-ttu-id="58818-137">Otevřete omezení</span><span class="sxs-lookup"><span data-stu-id="58818-137">Open Restriction</span></span>
<span data-ttu-id="58818-138">Otevřete omezení znamená, že systém hello dodá hello tooanyone klíče, který vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="58818-138">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="58818-139">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="58818-139">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="58818-140">Hello následující ukázka vytvoří zásadu otevřete autorizace a přidá ho toohello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="58818-140">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions
        // and create authorization policy
        IContentKeyAuthorizationPolicy policy = _context.
        ContentKeyAuthorizationPolicies.
        CreateAsync("Open Authorization Policy").Result;
        
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };

        restrictions.Add(restriction);

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a><span data-ttu-id="58818-141">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="58818-141">Token Restriction</span></span>
<span data-ttu-id="58818-142">Tato část popisuje, jak toocreate obsahu klíče zásad autorizace a přidružte ji k hello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="58818-142">This section describes how toocreate a content key authorization policy and associate it with hello content key.</span></span> <span data-ttu-id="58818-143">zásady autorizace Hello popisuje, jaké požadavky na ověřování musí být nesplnění toodetermine, pokud je uživatel hello autorizovaný tooreceive hello klíč (například seznamu "ověřovací klíč" hello obsahovat hello klíč byl podepsaný token tuto hello).</span><span class="sxs-lookup"><span data-stu-id="58818-143">hello authorization policy describes what authorization requirements must be met toodetermine if hello user is authorized tooreceive hello key (for example, does hello “verification key” list contain hello key that hello token was signed with).</span></span>

<span data-ttu-id="58818-144">možnost omezení s tokenem hello tooconfigure, je nutné toouse XML tokenu hello toodescribe autorizace požadavky.</span><span class="sxs-lookup"><span data-stu-id="58818-144">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="58818-145">Konfigurace omezení s tokenem Hello XML musí odpovídat toohello následující schématu XML.</span><span class="sxs-lookup"><span data-stu-id="58818-145">hello token restriction configuration XML must conform toohello following XML schema.</span></span>

#### <span data-ttu-id="58818-146"><a id="schema"></a>Omezení s tokenem schématu</span><span class="sxs-lookup"><span data-stu-id="58818-146"><a id="schema"></a>Token restriction schema</span></span>
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

<span data-ttu-id="58818-147">Při konfiguraci hello **tokenu** omezený zásady, je nutné zadat hello primární ** ověřovací klíč **, **vystavitele** a **cílovou skupinu** parametry.</span><span class="sxs-lookup"><span data-stu-id="58818-147">When configuring hello **token** restricted policy, you must specify hello primary** verification key**, **issuer** and **audience** parameters.</span></span> <span data-ttu-id="58818-148">Hello ** primární ověření klíč ** obsahuje hello klíč, který hello token byl podepsán, **vystavitele** služby tokenů zabezpečení hello je tento token hello problémy.</span><span class="sxs-lookup"><span data-stu-id="58818-148">hello **primary verification key **contains hello key that hello token was signed with, **issuer** is hello secure token service that issues hello token.</span></span> <span data-ttu-id="58818-149">Hello **cílovou skupinu** (někdy nazývané **oboru**) popisuje hello záměr hello tokenu nebo hello prostředků hello token povolí přístup k.</span><span class="sxs-lookup"><span data-stu-id="58818-149">hello **audience** (sometimes called **scope**) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="58818-150">Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="58818-150">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span> 

<span data-ttu-id="58818-151">Při použití **sady Media Services SDK pro .NET**, můžete použít hello **TokenRestrictionTemplate** třída toogenerate hello omezení token.</span><span class="sxs-lookup"><span data-stu-id="58818-151">When using **Media Services SDK for .NET**, you can use hello **TokenRestrictionTemplate** class toogenerate hello restriction token.</span></span>
<span data-ttu-id="58818-152">Hello následující ukázka vytvoří token omezení zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="58818-152">hello following example creates an authorization policy with a token restriction.</span></span> <span data-ttu-id="58818-153">V tomto příkladu hello by klient musel předem toopresent token, který obsahuje: podpisový klíč (VerificationKey), vydavatel tokenu a požadované deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="58818-153">In this example, hello client would have toopresent a token that contains: signing key (VerificationKey), a token issuer, and required claims.</span></span>

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };

        restrictions.Add(restriction);

        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

#### <span data-ttu-id="58818-154"><a id="test"></a>Testovací token</span><span class="sxs-lookup"><span data-stu-id="58818-154"><a id="test"></a>Test token</span></span>
<span data-ttu-id="58818-155">tooget testovací token podle hello tokenu omezení, která byla použita pro zásad autorizace pro klíč hello, hello následující.</span><span class="sxs-lookup"><span data-stu-id="58818-155">tooget a test token based on hello token restriction that was used for hello key authorization policy, do hello following.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
    // Note, you need toopass hello key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a><span data-ttu-id="58818-156">Šifrování PlayReady dynamický</span><span class="sxs-lookup"><span data-stu-id="58818-156">PlayReady Dynamic Encryption</span></span>
<span data-ttu-id="58818-157">Služba Media Services umožňuje tooconfigure hello práva a omezení, že chcete pro hello tooenforce runtime PlayReady DRM při uživatele, že tooplay zpět chráněného obsahu.</span><span class="sxs-lookup"><span data-stu-id="58818-157">Media Services enables you tooconfigure hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplay back protected content.</span></span> 

<span data-ttu-id="58818-158">Při ochraně obsahu pomocí technologie PlayReady, jednou z věcí hello potřebujete toospecify ve vaší zásady autorizace je řetězec XML, který definuje hello [šablona licence PlayReady](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="58818-158">When protecting your content with PlayReady, one of hello things you need toospecify in your authorization policy is an XML string that defines hello [PlayReady license template](media-services-playready-license-template-overview.md).</span></span> <span data-ttu-id="58818-159">V sady Media Services SDK pro .NET, hello **PlayReadyLicenseResponseTemplate** a **PlayReadyLicenseTemplate** třídy vám pomůže definovat hello šablona licence PlayReady.</span><span class="sxs-lookup"><span data-stu-id="58818-159">In Media Services SDK for .NET, hello **PlayReadyLicenseResponseTemplate** and **PlayReadyLicenseTemplate** classes will help you define hello PlayReady License Template.</span></span>

<span data-ttu-id="58818-160">[Toto téma](media-services-protect-with-drm.md) ukazuje, jak tooencrypt svůj obsah pomocí **PlayReady** a **Widevine**.</span><span class="sxs-lookup"><span data-stu-id="58818-160">[This topic](media-services-protect-with-drm.md) shows how tooencrypt your content with **PlayReady** and **Widevine**.</span></span>

### <a name="open-restriction"></a><span data-ttu-id="58818-161">Otevřete omezení</span><span class="sxs-lookup"><span data-stu-id="58818-161">Open Restriction</span></span>
<span data-ttu-id="58818-162">Otevřete omezení znamená, že systém hello dodá hello tooanyone klíče, který vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="58818-162">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="58818-163">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="58818-163">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="58818-164">Hello následující ukázka vytvoří zásadu otevřete autorizace a přidá ho toohello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="58818-164">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {

        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;


        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate hello content key authorization policy with hello content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a><span data-ttu-id="58818-165">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="58818-165">Token Restriction</span></span>
<span data-ttu-id="58818-166">možnost omezení s tokenem hello tooconfigure, je nutné toouse XML tokenu hello toodescribe autorizace požadavky.</span><span class="sxs-lookup"><span data-stu-id="58818-166">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="58818-167">Konfigurace omezení s tokenem Hello XML musí odpovídat schématu XML toohello ukazuje [to](#schema) části.</span><span class="sxs-lookup"><span data-stu-id="58818-167">hello token restriction configuration XML must conform toohello XML schema shown in [this](#schema) section.</span></span>

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate hello content key authorization policy with hello content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {

        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();


        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 

    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // hello following code configures PlayReady License Template using .NET classes
        // and returns hello XML string.

        //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user. 
        //It contains a field for a custom data string between hello license server and hello application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // toobe returned toohello end users. 
        //It contains hello data on hello content key in hello license and any rights or restrictions toobe 
        //enforced by hello PlayReady DRM runtime when using hello content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether hello license is persistent (saved in persistent storage on hello client) 
        //or non-persistent (only held in memory while hello player is using hello license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use hello license or not.  
        // If true, hello MinimumSecurityLevel property of hello license
        // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class. 
        // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions 
        // configured in hello license and on hello PlayRight itself (for playback specific policy). 
        // Much of hello policy on hello PlayRight has toodo with output restrictions 
        // which control hello types of outputs that hello content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if hello DigitalVideoOnlyContentRestriction is enabled, 
        //then hello DRM runtime will only allow hello video toobe displayed over digital outputs 
        //(analog video outputs won’t be allowed toopass hello content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience. 
        // If hello output protections are configured too restrictive, 
        // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


<span data-ttu-id="58818-168">tooget testovací token podle hello tokenu omezení, která byla použita hello autorizace pro klíč zásad najdete v tématu [to](#test) části.</span><span class="sxs-lookup"><span data-stu-id="58818-168">tooget a test token based on hello token restriction that was used for hello key authorization policy see [this](#test) section.</span></span> 

## <span data-ttu-id="58818-169"><a id="types"></a>Typy používané při definování ContentKeyAuthorizationPolicy</span><span class="sxs-lookup"><span data-stu-id="58818-169"><a id="types"></a>Types used when defining ContentKeyAuthorizationPolicy</span></span>
### <span data-ttu-id="58818-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span><span class="sxs-lookup"><span data-stu-id="58818-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span></span>
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <span data-ttu-id="58818-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="58818-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <span data-ttu-id="58818-172"><a id="TokenType"></a>TokenType</span><span class="sxs-lookup"><span data-stu-id="58818-172"><a id="TokenType"></a>TokenType</span></span>
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="58818-173">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="58818-173">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="58818-174">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="58818-174">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="58818-175">Další krok</span><span class="sxs-lookup"><span data-stu-id="58818-175">Next step</span></span>
<span data-ttu-id="58818-176">Teď, když jste nakonfigurovali zásady autorizace klíče obsahu, přejděte toohello [jak zásady doručení assetu tooconfigure](media-services-dotnet-configure-asset-delivery-policy.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="58818-176">Now that you have configured content key's authorization policy, go toohello [How tooconfigure asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md) topic.</span></span>

