---
title: "aaaAnalyze média pomocí hello portálu Azure | Microsoft Docs"
description: "Toto téma popisuje, jak tooprocess hello médií s procesory médií Media Analytics (sad Management Pack) pomocí portálu Azure."
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
ms.openlocfilehash: d549c0315cd58c3771420379316b4f2ad3c2cea6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-media-using-hello-azure-portal"></a><span data-ttu-id="dc019-103">Analýza médiu pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dc019-103">Analyze your media using hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="dc019-104">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="dc019-104">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="dc019-105">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dc019-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

## <a name="overview"></a><span data-ttu-id="dc019-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="dc019-106">Overview</span></span>
<span data-ttu-id="dc019-107">Azure Media Services Analytics je kolekce řečových a vizuálních komponent (na škálování enterprise, dodržování předpisů, zabezpečení a globální reach), které usnadňují organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů.</span><span class="sxs-lookup"><span data-stu-id="dc019-107">Azure Media Services Analytics is a collection of speech and vision components (at enterprise scale, compliance, security and global reach) that make it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="dc019-108">Podrobnější přehled Azure Media Services Analytics najdete v článku [to](media-services-analytics-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="dc019-108">For more detailed overview of Azure Media Services Analytics see [this](media-services-analytics-overview.md) topic.</span></span> 

<span data-ttu-id="dc019-109">Toto téma popisuje, jak tooprocess hello médií s procesory médií Media Analytics (sad Management Pack) pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dc019-109">This topic discusses how tooprocess your media with Media Analytics media processors (MPs) using hello Azure portal.</span></span> <span data-ttu-id="dc019-110">MP Media Analytics vytvářejí soubory MP4 nebo soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="dc019-110">Media Analytics MPs produce MP4 files or JSON files.</span></span> <span data-ttu-id="dc019-111">Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat hello souboru.</span><span class="sxs-lookup"><span data-stu-id="dc019-111">If a media processor produced an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="dc019-112">Pokud procesor médií vytvořil soubor JSON, můžete stáhnout soubor hello z hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="dc019-112">If a media processor produced a JSON file, you can download hello file from hello Azure blob storage.</span></span> 

## <a name="choose-an-asset-that-you-want-tooanalyze"></a><span data-ttu-id="dc019-113">Vyberte prostředek, které chcete tooanalyze</span><span class="sxs-lookup"><span data-stu-id="dc019-113">Choose an asset that you want tooanalyze</span></span>
1. <span data-ttu-id="dc019-114">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="dc019-114">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="dc019-115">V hello **nastavení** vyberte **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="dc019-115">In hello **Settings** window, select **Assets**.</span></span>  
   <span data-ttu-id="dc019-116">.</span><span class="sxs-lookup"><span data-stu-id="dc019-116">.</span></span>
    <span data-ttu-id="dc019-117">![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span><span class="sxs-lookup"><span data-stu-id="dc019-117">![Analyze videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span></span>
3. <span data-ttu-id="dc019-118">Vyberte hello asset, který chcete tooanalyze a stiskněte klávesu hello **analyzovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dc019-118">Select hello asset that you would like tooanalyze and press hello **Analyze** button.</span></span>
   
    ![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. <span data-ttu-id="dc019-120">V hello **proces média asset Media Analytics** okno, vyberte hello procesoru.</span><span class="sxs-lookup"><span data-stu-id="dc019-120">In hello **Process media asset with  Media Analytics** window, select hello processor.</span></span> 
   
    <span data-ttu-id="dc019-121">Hello zbytek hello článek vysvětluje, proč a jak toouse každý procesor.</span><span class="sxs-lookup"><span data-stu-id="dc019-121">hello rest of hello article explains why and how toouse each processor.</span></span> 
5. <span data-ttu-id="dc019-122">Stiskněte klávesu **vytvořit** toohello spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="dc019-122">Press **Create** toohello start a job.</span></span>

## <a name="azure-media-indexer"></a><span data-ttu-id="dc019-123">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="dc019-123">Azure Media Indexer</span></span>
<span data-ttu-id="dc019-124">Hello **Azure Media Indexer** procesor médií vám umožní toomake mediálních souborů a obsah s možností vyhledávání, a také generování uzavřené titulků sleduje.</span><span class="sxs-lookup"><span data-stu-id="dc019-124">hello **Azure Media Indexer** media processor enables you toomake media files and content searchable, as well as generate closed captioning tracks.</span></span> <span data-ttu-id="dc019-125">Tato část obsahuje některé údaje o možnostech, které můžete zadat pro tento bod.</span><span class="sxs-lookup"><span data-stu-id="dc019-125">This sections gives some details about options that you can specify for this MP.</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a><span data-ttu-id="dc019-127">Jazyk</span><span class="sxs-lookup"><span data-stu-id="dc019-127">Language</span></span>
<span data-ttu-id="dc019-128">toobe přirozeného jazyka Hello rozpoznán v hello multimediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="dc019-128">hello natural language toobe recognized in hello multimedia file.</span></span> <span data-ttu-id="dc019-129">Například angličtina nebo španělštině.</span><span class="sxs-lookup"><span data-stu-id="dc019-129">For example, English or Spanish.</span></span> 

### <a name="captions"></a><span data-ttu-id="dc019-130">Titulky</span><span class="sxs-lookup"><span data-stu-id="dc019-130">Captions</span></span>
<span data-ttu-id="dc019-131">Můžete formátu popisek, který bude vydána z vašeho obsahu.</span><span class="sxs-lookup"><span data-stu-id="dc019-131">You can choose a caption format that will be generated from your content.</span></span> <span data-ttu-id="dc019-132">Indexování úlohy mohou vytvářet soubory titulků v hello následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="dc019-132">An indexing job can generate closed caption files in hello following formats:</span></span>  

* <span data-ttu-id="dc019-133">**SAMI**</span><span class="sxs-lookup"><span data-stu-id="dc019-133">**SAMI**</span></span>
* <span data-ttu-id="dc019-134">**TTML**</span><span class="sxs-lookup"><span data-stu-id="dc019-134">**TTML**</span></span>
* <span data-ttu-id="dc019-135">**WebVTT**</span><span class="sxs-lookup"><span data-stu-id="dc019-135">**WebVTT**</span></span>

<span data-ttu-id="dc019-136">Uzavřené popisek (kopie) soubory v těchto formátů lze použít toomake zvuk a video soubory přístupné toopeople s postižení sluchu.</span><span class="sxs-lookup"><span data-stu-id="dc019-136">Closed Caption (CC) files in these formats can be used toomake audio and video files accessible toopeople with hearing disability.</span></span>

### <a name="aib-file"></a><span data-ttu-id="dc019-137">Soubor AIB</span><span class="sxs-lookup"><span data-stu-id="dc019-137">AIB file</span></span>
<span data-ttu-id="dc019-138">Tuto možnost vyberte, pokud jste by jako soubor Blob Index zvuk hello toogenerate pro použití s hello vlastní IFilter serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="dc019-138">Select this option if you would like toogenerate hello Audio Index Blob file for use with hello custom SQL Server IFilter.</span></span> <span data-ttu-id="dc019-139">Další informace najdete v tématu [to](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.</span><span class="sxs-lookup"><span data-stu-id="dc019-139">For more information, see [this](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span></span>

### <a name="keywords"></a><span data-ttu-id="dc019-140">Klíčová slova</span><span class="sxs-lookup"><span data-stu-id="dc019-140">Keywords</span></span>
<span data-ttu-id="dc019-141">Tuto možnost vyberte, pokud chcete toogenerate soubor XML klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="dc019-141">Select this option if you would like toogenerate a keywords XML file.</span></span> <span data-ttu-id="dc019-142">Tento soubor obsahuje klíčová slova extrahuje z obsahu hello rozpoznávání řeči, četnost a informace o posunu.</span><span class="sxs-lookup"><span data-stu-id="dc019-142">This file contains keywords extracted from hello speech content, with frequency and offset information.</span></span>

### <a name="job-name"></a><span data-ttu-id="dc019-143">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="dc019-143">Job name</span></span>
<span data-ttu-id="dc019-144">Popisný název, který umožňuje identifikovat hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-144">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="dc019-145">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-145">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="dc019-146">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="dc019-146">Output file</span></span>
<span data-ttu-id="dc019-147">Popisný název, který umožňuje identifikovat obsah výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dc019-147">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-hyperlapse"></a><span data-ttu-id="dc019-148">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="dc019-148">Azure Media Hyperlapse</span></span>
<span data-ttu-id="dc019-149">Azure Media Hyperlapse je na sadu Management Pack vytvoří smooth vypršelo čas videa z první, kdo nebo akce fotoaparát obsah.</span><span class="sxs-lookup"><span data-stu-id="dc019-149">Azure Media Hyperlapse is an MP that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="dc019-150">Další informace najdete v [tomto](media-services-hyperlapse-content.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="dc019-150">For more information, see [this](media-services-hyperlapse-content.md) topic.</span></span> <span data-ttu-id="dc019-151">Tato část obsahuje některé údaje o možnostech, které můžete zadat pro tento bod.</span><span class="sxs-lookup"><span data-stu-id="dc019-151">This sections gives some details about options that you can specify for this MP.</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a><span data-ttu-id="dc019-153">Rychlost</span><span class="sxs-lookup"><span data-stu-id="dc019-153">Speed</span></span>
<span data-ttu-id="dc019-154">Určete rychlost hello s které toospeed až vstupní video hello.</span><span class="sxs-lookup"><span data-stu-id="dc019-154">Specify hello speed with which toospeed up hello input video.</span></span> <span data-ttu-id="dc019-155">výstup Hello je stabilizované a čas vypršelo interpretace hello vstupní videa.</span><span class="sxs-lookup"><span data-stu-id="dc019-155">hello output is a stabilized and time-lapsed rendition of hello input video.</span></span>

### <a name="job-name"></a><span data-ttu-id="dc019-156">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="dc019-156">Job name</span></span>
<span data-ttu-id="dc019-157">Popisný název, který umožňuje identifikovat hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-157">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="dc019-158">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-158">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="dc019-159">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="dc019-159">Output file</span></span>
<span data-ttu-id="dc019-160">Popisný název, který umožňuje identifikovat obsah výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dc019-160">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-face-detector"></a><span data-ttu-id="dc019-161">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="dc019-161">Azure Media Face Detector</span></span>
<span data-ttu-id="dc019-162">Hello **Azure Media vzhled detektor** procesor médií (PP) vám umožní toocount, sledovat pohybů a i zapojení cílové skupiny měřidla a reakce prostřednictvím obličeje výrazy.</span><span class="sxs-lookup"><span data-stu-id="dc019-162">hello **Azure Media Face Detector** media processor (MP) enables you toocount, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="dc019-163">Tato služba obsahuje dvě funkce:</span><span class="sxs-lookup"><span data-stu-id="dc019-163">This service contains two features:</span></span> 

* <span data-ttu-id="dc019-164">**Vzhled detekce**</span><span class="sxs-lookup"><span data-stu-id="dc019-164">**Face detection**</span></span>
  
    <span data-ttu-id="dc019-165">Vzhled zjišťování vyhledá a sleduje lidského tyto řezy v rámci video.</span><span class="sxs-lookup"><span data-stu-id="dc019-165">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="dc019-166">Více řezy lze zjistit a následně je sledovat jako pohyb se s hello čas a umístění metadata vrácená v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="dc019-166">Multiple faces can be detected and subsequently be tracked as they move around, with hello time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="dc019-167">Při sledování pokusí toogive konzistentní toohello ID stejné čelí, zatímco je osoba hello pohyb na obrazovce, přestože bráněno nebo stručně nechte hello rámce.</span><span class="sxs-lookup"><span data-stu-id="dc019-167">During tracking, it will attempt toogive a consistent ID toohello same face while hello person is moving around on screen, even if they are obstructed or briefly leave hello frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="dc019-168">Této služby nebude provádět rozpoznávání obličeje.</span><span class="sxs-lookup"><span data-stu-id="dc019-168">This services does not perform facial recognition.</span></span> <span data-ttu-id="dc019-169">Osoba, která zůstane hello rámce nebo se stane nelze blokovat. pro příliš dlouho přidělí nové ID když se vrátí.</span><span class="sxs-lookup"><span data-stu-id="dc019-169">An individual who leaves hello frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="dc019-170">**Emocí**</span><span class="sxs-lookup"><span data-stu-id="dc019-170">**Emotion detection**</span></span>
  
    <span data-ttu-id="dc019-171">Emocí je volitelná součást hello vzhled detekce média procesor, který vrátí analýzy na více duševní atributů z hello řezy detekuje, včetně štěstí, sadness, obavy, anger a další.</span><span class="sxs-lookup"><span data-stu-id="dc019-171">Emotion Detection is an optional component of hello Face Detection Media Processor that returns analysis on multiple emotional attributes from hello faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a><span data-ttu-id="dc019-173">Detekce režimu</span><span class="sxs-lookup"><span data-stu-id="dc019-173">Detection mode</span></span>
<span data-ttu-id="dc019-174">Jeden z následujících režimů hello dá procesorem hello:</span><span class="sxs-lookup"><span data-stu-id="dc019-174">One of hello following modes can be used by hello processor:</span></span>

* <span data-ttu-id="dc019-175">Vzhled detekce</span><span class="sxs-lookup"><span data-stu-id="dc019-175">face detection</span></span>
* <span data-ttu-id="dc019-176">každý řez emocí</span><span class="sxs-lookup"><span data-stu-id="dc019-176">per face emotion detection</span></span>
* <span data-ttu-id="dc019-177">agregační emocí</span><span class="sxs-lookup"><span data-stu-id="dc019-177">aggregate emotion detection</span></span>

### <a name="job-name"></a><span data-ttu-id="dc019-178">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="dc019-178">Job name</span></span>
<span data-ttu-id="dc019-179">Popisný název, který umožňuje identifikovat hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-179">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="dc019-180">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-180">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="dc019-181">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="dc019-181">Output file</span></span>
<span data-ttu-id="dc019-182">Popisný název, který umožňuje identifikovat obsah výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dc019-182">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-motion-detector"></a><span data-ttu-id="dc019-183">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="dc019-183">Azure Media Motion Detector</span></span>
<span data-ttu-id="dc019-184">Hello **detektor pohybu médií Azure** média procesoru (PP) umožňuje tooefficiently můžete identifikovat části týkající se v rámci souboru jinak dlouhé a bezproblémové video.</span><span class="sxs-lookup"><span data-stu-id="dc019-184">hello **Azure Media Motion Detector** media processor (MP) enables you tooefficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="dc019-185">Detekce pohybu lze použít na statické fotoaparát záznamů tooidentify části hello videa kde dojde k pohybu.</span><span class="sxs-lookup"><span data-stu-id="dc019-185">Motion detection can be used on static camera footage tooidentify sections of hello video where motion occurs.</span></span> <span data-ttu-id="dc019-186">Vygeneruje soubor JSON obsahující metadata s časová razítka a hello ohraničujícího oblasti, kde došlo k události hello.</span><span class="sxs-lookup"><span data-stu-id="dc019-186">It generates a JSON file containing a metadata with timestamps and hello bounding region where hello event occurred.</span></span>

<span data-ttu-id="dc019-187">Cílem směrem zabezpečení video informační kanály, tato technologie je možné toocategorize pohybu do příslušné události a falešně pozitivních například stínů a osvětlení změny.</span><span class="sxs-lookup"><span data-stu-id="dc019-187">Targeted towards security video feeds, this technology is able toocategorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="dc019-188">To umožňuje toogenerate výstrahy zabezpečení z fotoaparátu kanály bez nevyžádané pošty s nekonečná důležité události, aniž by byly situacích možné tooextract zájmu z extrémně dlouhé sledováním videa.</span><span class="sxs-lookup"><span data-stu-id="dc019-188">This allows you toogenerate security alerts from camera feeds without being spammed with endless irrelevant events, while being able tooextract moments of interest from extremely long surveillance videos.</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a><span data-ttu-id="dc019-190">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="dc019-190">Azure Media Video Thumbnails</span></span>
<span data-ttu-id="dc019-191">Tato procesoru vám pomůžou vytvořit souhrnných informací o dlouho videa automaticky výběrem zajímavé fragmenty kódu z hello zdroj videa.</span><span class="sxs-lookup"><span data-stu-id="dc019-191">This processor can help you create summaries of long videos by automatically selecting interesting snippets from hello source video.</span></span> <span data-ttu-id="dc019-192">To je užitečné, když chcete, aby tooprovide rychlý přehled toho, jaké tooexpect v dlouho videa.</span><span class="sxs-lookup"><span data-stu-id="dc019-192">This is useful when you want tooprovide a quick overview of what tooexpect in a long video.</span></span> <span data-ttu-id="dc019-193">Podrobné informace a příklady naleznete v tématu [použití miniatur videa v aplikaci Azure Media tooCreate Videosouhrnu](media-services-video-summarization.md)</span><span class="sxs-lookup"><span data-stu-id="dc019-193">For detailed information and examples, see [Use Azure Media Video Thumbnails tooCreate a Video Summarization](media-services-video-summarization.md)</span></span>

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a><span data-ttu-id="dc019-195">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="dc019-195">Job name</span></span>
<span data-ttu-id="dc019-196">Popisný název, který umožňuje identifikovat hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-196">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="dc019-197">[To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="dc019-197">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="dc019-198">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="dc019-198">Output file</span></span>
<span data-ttu-id="dc019-199">Popisný název, který umožňuje identifikovat obsah výstup hello.</span><span class="sxs-lookup"><span data-stu-id="dc019-199">A friendly name that lets you identify hello output content.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="dc019-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc019-200">Next steps</span></span>
<span data-ttu-id="dc019-201">Zobrazení Media Services kurzů.</span><span class="sxs-lookup"><span data-stu-id="dc019-201">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="dc019-202">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="dc019-202">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

