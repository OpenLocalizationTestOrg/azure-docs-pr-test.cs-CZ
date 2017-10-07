---
title: "Přehled dynamického balení aaaAzure Media Services | Microsoft Docs"
description: "Přehled dynamického balení a poskytuje tématu Hello."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 970e24eba800e098774172c87f56629430b227a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-packaging"></a>Dynamické balení
## <a name="overview"></a>Přehled
Microsoft Azure Media Services lze použít toodeliver mnoho média zdrojového formáty souborů, formátů streamování médií a ochrana obsahu formáty tooa různých technologií, klienta (například iOS, XBOX, Silverlight, Windows 8). Tito klienti pochopit různé protokoly, například vyžaduje formátu V4 HTTP Live Streaming (HLS), iOS a Silverlight a Xbox vyžadují, technologie Smooth Streaming. Pokud máte sadu s adaptivní přenosovou rychlostí (více přenosovými rychlostmi) MP4 soubory (ISO základní médium 14496-12) nebo sady souborů s adaptivní přenosovou rychlostí technologie Smooth Streaming, které chcete tooclients tooserve, který pochopit MPEG DASH, HLS nebo technologie Smooth Streaming, byste měli vzít výhod média Dynamické balení služby.

S dynamické balení, stačí je toocreate asset, který obsahuje sadu souborů MP4 nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí. Pak na základě zadaného formátu hello v manifestu hello nebo fragmentovat požadavku, hello streamování na vyžádání server zajistí zobrazí datový proud hello hello protokolu, kterou jste vybrali. V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.

Hello následující diagram znázorňuje hello tradiční kódování a statické balení pracovního postupu.

![Statické kódování](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Hello následující diagram znázorňuje hello dynamické balení pracovního postupu.

![Dynamické kódování](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a>Běžný scénář
1. Nahrajte vstupní soubor (nazývané soubor mezzanine). Například H.264, MP4 nebo WMV (hello seznam podporovaných formátů naleznete v části [formáty nepodporuje hello Media Encoder Standard](media-services-media-encoder-standard-formats.md).
2. Zakódujte váš soubor mezzanine souboru tooH.264 MP4 s adaptivní přenosovou rychlostí sady.
3. Publikujte hello asset, který obsahuje hello s adaptivní přenosovou rychlostí sady souborů MP4 vytvořením hello Lokátor na vyžádání.
4. Sestavte hello tooaccess adresy URL streamování a Streamovat obsah.

## <a name="preparing-assets-for-dynamic-streaming"></a>Příprava prostředky pro dynamické streamování
tooprepare asset pro dynamické vysílání datového proudu je k dispozici dvě možnosti:

1. [Nahrát soubor hlavní](media-services-dotnet-upload-files.md).
2. [Použít hello Media Encoder Standard kodér tooproduce H.264 MP4 s adaptivní přenosovou rychlostí sady](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Streamovat obsah](media-services-deliver-content-overview.md).

## <a id="unsupported_formats"></a>Formáty, které nepodporuje dynamické balení
Hello následující formáty souborů zdroje nepodporuje dynamické balení.

* Soubory mp4 digitální Dolby.
* Digitální soubory technologie smooth Dolby.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

