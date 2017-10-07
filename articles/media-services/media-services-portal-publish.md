---
title: "AAA\"publikování obsahu pomocí hello portálu Azure | Microsoft Docs\""
description: "Tento kurz vás provede kroky hello publikování svůj obsah pomocí hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a7a3867a6939b4b9da883176c6cc20c99d6c54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-content-with-hello-azure-portal"></a>Publikovat obsah s hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Přehled
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

tooprovide uživatelů s adresou URL, která se dá použít toostream nebo stažení vašeho obsahu, je nejprve nutné příliš "publikovat" asset vytvořením lokátoru. Lokátory zajišťují přístup toofiles obsažené v hello asset. Služba Media Services podporuje dva typy lokátorů: 

* Streamování (OnDemandOrigin) lokátory, používají pro adaptivní streamování (například toostream MPEG DASH, HLS nebo technologie Smooth Streaming). toocreate Lokátor streamování váš asset musí obsahovat soubor .ism. 
* Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.

Adresu URL streamování má následující formát hello a můžete ji použít tooplay assetů technologie Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

připojit toobuild na adresu URL, streamování HLS (format = m3u8-aapl) toohello adresy URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

připojit toobuild na adresu URL, streamování MPEG DASH (formát = mpd. čas csf) toohello adresy URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Adresa URL typu SAS má následující formát hello.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Další informace najdete v tématu [doručování obsahu přehled](media-services-deliver-content-overview.md).

> [!NOTE]
> Pokud jste použili hello portálu toocreate lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.  
> 
> 

tooupdate na datum vypršení platnosti lokátoru, použijte [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) rozhraní API. Všimněte si, že při aktualizaci hello datum vypršení platnosti lokátoru SAS hello se změní adresa URL.

### <a name="toouse-hello-portal-toopublish-an-asset"></a>toouse hello portálu toopublish prostředek
toouse hello portálu toopublish prostředek, hello následující:

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. Vyberte **Nastavení** > **Assety**.
3. Vyberte, které chcete toopublish asset hello.
4. Klikněte na tlačítko hello **publikovat** tlačítko.
5. Vyberte typ lokátoru hello.
6. Stiskněte **Přidat**.
   
    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adresa URL Hello přidá toohello seznam **publikovaných adres URL**.

## <a name="play-content-from-hello-portal"></a>Přehrávání obsahu z portálu hello
Hello portál Azure nabízí přehrávač obsahu, které můžete použít tootest videa.

Klikněte na tlačítko hello požadovaného video a potom klikněte na hello **přehrání** tlačítko.

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

Musí být splněny určité předpoklady:

* Zajistěte, aby byla publikována hello videa.
* To **přehrávač médií** přehrává z výchozího hello koncového bodu streamování. Pokud chcete, aby tooplay z jiného než výchozího koncového bodu, streamování klikněte toocopy hello adresu URL a použijte jiný přehrávač. Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
* koncový bod, ze kterého jsou datové proudy streamování Hello musí být spuštěna.  

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

