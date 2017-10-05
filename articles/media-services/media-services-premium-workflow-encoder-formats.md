---
title: "Formáty Media Encoder Premium pracovního postupu a kodeky | Microsoft Docs"
description: "Toto téma poskytuje přehled Media Encoder Premium pracovního postupu formáty formáty a kodeky"
services: media-services
documentationcenter: 
author: juliako
manager: erik43
editor: 
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: e18de2adc9aac585d6890dd7b43a54f1a0ca177e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a><span data-ttu-id="031bc-103">Formáty Media Encoder Premium pracovního postupu a kodeky</span><span class="sxs-lookup"><span data-stu-id="031bc-103">Media Encoder Premium Workflow formats and codecs</span></span>
> [!NOTE]
> <span data-ttu-id="031bc-104">Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="031bc-104">For premium encoder questions, email mepd at Microsoft.com.</span></span>
> 
> <span data-ttu-id="031bc-105">Procesor médií Media Encoder Premium pracovního postupu popsané v tomto tématu není k dispozici v Číně.</span><span class="sxs-lookup"><span data-stu-id="031bc-105">Media Encoder Premium Workflow media processor discussed in this topic is not available in China.</span></span> 
> 
> 

<span data-ttu-id="031bc-106">Tento dokument obsahuje seznam formáty vstupních a výstupních souborů a kodeků, které jsou podporovány ve verzi public preview verzi **Media Encoder Premium pracovního postupu** kodér.</span><span class="sxs-lookup"><span data-stu-id="031bc-106">This document contains a list of input and output file formats and codecs that are supported by the public preview version of the **Media Encoder Premium Workflow** encoder.</span></span>

[<span data-ttu-id="031bc-107">Media Encoder Premium Worflow vstup formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="031bc-107">Media Encoder Premium Worflow Input Formats and Codecs</span></span>](#input_formats)

[<span data-ttu-id="031bc-108">Media Encoder Premium Worflow výstupní formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="031bc-108">Media Encoder Premium Worflow Output Formats and Codecs</span></span>](#output_formats)

<span data-ttu-id="031bc-109">**Pracovní postup Premium Media Encoder** podporuje titulky popsané v [to](#closed_captioning) části.</span><span class="sxs-lookup"><span data-stu-id="031bc-109">**Media Encoder Premium Workflow** supports closed captioning described in [this](#closed_captioning) section.</span></span> 

## <span data-ttu-id="031bc-110"><a id="input_formats"></a>Pracovní postup Premium Media Encoder vstupní formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="031bc-110"><a id="input_formats"></a>Media Encoder Premium Workflow Input Formats and Codecs</span></span>
<span data-ttu-id="031bc-111">V následující části jsou uvedeny kodeky a soubor formátů, které podporuje tento procesor médií jako vstup.</span><span class="sxs-lookup"><span data-stu-id="031bc-111">The following section lists the codecs and file formats that this media processor supports as input.</span></span>

### <a name="input-containerfile-formats"></a><span data-ttu-id="031bc-112">Zadejte kontejner nebo formátů</span><span class="sxs-lookup"><span data-stu-id="031bc-112">Input Container/File Formats</span></span>
* <span data-ttu-id="031bc-113">Adobe® Flash® F4V</span><span class="sxs-lookup"><span data-stu-id="031bc-113">Adobe® Flash® F4V</span></span>
* <span data-ttu-id="031bc-114">MXF/SMPTE 377M</span><span class="sxs-lookup"><span data-stu-id="031bc-114">MXF/SMPTE 377M</span></span>
* <span data-ttu-id="031bc-115">GXF</span><span class="sxs-lookup"><span data-stu-id="031bc-115">GXF</span></span>
* <span data-ttu-id="031bc-116">Datové proudy MPEG-2 Transport</span><span class="sxs-lookup"><span data-stu-id="031bc-116">MPEG-2 Transport Streams</span></span>
* <span data-ttu-id="031bc-117">Datové proudy MPEG-2 programu</span><span class="sxs-lookup"><span data-stu-id="031bc-117">MPEG-2 Program Streams</span></span>
* <span data-ttu-id="031bc-118">MPEG-4 NEBO MP4</span><span class="sxs-lookup"><span data-stu-id="031bc-118">MPEG-4/MP4</span></span>
* <span data-ttu-id="031bc-119">Windows Media/amp</span><span class="sxs-lookup"><span data-stu-id="031bc-119">Windows Media/ASF</span></span>
* <span data-ttu-id="031bc-120">AVI (nekomprimované 8bitové/10 bitů)</span><span class="sxs-lookup"><span data-stu-id="031bc-120">AVI (Uncompressed 8bit/10bit)</span></span>

### <a name="input-video-codecs"></a><span data-ttu-id="031bc-121">Vstupní Video kodeky</span><span class="sxs-lookup"><span data-stu-id="031bc-121">Input Video Codecs</span></span>
* <span data-ttu-id="031bc-122">AVC 8-bit nebo 10-bit, až 4:2:2, včetně AVCIntra</span><span class="sxs-lookup"><span data-stu-id="031bc-122">AVC 8-bit/10-bit, up to 4:2:2, including AVCIntra</span></span>
* <span data-ttu-id="031bc-123">Avid DNxHD (v MXF)</span><span class="sxs-lookup"><span data-stu-id="031bc-123">Avid DNxHD (in MXF)</span></span>
* <span data-ttu-id="031bc-124">DVCPro/DVCProHD (v MXF)</span><span class="sxs-lookup"><span data-stu-id="031bc-124">DVCPro/DVCProHD (in MXF)</span></span>
* <span data-ttu-id="031bc-125">JPEG2000</span><span class="sxs-lookup"><span data-stu-id="031bc-125">JPEG2000</span></span>
* <span data-ttu-id="031bc-126">MPEG-2 (až 422 profil a vysokou úroveň, včetně například XDCAM, XDCAM HD, XDCAM IMX, CableLabs® a D10 variant)</span><span class="sxs-lookup"><span data-stu-id="031bc-126">MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span>
* <span data-ttu-id="031bc-127">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="031bc-127">MPEG-1</span></span>
* <span data-ttu-id="031bc-128">Windows Media Video/VC-1</span><span class="sxs-lookup"><span data-stu-id="031bc-128">Windows Media Video/VC-1</span></span>

### <a name="input-audio-codecs"></a><span data-ttu-id="031bc-129">Vstupní zvukových kodeků</span><span class="sxs-lookup"><span data-stu-id="031bc-129">Input Audio Codecs</span></span>
* <span data-ttu-id="031bc-130">AES (SMPTE 331 M a 302 M, AES3-2003)</span><span class="sxs-lookup"><span data-stu-id="031bc-130">AES (SMPTE 331M and 302M, AES3-2003)</span></span>
* <span data-ttu-id="031bc-131">Dolby® E</span><span class="sxs-lookup"><span data-stu-id="031bc-131">Dolby® E</span></span>
* <span data-ttu-id="031bc-132">Digitální Dolby® (AC3)</span><span class="sxs-lookup"><span data-stu-id="031bc-132">Dolby® Digital (AC3)</span></span>
* <span data-ttu-id="031bc-133">AAC (AAC-LC, AAC HE a AAC-HEv2; až 5.1)</span><span class="sxs-lookup"><span data-stu-id="031bc-133">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)</span></span>
* <span data-ttu-id="031bc-134">MPEG vrstvy 2</span><span class="sxs-lookup"><span data-stu-id="031bc-134">MPEG Layer 2</span></span>
* <span data-ttu-id="031bc-135">MP3 (MPEG-1 zvuk vrstvy 3)</span><span class="sxs-lookup"><span data-stu-id="031bc-135">MP3 (MPEG-1 Audio Layer 3)</span></span>
* <span data-ttu-id="031bc-136">Zvuk média systému Windows</span><span class="sxs-lookup"><span data-stu-id="031bc-136">Windows Media Audio</span></span>
* <span data-ttu-id="031bc-137">WAV NEBO PCM</span><span class="sxs-lookup"><span data-stu-id="031bc-137">WAV/PCM</span></span>

## <span data-ttu-id="031bc-138"><a id="output_format"></a>Media Encoder Premium pracovního postupu výstupní formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="031bc-138"><a id="output_format"></a>Media Encoder Premium Workflow Output Formats and Codecs</span></span>
<span data-ttu-id="031bc-139">V následující části jsou uvedeny formáty kodeky a souborů, které jsou podporované jako výstup z tohoto média procesoru.</span><span class="sxs-lookup"><span data-stu-id="031bc-139">The following section lists the codecs and file formats that are supported as output from this media processor.</span></span>

### <a name="output-containerfile-formats"></a><span data-ttu-id="031bc-140">Výstup kontejneru nebo formátů</span><span class="sxs-lookup"><span data-stu-id="031bc-140">Output Container/File Formats</span></span>
* <span data-ttu-id="031bc-141">Adobe® Flash® F4V</span><span class="sxs-lookup"><span data-stu-id="031bc-141">Adobe® Flash® F4V</span></span>
* <span data-ttu-id="031bc-142">MXF (OP1a, XDCAM a AS02)</span><span class="sxs-lookup"><span data-stu-id="031bc-142">MXF (OP1a, XDCAM and AS02)</span></span>
* <span data-ttu-id="031bc-143">DPP (včetně AS11)</span><span class="sxs-lookup"><span data-stu-id="031bc-143">DPP (including AS11)</span></span>
* <span data-ttu-id="031bc-144">GXF</span><span class="sxs-lookup"><span data-stu-id="031bc-144">GXF</span></span>
* <span data-ttu-id="031bc-145">MPEG-4 NEBO MP4</span><span class="sxs-lookup"><span data-stu-id="031bc-145">MPEG-4/MP4</span></span>
* <span data-ttu-id="031bc-146">Windows Media/amp</span><span class="sxs-lookup"><span data-stu-id="031bc-146">Windows Media/ASF</span></span>
* <span data-ttu-id="031bc-147">AVI (nekomprimované 8bitové/10 bitů)</span><span class="sxs-lookup"><span data-stu-id="031bc-147">AVI (Uncompressed 8bit/10bit)</span></span>
* <span data-ttu-id="031bc-148">Formát souborů Smooth datového proudu (PIFF 1.3)</span><span class="sxs-lookup"><span data-stu-id="031bc-148">Smooth Streaming File Format (PIFF 1.3)</span></span>
* <span data-ttu-id="031bc-149">MPEG-TS</span><span class="sxs-lookup"><span data-stu-id="031bc-149">MPEG-TS</span></span> 

### <a name="output-video-codecs"></a><span data-ttu-id="031bc-150">Výstup kodeky videa</span><span class="sxs-lookup"><span data-stu-id="031bc-150">Output Video Codecs</span></span>
* <span data-ttu-id="031bc-151">AVC (H.264; 8bitové; až profil vysokou úroveň 5.2; 4 kB Ultra HD; Uvnitř AVC)</span><span class="sxs-lookup"><span data-stu-id="031bc-151">AVC (H.264; 8-bit; up to High Profile, Level 5.2; 4K Ultra HD; AVC Intra)</span></span>
* <span data-ttu-id="031bc-152">Avid DNxHD (v MXF)</span><span class="sxs-lookup"><span data-stu-id="031bc-152">Avid DNxHD (in MXF)</span></span>
* <span data-ttu-id="031bc-153">DVCPro/DVCProHD (v MXF)</span><span class="sxs-lookup"><span data-stu-id="031bc-153">DVCPro/DVCProHD (in MXF)</span></span>
* <span data-ttu-id="031bc-154">MPEG-2 (až 422 profil a vysokou úroveň, včetně například XDCAM, XDCAM HD, XDCAM IMX, CableLabs® a D10 variant)</span><span class="sxs-lookup"><span data-stu-id="031bc-154">MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span>
* <span data-ttu-id="031bc-155">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="031bc-155">MPEG-1</span></span>
* <span data-ttu-id="031bc-156">Windows Media Video/VC-1</span><span class="sxs-lookup"><span data-stu-id="031bc-156">Windows Media Video/VC-1</span></span>
* <span data-ttu-id="031bc-157">Vytváření miniatur JPEG</span><span class="sxs-lookup"><span data-stu-id="031bc-157">JPEG thumbnail creation</span></span>

### <a name="output-audio-codecs"></a><span data-ttu-id="031bc-158">Výstup zvukových kodeků</span><span class="sxs-lookup"><span data-stu-id="031bc-158">Output Audio Codecs</span></span>
* <span data-ttu-id="031bc-159">AES (SMPTE 331 M a 302 M, AES3-2003)</span><span class="sxs-lookup"><span data-stu-id="031bc-159">AES (SMPTE 331M and 302M, AES3-2003)</span></span>
* <span data-ttu-id="031bc-160">Digitální Dolby® (AC3)</span><span class="sxs-lookup"><span data-stu-id="031bc-160">Dolby® Digital (AC3)</span></span>
* <span data-ttu-id="031bc-161">Dolby® Digital Plus (E-AC3) až 7.1</span><span class="sxs-lookup"><span data-stu-id="031bc-161">Dolby® Digital Plus (E-AC3) up to 7.1</span></span>
* <span data-ttu-id="031bc-162">AAC (AAC-LC, AAC HE a AAC-HEv2; až 5.1)</span><span class="sxs-lookup"><span data-stu-id="031bc-162">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)</span></span>
* <span data-ttu-id="031bc-163">MPEG vrstvy 2</span><span class="sxs-lookup"><span data-stu-id="031bc-163">MPEG Layer 2</span></span>
* <span data-ttu-id="031bc-164">MP3 (MPEG-1 zvuk vrstvy 3)</span><span class="sxs-lookup"><span data-stu-id="031bc-164">MP3 (MPEG-1 Audio Layer 3)</span></span>
* <span data-ttu-id="031bc-165">Zvuk média systému Windows</span><span class="sxs-lookup"><span data-stu-id="031bc-165">Windows Media Audio</span></span>

>[!NOTE]
><span data-ttu-id="031bc-166">Pokud jste dekódovat digitální® Dolby (AC3), výstup lze zapsat pouze do souboru ISO MP4.</span><span class="sxs-lookup"><span data-stu-id="031bc-166">If you encode to Dolby® Digital (AC3), the output can only be written into an ISO MP4 file.</span></span>

## <span data-ttu-id="031bc-167"><a id="closed_captioning"></a>Podpora pro titulků</span><span class="sxs-lookup"><span data-stu-id="031bc-167"><a id="closed_captioning"></a>Support for Closed Captioning</span></span>
<span data-ttu-id="031bc-168">Na ingestování, **Media Encoder Premium pracovního postupu** podporuje:</span><span class="sxs-lookup"><span data-stu-id="031bc-168">On ingest, **Media Encoder Premium Workflow** supports:</span></span>

1. <span data-ttu-id="031bc-169">Soubory SCC</span><span class="sxs-lookup"><span data-stu-id="031bc-169">SCC files</span></span>
2. <span data-ttu-id="031bc-170">Soubory SMPTE TT</span><span class="sxs-lookup"><span data-stu-id="031bc-170">SMPTE-TT files</span></span>
3. <span data-ttu-id="031bc-171">CEA-608/CEA-708 – provádí jako uživatelská data (SEI zpráv H.264 základní datové proudy, ATSC/53, SCTE20) nebo provádět jako pomocnou dat v souborech MXF/GXF</span><span class="sxs-lookup"><span data-stu-id="031bc-171">CEA-608/CEA-708 – carried as user data (SEI messages of H.264 elementary streams, ATSC/53, SCTE20) or carried as ancillary data in MXF/GXF files</span></span>
4. <span data-ttu-id="031bc-172">Soubory subtitle STL</span><span class="sxs-lookup"><span data-stu-id="031bc-172">STL subtitle files</span></span>

<span data-ttu-id="031bc-173">Na výstupu jsou k dispozici následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="031bc-173">On output, the following options are available:</span></span>

1. <span data-ttu-id="031bc-174">CEA 608 k CEA 708 překlad</span><span class="sxs-lookup"><span data-stu-id="031bc-174">CEA-608 to CEA-708 translation</span></span>
2. <span data-ttu-id="031bc-175">CEA-608/CEA-708 předávání (vložených v SEI zprávy základní datové proudy H.264 nebo provádět jako pomocnou dat v souborech MXF)</span><span class="sxs-lookup"><span data-stu-id="031bc-175">CEA-608/CEA-708 pass through (embedded in SEI messages of H.264 elementary streams, or carried as ancillary data in MXF files)</span></span>
3. <span data-ttu-id="031bc-176">SCC</span><span class="sxs-lookup"><span data-stu-id="031bc-176">SCC</span></span>
4. <span data-ttu-id="031bc-177">Text SMPTE vypršel časový limit (ze zdroje CEA 608 za SMPTE RP2052, včetně vytváření souborů DFXP)</span><span class="sxs-lookup"><span data-stu-id="031bc-177">SMPTE Timed Text (from source CEA-608 per SMPTE RP2052; including DFXP file creation)</span></span>
5. <span data-ttu-id="031bc-178">Soubor Subtitle SRT aplikace</span><span class="sxs-lookup"><span data-stu-id="031bc-178">SRT Subtitle file</span></span>
6. <span data-ttu-id="031bc-179">Subtitle DVB datové proudy</span><span class="sxs-lookup"><span data-stu-id="031bc-179">DVB subtitle streams</span></span>

<span data-ttu-id="031bc-180">Poznámka: výše uvedené výstupním formátu jsou podporovány pro doručení pomocí vysílání datového proudu ve službě Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="031bc-180">Note: not all of the above output formats are supported for delivery via streaming in Azure Media Services.</span></span>

## <a name="known-issues"></a><span data-ttu-id="031bc-181">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="031bc-181">Known issues</span></span>
<span data-ttu-id="031bc-182">Pokud vaše vstupní video neobsahuje uzavřen, přidávání titulků, výstup Asset bude stále obsahovat prázdný soubor TTML.</span><span class="sxs-lookup"><span data-stu-id="031bc-182">If your input video does not contain closed captioning, the output Asset will still contain an empty TTML file.</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="031bc-183">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="031bc-183">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="031bc-184">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="031bc-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

