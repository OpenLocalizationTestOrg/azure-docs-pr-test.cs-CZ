---
title: "Konfigurace zásad autorizace klíče obsahu pomocí sady Media Services .NET SDK | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat zásady autorizace pro klíč obsahu pomocí sady Media Services .NET SDK."
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
ms.openlocfilehash: 75dd9107dca215a0b31db3d44bada69210fe9ac6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a><span data-ttu-id="58f9a-103">Dynamické šifrování: Nakonfigurujte zásady autorizace klíče obsahu</span><span class="sxs-lookup"><span data-stu-id="58f9a-103">Dynamic encryption: configure content key authorization policy</span></span>
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a><span data-ttu-id="58f9a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="58f9a-104">Overview</span></span>
<span data-ttu-id="58f9a-105">Microsoft Azure Media Services umožňuje doručovat datové proudy MPEG-DASH, technologie Smooth Streaming a HTTP-Live-Streaming (HLS) chráněné pomocí Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="58f9a-105">Microsoft Azure Media Services enables you to deliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with Advanced Encryption Standard (AES) (using 128-bit encryption keys) or [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="58f9a-106">AMS také umožňuje doručovat datové proudy DASH šifrované pomocí Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="58f9a-106">AMS also enables you to deliver DASH streams encrypted with Widevine DRM.</span></span> <span data-ttu-id="58f9a-107">Technologie PlayReady a Widevine jsou šifrované podle specifikace Common Encryption (ISO/IEC CENC 23001-7).</span><span class="sxs-lookup"><span data-stu-id="58f9a-107">Both PlayReady and Widevine are encrypted per the Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span>

<span data-ttu-id="58f9a-108">Služba Media Services také poskytuje **klíč nebo licenční služby doručení** klienty, kteří mohou získat klíče AES nebo licence PlayReady nebo Widevine přehrávání šifrovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="58f9a-108">Media Services also provides a **Key/License Delivery Service** from which clients can obtain AES keys or PlayReady/Widevine licenses to play the encrypted content.</span></span>

<span data-ttu-id="58f9a-109">Pokud chcete pro Media Services k šifrování prostředek, je potřeba přidružit šifrovací klíč (**CommonEncryption** nebo **EnvelopeEncryption**) k assetu (jak je popsáno [sem](media-services-dotnet-create-contentkey.md)) a taky nakonfigurovat zásady autorizace pro klíč (jak je popsáno v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="58f9a-109">If you want for Media Services to encrypt an asset, you need to associate an encryption key (**CommonEncryption** or **EnvelopeEncryption**) with the asset (as described [here](media-services-dotnet-create-contentkey.md)) and also configure authorization policies for the key (as described in this article).</span></span>

<span data-ttu-id="58f9a-110">Datový proud je žádost přehrávač, Media Services používá k zadanému klíči dynamicky šifrovat obsah s šifrováním AES nebo DRM.</span><span class="sxs-lookup"><span data-stu-id="58f9a-110">When a stream is requested by a player, Media Services uses the specified key to dynamically encrypt your content using AES or DRM encryption.</span></span> <span data-ttu-id="58f9a-111">K dešifrování datového proudu, bude přehrávač požadovat klíč ze služby doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="58f9a-111">To decrypt the stream, the player will request the key from the key delivery service.</span></span> <span data-ttu-id="58f9a-112">Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.</span><span class="sxs-lookup"><span data-stu-id="58f9a-112">To decide whether or not the user is authorized to get the key, the service evaluates the authorization policies that you specified for the key.</span></span>

<span data-ttu-id="58f9a-113">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="58f9a-113">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="58f9a-114">Zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.</span><span class="sxs-lookup"><span data-stu-id="58f9a-114">The content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span> <span data-ttu-id="58f9a-115">Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS).</span><span class="sxs-lookup"><span data-stu-id="58f9a-115">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="58f9a-116">Služba Media Services podporuje tokeny ve **jednoduchých webových tokenů** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátu a **webových tokenů JSON** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) formátu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-116">Media Services supports tokens in the **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format and **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span></span>

<span data-ttu-id="58f9a-117">Služba Media Services neposkytuje zabezpečení tokenu služby.</span><span class="sxs-lookup"><span data-stu-id="58f9a-117">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="58f9a-118">Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS problém tokeny.</span><span class="sxs-lookup"><span data-stu-id="58f9a-118">You can create a custom STS or leverage Microsoft Azure ACS to issue tokens.</span></span> <span data-ttu-id="58f9a-119">Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem (jak je popsáno v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="58f9a-119">The STS must be configured to create a token signed with the specified key and issue claims that you specified in the token restriction configuration (as described in this article).</span></span> <span data-ttu-id="58f9a-120">Doručení klíče služby Media Services bude vrácena klientovi šifrovací klíč, pokud token je platný a deklarace identity v tokenu shodují s těmi, nakonfigurované pro klíč k obsahu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-120">The Media Services key delivery service will return the encryption key to the client if the token is valid and the claims in the token match those configured for the content key.</span></span>

<span data-ttu-id="58f9a-121">Další informace najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="58f9a-121">For more information, see</span></span>

[<span data-ttu-id="58f9a-122">Ověření pomocí tokenu JWT</span><span class="sxs-lookup"><span data-stu-id="58f9a-122">JWT token authentication</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="58f9a-123">[Integrace aplikace Azure Media Services OWIN MVC se službou Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="58f9a-123">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="58f9a-124">[Pomocí Azure ACS problém tokeny](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="58f9a-124">[Use Azure ACS to issue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

### <a name="some-considerations-apply"></a><span data-ttu-id="58f9a-125">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="58f9a-125">Some considerations apply:</span></span>
* <span data-ttu-id="58f9a-126">Při vytvoření účtu AMS **výchozí** koncový bod streamování je přidána k vašemu účtu v **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-126">When your AMS account is created a **default** streaming endpoint is added  to your account in the **Stopped** state.</span></span> <span data-ttu-id="58f9a-127">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamické šifrování, musí být v koncovém bodu streamování **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-127">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, your streaming endpoint has to be in the **Running** state.</span></span> 
* <span data-ttu-id="58f9a-128">Váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="58f9a-128">Your asset must contain a set of adaptive bitrate MP4s or  adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="58f9a-129">Další informace najdete v tématu [kódovat asset](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="58f9a-129">For more information, see [Encode an asset](media-services-encode-asset.md).</span></span>
* <span data-ttu-id="58f9a-130">Nahrání a kódování vaše prostředky pomocí **AssetCreationOptions.StorageEncrypted** možnost.</span><span class="sxs-lookup"><span data-stu-id="58f9a-130">Upload and encode your assets using **AssetCreationOptions.StorageEncrypted** option.</span></span>
* <span data-ttu-id="58f9a-131">Pokud budete chtít mít několik klíčů obsahu, které vyžadují stejnou konfiguraci zásad, důrazně doporučujeme vytvořit zásadu jeden autorizace a opakovaně ji používat s více klíčů k obsahu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-131">If you plan to have multiple content keys that require the same policy configuration, it is strongly recommended to create a single authorization policy and reuse it with multiple content keys.</span></span>
* <span data-ttu-id="58f9a-132">Službu doručování klíč ukládá do mezipaměti ContentKeyAuthorizationPolicy a související objekty (Možnosti zásad a omezení) pro 15 minut.</span><span class="sxs-lookup"><span data-stu-id="58f9a-132">The Key Delivery service caches ContentKeyAuthorizationPolicy and its related objects (policy options and restrictions) for 15 minutes.</span></span>  <span data-ttu-id="58f9a-133">Pokud vytvoříte ContentKeyAuthorizationPolicy a nastavení, aby používal "Token" omezení, pak otestovat ji a potom aktualizovat zásady "Otevřené" omezení, bude trvat přibližně 15 minut, než zásady přepne do "Otevřené" verze zásad.</span><span class="sxs-lookup"><span data-stu-id="58f9a-133">If you create a ContentKeyAuthorizationPolicy and specify to use a “Token” restriction, then test it, and then update the policy to “Open” restriction, it will take roughly 15 minutes before the policy switches to the “Open” version of the policy.</span></span>
* <span data-ttu-id="58f9a-134">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="58f9a-134">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
* <span data-ttu-id="58f9a-135">V současné době nelze zašifrovat progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="58f9a-135">Currently, you cannot encrypt progressive downloads.</span></span>

## <a name="aes-128-dynamic-encryption"></a><span data-ttu-id="58f9a-136">Dynamické šifrování AES-128</span><span class="sxs-lookup"><span data-stu-id="58f9a-136">AES-128 Dynamic Encryption</span></span>
### <a name="open-restriction"></a><span data-ttu-id="58f9a-137">Otevřete omezení</span><span class="sxs-lookup"><span data-stu-id="58f9a-137">Open Restriction</span></span>
<span data-ttu-id="58f9a-138">Otevřete omezení znamená, že systém bude poskytovat klíč všem uživatelům, kteří vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="58f9a-138">Open restriction means the system will deliver the key to anyone who makes a key request.</span></span> <span data-ttu-id="58f9a-139">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="58f9a-139">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="58f9a-140">Následující příklad vytvoří zásadu otevřete autorizace a přidává ji k klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-140">The following example creates an open authorization policy and adds it to the content key.</span></span>

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

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a><span data-ttu-id="58f9a-141">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="58f9a-141">Token Restriction</span></span>
<span data-ttu-id="58f9a-142">Tato část popisuje, jak vytvořit zásady autorizace klíče obsahu a přidružte ji k klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-142">This section describes how to create a content key authorization policy and associate it with the content key.</span></span> <span data-ttu-id="58f9a-143">Zásady autorizace popisuje, jaké autorizace musí být splněny k určení, pokud je uživatel oprávněn přijímat klíč (například nemá seznam "ověřovací klíč" obsahovat klíč, který byl podepsaný token).</span><span class="sxs-lookup"><span data-stu-id="58f9a-143">The authorization policy describes what authorization requirements must be met to determine if the user is authorized to receive the key (for example, does the “verification key” list contain the key that the token was signed with).</span></span>

<span data-ttu-id="58f9a-144">Pokud chcete konfigurovat omezení s tokenem možnost, budete muset použít XML k popisu tokenu autorizace požadavky.</span><span class="sxs-lookup"><span data-stu-id="58f9a-144">To configure the token restriction option, you need to use an XML to describe the token’s authorization requirements.</span></span> <span data-ttu-id="58f9a-145">Konfigurace omezení s tokenem XML musí odpovídat následujícím schématu XML.</span><span class="sxs-lookup"><span data-stu-id="58f9a-145">The token restriction configuration XML must conform to the following XML schema.</span></span>

#### <span data-ttu-id="58f9a-146"><a id="schema"></a>Omezení s tokenem schématu</span><span class="sxs-lookup"><span data-stu-id="58f9a-146"><a id="schema"></a>Token restriction schema</span></span>
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

<span data-ttu-id="58f9a-147">Při konfiguraci **tokenu** omezený zásady, je nutné zadat primární ** ověřovací klíč **, **vystavitele** a **cílovou skupinu** parametry.</span><span class="sxs-lookup"><span data-stu-id="58f9a-147">When configuring the **token** restricted policy, you must specify the primary** verification key**, **issuer** and **audience** parameters.</span></span> <span data-ttu-id="58f9a-148">** Primární ověření klíč ** obsahuje klíč, který byl podepsaný token, **vystavitele** je zabezpečený tokenu služba, která vydá token.</span><span class="sxs-lookup"><span data-stu-id="58f9a-148">The **primary verification key **contains the key that the token was signed with, **issuer** is the secure token service that issues the token.</span></span> <span data-ttu-id="58f9a-149">**Cílovou skupinu** (někdy nazývané **oboru**) popisuje záměr token nebo token povolí přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="58f9a-149">The **audience** (sometimes called **scope**) describes the intent of the token or the resource the token authorizes access to.</span></span> <span data-ttu-id="58f9a-150">Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.</span><span class="sxs-lookup"><span data-stu-id="58f9a-150">The Media Services key delivery service validates that these values in the token match the values in the template.</span></span> 

<span data-ttu-id="58f9a-151">Při použití **sady Media Services SDK pro .NET**, můžete použít **TokenRestrictionTemplate** k vygenerování tokenu omezení.</span><span class="sxs-lookup"><span data-stu-id="58f9a-151">When using **Media Services SDK for .NET**, you can use the **TokenRestrictionTemplate** class to generate the restriction token.</span></span>
<span data-ttu-id="58f9a-152">Následující příklad vytvoří zásad autorizace pomocí tokenu omezení.</span><span class="sxs-lookup"><span data-stu-id="58f9a-152">The following example creates an authorization policy with a token restriction.</span></span> <span data-ttu-id="58f9a-153">V tomto příkladu by klient musel token, který obsahuje k dispozici: podpisový klíč (VerificationKey), vydavatel tokenu a požadované deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="58f9a-153">In this example, the client would have to present a token that contains: signing key (VerificationKey), a token issuer, and required claims.</span></span>

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

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

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

#### <span data-ttu-id="58f9a-154"><a id="test"></a>Testovací token</span><span class="sxs-lookup"><span data-stu-id="58f9a-154"><a id="test"></a>Test token</span></span>
<span data-ttu-id="58f9a-155">Chcete-li získat token testovací podle tokenu omezení, která byla použita pro zásad autorizace pro klíč, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="58f9a-155">To get a test token based on the token restriction that was used for the key authorization policy, do the following.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a><span data-ttu-id="58f9a-156">Šifrování PlayReady dynamický</span><span class="sxs-lookup"><span data-stu-id="58f9a-156">PlayReady Dynamic Encryption</span></span>
<span data-ttu-id="58f9a-157">Služba Media Services umožňuje nakonfigurovat práva a omezení, které chcete použít pro modul runtime PlayReady DRM vynucovat, když uživatel se pokouší o přehrání chráněný obsah.</span><span class="sxs-lookup"><span data-stu-id="58f9a-157">Media Services enables you to configure the rights and restrictions that you want for the PlayReady DRM runtime to enforce when a user is trying to play back protected content.</span></span> 

<span data-ttu-id="58f9a-158">Při ochraně obsahu pomocí technologie PlayReady, jednou z věcí, je třeba zadat ve vaší zásady autorizace je řetězec XML, který definuje [šablona licence PlayReady](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="58f9a-158">When protecting your content with PlayReady, one of the things you need to specify in your authorization policy is an XML string that defines the [PlayReady license template](media-services-playready-license-template-overview.md).</span></span> <span data-ttu-id="58f9a-159">V sady Media Services SDK pro platformu .NET **PlayReadyLicenseResponseTemplate** a **PlayReadyLicenseTemplate** třídy vám pomohou definovat šablonu licence PlayReady.</span><span class="sxs-lookup"><span data-stu-id="58f9a-159">In Media Services SDK for .NET, the **PlayReadyLicenseResponseTemplate** and **PlayReadyLicenseTemplate** classes will help you define the PlayReady License Template.</span></span>

<span data-ttu-id="58f9a-160">[Toto téma](media-services-protect-with-drm.md) ukazuje, jak šifrování svůj obsah pomocí **PlayReady** a **Widevine**.</span><span class="sxs-lookup"><span data-stu-id="58f9a-160">[This topic](media-services-protect-with-drm.md) shows how to encrypt your content with **PlayReady** and **Widevine**.</span></span>

### <a name="open-restriction"></a><span data-ttu-id="58f9a-161">Otevřete omezení</span><span class="sxs-lookup"><span data-stu-id="58f9a-161">Open Restriction</span></span>
<span data-ttu-id="58f9a-162">Otevřete omezení znamená, že systém bude poskytovat klíč všem uživatelům, kteří vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="58f9a-162">Open restriction means the system will deliver the key to anyone who makes a key request.</span></span> <span data-ttu-id="58f9a-163">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="58f9a-163">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="58f9a-164">Následující příklad vytvoří zásadu otevřete autorizace a přidává ji k klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-164">The following example creates an open authorization policy and adds it to the content key.</span></span>

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

        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a><span data-ttu-id="58f9a-165">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="58f9a-165">Token Restriction</span></span>
<span data-ttu-id="58f9a-166">Pokud chcete konfigurovat omezení s tokenem možnost, budete muset použít XML k popisu tokenu autorizace požadavky.</span><span class="sxs-lookup"><span data-stu-id="58f9a-166">To configure the token restriction option, you need to use an XML to describe the token’s authorization requirements.</span></span> <span data-ttu-id="58f9a-167">Konfigurace omezení s tokenem XML musí odpovídat schématu XML ukazuje [to](#schema) části.</span><span class="sxs-lookup"><span data-stu-id="58f9a-167">The token restriction configuration XML must conform to the XML schema shown in [this](#schema) section.</span></span>

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

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key
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
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


<span data-ttu-id="58f9a-168">Chcete-li získat token testovací podle omezení s tokenem, který byl použit zásady autorizace pro klíč, najdete v tématu [to](#test) části.</span><span class="sxs-lookup"><span data-stu-id="58f9a-168">To get a test token based on the token restriction that was used for the key authorization policy see [this](#test) section.</span></span> 

## <span data-ttu-id="58f9a-169"><a id="types"></a>Typy používané při definování ContentKeyAuthorizationPolicy</span><span class="sxs-lookup"><span data-stu-id="58f9a-169"><a id="types"></a>Types used when defining ContentKeyAuthorizationPolicy</span></span>
### <span data-ttu-id="58f9a-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span><span class="sxs-lookup"><span data-stu-id="58f9a-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span></span>
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <span data-ttu-id="58f9a-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="58f9a-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <span data-ttu-id="58f9a-172"><a id="TokenType"></a>TokenType</span><span class="sxs-lookup"><span data-stu-id="58f9a-172"><a id="TokenType"></a>TokenType</span></span>
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="58f9a-173">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="58f9a-173">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="58f9a-174">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="58f9a-174">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="58f9a-175">Další krok</span><span class="sxs-lookup"><span data-stu-id="58f9a-175">Next step</span></span>
<span data-ttu-id="58f9a-176">Teď, když jste nakonfigurovali zásady autorizace klíče obsahu, přejděte na [postup konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="58f9a-176">Now that you have configured content key's authorization policy, go to the [How to configure asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md) topic.</span></span>

