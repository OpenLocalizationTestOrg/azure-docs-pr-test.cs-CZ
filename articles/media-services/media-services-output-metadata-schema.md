---
title: "Azure Media Services výstup metadat schématu | Microsoft Docs"
description: "Téma nabízí přehled Azure Media Services výstup metadat schématu."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: c175d359f93e7cd8cd73aa498ad8b71c4ec497f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="output-metadata"></a><span data-ttu-id="99f0e-103">Výstup metadat</span><span class="sxs-lookup"><span data-stu-id="99f0e-103">Output Metadata</span></span>
## <a name="overview"></a><span data-ttu-id="99f0e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="99f0e-104">Overview</span></span>
<span data-ttu-id="99f0e-105">Kódování úlohy jsou přiřazeny prostředek vstupní (nebo prostředky) na který chcete provést některé úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="99f0e-105">An encoding job is associated with an input asset (or assets) on which you want to perform some encoding tasks.</span></span> <span data-ttu-id="99f0e-106">Můžete například kódovat soubor MP4 do sad H.264 MP4 s adaptivní přenosovou rychlostí; Vytvořte na miniaturu; Vytvořte překryvy.</span><span class="sxs-lookup"><span data-stu-id="99f0e-106">For example, encode an MP4 file to H.264 MP4 adaptive bitrate sets; create a thumbnail; create overlays.</span></span> <span data-ttu-id="99f0e-107">Po dokončení úlohy se vytvářejí výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="99f0e-107">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="99f0e-108">Výstupní asset obsahuje video, zvuk, miniatur, atd. Výstupní asset obsahuje také soubor s metadata o výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="99f0e-108">The output asset contains video, audio, thumbnails, etc. The output asset also contains a file with metadata about the output asset.</span></span> <span data-ttu-id="99f0e-109">Název souboru XML metadat má následující formát: &lt;source_file_name&gt;_manifest.xml (například BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-109">The name of the metadata XML file has the following format: &lt;source_file_name&gt;_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span>  

<span data-ttu-id="99f0e-110">Pokud chcete zkontrolovat soubor metadat, můžete vytvořit **SAS** Lokátor a stahování souborů do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="99f0e-110">If you want to examine the metadata file, you can create a **SAS** locator and download the file to your local computer.</span></span>  

<span data-ttu-id="99f0e-111">Toto téma popisuje elementy a typy schématu XML, na kterém metada výstup (&lt;source_file_name&gt;_manifest.xml) je založena.</span><span class="sxs-lookup"><span data-stu-id="99f0e-111">This topic discusses the elements and types of the XML schema on which the output metada (&lt;source_file_name&gt;_manifest.xml) is based.</span></span> <span data-ttu-id="99f0e-112">Informace o souboru, který obsahuje metadata o vstupní asset najdete v tématu [vstupní Metadata](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-112">For information about the file that contains metadata about the input asset, see [Input Metadata](media-services-input-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="99f0e-113">Můžete najít kód úplné schéma a ukázkový kód XML na konci tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-113">You can find the complete schema code and XML example at the end of this topic.</span></span>  
>
>

## <span data-ttu-id="99f0e-114"><a name="AssetFiles "></a>Kořenový element AssetFiles</span><span class="sxs-lookup"><span data-stu-id="99f0e-114"><a name="AssetFiles "></a> AssetFiles root element</span></span>
<span data-ttu-id="99f0e-115">Kolekce položek AssetFile pro úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="99f0e-115">Collection of AssetFile entries for the encoding job.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="99f0e-116">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="99f0e-116">Child elements</span></span>
| <span data-ttu-id="99f0e-117">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-117">Name</span></span> | <span data-ttu-id="99f0e-118">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-118">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99f0e-119">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="99f0e-119">**AssetFile**</span></span><br/><br/> <span data-ttu-id="99f0e-120">Hodnota minOccurs = maxOccurs "0" = "1"</span><span class="sxs-lookup"><span data-stu-id="99f0e-120">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="99f0e-121">[AssetFile element](media-services-output-metadata-schema.md) který je součástí AssetFiles kolekce.</span><span class="sxs-lookup"><span data-stu-id="99f0e-121">An [AssetFile element](media-services-output-metadata-schema.md) that is part of the AssetFiles collection.</span></span> |

## <span data-ttu-id="99f0e-122"><a name="AssetFile "></a>AssetFile element</span><span class="sxs-lookup"><span data-stu-id="99f0e-122"><a name="AssetFile "></a> AssetFile element</span></span>
<span data-ttu-id="99f0e-123">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-123">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="99f0e-124">Atributy</span><span class="sxs-lookup"><span data-stu-id="99f0e-124">Attributes</span></span>
| <span data-ttu-id="99f0e-125">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-125">Name</span></span> | <span data-ttu-id="99f0e-126">Typ</span><span class="sxs-lookup"><span data-stu-id="99f0e-126">Type</span></span> | <span data-ttu-id="99f0e-127">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99f0e-128">**Název**</span><span class="sxs-lookup"><span data-stu-id="99f0e-128">**Name**</span></span><br/><br/> <span data-ttu-id="99f0e-129">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-129">Required</span></span> |<span data-ttu-id="99f0e-130">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-130">**xs:string**</span></span> |<span data-ttu-id="99f0e-131">Název souboru asset média.</span><span class="sxs-lookup"><span data-stu-id="99f0e-131">The media asset file name.</span></span> |
| <span data-ttu-id="99f0e-132">**Velikost**</span><span class="sxs-lookup"><span data-stu-id="99f0e-132">**Size**</span></span><br/><br/> <span data-ttu-id="99f0e-133">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-133">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-134">Required</span></span> |<span data-ttu-id="99f0e-135">**xs:Long**</span><span class="sxs-lookup"><span data-stu-id="99f0e-135">**xs:long**</span></span> |<span data-ttu-id="99f0e-136">Velikost souboru assetu v bajtech.</span><span class="sxs-lookup"><span data-stu-id="99f0e-136">Size of the asset file in bytes.</span></span> |
| <span data-ttu-id="99f0e-137">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="99f0e-137">**Duration**</span></span><br/><br/> <span data-ttu-id="99f0e-138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-138">Required</span></span> |<span data-ttu-id="99f0e-139">**xs**</span><span class="sxs-lookup"><span data-stu-id="99f0e-139">**xs:duration**</span></span> |<span data-ttu-id="99f0e-140">Obsahu play back doba trvání.</span><span class="sxs-lookup"><span data-stu-id="99f0e-140">Content play back duration.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="99f0e-141">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="99f0e-141">Child elements</span></span>
| <span data-ttu-id="99f0e-142">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-142">Name</span></span> | <span data-ttu-id="99f0e-143">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99f0e-144">**Zdroje**</span><span class="sxs-lookup"><span data-stu-id="99f0e-144">**Sources**</span></span> |<span data-ttu-id="99f0e-145">Kolekce vstup nebo zdrojové soubory médií, který byl zpracován, aby bylo možné vytvořit tento AssetFile.</span><span class="sxs-lookup"><span data-stu-id="99f0e-145">Collection of input/source media files, that was processed in order to produce this AssetFile.</span></span> <span data-ttu-id="99f0e-146">Další informace najdete v tématu [Source element](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-146">For more information, see [Source element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="99f0e-147">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="99f0e-147">**VideoTracks**</span></span><br/><br/> <span data-ttu-id="99f0e-148">Hodnota minOccurs = maxOccurs "0" = "1"</span><span class="sxs-lookup"><span data-stu-id="99f0e-148">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="99f0e-149">Každý fyzický AssetFile může obsahovat v ní nula nebo více video sleduje prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="99f0e-149">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="99f0e-150">Toto je kolekce těchto video sleduje.</span><span class="sxs-lookup"><span data-stu-id="99f0e-150">This is the collection of all those video tracks.</span></span> <span data-ttu-id="99f0e-151">Další informace najdete v tématu [VideoTracks element](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-151">For more information, see [VideoTracks element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="99f0e-152">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="99f0e-152">**AudioTracks**</span></span><br/><br/> <span data-ttu-id="99f0e-153">Hodnota minOccurs = maxOccurs "0" = "1"</span><span class="sxs-lookup"><span data-stu-id="99f0e-153">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="99f0e-154">Každý fyzický AssetFile může obsahovat v ní nula nebo více zvukových stop prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="99f0e-154">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="99f0e-155">Toto je kolekce těchto zvukových stop.</span><span class="sxs-lookup"><span data-stu-id="99f0e-155">This is the collection of all those audio tracks.</span></span> <span data-ttu-id="99f0e-156">Další informace najdete v tématu [AudioTracks element](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-156">For more information, see [AudioTracks element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="99f0e-157"><a name="Sources "></a>Zdrojový element</span><span class="sxs-lookup"><span data-stu-id="99f0e-157"><a name="Sources "></a> Sources element</span></span>
<span data-ttu-id="99f0e-158">Kolekce vstup nebo zdrojové soubory médií, který byl zpracován, aby bylo možné vytvořit tento AssetFile.</span><span class="sxs-lookup"><span data-stu-id="99f0e-158">Collection of input/source media files, that was processed in order to produce this AssetFile.</span></span>  

<span data-ttu-id="99f0e-159">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-159">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="99f0e-160">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="99f0e-160">Child elements</span></span>
| <span data-ttu-id="99f0e-161">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-161">Name</span></span> | <span data-ttu-id="99f0e-162">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99f0e-163">**Zdroj**</span><span class="sxs-lookup"><span data-stu-id="99f0e-163">**Source**</span></span><br/><br/> <span data-ttu-id="99f0e-164">Hodnota minOccurs = "1" maxOccurs = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="99f0e-164">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="99f0e-165">Vstup/zdrojový soubor, používá při generování tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="99f0e-165">An input/source file used when generating this asset.</span></span> <span data-ttu-id="99f0e-166">Další informace najdete v části [Source element](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-166">For more information see [Source element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="99f0e-167"><a name="Source "></a>Source element</span><span class="sxs-lookup"><span data-stu-id="99f0e-167"><a name="Source "></a> Source element</span></span>
<span data-ttu-id="99f0e-168">Vstup/zdrojový soubor, používá při generování tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="99f0e-168">An input/source file used when generating this asset.</span></span>  

<span data-ttu-id="99f0e-169">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-169">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="99f0e-170">Atributy</span><span class="sxs-lookup"><span data-stu-id="99f0e-170">Attributes</span></span>
| <span data-ttu-id="99f0e-171">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-171">Name</span></span> | <span data-ttu-id="99f0e-172">Typ</span><span class="sxs-lookup"><span data-stu-id="99f0e-172">Type</span></span> | <span data-ttu-id="99f0e-173">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-173">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99f0e-174">**Název**</span><span class="sxs-lookup"><span data-stu-id="99f0e-174">**Name**</span></span><br/><br/> <span data-ttu-id="99f0e-175">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-175">Required</span></span> |<span data-ttu-id="99f0e-176">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-176">**xs:string**</span></span> |<span data-ttu-id="99f0e-177">Název souboru vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="99f0e-177">Input source file name.</span></span> |

## <span data-ttu-id="99f0e-178"><a name="VideoTracks "></a>VideoTracks element</span><span class="sxs-lookup"><span data-stu-id="99f0e-178"><a name="VideoTracks "></a> VideoTracks element</span></span>
<span data-ttu-id="99f0e-179">Každý fyzický AssetFile může obsahovat v ní nula nebo více video sleduje prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="99f0e-179">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="99f0e-180">Toto je kolekce těchto video sleduje.</span><span class="sxs-lookup"><span data-stu-id="99f0e-180">This is the collection of all those video tracks.</span></span>  

<span data-ttu-id="99f0e-181">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-181">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="99f0e-182">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="99f0e-182">Child elements</span></span>
| <span data-ttu-id="99f0e-183">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-183">Name</span></span> | <span data-ttu-id="99f0e-184">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99f0e-185">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="99f0e-185">**VideoTrack**</span></span><br/><br/> <span data-ttu-id="99f0e-186">Hodnota minOccurs = "1" maxOccurs = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="99f0e-186">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="99f0e-187">V nadřazené AssetFile sledovat konkrétní video.</span><span class="sxs-lookup"><span data-stu-id="99f0e-187">A specific video track in the parent AssetFile.</span></span> <span data-ttu-id="99f0e-188">Další informace najdete v tématu [VideoTrack element](media-services-output-metadata-schema.md#VideoTrack).</span><span class="sxs-lookup"><span data-stu-id="99f0e-188">For more information, see [VideoTrack element](media-services-output-metadata-schema.md#VideoTrack).</span></span> |

## <span data-ttu-id="99f0e-189"><a name="VideoTrack"></a>VideoTrack element</span><span class="sxs-lookup"><span data-stu-id="99f0e-189"><a name="VideoTrack"></a> VideoTrack element</span></span>
<span data-ttu-id="99f0e-190">V nadřazené AssetFile sledovat konkrétní video.</span><span class="sxs-lookup"><span data-stu-id="99f0e-190">A specific video track in the parent AssetFile.</span></span>  

<span data-ttu-id="99f0e-191">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-191">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="99f0e-192">Atributy</span><span class="sxs-lookup"><span data-stu-id="99f0e-192">Attributes</span></span>
| <span data-ttu-id="99f0e-193">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-193">Name</span></span> | <span data-ttu-id="99f0e-194">Typ</span><span class="sxs-lookup"><span data-stu-id="99f0e-194">Type</span></span> | <span data-ttu-id="99f0e-195">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-195">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99f0e-196">**ID**</span><span class="sxs-lookup"><span data-stu-id="99f0e-196">**Id**</span></span><br/><br/> <span data-ttu-id="99f0e-197">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-197">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-198">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-198">Required</span></span> |<span data-ttu-id="99f0e-199">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-199">**xs:int**</span></span> |<span data-ttu-id="99f0e-200">Index počítaný od nuly toto video sledovat. **Poznámka:** to není nezbytně TrackID jako použít v souboru MP4.</span><span class="sxs-lookup"><span data-stu-id="99f0e-200">Zero-based index of this video track. **Note:**  This is not necessarily the TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="99f0e-201">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="99f0e-201">**FourCC**</span></span><br/><br/> <span data-ttu-id="99f0e-202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-202">Required</span></span> |<span data-ttu-id="99f0e-203">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-203">**xs:string**</span></span> |<span data-ttu-id="99f0e-204">Video kodeků FourCC kódu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-204">Video codec FourCC code.</span></span> |
| <span data-ttu-id="99f0e-205">**Profil**</span><span class="sxs-lookup"><span data-stu-id="99f0e-205">**Profile**</span></span> |<span data-ttu-id="99f0e-206">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-206">**xs:string**</span></span> |<span data-ttu-id="99f0e-207">Profil H264 (platí jenom pro H264 kodeků).</span><span class="sxs-lookup"><span data-stu-id="99f0e-207">H264 profile (only applicable to H264 codec).</span></span> |
| <span data-ttu-id="99f0e-208">**Úroveň**</span><span class="sxs-lookup"><span data-stu-id="99f0e-208">**Level**</span></span> |<span data-ttu-id="99f0e-209">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-209">**xs:string**</span></span> |<span data-ttu-id="99f0e-210">Úroveň H264 (platí jenom pro H264 kodeků).</span><span class="sxs-lookup"><span data-stu-id="99f0e-210">H264 level (only applicable to H264 codec).</span></span> |
| <span data-ttu-id="99f0e-211">**Šířka**</span><span class="sxs-lookup"><span data-stu-id="99f0e-211">**Width**</span></span><br/><br/> <span data-ttu-id="99f0e-212">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-212">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-213">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-213">Required</span></span> |<span data-ttu-id="99f0e-214">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-214">**xs:int**</span></span> |<span data-ttu-id="99f0e-215">Kódovaný video šířku v pixelech.</span><span class="sxs-lookup"><span data-stu-id="99f0e-215">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="99f0e-216">**Výška**</span><span class="sxs-lookup"><span data-stu-id="99f0e-216">**Height**</span></span><br/><br/> <span data-ttu-id="99f0e-217">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-217">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-218">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-218">Required</span></span> |<span data-ttu-id="99f0e-219">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-219">**xs:int**</span></span> |<span data-ttu-id="99f0e-220">Kódovaný Výška videa v pixelech.</span><span class="sxs-lookup"><span data-stu-id="99f0e-220">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="99f0e-221">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="99f0e-221">**DisplayAspectRatioNumerator**</span></span><br/><br/> <span data-ttu-id="99f0e-222">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-222">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-223">Required</span></span> |<span data-ttu-id="99f0e-224">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="99f0e-224">**xs:double**</span></span> |<span data-ttu-id="99f0e-225">Zobrazení videa čítači poměr stran.</span><span class="sxs-lookup"><span data-stu-id="99f0e-225">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="99f0e-226">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="99f0e-226">**DisplayAspectRatioDenominator**</span></span><br/><br/> <span data-ttu-id="99f0e-227">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-227">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-228">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-228">Required</span></span> |<span data-ttu-id="99f0e-229">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="99f0e-229">**xs:double**</span></span> |<span data-ttu-id="99f0e-230">Jmenovatel grafického poměr stran.</span><span class="sxs-lookup"><span data-stu-id="99f0e-230">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="99f0e-231">**Kmitočet snímků**</span><span class="sxs-lookup"><span data-stu-id="99f0e-231">**Framerate**</span></span><br/><br/> <span data-ttu-id="99f0e-232">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-232">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-233">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-233">Required</span></span> |<span data-ttu-id="99f0e-234">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="99f0e-234">**xs:decimal**</span></span> |<span data-ttu-id="99f0e-235">Měří video obnovovací frekvence ve formátu .3f.</span><span class="sxs-lookup"><span data-stu-id="99f0e-235">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="99f0e-236">**TargetFramerate**</span><span class="sxs-lookup"><span data-stu-id="99f0e-236">**TargetFramerate**</span></span><br/><br/> <span data-ttu-id="99f0e-237">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-237">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-238">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-238">Required</span></span> |<span data-ttu-id="99f0e-239">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="99f0e-239">**xs:decimal**</span></span> |<span data-ttu-id="99f0e-240">Video obnovovací frekvence přednastavené cíl ve formátu .3f.</span><span class="sxs-lookup"><span data-stu-id="99f0e-240">Preset target video frame rate in .3f format.</span></span> |
| <span data-ttu-id="99f0e-241">**Přenosovou rychlostí**</span><span class="sxs-lookup"><span data-stu-id="99f0e-241">**Bitrate**</span></span><br/><br/> <span data-ttu-id="99f0e-242">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-242">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-243">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-243">Required</span></span> |<span data-ttu-id="99f0e-244">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-244">**xs:int**</span></span> |<span data-ttu-id="99f0e-245">Průměrná přenosová rychlost videa v počítané od AssetFile kilobity za sekundu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-245">Average video bit rate in kilobits per second, as calculated from the AssetFile.</span></span> <span data-ttu-id="99f0e-246">Počty jenom datové části Základní datový proud a nezahrnuje balení režii.</span><span class="sxs-lookup"><span data-stu-id="99f0e-246">Counts only the elementary stream payload, and does not include the packaging overhead.</span></span> |
| <span data-ttu-id="99f0e-247">**TargetBitrate**</span><span class="sxs-lookup"><span data-stu-id="99f0e-247">**TargetBitrate**</span></span><br/><br/> <span data-ttu-id="99f0e-248">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-248">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-249">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-249">Required</span></span> |<span data-ttu-id="99f0e-250">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-250">**xs:int**</span></span> |<span data-ttu-id="99f0e-251">Cíle průměrná přenosovou rychlostí pro toto video sledovat, jak si vyžádal prostřednictvím kódování přednastavené v kilobity za sekundu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-251">Target average bitrate for this video track, as requested via the encoding preset, in kilobits per second.</span></span> |
| <span data-ttu-id="99f0e-252">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="99f0e-252">**MaxGOPBitrate**</span></span><br/><br/> <span data-ttu-id="99f0e-253">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-253">minInclusive ="0"</span></span> |<span data-ttu-id="99f0e-254">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-254">**xs:int**</span></span> |<span data-ttu-id="99f0e-255">Maximální počet GOP průměrná přenosovou rychlostí pro tento stopy videa, v kB.</span><span class="sxs-lookup"><span data-stu-id="99f0e-255">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |

## <span data-ttu-id="99f0e-256"><a name="AudioTracks "></a>AudioTracks element</span><span class="sxs-lookup"><span data-stu-id="99f0e-256"><a name="AudioTracks "></a> AudioTracks element</span></span>
<span data-ttu-id="99f0e-257">Každý fyzický AssetFile může obsahovat v ní nula nebo více zvukových stop prokládaný do formátu odpovídajícího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="99f0e-257">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="99f0e-258">Toto je kolekce těchto zvukových stop.</span><span class="sxs-lookup"><span data-stu-id="99f0e-258">This is the collection of all those audio tracks.</span></span>  

<span data-ttu-id="99f0e-259">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-259">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="99f0e-260">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="99f0e-260">Child elements</span></span>
| <span data-ttu-id="99f0e-261">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-261">Name</span></span> | <span data-ttu-id="99f0e-262">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-262">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99f0e-263">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="99f0e-263">**AudioTrack**</span></span><br/><br/> <span data-ttu-id="99f0e-264">Hodnota minOccurs = "1" maxOccurs = "bez vazby"</span><span class="sxs-lookup"><span data-stu-id="99f0e-264">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="99f0e-265">V nadřazené AssetFile sledovat konkrétní zvukovém souboru.</span><span class="sxs-lookup"><span data-stu-id="99f0e-265">A specific audio track in the parent AssetFile.</span></span> <span data-ttu-id="99f0e-266">Další informace najdete v tématu [AudioTrack element](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-266">For more information, see [AudioTrack element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="99f0e-267"><a name="AudioTrack "></a>AudioTrack element</span><span class="sxs-lookup"><span data-stu-id="99f0e-267"><a name="AudioTrack "></a> AudioTrack element</span></span>
<span data-ttu-id="99f0e-268">V nadřazené AssetFile sledovat konkrétní zvukovém souboru.</span><span class="sxs-lookup"><span data-stu-id="99f0e-268">A specific audio track in the parent AssetFile.</span></span>  

<span data-ttu-id="99f0e-269">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-269">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="99f0e-270">Atributy</span><span class="sxs-lookup"><span data-stu-id="99f0e-270">Attributes</span></span>
| <span data-ttu-id="99f0e-271">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-271">Name</span></span> | <span data-ttu-id="99f0e-272">Typ</span><span class="sxs-lookup"><span data-stu-id="99f0e-272">Type</span></span> | <span data-ttu-id="99f0e-273">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-273">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99f0e-274">**ID**</span><span class="sxs-lookup"><span data-stu-id="99f0e-274">**Id**</span></span><br/><br/> <span data-ttu-id="99f0e-275">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-275">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-276">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-276">Required</span></span> |<span data-ttu-id="99f0e-277">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-277">**xs:int**</span></span> |<span data-ttu-id="99f0e-278">Index tento zvuk dráhy nule. **Poznámka:** to není nezbytně TrackID jako použít v souboru MP4.</span><span class="sxs-lookup"><span data-stu-id="99f0e-278">Zero-based index of this audio track. **Note:**  This is not necessarily the TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="99f0e-279">**Kodeků**</span><span class="sxs-lookup"><span data-stu-id="99f0e-279">**Codec**</span></span> |<span data-ttu-id="99f0e-280">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-280">**xs:string**</span></span> |<span data-ttu-id="99f0e-281">Zvuk sledovat kodeků řetězec.</span><span class="sxs-lookup"><span data-stu-id="99f0e-281">Audio track codec string.</span></span> |
| <span data-ttu-id="99f0e-282">**EncoderVersion**</span><span class="sxs-lookup"><span data-stu-id="99f0e-282">**EncoderVersion**</span></span> |<span data-ttu-id="99f0e-283">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-283">**xs:string**</span></span> |<span data-ttu-id="99f0e-284">Řetězec verze volitelné kodér požadované pro EAC3.</span><span class="sxs-lookup"><span data-stu-id="99f0e-284">Optional encoder version string, required for EAC3.</span></span> |
| <span data-ttu-id="99f0e-285">**Kanály**</span><span class="sxs-lookup"><span data-stu-id="99f0e-285">**Channels**</span></span><br/><br/> <span data-ttu-id="99f0e-286">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-286">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-287">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-287">Required</span></span> |<span data-ttu-id="99f0e-288">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-288">**xs:int**</span></span> |<span data-ttu-id="99f0e-289">Počet zvukových kanálů.</span><span class="sxs-lookup"><span data-stu-id="99f0e-289">Number of audio channels.</span></span> |
| <span data-ttu-id="99f0e-290">**SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="99f0e-290">**SamplingRate**</span></span><br/><br/> <span data-ttu-id="99f0e-291">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-291">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-292">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-292">Required</span></span> |<span data-ttu-id="99f0e-293">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-293">**xs:int**</span></span> |<span data-ttu-id="99f0e-294">Zvuk vzorkovací frekvenci v ukázky za sekundu nebo Hz.</span><span class="sxs-lookup"><span data-stu-id="99f0e-294">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="99f0e-295">**Přenosovou rychlostí**</span><span class="sxs-lookup"><span data-stu-id="99f0e-295">**Bitrate**</span></span><br/><br/> <span data-ttu-id="99f0e-296">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-296">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-297">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-297">Required</span></span> |<span data-ttu-id="99f0e-298">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-298">**xs:int**</span></span> |<span data-ttu-id="99f0e-299">Průměrná přenosová rychlost zvuku v bitech za sekundu, počítané od AssetFile.</span><span class="sxs-lookup"><span data-stu-id="99f0e-299">Average audio bit rate in bits per second, as calculated from the AssetFile.</span></span> <span data-ttu-id="99f0e-300">Počty jenom datové části Základní datový proud a nezahrnuje balení režii.</span><span class="sxs-lookup"><span data-stu-id="99f0e-300">Counts only the elementary stream payload, and does not include the packaging overhead.</span></span> |
| <span data-ttu-id="99f0e-301">**BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="99f0e-301">**BitsPerSample**</span></span><br/><br/> <span data-ttu-id="99f0e-302">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="99f0e-302">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="99f0e-303">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-303">Required</span></span> |<span data-ttu-id="99f0e-304">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-304">**xs:int**</span></span> |<span data-ttu-id="99f0e-305">Bitů na vzorek pro formát wFormatTag typu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-305">Bits per sample for the wFormatTag format type.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="99f0e-306">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="99f0e-306">Child elements</span></span>
| <span data-ttu-id="99f0e-307">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-307">Name</span></span> | <span data-ttu-id="99f0e-308">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-308">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99f0e-309">**LoudnessMeteringResultParameters**</span><span class="sxs-lookup"><span data-stu-id="99f0e-309">**LoudnessMeteringResultParameters**</span></span><br/><br/> <span data-ttu-id="99f0e-310">Hodnota minOccurs = maxOccurs "0" = "1"</span><span class="sxs-lookup"><span data-stu-id="99f0e-310">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="99f0e-311">Parametry výsledek měření hlasitosti.</span><span class="sxs-lookup"><span data-stu-id="99f0e-311">Loudness metering result parameters.</span></span> <span data-ttu-id="99f0e-312">Další informace najdete v tématu [LoudnessMeteringResultParameters element](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="99f0e-312">For more information, see [LoudnessMeteringResultParameters element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="99f0e-313"><a name="LoudnessMeteringResultParameters "></a>LoudnessMeteringResultParameters element</span><span class="sxs-lookup"><span data-stu-id="99f0e-313"><a name="LoudnessMeteringResultParameters "></a> LoudnessMeteringResultParameters element</span></span>
<span data-ttu-id="99f0e-314">Parametry výsledek měření hlasitosti.</span><span class="sxs-lookup"><span data-stu-id="99f0e-314">Loudness metering result parameters.</span></span>  

<span data-ttu-id="99f0e-315">Příklad XML můžete nalézt [ukázkový kód XML](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="99f0e-315">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="99f0e-316">Atributy</span><span class="sxs-lookup"><span data-stu-id="99f0e-316">Attributes</span></span>
| <span data-ttu-id="99f0e-317">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="99f0e-317">Name</span></span> | <span data-ttu-id="99f0e-318">Typ</span><span class="sxs-lookup"><span data-stu-id="99f0e-318">Type</span></span> | <span data-ttu-id="99f0e-319">Popis</span><span class="sxs-lookup"><span data-stu-id="99f0e-319">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="99f0e-320">**DPLMVersionInformation**</span><span class="sxs-lookup"><span data-stu-id="99f0e-320">**DPLMVersionInformation**</span></span> |<span data-ttu-id="99f0e-321">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-321">**xs:string**</span></span> |<span data-ttu-id="99f0e-322">**Dolby** professional hlasitosti měření verzi development kit.</span><span class="sxs-lookup"><span data-stu-id="99f0e-322">**Dolby** professional loudness metering development kit version.</span></span> |
| <span data-ttu-id="99f0e-323">**DialogNormalization**</span><span class="sxs-lookup"><span data-stu-id="99f0e-323">**DialogNormalization**</span></span><br/><br/> <span data-ttu-id="99f0e-324">minInclusive = "-31" maxInclusive = "-1"</span><span class="sxs-lookup"><span data-stu-id="99f0e-324">minInclusive="-31" maxInclusive="-1"</span></span><br/><br/> <span data-ttu-id="99f0e-325">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-325">Required</span></span> |<span data-ttu-id="99f0e-326">**xs:int**</span><span class="sxs-lookup"><span data-stu-id="99f0e-326">**xs:int**</span></span> |<span data-ttu-id="99f0e-327">DialogNormalization vygenerované pomocí DPLM, požadováno pro nastavení LoudnessMetering</span><span class="sxs-lookup"><span data-stu-id="99f0e-327">DialogNormalization generated through DPLM, required when LoudnessMetering is set</span></span> |
| <span data-ttu-id="99f0e-328">**IntegratedLoudness**</span><span class="sxs-lookup"><span data-stu-id="99f0e-328">**IntegratedLoudness**</span></span><br/><br/> <span data-ttu-id="99f0e-329">minInclusive = "-70" maxInclusive = "10"</span><span class="sxs-lookup"><span data-stu-id="99f0e-329">minInclusive="-70" maxInclusive="10"</span></span><br/><br/> <span data-ttu-id="99f0e-330">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-330">Required</span></span> |<span data-ttu-id="99f0e-331">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="99f0e-331">**xs:float**</span></span> |<span data-ttu-id="99f0e-332">Integrované hlasitosti</span><span class="sxs-lookup"><span data-stu-id="99f0e-332">Integrated loudness</span></span> |
| <span data-ttu-id="99f0e-333">**IntegratedLoudnessUnit**</span><span class="sxs-lookup"><span data-stu-id="99f0e-333">**IntegratedLoudnessUnit**</span></span><br/><br/> <span data-ttu-id="99f0e-334">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-334">Required</span></span> |<span data-ttu-id="99f0e-335">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-335">**xs:string**</span></span> |<span data-ttu-id="99f0e-336">Integrované hlasitosti jednotky.</span><span class="sxs-lookup"><span data-stu-id="99f0e-336">Integrated loudness unit.</span></span> |
| <span data-ttu-id="99f0e-337">**IntegratedLoudnessGatingMethod**</span><span class="sxs-lookup"><span data-stu-id="99f0e-337">**IntegratedLoudnessGatingMethod**</span></span><br/><br/> <span data-ttu-id="99f0e-338">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-338">Required</span></span> |<span data-ttu-id="99f0e-339">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="99f0e-339">**xs:string**</span></span> |<span data-ttu-id="99f0e-340">Identifikátor prostřednictvím brány</span><span class="sxs-lookup"><span data-stu-id="99f0e-340">Gating identifier</span></span> |
| <span data-ttu-id="99f0e-341">**IntegratedLoudnessSpeechPercentage**</span><span class="sxs-lookup"><span data-stu-id="99f0e-341">**IntegratedLoudnessSpeechPercentage**</span></span><br/><br/> <span data-ttu-id="99f0e-342">minInclusive = "0" maxInclusive = "100"</span><span class="sxs-lookup"><span data-stu-id="99f0e-342">minInclusive ="0" maxInclusive="100"</span></span> |<span data-ttu-id="99f0e-343">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="99f0e-343">**xs:float**</span></span> |<span data-ttu-id="99f0e-344">Rozpoznávání řeči obsah prostřednictvím programu, jako procentuální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="99f0e-344">Speech content over the program, as a percentage.</span></span> |
| <span data-ttu-id="99f0e-345">**SamplePeak**</span><span class="sxs-lookup"><span data-stu-id="99f0e-345">**SamplePeak**</span></span><br/><br/> <span data-ttu-id="99f0e-346">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-346">Required</span></span> |<span data-ttu-id="99f0e-347">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="99f0e-347">**xs:float**</span></span> |<span data-ttu-id="99f0e-348">Ukázka absolutní hodnota ve špičce, od resetování nebo od posledního vymazat, na kanál.</span><span class="sxs-lookup"><span data-stu-id="99f0e-348">Peak absolute sample value, since reset or since it was last cleared, per channel.</span></span>  <span data-ttu-id="99f0e-349">Jednotky jsou dBFS.</span><span class="sxs-lookup"><span data-stu-id="99f0e-349">Units are dBFS.</span></span> |
| <span data-ttu-id="99f0e-350">**SamplePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="99f0e-350">**SamplePeakUnit**</span></span><br/><br/> <span data-ttu-id="99f0e-351">pevné = "dBFS"</span><span class="sxs-lookup"><span data-stu-id="99f0e-351">fixed="dBFS"</span></span><br/><br/> <span data-ttu-id="99f0e-352">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-352">Required</span></span> |<span data-ttu-id="99f0e-353">**xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="99f0e-353">**xs:anySimpleType**</span></span> |<span data-ttu-id="99f0e-354">Ukázka jednotky ve špičce.</span><span class="sxs-lookup"><span data-stu-id="99f0e-354">Sample peak unit.</span></span> |
| <span data-ttu-id="99f0e-355">**TruePeak**</span><span class="sxs-lookup"><span data-stu-id="99f0e-355">**TruePeak**</span></span><br/><br/> <span data-ttu-id="99f0e-356">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-356">Required</span></span> |<span data-ttu-id="99f0e-357">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="99f0e-357">**xs:float**</span></span> |<span data-ttu-id="99f0e-358">Maximální hodnota true ve špičce hodnotu, podle BS.1770 ITU-R-2, od resetování nebo od posledního vymazat na kanál.</span><span class="sxs-lookup"><span data-stu-id="99f0e-358">Maximum true peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.</span></span> <span data-ttu-id="99f0e-359">Jednotky jsou dBTP.</span><span class="sxs-lookup"><span data-stu-id="99f0e-359">Units are dBTP.</span></span> |
| <span data-ttu-id="99f0e-360">**TruePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="99f0e-360">**TruePeakUnit**</span></span><br/><br/> <span data-ttu-id="99f0e-361">pevné = "dBTP"</span><span class="sxs-lookup"><span data-stu-id="99f0e-361">fixed="dBTP"</span></span><br/><br/> <span data-ttu-id="99f0e-362">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="99f0e-362">Required</span></span> |<span data-ttu-id="99f0e-363">**xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="99f0e-363">**xs:anySimpleType**</span></span> |<span data-ttu-id="99f0e-364">Jednotka true ve špičce.</span><span class="sxs-lookup"><span data-stu-id="99f0e-364">True peak unit.</span></span> |

## <a name="schema-code"></a><span data-ttu-id="99f0e-365">Schéma kódu</span><span class="sxs-lookup"><span data-stu-id="99f0e-365">Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
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
                  <xs:element name="Sources">  
                    <xs:annotation>  
                      <xs:documentation>Collection of input/source media files, that was processed in order to produce this AssetFile</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Source" minOccurs="1" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>An input/source file used when generating this asset</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Name" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>input source file name</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
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
                            <xs:attribute name="FourCC" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video codec FourCC code</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Profile" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 profile (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Level" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 level (only appliable for H264 codec)</xs:documentation>  
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
                            <xs:attribute name="Framerate" use="required">  
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
                            <xs:attribute name="TargetFramerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>preset target video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via the encoding preset, in kilobits per second</xs:documentation>  
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
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:sequence>  
                              <xs:element name="LoudnessMeteringResultParameters" minOccurs="0" maxOccurs="1">  
                                <xs:annotation>  
                                  <xs:documentation>Loudness Metering Result Parameters</xs:documentation>  
                                </xs:annotation>  
                                <xs:complexType>  
                                  <xs:attribute name="DPLMVersionInformation" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Dolby Professional Loudness Metering Development Kit Version</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="DialogNormalization" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation> DialogNormalization generated through DPLM, required when LoudnessMetering is set</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:int">  
                                        <xs:minInclusive value="-31"/>  
                                        <xs:maxInclusive value="-1"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudness" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation>Integrated loudness</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="-70"/>  
                                        <xs:maxInclusive value="10"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessUnit" use="required" type="xs:string">  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessGatingMethod" use="required" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Gating identifier</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessSpeechPercentage">  
                                    <xs:annotation>  
                                      <xs:documentation>Speech content over the program, as a percentage.</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="0"/>  
                                        <xs:maxInclusive value="100"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Peak absolute sample value, since reset or since it was last cleared, per channel.  Units are dBFS.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeakUnit" use="required" fixed="dBFS">  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Maximum True Peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.  Units are dBTP.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeakUnit" use="required" fixed="dBTP">  
                                  </xs:attribute>  
                                </xs:complexType>  
                              </xs:element>  
                            </xs:sequence>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Codec" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>audio track codec string</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="EncoderVersion" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>optional encoder version string, required for EAC3</xs:documentation>  
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
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
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
                <xs:attribute name="Duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:duration"/>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  



## <span data-ttu-id="99f0e-366"><a name="xml"></a>Ukázkový kód XML</span><span class="sxs-lookup"><span data-stu-id="99f0e-366"><a name="xml"></a> XML example</span></span>
 <span data-ttu-id="99f0e-367">Následuje příklad výstupního souboru metadat.</span><span class="sxs-lookup"><span data-stu-id="99f0e-367">The following is an example of the Output metadata file.</span></span>  

    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
                xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata">  
      <AssetFile Name="BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4" Size="4646283" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.2" Width="1280" Height="720" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="4250" TargetBitrate="3400" MaxGOPBitrate="5514"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4" Size="3166728" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="2846" TargetBitrate="2250" MaxGOPBitrate="3630"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4" Size="2205095" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1932" TargetBitrate="1500" MaxGOPBitrate="2513"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4" Size="1508567" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1271" TargetBitrate="1000" MaxGOPBitrate="1527"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4" Size="1057155" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="843" TargetBitrate="650" MaxGOPBitrate="1086"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4" Size="699262" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="1.3" Width="320" Height="180" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="503" TargetBitrate="400" MaxGOPBitrate="661"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_96kbps.mp4" Size="166780" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_56kbps.mp4" Size="124576" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="53" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a><span data-ttu-id="99f0e-368">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99f0e-368">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="99f0e-369">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="99f0e-369">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
