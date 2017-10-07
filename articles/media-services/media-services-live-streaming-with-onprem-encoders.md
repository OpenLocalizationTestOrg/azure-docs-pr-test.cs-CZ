---
title: "aaaStream za provozu s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi - Azure | Microsoft Docs"
description: "Toto téma popisuje, jak tooset do kanálu, který obdrží více přenosovými rychlostmi živý datový proud z kodéru místně. Hello datového proudu můžete pak doručena tooclient přehrávání aplikací přes jeden nebo více koncových bodů streamování, pomocí jedné z následujících adaptivní streamování protokoly hello: HLS, technologie Smooth Streaming, čárka."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/12/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 00709cecfc3b5b5dcfaa8f1e4b25bcf9d470d50b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="live-streaming-with-on-premises-encoders-that-create-multi-bitrate-streams"></a>Živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi
## <a name="overview"></a>Přehled
Ve službě Azure Media Services *kanál* představuje cestu pro zpracování datových proudů za provozu obsahu. Kanál, který obdrží živé vstupní datové proudy v jednom ze dvou způsobů:

* Místní kodér za provozu odešle RTMP více přenosovými rychlostmi nebo technologie Smooth Streaming (fragmentovaný MP4) datového proudu toohello kanálu, který není povoleno tooperform live kódování pomocí služby Media Services. Hello požity datové proudy předávání kanály bez dalšího zpracování. Tato metoda je volána *průchozí*. Můžete použít následující kodéry, které mají více přenosovými rychlostmi technologie Smooth Streaming jako výstup hello: Excel média, Ateme, představte si komunikace, Envivio, Cisco a Elemental. Hello následující kodéry mít jako výstupu RTMP: Adobe Flash Live kodér médií, Telestream Wirecast, Haivision, Teradek a čase. Za provozu encoder můžete také odeslat kanálu tooa jednou přenosovou rychlostí datový proud, který není povolen pro kódování v reálném čase, ale který nedoporučujeme. Služba Media Services doručí toocustomers hello datový proud, který ji.

  > [!NOTE]
  > Použití průchozí metody je nejekonomičtější způsob hello toodo živě Streamovat.


* Místní kodér za provozu odešle kanálu toohello jednou přenosovou rychlostí datový proud, který je povolen tooperform za provozu, kódování pomocí služby Media Services v jednom z následujících formátů hello: RTP (MPEG-TS), RTMP nebo technologie Smooth Streaming (fragmentovaný MP4). Hello kanál potom provede kódování v reálném čase hello příchozího jednou přenosovou rychlostí datového proudu tooa více přenosovými rychlostmi (adaptivní) video datového proudu. Služba Media Services doručí toocustomers hello datový proud, který ji.

Od verze hello Media Services 2.10, když vytvoříte kanál, můžete určit, jak se mají kanál tooreceive hello vstupního datového proudu. Také můžete zadat, jestli chcete hello kanál tooperform za provozu, kódování datového proudu. Máte dvě možnosti:

* **Předávání**: Pokud máte v plánu toouse místní kodér za provozu, který bude mít datový proud s více přenosovými rychlostmi (průchozí datový proud) jako výstup tuto hodnotu zadat. V takovém případě příchozího datového proudu hello projdou toohello výstup bez jakékoli kódování. Toto je hello chování kanál před hello 2.10 verze. Toto téma nabízí informace o práci s kanály tohoto typu.
* **Live Encoding**: Zvolte tuto hodnotu, pokud máte v plánu toouse Media Services tooencode datového proudu jednou přenosovou rychlostí živý datový proud tooa více přenosovými rychlostmi. Mějte na paměti, ponechat kódování v reálném čase v kanálu **systémem** stavu bude toho vám být účtovány poplatky. Doporučujeme, abyste po událost live-streaming okamžitě zastavit spuštěný kanálů je kompletní tooavoid velmi hodinové poplatky. Služba Media Services doručí toocustomers hello datový proud, který ji.

> [!NOTE]
> Toto téma popisuje atributy kanály, které nejsou povolené tooperform kódování v reálném čase. Informace o práci s kanály, které jsou povolené, tooperform kódování v reálném čase, najdete v části [živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy](media-services-manage-live-encoder-enabled-channels.md).
>
>

Hello následující diagram představuje pracovním postupu živého streamování, který používá datové proudy (technologie Smooth Streaming) místní kodér za provozu toohave RTMP s více přenosovými rychlostmi nebo fragmentovaný MP4 jako výstup.

![Živý pracovní postup][live-overview]

## <a id="scenario"></a>Běžný scénář live-streaming
Hello následující kroky popisují úlohy týkající se vytváření běžné aplikace live-streaming.

1. Připojení počítače tooa videokameru. Spusťte a nakonfigurujte místní kodér za provozu, který má RTMP s více přenosovými rychlostmi nebo fragmentovaný MP4 (technologie Smooth Streaming) datového proudu jako výstup. Další informace najdete v článku [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase](http://go.microsoft.com/fwlink/?LinkId=532824).

    Můžete také provést tento krok po vytvoření kanálu.
2. Vytvoření a spuštění kanálu.

3. URL ingestování kanálu hello načtení.

    Hello hello používá za provozu kodér ingestování URL toosend hello datového proudu toohello kanál.
4. Načíst URL náhledu kanálu hello.

    Pomocí této adresy URL tooverify, jestli kanál správně přijímá živý datový proud hello.
5. Vytvořte program.

    Pokud používáte hello portálu Azure, vytváření program vytvoří také asset.

    Při použití hello .NET SDK nebo REST, třeba toocreate prostředek a při vytváření program zadat toouse tento prostředek.
6. Publikujte hello asset, který je spojen s programem hello.   

    >[!NOTE]
    >Při vytvoření účtu Azure Media Services **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. Hello koncový bod, ze kterého chcete obsah toostream streamování má toobe v hello **systémem** stavu.

7. Až budete připravené toostart Streamovat a archivovat, spusťte hello program.

8. Volitelně lze za provozu kodér hello signalizovaného toostart oznámení o inzerovaném programu. Hello reklama bude vložena do výstupního datového proudu hello.

9. Zastavte programu hello vždy, když chcete toostop streamování a archivaci události hello.

10. Odstraňte hello program (a volitelně můžete odstranit hello asset).     

## <a id="channel"></a>Popis kanálu a jeho souvisejících součástí
### <a id="channel_input"></a>Vstup kanálu (ingestování) konfigurace
#### <a id="ingest_protocols"></a>Ingestování datových proudů
Služba Media Services podporuje příjem provozu kanály pomocí více přenosovými rychlostmi fragmentovaný soubor MP4 a RTMP s více přenosovými rychlostmi jako protokoly datových proudů. Když ingestování hello RTMP streamování protokol je vybraná, dva ingestování (vstupu) koncové body jsou vytvořené pro kanál hello:

* **Primární adresa URL**: Určuje hello primární RTMP hello kanál plně kvalifikovanou adresu URL ingestování koncový bod.
* **Sekundární adresa URL** (volitelné): Určuje hello sekundární RTMP hello kanál plně kvalifikovanou adresu URL ingestování koncový bod.

Sekundární adresa URL hello Pokud chcete použijte tooimprove hello odolnost a odolnost proti chybám vaší ingestování datového proudu (stejně jako kodéru převzetí služeb při selhání a odolnost proti chybám), hlavně pro hello následující scénáře:

- Jeden kodér dvojitou předání tooboth primární a sekundární adresy URL:

    Hello hlavním účelem tohoto scénáře je tooprovide další kolísání toonetwork odolnost proti chybám a tlumení. Některé RTMP kodéry nezpracovávají sítě odpojí dobře. Při odpojení síťového se stane, může kodér zastavit kódování a pak není hello uložená do vyrovnávací paměti při odesílaly data volání metody reconnect se stane. To způsobí, že nespojitosti a ztrátě dat. Odpojení sítě může dojít z důvodu chybné sítě nebo Údržba na straně Azure hello. Primární a sekundární adresy URL snižují hello problémy se sítí a poskytují řízené procesu upgradu. Pokaždé, když odpojení síťového naplánované se stane Media Services spravuje hello primární a sekundární odpojí a poskytuje zpožděné odpojit mezi dvěma hello. Kodéry pak mít tookeep čas odeslání dat a znovu připojit znovu. Odpojí Hello pořadí hello může být náhodné, ale bude vždy zpoždění mezi primární a sekundární nebo sekundární nebo primární adresy URL. V tomto scénáři je hello kodér stále hello jediný bod selhání.

- Více kodéry s každou kodér vkládání tooa vyhrazené bodu:

    V tomto scénáři poskytuje i kodér a ingestování redundance. V tomto scénáři encoder1 nabízených oznámení toohello primární adresy URL a encoder2 nabízených oznámení toohello sekundární adresy URL. Pokud kodér nezdaří, hello jiných kodér nepřetržitě odesílat data. Je možné udržovat redundanci dat, protože služba Media Services neodpojí primární a sekundární adresy URL v hello stejnou dobu. Tento scénář předpokládá, že kodéry se synchronizovat čas a zadejte přesně hello stejná data.  

- Více kodéry dvojitou předání tooboth primární a sekundární adresy URL:

    V tomto scénáři obou kodéry nabízet data tooboth primární a sekundární adresy URL. To poskytuje nejlepší spolehlivost hello a odolnost proti chybám, jakož i redundanci dat. V tomto scénáři může tolerovat selhání obou kodér a odpojí, i když jedna kodér přestane fungovat. Se předpokládá, že kodéry se synchronizovat čas a zadejte přesně hello stejná data.  

Informace o RTMP kodéry najdete v tématu [podpora RTMP ve službě Azure Media Services a kodéry Live](http://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>Ingestovaných adres URL (koncových bodů)
Kanál, který obsahuje vstupní koncový bod (ingestovanou adresu URL), hello encoder můžete posouvat nastavení zadejte v za provozu encoder hello datové proudy tooyour kanály.   

Můžete získat hello ingestovaných adres URL, když vytvoříte kanál hello. Pro tooget vám tyto adresy URL hello kanál nemá toobe v hello **systémem** stavu. Pokud jste připravené toostart vkládání datový kanál toohello, kanál hello musí být v hello **systémem** stavu. Po hello kanál spuštění příjem dat, můžete zobrazit náhled datového proudu prostřednictvím URL náhledu hello.

Máte možnost z příjem fragmentovaných MP4 (technologie Smooth Streaming) živý datový proud připojení přes protokol SSL. tooingest přes protokol SSL, ujistěte se, že hello tooupdate ingestování tooHTTPS adresy URL. V současné době nelze ingestování RTMP přes protokol SSL.

#### <a id="keyframe_interval"></a>Interval klíčový snímek
Při použití datového proudu místní kodér za provozu toogenerate více přenosovými rychlostmi, hello @keyframe, které určuje interval Určuje dobu trvání hello hello skupinu obrázků (GOP) používá tento externí kodér. Po hello kanál obdrží tento příchozí datový proud, abyste mohli zajistit živého datového proudu aplikace pro přehrávání tooclient v některém z následujících formátů hello: technologie Smooth Streaming, dynamické adaptivní datové proudy přes protokol HTTP (pomlčka) a HTTP Live Streaming (HLS). Když provádíte živé vysílání datového proudu, HLS je vždy zabalené dynamicky. Ve výchozím nastavení služba Media Services automaticky vypočítá podle hello @keyframe, které určuje interval, který při obdržení ze za provozu kodér hello hello HLS segment balení poměr (fragmenty jednotlivých segmentů).

Hello následující tabulka uvádí výpočtu doby trvání segment hello:

| Interval klíčový snímek | HLS segment balení poměr (FragmentsPerSegment) | Příklad |
| --- | --- | --- |
| Menší než nebo roven hodnotě too3 sekund |3:1 |Pokud KeyFrameInterval (nebo GOP) 2 sekundy, hello výchozí HLS segment balení poměr je 3 too1. Tím se vytvoří segment HLS 6 sekundu. |
| 3 too5 sekund |2:1 |Pokud KeyFrameInterval (nebo GOP) je 4 a víc sekund, je hello výchozí HLS segment balení poměr 2 too1. Tím se vytvoří segment HLS 8 sekundu. |
| Větší než 5 sekund |1:1 |Pokud KeyFrameInterval (nebo GOP) 6 sekund, hello výchozí HLS segment balení poměr je 1 too1. Tím se vytvoří segment HLS 6 sekundu. |

Poměr fragmenty za segment hello výstup a nastavení FragmentsPerSegment na ChannelOutputHls konfiguraci hello kanál, můžete změnit.

Hodnota intervalu @keyframe, které určuje hello můžete také změnit nastavením vlastnosti KeyFrameInterval hello u ChanneInput. Pokud nastavíte explicitně KeyFrameInterval hello HLS segment balení poměr vypočítané FragmentsPerSegment prostřednictvím pravidel hello popsané.  

Pokud nastavíte explicitně KeyFrameInterval a FragmentsPerSegment, Media Services použije hello hodnoty, které nastavíte.

#### <a name="allowed-ip-addresses"></a>Povolené IP adresy
Můžete definovat hello IP adresy, které jsou povoleny toopublish video toothis kanál. Povolené IP adresu lze zadat jako jedna z následujících hello:

* Jednu IP adresu (například 10.0.0.1)
* Rozsah adres IP, který používá IP adresy a masky podsítě CIDR (například 10.0.0.1/22)
* Rozsah adres IP, která používá IP adresu a masku podsítě s tečkou (například 10.0.0.1(255.255.252.0))

Pokud nejsou zadány žádné IP adresy a neexistuje žádná definice pravidla, nebudou mít dovoleno žádná IP adresa. tooallow libovolné IP adresy, vytvořte pravidlo a nastavte 0.0.0.0/0.

### <a name="channel-preview"></a>Kanál preview
#### <a name="preview-urls"></a>Adresy URL Preview
Kanály poskytují preview koncový bod (URL náhledu) použít toopreview a ověřit před další zpracování a doručení datového proudu.

Když vytvoříte kanál hello můžete získat URL náhledu hello. Můžete tooget hello adresy URL, hello kanál nemá toobe v hello **systémem** stavu. Po hello kanál spuštění příjem dat, můžete zobrazit náhled datového proudu.

V současné době mohou být zajišťovány hello preview datového proudu pouze v fragmentovaných MP4 formátu (technologie Smooth Streaming), bez ohledu na to hello zadaný typ vstupu. Můžete použít hello [monitorování stavu technologie Smooth Streaming](http://smf.cloudapp.net/healthmonitor) player datový proud hello smooth tootest. Můžete také použít přehrávač, který je hostován v hello Azure portálu tooview datového proudu.

#### <a name="allowed-ip-addresses"></a>Povolené IP adresy
Můžete definovat hello IP adresy, které jsou povoleny koncového bodu náhledu toohello tooconnect. Pokud nejsou zadány žádné IP adresy, bude možné jakékoli IP adresy. Povolené IP adresu lze zadat jako jedna z následujících hello:

* Jednu IP adresu (například 10.0.0.1)
* Rozsah adres IP, který používá IP adresy a masky podsítě CIDR (například 10.0.0.1/22)
* Rozsah adres IP, která používá IP adresu a masku podsítě s tečkou (například 10.0.0.1(255.255.252.0))

### <a name="channel-output"></a>Výstup kanál
Informace o kanálu výstup najdete v tématu hello [@keyframe, které určuje interval](#keyframe_interval) části.

### <a name="channel-managed-programs"></a>Spravované kanál programy
Kanál je přidružený k programům, které můžete toocontrol hello publikování a ukládání segmentů v živém datovém proudu. Kanály spravují programy. Hello vztah kanálů a programů se velmi podobné tootraditional média, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezená toosome vypršel časový limit událostí v tomto kanálu.

Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain programu hello podle nastavení hello **archivačního okna** délka. Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin. Délka archivačního okna také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello. Programy můžou běžet po hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.

Každý program je přidružen asset, který ukládá obsah hello datového proudu. Prostředek je namapované tooa kontejneru objektů blob bloku v účtu úložiště Azure hello a hello soubory v prostředku hello jsou uloženy jako objekty BLOB v tomto kontejneru. programu hello toopublish tak vašim zákazníkům můžete zobrazit hello datového proudu, je nutné vytvořit lokátor OnDemand pro hello související prostředek. Tento Lokátor toobuild můžete použít adresu URL pro streamování, kterou potom poskytnete tooyour klientů.

Kanál podporuje až toothree souběžně spuštěnými programy, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu. Můžete publikovat a archivovat různé části události podle potřeby. Představte si například, že vaše obchodní požadavek zaměřuje tooarchive 6 hodin programu, ale pouze hello toobroadcast posledních 10 minut. tooaccomplish, je nutné toocreate dva současně spuštěné programy. Jeden program nastaven tooarchive 6 hodin hello události, ale není publikována programu hello. Hello jiný program je sada tooarchive 10 minut a tento program budete publikovat.

Existující programy nepoužívejte znovu pro nové události. Místo toho vytvořte nový program pro každou jednotlivou událost. Až budete připravené toostart Streamovat a archivovat, spusťte hello program. Zastavte programu hello vždy, když chcete toostop streamování a archivaci události hello.

toodelete archivovat obsah, zastavte a odstraňte programu hello a potom odstraňte přidružený asset hello. Asset nemůžete odstranit, pokud program používá je. Nejprve je třeba odstranit programu Hello.

I po zastavení a odstranění programu hello můžou uživatelé Streamovat archivovaný obsah jako video na vyžádání, dokud neodstraníte hello asset. Pokud chcete tooretain hello archivovat obsah, ale není ho mít dostupný pro streamování, odstraňte Lokátor streamování hello.

## <a id="states"></a>Stavy kanál a fakturace
Možné hodnoty pro aktuální stav hello kanál:

* **Zastavit**: Toto je počáteční stav hello hello kanál po jeho vytvoření. V tomto stavu můžete aktualizovat vlastnosti hello kanál, ale datový proud není povolen.
* **Spouštění**: hello kanálu se spouští. V tomto stavu nejsou povolené žádné aktualizace ani streamování. Pokud dojde k chybě, vrátí hello kanál toohello **Zastaveno** stavu.
* **Spuštění**: hello kanál může zpracovat datových proudů za provozu.
* **Zastavení**: kanál hello se zastaví. V tomto stavu nejsou povolené žádné aktualizace ani streamování.
* **Odstranění**: hello kanálu se odstraňuje. V tomto stavu nejsou povolené žádné aktualizace ani streamování.

Hello následující tabulka ukazuje, jak kanál stavy režimu fakturace toohello mapy.

| Stav kanálu | Indikátory portálu uživatelského rozhraní | Fakturováno? |
| --- | --- | --- | --- |
| **Spouštění** |**Spouštění** |Ne (přechodný stav) |
| **Spuštění** |**Připraveno** (žádné spuštěné programy)<p><p>nebo<p>**Streamování** (alespoň jeden spuštěným programem) |Ano |
| **Zastavení** |**Zastavení** |Ne (přechodný stav) |
| **Byla zastavena** |**Byla zastavena** |Ne |

## <a id="cc_and_ads"></a>Uzavřené přidávání titulků a ad vložení
Hello následující tabulka ukazuje podporované standardy titulků a vkládání reklam.

| Standard | Poznámky |
| --- | --- |
| CEA 708 a EIA 608 (708/608) |Titulky jsou CEA 708 a EIA 608 standardy pro hello Spojené státy americké a Kanadu.<p><p>V současné době přidávání titulků je podporována pouze v případě, že se provádí v hello kódovaného vstupního datového proudu. Je nutné toouse kodér médií za provozu, která můžete vložit 608 nebo 708 titulky hello kódovaného proudu, který poslal tooMedia služby. Služba Media Services doručí hello obsahu pomocí prohlížeče tooyour vložené titulky. |
| TTML uvnitř .ismt (technologie Smooth Streaming textové stopy) |Dynamické balení Media Services umožňuje obsah toostream klienti v některém z následujících formátů hello: DASH, HLS nebo technologie Smooth Streaming. Ale pokud jste ingestování fragmentovaný soubor MP4 (technologie Smooth Streaming) s titulky uvnitř .ismt (technologie Smooth Streaming textové stopy), abyste mohli zajistit hello datového proudu tooonly technologie Smooth Streaming klientů. |
| SCTE 35 |SCTE 35 je digitální signalizační systém, který byl použit toocue inzerování vložení. Podřízené příjemci použít hello signál toosplice reklamy do datového proudu hello pro hello vyhrazených čas. SCTE 35, musí se poslat jako zhuštěné sledování v hello vstupního datového proudu.<p><p>V současné době hello podporována pouze formát vstupního datového proudu, který představuje signály ad fragmentovaný soubor MP4 (technologie Smooth Streaming). Hello podporuje jenom výstupní formát je také technologie Smooth Streaming. |

## <a id="considerations"></a>Důležité informace
Pokud používáte místní kodér za provozu toosend více přenosovými rychlostmi datového proudu tooa kanál, nastavte hello následující omezení:

* Ujistěte se, že máte dostatek volného internetové připojení toosend data toohello ingestování body.
* Ingestovanou adresu URL, pomocí sekundární vyžaduje dodatečnou šířku pásma.
* příchozí Hello datového proudu s více přenosovými rychlostmi může mít maximálně 10 úrovně kvalitu videa (vrstvy) a nesmí být delší než 5 zvukových stop.
* Hello nejvyšší průměrné přenosovou rychlostí v žádném z úrovně kvalitu videa hello by měla být nižší než 10 MB/s.
* Hello agregace hello průměrná přenosových rychlostí pro všechny datové proudy o videa a zvuku hello by měla být nižší než 25 MB/s.
* Nelze změnit hello vstupní protokol při hello kanál nebo jeho přidružené programy běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý vstupní protokol.
* Můžete ingestování s jednou přenosovou rychlostí v kanálu. Ale protože kanál hello nezpracovává hello datového proudu, hello klientské aplikace dostanou také datový proud s jednou přenosovou rychlostí. (Nedoporučujeme tuto možnost.)

Zde jsou další aspekty související tooworking s kanály a související součásti:

* Pokaždé, když překonfigurujete kodér hello za provozu, volání hello **resetovat** metoda na hello kanálu. Než resetujete hello kanál, musíte toostop hello programu. Po resetování kanálu hello restartujte programu hello.
* Kanál se dá zastavit jenom v případě, že je v hello **systémem** stavu a všechny programy na kanálu hello byly zastaveny.
* Ve výchozím nastavení můžete přidat účet Media Services tooyour jenom 5 kanály. Další informace najdete v tématu [kvóty a omezení](media-services-quotas-and-limitations.md).
* Fakturuje se jenom v případě, že je kanál v hello **systémem** stavu. Další informace najdete v části toohello [kanálu stavy a fakturace](media-services-live-streaming-with-onprem-encoders.md#states) části.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Váš názor
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Související témata
[Azure Media Services fragmentovaných MP4 live ingestování specifikace](media-services-fmp4-live-ingest-overview.md)

[Přehled služby Azure Media Services a běžné scénáře](media-services-overview.md)

[Koncepty služby Media Services](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
