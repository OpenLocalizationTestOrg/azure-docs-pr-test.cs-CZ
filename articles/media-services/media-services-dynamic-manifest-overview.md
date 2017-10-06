---
title: "aaaFilters a dynamické manifesty | Microsoft Docs"
description: "Toto téma popisuje, jak filtry toocreate abyste vašeho klienta mohli používat určité části toostream datového proudu. Služba Media Services vytvoří dynamické manifesty tooarchive selektivní streamování."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 9527a011438c11da07a363d701ea736414412ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filters-and-dynamic-manifests"></a>Filtry a dynamické manifestů
Od verze 2.11, Media Services umožňuje toodefine filtry pro vaše prostředky. Tyto filtry jsou pravidla na straně serveru, které vám umožní vašim zákazníkům toochoose toodo věci jako: přehrávání pouze část videa (namísto přehrávání hello celý video), nebo zadejte pouze podmnožinu interpretace audia a videa, vašeho zákazníka zařízení dokáže zpracovat ( Ne všechny interpretace hello které jsou přidruženy hello asset). Tento filtrování vaše prostředky bude archivován prostřednictvím **dynamické Manifest**ů, které jsou vytvořené při vašeho zákazníka žádost toostream video podle zadané filtry.

Tato témata popisuje běžné scénáře, ve kterém pomocí filtrů by tootopics velmi užitečné tooyour zákazníků a odkazy, které ukazují, jak filtry toocreate prostřednictvím kódu programu (v současné době můžete vytvořit filtry pomocí rozhraní REST API pouze).

## <a name="overview"></a>Přehled
Při doručování vašeho obsahu toocustomers (streamování živé události nebo vyžádání, video-on-demand) vaším cílem je toodeliver vysoce kvalitního videa toovarious zařízení v různých síťových podmínkách. tooachieve tento cíl hello následující:

* zakódovat váš datový proud přenosovými toomulti ([s adaptivní přenosovou rychlostí](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) datový proud videa (to se postará o kvalitu i síťové podmínky) a 
* používání Media Services [dynamické balení](media-services-dynamic-packaging-overview.md) toodynamically znovu zabalte datový proud do jiné protokoly (to se postará o streamování na různá zařízení). Služba Media Services podporuje doručování následujících adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming a MPEG DASH. 

### <a name="manifest-files"></a>Soubory manifestu
Při kódování prostředek pro streamování s adaptivní přenosovou rychlostí **manifest** se vytvoří soubor (STOP) (soubor hello je založený na textu nebo na základě XML). Hello **manifest** soubor obsahuje metadata, jako streamování: sledování typu (zvuk, video nebo text), sledovat název, počáteční a koncový čas, přenosovou rychlostí (Vlastnosti), sledovat jazyky, prezentace okno (posuvné okno Pevná doba trvání), video kodeků (FourCC). Je také dá pokyn hello player tooretrieve hello další fragment tím, že poskytuje informace o hello další možné přehrát video fragmenty dostupné a jejich umístění. Fragmenty (nebo segmenty) jsou hello skutečné "skupiny" video obsahu.

Tady je příklad souboru manifestu: 

    <?xml version="1.0" encoding="UTF-8"?>    
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">

    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />

    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>

    </SmoothStreamingMedia>

### <a name="dynamic-manifests"></a>Dynamické manifestů
Existují [scénáře](media-services-dynamic-manifest-overview.md#scenarios) když váš klient potřebuje více flexibility než co je popsaný v souboru manifestu hello výchozí asset. Například:

* Zařízení, konkrétní: poskytovat hello zadána interpretace nebo zadat pouze jazyk sleduje, kterými hello zařízení, které je použité tooplayback hello ("interpretace filtrování obsahu"). 
* Snižte hello manifestu tooshow dílčí klip živé události ("dílčí klip filtrování").
* Trim hello začátek video ("ořezávání video").
* Úprava prezentace okno (DVR) v pořadí tooprovide omezenou délku hello formátu DVR okno v player hello ("okno s úpravou prezentace").

tooachieve tuto flexibilitu, Media Services nabízí **dynamické manifesty** založené na předem definované [filtry](media-services-dynamic-manifest-overview.md#filters).  Jakmile definujete hello filtry, vaši klienti může je používat toostream konkrétní interpretace nebo dílčí klipů videa. Filtry se určují v hello adresu URL streamování. Filtry může být použité tooadaptive přenosové rychlosti streamování protokolů podporovaných [dynamické balení](media-services-dynamic-packaging-overview.md): HLS, MPEG DASH a technologie Smooth Streaming. Například:

Adresa URL MPEG DASH s filtrem

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Technologie Smooth Streaming adresu URL pomocí filtru

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Další informace o tom, toodeliver obsah a sestavení streamování adresy URL, najdete v části [doručování obsahu přehled](media-services-deliver-content-overview.md).

> [!NOTE]
> Všimněte si, že dynamické manifesty neměňte hello asset a hello výchozí manifest pro tento prostředek. Váš klient můžete zvolit toorequest datového proudu s nebo bez filtry. 
> 
> 

### <a id="filters"></a>Filtry
Existují dva typy filtrů asset: 

* Globální filtry (můžou být použité tooany prostředek hello účtu Azure Media Services, mají životnost hello účet) a 
* Místní filtry (může být pouze s které hello filtr byl po vytvoření přidružený asset použité tooan, mají životnost hello asset). 

Globální a místní filtrování typů mít přesně hello stejné vlastnosti. Hello hlavní rozdíl mezi hello dva je, jaký typ souborového pro scénáře, které je vhodnější. Globální filtry jsou obecně vhodné pro profily zařízení (interpretace filtrování), kde může být místní filtry použité tootrim konkrétní asset.

## <a id="scenarios"></a>Běžné scénáře
Jak již bylo zmíněno, než po doručení vašeho obsahu toocustomers (streamování živé události nebo vyžádání, video-on-demand) vaším cílem je toodeliver vysoce kvalitního videa sítě toovarious zařízení v rámci jiné podmínky. Kromě toho vaší může mít jiné požadavky, které zahrnují filtrování vaše prostředky a použití **dynamické Manifest**s. Hello následující oddíly poskytují stručný přehled různých scénářů filtrování.

* Zadejte jenom podmnožinu interpretace audia a videa, které může zpracovat určitých zařízení (ne všechny interpretace hello, které jsou přidruženy hello asset). 
* Přehrávání pouze část videa (namísto přehrávání hello celý video).
* Upravte formátu DVR prezentace okno.

## <a name="rendition-filtering"></a>Interpretace filtrování
Můžete se rozhodnout tooencode váš asset toomultiple kódování profilů (H.264 směrného plánu, H.264 vysoké, AACL, AACH, Dolby Digital Plus) a více přenosových rychlostí kvality. Ne všechna zařízení klienta bude však podporovat všechny assetu profily a přenosových rychlostí. Například starší zařízení se systémem Android podporuje pouze H.264 směrného plánu + AACL. Odesílání vyšší přenosových rychlostí tooa zařízení, které nelze získat výhody hello, zbytečně plýtvá výpočet šířky pásma a zařízení. Taková zařízení musí dekódovat všechny hello dané informace, jenom tooscale ho dolů pro zobrazení.

Pomocí dynamických Manifest můžete vytvářet profily zařízení například mobile, konzoly, HD/SD atd. a zahrnují hello sleduje a vlastnosti, které chcete toobe součástí každý profil.

![Příklad filtrování interpretace][renditions2]

V následující ukázka hello byla kodér použité tooencode asset mezzanine do sedmi interpretace video soubory MP4 s rychlostmi ISO (z too1080p 180p). Hello zakódovanému assetu můžete dynamicky zabalené do některé z následujících protokolů datových proudů hello: HLS, Smooth a MPEG DASH.  V horní části hello hello diagramu, se zobrazí hello HLS manifest pro prostředek hello s žádné filtry (obsahuje všechny sedm interpretace).  V levém dolním hello se zobrazí hello HLS manifest toowhich, který byl použit filtr s názvem "ott". Filtr "ott" Hello určuje tooremove všechny přenosových rychlostí nižší než 1 MB/s, čímž vznikl hello dolní dvě úrovně kvality se odstraní v odpovědi hello.  V pravé, dolní hello hello HLS se zobrazí toowhich manifestu, který byl použit filtr s názvem "mobilní". Filtr "mobilní" Hello určuje tooremove interpretace kde hello řešení je větší než 720p, čímž vznikl hello dvě 1080p interpretace se odstraní a.

![Interpretace filtrování][renditions1]

## <a name="removing-language-tracks"></a>Odebírání sleduje jazyk
Vaše prostředky může obsahovat více zvuk jazyků, jako je angličtina, španělština, francouzština, atd. Obvykle hello Player SDK správci výchozí výběr zvukové stopy a k dispozici zvuk sleduje za výběr uživatele. Je náročné toodevelop takové sady SDK Player, vyžaduje různými implementacemi napříč architektury přehrávačů konkrétní zařízení. Na některých platformách také Player rozhraní API jsou omezené a nezahrnují funkce audia výběr kde nelze uživatelů vyberte nebo změňte hello výchozí zvuk sledovat. S filtry asset můžete řídit chování hello vytvořením filtry, které pouze zahrnout požadované zvuk jazyky.

![Sleduje jazyka filtrování][language_filter]

## <a name="trimming-start-of-an-asset"></a>Oříznutí začátek prostředek
Ve většině živě streamovaných událostí operátoři spustit některé testy před hello skutečné události. Například může zahrnovat projektem takto před hello začátku události hello: "Programu zbývá na okamžik". Pokud je hello program archivace, testovací hello a projektem data jsou také archivovány a budou zahrnuty do prezentace hello. Tyto informace však by neměla zobrazit toohello klientů. S dynamické manifestu můžete vytvořit filtr času zahájení a odebrat hello nežádoucí data z manifestu hello.

![Oříznutí start][trim_filter]

## <a name="creating-sub-clips-views-from-a-live-archive"></a>Vytváření dílčí klipů (zobrazení) z archivu za provozu
Mnoho živé události jsou dlouho spuštěný a za provozu archivu může obsahovat více událostí. Po televizního končí hello živé události může být vhodné toobreak až hello live archivu do logické program spustit a zastavit pořadí. Potom publikujte samostatně tyto virtuální programy bez post zpracování archivu živé hello a nevytváří samostatné prostředky (které nebude získat výhody hello existující uložené v mezipaměti fragmenty v hello sítím CDN). Mezi tyto virtuální programy (dílčí klipy) jsou hello čtvrtletí fotbalové nebo Basketbalový hra, hello innings v baseballové nebo jednotlivé události odpoledne olympiáda programu.

S dynamické Manifest můžete vytvářet filtry, pomocí počáteční/koncový čas a vytvořit virtuální zobrazení nad horní hello vaší živé archivu. 

![Subclip filtru][subclip_filter]

Filtrované Asset:

![Lyžování][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Nastavení prezentace okno (DVR)
V současné době Azure Media Services nabízí cyklické archivu, kde můžete hello trvání nakonfigurované mezi 5 minut – 25 hodin. Manifestu filtrování můžou být použité toocreate okno postupného formátu DVR over hello horní části hello archivu bez odstranění média. Existuje mnoho scénářů, kde chcete televizního tooprovide okno omezené formátu DVR, který za provozu okraj a na hello současně zachovat větší archivační okno se přesune s hello. Zdroje všesměrového vysílání může být vhodné toouse hello data, která je mimo hello formátu DVR okno toohighlight klipů nebo he\she může být vhodné tooprovide různých formátu DVR windows pro různá zařízení. Například většina hello mobilních zařízení není zpracovávat velký formátu DVR windows (může mít 2 minuty formátu DVR okna pro mobilní zařízení a 1 hodina klientů plochy).

![Okno formátu DVR][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Úprava LiveBackoff (živé pozice)
Manifestu filtrování lze použít tooremove několik sekund od hello za provozu okraje za chodu programu. To umožňuje televizního toowatch hello prezentace v publikaci preview hello bodu a vytvořit oznámení o inzerovaném programu body vložení před hello prohlížeče zobrazí datový proud hello (obvykle zálohovaný vypnuto 30 sekund). Televizního pak vkládat tyto oznámení o inzerovaném programu tootheir klienta architektury v čase pro ně tooreceived a proces hello informace před možnost oznámení o inzerovaném programu hello.

Kromě toho podporují toohello oznámení o inzerovaném programu, LiveBackoff lze použít pro úpravu pozice za provozu stažení klienta tak, že když se klienti soubor a stiskněte tlačítko za provozu edge hello mohou stále získat fragmenty ze serveru, místo dochází k chybám protokolu HTTP 404 nebo 412.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Kombinování více pravidel v jednom filtru
Můžete kombinovat více pravidel filtrování v jednom filtru. Jako příklad můžete definovat rozsah pravidla tooremove projektem z archivu za provozu a také filtrovat dostupné přenosových rychlostí. Pro více element end hello pravidla filtrování výsledků je hello sestavení (pouze průnik) těchto pravidel.

![více pravidel][multiple-rules]

## <a name="create-filters-programmatically"></a>Vytvoření filtrů prostřednictvím kódu programu
Hello následující téma popisuje Media Services entit, které jsou související toofilters. Hello téma také ukazuje, jak vytvořit tooprogrammatically filtry.  

[Vytvoření filtrů rozhraní REST API](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombinace více filtrů (složení filtr)
Můžete také kombinovat více filtrů v jednu adresu URL. 

Hello následující scénář předvádí, proč byste měli toocombine filtry:

1. Je nutné toofilter vaše video vlastnosti pro mobilní zařízení, třeba na Android nebo iPAD (v pořadí toolimit video vlastnosti). tooremove hello nežádoucí kvality, vytvořili byste globální filtr, který je vhodný pro profily zařízení. Jak je uvedeno nahoře, globální filtry lze použít pro všechny prostředky v rámci hello účet bez jakékoli další přidružení služeb stejná média. 
2. Také chcete tootrim hello počáteční a koncový čas prostředek. tooachieve, by vytvořit místní filtr a nastavit čas zahájení a ukončení hello. 
3. Chcete-li toocombine obě tyto filtry (bez kombinace potřebovali byste tooadd kvality filtrování toohello oříznutí filtr to bude obtížné filtru využití).

filtry toocombine, je nutné tooset hello filtru názvy toohello manifest/stop adresu URL s oddělený středníky. Předpokládejme, máte filtr s názvem *MyMobileDevice* , filtry vlastností a druhou s názvem máte *MyStartTime* tooset konkrétní počáteční čas. Můžete je spojit takto:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Můžete kombinovat až too3 filtry. 

Další informace najdete v části [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a name="know-issues-and-limitations"></a>Vědět, problémy a omezení
* Dynamické manifest funguje v GOP hranice (klíč rámce) proto ořezávání má GOP přesnost. 
* Můžete použít stejný název filtru pro místní a globální filtry. Všimněte si, že místní filtr mají vyšší prioritu a přepíše globálních filtrů.
* Pokud aktualizujete filtr, může trvat až minut too2 pravidla hello toorefresh koncový bod streamování. Pokud obsah hello zpracování pomocí některé filtry (a uložené v mezipaměti v proxy servery a CDN mezipaměti), aktualizace tyto filtry může způsobit selhání přehrávač. Je vhodné tooclear hello mezipaměti po aktualizaci hello filtru. Pokud tato možnost není možné, zvažte použití jiný filtr.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Doručování obsahu tooCustomers – přehled](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
