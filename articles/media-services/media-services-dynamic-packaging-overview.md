---
title: "Přehled dynamického balení Azure Media Services | Microsoft Docs"
description: "Téma nabízí a přehled dynamického balení."
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
ms.openlocfilehash: 2d212599302fced3f60085ab30cdeaefc1ee2e6a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="dynamic-packaging"></a><span data-ttu-id="32b8a-103">Dynamické balení</span><span class="sxs-lookup"><span data-stu-id="32b8a-103">Dynamic packaging</span></span>
## <a name="overview"></a><span data-ttu-id="32b8a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="32b8a-104">Overview</span></span>
<span data-ttu-id="32b8a-105">Ochrana obsahu formáty do různých technologií, klienta nebo Microsoft Azure Media Services umožňuje doručovat mnoho média zdrojového formáty souborů, formátů streamování médií (například iOS, XBOX, Silverlight, Windows 8).</span><span class="sxs-lookup"><span data-stu-id="32b8a-105">Microsoft Azure Media Services can be used to deliver many media source file formats, media streaming formats, and content protection formats to a variety of client technologies (for example, iOS, XBOX, Silverlight, Windows 8).</span></span> <span data-ttu-id="32b8a-106">Tito klienti pochopit různé protokoly, například vyžaduje formátu V4 HTTP Live Streaming (HLS), iOS a Silverlight a Xbox vyžadují, technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="32b8a-106">These clients understand different protocols, for example iOS requires an HTTP Live Streaming (HLS) V4 format and Silverlight and Xbox require Smooth Streaming.</span></span> <span data-ttu-id="32b8a-107">Pokud máte sadu s adaptivní přenosovou rychlostí (více přenosovými rychlostmi) MP4 soubory (ISO základní médium 14496-12) nebo sadu technologie Smooth Streaming souborů s adaptivní přenosovou rychlostí, která má sloužit ke klientům, kteří pochopit MPEG DASH, HLS nebo technologie Smooth Streaming, byste měli vzít výhod média Dynamické balení služby.</span><span class="sxs-lookup"><span data-stu-id="32b8a-107">If you have a set of adaptive bitrate (multi-bitrate) MP4 (ISO Base Media 14496-12) files or a set of adaptive bitrate Smooth Streaming files that you want to serve to clients that understand MPEG DASH, HLS or Smooth Streaming, you should take advantage of Media Services dynamic packaging.</span></span>

<span data-ttu-id="32b8a-108">Při dynamickém balení, stačí je vytvoření asset, který obsahuje sadu souborů MP4 nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="32b8a-108">With dynamic packaging all you need is to create an asset that contains a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="32b8a-109">Pak na základě formátu určeného v manifestu nebo fragmentu požadavek streamingu na vyžádání serveru zajistí datový proud obdrželi v protokolu, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="32b8a-109">Then, based on the specified format in the manifest or fragment request, the On-Demand Streaming server will ensure that you receive the stream in the protocol you have chosen.</span></span> <span data-ttu-id="32b8a-110">Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="32b8a-110">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="32b8a-111">Následující diagram znázorňuje tradiční kódování a statické balení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="32b8a-111">The following diagram shows the traditional encoding and static packaging workflow.</span></span>

![Statické kódování](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

<span data-ttu-id="32b8a-113">Následující diagram znázorňuje dynamické balení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="32b8a-113">The following diagram shows the dynamic packaging workflow.</span></span>

![Dynamické kódování](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a><span data-ttu-id="32b8a-115">Běžný scénář</span><span class="sxs-lookup"><span data-stu-id="32b8a-115">Common scenario</span></span>
1. <span data-ttu-id="32b8a-116">Nahrajte vstupní soubor (nazývané soubor mezzanine).</span><span class="sxs-lookup"><span data-stu-id="32b8a-116">Upload an input file (called a mezzanine file).</span></span> <span data-ttu-id="32b8a-117">Například H.264, MP4 nebo WMV (seznam podporovaných formátů naleznete v části [formáty nepodporuje Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span><span class="sxs-lookup"><span data-stu-id="32b8a-117">For example, H.264, MP4, or WMV (for the list of supported formats see [Formats Supported by the Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span></span>
2. <span data-ttu-id="32b8a-118">Zakódujte váš soubor mezzanine do sady H.264 MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="32b8a-118">Encode your mezzanine file to H.264 MP4 adaptive bitrate sets.</span></span>
3. <span data-ttu-id="32b8a-119">Publikujte asset, který obsahuje tak, že vytvoříte Lokátor na vyžádání sady souborů MP4 s adaptivní přenosovou.</span><span class="sxs-lookup"><span data-stu-id="32b8a-119">Publish the asset that contains the adaptive bitrate MP4 set by creating the On-Demand Locator.</span></span>
4. <span data-ttu-id="32b8a-120">Vytvoření datových proudů adres URL pro přístup k a Streamovat obsah.</span><span class="sxs-lookup"><span data-stu-id="32b8a-120">Build the streaming URLs to access and stream your content.</span></span>

## <a name="preparing-assets-for-dynamic-streaming"></a><span data-ttu-id="32b8a-121">Příprava prostředky pro dynamické streamování</span><span class="sxs-lookup"><span data-stu-id="32b8a-121">Preparing assets for dynamic streaming</span></span>
<span data-ttu-id="32b8a-122">Příprava asset pro dynamické streamování máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="32b8a-122">To prepare your asset for dynamic streaming you have two options:</span></span>

1. <span data-ttu-id="32b8a-123">[Nahrát soubor hlavní](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="32b8a-123">[Upload a master file](media-services-dotnet-upload-files.md).</span></span>
2. <span data-ttu-id="32b8a-124">[Vytvoření sady H.264 MP4 s adaptivní přenosovou rychlostí pomocí kodéru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="32b8a-124">[Use the Media Encoder Standard encoder to produce H.264 MP4 adaptive bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>
3. <span data-ttu-id="32b8a-125">[Streamovat obsah](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32b8a-125">[Stream your content](media-services-deliver-content-overview.md).</span></span>

## <span data-ttu-id="32b8a-126"><a id="unsupported_formats"></a>Formáty, které nepodporuje dynamické balení</span><span class="sxs-lookup"><span data-stu-id="32b8a-126"><a id="unsupported_formats"></a>Formats that are not supported by dynamic packaging</span></span>
<span data-ttu-id="32b8a-127">Dynamické balení nejsou podporovány následující formáty souborů zdroje.</span><span class="sxs-lookup"><span data-stu-id="32b8a-127">The following source file formats are not supported by dynamic packaging.</span></span>

* <span data-ttu-id="32b8a-128">Soubory mp4 digitální Dolby.</span><span class="sxs-lookup"><span data-stu-id="32b8a-128">Dolby digital mp4 files.</span></span>
* <span data-ttu-id="32b8a-129">Digitální soubory technologie smooth Dolby.</span><span class="sxs-lookup"><span data-stu-id="32b8a-129">Dolby digital smooth files.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="32b8a-130">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="32b8a-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="32b8a-131">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="32b8a-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

