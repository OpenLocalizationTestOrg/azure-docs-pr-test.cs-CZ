---
title: "Redigovat tyto řezy v Průvodci Azure Media Analytics | Microsoft Docs"
description: "Toto téma ukazuje podrobné informace o tom, jak spustit workflow úplné redigování pomocí Azure Media Services Explorer (AMSE) a Azure Media Redactor vizualizér (nástroj s otevřeným zdrojem)."
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
ms.openlocfilehash: c0c622237f8cdca65fb6933f14cc21e9eb9ac036
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a><span data-ttu-id="0bc21-103">Redigovat tyto řezy v Průvodci Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="0bc21-103">Redact faces with Azure Media Analytics walkthrough</span></span>

## <a name="overview"></a><span data-ttu-id="0bc21-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0bc21-104">Overview</span></span>

<span data-ttu-id="0bc21-105">**Azure Media Redactor** je [Azure Media Analytics](media-services-analytics-overview.md) procesor médií (PP), která nabízí redigování škálovatelné řez v cloudu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in the cloud.</span></span> <span data-ttu-id="0bc21-106">Vzhled redigování umožní vám upravit videa k rozostření řezy vybrané jednotlivce.</span><span class="sxs-lookup"><span data-stu-id="0bc21-106">Face redaction enables you to modify your video in order to blur faces of selected individuals.</span></span> <span data-ttu-id="0bc21-107">Můžete použít službu redigování řez ve veřejné scénáře zabezpečení a média zprávy.</span><span class="sxs-lookup"><span data-stu-id="0bc21-107">You may want to use the face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="0bc21-108">Pár minut záznamů, která obsahuje více řezy může trvat hodiny redigovat ručně, ale s touto službou proces redigování řez bude vyžadovat několika jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="0bc21-108">A few minutes of footage that contains multiple faces can take hours to redact manually, but with this service the face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="0bc21-109">Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="0bc21-110">Podrobnosti o **Azure Media Redactor**, najdete v článku [vzhled redigování přehled](media-services-face-redaction.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-110">For details about  **Azure Media Redactor**, see the [Face redaction overview](media-services-face-redaction.md) topic.</span></span>

<span data-ttu-id="0bc21-111">Toto téma ukazuje podrobné informace o tom, jak spustit workflow úplné redigování pomocí Azure Media Services Explorer (AMSE) a Azure Media Redactor vizualizér (nástroj s otevřeným zdrojem).</span><span class="sxs-lookup"><span data-stu-id="0bc21-111">This topic shows step by step instructions on how to run a full redaction workflow using Azure Media Services Explorer (AMSE) and Azure Media Redactor Visualizer (open source tool).</span></span>

<span data-ttu-id="0bc21-112">**Azure Media Redactor** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="0bc21-112">The **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="0bc21-113">Je k dispozici ve všech veřejných oblastí Azure a také datových centrech US Government a Číně.</span><span class="sxs-lookup"><span data-stu-id="0bc21-113">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="0bc21-114">Tato verze preview je aktuálně zdarma.</span><span class="sxs-lookup"><span data-stu-id="0bc21-114">This preview is currently free of charge.</span></span> <span data-ttu-id="0bc21-115">V aktuální verzi je omezena na 10 minut na zpracování video délka.</span><span class="sxs-lookup"><span data-stu-id="0bc21-115">In the current release, there is a 10 minute limit on processed video length.</span></span>

<span data-ttu-id="0bc21-116">Další informace najdete v tématu [to](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blogu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-116">For more information, see [this](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span></span>

## <a name="azure-media-services-explorer-workflow"></a><span data-ttu-id="0bc21-117">Azure Media Services Explorer pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="0bc21-117">Azure Media Services Explorer workflow</span></span>

<span data-ttu-id="0bc21-118">Nejjednodušší způsob, jak začít pracovat s Redactor je použít nástroj AMSE open source na githubu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-118">The easiest way to get started with Redactor is to use the open source AMSE tool on github.</span></span> <span data-ttu-id="0bc21-119">Můžete spustit zjednodušené pracovního postupu prostřednictvím **kombinaci** režim, pokud nepotřebujete přístup k json poznámky nebo obrázků jpg řez.</span><span class="sxs-lookup"><span data-stu-id="0bc21-119">You can run a simplified workflow via **combined** mode if you don’t need access to the annotation json or the face jpg images.</span></span>

### <a name="download-and-setup"></a><span data-ttu-id="0bc21-120">Stažení a instalaci</span><span class="sxs-lookup"><span data-stu-id="0bc21-120">Download and setup</span></span>

1. <span data-ttu-id="0bc21-121">Stáhněte si nástroj AMSE z [zde](https://github.com/Azure/Azure-Media-Services-Explorer).</span><span class="sxs-lookup"><span data-stu-id="0bc21-121">Download the AMSE tool from [here](https://github.com/Azure/Azure-Media-Services-Explorer).</span></span>
1. <span data-ttu-id="0bc21-122">Přihlaste se k vašemu účtu Media Services pomocí klíč služby.</span><span class="sxs-lookup"><span data-stu-id="0bc21-122">Log in to your Media Services account using your service key.</span></span>

    <span data-ttu-id="0bc21-123">Pokud chcete získat název účtu a informace o klíči, přejděte na [Azure Portal](https://portal.azure.com/) a vyberte svůj účet AMS.</span><span class="sxs-lookup"><span data-stu-id="0bc21-123">To obtain the account name and key information, go to the [Azure portal](https://portal.azure.com/) and select your AMS account.</span></span> <span data-ttu-id="0bc21-124">Zvolte Nastavení > klíče.</span><span class="sxs-lookup"><span data-stu-id="0bc21-124">Then, select Settings > Keys.</span></span> <span data-ttu-id="0bc21-125">Zobrazí se okno Správa klíčů, které ukazuje název účtu a primární a sekundární klíče.</span><span class="sxs-lookup"><span data-stu-id="0bc21-125">The Manage keys windows shows the account name and the primary and secondary keys is displayed.</span></span> <span data-ttu-id="0bc21-126">Zkopírujte hodnoty názvu účtu a primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="0bc21-126">Copy values of the account name and the primary key.</span></span>

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a><span data-ttu-id="0bc21-128">První předání – analyzovat režimu</span><span class="sxs-lookup"><span data-stu-id="0bc21-128">First pass – analyze mode</span></span>

1. <span data-ttu-id="0bc21-129">Nahrávání souborů médií prostřednictvím Asset –> nahrávání, nebo prostřednictvím přetažení.</span><span class="sxs-lookup"><span data-stu-id="0bc21-129">Upload your media file through Asset –> Upload, or via drag and drop.</span></span> 
1. <span data-ttu-id="0bc21-130">Klikněte pravým tlačítkem a zpracování souboru média pomocí Media Analytics –> Azure Media Redactor –> analyzovat režimu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-130">Right click and process your media file using Media Analytics –> Azure Media Redactor –> Analyze mode.</span></span> 


![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

<span data-ttu-id="0bc21-133">Výstup bude obsahovat soubor json poznámky se data o umístění řez, jakož i jpg každý zjištěný řezu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-133">The output will include an annotations json file with face location data, as well as a jpg of each detected face.</span></span> 

![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a><span data-ttu-id="0bc21-135">Druhý předat – redigovat režimu</span><span class="sxs-lookup"><span data-stu-id="0bc21-135">Second pass – redact mode</span></span>

1. <span data-ttu-id="0bc21-136">Nahrát asset videa původní k výstupu z první fáze a nastavit jako primární asset.</span><span class="sxs-lookup"><span data-stu-id="0bc21-136">Upload your original video asset to the output from the first pass and set as a primary asset.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. <span data-ttu-id="0bc21-138">(Volitelné) Nahrajte soubor 'Dance_idlist.txt', který zahrnuje nový řádek oddělený seznam ID chcete redigovat.</span><span class="sxs-lookup"><span data-stu-id="0bc21-138">(Optional) Upload a 'Dance_idlist.txt' file which includes a newline delimited list of the IDs you wish to redact.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. <span data-ttu-id="0bc21-140">(Volitelné) Proveďte veškeré úpravy souboru annotations.json například zvýšení ohraničující hranice pole.</span><span class="sxs-lookup"><span data-stu-id="0bc21-140">(Optional) Make any edits to the annotations.json file such as increasing the bounding box boundaries.</span></span> 
4. <span data-ttu-id="0bc21-141">Klikněte pravým tlačítkem na výstupní asset z první fáze, vyberte Redactor a spustit se **Redact** režimu.</span><span class="sxs-lookup"><span data-stu-id="0bc21-141">Right click the output asset from the first pass, select the Redactor, and run with the **Redact** mode.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. <span data-ttu-id="0bc21-143">Stažení nebo sdílet konečné zredigované výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="0bc21-143">Download or share the final redacted output asset.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a><span data-ttu-id="0bc21-145">Nástroj Azure vizualizér Redactor médií s otevřeným zdrojem</span><span class="sxs-lookup"><span data-stu-id="0bc21-145">Azure Media Redactor Visualizer open source tool</span></span>

<span data-ttu-id="0bc21-146">Typu open source [vizualizér nástroj](https://github.com/Microsoft/azure-media-redactor-visualizer) pomáhá vývojářům právě spouští s formátem poznámky s analýzou a pomocí výstup.</span><span class="sxs-lookup"><span data-stu-id="0bc21-146">An open source [visualizer tool](https://github.com/Microsoft/azure-media-redactor-visualizer) is designed to help developers just starting with the annotations format with parsing and using the output.</span></span>

<span data-ttu-id="0bc21-147">Po naklonujte úložiště, aby bylo možné spustit projekt, budete muset stáhnout FFMPEG z jejich [oficiální web](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="0bc21-147">After you clone the repo, in order to run the project, you will need to download FFMPEG from their [official site](https://ffmpeg.org/download.html).</span></span>

<span data-ttu-id="0bc21-148">Pokud jste vývojář pokusu o analýzu poznámky data JSON, podívejte se do Models.MetaData příklady ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="0bc21-148">If you are a developer trying to parse the JSON annotation data, look inside Models.MetaData for sample code examples.</span></span>

### <a name="set-up-the-tool"></a><span data-ttu-id="0bc21-149">Nastavení nástroje</span><span class="sxs-lookup"><span data-stu-id="0bc21-149">Set up the tool</span></span>

1.  <span data-ttu-id="0bc21-150">Stáhněte si a sestavte celé řešení.</span><span class="sxs-lookup"><span data-stu-id="0bc21-150">Download and build the entire solution.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  <span data-ttu-id="0bc21-152">Stáhněte si FFMPEG z [zde](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="0bc21-152">Download FFMPEG from [here](https://ffmpeg.org/download.html).</span></span> <span data-ttu-id="0bc21-153">Tento projekt byl původně vytvořen s verze be1d324 (2016-10-04) s statické propojení.</span><span class="sxs-lookup"><span data-stu-id="0bc21-153">This project was originally developed with version be1d324 (2016-10-04) with static linking.</span></span> 
3.  <span data-ttu-id="0bc21-154">Zkopírujte ffmpeg.exe a ffprobe.exe do stejné složky jako AzureMediaRedactor.exe výstup.</span><span class="sxs-lookup"><span data-stu-id="0bc21-154">Copy ffmpeg.exe and ffprobe.exe to the same output folder as AzureMediaRedactor.exe.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. <span data-ttu-id="0bc21-156">Spusťte AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="0bc21-156">Run AzureMediaRedactor.exe.</span></span> 

### <a name="use-the-tool"></a><span data-ttu-id="0bc21-157">Pomocí nástroje</span><span class="sxs-lookup"><span data-stu-id="0bc21-157">Use the tool</span></span>

1. <span data-ttu-id="0bc21-158">Zpracovat videa v účtu Azure Media Services s Redactor MP na režimu analyzovat.</span><span class="sxs-lookup"><span data-stu-id="0bc21-158">Process your video in your Azure Media Services account with the Redactor MP on Analyze mode.</span></span> 
2. <span data-ttu-id="0bc21-159">Stáhněte si původní soubor videa a výstup redigování – analyzovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="0bc21-159">Download both the original video file and the output of the Redaction - Analyze job.</span></span> 
3. <span data-ttu-id="0bc21-160">Spusťte aplikaci vizualizér a vyberte soubory výše.</span><span class="sxs-lookup"><span data-stu-id="0bc21-160">Run the visualizer application and choose the files above.</span></span> 

    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. <span data-ttu-id="0bc21-162">Zobrazte náhled souboru.</span><span class="sxs-lookup"><span data-stu-id="0bc21-162">Preview your file.</span></span> <span data-ttu-id="0bc21-163">Vyberte řezy, které byste chtěli rozostření prostřednictvím na bočním panelu na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="0bc21-163">Select which faces you'd like to blur via the sidebar on the right.</span></span> 
    
    ![Rozmazání obličejů](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  <span data-ttu-id="0bc21-165">Písmo ID aktualizuje dolní textové pole.</span><span class="sxs-lookup"><span data-stu-id="0bc21-165">The bottom text field will update with the face IDs.</span></span> <span data-ttu-id="0bc21-166">Vytvořte soubor s názvem "idlist.txt" v těchto ID jako seznam oddělený nový řádek.</span><span class="sxs-lookup"><span data-stu-id="0bc21-166">Create a file called "idlist.txt" with these IDs as a newline delimited list.</span></span> 

    >[!NOTE]
    > <span data-ttu-id="0bc21-167">Idlist.txt má být uložen v ANSI.</span><span class="sxs-lookup"><span data-stu-id="0bc21-167">The idlist.txt should be saved in ANSI.</span></span> <span data-ttu-id="0bc21-168">Poznámkový blok můžete uložit v ANSI.</span><span class="sxs-lookup"><span data-stu-id="0bc21-168">You can use notepad to save in ANSI.</span></span>
    
6.  <span data-ttu-id="0bc21-169">Tento soubor nahrajte na výstupní asset z kroku 1.</span><span class="sxs-lookup"><span data-stu-id="0bc21-169">Upload this file to the output asset from step 1.</span></span> <span data-ttu-id="0bc21-170">Pro tento prostředek také nahrát původní video a nastavit jako primární asset.</span><span class="sxs-lookup"><span data-stu-id="0bc21-170">Upload the original video to this asset as well and set as primary asset.</span></span> 
7.  <span data-ttu-id="0bc21-171">Spustit úlohu redigování na tento prostředek s režimem "Redact" získat konečné redigovaný video.</span><span class="sxs-lookup"><span data-stu-id="0bc21-171">Run Redaction job on this asset with "Redact" mode to get the final redacted video.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0bc21-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0bc21-172">Next steps</span></span> 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0bc21-173">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="0bc21-173">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="0bc21-174">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="0bc21-174">Related links</span></span>
[<span data-ttu-id="0bc21-175">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="0bc21-175">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="0bc21-176">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="0bc21-176">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[<span data-ttu-id="0bc21-177">Uvedení vzhled redigování pro Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="0bc21-177">Announcing Face Redaction for Azure Media Analytics</span></span>](https://azure.microsoft.com/blog/azure-media-redactor/)
