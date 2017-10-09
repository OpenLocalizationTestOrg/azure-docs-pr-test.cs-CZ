---
title: "aaaMicrosoft Azure Media Services scénáře a dostupnost funkcí přes datových center | Microsoft Docs"
description: "V tomto tématu najdete přehled scénářů využití služby Microsoft Azure Media Services a dostupnost funkcí a služeb v datových centrech."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 3dbab6998ed5da738baf8f1e2fb096dfba336e19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a>Scénáře a dostupnost funkcí služby Media Services v datových centrech

Microsoft Azure Media Services (AMS) vám umožní toosecurely nahrávání, ukládání, kódovat a video nebo zvuk obsah balíčku pro vysílání na vyžádání i živé streamování doručení toovarious klienty (například TV, počítače a mobilní zařízení).

AMS funguje v několika datových centrech kolem hello, world. Těchto datových centrech jsou seskupené v oblastech toogeographic, která poskytuje flexibilitu při výběru místa toobuild vaší aplikace. Můžete zkontrolovat hello [seznam oblastí a jejich umístění](https://azure.microsoft.com/regions/). 

Toto téma představuje běžné scénáře doručování obsahu [živě](#live_scenarios) nebo [na vyžádání](#vod_scenarios). Hello téma obsahuje také podrobnosti o dostupnosti funkce média a služby napříč datovými centry.

## <a name="overview"></a>Přehled

### <a name="prerequisites"></a>Požadavky

toostart využívající Azure Media Services, měli byste mít hello následující:

* Účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).
* Účet Azure Media Services. Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).
* Hello koncový bod, ze kterého chcete obsah toostream streamování má toobe v hello **systémem** stavu.

    Při vytváření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello koncový bod streamování má toobe v hello **systémem** stavu.

### <a name="commonly-used-objects-when-developing-against-hello-ams-odata-model"></a>Běžně používané objekty při vývoji proti hello AMS OData modelu

Hello následující obrázek ukazuje některé objekty hello nejčastěji používaná při vývoji vůči modelu hello Media Services OData.

Klikněte na tlačítko tooview hello bitové kopie je plná velikost.  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

Můžete zobrazit celý modelu hello [zde](https://media.windows.net/API/$metadata?api-version=2.15).  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-hello-clear-non-encrypted"></a>Ochrana obsahu v úložišti a doručování streamovaných médií v hello clear (bez šifrování)

![Pracovní postup videa na vyžádání (VoD)](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. Nahrajte do prostředku vysoce kvalitní mediální soubor.

    Doporučujeme majetku, tooyour možnost šifrování úložiště tooapply v pořadí tooprotect svůj obsah během nahrávání a při umístěná v úložišti.
2. Zakódujte tooa sadu souborů MP4 s adaptivní přenosovou rychlostí.

    Doporučuje se, že toohello možnost šifrování úložiště tooapply výstupní asset v pořadí tooprotect váš obsah v úložišti.
3. Nakonfigurujte zásady doručení assetu (používané dynamickým balením).

    Pokud váš asset používá šifrování úložiště, **musíte** nakonfigurovat zásady doručení assetu.
4. Publikujte hello asset tak, že vytvoříte Lokátor OnDemand.
5. Streamujte publikovaný obsah.

Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Ochrana obsahu v úložišti, doručování dynamicky šifrovaných streamovaných médií

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. Nahrajte do prostředku vysoce kvalitní mediální soubor. Použijte asset toohello možnost šifrování úložiště.
2. Zakódujte tooa sadu souborů MP4 s adaptivní přenosovou rychlostí. Použijte úložiště šifrování možnost toohello výstupní asset.
3. Vytvořte šifrovací klíč obsahu pro prostředek hello chcete toobe během přehrávání dynamicky šifrovat.
4. Nakonfigurujte zásady autorizace klíče obsahu.
5. Nakonfigurujte zásady doručení assetu (používané dynamickým balením a dynamickým šifrováním).
6. Publikujte hello asset tak, že vytvoříte Lokátor OnDemand.
7. Streamujte publikovaný obsah.

Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.

## <a name="use-media-analytics-tooderive-actionable-insights-from-your-videos"></a>Použití Media Analytics tooderive prakticky uplatnitelných informací z videí

Media Analytics je kolekce řečových a vizuálních komponent, které usnadňují organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů. Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).

1. Nahrajte do prostředku vysoce kvalitní mediální soubor.
2. Zpracujte videa s jedním z služeb Media Analytics hello popsaných v hello [Media Analytics přehled](media-services-analytics-overview.md) části.
3. Procesory médií z Media Analytics vytvářejí soubory MP4 nebo soubory JSON. Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat hello souboru. Pokud procesor médií vytvořil soubor JSON, můžete stáhnout soubor hello z hello úložiště objektů blob Azure.

Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.

## <a name="deliver-progressive-download"></a>Doručení progresivního stahování

1. Nahrajte do prostředku vysoce kvalitní mediální soubor.
2. Zakódujte tooa jednoho souboru MP4.
3. Publikujte hello asset tak, že vytvoříte Lokátor OnDemand nebo SAS.

    Pokud používáte Lokátor SAS, hello obsah se stáhne ze hello úložiště objektů blob Azure. V takovém případě nepotřebujete toohave koncové body v Začínáme stavu streamování.
4. Progresivně stáhněte obsah.

## <a id="live_scenarios"></a>Doručování živě streamovaných událostí 

1. Ingestujte živý obsah pomocí různých protokolů pro živé streamování (například RTMP nebo technologie Smooth Streaming).
2. (volitelně) Kódujte datový proud na datový proud s adaptivní přenosovou rychlostí.
3. Zobrazte náhled živého streamování.
4. doručování obsahu hello prostřednictvím běžných streamovacích protokolů (například MPEG DASH, Smooth, HLS) přímo tooyour zákazníky nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci.

    -nebo-

    Záznam a úložiště hello požity obsah v pořadí toobe Streamovat později (video na vyžádání).

Při provádění živé vysílání datového proudu, je možné, že jedna z následujících hello tras:

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů (průchozí)

Hello následující diagram znázorňuje hello hlavní části platformy hello AMS, které se podílejí na hello **průchozí** pracovního postupu.

![Živý pracovní postup](./media/scenarios-and-availability/media-services-live-streaming-current.png)

Další informace najdete v článku o [práci s kanály, které přijímají živé streamování s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a>Práce s kanály, které jsou povolené tooperform za provozu, kódování službou Azure Media Services

Hello následující diagram znázorňuje hello hlavní části platformy AMS hello souvisejících s pracovním postupu živého streamování kde kanál je s povoleným tooperform za provozu, kódování pomocí služby Media Services.

![Živý pracovní postup](./media/scenarios-and-availability/media-services-live-streaming-new.png)

Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.

## <a name="consuming-content"></a>Konzumace obsahu

Azure Media Services poskytuje hello nástroje budete potřebovat toocreate bohaté aplikací pro dynamické klientské přehrávače pro většinu platforem včetně: zařízení iOS, zařízení se systémem Android, Windows, Windows Phone, Xbox a Set top polí. Hello následující téma obsahuje odkazy tooSDKs a architektury přehrávačů, které můžete použít toodevelop vlastních klientských aplikací, které můžou využívat streamovaná média ze služby Media Services. Další informace najdete v tématu [Vývoj aplikací pro přehrávání videa](media-services-develop-video-players.md).

## <a name="enabling-azure-cdn"></a>Povolení Azure CDN

Služba Media Services podporuje integraci s Azure CDN. Informace o tom najdete v části tooenable Azure CDN [jak tooManage koncových bodů streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).

## <a id="scaling"></a>Škálování účtu Media Services

Zákazníci AMS můžou ve svých účtech AMS škálovat koncové body streamování, zpracování médií a úložiště.

* Zákazníci Media Services si můžou zvolit koncový bod streamování **Standard**, nebo koncový bod streamování **Premium**. Koncový bod streamování **Standard** je vhodný pro většinu úloh streamování. Obsahuje hello stejné funkce jako **Premium** streamování koncových bodů a škálují šířky odchozího pásma automaticky. 

    Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma. Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU). koncový bod streamování Hello je možné rozšířit přidáním služby SUs. Každý SU poskytuje dodatečnou šířku pásma kapacity toohello aplikace. Další informace o škálování **Premium** koncové body streamování, najdete v části hello [škálování koncových bodů streamování](media-services-portal-scale-streaming-endpoints.md) tématu.

* Účet Media Services je přidružen vyhrazené jednotky typu, který určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy. Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**. Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu.

    Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision vašeho účtu pomocí **jednotek rezervovaných** (ruština). Hello počet zřízené RUs určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu.

    >[!NOTE]
    >RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru. Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.

    Další informace najdete v tématu [Škálování zpracování médií](media-services-portal-scale-media-processing.md).
* Váš účet Media Services můžete škálovat také přidáním tooit účty úložiště. Každý účet úložiště je omezená too500 TB. tooexpand úložiště nad rámec hello výchozí omezení, můžete zvolit tooattach více úložiště účtů tooa jednomu účtu Media Services. Další informace najdete v tématu [Správa účtů úložiště](meda-services-managing-multiple-storage-accounts.md).

##<a id="availability"></a>Dostupnost funkcí služby Media Services v datových centrech

V této části najdete podrobnosti o dostupnosti funkcí služby Media Services v datových centrech.

### <a name="ams-accounts"></a>Účty AMS

#### <a name="availability"></a>Dostupnost

Můžete vytvořit účty služby Media Services v hello následující oblasti: Severní Evropa, západní Evropa, západní USA, Východ USA, jihovýchodní Asie, východní Asie, Japonsko – Západ, Japonsko – východ, Brazílie – Jih, Indie – Západ, Indie – jih a Indie – střed. 

### <a name="streaming-endpoints"></a>Koncové body streamování 

Zákazníci Media Services si můžou zvolit koncový bod streamování **Standard**, nebo koncový bod streamování **Premium**. Další informace najdete v tématu hello [škálování](#scaling) části.

#### <a name="availability"></a>Dostupnost

|Name (Název)|Status|Datová centra
|---|---|---|
|Standard|GA|Všechny|
|Premium|GA|Všechny|

### <a name="live-encoding"></a>Kódování v reálném čase

#### <a name="availability"></a>Dostupnost

K dispozici ve všech datových centrech kromě oblastí: Německo, Brazílie – jih, Indie – západ, Indie – jih a Indie – střed. 

### <a name="encoding-media-processors"></a>Kódovací procesory médií

AMS nabízí dva kodéry na vyžádání – **Media Encoder Standard** a **Pracovní postup kodéru Media Encoder Premium**. Další informace najdete v tématu [Přehled a porovnání kodérů médií na vyžádání v Azure](media-services-encode-asset.md). 

#### <a name="availability"></a>Dostupnost

|Název procesoru médií|Status|Datová centra
|---|---|---|
|Media Encoder Standard|GA|Všechny|
|Pracovní postup kodéru Media Encoder Premium|GA|Všechny s výjimkou Číny|

### <a name="analytics-media-processors"></a>Analytické procesory médií

Media Analytics je kolekce řečových a vizuálních komponent, které usnadňuje organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů. Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).

#### <a name="availability"></a>Dostupnost

|Název procesoru médií|Status|Datová centra
|---|---|---|
|Azure Media Face Detector|Preview|Všechny|
|Azure Media Hyperlapse|Preview|Všechny|
|Azure Media Indexer|GA|Všechny|
|Azure Media Motion Detector|Preview|Všechny|
|Azure Media OCR|Preview|Všechny|
|Azure Media Redactor|Preview|Všechny|
|Azure Media Stabilizer|Preview|Všechny|
|Azure Media Video Thumbnails|Preview|Všechny|
|Azure Media Indexer 2|Preview|Všechny s výjimkou Číny a oblasti federální vlády|

### <a name="protection"></a>Ochrana

Microsoft Azure Media Services umožňuje vám toosecure médiu z hello doby, kdy opouští váš počítač prostřednictvím úložiště, zpracování a doručení. Další informace najdete v tématu [Ochrana obsahu AMS](media-services-content-protection-overview.md).

#### <a name="availability"></a>Dostupnost

|Šifrování|Status|Datová centra|
|---|---|---| 
|Úložiště|GA|Všechny|
|Klíče AES-128|GA|Všechny|
|FairPlay|GA|Všechny|
|PlayReady|GA|Všechny|
|Widevine|GA|Všechna kromě oblastí Německo, Federální vláda a Čína.

### <a name="reserved-units-rus"></a>Rezervované jednotky (RU)

Hello počet jednotek rezervovaných pro zřízené určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu. 

Další informace najdete v tématu hello [škálování](#scaling) části.

#### <a name="availability"></a>Dostupnost

K dispozici ve všech datových centrech.

### <a name="reserved-unit-ru-type"></a>Typ rezervované jednotky (RU)

Účet Media Services je přidružený k typu jednotku rezervovanou, která určuje, ke které se zpracovávají médiu zpracování úlohy rychlost hello. Můžete si vybrat mezi hello následující vyhrazené typy jednotka: S1, S2 nebo S3.

Další informace najdete v tématu hello [škálování](#scaling) části.

#### <a name="availability"></a>Dostupnost

|Název typu RU|Status|Datová centra
|---|---|---|
|S1|GA|Všechny|
|S2|GA|Všechna kromě oblastí Brazílie – jih a Indie – západ|
|S3|GA|Všechna kromě oblasti Indie – západ|

## <a name="next-steps"></a>Další kroky

Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

