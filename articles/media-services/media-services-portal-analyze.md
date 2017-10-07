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
# <a name="analyze-your-media-using-hello-azure-portal"></a>Analýza médiu pomocí hello portálu Azure
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

## <a name="overview"></a>Přehled
Azure Media Services Analytics je kolekce řečových a vizuálních komponent (na škálování enterprise, dodržování předpisů, zabezpečení a globální reach), které usnadňují organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů. Podrobnější přehled Azure Media Services Analytics najdete v článku [to](media-services-analytics-overview.md) tématu. 

Toto téma popisuje, jak tooprocess hello médií s procesory médií Media Analytics (sad Management Pack) pomocí portálu Azure. MP Media Analytics vytvářejí soubory MP4 nebo soubory JSON. Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat hello souboru. Pokud procesor médií vytvořil soubor JSON, můžete stáhnout soubor hello z hello úložiště objektů blob Azure. 

## <a name="choose-an-asset-that-you-want-tooanalyze"></a>Vyberte prostředek, které chcete tooanalyze
1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. V hello **nastavení** vyberte **prostředky**.  
   .
    ![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Vyberte hello asset, který chcete tooanalyze a stiskněte klávesu hello **analyzovat** tlačítko.
   
    ![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. V hello **proces média asset Media Analytics** okno, vyberte hello procesoru. 
   
    Hello zbytek hello článek vysvětluje, proč a jak toouse každý procesor. 
5. Stiskněte klávesu **vytvořit** toohello spustit úlohu.

## <a name="azure-media-indexer"></a>Azure Media Indexer
Hello **Azure Media Indexer** procesor médií vám umožní toomake mediálních souborů a obsah s možností vyhledávání, a také generování uzavřené titulků sleduje. Tato část obsahuje některé údaje o možnostech, které můžete zadat pro tento bod.

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Jazyk
toobe přirozeného jazyka Hello rozpoznán v hello multimediálních souborů. Například angličtina nebo španělštině. 

### <a name="captions"></a>Titulky
Můžete formátu popisek, který bude vydána z vašeho obsahu. Indexování úlohy mohou vytvářet soubory titulků v hello následujících formátů:  

* **SAMI**
* **TTML**
* **WebVTT**

Uzavřené popisek (kopie) soubory v těchto formátů lze použít toomake zvuk a video soubory přístupné toopeople s postižení sluchu.

### <a name="aib-file"></a>Soubor AIB
Tuto možnost vyberte, pokud jste by jako soubor Blob Index zvuk hello toogenerate pro použití s hello vlastní IFilter serveru SQL. Další informace najdete v tématu [to](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.

### <a name="keywords"></a>Klíčová slova
Tuto možnost vyberte, pokud chcete toogenerate soubor XML klíčová slova. Tento soubor obsahuje klíčová slova extrahuje z obsahu hello rozpoznávání řeči, četnost a informace o posunu.

### <a name="job-name"></a>Název úlohy
Popisný název, který umožňuje identifikovat hello úlohy. [To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy. 

### <a name="output-file"></a>Výstupní soubor
Popisný název, který umožňuje identifikovat obsah výstup hello. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse
Azure Media Hyperlapse je na sadu Management Pack vytvoří smooth vypršelo čas videa z první, kdo nebo akce fotoaparát obsah.  Další informace najdete v [tomto](media-services-hyperlapse-content.md) tématu. Tato část obsahuje některé údaje o možnostech, které můžete zadat pro tento bod.

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Rychlost
Určete rychlost hello s které toospeed až vstupní video hello. výstup Hello je stabilizované a čas vypršelo interpretace hello vstupní videa.

### <a name="job-name"></a>Název úlohy
Popisný název, který umožňuje identifikovat hello úlohy. [To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy. 

### <a name="output-file"></a>Výstupní soubor
Popisný název, který umožňuje identifikovat obsah výstup hello. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
Hello **Azure Media vzhled detektor** procesor médií (PP) vám umožní toocount, sledovat pohybů a i zapojení cílové skupiny měřidla a reakce prostřednictvím obličeje výrazy. Tato služba obsahuje dvě funkce: 

* **Vzhled detekce**
  
    Vzhled zjišťování vyhledá a sleduje lidského tyto řezy v rámci video. Více řezy lze zjistit a následně je sledovat jako pohyb se s hello čas a umístění metadata vrácená v souboru JSON. Při sledování pokusí toogive konzistentní toohello ID stejné čelí, zatímco je osoba hello pohyb na obrazovce, přestože bráněno nebo stručně nechte hello rámce.
  
  > [!NOTE]
  > Této služby nebude provádět rozpoznávání obličeje. Osoba, která zůstane hello rámce nebo se stane nelze blokovat. pro příliš dlouho přidělí nové ID když se vrátí.
  > 
  > 
* **Emocí**
  
    Emocí je volitelná součást hello vzhled detekce média procesor, který vrátí analýzy na více duševní atributů z hello řezy detekuje, včetně štěstí, sadness, obavy, anger a další. 

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Detekce režimu
Jeden z následujících režimů hello dá procesorem hello:

* Vzhled detekce
* každý řez emocí
* agregační emocí

### <a name="job-name"></a>Název úlohy
Popisný název, který umožňuje identifikovat hello úlohy. [To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy. 

### <a name="output-file"></a>Výstupní soubor
Popisný název, který umožňuje identifikovat obsah výstup hello. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
Hello **detektor pohybu médií Azure** média procesoru (PP) umožňuje tooefficiently můžete identifikovat části týkající se v rámci souboru jinak dlouhé a bezproblémové video. Detekce pohybu lze použít na statické fotoaparát záznamů tooidentify části hello videa kde dojde k pohybu. Vygeneruje soubor JSON obsahující metadata s časová razítka a hello ohraničujícího oblasti, kde došlo k události hello.

Cílem směrem zabezpečení video informační kanály, tato technologie je možné toocategorize pohybu do příslušné události a falešně pozitivních například stínů a osvětlení změny. To umožňuje toogenerate výstrahy zabezpečení z fotoaparátu kanály bez nevyžádané pošty s nekonečná důležité události, aniž by byly situacích možné tooextract zájmu z extrémně dlouhé sledováním videa.

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Tato procesoru vám pomůžou vytvořit souhrnných informací o dlouho videa automaticky výběrem zajímavé fragmenty kódu z hello zdroj videa. To je užitečné, když chcete, aby tooprovide rychlý přehled toho, jaké tooexpect v dlouho videa. Podrobné informace a příklady naleznete v tématu [použití miniatur videa v aplikaci Azure Media tooCreate Videosouhrnu](media-services-video-summarization.md)

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Název úlohy
Popisný název, který umožňuje identifikovat hello úlohy. [To](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh hello úlohy. 

### <a name="output-file"></a>Výstupní soubor
Popisný název, který umožňuje identifikovat obsah výstup hello. 

## <a name="next-steps"></a>Další kroky
Zobrazení Media Services kurzů.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

