---
title: "na platformě Media Services hello aaaMedia Analytics | Microsoft Docs"
description: "Přehled verze public Preview Media Analytics, kolekce řečových a počítače služby vize na škálování enterprise, dodržování předpisů, zabezpečení a globální sítě"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 7545f0532d7618164ebe65e2f4232c5f63453cfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="media-analytics-on-hello-media-services-platform"></a><span data-ttu-id="e42b9-103">Media Analytics na platformě Media Services hello</span><span class="sxs-lookup"><span data-stu-id="e42b9-103">Media Analytics on hello Media Services platform</span></span>
## <a name="overview"></a><span data-ttu-id="e42b9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e42b9-104">Overview</span></span>
<span data-ttu-id="e42b9-105">Další organizace používáte video jako hello upřednostňované střední tootrain svým zaměstnancům, zaujmout své zákazníky a dokumentu podnikovým funkcím.</span><span class="sxs-lookup"><span data-stu-id="e42b9-105">More organizations are using video as hello preferred medium tootrain their employees, engage their customers, and document business functions.</span></span> <span data-ttu-id="e42b9-106">Cloud computing poskytuje toostore způsob datového proudu a přístup k těmto souborům velké média.</span><span class="sxs-lookup"><span data-stu-id="e42b9-106">Cloud computing provides a way toostore, stream, and access these large media files.</span></span> <span data-ttu-id="e42b9-107">Ale s růstem společnosti knihovny obsahu videa, je nutné extrahování statistiky z obsahu hello stejně účinnou.</span><span class="sxs-lookup"><span data-stu-id="e42b9-107">But as a company's library of video content grows, it needs an equally effective means of extracting insights from hello content.</span></span> 

<span data-ttu-id="e42b9-108">tooaddress tento rostoucí potřeby Azure Media Services nabízí Azure Media Analytics.</span><span class="sxs-lookup"><span data-stu-id="e42b9-108">tooaddress this growing need, Azure Media Services offers Azure Media Analytics.</span></span> <span data-ttu-id="e42b9-109">Media Analytics je kolekce řečových a vizuálních komponent, které usnadňuje organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů.</span><span class="sxs-lookup"><span data-stu-id="e42b9-109">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="e42b9-110">Vytvořené s použitím komponenty platformy Media Services hello jádra, Media Analytics může zpracovávat média zpracování škálované na jeden den.</span><span class="sxs-lookup"><span data-stu-id="e42b9-110">Built by using hello core Media Services platform components, Media Analytics can handle media processing at scale on day one.</span></span>

<span data-ttu-id="e42b9-111">Pomocí Media Analytics vývojáři rychle uveďte pokročilé funkce video do aplikace.</span><span class="sxs-lookup"><span data-stu-id="e42b9-111">With Media Analytics, developers can quickly bring advanced video functionality into applications.</span></span> <span data-ttu-id="e42b9-112">Poskytne podnikových prostředích hello úplné škálování, dodržování předpisů, zabezpečení a globální reach potřebné ve velkých organizacích.</span><span class="sxs-lookup"><span data-stu-id="e42b9-112">It provides enterprise environments with hello full scale, compliance, security, and global reach required by large organizations.</span></span>

<span data-ttu-id="e42b9-113">Hello následující diagram znázorňuje analýzy mediálních služeb a jiných hlavní části platformy Media Services hello.</span><span class="sxs-lookup"><span data-stu-id="e42b9-113">hello following diagram shows Media Analytics and other major parts of hello Media Services platform.</span></span> 

![Pracovní postup videa na vyžádání (VoD)](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

<span data-ttu-id="e42b9-115">Procesory médií z Media Analytics vytvářejí soubory MP4 nebo soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="e42b9-115">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="e42b9-116">Pokud procesor médií vytvoří soubor MP4, můžete ho progresivně stahovat hello souboru.</span><span class="sxs-lookup"><span data-stu-id="e42b9-116">If a media processor produces an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="e42b9-117">Pokud procesor médií vytvoří soubor JSON, hello soubor můžete stáhnout z úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="e42b9-117">If a media processor produces a JSON file, you can download hello file from Azure Blob storage.</span></span> 

## <a name="media-analytics-services"></a><span data-ttu-id="e42b9-118">Služeb Media Analytics</span><span class="sxs-lookup"><span data-stu-id="e42b9-118">Media Analytics services</span></span>

### <a name="indexer"></a><span data-ttu-id="e42b9-119">Indexovací modul</span><span class="sxs-lookup"><span data-stu-id="e42b9-119">Indexer</span></span>
<span data-ttu-id="e42b9-120">S Azure Media Indexer můžete provést s možností vyhledávání obsahu a generovat titulky sleduje.</span><span class="sxs-lookup"><span data-stu-id="e42b9-120">With Azure Media Indexer, you can make content searchable and generate closed-captioning tracks.</span></span> <span data-ttu-id="e42b9-121">Porovnání toohello předchozí verze, Azure Media Indexer 2 Preview má rychlejší indexování a širší jazykové podpory.</span><span class="sxs-lookup"><span data-stu-id="e42b9-121">Compared toohello previous version, Azure Media Indexer 2 Preview has faster indexing and broader language support.</span></span> <span data-ttu-id="e42b9-122">Mezi podporované jazyky patří angličtina, španělština, francouzština, němčina, italština, čínština, portugalština a arabské.</span><span class="sxs-lookup"><span data-stu-id="e42b9-122">Supported languages include English, Spanish, French, German, Italian, Chinese, Portuguese, and Arabic.</span></span> <span data-ttu-id="e42b9-123">Podrobné informace a příklady naleznete v tématu [zpracování videí pomocí Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span><span class="sxs-lookup"><span data-stu-id="e42b9-123">For detailed information and examples, see [Process videos with Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span></span>
### <a name="hyperlapse"></a><span data-ttu-id="e42b9-124">Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="e42b9-124">Hyperlapse</span></span>
<span data-ttu-id="e42b9-125">Microsoft Hyperlapse kombinuje video ustálení a časové závislosti schopností toocreate rychlý a použití videa z webu dlouhých obsah.</span><span class="sxs-lookup"><span data-stu-id="e42b9-125">Microsoft Hyperlapse combines video stabilization and time-lapse capability toocreate quick, consumable videos from your long-form content.</span></span> <span data-ttu-id="e42b9-126">Kromě vytvoření časové závislosti video, můžete použít Hyperlapse toocreate stabilní videa z webu zobrazuje rozostřený videa zachytit přes mobilní telefony a videokamer.</span><span class="sxs-lookup"><span data-stu-id="e42b9-126">Besides creating time-lapse video, you can use Hyperlapse toocreate stable videos from shaky videos captured via cell phones and camcorders.</span></span> <span data-ttu-id="e42b9-127">Podrobné informace a příklady naleznete v tématu [Hyperlapse mediálních souborů pomocí Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span><span class="sxs-lookup"><span data-stu-id="e42b9-127">For detailed information and examples, see [Hyperlapse media files with Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span></span>
### <a name="motion-detector"></a><span data-ttu-id="e42b9-128">Detektor pohybu</span><span class="sxs-lookup"><span data-stu-id="e42b9-128">Motion Detector</span></span>
<span data-ttu-id="e42b9-129">Detektor pohybu toodetect pohybu můžete použít v video s stojící pozadí.</span><span class="sxs-lookup"><span data-stu-id="e42b9-129">You can use Motion Detector toodetect motion in a video with stationary backgrounds.</span></span> <span data-ttu-id="e42b9-130">Díky tomu je možné toocheck pro falešně pozitivních na pohybu událostí detekovaných službou sledováním kamery.</span><span class="sxs-lookup"><span data-stu-id="e42b9-130">This makes it possible toocheck for false positives on motion events detected by surveillance cameras.</span></span> <span data-ttu-id="e42b9-131">Podrobné informace a příklady naleznete v tématu [pro Azure Media Analytics detekce pohybu](media-services-motion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="e42b9-131">For detailed information and examples, see [Motion detection for Azure Media Analytics](media-services-motion-detection.md).</span></span>
### <a name="face-detector"></a><span data-ttu-id="e42b9-132">Detektor tváří</span><span class="sxs-lookup"><span data-stu-id="e42b9-132">Face Detector</span></span>
<span data-ttu-id="e42b9-133">Pomocí detektor vzhled, můžete zjistit řezy uživatelů a jejich emoce, včetně štěstí, sadness a neočekávaném.</span><span class="sxs-lookup"><span data-stu-id="e42b9-133">By using Face Detector, you can detect people’s faces and their emotions, including happiness, sadness, and surprise.</span></span> <span data-ttu-id="e42b9-134">To má několik užitečné odvětví aplikace, popsané dál, včetně agregaci a analýzu reakce lidí, kteří se účastní událost.</span><span class="sxs-lookup"><span data-stu-id="e42b9-134">This has several useful industry applications, described later, including aggregating and analyzing reactions of people attending an event.</span></span> <span data-ttu-id="e42b9-135">Podrobné informace a příklady naleznete v tématu [vzhled a rozpoznávání emocí úrovně zjišťování pro Azure Media Analytics](media-services-face-and-emotion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="e42b9-135">For detailed information and examples, see [Face and emotion detection for Azure Media Analytics](media-services-face-and-emotion-detection.md).</span></span>
### <a name="video-summarization"></a><span data-ttu-id="e42b9-136">Videosouhrn</span><span class="sxs-lookup"><span data-stu-id="e42b9-136">Video summarization</span></span>
<span data-ttu-id="e42b9-137">Videosouhrn vám pomůžou vytvořit souhrnných informací o dlouho videa automaticky výběrem zajímavé fragmenty kódu z hello zdroj videa.</span><span class="sxs-lookup"><span data-stu-id="e42b9-137">Video summarization can help you create summaries of long videos by automatically selecting interesting snippets from hello source video.</span></span> <span data-ttu-id="e42b9-138">Tato možnost je užitečná, pokud chcete tooprovide rychlý přehled toho, jaké tooexpect v dlouho video.</span><span class="sxs-lookup"><span data-stu-id="e42b9-138">This ability is useful when you want tooprovide a quick overview of what tooexpect in a long video.</span></span> <span data-ttu-id="e42b9-139">Podrobné informace a příklady naleznete v tématu [pomocí Azure Media Video miniatur toocreate videosouhrn](media-services-video-summarization.md).</span><span class="sxs-lookup"><span data-stu-id="e42b9-139">For detailed information and examples, see [Use Azure Media Video Thumbnails toocreate video summarization](media-services-video-summarization.md).</span></span>
### <a name="optical-character-recognition"></a><span data-ttu-id="e42b9-140">Optické rozpoznávání znaků</span><span class="sxs-lookup"><span data-stu-id="e42b9-140">Optical character recognition</span></span>
<span data-ttu-id="e42b9-141">S Azure Media rozpoznávání znaků (optické rozpoznávání znaků) můžete upravovat, vyhledávat digitální text převést textového obsahu v video soubory.</span><span class="sxs-lookup"><span data-stu-id="e42b9-141">With Azure Media OCR (optical character recognition), you can convert text content in video files into editable, searchable digital text.</span></span> <span data-ttu-id="e42b9-142">Pak můžete automatizovat hello extrakce smysluplný metadata z hello signál video média.</span><span class="sxs-lookup"><span data-stu-id="e42b9-142">You can then automate hello extraction of meaningful metadata from hello video signal of your media.</span></span>
### <a name="scalable-face-redaction"></a><span data-ttu-id="e42b9-143">Škálovatelné vzhled redigování</span><span class="sxs-lookup"><span data-stu-id="e42b9-143">Scalable face redaction</span></span>
<span data-ttu-id="e42b9-144">Azure Media Redactor je procesor médií Media Analytics, která nabízí redigování škálovatelné řez v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e42b9-144">Azure Media Redactor is a Media Analytics media processor that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="e42b9-145">Pomocí redigování vzhled můžete upravit vaše video tooblur řezy vybrané osob.</span><span class="sxs-lookup"><span data-stu-id="e42b9-145">By using face redaction, you can modify your video tooblur faces of selected individuals.</span></span> <span data-ttu-id="e42b9-146">Můžete chtít toouse hello vzhled redigování služby v média zprávy, nebo když je zahrnuta veřejné zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e42b9-146">You might want toouse hello face redaction service in news media or when public safety is involved.</span></span> <span data-ttu-id="e42b9-147">Pár minut záznamů, která obsahuje více řezy může trvat hodiny tooredact ručně, ale s touto službou vzhled redigování trvá jenom pár jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="e42b9-147">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service, face redaction takes just a few simple steps.</span></span> <span data-ttu-id="e42b9-148">Další informace najdete v tématu hello [redigovat řezy s Azure Media Analytics](media-services-face-redaction.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e42b9-148">For more information, see hello [Redact faces with Azure Media Analytics](media-services-face-redaction.md) article.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="e42b9-149">Obvyklé scénáře</span><span class="sxs-lookup"><span data-stu-id="e42b9-149">Common scenarios</span></span>
<span data-ttu-id="e42b9-150">Media Analytics může pomoci organizacím a podnikům glean nové přehledy z video a další efektivně spravovat velké objemy obsahu videa.</span><span class="sxs-lookup"><span data-stu-id="e42b9-150">Media Analytics can help organizations and enterprises glean new insights from video and more effectively manage large volumes of video content.</span></span> <span data-ttu-id="e42b9-151">Tady je několik scénářů:</span><span class="sxs-lookup"><span data-stu-id="e42b9-151">Here are several scenarios:</span></span>

* <span data-ttu-id="e42b9-152">**Volání centrech**.</span><span class="sxs-lookup"><span data-stu-id="e42b9-152">**Call centers**.</span></span> <span data-ttu-id="e42b9-153">I s nástupem hello sociálních médií stále zákazníka telefonní centra usnadnění vysoké procento oddělení služeb zákazníkům transakce.</span><span class="sxs-lookup"><span data-stu-id="e42b9-153">Even with hello advent of social media, customer call centers still facilitate a large percentage of customer-service transactions.</span></span> <span data-ttu-id="e42b9-154">V této zvuková data kódování je velké množství informace o zákazníkovi, který lze analyzovat tooachieve vyšší spokojenost zákazníků.</span><span class="sxs-lookup"><span data-stu-id="e42b9-154">Encoded in this audio data is a large amount of customer information that can be analyzed tooachieve higher customer satisfaction.</span></span> <span data-ttu-id="e42b9-155">Pomocí Media Indexer organizace rozbalte text a vytvářejte indexy vyhledávání a řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="e42b9-155">By using Media Indexer, organizations can extract text and build search indexes and dashboards.</span></span> <span data-ttu-id="e42b9-156">Potom mohli extrahovat intelligence kolem častých stížností, zdroje stížností a další relevantní data.</span><span class="sxs-lookup"><span data-stu-id="e42b9-156">Then they can extract intelligence around common complaints, sources of complaints, and other relevant data.</span></span>
* <span data-ttu-id="e42b9-157">**Uživatelem generovaný obsah přerušování**.</span><span class="sxs-lookup"><span data-stu-id="e42b9-157">**User-generated content moderation**.</span></span> <span data-ttu-id="e42b9-158">Z média zprávy výstupy toopolice oddělení má řada organizací veřejné portály, které přijímají uživatelem generovaný média, například videa a obrázků.</span><span class="sxs-lookup"><span data-stu-id="e42b9-158">From news media outlets toopolice departments, many organizations have public-facing portals that accept user-generated media such as videos and images.</span></span> <span data-ttu-id="e42b9-159">z důvodu události toounexpected můžete špiček Hello množstvím obsahu.</span><span class="sxs-lookup"><span data-stu-id="e42b9-159">hello volume of content can spike due toounexpected events.</span></span> <span data-ttu-id="e42b9-160">V těchto scénářích je obtížné tooconduct efektivní ruční recenze obsahu na základě vhodnosti.</span><span class="sxs-lookup"><span data-stu-id="e42b9-160">In these scenarios, it is difficult tooconduct effective manual reviews of content for appropriateness.</span></span> <span data-ttu-id="e42b9-161">Zákazníci může se spoléhat na hello obsah přerušování služby toofocus na obsah, který je vhodný.</span><span class="sxs-lookup"><span data-stu-id="e42b9-161">Customers can rely on hello content-moderation service toofocus on content that is appropriate.</span></span>
* <span data-ttu-id="e42b9-162">**Sledováním**.</span><span class="sxs-lookup"><span data-stu-id="e42b9-162">**Surveillance**.</span></span> <span data-ttu-id="e42b9-163">S hello dodává růst v použití kamer IP rostoucí inventáře sledováním videa.</span><span class="sxs-lookup"><span data-stu-id="e42b9-163">With hello growth in use of IP cameras comes a growing inventory of surveillance video.</span></span> <span data-ttu-id="e42b9-164">Ruční kontrola sledováním video je čas náročné a náchylné k chybám toohuman chyba.</span><span class="sxs-lookup"><span data-stu-id="e42b9-164">Manually reviewing surveillance video is time intensive and prone toohuman error.</span></span> <span data-ttu-id="e42b9-165">Media Analytics poskytuje službám, jako je detekce pohybu, detekce vzhled a Hyperlapse toomake hello proces kontrola, Správa a vytváření odvozené konfigurace jednodušší.</span><span class="sxs-lookup"><span data-stu-id="e42b9-165">Media Analytics provides services such as motion detection, face detection, and Hyperlapse toomake hello process of reviewing, managing, and creating derivatives easier.</span></span>

## <a name="media-analytics-media-processors"></a><span data-ttu-id="e42b9-166">Procesory médií z Media Analytics</span><span class="sxs-lookup"><span data-stu-id="e42b9-166">Media Analytics media processors</span></span>
<span data-ttu-id="e42b9-167">Tato část jak procesory médií z Media Analytics hello seznamy a ukazuje toouse .NET nebo REST tooget objekt média procesoru (PP).</span><span class="sxs-lookup"><span data-stu-id="e42b9-167">This section lists hello Media Analytics media processors and shows how toouse .NET or REST tooget a media processor (MP) object.</span></span>

### <a name="mp-names"></a><span data-ttu-id="e42b9-168">Názvy MP</span><span class="sxs-lookup"><span data-stu-id="e42b9-168">MP names</span></span>
* <span data-ttu-id="e42b9-169">Azure Media Indexer 2 Preview</span><span class="sxs-lookup"><span data-stu-id="e42b9-169">Azure Media Indexer 2 Preview</span></span>
* <span data-ttu-id="e42b9-170">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="e42b9-170">Azure Media Indexer</span></span>
* <span data-ttu-id="e42b9-171">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="e42b9-171">Azure Media Hyperlapse</span></span>
* <span data-ttu-id="e42b9-172">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="e42b9-172">Azure Media Face Detector</span></span>
* <span data-ttu-id="e42b9-173">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="e42b9-173">Azure Media Motion Detector</span></span>
* <span data-ttu-id="e42b9-174">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="e42b9-174">Azure Media Video Thumbnails</span></span>
* <span data-ttu-id="e42b9-175">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="e42b9-175">Azure Media OCR</span></span>

### <a name="net"></a><span data-ttu-id="e42b9-176">.NET</span><span class="sxs-lookup"><span data-stu-id="e42b9-176">.NET</span></span>
<span data-ttu-id="e42b9-177">Následující funkce trvá jeden Dobrý den Hello zadané MP názvy a vrátí objekt sady Management Pack.</span><span class="sxs-lookup"><span data-stu-id="e42b9-177">hello following function takes one of hello specified MP names and returns an MP object.</span></span>

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a><span data-ttu-id="e42b9-178">REST</span><span class="sxs-lookup"><span data-stu-id="e42b9-178">REST</span></span>
<span data-ttu-id="e42b9-179">Žádost:</span><span class="sxs-lookup"><span data-stu-id="e42b9-179">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

<span data-ttu-id="e42b9-180">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="e42b9-180">Response:</span></span>

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a><span data-ttu-id="e42b9-181">Ukázky</span><span class="sxs-lookup"><span data-stu-id="e42b9-181">Demos</span></span>
<span data-ttu-id="e42b9-182">V tématu [ukázek Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span><span class="sxs-lookup"><span data-stu-id="e42b9-182">See [Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e42b9-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e42b9-183">Next steps</span></span>
<span data-ttu-id="e42b9-184">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="e42b9-184">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e42b9-185">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e42b9-185">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="e42b9-186">Související články</span><span class="sxs-lookup"><span data-stu-id="e42b9-186">Related articles</span></span>
<span data-ttu-id="e42b9-187">V tématu [Media Services Analytics oznámení](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span><span class="sxs-lookup"><span data-stu-id="e42b9-187">See [Media Services Analytics announcement](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span></span>

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
