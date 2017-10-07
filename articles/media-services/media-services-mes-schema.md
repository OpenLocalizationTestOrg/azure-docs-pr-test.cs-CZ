---
title: "schéma standardu pro kodér aaaMedia | Microsoft Docs"
description: "Hello téma nabízí přehled hello Media Encoder Standard schématu."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4c060062-8ef2-41d9-834e-e81e8eafcf2e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 82bad27b9546f75557ac691ff148b46990647632
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard schématu
Toto téma popisuje některé prvky hello a typy schématu XML hello, na kterém [Media Encoder Standard přednastavení](media-services-mes-presets-overview.md) jsou založené. Hello tématu poskytuje vysvětlení elementů a jejich platné hodnoty. úplné schématu Hello bude publikována později.  

## <a name="Preset"></a>Přednastavení (kořenový element)
Definuje předvolby kódování.  

### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Kódování** |[Kódování](media-services-mes-schema.md#Encoding) |Kořenový element udává, že vstupní zdroje hello toobe kódování. |
| **Výstupy** |[Výstupy](media-services-mes-schema.md#Output) |Kolekce požadovaný výstupní soubory. |

### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Verze**<br/><br/> Požaduje se |**xs:decimal** |Hello přednastavené verze. Hello se vztahují následující omezení: hodnota xs:fractionDigits = "1" a xs:minInclusive value = "1", například **verze = "1.0"**. |

## <a name="Encoding"></a>Kódování
Obsahuje posloupnost hello následující prvky.  

### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Nastavení kódování H.264 videa. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Nastavení pro AAC kódování zvukovém souboru. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Nastavení pro obrázku Bpm na obrázek. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Nastavení pro obrázek Png. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Nastavení pro bitovou kopii Jpg. |

## <a name="H264Video"></a>H264Video
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **TwoPass**<br/><br/> Hodnota minOccurs = "0" |**xs:Boolean** |V současné době je podporováno pouze jeden průchodu kódování. |
| **KeyFrameInterval**<br/><br/> Hodnota minOccurs = "0"<br/><br/> **Výchozí = "00: 00:02"** |**: Time** |Určuje hello pevné mezery mezi IDR rámce v jednotkách sekund. Také označuje tooas hello GOP trvání. V tématu **SceneChangeDetection** (pod) pro řízení zda hello encoder můžete odchylují od tuto hodnotu. |
| **SceneChangeDetection**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "false" |**xs:Boolean** |Pokud sada tootrue, pokusy kodér toodetect scény změnit v hello video a vloží již IDR rámečku. |
| **Složitost**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "Rovnováha" |**xs:String** |Ovládací prvky hello kompromis mezi zakódovat kvality rychlost a video. Může mít jednu z následujících hodnot hello: **rychlost**, **Rovnováha**, nebo **kvality**<br/><br/> Výchozí hodnota: **vyrovnáváním** |
| **SyncMode**<br/><br/> Hodnota minOccurs = "0" | |V budoucích verzích se zveřejní funkce. |
| **H264Layers**<br/><br/> Hodnota minOccurs = "0" |[H264Layers](media-services-mes-schema.md#H264Layers) |Kolekce vrstev výstupu videa. |

### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Podmínka** |**xs:String** | Pokud vstup hello žádné video, může být vhodné tooforce hello kodér tooinsert černobílý video sledovat. toodo, který použít podmínku = "InsertBlackIfNoVideoBottomLayerOnly" (tooinsert videa na pouze nejnižší přenosovou rychlostí hello) nebo podmínku = "InsertBlackIfNoVideo" (tooinsert videa na všechny přenosových rychlostí výstup). Další informace najdete v [tomto](media-services-advanced-encoding-with-mes.md#no_video) tématu.|

## <a name="H264Layers"></a>H264Layers

Ve výchozím nastavení odeslání vstupní toohello kodér, který obsahuje pouze zvuk a obraz, bude obsahovat hello výstupní asset soubory s pouze zvuk data. Některé přehrávače nemusí být možné toohandle takové výstupní datové proudy. Můžete použít hello H264Video **InsertBlackIfNoVideo** atribut nastavení tooforce hello kodér tooadd toohello výstup sledovat videa v tomto scénáři. Další informace najdete v [tomto](media-services-advanced-encoding-with-mes.md#no_video) tématu.
              
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **H264Layer**<br/><br/> Hodnota minOccurs = maxOccurs "0" = "bez vazby" |[H264Layer](media-services-mes-schema.md#H264Layer) |Kolekce H264 vrstvy. |

## <a name="H264Layer"></a>H264Layer
> [!NOTE]
> Video omezení jsou založené na hodnoty hello popsané v hello [H264 úrovně](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tabulky.  
> 
> 

### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Profil**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "Auto" |**xs:String** |Může být jedna z následujících hello **xs:string** hodnoty: **automaticky**, **směrného plánu**, **hlavní**, **vysokou**. |
| **Úroveň**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "Auto" |**xs:String** | |
| **Přenosovou rychlostí**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Hello přenosovou rychlostí použít pro tuto vrstvu videa zadaný v kb/s. |
| **MaxBitrate**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Hello maximální přenosovou rychlostí použít pro tuto vrstvu videa zadaný v kb/s. |
| **BufferWindow**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "00: 00:05" |**: Time** |Délka vyrovnávací paměti video hello. |
| **Šířka**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Šířka rámečku, videa hello výstup v pixelech.<br/><br/> Všimněte si, že v současné době musíte zadat šířku a výšku. Hello šířka a výška potřebovat toobe sudým číslům. |
| **Výška**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Výška rámečku, videa hello výstup v pixelech.<br/><br/> Všimněte si, že v současné době musíte zadat šířku a výšku. Hello šířka a výška potřebovat toobe sudým číslům.|
| **BFrames**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Počet snímků B mezi odkaz snímky. |
| **ReferenceFrames**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "3" |**xs:int** |Počet snímků odkaz v GOP. |
| **EntropyMode**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "Cabac" |**xs:String** |Může mít jednu z následujících hodnot hello: **Cabac** a **Cavlc**. |
| **Kmitočet snímků**<br/><br/> Hodnota minOccurs = "0" |racionální číslo |Určuje hello obnovovací frekvence hello výstupu videa. Použijte výchozí hodnotu "0 nebo 1" toolet hello kodér použití hello stejné rámce míra jako hello vstup videa. Povolené hodnoty jsou očekávané toobe běžné frekvenci snímků videa, jak je uvedeno níže. Libovolný platný rozumné je však povolena. Příklad 1 nebo 1 by 1 fps a je platný.<br/><br/> -12/1 (12 fps)<br/><br/> -15/1 (15 fps)<br/><br/> -24/1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> -25/1 (25 fps)<br/><br/>  -30/1 (30 fps)<br/><br/> 30000/1001 (29,97 fps) <br/> <br/>**Poznámka:** při vytváření vlastní přednastavení pro více přenosovými rychlostmi kódování, pak všechny vrstvy hello přednastavení **musí** použití hello stejná hodnota kmitočet snímků.|
| **AdaptiveBFrame**<br/><br/> Hodnota minOccurs = "0" |**xs:Boolean** |Zkopírujte z Azure media encoder |
| **Řezy**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "0" |**xs:int** |Určuje, kolik řezy rámeček je rozdělené do. Doporučujeme používat výchozí. |

## <a name="AACAudio"></a>AACAudio
 Obsahuje posloupnost hello následující prvky a skupiny.  

 Další informace o AAC najdete v tématu [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Profil**<br/><br/> Hodnota minOccurs = "0"<br/><br/> Výchozí = "AACLC" |**xs:String** |Může mít jednu z následujících hodnot hello: **AACLC**, **HEAACV1**, nebo **HEAACV2**. |

### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Podmínka** |**xs:String** |tooforce hello kodér tooproduce asset, který obsahuje tichou zvuk sledovat, kdy se vstup neobsahuje žádný zvuk, zadejte hodnotu "InsertSilenceIfNoAudio" hello.<br/><br/> Ve výchozím nastavení Pokud odesíláte vstupní toohello kodér, který obsahuje pouze video a žádné zvuk, pak hello výstupní asset bude obsahovat soubory, které obsahují pouze video data. Některé přehrávače nemusí být možné toohandle takové výstupní datové proudy. V tomto scénáři můžete použít tato nastavení tooforce hello kodér tooadd výstup toohello tichou zvuk sledovat. |

### <a name="groups"></a>Skupiny
| Referenční informace | Popis |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> Hodnota minOccurs = "0" |Popis [AudioGroup](media-services-mes-schema.md#AudioGroup) tooknow hello odpovídající počet kanálů, vzorkovací frekvenci a přenosovou rychlost, která může být nastavena pro každý profil. |

## <a name="AudioGroup"></a>AudioGroup
Podrobnosti o tom, jaké hodnoty jsou platné pro každý profil najdete v tématu Tabulka "Zvuk kodeků podrobnosti" hello, který následuje.  

### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Kanály**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Hello počet zvukových kanálů kódování. Hello následují platné možnosti: 1, 2, 5, 6, 8.<br/><br/> Výchozí: 2. |
| **SamplingRate**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Hello zvuk vzorkovací frekvenci, zadaný v Hz. |
| **Přenosovou rychlostí**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |přenosovou rychlostí Hello používá při kódování hello zvuk zadané v kb/s. |

### <a name="audio-codec-details"></a>Podrobnosti o zvukových kodeků
Zvukových kodeků|Podrobnosti  
-----------------|---  
**AACLC**|1:<br/><br/> -11025: 8 &lt;= přenosovou rychlostí &lt; 16<br/><br/> -12000: 8 &lt;= přenosovou rychlostí &lt; 16<br/><br/> -16000: 8 &lt;= přenosovou rychlostí &lt;32.<br/><br/>-22050: 24 &lt;= přenosovou rychlostí &lt; 32.<br/><br/> -24000: 24 &lt;= přenosovou rychlostí &lt; 32.<br/><br/> -32000: 32 &lt;= přenosovou rychlostí &lt;= 192<br/><br/> -44100: 56 &lt;= přenosovou rychlostí &lt;= 288<br/><br/> -48000: 56 &lt;= přenosovou rychlostí &lt;= 288<br/><br/> -88200: 128 &lt;= přenosovou rychlostí &lt;= 288<br/><br/> -96000: 128 &lt;= přenosovou rychlostí &lt;= 288<br/><br/> 2:<br/><br/> -11025: 16 &lt;= přenosovou rychlostí &lt; 24<br/><br/> -12000: 16 &lt;= přenosovou rychlostí &lt; 24<br/><br/> -16000: 16 &lt;= přenosovou rychlostí &lt; 40<br/><br/> -22050: 32 &lt;= přenosovou rychlostí &lt; 40<br/><br/> -24000: 32 &lt;= přenosovou rychlostí &lt; 40<br/><br/> -32000: 40 &lt;= přenosovou rychlostí &lt;= 384<br/><br/> -44100: 96 &lt;= přenosovou rychlostí &lt;= 576<br/><br/> -48000: 96 &lt;= přenosovou rychlostí &lt;= 576<br/><br/> -88200: 256 &lt;= přenosovou rychlostí &lt;= 576<br/><br/> -96000: 256 &lt;= přenosovou rychlostí &lt;= 576<br/><br/> 5/6:<br/><br/> -32000: 160 &lt;= přenosovou rychlostí &lt;= 896<br/><br/> -44100: 240 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -48000: 240 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -88200: 640 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -96000: 640 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> 8:<br/><br/> -32000: 224 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -44100: 384 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -48000: 384 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -88200: 896 &lt;= přenosovou rychlostí &lt;= 1024<br/><br/> -96000: 896 &lt;= přenosovou rychlostí &lt;= 1024  
**HEAACV1**|1:<br/><br/> -22050: přenosovou rychlostí = 8<br/><br/> -24000: 8 &lt;= přenosovou rychlostí &lt;= 10<br/><br/> -32000: 12 &lt;= přenosovou rychlostí &lt;= 64<br/><br/> -44100: 20 &lt;= přenosovou rychlostí &lt;= 64<br/><br/> -48000: 20 &lt;= přenosovou rychlostí &lt;= 64<br/><br/> -88200: přenosovou rychlostí = 64<br/><br/> 2:<br/><br/> -32000: 16 &lt;= přenosovou rychlostí &lt;= 128<br/><br/> -44100: 16 &lt;= přenosovou rychlostí &lt;= 128<br/><br/> -48000: 16 &lt;= přenosovou rychlostí &lt;= 128<br/><br/> -88200: 96 &lt;= přenosovou rychlostí &lt;= 128<br/><br/> -96000: 96 &lt;= přenosovou rychlostí &lt;= 128<br/><br/> 5/6:<br/><br/> -32000: 64 &lt;= přenosovou rychlostí &lt;= 320<br/><br/> -44100: 64 &lt;= přenosovou rychlostí &lt;= 320<br/><br/> -48000: 64 &lt;= přenosovou rychlostí &lt;= 320<br/><br/> -88200: 256 &lt;= přenosovou rychlostí &lt;= 320<br/><br/> -96000: 256 &lt;= přenosovou rychlostí &lt;= 320<br/><br/> 8:<br/><br/> -32000: 96 &lt;= přenosovou rychlostí &lt;= 448<br/><br/> -44100: 96 &lt;= přenosovou rychlostí &lt;= 448<br/><br/> -48000: 96 &lt;= přenosovou rychlostí &lt;= 448<br/><br/> -88200: 384 &lt;= přenosovou rychlostí &lt;= 448<br/><br/> -96000: 384 &lt;= přenosovou rychlostí &lt;= 448  
**HEAACV2**|2:<br/><br/> -22050: 8 &lt;= přenosovou rychlostí &lt;= 10<br/><br/> -24000: 8 &lt;= přenosovou rychlostí &lt;= 10<br/><br/> -32000: 12 &lt;= přenosovou rychlostí &lt;= 64<br/><br/> -44100: 20 &lt;= přenosovou rychlostí &lt;= 64<br/><br/> -48000: 20 &lt;= přenosovou rychlostí &lt;= 64<br/><br/> -88200: 64 &lt;= přenosovou rychlostí &lt;= 64  
  

## <a name="Clip"></a>Klip
### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Čas spuštění** |**xs** |Určuje čas zahájení hello prezentace. Hodnota Hello StartTime musí vstupní videa hello toomatch časová razítka absolutní hello. Například pokud hello první snímek hello vstupní video má časovým razítkem 12:00:10.000, pak čas spuštění by mělo obsahovat aspoň 12:00:10.000 nebo vyšší. |
| **Doba trvání** |**xs** |Určuje dobu trvání hello prezentace (například vzhled překrytí v hello video). |

## <a name="Output"></a>Výstup
### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Název souboru** |**xs:String** |Název Hello hello výstupního souboru.<br/><br/> Můžete použít makra popsané v následující názvy výstupního souboru tabulky toobuild hello hello. Například:<br/><br/> **"Výstupy": [{"Název souboru": "{Basename}*{řešení}*.mp4 {přenosovou rychlostí}", "Format": {"Typ": "MP4Format"}}] ** |

### <a name="macros"></a>Makra
| – Makro | Popis |
| --- | --- |
| **{Basename}** |Při provádění VoD kódování, je nejprve 32 znaků hello AssetFile.Name vlastnosti hello primární soubor v vstupní asset hello hello hello {Basename}.<br/><br/> Pokud vstupní asset hello za provozu archivu, pak hello {Basename} je odvozená od hello trackName atributy v manifestu server hello. Pokud jste odeslali úlohu subclip pomocí hello TopBitrate, jako v: "< VideoStream\>TopBitrate < / VideoStream\>" a hello výstupní soubor obsahuje video a potom hello {Basename} je hello nejprve 32 znaků hello trackName Dobrý den vrstvu videa s nejvyšší přenosovou rychlostí hello.<br/><br/> Pokud místo toho jsou odesílání úlohu subclip pomocí všechny hello vstupní přenosových rychlostí, například "< VideoStream\>* < / VideoStream\>" a hello výstupní soubor obsahuje video a potom {Basename} je hello nejprve 32 znaků hello trackName systému Hello odpovídající vrstvu videa. |
| **{Kodeků}** |Mapuje příliš "H264" pro video a "AAC" pro zvukovém souboru. |
| **{Přenosovou rychlostí}** |Hello cíl video přenosovou rychlostí Pokud hello výstupní soubor obsahuje videa a zvuku, nebo zvuk přenosovou rychlostí cíl Pokud hello výstup souboru obsahuje pouze zvuk. Hello používá, je hello přenosovou rychlostí v kb/s. |
| **{Kanál}** |Počet zvukových kanál Pokud hello soubor obsahuje zvukovém souboru. |
| **{Šířka}** |Šířka hello video, v pixelech, ve výstupním souboru hello, pokud soubor hello obsahuje video. |
| **{Výška}** |Výška hello video, v pixelech, ve výstupním souboru hello, pokud soubor hello obsahuje video. |
| **{Rozšíření}** |Dědí od hello vlastnost "Type" pro hello výstupní soubor. název výstupního souboru Hello bude mít příponu, což je jedna z: "mp4", "ts", "jpg", "png" nebo "bmp". |
| **{Index}** |Povinné pro miniaturu. Mají jenom přítomen jednou. |

## <a name="Video"></a>Video (komplexní typ dědí od kodeků)
### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Start** |**xs:String** | |
| **Krok** |**xs:String** | |
| **Rozsah** |**xs:String** | |
| **PreserveResolutionAfterRotation** |**xs:Boolean** |Podrobné vysvětlení, najdete v následující části hello: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a>PreserveResolutionAfterRotation
Je doporučeno toouse hello PreserveResolutionAfterRotation příznak v kombinaci s hodnotami řešení vyjádřené v procentech (Width = "100 %", výška = "100 %").  

Ve výchozím nastavení zakódovat hello nastavení překladu (šířka a výška) v hello přednastavení Media Encoder Standard (MES) se budou zaměřovat na videa s otočení 0 stupňů. Například pokud vaše vstupní video 1280 × 720 nulové stupeň otáčení, pak hello výchozí přednastavení zajistěte, aby hello výstup hello stejné řešení. Viz následující obrázek.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Nicméně to znamená, že pokud vstupní video hello se zaznamenala s nenulovou hodnotou otočení (např. tablet nebo smartphone uchovávat svisle), pak MES ve výchozím nastavení budou platit hello kódování řešení nastavení (šířka a výška) toohello vstupní video a potom kompenzovat otočení hello. Například viz následující obrázek hello. Hello přednastavených používá Width = "100 %", výška = "100 %", který MES interpretuje jako vyžadování hello výstup toobe 1 280 pixelů na šířku a výšku 720 pixelů. Po rotaci hello video, pak zaplňování hello obrázek toofit do tohoto okna a úvodní oblasti toopillar pole na hello vlevo a vpravo.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Pokud hello výše není hello požadovaného chování, pak provedete použití hello PreserveResolutionAfterRotation příznak a nastavte ji příliš "PRAVDA" (výchozí hodnota je "false"). Takže pokud vaše přednastavené má šířku = "100 %", výška = "100 %" a nastavte příliš "PRAVDA", vstupní video, který je 1 280 pixelů a 720 pixelů výšku s 90 stupeň otočení vytvoří výstup s nulové otočení míry, ale 720 pixelů široké a 1280 PreserveResolutionAfterRotation Vysoký pixelů. Viz následující obrázek hello.  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a>FormatGroup (skupiny)
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a>BmpLayer
### <a name="element"></a>Element
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Šířka**<br/><br/> Hodnota minOccurs = "0" |**xs:int** | |
| **Výška**<br/><br/> Hodnota minOccurs = "0" |**xs:int** | |

### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Podmínka** |**xs:String** | |

## <a name="PngLayer"></a>PngLayer
### <a name="element"></a>Element
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Šířka**<br/><br/> Hodnota minOccurs = "0" |**xs:int** | |
| **Výška**<br/><br/> Hodnota minOccurs = "0" |**xs:int** | |

### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Podmínka** |**xs:String** | |

## <a name="JpgLayer"></a>JpgLayer
### <a name="element"></a>Element
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Šířka**<br/><br/> Hodnota minOccurs = "0" |**xs:int** | |
| **Výška**<br/><br/> Hodnota minOccurs = "0" |**xs:int** | |
| **Kvality**<br/><br/> Hodnota minOccurs = "0" |**xs:int** |Platné hodnoty: 1(worst)-100(best) |

### <a name="attributes"></a>Atributy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **Podmínka** |**xs:String** | |

## <a name="PngLayers"></a>PngLayers
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **PngLayer**<br/><br/> Hodnota minOccurs = maxOccurs "0" = "bez vazby" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a>BmpLayers
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **BmpLayer**<br/><br/> Hodnota minOccurs = maxOccurs "0" = "bez vazby" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a>JpgLayers
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **JpgLayer**<br/><br/> Hodnota minOccurs = maxOccurs "0" = "bez vazby" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a>BmpImage (komplexní typ dědí od Video)
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **PngLayers**<br/><br/> Hodnota minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG vrstev |

## <a name="JpgImage"></a>JpgImage (komplexní typ dědí od Video)
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **PngLayers**<br/><br/> Hodnota minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG vrstev |

## <a name="PngImage"></a>PngImage (komplexní typ dědí od Video)
### <a name="elements"></a>Elementy
| Name (Název) | Typ | Popis |
| --- | --- | --- |
| **PngLayers**<br/><br/> Hodnota minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG vrstev |

## <a name="examples"></a>Příklady
Najdete příklady přednastavení XML, které jsou vytvořené na základě této schématu, najdete v části [přednastavení úloh pro MES (Media Encoder Standard)](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

