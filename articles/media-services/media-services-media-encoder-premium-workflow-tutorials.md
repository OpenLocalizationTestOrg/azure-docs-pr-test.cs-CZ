---
title: "kurzy aaaAvanced Media Encoder Premium pracovního postupu"
description: "Tento dokument obsahuje postupů, které ukazují, jak tooperform pokročilé úlohy s Media Encoder Premium pracovního postupu a také jak toocreate komplexní pracovní postupy pomocí Návrháře pracovního postupu."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 641bd1be3bd795b5e138fa7ffdf299ff6710904e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Pokročilé kurzy Media Encoder Premium pracovního postupu
## <a name="overview"></a>Přehled
Tento dokument obsahuje postupů, které ukazují jak pracovních toocustomize s **Návrhář postupu provádění**. Můžete najít hello soubory skutečné pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>TOC
jsou popsané Hello následující témata:

* [Kódování MXF do MP4 s jednou přenosovou rychlostí](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Spuštění nového pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Pomocí hello vstupní soubor média](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Probíhá kontrola mediální datové proudy](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Přidání video kodéru pro. Generování souboru MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [Kódování zvukový datový proud hello](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Multiplexní Audio a Video datových proudů do kontejner MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [Zapisování souboru MP4 hello](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Vytváření Asset Media Services z hello výstupní soubor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [Test hello dokončení pracovního postupu místně](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [Kódování MXF do multibitrate soubory MP4 s rychlostmi – dynamické balení povoleno](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Přidání další jeden nebo více MP4 výstupy](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [Konfigurace názvy výstupního souboru hello](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Přidání samostatných sledovat zvuk](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [Přidání hello. Soubor SMIL ISM](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [Kódování MXF do multibitrate MP4 - rozšířené plán, podle kterého](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [Přehled tooenhance pracovního postupu](#workflow-overview-to-enhance)
  * [Konvence pro pojmenování souborů](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [Publikování vlastnosti komponenty do kořenové hello pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Vygenerování výstupního souboru, jejichž názvy na hodnoty publikovaných vlastností](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Přidává se výstup MP4 toomultibitrate miniatur](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [Miniatury tooadd přehled pracovního postupu pro](#workflow-overview-to-add-thumbnails-to)
  * [Přidání JPG kódování](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Práci s barevný prostor převod](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Zápis hello miniatur](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [Zjištění chyb v pracovním postupu](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Dokončení pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Oříznutí založené na čase multibitrate MP4 výstupu](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Pracovní postup přehled toostart oříznutí k přidání](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Pomocí hello oříznutí datového proudu](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Dokončení pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Představení hello skriptování součásti](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [Skriptování v rámci pracovního postupu: hello, world](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Na základě rámce oříznutí multibitrate MP4 výstupu](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Plán, podle kterého přehled toostart oříznutí k přidání](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Pomocí hello klip seznamu XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [Úprava seznamu klip hello z součásti skriptování](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [Přidání vlastnosti ClippingEnabled usnadnění práce](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>Kódování MXF do MP4 s jednou přenosovou rychlostí
V tomto návodu vytvoříme s jednou přenosovou rychlostí. Soubor MP4 s AAC-HE kódovaný zvuk ze. MXF vstupní soubor.

### <a id="MXF_to_MP4_start_new"></a>Spuštění nového pracovního postupu
Otevřete návrháře pracovních postupů a vyberte "Soubor"-"nový pracovní prostor"-"převod plán, podle kterého"

nový pracovní postup Hello zobrazí 3 prvky:

* Primární zdrojový soubor
* Seznam klip ve formátu XML
* Výstupní soubor nebo prostředek  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nový pracovní postup kódování*

### <a id="MXF_to_MP4_with_file_input"></a>Pomocí hello vstupní soubor média
V pořadí tooaccept naše vstupními médii souborů, jeden začíná přidání komponentu vstupní soubor média. tooadd toohello součást pracovního postupu, podívejte se do vyhledávacího pole úložiště hello a přetáhněte hello potřeby položku na návrháře podokně hello. To udělat pro vstupní soubor média hello a připojte hello primární zdrojový soubor součást toohello Filename vstupní PIN kódu z hello vstupní soubor média.

![Připojených mediálních souborů vstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Připojených mediálních souborů vstup*

Před jsme můžete provést, nejdřív potřebujeme návrháře pracovních postupů toohello tooindicate jaké ukázkový soubor rádi bychom znali toouse toodesign naše pracovního postupu se. toodo Ano, klikněte na pozadí návrháře podokně hello a vyhledejte hello primární zdrojový soubor vlastnosti v podokně pravém vlastnost hello. Klikněte na ikonu hello složky a vyberte hello požadovaného souboru tootest pracovního postupu hello se. Jakmile to uděláte, bude součást vstupní soubor média hello zkontrolujte soubor hello a naplnit její výstup PIN tooreflect hello soubor, který ho prověřovány.

![Vstupní soubor vyplněná média](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Vstupní soubor vyplněná média*

Při určuje s co zadané že rádi bychom znali toowork s, neříká ještě kde výstup hello kódovaný moct. Podobně jako hello toohow byla nakonfigurována primární zdrojový soubor, teď nakonfigurovat hello výstupní složky proměnné vlastnost pod ním.

![Nakonfigurovaný vstupní a výstupní vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Nakonfigurovaný vstupní a výstupní vlastnosti*

### <a id="MXF_to_MP4_streams"></a>Probíhá kontrola mediální datové proudy
Je často žádoucí, že tooknow jak hello datového proudu vypadá jako je například toků hello pracovním postupu. tooinspect datový proud v libovolném bodě v pracovním postupu hello, stačí kliknout na výstupu nebo vstupní PIN kód na všech součástí hello. V takovém případě zkuste klepnout na pin výstup hello nekomprimované Video z naší vstupní soubor média. Zobrazí se dialogové okno otevřete umožňuje odchozí video tooinspect hello.

![Probíhá kontrola hello nekomprimované Video výstupní kód pin](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Probíhá kontrola hello nekomprimované Video výstupní kód pin*

V našem případě sdělí nám například jsme se zabývají 1920 × 1080 vstupu v 24 snímků za sekundu v 4:2:2 vzorkování video o téměř 2 minut.

### <a id="MXF_to_MP4_file_generation"></a>Přidání video kodéru pro. Generování souboru MP4
Všimněte si, že nyní, nekomprimované Video a více nekomprimované zvuk výstup kódy PIN nejsou k dispozici pro použití na našich vstupní soubor média. V pořadí tooencode hello příchozí video, potřebujeme komponentu kódování – v takovém případě pro generování. Soubory MP4.

tooencode hello tooH.264 datový proud videa, přidejte plochu návrháře součást toohello AVC kodér videa pro hello. Tato součást využívá uncompress datový proud videa jako vstup a poskytuje AVC komprimovaný datový proud videa na jeho výstupní kód pin.

![Bez připojení kodér AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Bez připojení kodér AVC*

Jeho vlastnosti určují, jak hello kódování přesně se stane. Pojďme Podíváme se na některé z hello důležitější nastavení:

* Výstupní šířky a výšky výstup: Tyto určují hello řešení hello kódovaný videa. V našem případě přejdeme s 640 x 360
* Obnovovací frekvence: toopassthrough sady, které se právě přijaté hello zdroj obnovovací frekvence, je možné toooverride tomto ale. Všimněte si, že takový převod kmitočet snímků není pohybu vyrovnanou.
* Profil a úroveň: Tyto určují profil hello AVC a úroveň. tooconveniently získat další informace o různých úrovních hello a profily, klikněte na ikonu otazníku hello v součásti kodér videa AVC hello a stránku nápovědy hello se zobrazit více podrobností o jednotlivých úrovní hello. Pro naše ukázka přejděme s profilem hlavní na úrovni 3.2 (hello výchozí nastavení).
* Míra režimu řízení a přenosovou rychlostí (kb/s): v tomto scénáři jsme zvolit konstantní přenosovou (CBR) výstup rychlostí 1 200 kb/s
* Video formát: jde o hello VUI (Video použitelnost informace), který získá zápisu do datového proudu hello H.264 (straně informace, které by mohly používat dekodér tooenhance hello zobrazení, ale není nezbytné toocorrectly dekódovat):
* NTSC (typická pro USA nebo Japonsko, pomocí 30 fps)
* PAL (typická pro Evropu, pomocí 25 fps)
* Režim velikost GOP: nakonfigurujeme pevné velikosti GOP pro naše účely s intervalu klíč 2 sekund s GOPs uzavřen. Tím se zajistí kompatibilitu s hello, které poskytuje dynamické balení Azure Media Services.

toofeed naše kodér AVC připojit pin výstup hello nekomprimované Video z hello vstupní soubor média součást toohello nekomprimované Video vstupní PIN kódu z hello AVC kodér.

![Kodér připojené AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Kodér připojené hlavní AVC*

### <a id="MXF_to_MP4_audio"></a>Kódování zvukový datový proud hello
V tomto okamžiku budeme mít kódování video ale hello původní nekomprimované zvukový stream dál potřebuje toobe komprimované. Pro tento budeme věnovat s AAC kódování tak, že hello součást kodér AAC (Dolby). Přidejte ji toohello pracovního postupu.

![Bez připojení kodér AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Bez připojení AAC kodér*

Teď je nekompatibility: při více než pravděpodobně hello vstupní soubor média bude mít dva různé nekomprimovaným zvuk datového proudu k dispozici je pouze jeden nekomprimované zvuk vstupní pin z hello AAC kodér: jeden pro hello zbývajících zvuk kanál a jeden pro hello práva. (V případě, že pracujete s prostorový zvuk, který je 6 kanálů.) Proto není možné připojit toodirectly hello zvuk z hello vstupní soubor média zdroj do hello AAC zvuk kodér. Hello AAC součást očekává takzvané "prokládaná" zvuk datový proud: jeden datový proud, který má oba hello v levém horním a hello správné kanály prokládaný mezi sebou. Jakmile víme z našich zdrojový soubor média audio stopy na jaké pozice ve zdroji hello, můžete se vygeneruje taková prokládaná zvuk datový proud s hello správně přiřazené mluvčího pozice pro doleva a doprava.

Nejdřív jednu bude chtít toogenerated prokládaná datový proud z hello požadované zdroj zvukové kanály. Abychom to bude zpracovávat Hello Interleaver zvuk datového proudu součást. Přidejte ji toohello pracovního postupu a připojte hello zvuk výstupy z hello vstupní soubor média do ní.

![Připojení Interleaver zvukový datový proud](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Připojení Interleaver zvukový datový proud*

Teď, když máme prokládaná zvukový datový proud, jsme ještě neurčili kde tooassign hello doleva nebo doprava mluvčího umisťuje k. V pořadí toospecify to, jsme můžete využít hello přidělující mluvčího pozice uživatel.

![Přidání přidělující uživatel mluvčího pozice](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Přidání přidělující uživatel mluvčího pozice*

Nakonfigurujte hello přidělující mluvčího pozice uživatel pro použití s stereo vstupního datového proudu prostřednictvím filtr přednastavení kodér z "Vlastní" a hello kanál přednastavení názvem "2.0 (L, R)". (To přiřadí hello levém mluvčího pozice toochannel 1 a hello správné mluvčího pozice toochannel 2.)

Připojte hello výstup hello přidělující mluvčího pozice uživatel toohello vstup z hello AAC kodér. Potom je nutné určit hello toowork kodér AAC s "2.0 (L, R)" kanál předvolby, aby věděl, že může toodeal s stereofonní zvuk jako vstup.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexní Audio a Video datových proudů do kontejner MP4
Zadaný naše AVC kódovaného datový proud videa a naše AAC kódovaný zvukový datový proud, jsme můžete zaznamenat do. Kontejner MP4. kombinování různých datových proudů v jeden Hello proces se nazývá "multiplexní" (nebo "muxing"). V takovém případě jsme se prokládání hello zvuk a video datové proudy hello v jediném souvislé. Balíček MP4. Hello komponenty, která koordinuje to pro. Kontejner MP4 se nazývá hello multiplexor ISO MPEG-4. Přidejte jeden plochu návrháře toohello a připojit hello AVC kodér videa a hello AAC kodér tooits vstupy.

![Připojené MPEG4 multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Připojené MPEG4 multiplexor*

### <a id="MXF_to_MP4_writing_mp4"></a>Zapisování souboru MP4 hello
Při zápisu výstupního souboru, se používá hello výstup souboru součásti. Jsme se můžete připojit tento toohello výstup hello ISO MPEG-4 multiplexor tak, aby získá jeho výstup zapsán toodisk. toodo tohoto připojení hello kontejneru (MPEG-4) výstupní kód pin toohello zápisu vstupní pin hello výstupního souboru.

![Připojení výstupního souboru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Připojení výstupního souboru*

Hello název souboru, který bude použit je určen podle hello vlastnosti souboru. Tuto vlastnost mohou být pevně zakódované tooa zadána hodnota, bude pravděpodobně jeden má tooset přes výraz místo.

pracovní postup hello toohave automaticky určit výstup hello souboru název vlastnosti z výrazu, klikněte na název další toohello souboru hello tlačítka (ikona další toohello složku). Z hello rozevírací nabídce a vyberte položku "Výraz". Tím se otevře editor výrazů hello. Nejprve vymažte obsah hello hello editoru.

![Prázdný výraz editoru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Prázdný výraz editoru*

Hello výraz editor umožňuje tooenter žádné literálovou hodnotou, smíšený s minimálně jednu proměnnou. Proměnné začínat znak dolaru. Jako zásahu hello $ klíč hello editor zobrazí rozevírací pole s volbou dostupným proměnným. V našem případě použijeme kombinaci hello výstupní adresář proměnnou a hello vstupní soubor základní název proměnné:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Naplní se Editor výrazů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Naplní se Editor výrazů*

> [!NOTE]
> V pořadí toosee najdete výstupního souboru kódování úlohy v Azure, je nutné zadat hodnotu v editoru výraz hello.
>
>

Jakmile potvrdíte hello výraz zasažení ok, bude okně Vlastnosti hello v tuto chvíli náhled toowhat hodnota hello souboru vlastnost vyřeší.

![Výraz souboru přeloží dir výstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Výraz souboru přeloží dir výstup*

### <a id="MXF_to_MP4_asset_from_output"></a>Vytváření Asset Media Services z hello výstupní soubor
Když jsme napsali výstupní soubor MP4, potřebujeme ještě tooindicate, že tento soubor patří toohello výstupní asset, který služba media services bude generovat v důsledku spuštění tento pracovní postup. toothis koncové hello výstupní soubor nebo Asset uzlu na plátně hello pracovní postup se používá. Všechny příchozí soubory do tohoto uzlu bude součástí Azure Media Services asset výsledné hello.

Připojte hello výstup souboru součást toohello výstupní soubor nebo Asset součást toofinish pracovního postupu hello.

![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Dokončení pracovního postupu*

### <a id="MXF_to_MP4_test"></a>Test hello dokončení pracovního postupu místně
pracovní postup hello tootest místně, stiskněte tlačítko play hello v hello nástrojů v horní části hello. Po dokončení provádění hello pracovního postupu zkontrolujte výstup hello generovaný v hello nakonfigurované výstupní složce. Uvidíte, že hello dokončení MP4 výstupního souboru, který byl zakódován z hello MXF vstupní zdrojový soubor.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Kódování MXF do MP4 - multibitrate povolené dynamické balení
V tomto návodu vytvoříme sadu souborů MP4 s více přenosovou rychlostí s kódováním AAC zvuk z jedné. MXF vstupní soubor.

Když výstupní asset více přenosovými rychlostmi požadované pro použití v kombinaci s funkce dynamické balení hello služeb Azure Media Services, více souborů MP4 zarovnaný GOP jednotlivých různých přenosovou rychlostí a řešení bude nutné toobe vygenerovat. Ano, hello toodo [kódování MXF do MP4 s jednou přenosovou rychlostí](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) návod poskytuje nám to dobrý výchozí bod.

![Spuštění pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Spuštění pracovního postupu*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Přidání další jeden nebo více MP4 výstupy
Každý soubor MP4 v našem výsledné asset Azure Media Services bude podporovat různé přenosovou rychlostí a řešení. Umožňuje přidat jeden nebo více MP4 výstupní soubory toohello pracovní postup.

toomake, že máme všechny naše video kodéry vytvořené pomocí hello stejné nastavení, nejpohodlnější tooduplicate hello stávající kodér videa AVC a nakonfigurovat jiné kombinace překlad IP adres a přenosovou rychlostí (přidejme mezi 960 x 540 na 25 snímků za sekundu na 2,5 MB/s). existující encoder hello tooduplicate kopírování vložte jej na plochu návrháře hello.

Připojte hello nekomprimované Video výstup hello vstupní soubor média do naší nové komponenty AVC pin.

![Druhý AVC kodér připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Druhý AVC kodér připojení*

Nyní přizpůsobíte hello konfigurace pro naše nové AVC kodér toooutput 960 x 540 2,5 MB/s. (Použijte jeho vlastnosti "výstup šířka", "Výstup výšky" a "Přenosovou rychlostí (kbps)" pro tuto.)

Zadané chceme toouse hello výsledné asset společně s Azure Media Services dynamické balení, hello koncový bod streamování musí toobe schopný vytvářet z těchto souborů MP4 s fragmenty HLS nebo fragmentovaný soubor MP4/DASH, které jsou právě zarovnaný tooeach/jiných způsobem že klienti, kteří jsou přepínání mezi různých přenosových rychlostí získat jediného smooth průběžné videa a zvuku rozhraní. toomake toho docílit, potřebujeme tooensure, že v hello vlastnosti obou AVC kodéry hello velikost GOP ("skupina obrázky") pro oba soubory MP4 je nastavený too2 sekund, což lze provést:

* nastavení velikosti GOP tooFixed GOP velikost režimu hello a
* Hello klíč rámce Interval tootwo (sekundy).
* také nastavte hello řízení IDR GOP tooClosed GOP tooensure, které jsou všechny GOP stálého na své vlastní bez závislosti

toomake naše pohodlný toounderstand pracovního postupu, přejmenujte hello první AVC kodér příliš "kodér videa AVC 640 x 360 1200 kb/s" a hello druhý kodér AVC "kodér videa AVC 960 x 540 2 500 kb/s".

Přidáte druhý multiplexor ISO MPEG-4 a druhý výstupního souboru. Připojte hello multiplexor toohello nové AVC kodér a ujistěte se, že její výstup se přesměruje do hello výstupního souboru. Také připojit hello AAC zvuk kodér výstup toohello nové multiplexor na vstup. Hello výstup souboru zase pak může být připojené toohello výstupní soubor nebo Asset uzlu tooadd ho toohello Media Services Asset, který bude vytvořen.

![Druhý multiplexor a výstup souboru připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Druhý multiplexor a výstup souboru připojení*

Zajištění kompatibility se službou Azure Media Services dynamické balení nakonfigurujte počet tooGOP hello multiplexor na režimu bloku nebo doba trvání a nastavte hello GOPs za too1 bloku. (To by měl být výchozí hello.)

![Režimy multiplexor bloku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Režimy multiplexor bloku*

Poznámka: může být vhodné toorepeat tento proces pro další přenosovou rychlostí a řešení, že chcete toohave kombinace přidal výstup toohello asset.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurace názvy výstupního souboru hello
Máme více než jeden jediný soubor přidané toohello výstupní asset. To poskytuje jistotu hello nutné toomake, které názvy souborů pro každou hello výstupní soubory se liší od sebe navzájem a možná i použít zadávání názvů tak, že se z názvu souboru hello co že pracujete s.

Názvy výstupního souboru se dá řídit přes výrazy v Návrháři hello. Otevřete podokno hello vlastnost pro jednu ze součástí výstup souboru hello a otevřete editor hello výraz pro hello vlastnost souboru. Naše první výstupní soubor byl nakonfigurovaný pomocí hello následující výraz (Zobrazit hello kurz pro přechod z [MXF tooa jednou přenosovou rychlostí MP4 výstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

To znamená, že naše filename je dáno dvou proměnných: hello výstupní adresář toowrite v a hello základní název zdrojového souboru. dřívějším Hello je k dispozici jako vlastnost v kořenovém adresáři hello pracovního postupu a pozdější hello je dáno hello příchozího souboru. Všimněte si, že hello výstupního adresáře je použít pro místní testování; Tato vlastnost bude přepsáno modul pracovních postupů hello při spuštění pracovního postupu hello procesorem hello médium založené na cloudu ve službě Azure Media Services.
toogive i naše výstupní soubory konzistentní výstup pojmenování, změna hello pojmenování výraz, který se nejprve souboru:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

a druhý hello na:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Spusťte test zprostředkující spustit toomake, že oba výstupní soubory MP4 správně generují.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Přidání samostatných sledovat zvuk
Jako ukážeme později při jsme generovat toogo soubor .ism s naše výstupní soubory MP4, bude také vyžadujeme pouze zvukový soubor MP4 jako hello zvuk sledování pro naše adaptivní streamování. toocreate to souboru, přidání pracovního postupu další multiplexor toohello (multiplexor ISO-MPEG-4) a připojte pin výstup hello AAC kodér s jeho vstupní PIN kódu pro sledování 1.

![Zvuk multiplexor přidán](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Zvuk multiplexor přidán*

Vytvoření výstupního souboru součást toooutput hello odchozí proudu třetí z hello multiplexor a nakonfigurujte výraz pojmenování souboru hello:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Zvuk multiplexor vytvoření výstupního souboru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Zvuk multiplexor vytvoření výstupního souboru*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Přidání hello. Soubor SMIL ISM
Hello dynamické balení toowork v kombinaci s obou MP4 soubory (a hello pouze MP4) v našem asset Media Services je také třeba soubor manifestu (také označovaný jako soubor "SMIL": synchronizované multimédia integrace jazyka). Tento soubor označuje tooAzure Media Services, jaké soubory MP4 jsou dostupné pro dynamické balení a které z těchto tooconsider pro hello zvuk streamování. Typické souboru manifestu sady na MP4 s jeden datový proud zvuku vypadá takto:

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

soubor .ism Hello obsahuje v rámci příkazu switch, odkaz tooeach hello jednotlivých MP4 videosouborů a kromě toho zvukový soubor toothose také jeden (nebo více) odkazuje na tooan MP4, obsahující pouze zvuk hello.

Generování souboru manifestu hello pro naše sadu na MP4 lze provést prostřednictvím komponenty s názvem hello "AMS Manifest zapisovače". toouse, přetáhněte ji na hello prostor a připojte PIN výstup "Zápis úplná" hello z hello tři výstup souboru součásti toohello AMS Manifest zapisovače vstup. Potom zkontrolujte, zda výstup hello tooconnect hello AMS Manifest zapisovače toohello výstupní soubor nebo Asset.

Stejně jako u našich souboru výstup součásti, nakonfigurujte název výstupního souboru hello .ism s výrazem:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Naše dokončení pracovního postupu vypadá hello níže:

![Dokončení MXF toomultibitrate MP4 pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Dokončení MXF toomultibitrate MP4 pracovního postupu*

## <a id="MXF_to__multibitrate_MP4"></a>Kódování MXF do multibitrate MP4 - rozšířené plán, podle kterého
V hello [předchozího návodu pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) jste viděli, jak jeden vstupní asset MXF, mohou být převedeny na výstupní asset s souborů MP4 s více přenosovými rychlostmi, pouze zvukový soubor MP4 a soubor manifestu pro použití ve spojení s Azure média Dynamické balení služby.

Tento názorný postup se zobrazí, jak některé aspekty hello jdou vylepšit a provedené pohodlnější.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Přehled tooenhance pracovního postupu
![Tooenhance Multibitrate MP4 pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Tooenhance Multibitrate MP4 pracovního postupu*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>Konvence pro pojmenování souborů
V předchozím postupu hello jsme jednoduchý výraz zadaný jako hello základ pro generování názvy výstupního souboru. Když máme některé duplikace: všech součástí souboru jednotlivých výstup hello hello zadaný takové výraz.

Například naše součást výstupní soubor pro první videosoubor hello nakonfigurovaná s tímto výrazem:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Když pro hello druhý výstup video, máme výraz jako:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Nebylo by čisticí méně chyba náchylné k chybám a pohodlnější, pokud jsme může odeberte některé z této duplikace a ujistěte se, co konfigurovatelnější místo? Naštěstí můžeme: možnosti výraz hello návrháře v kombinaci s hello možnost toocreate vlastní vlastnosti v kořenovém adresáři naše pracovního postupu nám umožní úroveň pohodlí.

Předpokládejme, že jsme budete jednotka filename konfigurace z přenosových rychlostí hello hello jednotlivých souborů MP4. Tyto přenosových rychlostí usilujeme budete tooconfigure v jednoho centrálního umístit (na hello root naše grafu), z kterých byly používaná tooconfigure a jednotku generování názvů souborů. toodo toho jsme spustit tím, že publikujete hello přenosovou rychlostí vlastnost z obou AVC kodéry toohello kořenové naše pracovního postupu, takže bude přístupný z obou hello kořenového také kvůli hello AVC kodérů. (I když se zobrazí v dva různé body, se jenom jedna nadřazená hodnota.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>Publikování vlastnosti komponenty do kořenové hello pracovního postupu
Otevřete hello první AVC kodér, přejděte vlastnost toohello přenosovou rychlostí (kbps) a z rozevíracího seznamu hello Volba možnosti publikovat.

![Publikování vlastnost hello přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Publikování vlastnost hello přenosovou rychlostí*

Konfigurace hello publikování dialogové okno kořenové toopublish toohello naše pracovního postupu grafu s názvem "video1bitrate" a publikovaná a čitelné zobrazovaný název "1 Video přenosovou rychlostí". Nakonfigurovat vlastní název skupiny označuje "Streamování přenosových rychlostí" a stiskněte tlačítko Publikovat.

![Publikování vlastnost hello přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Dialogové okno Vlastnosti přenosovou rychlostí*

Opakování hello stejné pro vlastnost přenosovou rychlostí hello hello druhé AVC kodér a pojmenujte ji "video2bitrate" s názvem "2 Video přenosovou rychlostí" a zobrazení, v hello stejné vlastní skupiny "Streamování přenosových rychlostí".

Pokud jsme teď prověřit vlastnosti kořenové hello pracovního postupu, uvidíme náš vlastní skupiny s hello, které se zobrazují dvě vlastnosti publikovaných. Obě jsou odrážející hello hodnotu jejich odpovídajících přenosovou rychlostí AVC kodér.

![Props publikované přenosovou rychlostí v kořenovém adresáři pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Vždy, když chceme tooaccess tyto vlastnosti z kódu nebo z výrazu, jsme tak postupujte takto:

* z vloženého kódu z komponenty vpravo pod kořenovou hello: node.getPropertyAsString('.. / video1bitrate ", null)
* ve výrazu: ${ROOT_video1bitrate}

Umožňuje dokončit skupiny "Streamování přenosových rychlostí" hello pomocí publikování naše zvuk sledovat přenosovou rychlostí na něm také. V rámci hello vlastnosti hello AAC kodér vyhledejte hello přenosovou rychlostí nastavení a vyberte možnost publikovat další tooit hello rozevíracího seznamu. Publikování toohello kořenové hello grafu s názvem "audio1bitrate" a zobrazované jméno "1 zvuk přenosovou rychlostí" v rámci naší vlastní skupiny "Streamování přenosových rychlostí".

![Dialogové okno pro zvuk přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Dialogové okno pro zvuk přenosovou rychlostí*

![Výsledný videa a zvuku props v kořenovém adresáři](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Výsledný videa a zvuku props v kořenovém adresáři*

Upozorňujeme, že některé z těchto tří změna hodnoty také znovu nakonfiguruje a hodnoty na hello příslušné součásti jsou propojeny s hello změny (a případně pomocí publikovat).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Vygenerování výstupního souboru, jejichž názvy na hodnoty publikovaných vlastností
Místo hardcoding naše názvy generovaný soubor jsme nyní můžete měnit naše filename výraz na všech hello výstup souboru součásti toorely na hello přenosovou rychlostí vlastnosti, které jsme právě publikovaný v kořenovém adresáři hello grafu. Počínaje naše první výstupního souboru, najít vlastnost souboru hello a upravit výraz hello takto:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Hello různé parametry v tomto výrazu můžete získat přístup a zadá stiskne hello dolaru na klávesnici hello při v okně výrazu hello. Jeden z dostupných parametrů hello je naše video1bitrate vlastnosti, které jsme dříve publikované.

![Přístup k parametry v rámci výrazu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Přístup k parametry v rámci výrazu*

Hello stejné pro výstup hello souboru pro naše druhý video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

a pro výstup hello pouze zvukový soubor:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Pokud nyní Změníme hello přenosovou rychlostí v žádném z hello video nebo zvuk soubory, bude třeba překonfigurovat příslušných kodér hello a konvence název souboru na základě přenosovou rychlostí hello bude dodržení všechny automatické.

## <a id="thumbnails_to__multibitrate_MP4"></a>Přidává se výstup MP4 toomultibitrate miniatur
Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do přidává se výstup toohello miniatur.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Miniatury tooadd přehled pracovního postupu pro
![Pracovní postup toostart Multibitrate MP4 z](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Pracovní postup toostart Multibitrate MP4 z*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Přidání JPG kódování
Hello srdcem naše miniatur generování bude hello JPG kodér součást možné toooutput JPG soubory.

![Kodér JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*Kodér JPG*

Naše nekomprimované Video datového proudu nelze připojit ale přímo z hello vstupní soubor média do kodér JPG hello. Místo toho očekává, že toobe předat jednotlivé snímky. To, provedeme prostřednictvím brány rámce Video komponent hello.

![Připojení kodér JPG toohello brány rámečkem](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Připojení kodér JPG toohello brány rámečkem*

Hello rámce brány po každé mnoho sekund nebo rámce umožňuje toopass snímku videa. Hello intervalu a hello časového posunu s který tomu je možné konfigurovat ve vlastnostech hello.

![Vlastnosti videa rámce brány](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Vlastnosti videa rámce brány*

Umožňuje vytvořit miniaturu každou minutu nastavením tooTime hello režimu (v sekundách) a hello too60 intervalu.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>Práci s barevný prostor převod
Při jeví jako logické jak nekomprimované Video PIN hello rámce brány a hello vstupní soubor média lze nyní připojit, jsme byste získali upozornění, když jsme by učinit.

![Vstupní barva místo chyby](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Vstupní barva místo chyby*

Je to proto hello způsob barvy, která je reprezentována informace v našem původní nezpracovaná nekomprimované datový proud videa, pocházejících z našich MXF se liší od jaké hello očekává kodér JPG. Více konkrétně takzvané "barvu místo" "RGB" nebo "Ve stupních šedi" je očekáván tooflow v. To znamená, že hello Video rámce brány na datový proud videa příchozí potřebovat toohave převodu z týkající se jeho barevný prostor použije první.

Přetáhněte na hello pracovního postupu hello barva místo převaděč - Intel a připojte ho tooour rámce brány.

![Připojení úpravy místo barev](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Připojení úpravy místo barev*

V okně vlastností hello vyberte hello BGR 24 položky ze seznamu přednastavených hello.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Zápis hello miniatur
Liší od našich video MP4, hello JPG kodér výstup součástí více než jeden soubor. V pořadí toodeal s tím, mohou být použity komponentu scény vyhledávání JPG souboru zapisovače: bude trvat hello příchozí miniatur JPG a zapsat je, každý název souboru se na konci pomocí jiné číslo. (číslo hello obvykle určující hello počet sekund/jednotky v hello datový proud, který hello miniaturu byl čerpají z).

![Představení hello scény vyhledávání JPG souboru zapisovače](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Představení hello scény vyhledávání JPG souboru zapisovače*

Nastavení vlastnosti hello výstupní cesta ke složce s výrazem hello: ${ROOT_outputWriteDirectory}

a vlastnost Filename předponu s hello:

    ${ROOT_sourceFileBaseName}_thumb_

Předpona Hello určí, jak jsou právě hello miniatur soubory pojmenovány. Budou se měly na konci číslo naznačuje hello pozici v datovém proudu hello.

![Vlastnosti scény zapisovače souboru JPG vyhledávání](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Vlastnosti scény zapisovače souboru JPG vyhledávání*

Připojte hello scény vyhledávání JPG souboru zapisovače toohello výstupní soubor nebo Asset uzlu.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>Zjištění chyb v pracovním postupu
Připojte hello vstup hello barva místo převaděč toohello nezpracovaná nekomprimované video výstupu. Nyní testu místní spuštění pro pracovní postup hello. Je pracovní postup hello šance najednou zastavit provádění a znamenat s červenou outline hello komponenty došlo k chybě:

![Chyba místo převaděč barev](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Chyba místo převaděč barev*

Klepněte na ikonu "E" hello trochu red v hello pravém horním rohu hello barva místo převaděč součást toosee co je důvod hello hello kódování pokus se nezdařil.

![Barva místo převaděč chybové dialogové okno](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Barva místo převaděč chybové dialogové okno*

Zjistíte, jak můžete vidět, že má hello příchozí barevný prostor standard pro hello barva místo převaděč toobe rec601 pro naše požadovaný převod YUV tooRGB. Naše datový proud nepodporuje zjevně znamenat rec601 jeho. (Dop. 601 je standard pro kódování prokládaných analogovým video signály v digitální podobě videa. Určuje oblast active pokrývajících 720 světlostí a 360 chrominance vzorků na každý řádek. Barva Hello kódování systému se označuje jako YCbCr 4:2:2.)

toofix, budete jsme uvedli na hello metadata z našich datový proud, který jsme pracujete s rec601 obsah. toodo proto použijeme Video datový typ aktualizační součásti, kterou jsme budete put mezi naše nezpracovaná zdroje a hello barva místo součást převodu. Tento datový typ aktualizační umožňuje hello ruční aktualizace určité video dat vlastnosti typu. Nakonfigurujte tooindicate místo barva standardní z "Rec 601". To způsobí hello Video datový typ aktualizační tootag hello datového proudu s hello "Rec 601" barevný prostor Pokud se žádné barevný prostor ještě definice. (Ho nebude přepsat všechny existující metadata, pokud byla zaškrtnutá políčka přepsání hello.)

![Aktualizace barva místo standardní na hello aktualizační typ dat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Aktualizace barva místo standardní na hello aktualizační typ dat*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Dokončení pracovního postupu
Teď, když naše dokončení naše pracovního postupu, proveďte jiného testu toosee jej předat.

![Dokončení pracovního postupu pro výstup více mp4 s miniaturami](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Dokončení pracovního postupu pro výstup více mp4 s miniaturami*

## <a id="time_based_trim"></a>Oříznutí založené na čase multibitrate MP4 výstupu
Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do ořezávání video hello zdroje podle časová razítka.

### <a id="time_based_trim_start"></a>Pracovní postup přehled toostart oříznutí k přidání
![Počáteční oříznutí tooadd pracovního postupu pro](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Počáteční oříznutí tooadd pracovního postupu pro*

### <a id="time_based_trim_use_stream_trimmer"></a>Pomocí hello oříznutí datového proudu
Oříznutí datového proudu součást Hello umožňuje tootrim hello začátek a konec vstupního datového proudu založit na informace o časování (sekund, minut,...). oříznutí Hello nepodporuje na základě rámce oříznutí.

![Oříznutí datového proudu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Oříznutí datového proudu*

Místo přímo propojení hello AVC kodéry a mluvčího pozice přidělující uživatel toohello vstupní soubor média, jsme budete uveďte mezi tyto oříznutí hello datového proudu. (Jeden pro hello signál video a jeden pro hello prokládaná zvukový signál.)

![V rozmezí PUT oříznutí datového proudu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*V rozmezí PUT oříznutí datového proudu*

Umožňuje nakonfigurovat hello oříznutí tak, aby jsme pouze zpracovat videa a zvuku mezi 15 sekund a 60 sekund v hello video.

Přejděte toohello vlastnosti hello oříznutí datový proud videa a nakonfigurovat čas spuštění (15s) a vlastnosti čas ukončení (60s). toomake se, že jak naše audio a video oříznutí jsou vždy nakonfigurované toohello stejné začínat a končit hodnoty, budeme publikovat těchto toohello kořenové hello pracovního postupu.

![Počáteční čas vlastnost z datového proudu oříznutí publikování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Počáteční čas vlastnost z datového proudu oříznutí publikování*

![Publikování dialogové okno vlastností pro spuštění](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Publikování dialogové okno vlastností pro spuštění*

![Publikování dialogové okno vlastností pro koncový čas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Publikování dialogové okno vlastností pro koncový čas*

Pokud jsme teď prověřit hello kořenové naše pracovního postupu, bude obě vlastnosti zobrazit a konfigurovat přehledně odtud.

![Publikované vlastnosti, které jsou k dispozici v kořenovém adresáři](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Publikované vlastnosti, které jsou k dispozici v kořenovém adresáři*

Nyní otevřete vlastnosti oříznutí hello z hello zvuk oříznutí a nakonfigurujte počáteční a koncový čas s výrazem, který odkazuje toohello publikovaná vlastnosti v kořenovém adresáři hello naše pracovního postupu.

Čas zahájení pro hello zvuk oříznutí:

    ${ROOT_TrimmingStartTime}

a pro její koncový čas:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Dokončení pracovního postupu
![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Dokončení pracovního postupu*

## <a id="scripting"></a>Představení hello skriptování součásti
Skriptované součásti můžete spustit libovolný skripty během fáze provádění hello naše pracovního postupu. Existují čtyři jiné skripty, které mohou být provedeny, každý s konkrétními vlastnostmi a jejich vlastní místo v cyklu hello pracovního postupu:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

Hello dokumentaci hello skriptování součást přejde podrobněji pro každou z výše uvedených hello. V [hello následující části](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** skriptování součást je použité tooconstruct cliplist xml na hello chodu při spuštění pracovního postupu hello. Tento skript je volána v průběhu instalace hello součástí, která se dělá jenom jednou v jeho průběhu životního cyklu.

### <a id="scripting_hello_world"></a>Skriptování v rámci pracovního postupu: hello, world
Přetáhněte součást skripty na plochu návrháře hello a přejmenujte ji (například "SetClipListXML").

![Přidání skriptované komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Přidání skriptované komponenty*

Když si prohlédnout vlastnosti hello hello skriptování součásti, hello, které se zobrazí čtyři typy jiný skript, každý konfigurovat tooa jiný skript.

![Skriptované vlastnosti komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptované vlastnosti komponenty*

Zrušte hello processInputScript a otevřete editor hello pro hello realizeScript. Nyní nastavení a připravené toostart skriptování.

Jazyk Groovy dynamicky kompilované skriptovací jazyk pro platformu Java hello, který bude mít kompatibilitu s Java. Většinu kódu v jazyce Java ve skutečnosti, je platný Groovy kód.

Můžete napsat skript groovy jednoduché hello world v kontextu hello naše realizeScript. Zadejte následující hello v editoru hello:

    node.log("hello world");

Teď spusťte místní testovací běh. Po této spustit, zkontrolujte (prostřednictvím karty systém hello na hello skriptování součásti) hello vlastnost protokoly.

![Výstup Hello world protokolu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Výstup Hello world protokolu*

objekt uzlu Hello, který jsme volat metodu protokolu hello, odkazuje tooour aktuální uzel"" nebo hello součást, kterou jsme se skriptování v rámci. Všechny komponenty jako takový má hello možnost toooutput protokoluje data, k dispozici prostřednictvím systému karta hello. V takovém případě jsme výstup hello řetězcový literál "hello, world". Důležité toounderstand tady je, že to může být toobe neocenitelnou pomocí ladicí nástroj vám poskytnou přehled na ve skutečnosti je to, jaké hello skriptu.

Z našich skriptovací prostředí máme také tooproperties přístup na dalších komponentách. Zkuste provést následující:

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up tooother nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Naše protokolu okně se zobrazí nám hello následující:

![Výstup protokolu pro přístup k uzlu cesty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Výstup protokolu pro přístup k uzlu cesty*

## <a id="frame_based_trim"></a>Na základě rámce oříznutí multibitrate MP4 výstupu
Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do ořezávání hello zdroje videa na základě počtu rámce.

### <a id="frame_based_trim_start"></a>Plán, podle kterého přehled toostart oříznutí k přidání
![Přidání oříznutí k toostart pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Přidání oříznutí k toostart pracovního postupu*

### <a id="frame_based_trim_clip_list"></a>Pomocí hello klip seznamu XML
Ve všech předchozích kurzech pracovního postupu jsme použili vstupní soubor média součást hello jako naše vstupní zdroj videa. Pro tento konkrétní scénář, když budeme používat hello klip seznamu zdrojovou součástí místo. Všimněte si, že nemělo by se jednat hello upřednostňovaný způsob práce; používá jenom hello zdrojového seznamu klip, pokud je skutečný důvod toodo tak (stejně jako v nástroji hello níže případu, kdy provádíme za použití funkcí oříznutí seznamu klip hello).

tooswitch z našich vstupní soubor média toohello klip zdrojového seznamu, přetáhněte součást zdrojového seznamu klip hello na hello návrhová plocha a připojte se uzel XML seznamu klip hello klip seznamu XML pin toohello hello Návrháře pracovního postupu. To by měl naplnit hello zdrojového seznamu klip s výstupní kód PIN, podle tooour vstupní video. Teď připojit hello nekomprimované videa a zvuku nekomprimované PIN z hello hello zdrojového seznamu klip toohello příslušných AVC kodéry a Interleaver zvuk datového proudu. Nyní odeberte hello vstupní soubor média.

![Nahradí hello zdrojového seznamu klip hello vstupní soubor média](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Nahradí hello zdrojového seznamu klip hello vstupní soubor média*

Hello klip seznamu zdrojovou součástí vezme jako vstupní "seznamu XML klip". Když vyberete hello zdrojového souboru tootest s místně, tento klip seznamu xml je automaticky vyplněna za vás.

![Automaticky vyplněna klip seznamu XML – vlastnost](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automaticky vyplněna klip seznamu XML – vlastnost*

Vyhledávání poněkud blíže toohello xml, to je, jak vypadá jako:

![Seznam klip dialogové okno Upravit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Seznam klip dialogové okno Upravit*

To ale nemusí odpovídat hello možnosti hello klip seznamu xml. Jednou z možností, které máme je tooadd element "Trim" v části hello videa a audia zdroje, jako je to:

![Přidávání seznamu klip toohello uvolnění dočasné paměti – element](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Přidávání seznamu klip toohello uvolnění dočasné paměti – element*

Je-li upravit hello klip seznamu xml takto výše a místní spustit test, zobrazí se hello video správně byl oříznut mezi 10 a 20 sekund za hello video.

Jinak zvláštní toowhat se stane, když provedete spuštění místní, i když tato konfigurace xml velmi stejné cliplist neměli hello stejné ovlivňuje při použití v pracovní postup, který běží v Azure Media Services. Při vygenerování Azure Premium kodér spustí, hello cliplist xml pokaždé, když znovu podle hello vstupní soubor hello kódování úlohy byl zadán. To znamená, že by všechny změny, které jsme provést na hello xml bohužel přepsat.

toocounter hello cliplist xml vymazáním při kódování úloha spuštěna, jsme můžete znovu vygenerovat ho v chodu hello právě po spuštění hello naše pracovního postupu. Prostřednictvím co se nazývá "Skriptování Component" můžete provedeny takové vlastní akce. Další informace najdete v tématu [Introducing hello skriptování součást](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Přetáhněte součást skripty na plochu návrháře hello a přejmenujte ji příliš "SetClipListXML".

![Přidání skriptované komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Přidání skriptované komponenty*

Když si prohlédnout vlastnosti hello hello skriptování součásti, hello, které se zobrazí čtyři typy jiný skript, každý konfigurovat tooa jiný skript.

![Skriptované vlastnosti komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptované vlastnosti komponenty*

### <a id="frame_based_trim_modify_clip_list"></a>Úprava seznamu klip hello z součásti skriptování
Než znovu jsme může zapisovat hello cliplist xml, který je generován během spouštění pracovního postupu, budeme potřebovat toohave přístup toohello cliplist xml vlastnosti a obsah. Jsme, postupujte takto:

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Příchozí klip seznamu protokolována](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Příchozí klip seznamu protokolována*

Nejdřív potřebujeme toodetermine způsob, jak z bod, který do bod, který má být tootrim hello videa. publikování tohoto uživatele pohodlný toohello méně technical hello pracovního postupu, toomake dvě vlastnosti toohello kořenové hello grafu. toodo, klikněte pravým tlačítkem na plochu návrháře hello a vyberte možnost "Přidat vlastnost":

* První vlastnost: "ClippingTimeStart" typu: "Časový kód"
* Druhé vlastnosti: "ClippingTimeEnd" typu: "Časový kód"

![Přidat dialogové okno vlastností pro výstřižek počáteční čas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Přidat dialogové okno vlastností pro výstřižek počáteční čas*

![Publikovaná výstřižek props čas v kořenovém adresáři pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Publikovaná výstřižek props čas v kořenovém adresáři pracovního postupu*

Nakonfigurujte obě vlastnosti tooa vhodnou hodnotu:

![Konfigurace hello výstřižek počáteční a koncové vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfigurace hello výstřižek počáteční a koncové vlastnosti*

Nyní z v rámci naší skriptu jsme přístup k obě vlastnosti, jako je to:

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Okno Protokol zobrazující začátku a konci výstřižek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Okno Protokol zobrazující začátku a konci výstřižek*

Umožňuje analyzovat hello časový kód řetězce do pohodlnější toouse formuláře, pomocí jednoduchého regulární výraz:

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Okno Protokol s výstupem Analyzovaná časový kód](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Okno Protokol s výstupem Analyzovaná časový kód*

Tyto informace po ruce jsme nyní můžete upravit hello cliplist xml tooreflect hello počáteční a koncový čas pro hello potřeby rámce přesné výstřižek film hello.

![Oříznutí elementů tooadd kód skriptu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Oříznutí elementů tooadd kód skriptu*

K tomu bylo potřeba prostřednictvím operace manipulace s řetězci normální. Hello výsledné změny klip seznamu xml se nezapisují zpět vlastnost clipListXML toohello v kořenovém adresáři pracovního postupu hello prostřednictvím metody "SetProperty –" hello. okno Protokol Hello po provedení jiného testu nám by zobrazit hello následující:

![Protokolování výsledný seznam klip hello](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Protokolování výsledný seznam klip hello*

Proveďte testovací běh toosee, jak mají byl oříznut datové proudy videa a audia hello. Jako více než jeden testovací běh s různými hodnotami pro hello oříznutí body, můžete to udělat, můžete si všimnout, že ty, nebude vzít v úvahu ale! Důvod Hello je tento hello designer, na rozdíl od hello Azure runtime, nemá přepsání hello cliplist xml každé spuštění. To znamená, že pouze hello velmi poprvé nastavíte hello a odhlašování body, způsobí, že hello xml tootransform, všech dalších hello krát, naše klauzule ochrana (pokud (clipListXML.indexOf ("<trim>") == -1)) zabrání přidáním další trim hello pracovního postupu element, pokud je již jedna nachází.

toomake naše pracovního postupu pohodlný tootest místně, jsme nejlepší přidejte některé údržby kód, který kontroluje, pokud element a uvolnění dočasné paměti byl již existuje. Pokud ano, jsme ho odebrat před pokračováním změnou hello xml s novými hodnotami hello. Místo použití manipulace prostý řetězec, je pravděpodobně bezpečnější toodo to prostřednictvím skutečné xml objektu modelu, analýze.

Předtím, než když jsme můžete přidat takový kód, budeme potřebovat tooadd naše skriptu nejdříve spustit počet prohlášení importu v hello:

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

Potom přidáme hello vyžaduje čištění kódu:

    //for local testing: delete any pre-existing trim elements from hello clip list xml by parsing hello xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
     //delete hello trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

Tento kód jenom překročí hello bodu, kdy přidáme hello trim elementy toohello cliplist xml.

V tuto chvíli jsme můžete spustit a změnit naše pracovní postup, jak tolik, jako chceme přitom má hello změny někdy čas.    

### <a id="frame_based_trim_clippingenabled_prop"></a>Přidání vlastnosti ClippingEnabled usnadnění práce
Chcete nemusí vždy toohappen oříznutí, můžeme Dokončit vypnout naše pracovního postupu přidáním pohodlný logický příznak, který určuje, zda chceme tooenable ořezávání / výstřižek.

Stejně jako dřív, a publikovat nové kořenové toohello vlastnost naše pracovního postupu názvem "ClippingEnabled" typu "Logická hodnota".

![Publikovaná vlastnost pro povolení výstřižek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Publikovaná vlastnost pro povolení výstřižek*

S hello níže klauzule jednoduché ochrana jsme můžete zkontrolujte, zda je vyžadován oříznutí a rozhodnout, pokud naše klip seznam jako takový musí toobe upravit, nebo ne.

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <a id="code"></a>Kompletní kód
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from hello clip list xml by parsing hello xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
    //delete hello trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements toocliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }


## <a name="also-see"></a>Viz také
[Představení Premium kódování v Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Jak tooUse Premium kódování ve službě Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Kódování obsahu na vyžádání pomocí Azure Media Service](media-services-encode-asset.md#media-encoder-premium-workflow)

[Formáty a kodeky pracovního postupu Media Encoderu Premium](media-services-premium-workflow-encoder-formats.md)

[Ukázkové soubory pracovního postupu](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Nástroj pro Azure Media Services Explorer](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
