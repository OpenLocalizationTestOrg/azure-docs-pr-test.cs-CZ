---
title: "aaaOverview živé streamování využívající Azure Media Services | Microsoft Docs"
description: "Toto téma poskytuje přehled živé streamování využívající Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: edc49069db6b491902bdcbb808b1974858cc92f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-live-streaming-using-azure-media-services"></a>Přehled živé streamování využívající Azure Media Services
## <a name="overview"></a>Přehled
Při doručování živé vysílání datového proudu událostí pomocí služby Azure Media Services hello následující součásti jsou běžně podílejí:

* Fotoaparát, který je použité toobroadcast událost.
* Za provozu kodér videa, který převádí signály z toostreams hello fotoaparát, která se posílají tooa živé streamování služby.

    Volitelně několik živých, časově synchronizovaných kodérů. V případě důležitých živých událostí, vyžadují velmi vysokou dostupnost a kvalitní uživatelský dojem, doporučujeme tooemploy aktivní aktivní redundantní kodéry s čas synchronizace tooachieve bezproblémové převzetí služeb při selhání bez ztráty dat..
* Služba živého streamování umožňující toodo hello následující:

  * ingestovat živý obsah pomocí různých protokolů pro živé streamování (například RTMP nebo technologie Smooth Streaming),
  * (volitelně) kódovat datový proud na datový proud s adaptivní přenosovou rychlostí
  * zobrazit náhled živého datového proudu,
  * záznam a úložiště hello požity obsah v pořadí toobe Streamovat později (video na vyžádání)
  * doručování obsahu hello prostřednictvím běžných streamovacích protokolů (například MPEG DASH, Smooth, HLS) přímo tooyour zákazníky nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci.

**Microsoft Azure Media Services** (AMS) nabízí hello možnost tooingest, kódovat, zobrazte náhled, ukládat a doručovat obsah vašeho živého streamování.

Při doručování vašeho obsahu toocustomers vaším cílem je toodeliver vysoce kvalitního videa toovarious zařízení v různých síťových podmínkách. tooachieve, použijte live kodéry tooencode datového proudu tooa více přenosovými rychlostmi (adaptivní přenosová rychlost) video datového proudu.  tootake péče o streamování na různá zařízení pomocí služby Media Services [dynamické balení](media-services-dynamic-packaging-overview.md) toodynamically znovu zabalit vaše protokoly toodifferent datového proudu. Služba Media Services podporuje doručování následujících adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.

Ve službě Azure Media Services **kanály**, **programy**, a **koncové body streamování** popisovač všechny hello živé streamování včetně ingestování, formátování, DVR, funkce zabezpečení, škálovatelnosti a redundance.

**Kanál** představuje cestu pro zpracování obsahu živého streamování. Kanál může přijímat živé vstupní datové proudy v hello následující způsoby:

* Místní kodér za provozu odešle více přenosovými rychlostmi **RTMP** nebo **technologie Smooth Streaming** (fragmentovaný soubor MP4) toohello kanálu, který je nakonfigurován pro **průchozí** doručení. Hello **průchozí** doručování nastává, když hello požity datové proudy prochází **kanál**s bez dalšího zpracování. Můžete použít následující kodéry, které výstupu technologie Smooth Streaming více přenosovými rychlostmi hello: MediaExcel, Ateme, představte si komunikace, Envivio, Cisco a Elemental. Hello následující kodéry výstupu RTMP: Adobe Flash média Live Encoder (FMLE), Telestream Wirecast, Haivision, Teradek a čase transkodéry.  Za provozu encoder můžete také odeslat kanál tooa jednou přenosovou rychlostí datový proud, který nemá povolené kódování v reálném čase, ale tato konfigurace se nedoporučuje. Po vyžádání Media Services doručí datový proud toocustomers hello.

  > [!NOTE]
  > Použití průchozí metody je nejekonomičtější způsob hello toodo živě Streamovat při pořádání několika událostí po dlouhou dobu a jste už investovali do místních kodérů. Viz podrobnosti o [cenách](https://azure.microsoft.com/pricing/details/media-services/).
  > 
  > 
* Místní kodér za provozu odešle datový proud s jednou přenosovou rychlostí toohello kanálu, který je povolený tooperform live kódování pomocí služby Media Services v jednom z následujících formátů hello: RTMP nebo technologie Smooth Streaming (fragmentovaný MP4). RTP (MPEG-TS) je také podporována, pokud máte vyhrazené připojení toohello Azure datového centra. výstup Hello následující kodéry s RTMP se ví, že toowork s kanály, tohoto typu: Telestream Wirecast, FMLE. Hello kanál potom provede kódování v reálném čase z hello příchozí jednou přenosovou rychlostí datového proudu tooa více přenosovými rychlostmi (adaptivní) datový proud videa. Po vyžádání Media Services doručí datový proud toocustomers hello.

Od verze hello Media Services 2.10, když vytvoříte kanál, můžete určit, jakým způsobem chcete použít pro kanál tooreceive hello vstupního datového proudu a zda chcete použít pro kanál tooperform hello za provozu kódování datového proudu. Máte dvě možnosti:

* **Žádný** (průchozí) – zadejte tuto hodnotu, pokud máte v plánu toouse za provozu kodér místní, který bude výstupního datového proudu více přenosovými rychlostmi (průchozí datový proud). V takovém případě příchozího datového proudu hello předána toohello výstup bez jakékoli kódování. Toto je chování hello kanál too2.10 předchozí verze.  
* **Standardní** – vyberte tuto hodnotu, pokud máte v plánu toouse Media Services tooencode datového proudu toomulti přenosovými živý datový proud jednou přenosovou rychlostí. Tato metoda je vyplatí pro Rychlé škálování pro nepravidelným události. Mějte na paměti, že je fakturace dopad pro kódování v reálném čase a měli pamatovat ponechat živého kanálu kódování ve stavu "Spuštění" hello je zpoplatněná fakturace poplatků.  Doporučuje se okamžitě zastavit spuštěný kanálů po živé streamování události je kompletní tooavoid velmi hodinové poplatky.

## <a name="comparison-of-channel-types"></a>Porovnání typů kanálu
Následující tabulka poskytuje návod toocomparing hello dvě kanál typy podporované ve službě Media Services

| Funkce | Průchozí kanál | Běžný kanál |
| --- | --- | --- |
| Jednou přenosovou rychlostí vstup je zakódován do více přenosových rychlostí v cloudu hello |Ne |Ano |
| Maximální rozlišení, počet vrstev |1080p, 8 vrstvy 60 + snímků za sekundu |720p, 6 vrstev, 30 snímků za sekundu |
| Vstupní protokoly |RTMP funkce Smooth Streaming |RTMP, technologie Smooth Streaming a protokol RTP |
| Cena |V tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/media-services/) a klikněte na kartu "Live Video" |V tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximální doba běhu |24 x 7 |8 hodin |
| Podpora pro vkládání slaty |Ne |Ano |
| Podpora pro signalizace ad |Ne |Ano |
| Průchozí CEA 608/708 titulky |Ano |Ano |
| Možnost toorecover z stručný míst v příspěvku informačního kanálu |Ano |Ne (kanál zahájíte slating po 6 + sekundách bez vstupní data) |
| Podpora pro vstupní GOPs neuniformní |Ano |Ne – vstup musí být vyřešeny 2 sekundu GOPs |
| Podpora pro vstup míra proměnné rámce |Ano |Ne – vstup je potřeba opravit obnovovací frekvence.<br/>Malé změny jsou například tolerovat během scény vysoké pohybu. Ale kodér nelze vyřadit too10 snímků za sekundu. |
| Auto přístupnými kanálů při vstupu kanálu dojde ke ztrátě |Ne |Po 12 hodinách, pokud neexistuje žádné Program spuštěn |

## <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů (průchozí)
Hello následující diagram znázorňuje hello hlavní části platformy hello AMS, které se podílejí na hello **průchozí** pracovního postupu.

![Živý pracovní postup](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Další informace najdete v článku o [práci s kanály, které přijímají živé streamování s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md).

## <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a>Práce s kanály, které jsou povolené tooperform za provozu, kódování službou Azure Media Services
Hello následující diagram znázorňuje hello hlavní části platformy AMS hello souvisejících s pracovním postupu živého streamování kde kanál je s povoleným tooperform za provozu, kódování pomocí služby Media Services.

![Živý pracovní postup](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

## <a name="description-of-a-channel-and-its-related-components"></a>Popis kanálu a jeho souvisejících součástí
### <a name="channel"></a>Kanál
Ve službě Media Services [kanál](https://docs.microsoft.com/rest/api/media/operations/channel)s jsou zodpovědná za zpracování obsahu živého streamování. Kanál, který obsahuje vstupní koncový bod (ingestovanou adresu URL), pak zadejte převaděč tooa za provozu. kanál Hello přijímá živé vstupní datové proudy ze za provozu převaděč hello a zpřístupní se k vysílání datového proudu prostřednictvím jeden nebo více koncové body streamování. Kanály také poskytují preview koncový bod (URL náhledu) použít toopreview a ověřit před další zpracování a doručení datového proudu.

Hello můžete získat ingestované adresy URL a adresy URL preview hello při vytváření kanálu hello. tooget tyto adresy URL hello kanál nemá toobe ve stavu spuštění hello. Pokud jste připravené toostart nabízet data z provozu převaděč do kanálu hello, musí být spuštěná hello kanálu. Jakmile se převaděč hello za provozu spustí příjem dat, můžete zobrazit náhled datového proudu.

Každý účet Media Services může obsahovat více typů komunikačních kanálů, více programů a více koncové body streamování. V závislosti na potřebách hello šířky pásma a zabezpečení může být StreamingEndpoint služby vyhrazené tooone nebo další kanály. Všechny StreamingEndpoint můžete načítat z jakékoli kanálu.

### <a name="program"></a>Program
A [Program](https://docs.microsoft.com/rest/api/media/operations/program) vám umožní toocontrol hello publikování a ukládání segmentů v živém datovém proudu. Kanály spravují programy. Hello vztah kanálů a programů se velmi podobné tootraditional médií, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezená toosome vypršel časový limit událostí v tomto kanálu.
Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain programu hello podle nastavení hello **ArchiveWindowLength** vlastnost. Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin.

ArchiveWindowLength také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello. Programy můžou běžet po hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.

Každý program je přidružený k assetu. program hello toopublish musí vytvořit lokátor pro hello související prostředek. Tento Lokátor vám umožní toobuild adresu URL pro streamování, kterou potom poskytnete tooyour klientů.

Kanál podporuje až toothree souběžně spuštěnými programy, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu. To vám umožní toopublish a archivovat různé části události podle potřeby. Například vaše firemní požadavky je tooarchive 6 hodin programu, ale toobroadcast pouze posledních 10 minut. tooaccomplish, je nutné toocreate dva současně spuštěné programy. Jeden program nastaven tooarchive 6 hodin hello události ale programu hello není publikována. Hello jiný program je sada tooarchive 10 minut a tento program budete publikovat.

## <a name="billing-implications"></a>Dopady na fakturaci
Kanál, který začíná fakturace při jeho stav přejde příliš "spuštění" prostřednictvím hello rozhraní API.  

Hello následující tabulka ukazuje, jak kanál stavy mapovány toobilling stavů v hello rozhraní API a portálu Azure. Upozorňujeme, že jsou mírně liší hello rozhraní API a portál UX hello stavy Jakmile kanál hello "Spuštěná" stav prostřednictvím hello rozhraní API, nebo v hello "Připravené" nebo "Streaming" stav v hello portálu Azure, bude fakturace aktivní.

Kanál hello toostop z jste další fakturace, máte tooStop hello kanál prostřednictvím hello rozhraní API nebo v hello portálu Azure.
Jste zodpovědní za zastavení kanálů až skončíte s hello kanál. Trvalá fakturace způsobí selhání toostop hello kanálu.

> [!NOTE]
> Při práci s kanály, Standard, AMS automaticky přístupnými žádné kanálu, který je stále ve stavu "Spuštění" 12 hodin po vstupní kanál hello dojde ke ztrátě a neexistují žádné programy spuštěné. Ale bude stále se fakturuje hello čas hello kanál byla ve stavu "Spuštění".
>
>

### <a id="states"></a>Stavy kanál a jak jsou mapovány toohello fakturace režimu
aktuální stav Hello kanálu. Možné hodnoty:

* **Zastavit**. Toto je počáteční stav hello hello kanál po jeho vytvoření (Pokud je automatické spuštění byla vybrána hello portálu.) V tomto stavu dojde k žádné fakturace. V tomto stavu můžete aktualizovat vlastnosti hello kanál, ale datový proud není povolen.
* **Spouštění**. Probíhá spouštění Hello kanálu. V tomto stavu dojde k žádné fakturace. V tomto stavu nejsou povolené žádné aktualizace ani streamování. Pokud dojde k chybě, vrátí hello kanál toohello zastaveném stavu.
* **Spuštění**. Hello kanál je schopen zpracování datových proudů za provozu. Nyní ji je fakturace využití. Je potřeba zastavit tooprevent kanál hello další fakturace.
* **Zastavení**. Kanál Hello se zastaví. V tomto dočasném stavu dojde k žádné fakturace. V tomto stavu nejsou povolené žádné aktualizace ani streamování.
* **Odstranění**. Probíhá odstranění Hello kanálu. V tomto dočasném stavu dojde k žádné fakturace. V tomto stavu nejsou povolené žádné aktualizace ani streamování.

Hello následující tabulka ukazuje, jak kanál stavy režimu fakturace toohello mapy.

| Stav kanálu | Indikátory v uživatelském rozhraní portálu | Je fakturace? |
| --- | --- | --- |
| Spouštění |Spouštění |Ne (přechodný stav) |
| Běžící (Spuštěno) |Připraveno (žádný běžící program)<br/>nebo<br/>Streamování (nejméně jeden běžící program) |ANO |
| Zastavování |Zastavování |Ne (přechodný stav) |
| Zastaveno |Zastaveno |Ne |

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Související témata
[Azure Media Services fragmentovaných MP4 Live Ingestování specifikace](media-services-fmp4-live-ingest-overview.md)

[Práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)

[Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md)

[Kvóty a omezení](media-services-quotas-and-limitations.md).  

[Koncepty služby Media Services](media-services-concepts.md)
