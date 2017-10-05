---
title: "Analýza médiu pomocí portálu Azure | Microsoft Docs"
description: "Toto téma popisuje postupy zpracování médií s procesory médií Media Analytics (sad Management Pack) pomocí portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 22032aef0cc8b7b015503043028873e70c21ee85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="analyze-your-media-using-the-azure-portal"></a><span data-ttu-id="ca8f7-103">Analýza médií s využitím webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ca8f7-103">Analyze your media using the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="ca8f7-104">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-104">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="ca8f7-105">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca8f7-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

## <a name="overview"></a><span data-ttu-id="ca8f7-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="ca8f7-106">Overview</span></span>
<span data-ttu-id="ca8f7-107">Azure Media Services Analytics je kolekce řečových a vizuálních komponent (na škálování enterprise, dodržování předpisů, zabezpečení a globální reach), které usnadňují organizacím a podnikům umožňují k získání prakticky uplatnitelných informací ze svých videosouborů.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-107">Azure Media Services Analytics is a collection of speech and vision components (at enterprise scale, compliance, security and global reach) that make it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="ca8f7-108">Podrobnější přehled Azure Media Services Analytics najdete v článku [to](media-services-analytics-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-108">For more detailed overview of Azure Media Services Analytics see [this](media-services-analytics-overview.md) topic.</span></span> 

<span data-ttu-id="ca8f7-109">Toto téma popisuje postupy zpracování médií s procesory médií Media Analytics (sad Management Pack) pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-109">This topic discusses how to process your media with Media Analytics media processors (MPs) using the Azure portal.</span></span> <span data-ttu-id="ca8f7-110">MP Media Analytics vytvářejí soubory MP4 nebo soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-110">Media Analytics MPs produce MP4 files or JSON files.</span></span> <span data-ttu-id="ca8f7-111">Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-111">If a media processor produced an MP4 file, you can progressively download the file.</span></span> <span data-ttu-id="ca8f7-112">Pokud procesor médií vytvořil soubor JSON, můžete ho stáhnout z úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-112">If a media processor produced a JSON file, you can download the file from the Azure blob storage.</span></span> 

## <a name="choose-an-asset-that-you-want-to-analyze"></a><span data-ttu-id="ca8f7-113">Vyberte asset, který chcete analyzovat</span><span class="sxs-lookup"><span data-stu-id="ca8f7-113">Choose an asset that you want to analyze</span></span>
1. <span data-ttu-id="ca8f7-114">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-114">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="ca8f7-115">V okně **Nastavení** vyberte **Assety**.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-115">In the **Settings** window, select **Assets**.</span></span>  
   <span data-ttu-id="ca8f7-116">.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-116">.</span></span>
    <span data-ttu-id="ca8f7-117">![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span><span class="sxs-lookup"><span data-stu-id="ca8f7-117">![Analyze videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span></span>
3. <span data-ttu-id="ca8f7-118">Vyberte asset, který chcete analyzovat a stiskněte klávesu **analyzovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-118">Select the asset that you would like to analyze and press the **Analyze** button.</span></span>
   
    ![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. <span data-ttu-id="ca8f7-120">V **proces média asset Media Analytics** okně vyberte nastavení procesoru.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-120">In the **Process media asset with  Media Analytics** window, select the processor.</span></span> 
   
    <span data-ttu-id="ca8f7-121">Zbývající část článek vysvětluje důvod, proč a jak používat každý procesor.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-121">The rest of the article explains why and how to use each processor.</span></span> 
5. <span data-ttu-id="ca8f7-122">Stiskněte klávesu **vytvořit** spuštění a úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-122">Press **Create** to the start a job.</span></span>

## <a name="azure-media-indexer"></a><span data-ttu-id="ca8f7-123">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="ca8f7-123">Azure Media Indexer</span></span>
<span data-ttu-id="ca8f7-124">**Azure Media Indexer** procesor médií umožňuje vytvoření mediálních souborů a obsah s možností vyhledávání, jakož i generovat uzavřené titulků sleduje.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-124">The **Azure Media Indexer** media processor enables you to make media files and content searchable, as well as generate closed captioning tracks.</span></span> <span data-ttu-id="ca8f7-125">Tato část obsahuje některé údaje o možnostech, které můžete zadat pro tento bod.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-125">This sections gives some details about options that you can specify for this MP.</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a><span data-ttu-id="ca8f7-127">Jazyk</span><span class="sxs-lookup"><span data-stu-id="ca8f7-127">Language</span></span>
<span data-ttu-id="ca8f7-128">Přirozeného jazyka rozpoznat v multimediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-128">The natural language to be recognized in the multimedia file.</span></span> <span data-ttu-id="ca8f7-129">Například angličtina nebo španělštině.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-129">For example, English or Spanish.</span></span> 

### <a name="captions"></a><span data-ttu-id="ca8f7-130">Titulky</span><span class="sxs-lookup"><span data-stu-id="ca8f7-130">Captions</span></span>
<span data-ttu-id="ca8f7-131">Můžete formátu popisek, který bude vydána z vašeho obsahu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-131">You can choose a caption format that will be generated from your content.</span></span> <span data-ttu-id="ca8f7-132">Úlohu indexování může generovat soubory titulků v následujících formátech:</span><span class="sxs-lookup"><span data-stu-id="ca8f7-132">An indexing job can generate closed caption files in the following formats:</span></span>  

* <span data-ttu-id="ca8f7-133">**SAMI**</span><span class="sxs-lookup"><span data-stu-id="ca8f7-133">**SAMI**</span></span>
* <span data-ttu-id="ca8f7-134">**TTML**</span><span class="sxs-lookup"><span data-stu-id="ca8f7-134">**TTML**</span></span>
* <span data-ttu-id="ca8f7-135">**WebVTT**</span><span class="sxs-lookup"><span data-stu-id="ca8f7-135">**WebVTT**</span></span>

<span data-ttu-id="ca8f7-136">Popisek (kopie) soubory v těchto formátů lze Zpřístupněte soubory audia a videa pro osoby s postižením sluchu uzavřít.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-136">Closed Caption (CC) files in these formats can be used to make audio and video files accessible to people with hearing disability.</span></span>

### <a name="aib-file"></a><span data-ttu-id="ca8f7-137">Soubor AIB</span><span class="sxs-lookup"><span data-stu-id="ca8f7-137">AIB file</span></span>
<span data-ttu-id="ca8f7-138">Tuto možnost vyberte, pokud chcete generovat soubor zvuk Index Blob pro použití s k objektu IFilter vlastní SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-138">Select this option if you would like to generate the Audio Index Blob file for use with the custom SQL Server IFilter.</span></span> <span data-ttu-id="ca8f7-139">Další informace najdete v tématu [to](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-139">For more information, see [this](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span></span>

### <a name="keywords"></a><span data-ttu-id="ca8f7-140">Klíčová slova</span><span class="sxs-lookup"><span data-stu-id="ca8f7-140">Keywords</span></span>
<span data-ttu-id="ca8f7-141">Tuto možnost vyberte, pokud chcete generovat soubor XML, klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-141">Select this option if you would like to generate a keywords XML file.</span></span> <span data-ttu-id="ca8f7-142">Tento soubor obsahuje klíčová slova extrahuje z obsahu řeči četnost a informace o posunu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-142">This file contains keywords extracted from the speech content, with frequency and offset information.</span></span>

### <a name="job-name"></a><span data-ttu-id="ca8f7-143">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="ca8f7-143">Job name</span></span>
<span data-ttu-id="ca8f7-144">Popisný název, který umožňuje identifikovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-144">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="ca8f7-145">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-145">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="ca8f7-146">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="ca8f7-146">Output file</span></span>
<span data-ttu-id="ca8f7-147">Popisný název, který umožňuje identifikovat obsah výstup.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-147">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-hyperlapse"></a><span data-ttu-id="ca8f7-148">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="ca8f7-148">Azure Media Hyperlapse</span></span>
<span data-ttu-id="ca8f7-149">Azure Media Hyperlapse je na sadu Management Pack vytvoří smooth vypršelo čas videa z první, kdo nebo akce fotoaparát obsah.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-149">Azure Media Hyperlapse is an MP that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="ca8f7-150">Další informace najdete v [tomto](media-services-hyperlapse-content.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-150">For more information, see [this](media-services-hyperlapse-content.md) topic.</span></span> <span data-ttu-id="ca8f7-151">Tato část obsahuje některé údaje o možnostech, které můžete zadat pro tento bod.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-151">This sections gives some details about options that you can specify for this MP.</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a><span data-ttu-id="ca8f7-153">Rychlost</span><span class="sxs-lookup"><span data-stu-id="ca8f7-153">Speed</span></span>
<span data-ttu-id="ca8f7-154">Určete rychlost ke které má být urychlení vstupní video.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-154">Specify the speed with which to speed up the input video.</span></span> <span data-ttu-id="ca8f7-155">Výstup je stabilizované a čas vypršelo interpretace vstupu videa.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-155">The output is a stabilized and time-lapsed rendition of the input video.</span></span>

### <a name="job-name"></a><span data-ttu-id="ca8f7-156">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="ca8f7-156">Job name</span></span>
<span data-ttu-id="ca8f7-157">Popisný název, který umožňuje identifikovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-157">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="ca8f7-158">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-158">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="ca8f7-159">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="ca8f7-159">Output file</span></span>
<span data-ttu-id="ca8f7-160">Popisný název, který umožňuje identifikovat obsah výstup.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-160">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-face-detector"></a><span data-ttu-id="ca8f7-161">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="ca8f7-161">Azure Media Face Detector</span></span>
<span data-ttu-id="ca8f7-162">**Azure Media vzhled detektor** procesor médií (PP) umožňuje count, sledovat pohybů a i vyhodnocení zapojení cílové skupiny a reakce prostřednictvím obličeje výrazy.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-162">The **Azure Media Face Detector** media processor (MP) enables you to count, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="ca8f7-163">Tato služba obsahuje dvě funkce:</span><span class="sxs-lookup"><span data-stu-id="ca8f7-163">This service contains two features:</span></span> 

* <span data-ttu-id="ca8f7-164">**Vzhled detekce**</span><span class="sxs-lookup"><span data-stu-id="ca8f7-164">**Face detection**</span></span>
  
    <span data-ttu-id="ca8f7-165">Vzhled zjišťování vyhledá a sleduje lidského tyto řezy v rámci video.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-165">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="ca8f7-166">Více řezy lze zjistit a následně je sledovat jako pohyb se s metadaty čas a umístění, vrátí se v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-166">Multiple faces can be detected and subsequently be tracked as they move around, with the time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="ca8f7-167">Při sledování, pokusí se dávat konzistentní ID stejné písmo při osoba, která je manipulaci se na obrazovce, přestože bráněno nebo stručně nechte rámečku.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-167">During tracking, it will attempt to give a consistent ID to the same face while the person is moving around on screen, even if they are obstructed or briefly leave the frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="ca8f7-168">Této služby nebude provádět rozpoznávání obličeje.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-168">This services does not perform facial recognition.</span></span> <span data-ttu-id="ca8f7-169">Osoba, která zůstane rámečku nebo se stane nelze blokovat. pro příliš dlouho přidělí nové ID když se vrátí.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-169">An individual who leaves the frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="ca8f7-170">**Emocí**</span><span class="sxs-lookup"><span data-stu-id="ca8f7-170">**Emotion detection**</span></span>
  
    <span data-ttu-id="ca8f7-171">Emocí je volitelná součást procesor média detekce řez, který vrátí analýzy na více duševní atributů z řezy detekuje, včetně štěstí, sadness, obavy, anger a další.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-171">Emotion Detection is an optional component of the Face Detection Media Processor that returns analysis on multiple emotional attributes from the faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a><span data-ttu-id="ca8f7-173">Detekce režimu</span><span class="sxs-lookup"><span data-stu-id="ca8f7-173">Detection mode</span></span>
<span data-ttu-id="ca8f7-174">Jeden z následujících režimů mohou využívat procesor:</span><span class="sxs-lookup"><span data-stu-id="ca8f7-174">One of the following modes can be used by the processor:</span></span>

* <span data-ttu-id="ca8f7-175">Vzhled detekce</span><span class="sxs-lookup"><span data-stu-id="ca8f7-175">face detection</span></span>
* <span data-ttu-id="ca8f7-176">každý řez emocí</span><span class="sxs-lookup"><span data-stu-id="ca8f7-176">per face emotion detection</span></span>
* <span data-ttu-id="ca8f7-177">agregační emocí</span><span class="sxs-lookup"><span data-stu-id="ca8f7-177">aggregate emotion detection</span></span>

### <a name="job-name"></a><span data-ttu-id="ca8f7-178">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="ca8f7-178">Job name</span></span>
<span data-ttu-id="ca8f7-179">Popisný název, který umožňuje identifikovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-179">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="ca8f7-180">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-180">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="ca8f7-181">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="ca8f7-181">Output file</span></span>
<span data-ttu-id="ca8f7-182">Popisný název, který umožňuje identifikovat obsah výstup.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-182">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-motion-detector"></a><span data-ttu-id="ca8f7-183">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="ca8f7-183">Azure Media Motion Detector</span></span>
<span data-ttu-id="ca8f7-184">**Detektor pohybu médií Azure** procesor médií (PP) umožňuje efektivně identifikaci části týkající se v rámci souboru jinak dlouhé a bezproblémové video.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-184">The **Azure Media Motion Detector** media processor (MP) enables you to efficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="ca8f7-185">Detekce pohybu dají použít na statické kamer k identifikaci části videa, kde dochází k pohybu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-185">Motion detection can be used on static camera footage to identify sections of the video where motion occurs.</span></span> <span data-ttu-id="ca8f7-186">Vygeneruje soubor JSON obsahující metadata s časová razítka a ohraničující oblasti, kde došlo k události.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-186">It generates a JSON file containing a metadata with timestamps and the bounding region where the event occurred.</span></span>

<span data-ttu-id="ca8f7-187">Cílem směrem zabezpečení video informační kanály, tato technologie je možné zařadit do kategorií pohybu do příslušné události a falešně pozitivních například stínů a osvětlení změny.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-187">Targeted towards security video feeds, this technology is able to categorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="ca8f7-188">To umožňuje generovat výstrahy zabezpečení z fotoaparátu kanály bez nevyžádané pošty s nekonečná důležité události, při schopnost extrahovat z extrémně dlouhé sledováním videa situacích, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-188">This allows you to generate security alerts from camera feeds without being spammed with endless irrelevant events, while being able to extract moments of interest from extremely long surveillance videos.</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a><span data-ttu-id="ca8f7-190">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="ca8f7-190">Azure Media Video Thumbnails</span></span>
<span data-ttu-id="ca8f7-191">Tato procesoru vám pomůžou vytvořit souhrnných informací o dlouho videa automaticky výběrem zajímavé fragmenty kódu z zdroj videa.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-191">This processor can help you create summaries of long videos by automatically selecting interesting snippets from the source video.</span></span> <span data-ttu-id="ca8f7-192">To je užitečné, pokud byste chtěli poskytnout rychlý přehled toho, co očekávat při dlouhé video.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-192">This is useful when you want to provide a quick overview of what to expect in a long video.</span></span> <span data-ttu-id="ca8f7-193">Podrobné informace a příklady naleznete v tématu [miniatur videa v používání Azure Media k vytvoření Videosouhrnu](media-services-video-summarization.md)</span><span class="sxs-lookup"><span data-stu-id="ca8f7-193">For detailed information and examples, see [Use Azure Media Video Thumbnails to Create a Video Summarization](media-services-video-summarization.md)</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a><span data-ttu-id="ca8f7-195">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="ca8f7-195">Job name</span></span>
<span data-ttu-id="ca8f7-196">Popisný název, který umožňuje identifikovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-196">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="ca8f7-197">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-197">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="ca8f7-198">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="ca8f7-198">Output file</span></span>
<span data-ttu-id="ca8f7-199">Popisný název, který umožňuje identifikovat obsah výstup.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-199">A friendly name that lets you identify the output content.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ca8f7-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ca8f7-200">Next steps</span></span>
<span data-ttu-id="ca8f7-201">Zobrazení Media Services kurzů.</span><span class="sxs-lookup"><span data-stu-id="ca8f7-201">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ca8f7-202">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ca8f7-202">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

