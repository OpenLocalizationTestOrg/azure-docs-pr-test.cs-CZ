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
# <a name="media-analytics-on-hello-media-services-platform"></a>Media Analytics na platformě Media Services hello
## <a name="overview"></a>Přehled
Další organizace používáte video jako hello upřednostňované střední tootrain svým zaměstnancům, zaujmout své zákazníky a dokumentu podnikovým funkcím. Cloud computing poskytuje toostore způsob datového proudu a přístup k těmto souborům velké média. Ale s růstem společnosti knihovny obsahu videa, je nutné extrahování statistiky z obsahu hello stejně účinnou. 

tooaddress tento rostoucí potřeby Azure Media Services nabízí Azure Media Analytics. Media Analytics je kolekce řečových a vizuálních komponent, které usnadňuje organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů. Vytvořené s použitím komponenty platformy Media Services hello jádra, Media Analytics může zpracovávat média zpracování škálované na jeden den.

Pomocí Media Analytics vývojáři rychle uveďte pokročilé funkce video do aplikace. Poskytne podnikových prostředích hello úplné škálování, dodržování předpisů, zabezpečení a globální reach potřebné ve velkých organizacích.

Hello následující diagram znázorňuje analýzy mediálních služeb a jiných hlavní části platformy Media Services hello. 

![Pracovní postup videa na vyžádání (VoD)](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Procesory médií z Media Analytics vytvářejí soubory MP4 nebo soubory JSON. Pokud procesor médií vytvoří soubor MP4, můžete ho progresivně stahovat hello souboru. Pokud procesor médií vytvoří soubor JSON, hello soubor můžete stáhnout z úložiště objektů Blob Azure. 

## <a name="media-analytics-services"></a>Služeb Media Analytics

### <a name="indexer"></a>Indexovací modul
S Azure Media Indexer můžete provést s možností vyhledávání obsahu a generovat titulky sleduje. Porovnání toohello předchozí verze, Azure Media Indexer 2 Preview má rychlejší indexování a širší jazykové podpory. Mezi podporované jazyky patří angličtina, španělština, francouzština, němčina, italština, čínština, portugalština a arabské. Podrobné informace a příklady naleznete v tématu [zpracování videí pomocí Azure Media Indexer 2](media-services-process-content-with-indexer2.md).
### <a name="hyperlapse"></a>Hyperlapse
Microsoft Hyperlapse kombinuje video ustálení a časové závislosti schopností toocreate rychlý a použití videa z webu dlouhých obsah. Kromě vytvoření časové závislosti video, můžete použít Hyperlapse toocreate stabilní videa z webu zobrazuje rozostřený videa zachytit přes mobilní telefony a videokamer. Podrobné informace a příklady naleznete v tématu [Hyperlapse mediálních souborů pomocí Azure Media Hyperlapse](media-services-hyperlapse-content.md).
### <a name="motion-detector"></a>Detektor pohybu
Detektor pohybu toodetect pohybu můžete použít v video s stojící pozadí. Díky tomu je možné toocheck pro falešně pozitivních na pohybu událostí detekovaných službou sledováním kamery. Podrobné informace a příklady naleznete v tématu [pro Azure Media Analytics detekce pohybu](media-services-motion-detection.md).
### <a name="face-detector"></a>Detektor tváří
Pomocí detektor vzhled, můžete zjistit řezy uživatelů a jejich emoce, včetně štěstí, sadness a neočekávaném. To má několik užitečné odvětví aplikace, popsané dál, včetně agregaci a analýzu reakce lidí, kteří se účastní událost. Podrobné informace a příklady naleznete v tématu [vzhled a rozpoznávání emocí úrovně zjišťování pro Azure Media Analytics](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Videosouhrn
Videosouhrn vám pomůžou vytvořit souhrnných informací o dlouho videa automaticky výběrem zajímavé fragmenty kódu z hello zdroj videa. Tato možnost je užitečná, pokud chcete tooprovide rychlý přehled toho, jaké tooexpect v dlouho video. Podrobné informace a příklady naleznete v tématu [pomocí Azure Media Video miniatur toocreate videosouhrn](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optické rozpoznávání znaků
S Azure Media rozpoznávání znaků (optické rozpoznávání znaků) můžete upravovat, vyhledávat digitální text převést textového obsahu v video soubory. Pak můžete automatizovat hello extrakce smysluplný metadata z hello signál video média.
### <a name="scalable-face-redaction"></a>Škálovatelné vzhled redigování
Azure Media Redactor je procesor médií Media Analytics, která nabízí redigování škálovatelné řez v cloudu hello. Pomocí redigování vzhled můžete upravit vaše video tooblur řezy vybrané osob. Můžete chtít toouse hello vzhled redigování služby v média zprávy, nebo když je zahrnuta veřejné zabezpečení. Pár minut záznamů, která obsahuje více řezy může trvat hodiny tooredact ručně, ale s touto službou vzhled redigování trvá jenom pár jednoduchých kroků. Další informace najdete v tématu hello [redigovat řezy s Azure Media Analytics](media-services-face-redaction.md) článku.

## <a name="common-scenarios"></a>Obvyklé scénáře
Media Analytics může pomoci organizacím a podnikům glean nové přehledy z video a další efektivně spravovat velké objemy obsahu videa. Tady je několik scénářů:

* **Volání centrech**. I s nástupem hello sociálních médií stále zákazníka telefonní centra usnadnění vysoké procento oddělení služeb zákazníkům transakce. V této zvuková data kódování je velké množství informace o zákazníkovi, který lze analyzovat tooachieve vyšší spokojenost zákazníků. Pomocí Media Indexer organizace rozbalte text a vytvářejte indexy vyhledávání a řídicí panely. Potom mohli extrahovat intelligence kolem častých stížností, zdroje stížností a další relevantní data.
* **Uživatelem generovaný obsah přerušování**. Z média zprávy výstupy toopolice oddělení má řada organizací veřejné portály, které přijímají uživatelem generovaný média, například videa a obrázků. z důvodu události toounexpected můžete špiček Hello množstvím obsahu. V těchto scénářích je obtížné tooconduct efektivní ruční recenze obsahu na základě vhodnosti. Zákazníci může se spoléhat na hello obsah přerušování služby toofocus na obsah, který je vhodný.
* **Sledováním**. S hello dodává růst v použití kamer IP rostoucí inventáře sledováním videa. Ruční kontrola sledováním video je čas náročné a náchylné k chybám toohuman chyba. Media Analytics poskytuje službám, jako je detekce pohybu, detekce vzhled a Hyperlapse toomake hello proces kontrola, Správa a vytváření odvozené konfigurace jednodušší.

## <a name="media-analytics-media-processors"></a>Procesory médií z Media Analytics
Tato část jak procesory médií z Media Analytics hello seznamy a ukazuje toouse .NET nebo REST tooget objekt média procesoru (PP).

### <a name="mp-names"></a>Názvy MP
* Azure Media Indexer 2 Preview
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR

### <a name="net"></a>.NET
Následující funkce trvá jeden Dobrý den Hello zadané MP názvy a vrátí objekt sady Management Pack.

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


### <a name="rest"></a>REST
Žádost:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Odpověď:

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

## <a name="demos"></a>Ukázky
V tématu [ukázek Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Související články
V tématu [Media Services Analytics oznámení](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
