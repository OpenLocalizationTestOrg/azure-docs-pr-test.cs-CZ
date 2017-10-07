---
title: aaaUsing Axinom toodeliver Widevine licence tooAzure Media Services | Microsoft Docs
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) toodeliver datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. licence PlayReady Hello pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje Axinom licenční server."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 2245d9269c30712ef779973ae021c00c76174d0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-axinom-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="3f2f3-104">Pomocí Axinom toodeliver Widevine licence tooAzure Media Services</span><span class="sxs-lookup"><span data-stu-id="3f2f3-104">Using Axinom toodeliver Widevine licenses tooAzure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f2f3-105">castLabs</span><span class="sxs-lookup"><span data-stu-id="3f2f3-105">castLabs</span></span>](media-services-castlabs-integration.md)
> * [<span data-ttu-id="3f2f3-106">Axinom</span><span class="sxs-lookup"><span data-stu-id="3f2f3-106">Axinom</span></span>](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3f2f3-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="3f2f3-107">Overview</span></span>
<span data-ttu-id="3f2f3-108">Azure Media Services (AMS) přidala Google Widevine dynamické ochrany (viz [Mingfei na blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-108">Azure Media Services (AMS) has added Google Widevine dynamic protection (see [Mingfei’s blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) for details).</span></span> <span data-ttu-id="3f2f3-109">Kromě toho je Azure Media Player (AMP) také přidána podpora Widevine (viz [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-109">In addition, Azure Media Player (AMP) has also added Widevine support (see [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details).</span></span> <span data-ttu-id="3f2f3-110">Toto je hlavní byly potřebné při streamování DASH obsah chráněný CENC s více-native technologiemi DRM (PlayReady a Widevine) na vybaven MSE a EME moderní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-110">This is a major accomplishment in streaming DASH content protected by CENC with multi-native-DRM (PlayReady and Widevine) on modern browsers equipped with MSE and EME.</span></span>

<span data-ttu-id="3f2f3-111">Počínaje hello sady Media Services .NET SDK verze 3.5.2, Media Services umožňuje vám tooconfigure Widevine šablona licence a získání licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-111">Starting with hello Media Services .NET SDK version 3.5.2, Media Services enables you tooconfigure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="3f2f3-112">Můžete také použít následující toohelp partneři AMS doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-112">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="3f2f3-113">Tento článek popisuje, jak toointegrate a testování Widevine licenční server spravuje Axinom.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-113">This article describes how toointegrate and test Widevine license server managed by Axinom.</span></span> <span data-ttu-id="3f2f3-114">Konkrétně obsahuje:</span><span class="sxs-lookup"><span data-stu-id="3f2f3-114">Specifically, it covers:</span></span>  

* <span data-ttu-id="3f2f3-115">Konfigurace běžného dynamického šifrování s více technologiemi DRM (PlayReady a Widevine) s odpovídající získání adresy URL licence;</span><span class="sxs-lookup"><span data-stu-id="3f2f3-115">Configuring dynamic Common Encryption with multi-DRM (PlayReady and Widevine) with corresponding license acquisition URLs;</span></span>
* <span data-ttu-id="3f2f3-116">Generování JWT token v pořadí toomeet hello licenční server požadavky;</span><span class="sxs-lookup"><span data-stu-id="3f2f3-116">Generating a JWT token in order toomeet hello license server requirements;</span></span>
* <span data-ttu-id="3f2f3-117">Vývoj aplikace Azure Media Player, která zpracovává získání licence s ověření pomocí tokenu JWT;</span><span class="sxs-lookup"><span data-stu-id="3f2f3-117">Developing Azure Media Player app which handles license acquisition with JWT token authentication;</span></span>

<span data-ttu-id="3f2f3-118">Hello celý systém a hello toku obsahu, které mohou hello následující diagram nejlépe popsány ID klíče, klíče, klíče počáteční hodnoty, JTW token a jeho deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-118">hello complete system and hello flow of content key, key ID, key seed, JTW token and its claims can be best described by hello following diagram.</span></span>

![DASH a šifrování CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a><span data-ttu-id="3f2f3-120">Content Protection</span><span class="sxs-lookup"><span data-stu-id="3f2f3-120">Content Protection</span></span>
<span data-ttu-id="3f2f3-121">Konfigurace dynamické ochrany a zásady doručení klíče, najdete v tématu Mingfei na blogu: [jak tooconfigure balení Widevine pomocí služby Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-121">For configuring dynamic protection and key delivery policy, please see Mingfei’s blog: [How tooconfigure Widevine packaging with Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).</span></span>

<span data-ttu-id="3f2f3-122">Můžete nakonfigurovat dynamické ochrany CENC s více technologiemi DRM pro DASH streamování současné hello následující:</span><span class="sxs-lookup"><span data-stu-id="3f2f3-122">You can configure dynamic CENC protection with multi-DRM for DASH streaming having both of hello following:</span></span>

1. <span data-ttu-id="3f2f3-123">PlayReady ochranu MS Edge a IE11, která by mohla mít omezení tokenu autorizace.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-123">PlayReady protection for MS Edge and IE11, that could have a token authorization restrictions.</span></span> <span data-ttu-id="3f2f3-124">zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS), jako je Azure Active Directory;</span><span class="sxs-lookup"><span data-stu-id="3f2f3-124">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS), such as Azure Active Directory;</span></span>
2. <span data-ttu-id="3f2f3-125">Pro Chrome Widevine ochranu, se může vyžadovat ověření pomocí tokenu s tokenem vydaným službou tokenů zabezpečení jiné.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-125">Widevine protection for Chrome, it can require token authentication with token issued by another STS.</span></span> 

<span data-ttu-id="3f2f3-126">Najdete v tématu [generování tokenů JWT](media-services-axinom-integration.md#jwt-token-generation) části Proč nelze použít Azure Active Directory jako služby tokenů zabezpečení pro Axinom na Widevine licenční server.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-126">Please see [JWT Token Generation](media-services-axinom-integration.md#jwt-token-generation) section for why Azure Active Directory cannot be used as an STS for Axinom’s Widevine license server.</span></span>

### <a name="considerations"></a><span data-ttu-id="3f2f3-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f2f3-127">Considerations</span></span>
1. <span data-ttu-id="3f2f3-128">Je nutné použít hello specifikovány Axinom počáteční hodnoty klíče (8888000000000000000000000000000000000000) a vygenerovaný nebo vybrané klíče ID toogenerate hello obsah klíče pro konfiguraci služby doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-128">You must use hello Axinom specified key seed (8888000000000000000000000000000000000000) and your generated or selected key ID toogenerate hello content key for configuring key delivery service.</span></span> <span data-ttu-id="3f2f3-129">Axinom licenční server vydá všechny licence obsahující obsahu klíče založené na hello stejný klíč počáteční hodnotu, která je platná pro testování a produkci.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-129">Axinom license server will issue all licenses containing content keys based on hello same key seed, which is valid for both testing and production.</span></span>
2. <span data-ttu-id="3f2f3-130">Hello adresu URL získání licence Widevine pro testování: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-130">hello Widevine license acquisition URL for testing: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense).</span></span> <span data-ttu-id="3f2f3-131">Protokol HTTP a HTTS jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-131">Both HTTP and HTTS are allowed.</span></span>

## <a name="azure-media-player-preparation"></a><span data-ttu-id="3f2f3-132">Příprava Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="3f2f3-132">Azure Media Player Preparation</span></span>
<span data-ttu-id="3f2f3-133">AMP v1.4.0 podporuje přehrávání AMS obsahu, který je dynamicky spojených s technologií PlayReady a Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-133">AMP v1.4.0 supports playback of AMS content that is dynamically packaged with both PlayReady and Widevine DRM.</span></span>
<span data-ttu-id="3f2f3-134">Pokud server licence Widevine nevyžaduje ověření pomocí tokenu, neexistuje žádné další, že budete potřebovat tootest toodo DASH obsah chráněný pomocí Widevine.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-134">If Widevine license server does not require token authentication, there is nothing additional you need toodo tootest a DASH content protected by Widevine.</span></span> <span data-ttu-id="3f2f3-135">Příklad, hello AMP tým poskytne jednoduchou [ukázka](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), kde uvidíte jejich práce v hraniční a IE11 s technologií PlayReady) a Chrome (s technologií Widevine.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-135">For an example, hello AMP team provides a simple [sample](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), where you can see it working in Edge and IE11 with PlayReady and Chrome with Widevine.</span></span>
<span data-ttu-id="3f2f3-136">poskytuje Axinom Hello Widevine licenční server vyžaduje ověření pomocí tokenu JWT.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-136">hello Widevine license server provided by Axinom requires JWT token authentication.</span></span> <span data-ttu-id="3f2f3-137">Hello JWT token musí toobe odeslal se požadavek na licenční prostřednictvím záhlaví HTTP "X-AxDRM – zpráva".</span><span class="sxs-lookup"><span data-stu-id="3f2f3-137">hello JWT token needs toobe submitted with license request through an HTTP header “X-AxDRM-Message”.</span></span> <span data-ttu-id="3f2f3-138">Pro tento účel je třeba tooadd hello následující javascript hello webové stránce hostování AMP před zdroj hello nastavení:</span><span class="sxs-lookup"><span data-stu-id="3f2f3-138">For this purpose, you need tooadd hello following javascript in hello web page hosting AMP before setting hello source:</span></span>

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

<span data-ttu-id="3f2f3-139">Hello zbytek AMP kód je standardní rozhraní API AMP jako dokument AMP [zde](http://amp.azure.net/libs/amp/latest/docs/).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-139">hello rest of AMP code is standard AMP API as in AMP document [here](http://amp.azure.net/libs/amp/latest/docs/).</span></span>

<span data-ttu-id="3f2f3-140">Všimněte si, že hello výše javascript pro nastavení, které vlastní autorizační hlavičky je stále přístup krátkodobou před hello oficiální, které vydání dlouhodobá přístup v AMP.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-140">Note that hello above javascript for setting custom authorization header is still a short term approach before hello official long term approach in AMP is released.</span></span>

## <a name="jwt-token-generation"></a><span data-ttu-id="3f2f3-141">Generování tokenů JWT</span><span class="sxs-lookup"><span data-stu-id="3f2f3-141">JWT Token Generation</span></span>
<span data-ttu-id="3f2f3-142">Server licence Axinom Widevine pro testování vyžaduje ověření pomocí tokenu JWT.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-142">Axinom Widevine license server for testing requires JWT token authentication.</span></span> <span data-ttu-id="3f2f3-143">Kromě toho jeden hello deklarací identity v tokenu JWT hello je komplexní objekt typu místo primitivní datový typ.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-143">In addition, one of hello claims in hello JWT token is of a complex object type instead of primitive data type.</span></span>

<span data-ttu-id="3f2f3-144">Bohužel Azure AD mohou pouze vystavovat tokeny JWT s primitivními typy.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-144">Unfortunately, Azure AD can only issue JWT tokens with primitive types.</span></span> <span data-ttu-id="3f2f3-145">Podobně rozhraní .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler a JwtPayload) lze pouze tooinput komplexní objekt typu jako deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-145">Similarly, .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler and JwtPayload) only allows you tooinput complex object type as claims.</span></span> <span data-ttu-id="3f2f3-146">Hello deklarace identity jsou stále serializovat jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-146">However, hello claims are still serialized as string.</span></span> <span data-ttu-id="3f2f3-147">Proto nemůžeme použít některé z hello dva pro generování hello token JWT pro požadavek na licenční Widevine.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-147">Therefore we cannot use any of hello two for generating hello JWT token for Widevine license request.</span></span>

<span data-ttu-id="3f2f3-148">Jan Sheehan [balíček JWT Nuget](https://www.nuget.org/packages/JWT) splňuje hello potřebám, přidáme toouse tento balíček Nuget.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-148">John Sheehan’s [JWT Nuget package](https://www.nuget.org/packages/JWT) meets hello needs so we are going toouse this Nuget package.</span></span>

<span data-ttu-id="3f2f3-149">Dole je, že hello kód pro generování token JWT s hello deklarace identity podle požadavků Axinom Widevine licenční server pro testování:</span><span class="sxs-lookup"><span data-stu-id="3f2f3-149">Below is hello code for generating JWT token with hello needed claims as required by Axinom Widevine license server for testing:</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;

    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string toobyte[] Note: Note that hello key is a hex string, however it must be treated as a series of bytes not a string when encoding.

                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };

                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);

                return token;
            }

            //convert hex string toobyte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "hello binary key cannot have an odd number of digits: {0}", hexString));
                }

                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }

                return HexAsBytes;
            }

        }  

    }  

<span data-ttu-id="3f2f3-150">Axinom Widevine licenčního serveru</span><span class="sxs-lookup"><span data-stu-id="3f2f3-150">Axinom Widevine license server</span></span>

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a><span data-ttu-id="3f2f3-151">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f2f3-151">Considerations</span></span>
1. <span data-ttu-id="3f2f3-152">I když služba doručování licencí AMS PlayReady vyžaduje, aby "nosiče =" předcházející ověřovací token, Axinom Widevine licenční server nepoužívá se.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-152">Even though AMS PlayReady license delivery service requires “Bearer=” preceding an authentication token, Axinom Widevine license server does not use it.</span></span>
2. <span data-ttu-id="3f2f3-153">Hello Axinom komunikace klíč slouží jako podpisového klíče.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-153">hello Axinom communication key is used as signing key.</span></span> <span data-ttu-id="3f2f3-154">Všimněte si, že je tento klíč hello šestnáctkový řetězec, ale musí být považované jako řadu bajtů není řetězec při kódování.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-154">Note that hello key is a hex string, however it must be treated as a series of bytes not a string when encoding.</span></span> <span data-ttu-id="3f2f3-155">Toho se dosáhne hello metoda ConvertHexStringToByteArray.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-155">This is achieved by hello method ConvertHexStringToByteArray.</span></span>

## <a name="retrieving-key-id"></a><span data-ttu-id="3f2f3-156">Načítání ID klíče</span><span class="sxs-lookup"><span data-stu-id="3f2f3-156">Retrieving Key ID</span></span>
<span data-ttu-id="3f2f3-157">Jste si všimli, že v hello kódu pro generování token JWT je požadováno ID tokenu, klíče.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-157">You may have noticed that in hello code for generating a JWT token, key ID is required.</span></span> <span data-ttu-id="3f2f3-158">Vzhledem k tomu, že hello JWT token potřebuje toobe připravené před načtením AMP player, klíče potřeb ID, které toobe načíst v pořadí toogenerate JWT token.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-158">Since hello JWT token needs toobe ready before loading AMP player, key ID needs toobe retrieved in order toogenerate JWT token.</span></span>

<span data-ttu-id="3f2f3-159">ID kurzu, který několika způsoby uložení tooget klíče.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-159">Of course there are multiple ways tooget hold of key ID.</span></span> <span data-ttu-id="3f2f3-160">Například může uložit jeden klíč ID společně s metadata obsahu v databázi.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-160">For example, one may store key ID together with content metadata in a database.</span></span> <span data-ttu-id="3f2f3-161">Nebo můžete načíst klíč ID ze souboru MPD DASH (popis prezentace média).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-161">Or you can retrieve key ID from DASH MPD (Media Presentation Description) file.</span></span> <span data-ttu-id="3f2f3-162">Následující kód Hello je pro pozdější hello.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-162">hello code below is for hello latter.</span></span>

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }

        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();

        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);

        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }

        return key_id;
    }

## <a name="summary"></a><span data-ttu-id="3f2f3-163">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3f2f3-163">Summary</span></span>
<span data-ttu-id="3f2f3-164">Uveďte nejnovější Widevine podpory v Azure Media Services ochranu obsahu a Azure Media Player jsme dokážou tooimplement streamování z DASH + native více technologiemi DRM (PlayReady a Widevine) i službou licence PlayReady v AMS a Widevine licenci Server z Axinom pro hello následující moderní prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="3f2f3-164">With latest addition of Widevine support in both Azure Media Services Content Protection and Azure Media Player, we are able tooimplement streaming of DASH + Multi-native-DRM (PlayReady + Widevine) with both PlayReady license service in AMS and Widevine license server from Axinom for hello following modern browsers:</span></span>

* <span data-ttu-id="3f2f3-165">Chrome</span><span class="sxs-lookup"><span data-stu-id="3f2f3-165">Chrome</span></span>
* <span data-ttu-id="3f2f3-166">Microsoft Edge ve Windows 10</span><span class="sxs-lookup"><span data-stu-id="3f2f3-166">Microsoft Edge on Windows 10</span></span>
* <span data-ttu-id="3f2f3-167">IE 11 na Windows 8.1 a Windows 10</span><span class="sxs-lookup"><span data-stu-id="3f2f3-167">IE 11 on Windows 8.1 and Windows 10</span></span>
* <span data-ttu-id="3f2f3-168">Firefox (Desktop) a Safari v systému Mac (ne iOS) jsou podporovány také prostřednictvím Silverlight a hello stejnou adresu URL s Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="3f2f3-168">Both Firefox (Desktop) and Safari on Mac (not iOS) are also supported via Silverlight and hello same URL with Azure Media Player</span></span>

<span data-ttu-id="3f2f3-169">Hello následující parametry jsou potřeba v hello zkrácená řešení využívání Axinom Widevine licenční server.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-169">hello following parameters are required in hello mini-solution leveraging Axinom Widevine license server.</span></span> <span data-ttu-id="3f2f3-170">S výjimkou klíč ID, hello zbytek parametrů jsou poskytovány na základě jejich nastavení serveru Widevine Axinom.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-170">Except for key ID, hello rest of parameters are provided by Axinom based on their Widevine server setup.</span></span>

| <span data-ttu-id="3f2f3-171">Parametr</span><span class="sxs-lookup"><span data-stu-id="3f2f3-171">Parameter</span></span> | <span data-ttu-id="3f2f3-172">Jak se používají</span><span class="sxs-lookup"><span data-stu-id="3f2f3-172">How it is used</span></span> |
| --- | --- |
| <span data-ttu-id="3f2f3-173">Komunikace klíče ID</span><span class="sxs-lookup"><span data-stu-id="3f2f3-173">Communication key ID</span></span> |<span data-ttu-id="3f2f3-174">Musí být zahrnut jako hodnota deklarace identity hello "com_key_id" v tokenu JWT (viz [to](media-services-axinom-integration.md#jwt-token-generation) části).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-174">Must be included as value of hello claim "com_key_id" in JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |
| <span data-ttu-id="3f2f3-175">Klíč komunikace</span><span class="sxs-lookup"><span data-stu-id="3f2f3-175">Communication key</span></span> |<span data-ttu-id="3f2f3-176">Musíte použít jako hello podpisový klíč tokenu JWT (najdete v části [to](media-services-axinom-integration.md#jwt-token-generation) části).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-176">Must be used as hello signing key of JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |
| <span data-ttu-id="3f2f3-177">Počáteční hodnoty klíče</span><span class="sxs-lookup"><span data-stu-id="3f2f3-177">Key seed</span></span> |<span data-ttu-id="3f2f3-178">Je třeba použít toogenerate klíč obsahu s žádným věnovat obsah klíče ID (v tématu [to](media-services-axinom-integration.md#content-protection) části).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-178">Must be used toogenerate content key with any given content key ID (see  [this](media-services-axinom-integration.md#content-protection) section).</span></span> |
| <span data-ttu-id="3f2f3-179">Adresa URL pro získání licence Widevine</span><span class="sxs-lookup"><span data-stu-id="3f2f3-179">Widevine License acquisition URL</span></span> |<span data-ttu-id="3f2f3-180">Je třeba použít konfigurace zásad doručení assetu pro streamování DASH (najdete v části [to](media-services-axinom-integration.md#content-protection) části).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-180">Must be used in configuring asset delivery policy for DASH streaming (see  [this](media-services-axinom-integration.md#content-protection) section ).</span></span> |
| <span data-ttu-id="3f2f3-181">ID obsahu klíče</span><span class="sxs-lookup"><span data-stu-id="3f2f3-181">Content Key ID</span></span> |<span data-ttu-id="3f2f3-182">Musí být součástí hello hodnoty deklarace identity nárocích zpráva tokenu JWT (viz [to](media-services-axinom-integration.md#jwt-token-generation) části).</span><span class="sxs-lookup"><span data-stu-id="3f2f3-182">Must be included as part of hello value of Entitlement Message claim of JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |

## <a name="media-services-learning-paths"></a><span data-ttu-id="3f2f3-183">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="3f2f3-183">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3f2f3-184">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="3f2f3-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="3f2f3-185">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="3f2f3-185">Acknowledgments</span></span>
<span data-ttu-id="3f2f3-186">Rádi bychom znali tooacknowledge hello následující osoby podílí k vytvoření tohoto dokumentu: Kristjan Jõgi z Axinom, Mingfei Jan a Amitu Rajput.</span><span class="sxs-lookup"><span data-stu-id="3f2f3-186">We would like tooacknowledge hello following people who contributed towards creating this document: Kristjan Jõgi of Axinom, Mingfei Yan, and Amit Rajput.</span></span>

