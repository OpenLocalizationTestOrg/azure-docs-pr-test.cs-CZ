---
title: "řezy aaaRedact v Průvodci Azure Media Analytics | Microsoft Docs"
description: "Toto téma ukazuje podrobné informace o tom, toorun úplné redigování pracovní postup pomocí Azure Media Services Explorer (AMSE) a Azure Media Redactor vizualizér (nástroj s otevřeným zdrojem)."
services: media-services
documentationcenter: 
author: Lichard
manager: cfowler
editor: 
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: ab28f4052b73fdb74fcd5766235eab35402a0c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a><span data-ttu-id="16a0f-103">Redigovat tyto řezy v Průvodci Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="16a0f-103">Redact faces with Azure Media Analytics walkthrough</span></span>

## <a name="overview"></a><span data-ttu-id="16a0f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="16a0f-104">Overview</span></span>

<span data-ttu-id="16a0f-105">**Azure Media Redactor** je [Azure Media Analytics](media-services-analytics-overview.md) procesor médií (PP), která nabízí redigování škálovatelné řez v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="16a0f-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="16a0f-106">Vzhled redigování umožňuje vám toomodify videa v pořadí tooblur řezy vybrané osob.</span><span class="sxs-lookup"><span data-stu-id="16a0f-106">Face redaction enables you toomodify your video in order tooblur faces of selected individuals.</span></span> <span data-ttu-id="16a0f-107">Můžete chtít toouse hello vzhled redigování služby ve veřejné scénáře zabezpečení a média zprávy.</span><span class="sxs-lookup"><span data-stu-id="16a0f-107">You may want toouse hello face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="16a0f-108">Pár minut záznamů, která obsahuje více řezy může trvat hodiny tooredact ručně, ale s této služby hello vzhled redigování proces bude vyžadovat několika jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="16a0f-108">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service hello face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="16a0f-109">Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="16a0f-110">Podrobnosti o **Azure Media Redactor**, najdete v části hello [vzhled redigování přehled](media-services-face-redaction.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-110">For details about  **Azure Media Redactor**, see hello [Face redaction overview](media-services-face-redaction.md) topic.</span></span>

<span data-ttu-id="16a0f-111">Toto téma ukazuje podrobné informace o tom, toorun úplné redigování pracovní postup pomocí Azure Media Services Explorer (AMSE) a Azure Media Redactor vizualizér (nástroj s otevřeným zdrojem).</span><span class="sxs-lookup"><span data-stu-id="16a0f-111">This topic shows step by step instructions on how toorun a full redaction workflow using Azure Media Services Explorer (AMSE) and Azure Media Redactor Visualizer (open source tool).</span></span>

<span data-ttu-id="16a0f-112">Hello **Azure Media Redactor** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="16a0f-112">hello **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="16a0f-113">Je k dispozici ve všech veřejných oblastí Azure a také datových centrech US Government a Číně.</span><span class="sxs-lookup"><span data-stu-id="16a0f-113">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="16a0f-114">Tato verze preview je aktuálně zdarma.</span><span class="sxs-lookup"><span data-stu-id="16a0f-114">This preview is currently free of charge.</span></span> <span data-ttu-id="16a0f-115">V aktuální verzi hello je omezena na 10 minut na zpracování video délka.</span><span class="sxs-lookup"><span data-stu-id="16a0f-115">In hello current release, there is a 10 minute limit on processed video length.</span></span>

<span data-ttu-id="16a0f-116">Další informace najdete v tématu [to](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blogu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-116">For more information, see [this](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span></span>

## <a name="azure-media-services-explorer-workflow"></a><span data-ttu-id="16a0f-117">Azure Media Services Explorer pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="16a0f-117">Azure Media Services Explorer workflow</span></span>

<span data-ttu-id="16a0f-118">Hello tooget nejjednodušší způsob, jak začít s Redactor je nástroj AMSE s otevřeným zdrojem toouse hello na githubu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-118">hello easiest way tooget started with Redactor is toouse hello open source AMSE tool on github.</span></span> <span data-ttu-id="16a0f-119">Můžete spustit zjednodušené pracovního postupu prostřednictvím **kombinaci** režim, pokud nemáte potřebujete přístup toohello poznámky json nebo hello vzhled jpg bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="16a0f-119">You can run a simplified workflow via **combined** mode if you don’t need access toohello annotation json or hello face jpg images.</span></span>

### <a name="download-and-setup"></a><span data-ttu-id="16a0f-120">Stažení a instalaci</span><span class="sxs-lookup"><span data-stu-id="16a0f-120">Download and setup</span></span>

1. <span data-ttu-id="16a0f-121">Stáhněte si nástroj AMSE hello z [zde](https://github.com/Azure/Azure-Media-Services-Explorer).</span><span class="sxs-lookup"><span data-stu-id="16a0f-121">Download hello AMSE tool from [here](https://github.com/Azure/Azure-Media-Services-Explorer).</span></span>
1. <span data-ttu-id="16a0f-122">Přihlaste se tooyour účtu Media Services pomocí klíč služby.</span><span class="sxs-lookup"><span data-stu-id="16a0f-122">Log in tooyour Media Services account using your service key.</span></span>

    <span data-ttu-id="16a0f-123">tooobtain hello název účtu a klíčové informace, přejděte toohello [portál Azure](https://portal.azure.com/) a vyberte svůj účet AMS.</span><span class="sxs-lookup"><span data-stu-id="16a0f-123">tooobtain hello account name and key information, go toohello [Azure portal](https://portal.azure.com/) and select your AMS account.</span></span> <span data-ttu-id="16a0f-124">Zvolte Nastavení > klíče.</span><span class="sxs-lookup"><span data-stu-id="16a0f-124">Then, select Settings > Keys.</span></span> <span data-ttu-id="16a0f-125">Zobrazuje název účtu hello Hello Správa klíčů windows a hello primární a sekundární klíče se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="16a0f-125">hello Manage keys windows shows hello account name and hello primary and secondary keys is displayed.</span></span> <span data-ttu-id="16a0f-126">Zkopírujte hodnoty hello název účtu a primární klíč hello.</span><span class="sxs-lookup"><span data-stu-id="16a0f-126">Copy values of hello account name and hello primary key.</span></span>

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a><span data-ttu-id="16a0f-128">První předání – analyzovat režimu</span><span class="sxs-lookup"><span data-stu-id="16a0f-128">First pass – analyze mode</span></span>

1. <span data-ttu-id="16a0f-129">Nahrávání souborů médií prostřednictvím Asset –> nahrávání, nebo prostřednictvím přetažení.</span><span class="sxs-lookup"><span data-stu-id="16a0f-129">Upload your media file through Asset –> Upload, or via drag and drop.</span></span> 
1. <span data-ttu-id="16a0f-130">Klikněte pravým tlačítkem a zpracování souboru média pomocí Media Analytics –> Azure Media Redactor –> analyzovat režimu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-130">Right click and process your media file using Media Analytics –> Azure Media Redactor –> Analyze mode.</span></span> 


![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

<span data-ttu-id="16a0f-133">výstup Hello bude obsahovat soubor json poznámky se data o umístění řez, jakož i jpg každý zjištěný řezu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-133">hello output will include an annotations json file with face location data, as well as a jpg of each detected face.</span></span> 

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a><span data-ttu-id="16a0f-135">Druhý předat – redigovat režimu</span><span class="sxs-lookup"><span data-stu-id="16a0f-135">Second pass – redact mode</span></span>

1. <span data-ttu-id="16a0f-136">Nahrajte vašeho původního asset videa toohello výstup z první fáze hello a nastavit jako primární asset.</span><span class="sxs-lookup"><span data-stu-id="16a0f-136">Upload your original video asset toohello output from hello first pass and set as a primary asset.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. <span data-ttu-id="16a0f-138">(Volitelné) Nahrajte soubor 'Dance_idlist.txt', který zahrnuje nový řádek oddělený seznam ID chcete tooredact hello.</span><span class="sxs-lookup"><span data-stu-id="16a0f-138">(Optional) Upload a 'Dance_idlist.txt' file which includes a newline delimited list of hello IDs you wish tooredact.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. <span data-ttu-id="16a0f-140">(Volitelné) Zkontrolujte všechny úpravy toohello annotations.json souboru, jako je zvýšení hello ohraničující hranice pole.</span><span class="sxs-lookup"><span data-stu-id="16a0f-140">(Optional) Make any edits toohello annotations.json file such as increasing hello bounding box boundaries.</span></span> 
4. <span data-ttu-id="16a0f-141">Klikněte pravým tlačítkem na výstupní asset hello z první fáze hello, vyberte hello Redactor a spustit s hello **Redact** režimu.</span><span class="sxs-lookup"><span data-stu-id="16a0f-141">Right click hello output asset from hello first pass, select hello Redactor, and run with hello **Redact** mode.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. <span data-ttu-id="16a0f-143">Stažení nebo sdílet hello konečné zredigované výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="16a0f-143">Download or share hello final redacted output asset.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a><span data-ttu-id="16a0f-145">Nástroj Azure vizualizér Redactor médií s otevřeným zdrojem</span><span class="sxs-lookup"><span data-stu-id="16a0f-145">Azure Media Redactor Visualizer open source tool</span></span>

<span data-ttu-id="16a0f-146">Typu open source [vizualizér nástroj](https://github.com/Microsoft/azure-media-redactor-visualizer) je navrženou toohelp vývojáři právě spouští ve formátu poznámky hello s analýzou a pomocí výstup hello.</span><span class="sxs-lookup"><span data-stu-id="16a0f-146">An open source [visualizer tool](https://github.com/Microsoft/azure-media-redactor-visualizer) is designed toohelp developers just starting with hello annotations format with parsing and using hello output.</span></span>

<span data-ttu-id="16a0f-147">Po klonování hello úložišti, v pořadí toorun hello projektu, budete potřebovat toodownload FFMPEG z jejich [oficiální web](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="16a0f-147">After you clone hello repo, in order toorun hello project, you will need toodownload FFMPEG from their [official site](https://ffmpeg.org/download.html).</span></span>

<span data-ttu-id="16a0f-148">Pokud jste vývojář při tooparse hello poznámky data JSON, podívejte se do Models.MetaData příklady ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="16a0f-148">If you are a developer trying tooparse hello JSON annotation data, look inside Models.MetaData for sample code examples.</span></span>

### <a name="set-up-hello-tool"></a><span data-ttu-id="16a0f-149">Nastavit nástroj hello</span><span class="sxs-lookup"><span data-stu-id="16a0f-149">Set up hello tool</span></span>

1.  <span data-ttu-id="16a0f-150">Stáhněte si a sestavte celé řešení hello.</span><span class="sxs-lookup"><span data-stu-id="16a0f-150">Download and build hello entire solution.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  <span data-ttu-id="16a0f-152">Stáhněte si FFMPEG z [zde](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="16a0f-152">Download FFMPEG from [here](https://ffmpeg.org/download.html).</span></span> <span data-ttu-id="16a0f-153">Tento projekt byl původně vytvořen s verze be1d324 (2016-10-04) s statické propojení.</span><span class="sxs-lookup"><span data-stu-id="16a0f-153">This project was originally developed with version be1d324 (2016-10-04) with static linking.</span></span> 
3.  <span data-ttu-id="16a0f-154">Zkopírujte ffmpeg.exe a ffprobe.exe toohello stejnou výstupní složky jako AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="16a0f-154">Copy ffmpeg.exe and ffprobe.exe toohello same output folder as AzureMediaRedactor.exe.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. <span data-ttu-id="16a0f-156">Spusťte AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="16a0f-156">Run AzureMediaRedactor.exe.</span></span> 

### <a name="use-hello-tool"></a><span data-ttu-id="16a0f-157">Pomocí nástroje hello</span><span class="sxs-lookup"><span data-stu-id="16a0f-157">Use hello tool</span></span>

1. <span data-ttu-id="16a0f-158">Zpracovat videa v účtu Azure Media Services s hello Redactor MP na režimu analyzovat.</span><span class="sxs-lookup"><span data-stu-id="16a0f-158">Process your video in your Azure Media Services account with hello Redactor MP on Analyze mode.</span></span> 
2. <span data-ttu-id="16a0f-159">Stáhnout hello původní soubor videa a hello výstup hello redigování - analyzovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="16a0f-159">Download both hello original video file and hello output of hello Redaction - Analyze job.</span></span> 
3. <span data-ttu-id="16a0f-160">Spusťte aplikaci vizualizér hello a vyberte soubory hello výše.</span><span class="sxs-lookup"><span data-stu-id="16a0f-160">Run hello visualizer application and choose hello files above.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. <span data-ttu-id="16a0f-162">Zobrazte náhled souboru.</span><span class="sxs-lookup"><span data-stu-id="16a0f-162">Preview your file.</span></span> <span data-ttu-id="16a0f-163">Vyberte, jenž čelí jste chtěli tooblur prostřednictvím hello bočním panelu na hello správné.</span><span class="sxs-lookup"><span data-stu-id="16a0f-163">Select which faces you'd like tooblur via hello sidebar on hello right.</span></span> 
    
    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  <span data-ttu-id="16a0f-165">Vzhled hello ID aktualizuje Hello dolní textové pole.</span><span class="sxs-lookup"><span data-stu-id="16a0f-165">hello bottom text field will update with hello face IDs.</span></span> <span data-ttu-id="16a0f-166">Vytvořte soubor s názvem "idlist.txt" v těchto ID jako seznam oddělený nový řádek.</span><span class="sxs-lookup"><span data-stu-id="16a0f-166">Create a file called "idlist.txt" with these IDs as a newline delimited list.</span></span> 

    >[!NOTE]
    > <span data-ttu-id="16a0f-167">Hello idlist.txt má být uložen v ANSI.</span><span class="sxs-lookup"><span data-stu-id="16a0f-167">hello idlist.txt should be saved in ANSI.</span></span> <span data-ttu-id="16a0f-168">Můžete použít Poznámkový blok toosave v ANSI.</span><span class="sxs-lookup"><span data-stu-id="16a0f-168">You can use notepad toosave in ANSI.</span></span>
    
6.  <span data-ttu-id="16a0f-169">Odešlete tento soubor toohello výstupní asset z kroku 1.</span><span class="sxs-lookup"><span data-stu-id="16a0f-169">Upload this file toohello output asset from step 1.</span></span> <span data-ttu-id="16a0f-170">Nahrání hello původní video toothis asset také a nastavit jako primární asset.</span><span class="sxs-lookup"><span data-stu-id="16a0f-170">Upload hello original video toothis asset as well and set as primary asset.</span></span> 
7.  <span data-ttu-id="16a0f-171">Spusťte úlohu redigování tento prostředek s "Redact" režimu tooget hello v posledním zredigované video.</span><span class="sxs-lookup"><span data-stu-id="16a0f-171">Run Redaction job on this asset with "Redact" mode tooget hello final redacted video.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="16a0f-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16a0f-172">Next steps</span></span> 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="16a0f-173">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="16a0f-173">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="16a0f-174">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="16a0f-174">Related links</span></span>
[<span data-ttu-id="16a0f-175">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="16a0f-175">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="16a0f-176">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="16a0f-176">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[<span data-ttu-id="16a0f-177">Uvedení vzhled redigování pro Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="16a0f-177">Announcing Face Redaction for Azure Media Analytics</span></span>](https://azure.microsoft.com/blog/azure-media-redactor/)
