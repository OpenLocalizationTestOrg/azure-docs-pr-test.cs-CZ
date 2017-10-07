---
title: "aaaLive streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy | Microsoft Docs"
description: "Toto téma popisuje, jak tooset do kanálu, který obdrží s jednou přenosovou rychlostí živý datový proud z kodéru na místě a pak provádí za provozu kódování tooadaptive datový proud přenosovou rychlostí pomocí služby Media Services. Hello datového proudu můžete pak doručena tooclient přehrávání aplikací přes jeden nebo více koncové body streamování, pomocí jedné z následujících adaptivní streamování protokoly hello: HLS, Smooth Stream MPEG DASH."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: a8bbdd1570cc9a11bfc2de7bb4ceb9006cc25534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams"></a>Živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy
## <a name="overview"></a>Přehled
V Azure Media Services (AMS) **kanál** představuje cestu pro zpracování obsahu živého streamování. A **kanál** přijímat živé vstupní datové proudy v jednom ze dvou způsobů:

* Místní kodér za provozu odešle datový proud s jednou přenosovou rychlostí toohello kanálu, který je povolený tooperform live kódování pomocí služby Media Services v jednom z následujících formátů hello: RTP (MPEG-TS), RTMP nebo technologie Smooth Streaming (fragmentovaný soubor MP4). Hello kanál potom provede kódování v reálném čase z hello příchozí jednou přenosovou rychlostí datového proudu tooa více přenosovými rychlostmi (adaptivní) datový proud videa. Po vyžádání Media Services doručí datový proud toocustomers hello.
* Místní kodér za provozu odešle více přenosovými rychlostmi **RTMP** nebo **technologie Smooth Streaming** kanálu toohello (fragmentovaný soubor MP4), který není povolen tooperform za provozu, kódování pomocí služby AMS. Hello požity datové proudy prochází **kanál**s bez dalšího zpracování. Tato metoda je volána **průchozí**. Můžete použít následující kodéry, které výstupu technologie Smooth Streaming více přenosovými rychlostmi hello: MediaExcel, Ateme, představte si komunikace, Envivio, Cisco a Elemental. Hello následující kodéry výstupu RTMP: Adobe Flash média Live Encoder (FMLE), Telestream Wirecast, Haivision, Teradek a čase kodérů.  Za provozu encoder můžete také odeslat kanál tooa jednou přenosovou rychlostí datový proud, který nemá povolené kódování v reálném čase, ale tato konfigurace se nedoporučuje. Po vyžádání Media Services doručí datový proud toocustomers hello.
  
  > [!NOTE]
  > Použití průchozí metody je nejekonomičtější způsob hello toodo živě Streamovat.
  > 
  > 

Od verze hello Media Services 2.10, když vytvoříte kanál, můžete určit, jakým způsobem chcete použít pro kanál tooreceive hello vstupního datového proudu a zda chcete použít pro kanál tooperform hello za provozu kódování datového proudu. Máte dvě možnosti:

* **Žádný** – zadejte tuto hodnotu, pokud máte v plánu toouse za provozu kodér místní, který bude výstupního datového proudu více přenosovými rychlostmi (průchozí datový proud). V takovém případě příchozího datového proudu hello předána toohello výstup bez jakékoli kódování. Toto je chování hello kanál too2.10 předchozí verze.  Podrobné informace o práci s kanály tohoto typu, najdete v části [živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md).
* **Standardní** – vyberte tuto hodnotu, pokud máte v plánu toouse Media Services tooencode datového proudu toomulti přenosovými živý datový proud jednou přenosovou rychlostí. Mějte na paměti, že je fakturace dopad pro kódování v reálném čase a měli pamatovat ponechat živého kanálu kódování ve stavu "Spuštění" hello je zpoplatněná fakturace poplatků.  Doporučuje se okamžitě zastavit spuštěný kanálů po živé streamování události je kompletní tooavoid velmi hodinové poplatky.

> [!NOTE]
> Toto téma popisuje atributy kanály, které jsou povolené tooperform kódování v reálném čase (**standardní** kódování typu). Informace o práci s kanály, které nejsou povolené tooperform kódování v reálném čase, najdete v části [živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md).
> 
> Ujistěte se, zda text hello tooreview [aspekty](media-services-manage-live-encoder-enabled-channels.md#Considerations) části.
> 
> 

## <a name="billing-implications"></a>Dopady na fakturaci
Živého kanálu kódování začne fakturace při jeho stav přejde příliš "spuštění" prostřednictvím hello rozhraní API.   Můžete také zobrazit stav hello v hello portál Azure nebo v nástroji Azure Media Services Explorer hello (http://aka.ms/amse).

Hello následující tabulka ukazuje, jak kanál stavy mapovány toobilling stavů v hello rozhraní API a portálu Azure. Upozorňujeme, že jsou mírně liší hello rozhraní API a portál UX hello stavy Jakmile kanál hello "Spuštěná" stav prostřednictvím hello rozhraní API, nebo v hello "Připravené" nebo "Streaming" stav v hello portálu Azure, bude fakturace aktivní.
Kanál hello toostop z jste další fakturace, máte tooStop hello kanál prostřednictvím hello rozhraní API nebo v hello portálu Azure.
Jste zodpovědní za zastavení kanálů, když jste hotovi s živého kanálu kódování hello.  Trvalá fakturace způsobí selhání toostop kódování kanálu.

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

### <a name="automatic-shut-off-for-unused-channels"></a>Automatické vypnutí nepoužívané kanálů
Počínaje 25 leden 2016 Media Services nasazuje aktualizace, která automaticky zastaví kanál (s kódováním v reálném čase povolit) po po dlouhou dobu spuštění v nepoužívané stavu. To platí tooChannels, které mají žádné aktivní programy a které neobdrželi vstupní příspěvku kanálu pro delší dobu.

Prahová hodnota Hello nepoužívané dobu je jmenovitě 12 hodin, ale je toochange subjektu.

## <a name="live-encoding-workflow"></a>Živý pracovní postup kódování
Hello následující diagram představuje živé streamování workflow, kde kanál, který přijímá datový proud s jednou přenosovou rychlostí v jednom z následujících protokolů hello: RTMP, technologie Smooth Streaming nebo RTP (MPEG-TS); kóduje pak hello datového proudu tooa více přenosovými rychlostmi datového proudu. 

![Živý pracovní postup][live-overview]

## <a id="scenario"></a>Běžný scénář živého streamování
Hello následují obecné kroky při vytváření aplikací pro běžné živé streamování.

> [!NOTE]
> V současné době hello maximální doporučená doba živé události je 8 hodin. Pokud potřebujete toorun kanál pro delší časové období, obraťte se na adrese amslived@Microsoft.com. Mějte na paměti, že je fakturace dopad pro kódování v reálném čase a měli pamatovat ponechat živého kanálu kódování ve stavu "Spuštění" hello je zpoplatněná hodinové poplatky.  Doporučuje se okamžitě zastavit spuštěný kanálů po živé streamování události je kompletní tooavoid velmi hodinové poplatky. 
> 
> 

1. Připojení počítače tooa videokameru. Spusťte a nakonfigurujte místní kodér za provozu, který můžete výstup **jeden** datový proud přenosovou rychlostí v jednom z následujících protokolů hello: RTMP, technologie Smooth Streaming nebo RTP (MPEG-TS). 
   
    Tento krok můžete provést i po vytvoření kanálu.
2. Vytvořte a spusťte kanál. 
3. Načtení hello kanál ingestovanou adresu URL. 
   
    Hello URL ingestování používá hello za provozu kodér toosend hello datového proudu toohello kanál.
4. Načíst URL náhledu kanálu hello. 
   
    Pomocí této adresy URL tooverify, jestli kanál správně přijímá živý datový proud hello.
5. Vytvořte program. 
   
    Při použití hello portálu Azure, vytváření program vytvoří také asset. 
   
    Při použití sady .NET SDK nebo REST potřebovat toocreate prostředek a při vytváření Program zadat toouse tento prostředek. 
6. Publikujte asset hello přidružené programu hello.   
   
    >[!NOTE]
    >Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. Hello koncový bod, ze kterého chcete obsah toostream streamování má toobe v hello **systémem** stavu. 
    
7. Když jsou připravené toostart Streamovat a archivovat, spusťte hello program.
8. Volitelně lze za provozu kodér hello signalizovaného toostart oznámení o inzerovaném programu. Hello reklama bude vložena do výstupního datového proudu hello.
9. Zastavte programu hello vždy, když chcete toostop streamování a archivaci události hello.
10. Odstraňte hello Program (a volitelně můžete odstranit hello asset).   

> [!NOTE]
> To je velmi důležité není tooforget tooStop kanál živé kódování. Mějte na paměti, že je každou hodinu fakturace vlivu pro kódování v reálném čase a měli pamatovat ponechat živého kanálu kódování ve stavu "Spuštění" hello je zpoplatněná fakturace poplatků.  Doporučuje se okamžitě zastavit spuštěný kanálů po živé streamování události je kompletní tooavoid velmi hodinové poplatky. 
> 
> 

## <a id="channel"></a>Vstup kanál (ingestování) konfigurace
### <a id="Ingest_Protocols"></a>Ingestování datových proudů
Pokud hello **kodér typ** je nastaven příliš**standardní**, platné možnosti jsou:

* **RTP** (MPEG-TS): datový proud přenosu MPEG-2 využívající RTP.  
* Jednou přenosovou rychlostí **RTMP**
* Jednou přenosovou rychlostí **fragmentovaný soubor MP4** (technologie Smooth Streaming)

#### <a name="rtp-mpeg-ts---mpeg-2-transport-stream-over-rtp"></a>RTP (MPEG-TS) - MPEG-2 Transport Stream využívající RTP.
Typický případ použití: 

Professional televizního obvykle spolupracovat s vyšší kategorie místními kodéry od dodavatelů, jako je technologie elementární Ericsson, Ateme, Imagine nebo Envivio toosend datového proudu. Často se používá ve spojení s IT oddělení a privátní sítě.

Aspekty:

* Důrazně se doporučuje použít Hello jeden program přenos datového proudu (SPTS) vstup. 
* Můžete zadat až too8 zvukové datové proudy MPEG-2 TS pomocí využívající RTP. 
* datový proud videa Hello by měl mít průměrná přenosovou rychlostí nižší než 15 MB/s
* Hello agregační průměrná přenosovou rychlostí zvuk streamů hello musí mít míň než 1 MB/s
* Podporované kodeky jsou hello následující:
  
  * MPEG-2 / H.262 Video 
    
    * Hlavní profilu (4:2:0)
    * Vysoce profilu (4:2:0, 4:2:2)
    * 422 profilu (4:2:0, 4:2:2)
  * MPEG-4 AVC / H.264 Video  
    
    * Směrný plán, hlavní, vysoce profilu (8bitové 4:2:0)
    * Vysoká 10 profilu (10 bitů 4:2:0)
    * Vysoká 422 profilu (10 bitů 4:2:2)
  * MPEG-2 AAC-LC zvuk 
    
    * Mono, stereofonním systémem, příkazu obklopit (5.1, 7.1)
    * Balení ADTS styl MPEG-2
  * Zvuk Dolby digitální (AC-3) 
    
    * Mono, stereofonním systémem, příkazu obklopit (5.1, 7.1)
  * Zvuk MPEG (vrstvy II a III) 
    
    * Mono, stereofonním systémem
* Doporučené všesměrové vysílání, které zahrnují kodéry:
  
  * Představte si komunikace Selenio ŠIF 1
  * Představte si komunikace Selenio ŠIF 2
  * Elemental za provozu

#### <a id="single_bitrate_RTMP"></a>S jednou přenosovou rychlostí RTMP
Aspekty:

* příchozí datový proud Hello nesmí obsahovat více přenosovými rychlostmi video
* datový proud videa Hello by měl mít průměrná přenosovou rychlostí nižší než 15 MB/s
* zvukový datový proud Hello by měl mít průměrná přenosovou rychlostí nižší než 1 MB/s
* Podporované kodeky jsou hello následující:
* MPEG-4 AVC / H.264 Video
* Směrný plán, hlavní, vysoce profilu (8bitové 4:2:0)
* Vysoká 10 profilu (10 bitů 4:2:0)
* Vysoká 422 profilu (10 bitů 4:2:2)
* MPEG-2 AAC-LC zvuk
* Mono, stereofonním systémem, příkazu obklopit (5.1, 7.1)
* 44,1 kHz vzorkovací frekvenci
* Balení ADTS styl MPEG-2
* Doporučené kodéry patří:
* Kodér Telestream Wirecast
* Flash Media Encoder za provozu

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Fragmentovaný soubor MP4 s jednou přenosovou rychlostí (technologie Smooth Streaming)
Typický případ použití:

Použijte místní kodéry od dodavatelů elementární technologie, Ericsson, Ateme, Envivio toosend hello vstupní stream hello otevřete internet tooa nedaleko datového centra Azure.

Aspekty:

Stejné jako u [s jednou přenosovou rychlostí RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Další důležité informace
* Nelze změnit hello vstupní protokol při hello kanál nebo jeho přidružené programy běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý vstupní protokol.
* Maximální rozlišení pro hello datový proud videa příchozí je 1920 × 1080 a maximálně 60 pole za sekundu Pokud prokládaných nebo 30 snímků za sekundu, pokud progresivní.

### <a name="ingest-urls-endpoints"></a>Ingestovaných adres URL (koncových bodů)
Kanál, který obsahuje vstupní koncový bod (ingestovanou adresu URL), hello encoder můžete posouvat nastavení zadejte v za provozu encoder hello datové proudy tooyour kanály.

Můžete získat hello ingestovaných adres URL, po vytvoření kanálu. tooget tyto adresy URL hello kanál nemá toobe v hello **systémem** stavu. Pokud jste připravené toostart předání dat do hello kanál, musí být v hello **systémem** stavu. Jakmile se kanál hello spustí příjem dat, můžete zobrazit náhled datového proudu prostřednictvím URL náhledu hello.

Máte možnost z příjem fragmentovaný soubor MP4 (technologie Smooth Streaming) živý datový proud připojení přes protokol SSL. tooingest přes protokol SSL, ujistěte se, že hello tooupdate ingestování tooHTTPS adresy URL. Všimněte si, že v současné době AMS nepodporuje SSL s vlastní domény.  

### <a name="allowed-ip-addresses"></a>Povolené IP adresy
Můžete definovat hello IP adresy, které jsou povoleny toopublish video toothis kanál. Povolené IP adresy můžete zadat buď jako jednu IP adresu (např. '10.0.0.1'), rozsah IP adres pomocí IP adresy a masky podsítě s technologií CIDR (např. 10.0.0.1/22), nebo jako rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (např. '10.0.0.1(255.255.252.0)').

Pokud žádné IP adresy nezadáte a nedefinujete žádné pravidlo, nebude povolená žádná IP adresa. tooallow libovolné IP adresy, vytvořte pravidlo a nastavte 0.0.0.0/0.

## <a name="channel-preview"></a>Kanál preview
### <a name="preview-urls"></a>Adresy URL Preview
Kanály poskytují preview koncový bod (URL náhledu) použít toopreview a ověřit před další zpracování a doručení datového proudu.

Když vytvoříte kanál hello můžete získat URL náhledu hello. Adresa URL hello tooget, hello kanál nemá toobe v hello **systémem** stavu.

Jakmile se kanál hello spustí příjem dat, můžete zobrazit náhled datového proudu.

> [!NOTE]
> Aktuálně datového proudu preview hello mohou být dodány pouze v fragmentovaný soubor MP4 formátu (technologie Smooth Streaming) bez ohledu na to hello zadaný typ vstupu. Můžete použít hello [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) player datový proud hello Smooth tootest. Můžete také použít přehrávač hostované v Azure portálu tooview hello datového proudu.
> 
> 

### <a name="allowed-ip-addresses"></a>Povolené IP adresy
Můžete definovat hello IP adresy, které jsou povoleny koncového bodu náhledu toohello tooconnect. Pokud nejsou zadány žádné IP adresy, bude možné jakékoli IP adresy. Povolené IP adresy můžete zadat buď jako jednu IP adresu (např. 10.0.0.1), rozsah IP adres pomocí IP adresy a masky podsítě CIDR (např. 10.0.0.1/22), nebo jako rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (např. 10.0.0.1(255.255.252.0)).

## <a name="live-encoding-settings"></a>Nastavení kódování za provozu
Tato část popisuje, jak může být hello nastavení pro hello kodér za provozu v rámci hello kanál upraveny, když hello **kódování typu** kanálu je nastaven příliš**standardní**.

> [!NOTE]
> Při vložení vícejazyčných stop a provádění kódování v reálném čase s Azure, je podporován pouze protokol RTP pro vícejazyčné vstup. Můžete definovat až too8 zvukové datové proudy MPEG-2 TS pomocí využívající RTP. Příjem více zvukových stop s RTMP nebo technologie Smooth streaming není aktuálně podporováno. Při provádění kódování v reálném čase s [místní live kóduje](media-services-live-streaming-with-onprem-encoders.md), neexistuje žádné takové omezení, protože ať je odeslán tooAMS prostřednictvím kanálu předá bez dalšího zpracování.
> 
> 

### <a name="ad-marker-source"></a>Zdroj značky služby AD
Můžete zadat hello zdroj signálů reklamních značek. Výchozí hodnota je **rozhraní Api**, což znamená, že za provozu kodér hello v rámci hello kanálu naslouchat požadavkům tooan asynchronní **rozhraní API reklamní značky**.

Hello další platnou možností je **Scte35** (povoleno pouze v případě hello ingestování datových proudů protokolu nastavena tooRTP (MPEG-TS). Pokud je zadána Scte35, budou za provozu kodér hello analyzovat SCTE 35 signály ze vstupního proudu RTP (MPEG-TS) hello.

### <a name="cea-708-closed-captions"></a>CEA 708 uzavřený titulky
Volitelné příznak, který informuje hello za provozu kodér tooignore žádná data titulky CEA 708 vložený příchozí video hello. Když nastavený příznak hello toofalse (výchozí), hello kodér rozpozná a CEA 708 data znovu vložit do výstupní hello video datové proudy.

### <a name="video-stream"></a>Datový proud videa
Volitelné. Popisuje hello vstupní video datového proudu. Pokud toto pole není zadán, je použita výchozí hodnota hello. Toto nastavení je povoleno pouze v případě, že hello zadejte, jestli je protokol pro streamování nastavený tooRTP (MPEG-TS).

#### <a name="index"></a>Index
Index počítaný od nuly, který určuje, který vstupní video datový proud, měla by být zpracována podle hello kodér za provozu v rámci hello kanálu. Toto nastavení platí jenom v případě ingestování datových proudů protokol je RTP (MPEG-TS).

Výchozí hodnota je nula. Doporučujeme toosend v jednom programu přenosový stream (SPTS). Pokud vstupní datový proud hello obsahuje více programy, za provozu kodér hello analyzuje hello Program Mapa tabulky (platba) ve vstupu hello, identifikuje hello vstupních hodnot, které mají název typu stream MPEG-2 Video nebo H.264 a uspořádá na hello pořadí zadaném v hello skonta index o základu 0 Hello se pak použije toopick až hello n tou položku tohoto uspořádání.

### <a name="audio-stream"></a>Zvukový datový proud
Volitelné. Popisuje hello vstupní zvuk datové proudy. Pokud toto pole nezadáte, použije hello výchozí hodnoty zadané. Toto nastavení je povoleno pouze v případě, že hello zadejte, jestli je protokol pro streamování nastavený tooRTP (MPEG-TS).

#### <a name="index"></a>Index
Doporučujeme toosend v jednom programu přenosový stream (SPTS). Pokud vstupní datový proud hello obsahuje více programy, hello kodér za provozu v rámci hello kanál analyzuje hello Program Mapa tabulky (platba) ve vstupu hello, identifikuje hello vstupních hodnot, které mít název, typ datového proudu ADTS AAC MPEG-2 nebo AC-3 A systému nebo AC 3 systému-B nebo privátní MPEG-2 PES nebo zvuk MPEG-1 nebo ve zvukovém souboru MPEG-2 a uspořádá na hello pořadí zadaném v hello skonta index o základu 0 Hello se pak použije toopick až hello n tou položku tohoto uspořádání.

#### <a name="language"></a>Jazyk
identifikátor jazyka hello zvuk datového proudu, který odpovídá tooISO 639-2, jako je například ENG. Hello Pokud není přítomný, výchozí hodnota hello je a (není definovaná).

Může být až too8 zvukový kanál, který nastaví zadán, pokud hello vstupní kanál toohello je TS MPEG-2 využívající RTP. Však může existovat žádné dvě položky s hello stejnou hodnotu indexu.

### <a id="preset"></a>Přednastavení systému
Určuje, že hello přednastavení toobe používané hello kodér za provozu v rámci tohoto kanálu. V současné době hello pouze povolené, hodnota je **Default720p** (výchozí).

Všimněte si, že pokud budete potřebovat vlastní přednastavení, měli byste požádat na adrese amslived@Microsoft.com.

**Default720p** bude zakódovat hello video do hello následující vrstvy 7.

#### <a name="output-video-stream"></a>Výstupní datový proud videa
| Přenosovou rychlostí | Šířka | Výška | MaxFPS | Profil | Název výstupního datového proudu |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Vysoký |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Main |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Main |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Main |Video_512x288_850kbps |
| 550 |384 |216 |30 |Main |Video_384x216_550kbps |
| 350 |340 |192 |30 |Směrný plán |Video_340x192_350kbps |
| 200 |340 |192 |30 |Směrný plán |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Výstupní datový proud zvuk
Zvuk je zakódovaný toostereo AAC-LC na 64 kb/s, vzorkování frekvenci 44,1 kHz.

## <a name="signaling-advertisements"></a>Signalizace oznámení o inzerovaném programu
Pokud kanál Live Encoding povolené, máte součást v vašeho kanálu, který je video zpracování a s ním pracovat. Můžete signál pro hello kanál tooinsert slaty nebo oznámení o inzerovaném programu odchozí hello datového proudu s adaptivní přenosovou rychlostí. Slaty jsou stále bitové kopie, můžete použít toocover až hello vstupní za provozu informačního kanálu v některých případech (například během zalomení komerční). Inzerování signály, jsou čas synchronizován signály, které můžete vložit do hello odchozí datového proudu tootell hello přehrávání videa tootake zvláštní akce – například tooswitch tooan inzerování na příslušnou dobu hello. Toto [blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) přehled hello SCTE 35 signalizační mechanismus používaný k tomuto účelu. Níže je typický scénář, který může implementovat v živé události.

1. Máte vašeho prohlížeče získat bitovou kopii před událostí před zahájením hello událostí.
2. Máte vašeho prohlížeče získat bitovou kopii po události po skončení hello událostí.
3. Máte vašeho prohlížeče získat bitovou kopii událost chyby, pokud došlo k potížím při hello událostí (například power Chyba v hello stadium).
4. Odeslání AD-BREAK bitové kopie toohide hello živé události během zalomení komerční informačního kanálu.

Hello následují hello vlastnosti, které můžete nastavit při signalizace oznámení o inzerovaném programu. 

### <a name="duration"></a>Doba trvání
Doba trvání Hello, v sekundách, přerušení obchodní hello. Tato akce nemá toobe kladnou hodnotu nula v pořadí toostart hello komerční přerušení. Po zalomení komerční průběh a doby trvání hello je sada toozero s hello CueId odpovídající hello průběžné komerční přerušení, pak tohoto rozdělení je zrušená.

### <a name="cueid"></a>CueId
Jedinečné ID pro komerční zalomení hello toobe používané aplikace pro příjem dat tootake příslušné akce. Musí toobe kladné celé číslo. Můžete nastavit tuto hodnotu tooany náhodné kladné celé číslo nebo použijte nadřazenému systému tootrack hello Cue ID. Zkontrolujte všechny celá čísla ID toopositive určité toonormalize před odesláním prostřednictvím hello rozhraní API.

### <a name="show-slate"></a>Zobrazit projektem
Volitelné. Signály hello za provozu kodér tooswitch toohello [výchozí projektem](media-services-manage-live-encoder-enabled-channels.md#default_slate) bitovou kopii při zalomení komerční a skrýt hello příchozí video informačního kanálu. Zvuk je také ztlumen během projektem. Výchozí hodnota je **false**. 

Hello bitová kopie používaná bude hello uvedené prostřednictvím hello výchozí projektem asset Vlastnost Id v době vytvoření kanálu hello hello. Hello projektem bude roztažen tak velikost image zobrazení toofit hello. 

## <a name="insert-slate--images"></a>Vložení obrázků projektem
Hello kodér za provozu v rámci hello kanál může být signalizovaného tooswitch tooa projektem image. Může být také signalizovaného tooend průběžné projektem. 

Kodér Hello za provozu můžete být nakonfigurované tooswitch tooa projektem a skrýt hello příchozí video signál v určitých situacích – například během pozastavení služby ad. Pokud takový projektem není nakonfigurováno, není během tohoto rozdělení ad maskovat vstupní video.

### <a name="duration"></a>Doba trvání
Hello dobu trvání hello projektem v sekundách. Tato akce nemá toobe kladnou hodnotu nula v pořadí toostart hello projektem. Pokud dojde průběžné projektem a je zadaná doba trvání nula, bude ukončen této průběžné projektem.

### <a name="insert-slate-on-ad-marker"></a>Vložit slate nebo reklamní značku
Sada tootrue, toto nastavení při konfiguraci hello za provozu kodér tooinsert bitovou kopii projektem během pozastavení služby ad. Hello výchozí hodnota je true. 

### <a id="default_slate"></a>Id prostředku výchozí projektem

Volitelné. Určuje hello Asset Id hello média služby Asset, který obsahuje bitovou kopii hello projektem. Výchozí hodnota je null. 


>[!NOTE] 
>Před vytvořením kanálu hello, hello projektem bitové kopie s hello následující omezení musí být nahrán jako vyhrazené asset (žádné další soubory musí být v této asset). Tuto bitovou kopii se používá, jen když za provozu kodér hello vkládá projektem kvůli tooan ad přerušení, nebo byl explicitně signál tooinsert projektem. Kodér Hello za provozu můžete také přejít do režimu projektem během určité chybové stavy – například pokud dojde ke ztrátě hello signál vstupní. Je aktuálně žádná možnost toouse vlastní image za provozu kodér hello zaujme takový stav vstupní signál ztráty. Pro tuto funkci můžete hlasovat [zde](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* Maximálně 1920 × 1080 v řešení.
* Maximálně 3 MB velikost.
* Název souboru Hello musí mít příponu *.jpg.
* Hello bitové kopie, musí se nahrát do prostředku jako hello pouze AssetFile v tohoto prostředku a tento AssetFile by měl být označen jako primární soubor hello. Hello prostředek nemůže být šifrování úložiště.

Pokud hello **výchozí z břidlice Asset Id** není zadán, a **vložit projektem na ad značky** je nastaven příliš**true**, výchozí obrázek Azure Media Services bude použité toohide hello vstup datový proud videa. Zvuk je také ztlumen během projektem. 

## <a name="channels-programs"></a>Kanál pro programy
Kanál je přidružený k programům, které umožňují toocontrol hello publikování a ukládání segmentů v živém datovém proudu. Kanály spravují programy. Hello vztah kanálů a programů se velmi podobné tootraditional médií, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezená toosome vypršel časový limit událostí v tomto kanálu.

Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain programu hello podle nastavení hello **archivačního okna** délka. Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin. Délka archivačního okna také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello. Programy můžou běžet po hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.

Každý program je přidružena k Assetu, která ukládá hello datového proudu obsahu. Prostředek je namapované tooa blokového kontejneru objektů blob v účtu Azure Storage hello a hello soubory v prostředku hello jsou uloženy jako objekty BLOB v tomto kontejneru. program toopublish hello, takže vaši zákazníci mohou zobrazit hello datového proudu, je nutné vytvořit lokátor OnDemand pro hello související prostředek. Tento Lokátor vám umožní toobuild adresu URL pro streamování, kterou potom poskytnete tooyour klientů.

Kanál podporuje až toothree souběžně spuštěnými programy, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu. To vám umožní toopublish a archivovat různé části události podle potřeby. Například vaše firemní požadavky je tooarchive 6 hodin programu, ale toobroadcast pouze posledních 10 minut. tooaccomplish, je nutné toocreate dva současně spuštěné programy. Jeden program nastaven tooarchive 6 hodin hello události ale programu hello není publikována. Hello jiný program je sada tooarchive 10 minut a tento program budete publikovat.

Existující programy nepoužívejte znovu pro nové události. Místo toho vytvořte a spusťte nový program pro každou jednotlivou událost, jak je popsáno v části hello programování aplikací živé vysílání datového proudu.

Když jsou připravené toostart Streamovat a archivovat, spusťte hello program. Zastavte programu hello vždy, když chcete toostop streamování a archivaci události hello. 

obsah toodelete archivovat, zastavení a odstranění programu hello a potom odstraňte přidružený asset hello. Asset nemůžete odstranit, pokud se používá pomocí programu. Nejprve je třeba odstranit programu Hello. 

I po zastavení a odstranění programu hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset.

Pokud chcete archivovat hello tooretain obsahu, ale není ho mít dostupný pro streamování, odstraňte Lokátor streamování hello.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Získávání miniatur náhled živého kanálu
Pokud je povolena funkce Live Encoding, teď se můžete náhled živého kanálu hello jako nedosáhne hello kanálu. Může to být cenným nástrojem toocheck, zda váš informační kanál za provozu ve skutečnosti dosahuje hello kanálu. 

## <a id="states"></a>Stavy kanál a jak stavy mapovány toohello fakturace režimu
aktuální stav Hello kanálu. Možné hodnoty:

* **Zastavit**. Toto je počáteční stav hello hello kanál po jeho vytvoření. V tomto stavu můžete aktualizovat vlastnosti hello kanál, ale datový proud není povolen.
* **Spouštění**. Probíhá spouštění Hello kanálu. V tomto stavu nejsou povolené žádné aktualizace ani streamování. Pokud dojde k chybě, vrátí hello kanál toohello zastaveném stavu.
* **Spuštění**. Hello kanál je schopen zpracování datových proudů za provozu.
* **Zastavení**. Kanál Hello se zastaví. V tomto stavu nejsou povolené žádné aktualizace ani streamování.
* **Odstranění**. Probíhá odstranění Hello kanálu. V tomto stavu nejsou povolené žádné aktualizace ani streamování.

Hello následující tabulka ukazuje, jak kanál stavy režimu fakturace toohello mapy. 

| Stav kanálu | Indikátory v uživatelském rozhraní portálu | Fakturováno? |
| --- | --- | --- |
| Spouštění |Spouštění |Ne (přechodný stav) |
| Běžící (Spuštěno) |Připraveno (žádný běžící program)<br/>nebo<br/>Streamování (nejméně jeden běžící program) |Ano |
| Zastavování |Zastavování |Ne (přechodný stav) |
| Zastaveno |Zastaveno |Ne |

> [!NOTE]
> V současné době hello kanál počáteční průměr je asi 2 minuty, ale v některých případech může trvat až too20 + minut. Resetování kanálu může trvat až too5 minut.
> 
> 

## <a id="Considerations"></a>Důležité informace
* Když kanál **standardní** typ kódování dojde ke ztrátě vstupní zdroj/příspěvku informačního kanálu, ho kompenzuje ho tak, že nahradíte zdroj hello video nebo zvuk s projektem chyba a nečinnosti. Hello kanál bude pokračovat tooemit projektem, dokud hello vstup/příspěvku kanálu obnoví. Doporučujeme vám, že nebyla ve stavu, po dobu delší než 2 hodiny zbývajících živého kanálu. Za tímto bodem, není zaručena hello chování hello kanál na opětovné připojení vstupní, ani je její chování v odpovědi tooa příkaz obnovení. Bude mít toostop hello kanál, odstraňte jej a vytvořte novou.
* Nelze změnit hello vstupní protokol při hello kanál nebo jeho přidružené programy běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý vstupní protokol.
* Pokaždé, když překonfigurujete kodér hello za provozu, volání hello **resetovat** metoda na hello kanálu. Než resetujete hello kanál, musíte toostop hello programu. Po resetování kanálu hello restartujte programu hello.
* Kanál se dá zastavit jenom v případě, že je ve stavu spuštění hello a byly zastaveny všechny programy na hello kanálu.
* Ve výchozím nastavení lze přidat pouze 5 kanály tooyour účtu Media Services. Toto je doporučené kvóty na všechny nové účty. Další informace najdete v tématu [kvóty a omezení](media-services-quotas-and-limitations.md).
* Nelze změnit hello vstupní protokol při hello kanál nebo jeho přidružené programy běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý vstupní protokol.
* Se účtují pouze pokud je kanál v hello **systémem** stavu. Další informace najdete v části příliš[to](media-services-manage-live-encoder-enabled-channels.md#states) části.
* V současné době hello maximální doporučená doba živé události je 8 hodin. Pokud potřebujete toorun kanál pro delší časové období, obraťte se na adrese amslived@Microsoft.com.
* Zajistěte, aby toohave hello streamování koncový bod, ze kterého chcete obsah toostream hello **systémem** stavu.
* Při vložení vícejazyčných stop a provádění kódování v reálném čase s Azure, je podporován pouze protokol RTP pro vícejazyčné vstup. Můžete definovat až too8 zvukové datové proudy MPEG-2 TS pomocí využívající RTP. Příjem více zvukových stop s RTMP nebo technologie Smooth streaming není aktuálně podporováno. Při provádění kódování v reálném čase s [místní live kóduje](media-services-live-streaming-with-onprem-encoders.md), neexistuje žádné takové omezení, protože ať je odeslán tooAMS prostřednictvím kanálu předá bez dalšího zpracování.
* Hello kódování přednastavené používá hello představu o "maximální kmitočet" 30 snímků za sekundu. Takže pokud hello vstup je 60fps / 59.97i, Vstupní rámce hello jsou vyřazeny nebo deaktivuje-interlaced too30/29,97 snímků za sekundu. Pokud vstupní hello je 50fps/50i, jsou vstupní rámce hello too25 vyřadit/deaktivuje-interlaced fps. Pokud vstup hello 25 snímků za sekundu, zůstane výstup 25 snímků za sekundu.
* Nezapomeňte tooSTOP YOUR kanály po dokončení. Pokud ne, bude pokračovat fakturace.

## <a name="known-issues"></a>Známé problémy
* Doba spuštění kanálu se zlepšila tooan průměr 2 minuty, ale v některých případech zvýšené poptávky stále může trvat až too20 + minut.
* Podpora RTP je catered směrem professional televizního. Přečtěte si poznámky hello k RTP v [to](https://azure.microsoft.com/blog/2015/04/13/an-introduction-to-live-encoding-with-azure-media-services/) blogu.
* Projektem obrázků musí být v souladu toorestrictions popsané [zde](media-services-manage-live-encoder-enabled-channels.md#default_slate). Pokud se pokusíte vytvořte kanál s chybou výchozí projektem, který je větší než 1920 × 1080 hello požadavek bude běh limitu.
* Ještě jednou... Nezapomeňte po dokončení tooSTOP YOUR kanály streamování. Pokud ne, bude pokračovat fakturace.

## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Související témata
[Doručování živě streamovaných událostí pomocí služby Azure Media Services](media-services-overview.md)

[Vytvoření kanálů, které provádějí kódování v reálném čase z jednom přenosovou rychlostí tooadaptive datový proud přenosovou rychlostí pomocí portálu](media-services-portal-creating-live-encoder-enabled-channel.md)

[Vytvoření kanálů, které provádějí kódování v reálném čase z jednom přenosovou rychlostí tooadaptive datový proud přenosovou rychlostí pomocí .NET SDK](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[Správa kanálů pomocí rozhraní REST API](https://docs.microsoft.com/rest/api/media/operations/channel)
 
[Koncepty služby Media Services](media-services-concepts.md)

[Azure Media Services fragmentovaných MP4 Live Ingestování specifikace](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

