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
# <a name="using-axinom-toodeliver-widevine-licenses-tooazure-media-services"></a>Pomocí Axinom toodeliver Widevine licence tooAzure Media Services
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Přehled
Azure Media Services (AMS) přidala Google Widevine dynamické ochrany (viz [Mingfei na blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) podrobnosti). Kromě toho je Azure Media Player (AMP) také přidána podpora Widevine (viz [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) podrobnosti). Toto je hlavní byly potřebné při streamování DASH obsah chráněný CENC s více-native technologiemi DRM (PlayReady a Widevine) na vybaven MSE a EME moderní prohlížeče.

Počínaje hello sady Media Services .NET SDK verze 3.5.2, Media Services umožňuje vám tooconfigure Widevine šablona licence a získání licence na Widevine. Můžete také použít následující toohelp partneři AMS doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Tento článek popisuje, jak toointegrate a testování Widevine licenční server spravuje Axinom. Konkrétně obsahuje:  

* Konfigurace běžného dynamického šifrování s více technologiemi DRM (PlayReady a Widevine) s odpovídající získání adresy URL licence;
* Generování JWT token v pořadí toomeet hello licenční server požadavky;
* Vývoj aplikace Azure Media Player, která zpracovává získání licence s ověření pomocí tokenu JWT;

Hello celý systém a hello toku obsahu, které mohou hello následující diagram nejlépe popsány ID klíče, klíče, klíče počáteční hodnoty, JTW token a jeho deklarací identity.

![DASH a šifrování CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Content Protection
Konfigurace dynamické ochrany a zásady doručení klíče, najdete v tématu Mingfei na blogu: [jak tooconfigure balení Widevine pomocí služby Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Můžete nakonfigurovat dynamické ochrany CENC s více technologiemi DRM pro DASH streamování současné hello následující:

1. PlayReady ochranu MS Edge a IE11, která by mohla mít omezení tokenu autorizace. zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS), jako je Azure Active Directory;
2. Pro Chrome Widevine ochranu, se může vyžadovat ověření pomocí tokenu s tokenem vydaným službou tokenů zabezpečení jiné. 

Najdete v tématu [generování tokenů JWT](media-services-axinom-integration.md#jwt-token-generation) části Proč nelze použít Azure Active Directory jako služby tokenů zabezpečení pro Axinom na Widevine licenční server.

### <a name="considerations"></a>Požadavky
1. Je nutné použít hello specifikovány Axinom počáteční hodnoty klíče (8888000000000000000000000000000000000000) a vygenerovaný nebo vybrané klíče ID toogenerate hello obsah klíče pro konfiguraci služby doručení klíče. Axinom licenční server vydá všechny licence obsahující obsahu klíče založené na hello stejný klíč počáteční hodnotu, která je platná pro testování a produkci.
2. Hello adresu URL získání licence Widevine pro testování: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). Protokol HTTP a HTTS jsou povoleny.

## <a name="azure-media-player-preparation"></a>Příprava Azure Media Player
AMP v1.4.0 podporuje přehrávání AMS obsahu, který je dynamicky spojených s technologií PlayReady a Widevine DRM.
Pokud server licence Widevine nevyžaduje ověření pomocí tokenu, neexistuje žádné další, že budete potřebovat tootest toodo DASH obsah chráněný pomocí Widevine. Příklad, hello AMP tým poskytne jednoduchou [ukázka](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), kde uvidíte jejich práce v hraniční a IE11 s technologií PlayReady) a Chrome (s technologií Widevine.
poskytuje Axinom Hello Widevine licenční server vyžaduje ověření pomocí tokenu JWT. Hello JWT token musí toobe odeslal se požadavek na licenční prostřednictvím záhlaví HTTP "X-AxDRM – zpráva". Pro tento účel je třeba tooadd hello následující javascript hello webové stránce hostování AMP před zdroj hello nastavení:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Hello zbytek AMP kód je standardní rozhraní API AMP jako dokument AMP [zde](http://amp.azure.net/libs/amp/latest/docs/).

Všimněte si, že hello výše javascript pro nastavení, které vlastní autorizační hlavičky je stále přístup krátkodobou před hello oficiální, které vydání dlouhodobá přístup v AMP.

## <a name="jwt-token-generation"></a>Generování tokenů JWT
Server licence Axinom Widevine pro testování vyžaduje ověření pomocí tokenu JWT. Kromě toho jeden hello deklarací identity v tokenu JWT hello je komplexní objekt typu místo primitivní datový typ.

Bohužel Azure AD mohou pouze vystavovat tokeny JWT s primitivními typy. Podobně rozhraní .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler a JwtPayload) lze pouze tooinput komplexní objekt typu jako deklarace identity. Hello deklarace identity jsou stále serializovat jako řetězec. Proto nemůžeme použít některé z hello dva pro generování hello token JWT pro požadavek na licenční Widevine.

Jan Sheehan [balíček JWT Nuget](https://www.nuget.org/packages/JWT) splňuje hello potřebám, přidáme toouse tento balíček Nuget.

Dole je, že hello kód pro generování token JWT s hello deklarace identity podle požadavků Axinom Widevine licenční server pro testování:

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

Axinom Widevine licenčního serveru

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a>Požadavky
1. I když služba doručování licencí AMS PlayReady vyžaduje, aby "nosiče =" předcházející ověřovací token, Axinom Widevine licenční server nepoužívá se.
2. Hello Axinom komunikace klíč slouží jako podpisového klíče. Všimněte si, že je tento klíč hello šestnáctkový řetězec, ale musí být považované jako řadu bajtů není řetězec při kódování. Toho se dosáhne hello metoda ConvertHexStringToByteArray.

## <a name="retrieving-key-id"></a>Načítání ID klíče
Jste si všimli, že v hello kódu pro generování token JWT je požadováno ID tokenu, klíče. Vzhledem k tomu, že hello JWT token potřebuje toobe připravené před načtením AMP player, klíče potřeb ID, které toobe načíst v pořadí toogenerate JWT token.

ID kurzu, který několika způsoby uložení tooget klíče. Například může uložit jeden klíč ID společně s metadata obsahu v databázi. Nebo můžete načíst klíč ID ze souboru MPD DASH (popis prezentace média). Následující kód Hello je pro pozdější hello.

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

## <a name="summary"></a>Souhrn
Uveďte nejnovější Widevine podpory v Azure Media Services ochranu obsahu a Azure Media Player jsme dokážou tooimplement streamování z DASH + native více technologiemi DRM (PlayReady a Widevine) i službou licence PlayReady v AMS a Widevine licenci Server z Axinom pro hello následující moderní prohlížeče:

* Chrome
* Microsoft Edge ve Windows 10
* IE 11 na Windows 8.1 a Windows 10
* Firefox (Desktop) a Safari v systému Mac (ne iOS) jsou podporovány také prostřednictvím Silverlight a hello stejnou adresu URL s Azure Media Player

Hello následující parametry jsou potřeba v hello zkrácená řešení využívání Axinom Widevine licenční server. S výjimkou klíč ID, hello zbytek parametrů jsou poskytovány na základě jejich nastavení serveru Widevine Axinom.

| Parametr | Jak se používají |
| --- | --- |
| Komunikace klíče ID |Musí být zahrnut jako hodnota deklarace identity hello "com_key_id" v tokenu JWT (viz [to](media-services-axinom-integration.md#jwt-token-generation) části). |
| Klíč komunikace |Musíte použít jako hello podpisový klíč tokenu JWT (najdete v části [to](media-services-axinom-integration.md#jwt-token-generation) části). |
| Počáteční hodnoty klíče |Je třeba použít toogenerate klíč obsahu s žádným věnovat obsah klíče ID (v tématu [to](media-services-axinom-integration.md#content-protection) části). |
| Adresa URL pro získání licence Widevine |Je třeba použít konfigurace zásad doručení assetu pro streamování DASH (najdete v části [to](media-services-axinom-integration.md#content-protection) části). |
| ID obsahu klíče |Musí být součástí hello hodnoty deklarace identity nárocích zpráva tokenu JWT (viz [to](media-services-axinom-integration.md#jwt-token-generation) části). |

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Potvrzování
Rádi bychom znali tooacknowledge hello následující osoby podílí k vytvoření tohoto dokumentu: Kristjan Jõgi z Axinom, Mingfei Jan a Amitu Rajput.

