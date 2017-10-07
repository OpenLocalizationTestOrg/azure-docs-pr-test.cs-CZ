---
title: "schéma vstupu metadata aaaAzure Media Services | Microsoft Docs"
description: "Hello téma nabízí přehled Azure Media Services vstupní metadata schématu."
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
ms.openlocfilehash: 9b72c6ff317aa98451ea75548465dc6023b44a55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="input-metadata"></a><span data-ttu-id="87b93-103">Vstupní metadat</span><span class="sxs-lookup"><span data-stu-id="87b93-103">Input Metadata</span></span>
<span data-ttu-id="87b93-104">Kódování úlohy jsou přiřazeny prostředek vstupní (nebo prostředky) na kterém chcete tooperform některé úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="87b93-104">An encoding job is associated with an input asset (or assets) on which you want tooperform some encoding tasks.</span></span>  <span data-ttu-id="87b93-105">Po dokončení úlohy se vytvářejí výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="87b93-105">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="87b93-106">Hello výstupní asset obsahuje video, zvuk, miniatur manifestu, atd. hello výstupní asset obsahuje také soubor s metadata o vstupní asset hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-106">hello output asset contains video, audio, thumbnails, manifest, etc. hello output asset also contains a file with metadata about hello input asset.</span></span> <span data-ttu-id="87b93-107">Hello název souboru XML metadat hello má hello následující formát: &lt;asset_id&gt;_metadata.xml (například 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), kde &lt;asset_id&gt; je hello ID Hodnota hello vstupní asset.</span><span class="sxs-lookup"><span data-stu-id="87b93-107">hello name of hello metadata XML file has hello following format: &lt;asset_id&gt;_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where &lt;asset_id&gt; is hello AssetId value of hello input asset.</span></span>  

<span data-ttu-id="87b93-108">Pokud chcete soubor metadat hello tooexamine, můžete vytvořit **SAS** Lokátor a stahování hello souboru tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="87b93-108">If you want tooexamine hello metadata file, you can create a **SAS** locator and download hello file tooyour local computer.</span></span> <span data-ttu-id="87b93-109">Příklad najdete na postupy toocreate lokátoru SAS a stáhnout soubor [pomocí rozšíření sady SDK pro .NET hello Media Services](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="87b93-109">You can find an example on how toocreate a SAS locator and download a file  [Using hello Media Services .NET SDK Extensions](media-services-dotnet-get-started.md).</span></span>  

<span data-ttu-id="87b93-110">Toto téma popisuje elementy hello a typy schématu XML hello na které vstupní metada hello (&lt;asset_id&gt;_metadata.xml) je založena.</span><span class="sxs-lookup"><span data-stu-id="87b93-110">This topic discusses hello elements and types of hello XML schema on which hello input metada (&lt;asset_id&gt;_metadata.xml) is based.</span></span>  <span data-ttu-id="87b93-111">Informace o hello souboru, který obsahuje metadata o hello výstupní asset najdete v tématu [výstup metadat](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="87b93-111">For information about hello file that contains metadata about hello output asset, see [Output Metadata](media-services-output-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="87b93-112">Můžete najít hello [kód schématu](media-services-input-metadata-schema.md#code) [ukázkový kód XML](media-services-input-metadata-schema.md#xml) na konci hello v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="87b93-112">You can find hello [Schema Code](media-services-input-metadata-schema.md#code) an [XML example](media-services-input-metadata-schema.md#xml) at hello end of this topic.</span></span>  
> 
> 

## <span data-ttu-id="87b93-113"><a name="AssetFiles"></a>Element AssetFiles (kořenový element)</span><span class="sxs-lookup"><span data-stu-id="87b93-113"><a name="AssetFiles"></a> AssetFiles element (root element)</span></span>
<span data-ttu-id="87b93-114">Obsahuje kolekci [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s pro úlohy kódování hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-114">Contains a collection of [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s for hello encoding job.</span></span>  

<span data-ttu-id="87b93-115">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-115">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

| <span data-ttu-id="87b93-116">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-116">Name</span></span> | <span data-ttu-id="87b93-117">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="87b93-118">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="87b93-118">**AssetFile**</span></span><br /><br /> <span data-ttu-id="87b93-119">Hodnota minOccurs = "1" maxOccurs = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="87b93-119">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="87b93-120">Jeden podřízený element.</span><span class="sxs-lookup"><span data-stu-id="87b93-120">A single child element.</span></span> <span data-ttu-id="87b93-121">Další informace najdete v tématu [AssetFile element](media-services-input-metadata-schema.md#AssetFile).</span><span class="sxs-lookup"><span data-stu-id="87b93-121">For more information, see [AssetFile element](media-services-input-metadata-schema.md#AssetFile).</span></span> |

## <span data-ttu-id="87b93-122"><a name="AssetFile"></a>AssetFile element</span><span class="sxs-lookup"><span data-stu-id="87b93-122"><a name="AssetFile"></a> AssetFile element</span></span>
 <span data-ttu-id="87b93-123">Obsahuje atributy a elementy, které popisují soubor asset.</span><span class="sxs-lookup"><span data-stu-id="87b93-123">Contains attributes and elements that describe an asset file.</span></span>  

 <span data-ttu-id="87b93-124">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-124">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-125">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-125">Attributes</span></span>
| <span data-ttu-id="87b93-126">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-126">Name</span></span> | <span data-ttu-id="87b93-127">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-127">Type</span></span> | <span data-ttu-id="87b93-128">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-128">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-129">**Název**</span><span class="sxs-lookup"><span data-stu-id="87b93-129">**Name**</span></span><br /><br /> <span data-ttu-id="87b93-130">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-130">Required</span></span> |<span data-ttu-id="87b93-131">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-131">**xs:string**</span></span> |<span data-ttu-id="87b93-132">Název souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="87b93-132">Asset file name.</span></span> |
| <span data-ttu-id="87b93-133">**Velikost**</span><span class="sxs-lookup"><span data-stu-id="87b93-133">**Size**</span></span><br /><br /> <span data-ttu-id="87b93-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-134">Required</span></span> |<span data-ttu-id="87b93-135">**xs:Long**</span><span class="sxs-lookup"><span data-stu-id="87b93-135">**xs:long**</span></span> |<span data-ttu-id="87b93-136">Velikost souboru assetu hello v bajtech.</span><span class="sxs-lookup"><span data-stu-id="87b93-136">Size of hello asset file in bytes.</span></span> |
| <span data-ttu-id="87b93-137">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="87b93-137">**Duration**</span></span><br /><br /> <span data-ttu-id="87b93-138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-138">Required</span></span> |<span data-ttu-id="87b93-139">**xs**</span><span class="sxs-lookup"><span data-stu-id="87b93-139">**xs:duration**</span></span> |<span data-ttu-id="87b93-140">Obsahu play back doba trvání.</span><span class="sxs-lookup"><span data-stu-id="87b93-140">Content play back duration.</span></span> <span data-ttu-id="87b93-141">Příklad: Doba trvání = "PT25M37.757S".</span><span class="sxs-lookup"><span data-stu-id="87b93-141">Example: Duration="PT25M37.757S".</span></span> |
| <span data-ttu-id="87b93-142">**NumberOfStreams**</span><span class="sxs-lookup"><span data-stu-id="87b93-142">**NumberOfStreams**</span></span><br /><br /> <span data-ttu-id="87b93-143">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-143">Required</span></span> |<span data-ttu-id="87b93-144">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-144">**xs:int**</span></span> |<span data-ttu-id="87b93-145">Počet datových proudů v souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-145">Number of streams in hello asset file.</span></span> |
| <span data-ttu-id="87b93-146">**FormatNames**</span><span class="sxs-lookup"><span data-stu-id="87b93-146">**FormatNames**</span></span><br /><br /> <span data-ttu-id="87b93-147">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-147">Required</span></span> |<span data-ttu-id="87b93-148">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-148">**xs:string**</span></span> |<span data-ttu-id="87b93-149">Názvy ve formátu.</span><span class="sxs-lookup"><span data-stu-id="87b93-149">Format names.</span></span> |
| <span data-ttu-id="87b93-150">**FormatVerboseNames**</span><span class="sxs-lookup"><span data-stu-id="87b93-150">**FormatVerboseNames**</span></span><br /><br /> <span data-ttu-id="87b93-151">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-151">Required</span></span> |<span data-ttu-id="87b93-152">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-152">**xs:string**</span></span> |<span data-ttu-id="87b93-153">Podrobné názvy ve formátu.</span><span class="sxs-lookup"><span data-stu-id="87b93-153">Format verbose names.</span></span> |
| <span data-ttu-id="87b93-154">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="87b93-154">**StartTime**</span></span> |<span data-ttu-id="87b93-155">**xs**</span><span class="sxs-lookup"><span data-stu-id="87b93-155">**xs:duration**</span></span> |<span data-ttu-id="87b93-156">Čas zahájení obsahu.</span><span class="sxs-lookup"><span data-stu-id="87b93-156">Content start time.</span></span> <span data-ttu-id="87b93-157">Příklad: StartTime = "PT2.669S".</span><span class="sxs-lookup"><span data-stu-id="87b93-157">Example: StartTime="PT2.669S".</span></span> |
| <span data-ttu-id="87b93-158">**OverallBitRate**</span><span class="sxs-lookup"><span data-stu-id="87b93-158">**OverallBitRate**</span></span> |<span data-ttu-id="87b93-159">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-159">**xs:int**</span></span> |<span data-ttu-id="87b93-160">Průměrná přenosovou rychlostí hello asset souboru v kb/s.</span><span class="sxs-lookup"><span data-stu-id="87b93-160">Average bitrate of hello asset file in kbps.</span></span> |

> [!NOTE]
> <span data-ttu-id="87b93-161">Hello následující 4 podřízené elementy musí být uvedena v pořadí.</span><span class="sxs-lookup"><span data-stu-id="87b93-161">hello following 4 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="87b93-162">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="87b93-162">Child elements</span></span>
| <span data-ttu-id="87b93-163">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-163">Name</span></span> | <span data-ttu-id="87b93-164">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-164">Type</span></span> | <span data-ttu-id="87b93-165">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-166">**Programy**</span><span class="sxs-lookup"><span data-stu-id="87b93-166">**Programs**</span></span><br /><br /> <span data-ttu-id="87b93-167">Hodnota minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="87b93-167">minOccurs="0"</span></span> | |<span data-ttu-id="87b93-168">Kolekce všech [programy element](media-services-input-metadata-schema.md#Programs) po hello asset soubor ve formátu MPEG-TS.</span><span class="sxs-lookup"><span data-stu-id="87b93-168">Collection of all [Programs element](media-services-input-metadata-schema.md#Programs) when hello asset file is in MPEG-TS format.</span></span> |
| <span data-ttu-id="87b93-169">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="87b93-169">**VideoTracks**</span></span><br /><br /> <span data-ttu-id="87b93-170">Hodnota minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="87b93-170">minOccurs="0"</span></span> | |<span data-ttu-id="87b93-171">Každý fyzický prostředek soubor může obsahovat nula nebo více video sleduje prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="87b93-171">Each physical asset file can contain zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="87b93-172">Tento prvek obsahuje kolekci všech [VideoTracks element](media-services-input-metadata-schema.md#VideoTracks) které jsou součástí souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-172">This element contains a collection of all [VideoTracks element](media-services-input-metadata-schema.md#VideoTracks) that are part of hello asset file.</span></span> |
| <span data-ttu-id="87b93-173">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="87b93-173">**AudioTracks**</span></span><br /><br /> <span data-ttu-id="87b93-174">Hodnota minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="87b93-174">minOccurs="0"</span></span> | |<span data-ttu-id="87b93-175">Každý fyzický prostředek soubor může obsahovat nula nebo více zvukových stop prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="87b93-175">Each physical asset file can contain zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="87b93-176">Tento prvek obsahuje kolekci všech [AudioTracks element](media-services-input-metadata-schema.md#AudioTracks) které jsou součástí souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-176">This element contains a collection of all [AudioTracks element](media-services-input-metadata-schema.md#AudioTracks) that are part of hello asset file.</span></span> |
| <span data-ttu-id="87b93-177">**Metadata**</span><span class="sxs-lookup"><span data-stu-id="87b93-177">**Metadata**</span></span><br /><br /> <span data-ttu-id="87b93-178">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="87b93-178">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="87b93-179">MetadataType</span><span class="sxs-lookup"><span data-stu-id="87b93-179">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="87b93-180">Metadata souboru assetu vyjádřené key\value řetězce.</span><span class="sxs-lookup"><span data-stu-id="87b93-180">Asset file’s metadata represented as key\value strings.</span></span> <span data-ttu-id="87b93-181">Například:</span><span class="sxs-lookup"><span data-stu-id="87b93-181">For example:</span></span><br /><br /> <span data-ttu-id="87b93-182">**&lt;Metadata key = "jazyk" value = "eng" /&gt;**</span><span class="sxs-lookup"><span data-stu-id="87b93-182">**&lt;Metadata key="language" value="eng" /&gt;**</span></span> |

## <span data-ttu-id="87b93-183"><a name="TrackType"></a>TrackType</span><span class="sxs-lookup"><span data-stu-id="87b93-183"><a name="TrackType"></a> TrackType</span></span>
<span data-ttu-id="87b93-184">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-184">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-185">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-185">Attributes</span></span>
| <span data-ttu-id="87b93-186">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-186">Name</span></span> | <span data-ttu-id="87b93-187">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-187">Type</span></span> | <span data-ttu-id="87b93-188">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-188">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-189">**ID**</span><span class="sxs-lookup"><span data-stu-id="87b93-189">**Id**</span></span><br /><br /> <span data-ttu-id="87b93-190">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-190">Required</span></span> |<span data-ttu-id="87b93-191">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-191">**xs:int**</span></span> |<span data-ttu-id="87b93-192">Index nule tento dráhy zvuku a videa.</span><span class="sxs-lookup"><span data-stu-id="87b93-192">Zero-based index of this audio or video track.</span></span><br /><br /> <span data-ttu-id="87b93-193">Nemusí se jednat této hello TrackID jako použít v souboru MP4.</span><span class="sxs-lookup"><span data-stu-id="87b93-193">This is not necessarily that hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="87b93-194">**Kodeků**</span><span class="sxs-lookup"><span data-stu-id="87b93-194">**Codec**</span></span> |<span data-ttu-id="87b93-195">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-195">**xs:string**</span></span> |<span data-ttu-id="87b93-196">Řetězec kodeků sledovat videa.</span><span class="sxs-lookup"><span data-stu-id="87b93-196">Video track codec string.</span></span> |
| <span data-ttu-id="87b93-197">**CodecLongName**</span><span class="sxs-lookup"><span data-stu-id="87b93-197">**CodecLongName**</span></span> |<span data-ttu-id="87b93-198">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-198">**xs:string**</span></span> |<span data-ttu-id="87b93-199">Sledování zvuku a videa kodeků dlouhý název.</span><span class="sxs-lookup"><span data-stu-id="87b93-199">Audio or video track codec long name.</span></span> |
| <span data-ttu-id="87b93-200">**Časové základny**</span><span class="sxs-lookup"><span data-stu-id="87b93-200">**TimeBase**</span></span><br /><br /> <span data-ttu-id="87b93-201">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-201">Required</span></span> |<span data-ttu-id="87b93-202">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-202">**xs:string**</span></span> |<span data-ttu-id="87b93-203">Základní doba.</span><span class="sxs-lookup"><span data-stu-id="87b93-203">Time base.</span></span> <span data-ttu-id="87b93-204">Příklad: Časové základny = "1/48000"</span><span class="sxs-lookup"><span data-stu-id="87b93-204">Example: TimeBase="1/48000"</span></span> |
| <span data-ttu-id="87b93-205">**NumberOfFrames**</span><span class="sxs-lookup"><span data-stu-id="87b93-205">**NumberOfFrames**</span></span> |<span data-ttu-id="87b93-206">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-206">**xs:int**</span></span> |<span data-ttu-id="87b93-207">Počet rámců (k dispozici pro video sleduje).</span><span class="sxs-lookup"><span data-stu-id="87b93-207">Number of frames (present for video tracks).</span></span> |
| <span data-ttu-id="87b93-208">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="87b93-208">**StartTime**</span></span> |<span data-ttu-id="87b93-209">**xs**</span><span class="sxs-lookup"><span data-stu-id="87b93-209">**xs:duration**</span></span> |<span data-ttu-id="87b93-210">Čas zahájení sledovat.</span><span class="sxs-lookup"><span data-stu-id="87b93-210">Track start time.</span></span> <span data-ttu-id="87b93-211">Příklad: StartTime = "PT2.669S"</span><span class="sxs-lookup"><span data-stu-id="87b93-211">Example: StartTime="PT2.669S"</span></span> |
| <span data-ttu-id="87b93-212">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="87b93-212">**Duration**</span></span> |<span data-ttu-id="87b93-213">**xs**</span><span class="sxs-lookup"><span data-stu-id="87b93-213">**xs:duration**</span></span> |<span data-ttu-id="87b93-214">Sledování doby trvání.</span><span class="sxs-lookup"><span data-stu-id="87b93-214">Track duration.</span></span> <span data-ttu-id="87b93-215">Příklad: Doba trvání = "PTSampleFormat M37.757S".</span><span class="sxs-lookup"><span data-stu-id="87b93-215">Example: Duration="PTSampleFormat M37.757S".</span></span> |

> [!NOTE]
> <span data-ttu-id="87b93-216">Hello následující 2 podřízené elementy musí být uvedena v pořadí.</span><span class="sxs-lookup"><span data-stu-id="87b93-216">hello following 2 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="87b93-217">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="87b93-217">Child elements</span></span>
| <span data-ttu-id="87b93-218">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-218">Name</span></span> | <span data-ttu-id="87b93-219">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-219">Type</span></span> | <span data-ttu-id="87b93-220">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-220">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-221">**Dispozice**</span><span class="sxs-lookup"><span data-stu-id="87b93-221">**Disposition**</span></span><br /><br /> <span data-ttu-id="87b93-222">Hodnota minOccurs = maxOccurs "0" = "1"</span><span class="sxs-lookup"><span data-stu-id="87b93-222">minOccurs="0" maxOccurs="1"</span></span> |[<span data-ttu-id="87b93-223">StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="87b93-223">StreamDispositionType</span></span>](media-services-input-metadata-schema.md#StreamDispositionType) |<span data-ttu-id="87b93-224">Obsahuje informace prezentace (například jestli konkrétní audio track je pro slabozraké uživatele).</span><span class="sxs-lookup"><span data-stu-id="87b93-224">Contains presentation information (for example, whether a particular audio track is for visually impaired viewers).</span></span> |
| <span data-ttu-id="87b93-225">**Metadata**</span><span class="sxs-lookup"><span data-stu-id="87b93-225">**Metadata**</span></span><br /><br /> <span data-ttu-id="87b93-226">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="87b93-226">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="87b93-227">MetadataType</span><span class="sxs-lookup"><span data-stu-id="87b93-227">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="87b93-228">Obecné klíč/hodnota řetězce, které se dají použít toohold různé informace.</span><span class="sxs-lookup"><span data-stu-id="87b93-228">Generic key/value strings that can be used toohold a variety of information.</span></span> <span data-ttu-id="87b93-229">Například klíč = "jazyk" a hodnota = "eng".</span><span class="sxs-lookup"><span data-stu-id="87b93-229">For example, key=”language”, and value=”eng”.</span></span> |

## <span data-ttu-id="87b93-230"><a name="AudioTrackType"></a>AudioTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="87b93-230"><a name="AudioTrackType"></a> AudioTrackType (inherits from TrackType)</span></span>
 <span data-ttu-id="87b93-231">**AudioTrackType** je globální komplexní typ, který dědí z [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="87b93-231">**AudioTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

 <span data-ttu-id="87b93-232">Typ Hello představuje konkrétní zvuk sledovat v souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-232">hello type represents a specific audio track in hello asset file.</span></span>  

 <span data-ttu-id="87b93-233">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-233">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-234">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-234">Attributes</span></span>
| <span data-ttu-id="87b93-235">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-235">Name</span></span> | <span data-ttu-id="87b93-236">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-236">Type</span></span> | <span data-ttu-id="87b93-237">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-237">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-238">**SampleFormat**</span><span class="sxs-lookup"><span data-stu-id="87b93-238">**SampleFormat**</span></span> |<span data-ttu-id="87b93-239">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-239">**xs:string**</span></span> |<span data-ttu-id="87b93-240">Ukázka formátu.</span><span class="sxs-lookup"><span data-stu-id="87b93-240">Sample format.</span></span> |
| <span data-ttu-id="87b93-241">**ChannelLayout**</span><span class="sxs-lookup"><span data-stu-id="87b93-241">**ChannelLayout**</span></span> |<span data-ttu-id="87b93-242">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-242">**xs:string**</span></span> |<span data-ttu-id="87b93-243">Kanál rozložení.</span><span class="sxs-lookup"><span data-stu-id="87b93-243">Channel layout.</span></span> |
| <span data-ttu-id="87b93-244">**Kanály**</span><span class="sxs-lookup"><span data-stu-id="87b93-244">**Channels**</span></span><br /><br /> <span data-ttu-id="87b93-245">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-245">Required</span></span> |<span data-ttu-id="87b93-246">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-246">**xs:int**</span></span> |<span data-ttu-id="87b93-247">Počet (0 nebo více) zvukové kanály.</span><span class="sxs-lookup"><span data-stu-id="87b93-247">Number (0 or more) of audio channels.</span></span> |
| <span data-ttu-id="87b93-248">**SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="87b93-248">**SamplingRate**</span></span><br /><br /> <span data-ttu-id="87b93-249">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-249">Required</span></span> |<span data-ttu-id="87b93-250">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-250">**xs:int**</span></span> |<span data-ttu-id="87b93-251">Zvuk vzorkovací frekvenci v ukázky za sekundu nebo Hz.</span><span class="sxs-lookup"><span data-stu-id="87b93-251">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="87b93-252">**Přenosovou rychlostí**</span><span class="sxs-lookup"><span data-stu-id="87b93-252">**Bitrate**</span></span> |<span data-ttu-id="87b93-253">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-253">**xs:int**</span></span> |<span data-ttu-id="87b93-254">Průměrná přenosová rychlost zvuku v bitech za sekundu, počítané ze souboru asset hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-254">Average audio bit rate in bits per second, as calculated from hello asset file.</span></span> <span data-ttu-id="87b93-255">Pouze datové části Základní datový proud hello se počítá a režijní náklady na hello balení není zahrnut do tohoto počtu.</span><span class="sxs-lookup"><span data-stu-id="87b93-255">Only hello elementary stream payload is counted, and hello packaging overhead is not included in this count.</span></span> |
| <span data-ttu-id="87b93-256">**BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="87b93-256">**BitsPerSample**</span></span> |<span data-ttu-id="87b93-257">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-257">**xs:int**</span></span> |<span data-ttu-id="87b93-258">Zadejte bitů na vzorek pro formát wFormatTag hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-258">Bits per sample for hello wFormatTag format type.</span></span> |

## <span data-ttu-id="87b93-259"><a name="VideoTrackType"></a>VideoTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="87b93-259"><a name="VideoTrackType"></a> VideoTrackType (inherits from TrackType)</span></span>
<span data-ttu-id="87b93-260">**VideoTrackType** je globální komplexní typ, který dědí z [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="87b93-260">**VideoTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

<span data-ttu-id="87b93-261">Typ Hello představuje konkrétní video sledovat v souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-261">hello type represents a specific video track in hello asset file.</span></span>  

<span data-ttu-id="87b93-262">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-262">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-263">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-263">Attributes</span></span>
| <span data-ttu-id="87b93-264">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-264">Name</span></span> | <span data-ttu-id="87b93-265">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-265">Type</span></span> | <span data-ttu-id="87b93-266">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-266">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-267">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="87b93-267">**FourCC**</span></span><br /><br /> <span data-ttu-id="87b93-268">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-268">Required</span></span> |<span data-ttu-id="87b93-269">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-269">**xs:string**</span></span> |<span data-ttu-id="87b93-270">Video kodeků FourCC kódu.</span><span class="sxs-lookup"><span data-stu-id="87b93-270">Video codec FourCC code.</span></span> |
| <span data-ttu-id="87b93-271">**Profil**</span><span class="sxs-lookup"><span data-stu-id="87b93-271">**Profile**</span></span> |<span data-ttu-id="87b93-272">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-272">**xs:string**</span></span> |<span data-ttu-id="87b93-273">Video sledovat profil.</span><span class="sxs-lookup"><span data-stu-id="87b93-273">Video track's profile.</span></span> |
| <span data-ttu-id="87b93-274">**Úroveň**</span><span class="sxs-lookup"><span data-stu-id="87b93-274">**Level**</span></span> |<span data-ttu-id="87b93-275">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-275">**xs:string**</span></span> |<span data-ttu-id="87b93-276">Video sledovat úroveň.</span><span class="sxs-lookup"><span data-stu-id="87b93-276">Video track's level.</span></span> |
| <span data-ttu-id="87b93-277">**PixelFormat**</span><span class="sxs-lookup"><span data-stu-id="87b93-277">**PixelFormat**</span></span> |<span data-ttu-id="87b93-278">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-278">**xs:string**</span></span> |<span data-ttu-id="87b93-279">Video sledovat Pixelový formát.</span><span class="sxs-lookup"><span data-stu-id="87b93-279">Video track's pixel format.</span></span> |
| <span data-ttu-id="87b93-280">**Šířka**</span><span class="sxs-lookup"><span data-stu-id="87b93-280">**Width**</span></span><br /><br /> <span data-ttu-id="87b93-281">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-281">Required</span></span> |<span data-ttu-id="87b93-282">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-282">**xs:int**</span></span> |<span data-ttu-id="87b93-283">Kódovaný video šířku v pixelech.</span><span class="sxs-lookup"><span data-stu-id="87b93-283">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="87b93-284">**Výška**</span><span class="sxs-lookup"><span data-stu-id="87b93-284">**Height**</span></span><br /><br /> <span data-ttu-id="87b93-285">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-285">Required</span></span> |<span data-ttu-id="87b93-286">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-286">**xs:int**</span></span> |<span data-ttu-id="87b93-287">Kódovaný Výška videa v pixelech.</span><span class="sxs-lookup"><span data-stu-id="87b93-287">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="87b93-288">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="87b93-288">**DisplayAspectRatioNumerator**</span></span><br /><br /> <span data-ttu-id="87b93-289">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-289">Required</span></span> |<span data-ttu-id="87b93-290">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="87b93-290">**xs:double**</span></span> |<span data-ttu-id="87b93-291">Zobrazení videa čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="87b93-291">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="87b93-292">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="87b93-292">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="87b93-293">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-293">Required</span></span> |<span data-ttu-id="87b93-294">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="87b93-294">**xs:double**</span></span> |<span data-ttu-id="87b93-295">Jmenovatel grafického poměr stran.</span><span class="sxs-lookup"><span data-stu-id="87b93-295">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="87b93-296">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="87b93-296">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="87b93-297">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-297">Required</span></span> |<span data-ttu-id="87b93-298">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="87b93-298">**xs:double**</span></span> |<span data-ttu-id="87b93-299">Ukázkové video čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="87b93-299">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="87b93-300">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="87b93-300">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="87b93-301">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="87b93-301">**xs:double**</span></span> |<span data-ttu-id="87b93-302">Ukázkové video čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="87b93-302">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="87b93-303">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="87b93-303">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="87b93-304">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="87b93-304">**xs:double**</span></span> |<span data-ttu-id="87b93-305">Ukázkové video jmenovatel poměr stran.</span><span class="sxs-lookup"><span data-stu-id="87b93-305">Video sample aspect ratio denominator.</span></span> |
| <span data-ttu-id="87b93-306">**Kmitočet snímků**</span><span class="sxs-lookup"><span data-stu-id="87b93-306">**FrameRate**</span></span><br /><br /> <span data-ttu-id="87b93-307">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-307">Required</span></span> |<span data-ttu-id="87b93-308">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="87b93-308">**xs:decimal**</span></span> |<span data-ttu-id="87b93-309">Měří video obnovovací frekvence ve formátu .3f.</span><span class="sxs-lookup"><span data-stu-id="87b93-309">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="87b93-310">**Přenosovou rychlostí**</span><span class="sxs-lookup"><span data-stu-id="87b93-310">**Bitrate**</span></span> |<span data-ttu-id="87b93-311">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-311">**xs:int**</span></span> |<span data-ttu-id="87b93-312">Průměrná přenosová rychlost videa v kilobity za sekundu, počítané ze souboru asset hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-312">Average video bit rate in kilobits per second, as calculated from hello asset file.</span></span> <span data-ttu-id="87b93-313">Pouze datové části Základní datový proud hello se počítá a režijní náklady na hello balení není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="87b93-313">Only hello elementary stream payload is counted, and hello packaging overhead is not included.</span></span> |
| <span data-ttu-id="87b93-314">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="87b93-314">**MaxGOPBitrate**</span></span> |<span data-ttu-id="87b93-315">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-315">**xs:int**</span></span> |<span data-ttu-id="87b93-316">Maximální počet GOP průměrná přenosovou rychlostí pro tento stopy videa, v kB.</span><span class="sxs-lookup"><span data-stu-id="87b93-316">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |
| <span data-ttu-id="87b93-317">**HasBFrames**</span><span class="sxs-lookup"><span data-stu-id="87b93-317">**HasBFrames**</span></span> |<span data-ttu-id="87b93-318">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-318">**xs:int**</span></span> |<span data-ttu-id="87b93-319">Video sledovat počet rámců B.</span><span class="sxs-lookup"><span data-stu-id="87b93-319">Video track number of B frames.</span></span> |

## <span data-ttu-id="87b93-320"><a name="MetadataType"></a>MetadataType</span><span class="sxs-lookup"><span data-stu-id="87b93-320"><a name="MetadataType"></a> MetadataType</span></span>
<span data-ttu-id="87b93-321">**MetadataType** je globální komplexní typ, který popisuje metadata souboru asset jako klíč/hodnota řetězce.</span><span class="sxs-lookup"><span data-stu-id="87b93-321">**MetadataType** is a global complex type that describes metadata of an asset file as key/value strings.</span></span> <span data-ttu-id="87b93-322">Například klíč = "jazyk" a hodnota = "eng".</span><span class="sxs-lookup"><span data-stu-id="87b93-322">For example, key=”language”, and value=”eng”.</span></span>  

<span data-ttu-id="87b93-323">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-323">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-324">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-324">Attributes</span></span>
| <span data-ttu-id="87b93-325">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-325">Name</span></span> | <span data-ttu-id="87b93-326">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-326">Type</span></span> | <span data-ttu-id="87b93-327">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-327">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-328">**klíč**</span><span class="sxs-lookup"><span data-stu-id="87b93-328">**key**</span></span><br /><br /> <span data-ttu-id="87b93-329">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-329">Required</span></span> |<span data-ttu-id="87b93-330">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-330">**xs:string**</span></span> |<span data-ttu-id="87b93-331">Hello klíč v hello dvojice klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="87b93-331">hello key in hello key/value pair.</span></span> |
| <span data-ttu-id="87b93-332">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="87b93-332">**value**</span></span><br /><br /> <span data-ttu-id="87b93-333">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-333">Required</span></span> |<span data-ttu-id="87b93-334">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="87b93-334">**xs:string**</span></span> |<span data-ttu-id="87b93-335">Hello hodnota ve dvojici klíč/hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-335">hello value in hello key/value pair.</span></span> |

## <span data-ttu-id="87b93-336"><a name="ProgramType"></a>ProgramType</span><span class="sxs-lookup"><span data-stu-id="87b93-336"><a name="ProgramType"></a> ProgramType</span></span>
<span data-ttu-id="87b93-337">**ProgramType** je globální komplexní typ, který popisuje program.</span><span class="sxs-lookup"><span data-stu-id="87b93-337">**ProgramType** is a global complex type that describes a program.</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-338">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-338">Attributes</span></span>
| <span data-ttu-id="87b93-339">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-339">Name</span></span> | <span data-ttu-id="87b93-340">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-340">Type</span></span> | <span data-ttu-id="87b93-341">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-341">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-342">**ProgramId**</span><span class="sxs-lookup"><span data-stu-id="87b93-342">**ProgramId**</span></span><br /><br /> <span data-ttu-id="87b93-343">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-343">Required</span></span> |<span data-ttu-id="87b93-344">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-344">**xs:int**</span></span> |<span data-ttu-id="87b93-345">Id programu</span><span class="sxs-lookup"><span data-stu-id="87b93-345">Program Id</span></span> |
| <span data-ttu-id="87b93-346">**NumberOfPrograms**</span><span class="sxs-lookup"><span data-stu-id="87b93-346">**NumberOfPrograms**</span></span><br /><br /> <span data-ttu-id="87b93-347">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-347">Required</span></span> |<span data-ttu-id="87b93-348">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-348">**xs:int**</span></span> |<span data-ttu-id="87b93-349">Počet programy.</span><span class="sxs-lookup"><span data-stu-id="87b93-349">Number of programs.</span></span> |
| <span data-ttu-id="87b93-350">**PmtPid**</span><span class="sxs-lookup"><span data-stu-id="87b93-350">**PmtPid**</span></span><br /><br /> <span data-ttu-id="87b93-351">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-351">Required</span></span> |<span data-ttu-id="87b93-352">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-352">**xs:int**</span></span> |<span data-ttu-id="87b93-353">Program Mapa tabulky (PMTs) obsahují informace o programy.</span><span class="sxs-lookup"><span data-stu-id="87b93-353">Program Map Tables (PMTs) contain information about programs.</span></span>  <span data-ttu-id="87b93-354">Další informace najdete v tématu [platba](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span><span class="sxs-lookup"><span data-stu-id="87b93-354">For more information, see [PMt](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span></span> |
| <span data-ttu-id="87b93-355">**PcrPid**</span><span class="sxs-lookup"><span data-stu-id="87b93-355">**PcrPid**</span></span><br /><br /> <span data-ttu-id="87b93-356">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-356">Required</span></span> |<span data-ttu-id="87b93-357">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-357">**xs:int**</span></span> |<span data-ttu-id="87b93-358">Používá decoder.</span><span class="sxs-lookup"><span data-stu-id="87b93-358">Used by decoder.</span></span> <span data-ttu-id="87b93-359">Další informace najdete v tématu [nejvyššího počtu buněk](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span><span class="sxs-lookup"><span data-stu-id="87b93-359">For more information, see [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span></span> |
| <span data-ttu-id="87b93-360">**StartPTS**</span><span class="sxs-lookup"><span data-stu-id="87b93-360">**StartPTS**</span></span> |<span data-ttu-id="87b93-361">**xs: dlouho**</span><span class="sxs-lookup"><span data-stu-id="87b93-361">**xs: long**</span></span> |<span data-ttu-id="87b93-362">Spouštění prezentace časové razítko.</span><span class="sxs-lookup"><span data-stu-id="87b93-362">Starting presentation time stamp.</span></span> |
| <span data-ttu-id="87b93-363">**EndPTS**</span><span class="sxs-lookup"><span data-stu-id="87b93-363">**EndPTS**</span></span> |<span data-ttu-id="87b93-364">**xs: dlouho**</span><span class="sxs-lookup"><span data-stu-id="87b93-364">**xs: long**</span></span> |<span data-ttu-id="87b93-365">Ukončování prezentace časové razítko.</span><span class="sxs-lookup"><span data-stu-id="87b93-365">Ending presentation time stamp.</span></span> |

## <span data-ttu-id="87b93-366"><a name="StreamDispositionType"></a>StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="87b93-366"><a name="StreamDispositionType"></a> StreamDispositionType</span></span>
<span data-ttu-id="87b93-367">**StreamDispositionType** je globální komplexní typ, který popisuje hello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="87b93-367">**StreamDispositionType** is a global complex type that describes hello stream.</span></span>  

<span data-ttu-id="87b93-368">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-368">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="87b93-369">Atributy</span><span class="sxs-lookup"><span data-stu-id="87b93-369">Attributes</span></span>
| <span data-ttu-id="87b93-370">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-370">Name</span></span> | <span data-ttu-id="87b93-371">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-371">Type</span></span> | <span data-ttu-id="87b93-372">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-372">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-373">**Výchozí**</span><span class="sxs-lookup"><span data-stu-id="87b93-373">**Default**</span></span><br /><br /> <span data-ttu-id="87b93-374">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-374">Required</span></span> |<span data-ttu-id="87b93-375">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-375">**xs:int**</span></span> |<span data-ttu-id="87b93-376">Nastavte tento atribut too1 tooindicate, to je výchozí prezentace hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-376">Set this attribute too1 tooindicate this is hello default presentation.</span></span> |
| <span data-ttu-id="87b93-377">**Dub**</span><span class="sxs-lookup"><span data-stu-id="87b93-377">**Dub**</span></span><br /><br /> <span data-ttu-id="87b93-378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-378">Required</span></span> |<span data-ttu-id="87b93-379">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-379">**xs:int**</span></span> |<span data-ttu-id="87b93-380">Nastavte tento atribut too1 tooindicate je hello dabované prezentace.</span><span class="sxs-lookup"><span data-stu-id="87b93-380">Set this attribute too1 tooindicate this is hello dubbed presentation.</span></span> |
| <span data-ttu-id="87b93-381">**Původní**</span><span class="sxs-lookup"><span data-stu-id="87b93-381">**Original**</span></span><br /><br /> <span data-ttu-id="87b93-382">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-382">Required</span></span> |<span data-ttu-id="87b93-383">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-383">**xs:int**</span></span> |<span data-ttu-id="87b93-384">Nastavte tento atribut too1 tooindicate, to je původní prezentace hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-384">Set this attribute too1 tooindicate this is hello original presentation.</span></span> |
| <span data-ttu-id="87b93-385">**Komentář**</span><span class="sxs-lookup"><span data-stu-id="87b93-385">**Comment**</span></span><br /><br /> <span data-ttu-id="87b93-386">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-386">Required</span></span> |<span data-ttu-id="87b93-387">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-387">**xs:int**</span></span> |<span data-ttu-id="87b93-388">Nastavte tento atribut too1 tooindicate sledovat obsahuje komentáře.</span><span class="sxs-lookup"><span data-stu-id="87b93-388">Set this attribute too1 tooindicate this track contains commentary.</span></span> |
| <span data-ttu-id="87b93-389">**Texty**</span><span class="sxs-lookup"><span data-stu-id="87b93-389">**Lyrics**</span></span><br /><br /> <span data-ttu-id="87b93-390">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-390">Required</span></span> |<span data-ttu-id="87b93-391">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-391">**xs:int**</span></span> |<span data-ttu-id="87b93-392">Nastavte tento atribut too1 tooindicate sledovat obsahuje text.</span><span class="sxs-lookup"><span data-stu-id="87b93-392">Set this attribute too1 tooindicate this track contains lyrics.</span></span> |
| <span data-ttu-id="87b93-393">**Karaoke**</span><span class="sxs-lookup"><span data-stu-id="87b93-393">**Karaoke**</span></span><br /><br /> <span data-ttu-id="87b93-394">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-394">Required</span></span> |<span data-ttu-id="87b93-395">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-395">**xs:int**</span></span> |<span data-ttu-id="87b93-396">Nastavte tento atribut too1 tooindicate představuje hello karaoke sledování (pozadí Hudba, žádné hlasy zpěváků).</span><span class="sxs-lookup"><span data-stu-id="87b93-396">Set this attribute too1 tooindicate this represents hello karaoke track (background music, no vocals).</span></span> |
| <span data-ttu-id="87b93-397">**Vynutit**</span><span class="sxs-lookup"><span data-stu-id="87b93-397">**Forced**</span></span><br /><br /> <span data-ttu-id="87b93-398">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-398">Required</span></span> |<span data-ttu-id="87b93-399">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-399">**xs:int**</span></span> |<span data-ttu-id="87b93-400">Nastavte tento atribut too1 tooindicate, to je prezentace hello vynutit.</span><span class="sxs-lookup"><span data-stu-id="87b93-400">Set this attribute too1 tooindicate this is hello forced presentation.</span></span> |
| <span data-ttu-id="87b93-401">**HearingImpaired**</span><span class="sxs-lookup"><span data-stu-id="87b93-401">**HearingImpaired**</span></span><br /><br /> <span data-ttu-id="87b93-402">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-402">Required</span></span> |<span data-ttu-id="87b93-403">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-403">**xs:int**</span></span> |<span data-ttu-id="87b93-404">Nastavte tento atribut too1 tooindicate, ke kterému se tento stopy hello vady sluchu.</span><span class="sxs-lookup"><span data-stu-id="87b93-404">Set this attribute too1 tooindicate this track is for hello hearing impaired.</span></span> |
| <span data-ttu-id="87b93-405">**VisualImpaired**</span><span class="sxs-lookup"><span data-stu-id="87b93-405">**VisualImpaired**</span></span><br /><br /> <span data-ttu-id="87b93-406">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-406">Required</span></span> |<span data-ttu-id="87b93-407">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-407">**xs:int**</span></span> |<span data-ttu-id="87b93-408">Nastavte tento atribut too1 tooindicate, ke kterému se tento stopy hello se zrakovým postižením.</span><span class="sxs-lookup"><span data-stu-id="87b93-408">Set this attribute too1 tooindicate this track is for hello visually impaired.</span></span> |
| <span data-ttu-id="87b93-409">**CleanEffects**</span><span class="sxs-lookup"><span data-stu-id="87b93-409">**CleanEffects**</span></span><br /><br /> <span data-ttu-id="87b93-410">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-410">Required</span></span> |<span data-ttu-id="87b93-411">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-411">**xs:int**</span></span> |<span data-ttu-id="87b93-412">Nastavte tento atribut too1 tooindicate, která má toto sledování čistou účinky.</span><span class="sxs-lookup"><span data-stu-id="87b93-412">Set this attribute too1 tooindicate this track has clean effects.</span></span> |
| <span data-ttu-id="87b93-413">**AttachedPic**</span><span class="sxs-lookup"><span data-stu-id="87b93-413">**AttachedPic**</span></span><br /><br /> <span data-ttu-id="87b93-414">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87b93-414">Required</span></span> |<span data-ttu-id="87b93-415">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="87b93-415">**xs:int**</span></span> |<span data-ttu-id="87b93-416">Nastavte tento atribut too1 tooindicate, která má toto sledování obrázky.</span><span class="sxs-lookup"><span data-stu-id="87b93-416">Set this attribute too1 tooindicate this track has pictures.</span></span> |

## <span data-ttu-id="87b93-417"><a name="Programs"></a>Element programy</span><span class="sxs-lookup"><span data-stu-id="87b93-417"><a name="Programs"></a> Programs element</span></span>
<span data-ttu-id="87b93-418">Element obálky, která uchovává více **Program** elementy.</span><span class="sxs-lookup"><span data-stu-id="87b93-418">Wrapper element holding multiple **Program** elements.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="87b93-419">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="87b93-419">Child elements</span></span>
| <span data-ttu-id="87b93-420">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-420">Name</span></span> | <span data-ttu-id="87b93-421">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-421">Type</span></span> | <span data-ttu-id="87b93-422">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-422">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-423">**Program**</span><span class="sxs-lookup"><span data-stu-id="87b93-423">**Program**</span></span><br /><br /> <span data-ttu-id="87b93-424">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="87b93-424">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="87b93-425">ProgramType</span><span class="sxs-lookup"><span data-stu-id="87b93-425">ProgramType</span></span>](media-services-input-metadata-schema.md#ProgramType) |<span data-ttu-id="87b93-426">Soubory prostředků, které jsou ve formátu MPEG-TS obsahuje informace o aplikacích v souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-426">For asset files that are in MPEG-TS format, contains information about programs in hello asset file.</span></span> |

## <span data-ttu-id="87b93-427"><a name="VideoTracks"></a>VideoTracks element</span><span class="sxs-lookup"><span data-stu-id="87b93-427"><a name="VideoTracks"></a> VideoTracks element</span></span>
 <span data-ttu-id="87b93-428">Element obálky, která uchovává více **VideoTrack** elementy.</span><span class="sxs-lookup"><span data-stu-id="87b93-428">Wrapper element holding multiple **VideoTrack** elements.</span></span>  

 <span data-ttu-id="87b93-429">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-429">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="87b93-430">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="87b93-430">Child elements</span></span>
| <span data-ttu-id="87b93-431">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-431">Name</span></span> | <span data-ttu-id="87b93-432">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-432">Type</span></span> | <span data-ttu-id="87b93-433">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-433">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-434">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="87b93-434">**VideoTrack**</span></span><br /><br /> <span data-ttu-id="87b93-435">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="87b93-435">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="87b93-436">VideoTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="87b93-436">VideoTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#VideoTrackType) |<span data-ttu-id="87b93-437">Obsahuje informace o video sleduje v souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-437">Contains information about video tracks in hello asset file.</span></span> |

## <span data-ttu-id="87b93-438"><a name="AudioTracks"></a>AudioTracks element</span><span class="sxs-lookup"><span data-stu-id="87b93-438"><a name="AudioTracks"></a> AudioTracks element</span></span>
 <span data-ttu-id="87b93-439">Element obálky, která uchovává více **AudioTrack** elementy.</span><span class="sxs-lookup"><span data-stu-id="87b93-439">Wrapper element holding multiple **AudioTrack** elements.</span></span>  

 <span data-ttu-id="87b93-440">Prohlédněte si příklad XML na konci hello v tomto tématu: [ukázkový kód XML](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="87b93-440">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="elements"></a><span data-ttu-id="87b93-441">Elementy</span><span class="sxs-lookup"><span data-stu-id="87b93-441">elements</span></span>
| <span data-ttu-id="87b93-442">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87b93-442">Name</span></span> | <span data-ttu-id="87b93-443">Typ</span><span class="sxs-lookup"><span data-stu-id="87b93-443">Type</span></span> | <span data-ttu-id="87b93-444">Popis</span><span class="sxs-lookup"><span data-stu-id="87b93-444">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87b93-445">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="87b93-445">**AudioTrack**</span></span><br /><br /> <span data-ttu-id="87b93-446">Hodnota minOccurs = maxOccurs "0" = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="87b93-446">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="87b93-447">AudioTrackType (dědí z TrackType)</span><span class="sxs-lookup"><span data-stu-id="87b93-447">AudioTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#AudioTrackType) |<span data-ttu-id="87b93-448">Obsahuje informace o zvukových stop v souboru assetu hello.</span><span class="sxs-lookup"><span data-stu-id="87b93-448">Contains information about audio tracks in hello asset file.</span></span> |

## <span data-ttu-id="87b93-449"><a name="code"></a>Schéma kódu</span><span class="sxs-lookup"><span data-stu-id="87b93-449"><a name="code"></a> Schema Code</span></span>
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
            <xs:documentation>zero-based index of this video track. Note: this is not necessarily hello TrackID as used in an MP4 file</xs:documentation>  
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
          <xs:documentation>A specific video track in hello parent AssetFile</xs:documentation>  
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
                <xs:documentation>average video bit rate in kilobits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
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
          <xs:documentation>a specific audio track in hello parent AssetFile</xs:documentation>  
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
                <xs:documentation>average audio bit rate in bits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for hello wFormatTag format type</xs:documentation>  
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
          <xs:documentation>Collection of AssetFile entries for hello encoding job</xs:documentation>  
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
                      <xs:documentation>This is hello collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is hello collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is hello collection of all those audio tracks</xs:documentation>  
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
                    <xs:documentation>hello media asset file name</xs:documentation>  
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
                    <xs:documentation>average bitrate of hello asset file in kbps</xs:documentation>  
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


## <span data-ttu-id="87b93-450"><a name="xml"></a>Ukázkový kód XML</span><span class="sxs-lookup"><span data-stu-id="87b93-450"><a name="xml"></a> XML example</span></span>
<span data-ttu-id="87b93-451">Hello následuje příklad hello vstupu metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="87b93-451">hello following is an example of hello Input metadata file.</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="87b93-452">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87b93-452">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="87b93-453">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="87b93-453">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

