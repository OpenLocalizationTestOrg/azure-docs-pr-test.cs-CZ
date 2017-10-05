---
title: "Schéma vstupu metadata Azure Media Services | Microsoft Docs"
description: "Téma nabízí přehled Azure Media Services vstupní metadata schématu."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d72848e2-4b65-4c84-94bc-e2a90a6e7f47
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 4787e4033e1afda6339b0b917263ecc165e400ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="input-metadata"></a><span data-ttu-id="0f950-103">Vstupní metadat</span><span class="sxs-lookup"><span data-stu-id="0f950-103">Input Metadata</span></span>
<span data-ttu-id="0f950-104">Kódování úlohy jsou přiřazeny prostředek vstupní (nebo prostředky) na který chcete provést některé úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="0f950-104">An encoding job is associated with an input asset (or assets) on which you want to perform some encoding tasks.</span></span>  <span data-ttu-id="0f950-105">Po dokončení úlohy se vytvářejí výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="0f950-105">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="0f950-106">Výstupní asset obsahuje video, zvuk, miniatur, manifest atd. Výstupní asset obsahuje také soubor s metadata o vstupní asset.</span><span class="sxs-lookup"><span data-stu-id="0f950-106">The output asset contains video, audio, thumbnails, manifest, etc. The output asset also contains a file with metadata about the input asset.</span></span> <span data-ttu-id="0f950-107">Název souboru XML metadat má následující formát: &lt;asset_id&gt;_metadata.xml (například 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), kde &lt;asset_id&gt; je hodnota ID vstupní prostředek.</span><span class="sxs-lookup"><span data-stu-id="0f950-107">The name of the metadata XML file has the following format: &lt;asset_id&gt;_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where &lt;asset_id&gt; is the AssetId value of the input asset.</span></span>  

<span data-ttu-id="0f950-108">Pokud chcete zkontrolovat soubor metadat, můžete vytvořit **SAS** Lokátor a stahování souborů do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="0f950-108">If you want to examine the metadata file, you can create a **SAS** locator and download the file to your local computer.</span></span> <span data-ttu-id="0f950-109">Příklad najdete na tom, jak vytvořit lokátor SAS a stáhnout soubor [pomocí rozšíření Media Services .NET SDK](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0f950-109">You can find an example on how to create a SAS locator and download a file  [Using the Media Services .NET SDK Extensions](media-services-dotnet-get-started.md).</span></span>  

<span data-ttu-id="0f950-110">Toto téma popisuje elementy a typy schématu XML, na kterém vstupní metada (&lt;asset_id&gt;_metadata.xml) je založena.</span><span class="sxs-lookup"><span data-stu-id="0f950-110">This topic discusses the elements and types of the XML schema on which the input metada (&lt;asset_id&gt;_metadata.xml) is based.</span></span>  <span data-ttu-id="0f950-111">Informace o souboru, který obsahuje metadata o výstupní asset najdete v tématu [výstup metadat](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="0f950-111">For information about the file that contains metadata about the output asset, see [Output Metadata](media-services-output-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="0f950-112">Můžete najít [kód schématu](media-services-input-metadata-schema.md#code) [ukázkový kód XML](media-services-input-metadata-schema.md#xml) na konci tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="0f950-112">You can find the [Schema Code](media-services-input-metadata-schema.md#code) an [XML example](media-services-input-metadata-schema.md#xml) at the end of this topic.</span></span>  
> 
> 

## <span data-ttu-id="0f950-113"><a name="AssetFiles"></a>Element AssetFiles (kořenový element)</span><span class="sxs-lookup"><span data-stu-id="0f950-113"><a name="AssetFiles"></a> AssetFiles element (root element)</span></span>
<span data-ttu-id="0f950-114">Obsahuje kolekci [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s pro úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="0f950-114">Contains a collection of [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s for the encoding job.</span></span>  

<span data-ttu-id="0f950-115">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-115">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

| <span data-ttu-id="0f950-116">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-116">Name</span></span> | <span data-ttu-id="0f950-117">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0f950-118">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="0f950-118">**AssetFile**</span></span><br /><br /> <span data-ttu-id="0f950-119">Hodnota minOccurs = "1" maxOccurs = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="0f950-119">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="0f950-120">Jeden podřízený element.</span><span class="sxs-lookup"><span data-stu-id="0f950-120">A single child element.</span></span> <span data-ttu-id="0f950-121">Další informace najdete v tématu [AssetFile element](media-services-input-metadata-schema.md#AssetFile).</span><span class="sxs-lookup"><span data-stu-id="0f950-121">For more information, see [AssetFile element](media-services-input-metadata-schema.md#AssetFile).</span></span> |

## <span data-ttu-id="0f950-122"><a name="AssetFile"></a>AssetFile element</span><span class="sxs-lookup"><span data-stu-id="0f950-122"><a name="AssetFile"></a> AssetFile element</span></span>
 <span data-ttu-id="0f950-123">Obsahuje atributy a elementy, které popisují soubor asset.</span><span class="sxs-lookup"><span data-stu-id="0f950-123">Contains attributes and elements that describe an asset file.</span></span>  

 <span data-ttu-id="0f950-124">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-124">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-125">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-125">Attributes</span></span>
| <span data-ttu-id="0f950-126">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-126">Name</span></span> | <span data-ttu-id="0f950-127">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-127">Type</span></span> | <span data-ttu-id="0f950-128">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-128">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-129">**Název**</span><span class="sxs-lookup"><span data-stu-id="0f950-129">**Name**</span></span><br /><br /> <span data-ttu-id="0f950-130">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-130">Required</span></span> |<span data-ttu-id="0f950-131">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-131">**xs:string**</span></span> |<span data-ttu-id="0f950-132">Název souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-132">Asset file name.</span></span> |
| <span data-ttu-id="0f950-133">**Velikost**</span><span class="sxs-lookup"><span data-stu-id="0f950-133">**Size**</span></span><br /><br /> <span data-ttu-id="0f950-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-134">Required</span></span> |<span data-ttu-id="0f950-135">**xs:Long**</span><span class="sxs-lookup"><span data-stu-id="0f950-135">**xs:long**</span></span> |<span data-ttu-id="0f950-136">Velikost souboru assetu v bajtech.</span><span class="sxs-lookup"><span data-stu-id="0f950-136">Size of the asset file in bytes.</span></span> |
| <span data-ttu-id="0f950-137">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="0f950-137">**Duration**</span></span><br /><br /> <span data-ttu-id="0f950-138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-138">Required</span></span> |<span data-ttu-id="0f950-139">**xs**</span><span class="sxs-lookup"><span data-stu-id="0f950-139">**xs:duration**</span></span> |<span data-ttu-id="0f950-140">Obsahu play back doba trvání.</span><span class="sxs-lookup"><span data-stu-id="0f950-140">Content play back duration.</span></span> <span data-ttu-id="0f950-141">Příklad: Doba trvání = "PT25M37.757S".</span><span class="sxs-lookup"><span data-stu-id="0f950-141">Example: Duration="PT25M37.757S".</span></span> |
| <span data-ttu-id="0f950-142">**NumberOfStreams**</span><span class="sxs-lookup"><span data-stu-id="0f950-142">**NumberOfStreams**</span></span><br /><br /> <span data-ttu-id="0f950-143">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-143">Required</span></span> |<span data-ttu-id="0f950-144">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-144">**xs:int**</span></span> |<span data-ttu-id="0f950-145">Počet datových proudů v souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-145">Number of streams in the asset file.</span></span> |
| <span data-ttu-id="0f950-146">**FormatNames**</span><span class="sxs-lookup"><span data-stu-id="0f950-146">**FormatNames**</span></span><br /><br /> <span data-ttu-id="0f950-147">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-147">Required</span></span> |<span data-ttu-id="0f950-148">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-148">**xs:string**</span></span> |<span data-ttu-id="0f950-149">Názvy ve formátu.</span><span class="sxs-lookup"><span data-stu-id="0f950-149">Format names.</span></span> |
| <span data-ttu-id="0f950-150">**FormatVerboseNames**</span><span class="sxs-lookup"><span data-stu-id="0f950-150">**FormatVerboseNames**</span></span><br /><br /> <span data-ttu-id="0f950-151">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-151">Required</span></span> |<span data-ttu-id="0f950-152">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-152">**xs:string**</span></span> |<span data-ttu-id="0f950-153">Podrobné názvy ve formátu.</span><span class="sxs-lookup"><span data-stu-id="0f950-153">Format verbose names.</span></span> |
| <span data-ttu-id="0f950-154">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="0f950-154">**StartTime**</span></span> |<span data-ttu-id="0f950-155">**xs**</span><span class="sxs-lookup"><span data-stu-id="0f950-155">**xs:duration**</span></span> |<span data-ttu-id="0f950-156">Čas zahájení obsahu.</span><span class="sxs-lookup"><span data-stu-id="0f950-156">Content start time.</span></span> <span data-ttu-id="0f950-157">Příklad: StartTime = "PT2.669S".</span><span class="sxs-lookup"><span data-stu-id="0f950-157">Example: StartTime="PT2.669S".</span></span> |
| <span data-ttu-id="0f950-158">**OverallBitRate**</span><span class="sxs-lookup"><span data-stu-id="0f950-158">**OverallBitRate**</span></span> |<span data-ttu-id="0f950-159">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-159">**xs:int**</span></span> |<span data-ttu-id="0f950-160">Průměrná přenosovou rychlostí souboru asset v kb/s.</span><span class="sxs-lookup"><span data-stu-id="0f950-160">Average bitrate of the asset file in kbps.</span></span> |

> [!NOTE]
> <span data-ttu-id="0f950-161">Následující 4 podřízené elementy musí být uvedena v pořadí.</span><span class="sxs-lookup"><span data-stu-id="0f950-161">The following 4 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="0f950-162">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="0f950-162">Child elements</span></span>
| <span data-ttu-id="0f950-163">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-163">Name</span></span> | <span data-ttu-id="0f950-164">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-164">Type</span></span> | <span data-ttu-id="0f950-165">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-166">**Programy**</span><span class="sxs-lookup"><span data-stu-id="0f950-166">**Programs**</span></span><br /><br /> <span data-ttu-id="0f950-167">Hodnota minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="0f950-167">minOccurs="0"</span></span> | |<span data-ttu-id="0f950-168">Kolekce všech [programy element](media-services-input-metadata-schema.md#Programs) po souboru prostředku ve formátu MPEG-TS.</span><span class="sxs-lookup"><span data-stu-id="0f950-168">Collection of all [Programs element](media-services-input-metadata-schema.md#Programs) when the asset file is in MPEG-TS format.</span></span> |
| <span data-ttu-id="0f950-169">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="0f950-169">**VideoTracks**</span></span><br /><br /> <span data-ttu-id="0f950-170">Hodnota minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="0f950-170">minOccurs="0"</span></span> | |<span data-ttu-id="0f950-171">Každý fyzický prostředek soubor může obsahovat nula nebo více video sleduje prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0f950-171">Each physical asset file can contain zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="0f950-172">Tento prvek obsahuje kolekci všech [VideoTracks element](media-services-input-metadata-schema.md#VideoTracks) které jsou součástí souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-172">This element contains a collection of all [VideoTracks element](media-services-input-metadata-schema.md#VideoTracks) that are part of the asset file.</span></span> |
| <span data-ttu-id="0f950-173">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="0f950-173">**AudioTracks**</span></span><br /><br /> <span data-ttu-id="0f950-174">Hodnota minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="0f950-174">minOccurs="0"</span></span> | |<span data-ttu-id="0f950-175">Každý fyzický prostředek soubor může obsahovat nula nebo více zvukových stop prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0f950-175">Each physical asset file can contain zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="0f950-176">Tento prvek obsahuje kolekci všech [AudioTracks element](media-services-input-metadata-schema.md#AudioTracks) které jsou součástí souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-176">This element contains a collection of all [AudioTracks element](media-services-input-metadata-schema.md#AudioTracks) that are part of the asset file.</span></span> |
| <span data-ttu-id="0f950-177">**Metadata**</span><span class="sxs-lookup"><span data-stu-id="0f950-177">**Metadata**</span></span><br /><br /> <span data-ttu-id="0f950-178">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="0f950-178">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="0f950-179">MetadataType</span><span class="sxs-lookup"><span data-stu-id="0f950-179">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="0f950-180">Metadata souboru assetu vyjádřené key\value řetězce.</span><span class="sxs-lookup"><span data-stu-id="0f950-180">Asset file’s metadata represented as key\value strings.</span></span> <span data-ttu-id="0f950-181">Například:</span><span class="sxs-lookup"><span data-stu-id="0f950-181">For example:</span></span><br /><br /> <span data-ttu-id="0f950-182">**&lt;Metadata key = "jazyk" value = "eng" /&gt;**</span><span class="sxs-lookup"><span data-stu-id="0f950-182">**&lt;Metadata key="language" value="eng" /&gt;**</span></span> |

## <span data-ttu-id="0f950-183"><a name="TrackType"></a>TrackType</span><span class="sxs-lookup"><span data-stu-id="0f950-183"><a name="TrackType"></a> TrackType</span></span>
<span data-ttu-id="0f950-184">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-184">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-185">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-185">Attributes</span></span>
| <span data-ttu-id="0f950-186">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-186">Name</span></span> | <span data-ttu-id="0f950-187">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-187">Type</span></span> | <span data-ttu-id="0f950-188">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-188">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-189">**ID**</span><span class="sxs-lookup"><span data-stu-id="0f950-189">**Id**</span></span><br /><br /> <span data-ttu-id="0f950-190">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-190">Required</span></span> |<span data-ttu-id="0f950-191">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-191">**xs:int**</span></span> |<span data-ttu-id="0f950-192">Index nule tento dráhy zvuku a videa.</span><span class="sxs-lookup"><span data-stu-id="0f950-192">Zero-based index of this audio or video track.</span></span><br /><br /> <span data-ttu-id="0f950-193">To není nezbytně, TrackID jako použitá v souboru MP4.</span><span class="sxs-lookup"><span data-stu-id="0f950-193">This is not necessarily that the TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="0f950-194">**Kodeků**</span><span class="sxs-lookup"><span data-stu-id="0f950-194">**Codec**</span></span> |<span data-ttu-id="0f950-195">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-195">**xs:string**</span></span> |<span data-ttu-id="0f950-196">Řetězec kodeků sledovat videa.</span><span class="sxs-lookup"><span data-stu-id="0f950-196">Video track codec string.</span></span> |
| <span data-ttu-id="0f950-197">**CodecLongName**</span><span class="sxs-lookup"><span data-stu-id="0f950-197">**CodecLongName**</span></span> |<span data-ttu-id="0f950-198">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-198">**xs:string**</span></span> |<span data-ttu-id="0f950-199">Sledování zvuku a videa kodeků dlouhý název.</span><span class="sxs-lookup"><span data-stu-id="0f950-199">Audio or video track codec long name.</span></span> |
| <span data-ttu-id="0f950-200">**Časové základny**</span><span class="sxs-lookup"><span data-stu-id="0f950-200">**TimeBase**</span></span><br /><br /> <span data-ttu-id="0f950-201">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-201">Required</span></span> |<span data-ttu-id="0f950-202">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-202">**xs:string**</span></span> |<span data-ttu-id="0f950-203">Základní doba.</span><span class="sxs-lookup"><span data-stu-id="0f950-203">Time base.</span></span> <span data-ttu-id="0f950-204">Příklad: Časové základny = "1/48000"</span><span class="sxs-lookup"><span data-stu-id="0f950-204">Example: TimeBase="1/48000"</span></span> |
| <span data-ttu-id="0f950-205">**NumberOfFrames**</span><span class="sxs-lookup"><span data-stu-id="0f950-205">**NumberOfFrames**</span></span> |<span data-ttu-id="0f950-206">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-206">**xs:int**</span></span> |<span data-ttu-id="0f950-207">Počet rámců (k dispozici pro video sleduje).</span><span class="sxs-lookup"><span data-stu-id="0f950-207">Number of frames (present for video tracks).</span></span> |
| <span data-ttu-id="0f950-208">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="0f950-208">**StartTime**</span></span> |<span data-ttu-id="0f950-209">**xs**</span><span class="sxs-lookup"><span data-stu-id="0f950-209">**xs:duration**</span></span> |<span data-ttu-id="0f950-210">Čas zahájení sledovat.</span><span class="sxs-lookup"><span data-stu-id="0f950-210">Track start time.</span></span> <span data-ttu-id="0f950-211">Příklad: StartTime = "PT2.669S"</span><span class="sxs-lookup"><span data-stu-id="0f950-211">Example: StartTime="PT2.669S"</span></span> |
| <span data-ttu-id="0f950-212">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="0f950-212">**Duration**</span></span> |<span data-ttu-id="0f950-213">**xs**</span><span class="sxs-lookup"><span data-stu-id="0f950-213">**xs:duration**</span></span> |<span data-ttu-id="0f950-214">Sledování doby trvání.</span><span class="sxs-lookup"><span data-stu-id="0f950-214">Track duration.</span></span> <span data-ttu-id="0f950-215">Příklad: Doba trvání = "PTSampleFormat M37.757S".</span><span class="sxs-lookup"><span data-stu-id="0f950-215">Example: Duration="PTSampleFormat M37.757S".</span></span> |

> [!NOTE]
> <span data-ttu-id="0f950-216">Následující 2 podřízené elementy musí být uvedena v pořadí.</span><span class="sxs-lookup"><span data-stu-id="0f950-216">The following 2 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="0f950-217">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="0f950-217">Child elements</span></span>
| <span data-ttu-id="0f950-218">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-218">Name</span></span> | <span data-ttu-id="0f950-219">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-219">Type</span></span> | <span data-ttu-id="0f950-220">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-220">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-221">**Dispozice**</span><span class="sxs-lookup"><span data-stu-id="0f950-221">**Disposition**</span></span><br /><br /> <span data-ttu-id="0f950-222">Hodnota minOccurs = maxOccurs "0" = "1"</span><span class="sxs-lookup"><span data-stu-id="0f950-222">minOccurs="0" maxOccurs="1"</span></span> |[<span data-ttu-id="0f950-223">StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="0f950-223">StreamDispositionType</span></span>](media-services-input-metadata-schema.md#StreamDispositionType) |<span data-ttu-id="0f950-224">Obsahuje informace prezentace (například jestli konkrétní audio track je pro slabozraké uživatele).</span><span class="sxs-lookup"><span data-stu-id="0f950-224">Contains presentation information (for example, whether a particular audio track is for visually impaired viewers).</span></span> |
| <span data-ttu-id="0f950-225">**Metadata**</span><span class="sxs-lookup"><span data-stu-id="0f950-225">**Metadata**</span></span><br /><br /> <span data-ttu-id="0f950-226">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="0f950-226">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="0f950-227">MetadataType</span><span class="sxs-lookup"><span data-stu-id="0f950-227">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="0f950-228">Obecné klíč/hodnota řetězce, které slouží k uložení různé informace.</span><span class="sxs-lookup"><span data-stu-id="0f950-228">Generic key/value strings that can be used to hold a variety of information.</span></span> <span data-ttu-id="0f950-229">Například klíč = "jazyk" a hodnota = "eng".</span><span class="sxs-lookup"><span data-stu-id="0f950-229">For example, key=”language”, and value=”eng”.</span></span> |

## <span data-ttu-id="0f950-230"><a name="AudioTrackType"></a>AudioTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="0f950-230"><a name="AudioTrackType"></a> AudioTrackType (inherits from TrackType)</span></span>
 <span data-ttu-id="0f950-231">**AudioTrackType** je globální komplexní typ, který dědí z [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="0f950-231">**AudioTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

 <span data-ttu-id="0f950-232">Typ představuje konkrétní zvuk sledovat v souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-232">The type represents a specific audio track in the asset file.</span></span>  

 <span data-ttu-id="0f950-233">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-233">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-234">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-234">Attributes</span></span>
| <span data-ttu-id="0f950-235">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-235">Name</span></span> | <span data-ttu-id="0f950-236">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-236">Type</span></span> | <span data-ttu-id="0f950-237">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-237">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-238">**SampleFormat**</span><span class="sxs-lookup"><span data-stu-id="0f950-238">**SampleFormat**</span></span> |<span data-ttu-id="0f950-239">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-239">**xs:string**</span></span> |<span data-ttu-id="0f950-240">Ukázka formátu.</span><span class="sxs-lookup"><span data-stu-id="0f950-240">Sample format.</span></span> |
| <span data-ttu-id="0f950-241">**ChannelLayout**</span><span class="sxs-lookup"><span data-stu-id="0f950-241">**ChannelLayout**</span></span> |<span data-ttu-id="0f950-242">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-242">**xs:string**</span></span> |<span data-ttu-id="0f950-243">Kanál rozložení.</span><span class="sxs-lookup"><span data-stu-id="0f950-243">Channel layout.</span></span> |
| <span data-ttu-id="0f950-244">**Kanály**</span><span class="sxs-lookup"><span data-stu-id="0f950-244">**Channels**</span></span><br /><br /> <span data-ttu-id="0f950-245">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-245">Required</span></span> |<span data-ttu-id="0f950-246">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-246">**xs:int**</span></span> |<span data-ttu-id="0f950-247">Počet (0 nebo více) zvukové kanály.</span><span class="sxs-lookup"><span data-stu-id="0f950-247">Number (0 or more) of audio channels.</span></span> |
| <span data-ttu-id="0f950-248">**SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="0f950-248">**SamplingRate**</span></span><br /><br /> <span data-ttu-id="0f950-249">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-249">Required</span></span> |<span data-ttu-id="0f950-250">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-250">**xs:int**</span></span> |<span data-ttu-id="0f950-251">Zvuk vzorkovací frekvenci v ukázky za sekundu nebo Hz.</span><span class="sxs-lookup"><span data-stu-id="0f950-251">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="0f950-252">**Přenosovou rychlostí**</span><span class="sxs-lookup"><span data-stu-id="0f950-252">**Bitrate**</span></span> |<span data-ttu-id="0f950-253">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-253">**xs:int**</span></span> |<span data-ttu-id="0f950-254">Průměrná přenosová rychlost zvuku v bitech za sekundu, počítané ze souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-254">Average audio bit rate in bits per second, as calculated from the asset file.</span></span> <span data-ttu-id="0f950-255">Pouze datové části Základní datový proud se počítá a nároky na balení není zahrnut do tohoto počtu.</span><span class="sxs-lookup"><span data-stu-id="0f950-255">Only the elementary stream payload is counted, and the packaging overhead is not included in this count.</span></span> |
| <span data-ttu-id="0f950-256">**BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="0f950-256">**BitsPerSample**</span></span> |<span data-ttu-id="0f950-257">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-257">**xs:int**</span></span> |<span data-ttu-id="0f950-258">Bitů na vzorek pro formát wFormatTag typu.</span><span class="sxs-lookup"><span data-stu-id="0f950-258">Bits per sample for the wFormatTag format type.</span></span> |

## <span data-ttu-id="0f950-259"><a name="VideoTrackType"></a>VideoTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="0f950-259"><a name="VideoTrackType"></a> VideoTrackType (inherits from TrackType)</span></span>
<span data-ttu-id="0f950-260">**VideoTrackType** je globální komplexní typ, který dědí z [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="0f950-260">**VideoTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

<span data-ttu-id="0f950-261">Typ představuje konkrétní video sledovat v souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-261">The type represents a specific video track in the asset file.</span></span>  

<span data-ttu-id="0f950-262">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-262">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-263">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-263">Attributes</span></span>
| <span data-ttu-id="0f950-264">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-264">Name</span></span> | <span data-ttu-id="0f950-265">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-265">Type</span></span> | <span data-ttu-id="0f950-266">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-266">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-267">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="0f950-267">**FourCC**</span></span><br /><br /> <span data-ttu-id="0f950-268">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-268">Required</span></span> |<span data-ttu-id="0f950-269">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-269">**xs:string**</span></span> |<span data-ttu-id="0f950-270">Video kodeků FourCC kódu.</span><span class="sxs-lookup"><span data-stu-id="0f950-270">Video codec FourCC code.</span></span> |
| <span data-ttu-id="0f950-271">**Profil**</span><span class="sxs-lookup"><span data-stu-id="0f950-271">**Profile**</span></span> |<span data-ttu-id="0f950-272">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-272">**xs:string**</span></span> |<span data-ttu-id="0f950-273">Video sledovat profil.</span><span class="sxs-lookup"><span data-stu-id="0f950-273">Video track's profile.</span></span> |
| <span data-ttu-id="0f950-274">**Úroveň**</span><span class="sxs-lookup"><span data-stu-id="0f950-274">**Level**</span></span> |<span data-ttu-id="0f950-275">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-275">**xs:string**</span></span> |<span data-ttu-id="0f950-276">Video sledovat úroveň.</span><span class="sxs-lookup"><span data-stu-id="0f950-276">Video track's level.</span></span> |
| <span data-ttu-id="0f950-277">**PixelFormat**</span><span class="sxs-lookup"><span data-stu-id="0f950-277">**PixelFormat**</span></span> |<span data-ttu-id="0f950-278">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-278">**xs:string**</span></span> |<span data-ttu-id="0f950-279">Video sledovat Pixelový formát.</span><span class="sxs-lookup"><span data-stu-id="0f950-279">Video track's pixel format.</span></span> |
| <span data-ttu-id="0f950-280">**Šířka**</span><span class="sxs-lookup"><span data-stu-id="0f950-280">**Width**</span></span><br /><br /> <span data-ttu-id="0f950-281">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-281">Required</span></span> |<span data-ttu-id="0f950-282">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-282">**xs:int**</span></span> |<span data-ttu-id="0f950-283">Kódovaný video šířku v pixelech.</span><span class="sxs-lookup"><span data-stu-id="0f950-283">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="0f950-284">**Výška**</span><span class="sxs-lookup"><span data-stu-id="0f950-284">**Height**</span></span><br /><br /> <span data-ttu-id="0f950-285">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-285">Required</span></span> |<span data-ttu-id="0f950-286">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-286">**xs:int**</span></span> |<span data-ttu-id="0f950-287">Kódovaný Výška videa v pixelech.</span><span class="sxs-lookup"><span data-stu-id="0f950-287">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="0f950-288">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="0f950-288">**DisplayAspectRatioNumerator**</span></span><br /><br /> <span data-ttu-id="0f950-289">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-289">Required</span></span> |<span data-ttu-id="0f950-290">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="0f950-290">**xs:double**</span></span> |<span data-ttu-id="0f950-291">Zobrazení videa čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="0f950-291">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="0f950-292">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="0f950-292">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="0f950-293">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-293">Required</span></span> |<span data-ttu-id="0f950-294">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="0f950-294">**xs:double**</span></span> |<span data-ttu-id="0f950-295">Jmenovatel grafického poměr stran.</span><span class="sxs-lookup"><span data-stu-id="0f950-295">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="0f950-296">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="0f950-296">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="0f950-297">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-297">Required</span></span> |<span data-ttu-id="0f950-298">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="0f950-298">**xs:double**</span></span> |<span data-ttu-id="0f950-299">Ukázkové video čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="0f950-299">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="0f950-300">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="0f950-300">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="0f950-301">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="0f950-301">**xs:double**</span></span> |<span data-ttu-id="0f950-302">Ukázkové video čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="0f950-302">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="0f950-303">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="0f950-303">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="0f950-304">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="0f950-304">**xs:double**</span></span> |<span data-ttu-id="0f950-305">Ukázkové video jmenovatel poměr stran.</span><span class="sxs-lookup"><span data-stu-id="0f950-305">Video sample aspect ratio denominator.</span></span> |
| <span data-ttu-id="0f950-306">**Kmitočet snímků**</span><span class="sxs-lookup"><span data-stu-id="0f950-306">**FrameRate**</span></span><br /><br /> <span data-ttu-id="0f950-307">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-307">Required</span></span> |<span data-ttu-id="0f950-308">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="0f950-308">**xs:decimal**</span></span> |<span data-ttu-id="0f950-309">Měří video obnovovací frekvence ve formátu .3f.</span><span class="sxs-lookup"><span data-stu-id="0f950-309">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="0f950-310">**Přenosovou rychlostí**</span><span class="sxs-lookup"><span data-stu-id="0f950-310">**Bitrate**</span></span> |<span data-ttu-id="0f950-311">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-311">**xs:int**</span></span> |<span data-ttu-id="0f950-312">Průměrná přenosová rychlost videa v kilobity za sekundu, počítané ze souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-312">Average video bit rate in kilobits per second, as calculated from the asset file.</span></span> <span data-ttu-id="0f950-313">Pouze datové části Základní datový proud se počítá a nároky na balení není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="0f950-313">Only the elementary stream payload is counted, and the packaging overhead is not included.</span></span> |
| <span data-ttu-id="0f950-314">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="0f950-314">**MaxGOPBitrate**</span></span> |<span data-ttu-id="0f950-315">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-315">**xs:int**</span></span> |<span data-ttu-id="0f950-316">Maximální počet GOP průměrná přenosovou rychlostí pro tento stopy videa, v kB.</span><span class="sxs-lookup"><span data-stu-id="0f950-316">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |
| <span data-ttu-id="0f950-317">**HasBFrames**</span><span class="sxs-lookup"><span data-stu-id="0f950-317">**HasBFrames**</span></span> |<span data-ttu-id="0f950-318">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-318">**xs:int**</span></span> |<span data-ttu-id="0f950-319">Video sledovat počet rámců B.</span><span class="sxs-lookup"><span data-stu-id="0f950-319">Video track number of B frames.</span></span> |

## <span data-ttu-id="0f950-320"><a name="MetadataType"></a>MetadataType</span><span class="sxs-lookup"><span data-stu-id="0f950-320"><a name="MetadataType"></a> MetadataType</span></span>
<span data-ttu-id="0f950-321">**MetadataType** je globální komplexní typ, který popisuje metadata souboru asset jako klíč/hodnota řetězce.</span><span class="sxs-lookup"><span data-stu-id="0f950-321">**MetadataType** is a global complex type that describes metadata of an asset file as key/value strings.</span></span> <span data-ttu-id="0f950-322">Například klíč = "jazyk" a hodnota = "eng".</span><span class="sxs-lookup"><span data-stu-id="0f950-322">For example, key=”language”, and value=”eng”.</span></span>  

<span data-ttu-id="0f950-323">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-323">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-324">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-324">Attributes</span></span>
| <span data-ttu-id="0f950-325">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-325">Name</span></span> | <span data-ttu-id="0f950-326">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-326">Type</span></span> | <span data-ttu-id="0f950-327">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-327">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-328">**klíč**</span><span class="sxs-lookup"><span data-stu-id="0f950-328">**key**</span></span><br /><br /> <span data-ttu-id="0f950-329">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-329">Required</span></span> |<span data-ttu-id="0f950-330">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-330">**xs:string**</span></span> |<span data-ttu-id="0f950-331">Klíč ve dvojici klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="0f950-331">The key in the key/value pair.</span></span> |
| <span data-ttu-id="0f950-332">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="0f950-332">**value**</span></span><br /><br /> <span data-ttu-id="0f950-333">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-333">Required</span></span> |<span data-ttu-id="0f950-334">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="0f950-334">**xs:string**</span></span> |<span data-ttu-id="0f950-335">Hodnota ve dvojici klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="0f950-335">The value in the key/value pair.</span></span> |

## <span data-ttu-id="0f950-336"><a name="ProgramType"></a>ProgramType</span><span class="sxs-lookup"><span data-stu-id="0f950-336"><a name="ProgramType"></a> ProgramType</span></span>
<span data-ttu-id="0f950-337">**ProgramType** je globální komplexní typ, který popisuje program.</span><span class="sxs-lookup"><span data-stu-id="0f950-337">**ProgramType** is a global complex type that describes a program.</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-338">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-338">Attributes</span></span>
| <span data-ttu-id="0f950-339">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-339">Name</span></span> | <span data-ttu-id="0f950-340">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-340">Type</span></span> | <span data-ttu-id="0f950-341">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-341">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-342">**ProgramId**</span><span class="sxs-lookup"><span data-stu-id="0f950-342">**ProgramId**</span></span><br /><br /> <span data-ttu-id="0f950-343">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-343">Required</span></span> |<span data-ttu-id="0f950-344">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-344">**xs:int**</span></span> |<span data-ttu-id="0f950-345">Id programu</span><span class="sxs-lookup"><span data-stu-id="0f950-345">Program Id</span></span> |
| <span data-ttu-id="0f950-346">**NumberOfPrograms**</span><span class="sxs-lookup"><span data-stu-id="0f950-346">**NumberOfPrograms**</span></span><br /><br /> <span data-ttu-id="0f950-347">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-347">Required</span></span> |<span data-ttu-id="0f950-348">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-348">**xs:int**</span></span> |<span data-ttu-id="0f950-349">Počet programy.</span><span class="sxs-lookup"><span data-stu-id="0f950-349">Number of programs.</span></span> |
| <span data-ttu-id="0f950-350">**PmtPid**</span><span class="sxs-lookup"><span data-stu-id="0f950-350">**PmtPid**</span></span><br /><br /> <span data-ttu-id="0f950-351">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-351">Required</span></span> |<span data-ttu-id="0f950-352">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-352">**xs:int**</span></span> |<span data-ttu-id="0f950-353">Program Mapa tabulky (PMTs) obsahují informace o programy.</span><span class="sxs-lookup"><span data-stu-id="0f950-353">Program Map Tables (PMTs) contain information about programs.</span></span>  <span data-ttu-id="0f950-354">Další informace najdete v tématu [platba](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span><span class="sxs-lookup"><span data-stu-id="0f950-354">For more information, see [PMt](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span></span> |
| <span data-ttu-id="0f950-355">**PcrPid**</span><span class="sxs-lookup"><span data-stu-id="0f950-355">**PcrPid**</span></span><br /><br /> <span data-ttu-id="0f950-356">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-356">Required</span></span> |<span data-ttu-id="0f950-357">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-357">**xs:int**</span></span> |<span data-ttu-id="0f950-358">Používá decoder.</span><span class="sxs-lookup"><span data-stu-id="0f950-358">Used by decoder.</span></span> <span data-ttu-id="0f950-359">Další informace najdete v tématu [nejvyššího počtu buněk](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span><span class="sxs-lookup"><span data-stu-id="0f950-359">For more information, see [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span></span> |
| <span data-ttu-id="0f950-360">**StartPTS**</span><span class="sxs-lookup"><span data-stu-id="0f950-360">**StartPTS**</span></span> |<span data-ttu-id="0f950-361">**xs: dlouho**</span><span class="sxs-lookup"><span data-stu-id="0f950-361">**xs: long**</span></span> |<span data-ttu-id="0f950-362">Spouštění prezentace časové razítko.</span><span class="sxs-lookup"><span data-stu-id="0f950-362">Starting presentation time stamp.</span></span> |
| <span data-ttu-id="0f950-363">**EndPTS**</span><span class="sxs-lookup"><span data-stu-id="0f950-363">**EndPTS**</span></span> |<span data-ttu-id="0f950-364">**xs: dlouho**</span><span class="sxs-lookup"><span data-stu-id="0f950-364">**xs: long**</span></span> |<span data-ttu-id="0f950-365">Ukončování prezentace časové razítko.</span><span class="sxs-lookup"><span data-stu-id="0f950-365">Ending presentation time stamp.</span></span> |

## <span data-ttu-id="0f950-366"><a name="StreamDispositionType"></a>StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="0f950-366"><a name="StreamDispositionType"></a> StreamDispositionType</span></span>
<span data-ttu-id="0f950-367">**StreamDispositionType** je globální komplexní typ, který popisuje datového proudu.</span><span class="sxs-lookup"><span data-stu-id="0f950-367">**StreamDispositionType** is a global complex type that describes the stream.</span></span>  

<span data-ttu-id="0f950-368">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-368">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="0f950-369">Atributy</span><span class="sxs-lookup"><span data-stu-id="0f950-369">Attributes</span></span>
| <span data-ttu-id="0f950-370">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-370">Name</span></span> | <span data-ttu-id="0f950-371">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-371">Type</span></span> | <span data-ttu-id="0f950-372">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-372">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-373">**Výchozí**</span><span class="sxs-lookup"><span data-stu-id="0f950-373">**Default**</span></span><br /><br /> <span data-ttu-id="0f950-374">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-374">Required</span></span> |<span data-ttu-id="0f950-375">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-375">**xs:int**</span></span> |<span data-ttu-id="0f950-376">Tento atribut lze nastavte na hodnotu 1 se indikovat, že toto je výchozí prezentace.</span><span class="sxs-lookup"><span data-stu-id="0f950-376">Set this attribute to 1 to indicate this is the default presentation.</span></span> |
| <span data-ttu-id="0f950-377">**Dub**</span><span class="sxs-lookup"><span data-stu-id="0f950-377">**Dub**</span></span><br /><br /> <span data-ttu-id="0f950-378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-378">Required</span></span> |<span data-ttu-id="0f950-379">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-379">**xs:int**</span></span> |<span data-ttu-id="0f950-380">Tento atribut lze nastavte na hodnotu 1 se indikovat, že to dubbed prezentaci.</span><span class="sxs-lookup"><span data-stu-id="0f950-380">Set this attribute to 1 to indicate this is the dubbed presentation.</span></span> |
| <span data-ttu-id="0f950-381">**Původní**</span><span class="sxs-lookup"><span data-stu-id="0f950-381">**Original**</span></span><br /><br /> <span data-ttu-id="0f950-382">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-382">Required</span></span> |<span data-ttu-id="0f950-383">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-383">**xs:int**</span></span> |<span data-ttu-id="0f950-384">Tento atribut lze nastavte na hodnotu 1 se indikovat, že to původní prezentaci.</span><span class="sxs-lookup"><span data-stu-id="0f950-384">Set this attribute to 1 to indicate this is the original presentation.</span></span> |
| <span data-ttu-id="0f950-385">**Komentář**</span><span class="sxs-lookup"><span data-stu-id="0f950-385">**Comment**</span></span><br /><br /> <span data-ttu-id="0f950-386">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-386">Required</span></span> |<span data-ttu-id="0f950-387">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-387">**xs:int**</span></span> |<span data-ttu-id="0f950-388">Tento atribut lze nastavte na hodnotu 1 se indikovat, že tento stopy obsahuje komentáře.</span><span class="sxs-lookup"><span data-stu-id="0f950-388">Set this attribute to 1 to indicate this track contains commentary.</span></span> |
| <span data-ttu-id="0f950-389">**Texty**</span><span class="sxs-lookup"><span data-stu-id="0f950-389">**Lyrics**</span></span><br /><br /> <span data-ttu-id="0f950-390">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-390">Required</span></span> |<span data-ttu-id="0f950-391">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-391">**xs:int**</span></span> |<span data-ttu-id="0f950-392">Tento atribut lze nastavte na hodnotu 1 se indikovat, že tento stopy obsahuje text.</span><span class="sxs-lookup"><span data-stu-id="0f950-392">Set this attribute to 1 to indicate this track contains lyrics.</span></span> |
| <span data-ttu-id="0f950-393">**Karaoke**</span><span class="sxs-lookup"><span data-stu-id="0f950-393">**Karaoke**</span></span><br /><br /> <span data-ttu-id="0f950-394">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-394">Required</span></span> |<span data-ttu-id="0f950-395">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-395">**xs:int**</span></span> |<span data-ttu-id="0f950-396">Tento atribut lze nastavte na hodnotu 1 se indikovat, že to představuje karaoke sledování (pozadí Hudba, žádné hlasy zpěváků).</span><span class="sxs-lookup"><span data-stu-id="0f950-396">Set this attribute to 1 to indicate this represents the karaoke track (background music, no vocals).</span></span> |
| <span data-ttu-id="0f950-397">**Vynutit**</span><span class="sxs-lookup"><span data-stu-id="0f950-397">**Forced**</span></span><br /><br /> <span data-ttu-id="0f950-398">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-398">Required</span></span> |<span data-ttu-id="0f950-399">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-399">**xs:int**</span></span> |<span data-ttu-id="0f950-400">Tento atribut lze nastavte na hodnotu 1 se indikovat, že to vynucené prezentace.</span><span class="sxs-lookup"><span data-stu-id="0f950-400">Set this attribute to 1 to indicate this is the forced presentation.</span></span> |
| <span data-ttu-id="0f950-401">**HearingImpaired**</span><span class="sxs-lookup"><span data-stu-id="0f950-401">**HearingImpaired**</span></span><br /><br /> <span data-ttu-id="0f950-402">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-402">Required</span></span> |<span data-ttu-id="0f950-403">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-403">**xs:int**</span></span> |<span data-ttu-id="0f950-404">Tento atribut lze nastavte na hodnotu 1 se indikovat, že tento stopy pro vady sluchu.</span><span class="sxs-lookup"><span data-stu-id="0f950-404">Set this attribute to 1 to indicate this track is for the hearing impaired.</span></span> |
| <span data-ttu-id="0f950-405">**VisualImpaired**</span><span class="sxs-lookup"><span data-stu-id="0f950-405">**VisualImpaired**</span></span><br /><br /> <span data-ttu-id="0f950-406">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-406">Required</span></span> |<span data-ttu-id="0f950-407">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-407">**xs:int**</span></span> |<span data-ttu-id="0f950-408">Tento atribut lze nastavte na hodnotu 1 se indikovat, že tento stopy pro uživatele se zrakovým postižením.</span><span class="sxs-lookup"><span data-stu-id="0f950-408">Set this attribute to 1 to indicate this track is for the visually impaired.</span></span> |
| <span data-ttu-id="0f950-409">**CleanEffects**</span><span class="sxs-lookup"><span data-stu-id="0f950-409">**CleanEffects**</span></span><br /><br /> <span data-ttu-id="0f950-410">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-410">Required</span></span> |<span data-ttu-id="0f950-411">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-411">**xs:int**</span></span> |<span data-ttu-id="0f950-412">Nastavte tento atribut na 1 se indikovat, že tento stopy má čistou důsledky.</span><span class="sxs-lookup"><span data-stu-id="0f950-412">Set this attribute to 1 to indicate this track has clean effects.</span></span> |
| <span data-ttu-id="0f950-413">**AttachedPic**</span><span class="sxs-lookup"><span data-stu-id="0f950-413">**AttachedPic**</span></span><br /><br /> <span data-ttu-id="0f950-414">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0f950-414">Required</span></span> |<span data-ttu-id="0f950-415">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="0f950-415">**xs:int**</span></span> |<span data-ttu-id="0f950-416">Tento atribut lze nastavte na hodnotu 1 se indikovat, že tento stopy má obrázky.</span><span class="sxs-lookup"><span data-stu-id="0f950-416">Set this attribute to 1 to indicate this track has pictures.</span></span> |

## <span data-ttu-id="0f950-417"><a name="Programs"></a>Element programy</span><span class="sxs-lookup"><span data-stu-id="0f950-417"><a name="Programs"></a> Programs element</span></span>
<span data-ttu-id="0f950-418">Element obálky, která uchovává více **Program** elementy.</span><span class="sxs-lookup"><span data-stu-id="0f950-418">Wrapper element holding multiple **Program** elements.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="0f950-419">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="0f950-419">Child elements</span></span>
| <span data-ttu-id="0f950-420">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-420">Name</span></span> | <span data-ttu-id="0f950-421">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-421">Type</span></span> | <span data-ttu-id="0f950-422">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-422">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-423">**Program**</span><span class="sxs-lookup"><span data-stu-id="0f950-423">**Program**</span></span><br /><br /> <span data-ttu-id="0f950-424">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="0f950-424">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="0f950-425">ProgramType</span><span class="sxs-lookup"><span data-stu-id="0f950-425">ProgramType</span></span>](media-services-input-metadata-schema.md#ProgramType) |<span data-ttu-id="0f950-426">Soubory prostředků, které jsou ve formátu MPEG-TS obsahuje informace o aplikacích v souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-426">For asset files that are in MPEG-TS format, contains information about programs in the asset file.</span></span> |

## <span data-ttu-id="0f950-427"><a name="VideoTracks"></a>VideoTracks element</span><span class="sxs-lookup"><span data-stu-id="0f950-427"><a name="VideoTracks"></a> VideoTracks element</span></span>
 <span data-ttu-id="0f950-428">Element obálky, která uchovává více **VideoTrack** elementy.</span><span class="sxs-lookup"><span data-stu-id="0f950-428">Wrapper element holding multiple **VideoTrack** elements.</span></span>  

 <span data-ttu-id="0f950-429">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-429">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="0f950-430">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="0f950-430">Child elements</span></span>
| <span data-ttu-id="0f950-431">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-431">Name</span></span> | <span data-ttu-id="0f950-432">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-432">Type</span></span> | <span data-ttu-id="0f950-433">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-433">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-434">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="0f950-434">**VideoTrack**</span></span><br /><br /> <span data-ttu-id="0f950-435">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="0f950-435">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="0f950-436">VideoTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="0f950-436">VideoTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#VideoTrackType) |<span data-ttu-id="0f950-437">Obsahuje informace o video sleduje v souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-437">Contains information about video tracks in the asset file.</span></span> |

## <span data-ttu-id="0f950-438"><a name="AudioTracks"></a>AudioTracks element</span><span class="sxs-lookup"><span data-stu-id="0f950-438"><a name="AudioTracks"></a> AudioTracks element</span></span>
 <span data-ttu-id="0f950-439">Element obálky, která uchovává více **AudioTrack** elementy.</span><span class="sxs-lookup"><span data-stu-id="0f950-439">Wrapper element holding multiple **AudioTrack** elements.</span></span>  

 <span data-ttu-id="0f950-440">Podívejte se příklad XML na konci tohoto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="0f950-440">See an XML example at the end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="elements"></a><span data-ttu-id="0f950-441">Elementy</span><span class="sxs-lookup"><span data-stu-id="0f950-441">elements</span></span>
| <span data-ttu-id="0f950-442">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0f950-442">Name</span></span> | <span data-ttu-id="0f950-443">Typ</span><span class="sxs-lookup"><span data-stu-id="0f950-443">Type</span></span> | <span data-ttu-id="0f950-444">Popis</span><span class="sxs-lookup"><span data-stu-id="0f950-444">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0f950-445">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="0f950-445">**AudioTrack**</span></span><br /><br /> <span data-ttu-id="0f950-446">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="0f950-446">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="0f950-447">AudioTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="0f950-447">AudioTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#AudioTrackType) |<span data-ttu-id="0f950-448">Obsahuje informace o zvukových stop v souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="0f950-448">Contains information about audio tracks in the asset file.</span></span> |

## <span data-ttu-id="0f950-449"><a name="code"></a>Schéma kódu</span><span class="sxs-lookup"><span data-stu-id="0f950-449"><a name="code"></a> Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
        <xs:attribute name="Id" use="required">  
          <xs:annotation>  
            <xs:documentation>zero-based index of this video track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Width" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video width in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Height" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video height in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
              <xs:annotation>  
                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:decimal">  
                  <xs:minInclusive value="0"/>  
                  <xs:fractionDigits value="3"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="MaxGOPBitrate">  
              <xs:annotation>  
                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Channels" use="required">  
              <xs:annotation>  
                <xs:documentation>number of audio channels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SamplingRate" use="required">  
              <xs:annotation>  
                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for the encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is the collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>the media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of the asset file in kbps</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:int">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  


## <span data-ttu-id="0f950-450"><a name="xml"></a>Ukázkový kód XML</span><span class="sxs-lookup"><span data-stu-id="0f950-450"><a name="xml"></a> XML example</span></span>
<span data-ttu-id="0f950-451">Následuje příklad vstupního souboru metadat.</span><span class="sxs-lookup"><span data-stu-id="0f950-451">The following is an example of the Input metadata file.</span></span>  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a><span data-ttu-id="0f950-452">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f950-452">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0f950-453">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="0f950-453">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

