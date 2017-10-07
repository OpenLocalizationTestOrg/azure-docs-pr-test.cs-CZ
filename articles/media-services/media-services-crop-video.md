---
title: aaaHow toocrop videa s Media Encoder Standard - Azure | Microsoft Docs
description: "Tento článek ukazuje, jak toocrop videa pomocí procesoru Media Encoder Standard."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 7628f674-2005-4531-8b61-d7a4f53e46ba
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2017
ms.author: anilmur;juliako;
ms.openlocfilehash: 2b4ac3d96228b93c890a38c57c4913988de1e8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a>Oříznutí videa pomocí kodéru Media Encoder Standard
Toocrop Media Encoder Standard (MES) můžete použít váš vstup videa. Oříznutí je proces hello kódování právě hello pixelů v rámci dané okno a výběrem obdélníkový okno v rámci video hello. Následující diagram Hello pomáhá hello proces znázorněn.

![Oříznutí video](./media/media-services-crop-video/media-services-crop-video01.png)

Předpokládejme, že máte jako vstupní video, které má rozlišení 1920 × 1080 pixelů (poměr stran 16:9), ale má černé pruhy (pilíře polí) na hello vlevo a vpravo, tak, aby pouze okno 4:3 nebo 1440 × 1080 pixelů obsahuje aktivní video. Můžete použít MES toocrop nebo upravte si hello černým řádky a kódování hello 1440 × 1080 oblast.

Oříznutí v MES je předběžného zpracování fáze, abyste hello oříznutí parametry v kódování přednastavených hello použít toohello původní vstupní video. Kódování je další fázi a použít nastavení šířky a výšky hello toohello *předem zpracovaných* video a není toohello původní video. Při navrhování vaší přednastavených budete potřebovat následující hello toodo: (a) vyberte hello ořezové parametry podle hello původní vstupní video a (b) vyberte vaše kódování nastavení podle hello oříznout video. Pokud se neshodují vaše kódování toohello nastavení oříznout video, výstup hello nebudou podle očekávání.

Hello [následující](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) téma ukazuje, jak toocreate úlohu kódování s MES a jak toospecify vlastní předvolby pro hello kódování úloh. 

## <a name="creating-a-custom-preset"></a>Vytváření vlastní předvolby
Hello příkladu v diagramu hello:

1. Původní vstup je 1920 × 1080
2. Je nutné toobe oříznout tooan výstup 1440 × 1080, který je umístěn na střed hello vstupní rámce
3. To znamená posunem X (1920 – 1440) / 2 = 240 a Y posunutí nula
4. Hello šířky a výšky obdélníku ořezové hello jsou 1440 a 1080, v uvedeném pořadí
5. V hello kódování fázi hello požádat je tooproduce tři vrstvy, jsou rozlišení 1440 × 1080 960 × 720 a 480 x 360, v uvedeném pořadí

### <a name="json-preset"></a>Přednastavení JSON
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


## <a name="restrictions-on-cropping"></a>Omezení oříznutí
Hello oříznutí funkce je určená toobe ručně. Potřebovali byste tooload váš vstup videa v vhodný úpravy nástroj, který umožňuje vybrat rámců, které vás zajímají, umístěte hello kurzor toodetermine posunutí pro hello oříznutí obdélníku, toodetermine hello kódování přednastavení, která je přizpůsobená pro tuto konkrétní video, atd. Tato funkce není určena tooenable věcmi, jako jsou: automatické zjišťování a odebrání ohraničení černým letterbox/pillarbox v váš vstup videa.

Následující omezení platí toohello oříznutí funkce. Pokud nejsou splněny tyto, hello kódování úloh může selhat nebo vytvoření neočekávané výstupu.

1. Hello souřadnic a velikost obdélníku ořezové hello mít toofit v rámci hello vstupní video
2. Jak je uvedeno nahoře, hello šířka a výška v hello kódování nastavení mít toocorrespond toohello oříznout video
3. Oříznutí platí toovideos zaznamenat v režimu na šířku (tj. není k dispozici toovideos zaznamenávají s smartphone uchovávat svisle nebo v režimu na výšku)
4. Funguje nejlépe s progresivní video zachytit pomocí odmocnina pixelů

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Další krok
V tématu Azure Media Services učení toohelp cesty, které další informace o skvělé funkce, které nabízí AMS.  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
