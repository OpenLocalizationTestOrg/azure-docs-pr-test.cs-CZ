---
title: "aaaMultiple vstupní soubory a vlastnosti komponenty pomocí kodéru Premium - Azure | Microsoft Docs"
description: "Toto téma vysvětluje, jak toouse setRuntimeProperties toouse více vstupní soubory a předat procesor médií Media Encoder Premium pracovního postupu toohello vlastní data."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: e14d10fbf9669e0b88e5ba1c519f1ba5e0bafdd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Použití více vstupní soubory a vlastnosti komponenty s kodér úrovně Premium
## <a name="overview"></a>Přehled
Existují scénáře, ve kterých může být nutné toocustomize vlastnosti komponenty, zadejte klip seznamu XML obsahu nebo odeslat více vstupních souborů při odesílání úlohy hello **Media Encoder Premium pracovního postupu** procesor médií. Tady je několik příkladů:

* Překrytí text na video a nastavení v době běhu pro každý vstupní video hello textové hodnoty (například hello aktuální datum).
* Přizpůsobení hello klip seznamu XML (toospecify jednu nebo několik zdrojových souborů, s nebo bez oříznutí, atd.).
* Obrázek loga překrytí na hello vstupní video, při hello video je zakódován.
* Více kódování zvuk jazyka.

toolet hello **Media Encoder Premium pracovního postupu** vědět, že chcete změnit některé vlastnosti v pracovním postupu hello při vytvoření úlohy hello nebo odeslání více vstupní soubory, máte toouse konfigurace řetězec, který obsahuje ** setRuntimeProperties** nebo **transcodeSource**. Toto téma vysvětluje, jak toouse je.

## <a name="configuration-string-syntax"></a>Konfigurace řetězec syntaxe
Hello konfigurační řetězec tooset v hello kódování úloh používá dokument XML, který vypadá takto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

Hello následuje hello C# kód, který čte konfigurační soubor XML hello ze souboru, aktualizujte jej s hello právo video filename a předává je toohello úloh pro úlohu:

```c#
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass tooit hello name of hello 
// processor toouse for hello specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with hello encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify hello input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset toocontain hello results of hello job. 
// This output is specified as AssetCreationOptions.None, which 
// means hello output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a>Přizpůsobení vlastnosti komponenty
### <a name="property-with-a-simple-value"></a>Vlastnost s hodnotou jednoduché
V některých případech je užitečné toocustomize vlastnost součásti společně s hello souboru pracovního postupu, který bude toobe provedený Media Encoder Premium pracovního postupu.

Předpokládejme, že určený pracovního postupu tento text překryvy videa a textu hello (například hello aktuální data) by měla toobe sadu za běhu. To provedete odesláním toobe textu hello nastavit jako hello novou hodnotu pro vlastnost textu hello hello překrytí součásti od hello kódování úloh. Tento mechanismus toochange můžete použít další vlastnosti komponenty v pracovním postupu hello (například hello pozici nebo barvu hello překrytí, přenosovou rychlostí hello hello AVC kodér atd.).

**setRuntimeProperties** je použité toooverride vlastnost v součásti hello hello pracovního postupu.

Příklad:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text tooImage Converter/text" value="Today is Friday hello 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a>Vlastnost s hodnotou XML
zapouzdření tooset vlastnost, která očekává hodnotu XML pomocí `<![CDATA[ and ]]>`.

Příklad:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> Zajistěte, aby tooput znaků CR vrátit pouze po `<![CDATA[`.

### <a name="propertypath-value"></a>Hodnota propertyPath
V předchozích příkladech hello, byla hello propertyPath "/ souborů médií vstup/filename" nebo "/ inactiveTimeout" nebo "clipListXml".
Toto je, obecně hello názvu komponenty hello pak hello název vlastnosti hello. Hello cesta může mít více nebo méně úrovně, jako je třeba "/ primarySourceFile" (protože hello vlastnost je v kořenovém hello hello pracovního postupu) nebo "/ Video zpracování nebo obrázek překrytí nebo krytí" (protože hello překrytí se nachází ve skupině).    

toocheck hello cesty a názvu vlastnosti, použijte hello akce tlačítka, které je okamžitě vedle každou vlastnost. Můžete kliknutím na toto tlačítko akce a vybrat **upravit**. Tato rutina ukáže vám hello skutečný název vlastnosti hello a okamžitě nad ním, hello oboru názvů.

![Akce či upravit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Vlastnost](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Více vstupní soubory
Každý úkol, odešlete toohello **Media Encoder Premium pracovního postupu** vyžaduje dva prostředky:

* Hello nejprve jeden je *pracovního postupu Asset* obsahující soubor pracovního postupu. Soubory pracovního postupu můžete navrhnout pomocí hello [Návrhář postupu provádění](media-services-workflow-designer.md).
* je technologie Hello druhá *média Asset* obsahující soubory hello média, které chcete tooencode.

Pokud odesíláte více toohello soubory médií **Media Encoder Premium pracovního postupu** použít kodér, hello následující omezení:

* Všechna média hello soubory musí být ve stejné hello *média Asset*. Použití více prostředků média není podporováno.
* V tento prostředek média je nutné nastavit primární soubor hello (v ideálním případě jde hello hlavní soubor videa, který hello kodér se zobrazí výzva, tooprocess).
* Je nezbytné toopass konfigurační data, která zahrnuje hello **setRuntimeProperties** nebo **transcodeSource** element toohello procesoru.
  * **setRuntimeProperties** je vlastnost filename hello použité toooverride nebo jinou vlastnost v součásti hello hello pracovního postupu.
  * **transcodeSource** je použité toospecify hello obsah klip seznamu XML.

Připojení v pracovním postupu hello:

* Pokud je použití jednoho nebo několika komponent vstupní soubor média a naplánujte toouse **setRuntimeProperties** toospecify hello název souboru a pak se nepřipojují hello primární soubor součást pin toothem. Ujistěte se, že neexistuje žádné připojení mezi objektu hello primární soubor a hello Input(s) soubor média.
* Pokud dáváte přednost toouse klip seznamu XML a jedna součást zdrojové médium, pak se můžete připojit i společně.

![Žádné připojení z primární zdrojový soubor tooMedia vstupní soubor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Pokud použijete vlastnost filename hello tooset setRuntimeProperties neexistuje žádné připojení z hello primární soubor tooMedia vstupní soubor součásti.*

![Připojení ze seznamu XML klip tooClip zdrojového seznamu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Můžete připojit klip seznamu XML tooMedia zdroje a použít transcodeSource.*

### <a name="clip-list-xml-customization"></a>Oříznutí seznamu XML přizpůsobení
Můžete zadat hello klip seznamu XML v pracovním postupu hello za běhu pomocí **transcodeSource** v konfiguraci hello řetězce XML. To vyžaduje hello klip seznamu XML pin toobe připojené toohello zdrojové médium součást v pracovním postupu hello.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Pokud chcete toouse /primarySourceFile toospecify tento výstup hello tooname vlastnost soubory pomocí "Výrazy", pak doporučujeme předávání hello klip seznamu XML jako vlastnost *po* hello /primarySourceFile vlastnost, tooavoid s hello klip seznamu přepsat nastavení /primarySourceFile hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

S další rámce přesné oříznutí:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-hello-video"></a>Příklad 1: Překrytí bitovou kopii nad hello video

### <a name="presentation"></a>Prezentace
Zvažte příklad, ve kterém chcete toooverlay obrázek loga na hello vstupní video při hello video je zakódován. V tomto příkladu hello vstupní video má název "Microsoft_HoloLens_Possibilities_816p24.mp4" a hello logo je s názvem "logo.png". Měli byste provést hello následující kroky:

* Vytvoření prostředku pracovního postupu s soubor pracovního postupu hello (viz následující ukázka hello).
* Vytvoření média prostředku, který obsahuje dva soubory: MyInputVideo.mp4 jako hello primární soubor a MyLogo.png.
* Odesílat toohello úlohy pracovního postupu Premium Media Encoder média prostředky procesoru s hello výše vstup a zadejte následující konfigurační řetězec hello.

Konfigurace:

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

V předchozím příkladu hello je odeslána hello název hello videosoubor toohello vstupní soubor média součásti a hello primarySourceFile vlastnost. Hello název souboru logo hello se odesílají tooanother vstup souboru média, který je připojený toohello součást grafické překrytí.

> [!NOTE]
> Název souboru videa Hello se odesílají toohello primarySourceFile vlastnost. Hello důvodem je tato vlastnost v hello pracovního postupu pro vytváření název souboru správný výstup hello pomocí výrazů, například toouse.

### <a name="step-by-step-workflow-creation"></a>Vytvoření pracovního postupu krok za krokem
Tady jsou kroky toocreate hello pracovní postup, který přebírá dva soubory jako vstup: video a bitovou kopii. Ho bude překrytí hello image nad hello videa.

Otevřete **Návrhář postupu provádění** a vyberte **soubor** > **nový pracovní prostor** > **převod plán, podle kterého**.

nový pracovní postup Hello ukazuje tři prvky:

* Primární zdrojový soubor
* Seznam klip ve formátu XML
* Výstupní soubor nebo prostředek  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nový pracovní postup kódování*

V pořadí tooaccept hello vstupními médii souboru začněte přidáte komponentu vstupní soubor média. tooadd toohello součást pracovního postupu, podívejte se do vyhledávacího pole úložiště hello a přetáhněte hello potřeby položku na návrháře podokně hello.

Dál přidejte toobe videosoubor hello používá pro navrhování pracovního postupu. toodo tak, klikněte na podokno pozadí hello v Návrháři pracovních postupů a vyhledejte hello primární zdrojový soubor vlastnosti v podokně pravém vlastnost hello. Klikněte na ikonu hello složky a vyberte příslušné videosoubor hello.

![Primární soubor zdroje](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primární soubor zdroje*

Potom zadejte hello videosoubor v komponentě hello vstupní soubor média.   

![Vstupní zdroj souborů médií](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Vstupní zdroj souborů médií*

Jakmile to uděláte, bude součást vstupní soubor média hello zkontrolujte soubor hello a naplnit její výstup PIN tooreflect hello soubor, který ho prověřovány.

dalším krokem Hello je tooadd tooRec.709 místo k barva "Aktualizační Video datového typu" toospecify hello. Přidání "Video formátu převaděč" nastavený typ rozložení a rozložení tooData = konfigurovat planární. Tato funkce převede hello datový proud videa tooa formátu, který může být přijata jako zdroj hello překrytí součásti.

![Video aktualizační datový typ a formát převaděč](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Aktualizační video datový typ a formát převaděč*

![Typ rozložení = konfigurovat planární](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Je-li konfigurovat planární typ rozložení*

V dalším kroku přidejte součást Video překrytí a připojte hello (nekomprimované) video pin toohello (nekomprimované) video pin vstupní soubor média hello.

Přidejte jiný média vstupní soubor (tooload hello logo soubor), klikněte na tuto součást a přejmenujte ji příliš "Média souboru vstup Logo" a vyberte bitovou kopii (soubor .png např.) v souboru vlastnost hello. Připojte hello nekomprimované image pin toohello nekomprimované image pin hello překrytí.

![Zdroj překrytí součásti a obrázkových souborů](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Zdroj překrytí součásti a obrázkových souborů*

Pokud chcete umístění hello toomodify hello logo na hello video (například chtít tooposition ho na 10 procent z horní části hello levého horního rohu videa hello), zrušte zaškrtnutí políčka "Ruční vstup" hello. To můžete provést, protože používáte komponentu vstupní soubor média tooprovide hello logo souboru toohello překrytí.

![Překrytí pozice](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Překrytí pozice*

tooencode hello tooH.264 datový proud videa, přidejte hello AVC kodér videa a AAC kodér součásti toohello návrháře prostor. Připojte hello PIN.
Nastavit hello AAC kodér a vybrat přednastavení převodu formátu zvuk: 2.0 (L, R).

![Audio a Video kodéry](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio a Video kodéry*

Nyní přidejte hello **ISO Mpeg-4 multiplexor** a **výstup souboru** součásti a připojte PIN hello, jak je znázorněno.

![MP4 multiplexor a výstupní soubor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexor a výstupní soubor*

Je třeba název hello tooset hello výstupní soubor. Klikněte na tlačítko hello **výstup souboru** hello výraz součásti a úpravy souboru hello:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Název výstupního souboru](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Název výstupního souboru*

Můžete spustit pracovní postup hello místně toocheck, zda je správně spuštěn.

Po jejím dokončení, můžete ho spustit v Azure Media Services.

Nejprve připravte ke dvěma souborům se prostředek ve službě Azure Media Services: hello videosoubor a hello logo. To provedete pomocí hello .NET nebo REST API. Můžete to provést taky pomocí hello portál Azure nebo [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Tento kurz ukazuje, jak toomanage prostředky s AMSE. Existují dva způsoby tooadd soubory tooan asset:

* Vytvořte místní složku, zkopírujte hello dva soubory v něm a přetažení hello složky toohello **Asset** kartě.
* Nahrát videosoubor hello jako prostředek, zobrazit informace o evidenčním hello, přejděte toohello kartě soubory a odeslat další soubor (logo).

> [!NOTE]
> Ujistěte se, že tooset primární soubor v hello asset (hello hlavní video soubor).

![Soubory prostředků v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Soubory prostředků v AMSE*

Vyberte hello asset a zvolte tooencode ho pomocí kodéru Premium. Nahrát hello pracovního postupu a vyberte jej.

Klikněte na tlačítko hello, toopass procesor toohello dat a přidejte následující XML tooset hello runtime vlastnosti hello:

![Kodér úrovně Premium v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Kodér úrovně Premium v AMSE*

Vložte následující XML data hello. Potřebujete název hello toospecify hello videosoubor hello vstupní soubor média a primarySourceFile. Zadejte název hello hello název souboru pro hello logo příliš.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*

Pokud používáte toocreate hello .NET SDK a spusťte úlohu hello, má tato data XML toobe předány jako hello konfigurační řetězec.

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

Po dokončení úlohy hello výstup hello MP4 souboru v hello asset zobrazí hello překrytí!

![Překrytí na hello video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Překrytí na hello video*

Můžete si stáhnout hello ukázkového pracovního postupu z [Githubu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>Příklad 2: Více zvuk jazyk kódování

Příkladem zvuk vícejazyčné kódování workfkow je k dispozici v [Githubu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Tato složka obsahuje ukázkový pracovní postup, který lze použít tooencode MXF souboru tooa více MP4 soubory asset s více zvukových stop.

Tento pracovní postup předpokládá, že soubor MXF hello obsahuje jeden zvuk sledovat; Další zvukových stop Hello mají být předány jako samostatné zvukové soubory (WAV nebo MP4...).

tooencode, postupujte podle těchto kroků:

* Vytvoření prostředku služby Media Services s hello MXF souboru a hello zvukové soubory (0 zvukové soubory too18).
* Ujistěte se, že tento soubor MXF hello je nastaven jako primární soubor.
* Vytvoření úlohy a úlohy pomocí hello kodér úrovně Premium pracovní postup procesoru. Použití pracovního postupu hello zadaný (MultiMP4-1080p-19audio-v1.workflow).
* Předejte hello setruntime.xml data toohello úloh (Pokud používáte Azure Media Services Explorer, "předat pracovní postup xml data toohello" hello použití – tlačítko).
  * Aktualizujte prosím hello data toospecify hello správný soubor názvy a jazyky značky XML.
  * pracovní postup Hello obsahuje zvuk komponenty s názvem tooAudio zvuk 1 18.
  * RFC5646 je podporována pro značky jazyka hello.

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* Hello kódovaný asset bude obsahovat více jazyk zvukových stop a musí být tyto sleduje vybrat v Azure Media Player.

## <a name="see-also"></a>Viz také
* [Představení Premium kódování v Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Jak toouse Premium kódování ve službě Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Kódování obsahu na vyžádání pomocí Azure Media Services](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Formáty Media Encoder Premium pracovního postupu a kodeky](media-services-premium-workflow-encoder-formats.md)
* [Ukázkové soubory pracovního postupu](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [Nástroj pro Azure Media Services Explorer](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
