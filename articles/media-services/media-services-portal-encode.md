---
title: "aaaEncode assetu pomocí kodéru Media Encoder Standard s hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello kódování assetu pomocí kodéru Media Encoder Standard s hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 0d118bbbe1fa9f4ba0bfa3ea3b10fb541d1d6379
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-hello-azure-portal"></a>Kódování assetu pomocí kodéru Media Encoder Standard s hello portálu Azure
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Při práci se službou Azure Media Services je jedním nejběžnější scénářů hello doručování tooyour klienti streamování s adaptivní přenosovou rychlostí. Služba Media Services podporuje následující adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH. tooprepare videa pro streamování s adaptivní přenosovou rychlostí, je nutné tooencode svůj zdroj videa do souborů s více přenosovými rychlostmi. Měli byste použít hello **Media Encoder Standard** tooencode kodér videa.  

Služba Media Services také poskytuje dynamické balení, což vám umožní toodeliver vaše soubory MP4 více přenosovými rychlostmi v hello následující streamování formáty: MPEG DASH, HLS, technologie Smooth Streaming, aniž byste museli toore balíček do těchto formátů streamování. Při dynamickém balení stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.

tootake výhod dynamického balení, je nutné tooencode zdrojového souboru do sady souborů MP4 s více přenosovými rychlostmi (postup hello kódování je ukázán později v této části).

média tooscale zpracování, najdete v části [to](media-services-portal-scale-media-processing.md) tématu.

## <a name="encode-with-hello-azure-portal"></a>Kódování s hello portálu Azure
Tato část popisuje kroky hello tooencode může trvat svůj obsah pomocí procesoru Media Encoder Standard.

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. V hello **nastavení** vyberte **prostředky**.  
3. V hello **prostředky** okno, vyberte hello asset, které chcete tooencode.
4. Stiskněte klávesu hello **kódovat** tlačítko.
5. V hello **kódovat asset** oken, vyberte hello procesor "Media Encoder Standard" a jedno z přednastavení. Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md). Pokud máte v plánu toocontrol které předvolby kódování se používá, mějte na paměti: je důležité tooselect hello přednastavení, která je nejvhodnější pro vaše vstupní video. Například pokud znáte vaše vstupní video má rozlišení 1920 × 1080 pixelů, pak můžete použít hello "H264 Multiple Bitrate 1080p" přednastavené. Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.
   
   Pro snadnější správu máte možnost úpravy hello název hello výstupní asset a název hello hello úlohy.
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Stiskněte **Vytvořit**.

## <a name="next-step"></a>Další krok
Průběhu úlohy kódování s hello portál Azure, můžete sledovat, jak je popsáno v [to](media-services-portal-check-job-progress.md) článku.  

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

