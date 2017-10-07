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
# <a name="dynamic-packaging"></a><span data-ttu-id="aadf2-103">Dynamické balení</span><span class="sxs-lookup"><span data-stu-id="aadf2-103">Dynamic packaging</span></span>
## <a name="overview"></a><span data-ttu-id="aadf2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="aadf2-104">Overview</span></span>
<span data-ttu-id="aadf2-105">Microsoft Azure Media Services lze použít toodeliver mnoho média zdrojového formáty souborů, formátů streamování médií a ochrana obsahu formáty tooa různých technologií, klienta (například iOS, XBOX, Silverlight, Windows 8).</span><span class="sxs-lookup"><span data-stu-id="aadf2-105">Microsoft Azure Media Services can be used toodeliver many media source file formats, media streaming formats, and content protection formats tooa variety of client technologies (for example, iOS, XBOX, Silverlight, Windows 8).</span></span> <span data-ttu-id="aadf2-106">Tito klienti pochopit různé protokoly, například vyžaduje formátu V4 HTTP Live Streaming (HLS), iOS a Silverlight a Xbox vyžadují, technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="aadf2-106">These clients understand different protocols, for example iOS requires an HTTP Live Streaming (HLS) V4 format and Silverlight and Xbox require Smooth Streaming.</span></span> <span data-ttu-id="aadf2-107">Pokud máte sadu s adaptivní přenosovou rychlostí (více přenosovými rychlostmi) MP4 soubory (ISO základní médium 14496-12) nebo sady souborů s adaptivní přenosovou rychlostí technologie Smooth Streaming, které chcete tooclients tooserve, který pochopit MPEG DASH, HLS nebo technologie Smooth Streaming, byste měli vzít výhod média Dynamické balení služby.</span><span class="sxs-lookup"><span data-stu-id="aadf2-107">If you have a set of adaptive bitrate (multi-bitrate) MP4 (ISO Base Media 14496-12) files or a set of adaptive bitrate Smooth Streaming files that you want tooserve tooclients that understand MPEG DASH, HLS or Smooth Streaming, you should take advantage of Media Services dynamic packaging.</span></span>

<span data-ttu-id="aadf2-108">S dynamické balení, stačí je toocreate asset, který obsahuje sadu souborů MP4 nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="aadf2-108">With dynamic packaging all you need is toocreate an asset that contains a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="aadf2-109">Pak na základě zadaného formátu hello v manifestu hello nebo fragmentovat požadavku, hello streamování na vyžádání server zajistí zobrazí datový proud hello hello protokolu, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="aadf2-109">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="aadf2-110">V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="aadf2-110">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="aadf2-111">Hello následující diagram znázorňuje hello tradiční kódování a statické balení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="aadf2-111">hello following diagram shows hello traditional encoding and static packaging workflow.</span></span>

![Statické kódování](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

<span data-ttu-id="aadf2-113">Hello následující diagram znázorňuje hello dynamické balení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="aadf2-113">hello following diagram shows hello dynamic packaging workflow.</span></span>

![Dynamické kódování](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a><span data-ttu-id="aadf2-115">Běžný scénář</span><span class="sxs-lookup"><span data-stu-id="aadf2-115">Common scenario</span></span>
1. <span data-ttu-id="aadf2-116">Nahrajte vstupní soubor (nazývané soubor mezzanine).</span><span class="sxs-lookup"><span data-stu-id="aadf2-116">Upload an input file (called a mezzanine file).</span></span> <span data-ttu-id="aadf2-117">Například H.264, MP4 nebo WMV (hello seznam podporovaných formátů naleznete v části [formáty nepodporuje hello Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span><span class="sxs-lookup"><span data-stu-id="aadf2-117">For example, H.264, MP4, or WMV (for hello list of supported formats see [Formats Supported by hello Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span></span>
2. <span data-ttu-id="aadf2-118">Zakódujte váš soubor mezzanine souboru tooH.264 MP4 s adaptivní přenosovou rychlostí sady.</span><span class="sxs-lookup"><span data-stu-id="aadf2-118">Encode your mezzanine file tooH.264 MP4 adaptive bitrate sets.</span></span>
3. <span data-ttu-id="aadf2-119">Publikujte hello asset, který obsahuje hello s adaptivní přenosovou rychlostí sady souborů MP4 vytvořením hello Lokátor na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="aadf2-119">Publish hello asset that contains hello adaptive bitrate MP4 set by creating hello On-Demand Locator.</span></span>
4. <span data-ttu-id="aadf2-120">Sestavte hello tooaccess adresy URL streamování a Streamovat obsah.</span><span class="sxs-lookup"><span data-stu-id="aadf2-120">Build hello streaming URLs tooaccess and stream your content.</span></span>

## <a name="preparing-assets-for-dynamic-streaming"></a><span data-ttu-id="aadf2-121">Příprava prostředky pro dynamické streamování</span><span class="sxs-lookup"><span data-stu-id="aadf2-121">Preparing assets for dynamic streaming</span></span>
<span data-ttu-id="aadf2-122">tooprepare asset pro dynamické vysílání datového proudu je k dispozici dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="aadf2-122">tooprepare your asset for dynamic streaming you have two options:</span></span>

1. <span data-ttu-id="aadf2-123">[Nahrát soubor hlavní](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="aadf2-123">[Upload a master file](media-services-dotnet-upload-files.md).</span></span>
2. <span data-ttu-id="aadf2-124">[Použít hello Media Encoder Standard kodér tooproduce H.264 MP4 s adaptivní přenosovou rychlostí sady](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="aadf2-124">[Use hello Media Encoder Standard encoder tooproduce H.264 MP4 adaptive bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>
3. <span data-ttu-id="aadf2-125">[Streamovat obsah](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aadf2-125">[Stream your content](media-services-deliver-content-overview.md).</span></span>

## <span data-ttu-id="aadf2-126"><a id="unsupported_formats"></a>Formáty, které nepodporuje dynamické balení</span><span class="sxs-lookup"><span data-stu-id="aadf2-126"><a id="unsupported_formats"></a>Formats that are not supported by dynamic packaging</span></span>
<span data-ttu-id="aadf2-127">Hello následující formáty souborů zdroje nepodporuje dynamické balení.</span><span class="sxs-lookup"><span data-stu-id="aadf2-127">hello following source file formats are not supported by dynamic packaging.</span></span>

* <span data-ttu-id="aadf2-128">Soubory mp4 digitální Dolby.</span><span class="sxs-lookup"><span data-stu-id="aadf2-128">Dolby digital mp4 files.</span></span>
* <span data-ttu-id="aadf2-129">Digitální soubory technologie smooth Dolby.</span><span class="sxs-lookup"><span data-stu-id="aadf2-129">Dolby digital smooth files.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="aadf2-130">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="aadf2-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="aadf2-131">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="aadf2-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

