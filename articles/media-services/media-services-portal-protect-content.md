---
title: "zásady ochrany obsahu aaaConfiguring pomocí hello portálu Azure | Microsoft Docs"
description: "Tento článek ukazuje, jak toouse hello Azure portálu tooconfigure zásady ochrany obsahu. Hello článku ukazuje také jak tooenable dynamického šifrování pro vaše prostředky."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 3e7ce6ddaa0e738b5a1e26dafe9eef2df221f039
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-content-protection-policies-using-hello-azure-portal"></a>Konfigurace zásad ochrany obsahu pomocí hello portálu Azure
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="overview"></a>Přehled
Microsoft Azure Media Services (AMS) umožňuje vám toosecure médiu z hello doby, kdy opouští váš počítač prostřednictvím úložiště, zpracování a doručení. Služba Media Services umožňuje toodeliver obsah šifrované dynamicky s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování), pomocí PlayReady nebo Widevine DRM a Apple FairPlay běžné šifrování (CENC). 

AMS poskytuje službu k doručování licencí DRM a AES zrušte klíče tooauthorized klientů. Hello portál Azure vám umožní toocreate jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.

Tento článek ukazuje, jak tooconfigure obsahu zásady ochrany s hello portálu Azure. Hello článku ukazuje také jak tooapply dynamického šifrování tooyour prostředky.


> [!NOTE]
> Pokud jste použili zásady ochrany Azure classic portálu toocreate hello, zásady hello nebude v hello [portál Azure](https://portal.azure.com/). Ale všechny hello staré zásady stále existují. Můžete je zkontrolovat pomocí hello Azure Media Services .NET SDK nebo hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) nástroj (toosee hello zásady, klikněte pravým tlačítkem na hello asset -> zobrazení informace (F4) -> klikněte na klíčů k obsahu -> karta Klikněte na klíč hello). 
> 
> Pokud chcete tooencrypt asset pomocí nové zásady, nakonfigurovat hello portálu Azure, klikněte na Uložit a znovu použít dynamické šifrování. 
> 
> 

## <a name="start-configuring-content-protection"></a>Zahájit konfiguraci ochrany obsahu
toouse hello portálu toostart Konfigurace ochrany obsahu, globální tooyour AMS účet hello následující:

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. Vyberte **nastavení** > **obsahu ochrany**.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Zásady autorizace pro klíč nebo licencí
AMS podporuje více způsobů ověřování uživatelů, kteří žádají o klíč nebo licencí. zásady autorizace klíče obsahu Hello musíte nakonfigurovat a splní vašeho klienta v pořadí pro hello klíč nebo licenci toobe delived toohello klienta. Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.

Hello portál Azure vám umožní toocreate jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.

### <a name="open"></a>Otevřenost
Otevřete omezení znamená, že hello systému dodá hello tooanyone klíče, který vytváří klíče požadavek. Toto omezení může být užitečná pro účely testování. 

### <a name="token"></a>Token
zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS). Služba Media Services podporuje tokeny ve formátu hello jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT). Služba Media Services neposkytuje zabezpečení tokenu služby. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny. Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a vystavování deklarací identity, které jste zadali v konfiguraci omezení s tokenem hello. Hello Media Services, který vrátí doručení klíče služby hello požadovanou klíč (nebo licencí) toohello klienta, když je platný hello token a hello deklarace identity ve hello tokenu shoda ty nakonfigurované pro klíč hello (nebo licencí).

Při konfiguraci zásady omezení tokenem hello, musíte zadat klíč hello primární ověření, vystavitele a cílová skupina parametry. Hello ověření primární klíč obsahuje hello klíče, že byl podepsaný hello token, vystavitele hello zabezpečené tokenu služba, která vydá hello token. Cílová skupina Hello (někdy nazývané oboru) popisuje hello záměr hello tokenu nebo token hello prostředku hello povolí přístup k. Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>Šablony práv PlayReady
Podrobné informace o šablony práv PlayReady hello najdete v tématu [Media Services PlayReady licence šablony přehled](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Bez trvalé
Pokud nakonfigurujete jako dočasnou licenci, pouze udržované ve paměti při hello player je pomocí hello licence.  

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Trvalé
Pokud konfigurujete hello licence jako trvalé, se uloží do trvalého úložiště na klientovi hello.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Šablony práv Widevine
Podrobné informace o šablony práv Widevine hello najdete v tématu [přehled šablonu licence Widevine](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Basic
Když vyberete **základní**, vytvoří se hello šablony s všechny výchozí hodnoty.

### <a name="advanced"></a>Advanced
Podrobné vysvětlení o – možnost zálohy Widevine konfigurací najdete v tématu [to](media-services-widevine-license-template-overview.md) tématu.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Konfigurace FairPlay
tooenable FairPlay šifrování, potřebujete tooprovide hello certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím hello možnost FairPlay konfigurace. Podrobné informace o konfiguraci FairPlay a požadavky najdete v tématu [to](media-services-protect-hls-with-fairplay.md) článku.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-tooyour-asset"></a>Použití dynamické šifrování tooyour asset
tootake využívat dynamické šifrování, je nutné tooencode zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.

### <a name="select-an-asset-that-you-want-tooencrypt"></a>Vyberte prostředek, který má tooencrypt
Vyberte všechny vaše prostředky toosee **nastavení** > **prostředky**.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Šifrování AES nebo DRM
Po stisknutí klávesy **šifrovat** na prostředek, se zobrazí dvě možnosti: **AES** nebo **DRM**. 

#### <a name="aes"></a>AES
AES zrušte Povolí šifrování pomocí klíče na všechny protokoly streamování: technologie Smooth Streaming, HLS a MPEG-DASH.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Když vyberete kartu hello DRM, budete mít různé možnosti zásad ochrany obsahu (který musíte nakonfigurovat nyní) + sadu protokolů streamování.

* **PlayReady a Widevine s MPEG-DASH** -bude dynamicky šifrovat MPEG-DASH datového proudu s technologií PlayReady a Widevine technologiemi DRM.
* **PlayReady a Widevine s MPEG-DASH + FairPlay s HLS** -bude dynamicky šifrovat stream MPEG-DASH s technologií PlayReady a Widevine technologiemi DRM. Bude se šifrovat taky vaše datové proudy HLS s FairPlay.
* **PlayReady pouze s technologie Smooth Streaming, HLS a MPEG-DASH** -bude dynamicky šifrovat technologie Smooth Streaming, HLS, datové proudy MPEG-DASH s technologií PlayReady DRM.
* **Widevine pouze s MPEG-DASH** -bude dynamicky šifrovat můžete MPEG-DASH pomocí Widevine DRM.
* **FairPlay pouze s HLS** -bude dynamicky šifrovat HLS datového proudu s FairPlay.

tooenable FairPlay šifrování, potřebujete tooprovide hello certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím hello možnost konfigurace FairPlay okna nastavení ochrany obsahu hello.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Až provedete výběr hello šifrování, stiskněte klávesu **použít**.

>[!NOTE] 
>Pokud plánujete tooplay AES šifrovat HLS v prohlížeči Safari najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

