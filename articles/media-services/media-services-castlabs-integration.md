---
title: aaaUsing castLabs toodeliver Widevine licence tooAzure Media Services | Microsoft Docs
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) toodeliver datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. licence PlayReady Hello pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje castLabs licenční server."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 80d2778fb283a96361e7e511990a36c2f551a310
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-castlabs-toodeliver-widevine-licenses-tooazure-media-services"></a>Pomocí castLabs toodeliver Widevine licence tooAzure Media Services
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Přehled
Tento článek popisuje, jak můžete použít Azure Media Services (AMS) toodeliver datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. licence PlayReady Hello pochází z Media Services PlayReady licenční server a je doručil licence Widevine **castLabs** licenční server.

tooplayback streamování obsahu chráněného CENC (PlayReady nebo Widevine), můžete pomocí [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). V tématu [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) podrobnosti.

Hello následující diagram ukazuje základní Architektura integrace Azure Media Services a castLabs.

![Integrace](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Typických systémových nastavení
* Mediální obsah uložený v AMS.
* Klíč ID obsahu klíče jsou uloženy v castLabs i AMS.
* castLabs a AMS mají součástí ověření tokenu. Hello následující oddíly popisují tokeny ověřování. 
* Když klient požádá o toostream hello video, je obsah hello dynamicky šifrován **Common Encryption** (CENC) a dynamicky zabalené AMS tooSmooth, Streaming a POMLČKY. Můžeme také poskytovat šifrování PlayReady M2TS základní stream pro protokol pro streamování HLS.
* Licence PlayReady se načítají AMS licenčního serveru a licence Widevine se načítají z castLabs licenční server. 
* Přehrávač médií automaticky rozhoduje, které toofetch licencí na základě funkcí platformy hello klienta. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Generování tokenů ověřování pro získání licence
CastLabs i AMS podporují tooauthorize používá formát tokenu JWT (JSON Web Token) licenci. 

### <a name="jwt-token-in-ams"></a>Token JWT v AMS
Hello následující tabulka popisuje token JWT v AMS. 

| Vystavitel | Řetězec vystavitele z hello vybrali zabezpečení tokenu služby (STS) |
| --- | --- |
| Cílová skupina |Cílová skupina řetězec z hello používá službu tokenů zabezpečení |
| Deklarace identity |Sadu deklarací identity |
| Neplatí před |Zahájení platnosti tokenu hello |
| Vypršení platnosti |End platnosti tokenu hello |
| SigningCredentials |Hello klíč, který je sdílen PlayReady licenční Server castLabs licenční Server a službu tokenů zabezpečení, může to být buď symetrický, nebo asymetrický klíč. |

### <a name="jwt-token-in-castlabs"></a>Token JWT v castLabs
Hello následující tabulka popisuje token JWT v castLabs. 

| Name (Název) | Popis |
| --- | --- |
| optData |Řetězec formátu JSON, který obsahuje informace o vás. |
| CRT |A JSON řetězec obsahující informace o hello asset, jeho licenční informace a přehrávání práva. |
| IAT |Hello aktuální datum a čas v epoch. |
| jti |Jedinečný identifikátor o tento token (každý token může být použit pouze jednou v systému castLabs hello). |

## <a name="sample-solution-set-up"></a>Nastavení ukázkové řešení
Hello [ukázkové řešení](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) se skládá ze dvou projektů:

* Konzolovou aplikaci, kterou lze použít tooset omezení prostředek už ingestovaný DRM PlayReady a Widevine.
* Webovou aplikaci, která předá se tokeny, které může považovat za velmi zjednodušené verzi služby tokenů zabezpečení.

toouse hello konzolové aplikace:

1. Změnit pověření toosetup AMS hello app.config, castLabs přihlašovací údaje, konfigurace služby tokenů zabezpečení a sdílený klíč.
2. Odešlete prostředek do AMS.
3. Get hello UUID z hello nahrát Asset a změňte 32 řádku v souboru Program.cs hello:
   
      var objIAsset = _kontextu. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1 500-80bd-b864-f1e4b62594cf"). FirstOrDefault();
4. Použijte ID pro pojmenování hello asset systému castLabs hello (44 řádku v souboru Program.cs hello).
   
   Je nutné nastavit ID pro **castLabs**; je nutné toobe jedinečný alfanumerický řetězec.
5. Spuštění programu hello.

toouse hello webové aplikace (STS):

1. Změna hello web.config toosetup castlabs obchodní ID, konfigurace služby tokenů zabezpečení hello a hello sdílený klíč.
2. Nasaďte tooAzure weby.
3. Přejděte toohello webu.

## <a name="playing-back-a-video"></a>Přehrávání videa
tooplayback video šifrován běžné šifrování (PlayReady nebo Widevine), můžete použít hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Když spustíte hello konzolovou aplikaci, jsou opakována hello ID obsahu klíč a hello Manifest adresy URL.

1. Otevřete novou kartu a spusťte vaší služby tokenů zabezpečení: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Přejděte příliš[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. Vložte adresu URL streamování hello.
4. Klikněte na tlačítko hello **pokročilé možnosti** zaškrtávací políčko.
5. V hello **ochrany** rozevíracího seznamu vyberte PlayReady nebo Widevine.
6. Vložte hello token, který jste dostali od vaší služby tokenů zabezpečení v textovém poli hello Token. 
   
   Hello castLab licenční server nemusí hello "nosiče =" předponu před hello token. Proto prosím odebrat před odesláním hello token.
7. Aktualizujte hello přehrávač.
8. měli přehrávání videa Hello.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

