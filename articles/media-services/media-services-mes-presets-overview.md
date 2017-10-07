---
title: "Přednastavení aaaTask pro MES (Media Encoder Standard) | Microsoft Docs"
description: "téma nabízí Hello a přehled přednastavení úloh pro MES (Media Encoder Standard)."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 56e098d6d8c8f84031421ec59f087f20370ba111
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="task-presets-for-mes-media-encoder-standard"></a>Přednastavení úloh pro MES (Media Encoder Standard)

**Media Encoder Standard** definuje sadu kódování přednastavení můžete použít při vytváření úlohy kódování. Doporučujeme toouse hello "Adaptivní datové proudy" předvolby, pokud chcete tooencode video pro streamování pomocí služby Media Services. Pokud zadáte přednastavené, budou Media Encoder Standard [automaticky generovat žebříku přenosovou rychlostí](media-services-autogen-bitrate-ladder-with-mes.md). 

Ale pokud budete potřebovat toocustomize předvolby kódování, byste měli vzít jeden hello kódování přednastavení definované v této části jako šablona pro vaši vlastní konfiguraci. Vysvětlení co každý prvek v těchto přednastavení znamená a hello platné hodnoty pro každý element, najdete v části hello [Media Encoder Standard schématu](media-services-mes-schema.md) tématu.  
  
> [!NOTE]
>  Pokud používáte jedno z přednastavení pro kóduje 4k, měli byste obdržet hello `S3` vyhrazený typ jednotky. Další informace najdete v tématu [jak tooScale kódování](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).  
  
Při práci s Media Encoder Standard, otočení ve výchozím nastavení zapnutá. Pokud byla zaznamenána videa na smartphone nebo jiného mobilního zařízení v režimu na výšku, pak tato přednastavení bude ve výchozím nastavení, jejich otočení tooLandscape režimu předchozí tooencoding (na rozdíl od, když pracujete s Azure Media Encoder, kde je video otočení ruční operace, jak je uvedeno v [to](http://azure.microsoft.com/blog/2014/08/21/advanced-encoding-features-in-azure-media-encoder/) blog, v části "Video otočení").  
  
K dispozici přednastavení:  
  
 [H264 Multiple Bitrate 1080p zvuk 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) vytvoří sadu 8 soubory MP4 zarovnaný GOP od 6000 kb/s too400 kb/s a zvuku AAC 5.1.  
  
 [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) vytvoří sadu 8 soubory MP4 zarovnaný GOP od 6000 kb/s too400 kb/s a stereofonní zvuk AAC.  
  
 [H264 Multiple Bitrate 16 x 9 pro iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) vytvoří sadu 8 soubory MP4 zarovnaný GOP od 8500 kb/s too200 kb/s a stereofonní zvuk AAC.  
  
 [Zvuk SD H264 Multiple Bitrate 16 x 9 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) vytvoří sadu 5 soubory MP4 zarovnaný GOP od 1900 kb/s too400 kb/s a zvuku AAC 5.1.  
  
 [H264 Multiple Bitrate 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) vytvoří sadu 5 soubory MP4 zarovnaný GOP od 1900 kb/s too400 kb/s a stereofonní zvuk AAC.  
  
 [Zvuk H264 Multiple Bitrate 4K 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) vytvoří sadu 12 souborů zarovnaný GOP MP4, od kb/s too1000 20000 kb/s a zvuku AAC 5.1.  
  
 [H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) vytvoří sadu 12 soubory MP4 zarovnaný GOP od kb/s too1000 20000 kb/s a stereofonní zvuk AAC.  
  
 [H264 Multiple Bitrate 4 x 3 pro iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) vytvoří sadu 8 soubory MP4 zarovnaný GOP od 8500 kb/s too200 kb/s a stereofonní zvuk AAC.  
  
 [Zvuk SD H264 Multiple Bitrate 4 x 3 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) vytvoří sadu 5 souborů zarovnaný GOP MP4, od 1 600 kb/s too400 kb/s a zvuku AAC 5.1.  
  
 [H264 Multiple Bitrate 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) vytvoří sadu 5 soubory MP4 zarovnaný GOP od 1 600 kb/s too400 kb/s a stereofonní zvuk AAC.  
  
 [Zvuk 720p H264 Multiple Bitrate 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) vytvoří sadu 6 soubory MP4 zarovnaný GOP od 3400 kb/s too400 kb/s a zvuku AAC 5.1.  
  
 [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) vytvoří sadu 6 soubory MP4 zarovnaný GOP od 3400 kb/s too400 kb/s a stereofonní zvuk AAC.  
  
 [Jednou přenosovou rychlostí H264 1080p zvuk 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 6750 kb/s a zvuku AAC 5.1.  
  
 [H264 jeden Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 6750 kb/s a stereofonní zvuk AAC.  
  
 [Zvuk H264 jednou přenosovou rychlostí 4K 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 18000 kb/s a zvuku AAC 5.1.  
  
 [H264 jednou přenosovou rychlostí 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 18000 kb/s a stereofonní zvuk AAC.  
  
 [Zvuk SD H264 jednou přenosovou rychlostí 4 x 3 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 1 800 kB/s a zvuku AAC 5.1.  
  
 [H264 jednou přenosovou rychlostí 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 1 800 kB/s a stereofonní zvuk AAC.  
  
 [Zvuk SD H264 jednou přenosovou rychlostí 16 x 9 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 2200 kb/s a zvuku AAC 5.1.  
  
 [H264 jednou přenosovou rychlostí 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 2200 kb/s a stereofonní zvuk AAC.  
  
 [Jednou přenosovou rychlostí H264 720p zvuk 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 4500 kb/s a zvuku AAC 5.1.  
  
 [H264 jednou přenosovou rychlostí 720p pro Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) přednastavených vytváří jednoho souboru MP4 s přenosovou rychlostí 2000 kb/s a stereo AAC.  
  
 [H264 jednou přenosovou rychlostí 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 4500 kb/s a stereofonní zvuk AAC.  
  
 [H264 jeden přenosovou rychlostí vysoké kvality SD pro Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 500 kb/s a stereofonní zvuk AAC...  
  
 [H264 jeden přenosovou rychlostí nižší kvalita SD pro Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) vytváří jednoho souboru MP4 s přenosovou rychlostí 56 kb/s a stereofonní zvuk AAC.  
  
 Další informace najdete v části služby kodéry související tooMedia, [kódování na vyžádání pomocí Azure Media Services](https://azure.microsoft.com/en-us/documentation/articles/media-services-encode-asset/).
