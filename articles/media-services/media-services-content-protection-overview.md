---
title: "aaaProtect svůj obsah pomocí Azure Media Services | Microsoft Docs"
description: "Tento článek poskytuje přehled toho Ochrana obsahu pomocí služby Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: abab7602d71d7357a692976420ca9a988c0d096f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-content-overview"></a>Ochrana obsahu – přehled
Microsoft Azure Media Services umožňuje vám toosecure médiu z hello doby, kdy opouští váš počítač prostřednictvím úložiště, zpracování a doručení. Služba Media Services umožňuje toodeliver obsah za provozu a na vyžádání šifrován dynamicky Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo některého z hello hlavní technologiemi DRM: Microsoft PlayReady, Google Widevine a Apple FairPlay. Služba Media Services také poskytuje službu k doručování klíčů AES a klienti tooauthorized licence DRM (PlayReady, Widevine a FairPlay). 

Hello následující obrázek ukazuje pracovní postupy hello ochrana obsahu, které podporuje AMS. 

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 

Toto téma vysvětluje [principy a terminologií](media-services-content-protection-overview.md) relevantní toounderstanding obsahu ochrany pomocí služby AMS. Hello téma obsahuje taky [odkazy](media-services-content-protection-overview.md#common-scenarios) tootopics, která ukazují, jak tooachieve obsahu úlohy ochrany. 

## <a name="dynamic-encryption"></a>Dynamické šifrování
Microsoft Azure Media Services umožňuje toodeliver obsah šifrován dynamicky zrušte klíče AES nebo DRM šifrování: Microsoft PlayReady, Google Widevine a Apple FairPlay.

V současné době můžete šifrovat hello následující streamování formáty: HLS, MPEG DASH a technologie Smooth Streaming. Nelze zašifrovat progresivní stahování.

Pokud chcete pro Media Services tooencrypt prostředek, třeba tooassociate šifrovací klíč (CommonEncryption nebo EnvelopeEncryption) s asset a taky nakonfigurovat zásady autorizace pro klíč hello.

Musíte také zásady doručení assetu tooconfigure hello. Pokud chcete toostream asset šifrované úložiště, ujistěte se, jak chcete, aby toospecify toodeliver ji tak, že nakonfigurujete zásady doručení assetu.

Když přehrávač vyžádá datový proud, služba Media Services použije hello zadané klíče toodynamically šifrování obsahu pomocí nezašifrovaného klíče AES nebo DRM šifrování. datový proud hello toodecrypt, hello player bude požadovat hello klíč z hello doručení klíče služby. toodecide, jestli je uživatel hello autorizovaný tooget hello klíč, hello služba vyhodnocuje hello zásady autorizace, které jste zadali pro klíč hello.


## <a name="storage-encryption"></a>Šifrování úložiště
Použít úložiště šifrování tooencrypt vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a nahrajte ho tooAzure úložiště, kde je uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku. Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure rest souborů vysoké kvality vstupními médii pomocí silného šifrování na disku.

Toodeliver order asset šifrované úložiště musíte nakonfigurovat zásady doručení assetu hello tak Media Services vědět, jak chcete toodeliver, obsah. Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení (například AES, běžným šifrováním nebo žádné šifrování).

## <a name="common-encryption-cenc"></a>Běžné šifrování (CENC)
Běžné šifrování se používá při šifrování svůj obsah pomocí PlayReady nebo / a Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Pomocí šifrování na cbcs-aapl
Cbcs-aapl se používá při šifrování svůj obsah pomocí FairPlay.

## <a name="envelope-encryption"></a>Šifrování obálky
Tuto možnost použijte, pokud chcete, aby tooprotect váš obsah s AES-128 nezašifrovaný klíč. Pokud chcete bezpečnější možnost, vyberte jednu z technologiemi DRM hello uvedené v tomto tématu. 

## <a name="licenses-and-keys-delivery-service"></a>Službu doručování licencí a klíče
Služba Media Services poskytuje službu k doručování licencí DRM (PlayReady, Widevine, FairPlay) a AES zrušte klíče tooauthorized klientů. Můžete použít [hello portál Azure](media-services-portal-protect-content.md), REST API nebo sady Media Services SDK pro zásady ověřování a ověřování tooconfigure .NET licence a klíčů.

## <a name="token-restriction"></a>Omezení s tokenem
Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevření nebo token omezení. zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS). Služba Media Services podporuje tokeny ve formátu hello jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT). Služba Media Services neposkytuje zabezpečení tokenu služby. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny. Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a vystavování deklarací identity, které jste zadali v konfiguraci omezení s tokenem hello. Hello Media Services, který vrátí doručení klíče služby hello požadovanou klíč (nebo licencí) toohello klienta, když je platný hello token a hello deklarace identity ve hello tokenu shoda ty nakonfigurované pro klíč hello (nebo licencí).

Při konfiguraci zásady omezení tokenem hello, je nutné zadat klíč hello primární ověřování, vystavitele a cílová skupina parametry. Hello ověření primární klíč obsahuje hello klíče, že byl podepsaný hello token, vystavitele hello zabezpečené tokenu služba, která vydá hello token. Cílová skupina Hello (někdy nazývané oboru) popisuje hello záměr hello tokenu nebo token hello prostředku hello povolí přístup k. Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.

## <a name="streaming-urls"></a>Adresy URL streamování
Pokud váš asset byla zašifrována pomocí více než jeden DRM, by měli používat značky šifrování v hello adresu URL streamování: (formát = 'm3u8-aapl' šifrování = 'xxx').

použít Hello následující aspekty:

* Lze zadat pouze nula nebo jeden typ šifrování.
* Typ šifrování nemá toobe zadaný v adrese url hello, pokud jenom jeden šifrování byl použité toohello asset.
* Typ šifrování je malá a velká písmena.
* určit lze následující typy šifrování Hello:  
  * **šifrování cenc**: Common encryption (Playready nebo Widevine)
  * **cbcs-aapl**: Fairplay
  * **CBC**: obálky šifrování AES.

## <a name="common-scenarios"></a>Obvyklé scénáře
Hello následující témata ukazují, jak tooprotect obsahu v úložišti, doručování dynamicky šifrovat streamování médií, použijte doručení klíč nebo licenční služby AMS

* [Ochrana pomocí standardu AES](media-services-protect-with-aes128.md) 
* [Chránit pomocí PlayReady nebo Widevine](media-services-protect-with-drm.md)
* [Stream obsahu chráněného vaše HLS Apple FairPlay a/nebo PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Další scénáře
* [Jak služba toointegrate licence PlayReady Azure s vlastní modul pro šifrování nebo streamování server](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [Pomocí castLabs toodeliver DRM licence tooAzure Media Services](media-services-castlabs-integration.md)

>[!NOTE]
>Scénář, ve kterém můžete používat externí DRM server(technology) a datový proud z AMS není aktuálně podporováno.


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Související odkazy
[Uvedení PlayReady jako dynamické šifrování AES pomocí Azure Media Services a služby](http://mingfeiy.com/playready)

[Vysvětlení Azure ceny doručování licencí Media Services PlayReady](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Jak toodebug AES šifrované datového proudu ve službě Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Authenitcation tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrace aplikace Azure Media Services OWIN MVC se službou Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Použití Azure ACS tooissue tokeny](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
