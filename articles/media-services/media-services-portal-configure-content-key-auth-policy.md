---
title: "pomocí zásad autorizace klíče obsahu aaaConfigure hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure zásad autorizace pro klíč obsahu."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 157fb691b7f71f4889228817e1dc64555e327d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-content-key-authorization-policy"></a>Nakonfigurujte zásady autorizace klíče obsahu
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Přehled
Microsoft Azure Media Services umožňuje toodeliver MPEG-DASH, technologie Smooth Streaming a datové proudy HTTP-Live-Streaming (HLS) chráněné pomocí Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS taky umožňuje vám toodeliver datové proudy DASH šifrované pomocí Widevine DRM. Technologie PlayReady a Widevine jsou šifrované podle specifikace Common Encryption (ISO/IEC CENC 23001-7) hello.

Služba Media Services také poskytuje **klíč nebo licenční služby doručení** ze kterých klienti můžete získat klíče AES nebo tooplay licence PlayReady nebo Widevine hello šifrovaný obsah.

Toto téma ukazuje, jak toouse hello zásady autorizace klíče obsahu Azure portálu tooconfigure hello. Hello klíč můžete později použít toodynamically šifrování vašeho obsahu. Všimněte si, že teď můžete šifrovat hello následujících formátů streamování: HLS, MPEG DASH a technologie Smooth Streaming. Nelze zašifrovat progresivní stahování.

Když přehrávač vyžádá datový proud, který je nastaven toobe dynamicky šifrovat, šifrování klíče toodynamically hello nakonfigurované Media Services používá svůj obsah pomocí šifrování AES nebo DRM. datový proud hello toodecrypt, hello player bude požadovat hello klíč z hello doručení klíče služby. toodecide, jestli je uživatel hello autorizovaný tooget hello klíč, hello služba vyhodnocuje hello zásady autorizace, které jste zadali pro klíč hello.

Plánování toohave více klíčů k obsahu nebo chcete toospecify **klíč nebo licenční služby doručení** URL než hello doručení klíče služby Media Services, pomocí sady Media Services .NET SDK nebo rozhraní REST API.

[Konfigurace obsahu zásad autorizace pro klíč pomocí sady Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurace obsahu zásad autorizace pro klíč pomocí Media Services REST API](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Musí být splněny určité předpoklady:
* Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení a dynamické šifrování, koncový bod streamování má toobe v hello **systémem** stavu. 
* Váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí. Další informace najdete v tématu [kódovat asset](media-services-encode-asset.md).
* Hello služba pro přenos klíče ukládá do mezipaměti ContentKeyAuthorizationPolicy a související objekty (Možnosti zásad a omezení) pro 15 minut.  Pokud vytvoříte ContentKeyAuthorizationPolicy a zadejte toouse "Token" omezení, pak otestovat ji a potom aktualizovat zásady hello příliš "Otevřít" omezení, bude trvat přibližně 15 minut před hello zásad přepínače toohello "Otevřené" verze hello zásad.

## <a name="how-to-configure-hello-key-authorization-policy"></a>Postupy: Konfigurace zásad autorizace pro klíč hello
zásady autorizace pro klíč tooconfigure hello, vyberte hello **ochrana obsahu** stránky.

Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč. zásady autorizace klíče obsahu Hello může mít **otevřete**, **tokenu**, nebo **IP** autorizace omezení (**IP** se dá nakonfigurovat s REST nebo .NET SDK).

### <a name="open-restriction"></a>Otevřete omezení
Hello **otevřete** omezení znamená hello systému dodá hello tooanyone klíče, který vytváří klíče požadavek. Toto omezení může být užitečná pro účely testování.

![Funkce OpenPolicy][open_policy]

### <a name="token-restriction"></a>Omezení s tokenem
toochoose hello token omezený zásady, stiskněte klávesu hello **TOKENU** tlačítko.

Hello **tokenu** zásady omezení musí být doplněny tokenem vydaným podle **služby tokenů zabezpečení** (STS). Služba Media Services podporuje tokeny ve hello **jednoduchých webových tokenů** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátu a **webových tokenů JSON** formátu (JWT). Informace najdete v tématu [ověření pomocí tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Služba Media Services neposkytuje **zabezpečení tokenu služby**. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny. Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a vystavování deklarací identity, které jste zadali v konfiguraci omezení s tokenem hello. Hello doručení klíče služby Media Services vrátí hello šifrovací klíče toohello klienta Pokud hello token je platný a hello deklarace v tokenu hello odpovídají nakonfigurovaným pro klíč obsahu hello. Další informace najdete v tématu [použití Azure ACS tooissue tokeny](http://mingfeiy.com/acs-with-key-services).

Při konfiguraci hello **TOKENU** zásady omezení, musíte nastavit hodnoty pro **ověřovací klíč**, **vystavitele** a **cílovou skupinu**. Hello ověření primární klíč obsahuje hello klíče, že byl podepsaný hello token, vystavitele hello zabezpečené tokenu služba, která vydá hello token. Cílová skupina Hello (někdy nazývané oboru) popisuje hello záměr hello tokenu nebo token hello prostředku hello povolí přístup k. Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.

### <a name="playready"></a>PlayReady
Při ochraně svůj obsah pomocí **PlayReady**, jeden hello věcí, je nutné toospecify ve vaší zásady autorizace je řetězec XML, který definuje šablona licence PlayReady hello. Ve výchozím nastavení je nastavené hello následující zásady:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>povolené</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

Můžete kliknout na hello **importovat zásady xml** tlačítko a zadejte jiný jazyk XML, který vyhovuje toohello XML schéma definované [zde](media-services-playready-license-template-overview.md).

## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

