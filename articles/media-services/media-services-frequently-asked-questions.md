---
title: "aaaAzure Media Services nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy (FAQ)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 6d48a5c1291f3c2559d8445921d571718d0a0a6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions"></a>Nejčastější dotazy

Tento článek řeší nejčastější dotazy aktivováno komunit uživatelů hello Azure Media Services (AMS).

## <a name="general-ams-faqs"></a>Nejčastější dotazy k AMS obecné
Otázka: jak můžete škálovat indexování?

Odpověď: hello vyhrazené jednotky jsou hello stejné pro kódování a indexování úlohy. Postupujte podle pokynů na [jak tooScale jednotky rezervované pro kódování](media-services-scale-media-processing-overview.md). **Poznámka:** Indexer výkonu není ovlivněn vyhrazený typ jednotky.

Otázka: I nahrán, kódování a publikování videa. Co by hello důvod hello video nehraje při toostream ho?

Odpověď: jeden z hello nejčastějším důvodů, proč je nemáte hello koncový bod, ze kterého se pokoušíte tooplayback v hello streamování **systémem** stavu.  

Otázka: je možné dělat skládání na živý datový proud?

A: skládání na datových proudů za provozu není aktuálně nabízí ve službě Azure Media Services, měli byste toopre-tvoří ve vašem počítači.

Otázka: je možné použít Azure CDN s živým streamováním?

Odpověď: Media Services podporuje integraci se službou Azure CDN (Další informace najdete v tématu [jak tooManage koncových bodů streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)).  Můžete použít živé streamování s CDN. Azure Media Services poskytuje technologie Smooth Streaming, MPEG-DASH v HLS výstupy. Všechny tyto formáty prostřednictvím protokolu HTTP pro přenos dat a získejte výhody ukládání do mezipaměti protokolu HTTP. V za provozu, streamování skutečná data video nebo zvuk je rozdělený toofragments a tento jednotlivé fragmenty získat uložené v mezipaměti v CDN. Pouze data potřebám toobe aktualizovat jsou hello manifestu data. CDN se pravidelně aktualizuje manifestu data.

Otázka: nemá Azure Media services podporuje ukládání Image?

Odpověď: Pokud hledáte pouze toostore JPEG nebo PNG obrázků, byste měli mít v Azure Blob Storage. Neexistuje žádné tooputting výhody, které služby Media Services je účet, pokud chcete, aby tookeep, který je přidružený k Video nebo zvuk prostředky. Nebo pokud bude možná potřeba toouse hello bitové kopie jako překryvy v kodér videa hello. Media Encoder Standard podporuje překrytí bitové kopie nad videa, a co obsahuje JPEG a PNG jako podporovaný vstupní formáty. Další informace najdete v tématu [vytváření překryvy](media-services-advanced-encoding-with-mes.md#overlay).

Otázka: jak můžete zkopírovat prostředků z jednoho tooanother účtu Media Services.

Odpověď: toocopy prostředků z jednoho tooanother účtu Media Services pomocí rozhraní .NET, použijte [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) rozšíření metoda dostupná ve hello [rozšíření Azure Media Services .NET SDK](https://github.com/Azure/azure-sdk-for-media-services-extensions/) úložiště. Další informace najdete v tématu [to](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum pro přístup z více vláken.

Otázka: co jsou hello podporovány znaků pro pojmenovávání souborů při práci s AMS?

Odpověď: Media Services použije hodnotu hello hello vlastnost IAssetFile.Name při sestavování adresy URL pro hello streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech. Hello hodnotu hello **název** vlastnost nemůže mít žádné z následujících hello [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Navíc může existovat pouze jedna '. " pro hello příponu názvu souboru.

Otázka: pomocí tooconnect jak REST?

Odpověď: Další informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI. 

Otázka: jak můžete otočit video během hello kódování procesu.

Odpověď: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje oběh podle úhly 90, 180 nebo 270. Hello výchozí chování je "Auto", kde se pokusí toodetect hello otočení metadata v souboru MP4 nebo MOV příchozí hello a kompenzovat ho. Zahrnout hello následující **zdroje** element tooone přednastavení json hello definované [sem](media-services-mes-presets-overview.md):

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
