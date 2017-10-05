---
title: "Více vstupní soubory a vlastnosti komponenty pomocí kodéru Premium - Azure | Microsoft Docs"
description: "Toto téma vysvětluje, jak používat setRuntimeProperties k použití více vstupní soubory a předat vlastní data procesor médií Media Encoder Premium pracovního postupu."
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
ms.openlocfilehash: df1ee5089a0af6ffce1431b658843fcb34a66ce5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a><span data-ttu-id="19155-103">Použití více vstupní soubory a vlastnosti komponenty s kodér úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="19155-103">Using multiple input files and component properties with Premium Encoder</span></span>
## <a name="overview"></a><span data-ttu-id="19155-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="19155-104">Overview</span></span>
<span data-ttu-id="19155-105">Existují scénáře, ve kterých možná budete muset upravit vlastnosti komponenty, zadejte klip seznamu XML obsahu nebo odeslat více vstupních souborů při odesílání úlohy **Media Encoder Premium pracovního postupu** procesor médií.</span><span class="sxs-lookup"><span data-stu-id="19155-105">There are scenarios in which you might need to customize component properties, specify Clip List XML content, or send multiple input files when you submit a task with the **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="19155-106">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="19155-106">Some examples are:</span></span>

* <span data-ttu-id="19155-107">Překrytí text na video a nastavení v době běhu pro každý vstupní video textové hodnoty (například aktuální datum).</span><span class="sxs-lookup"><span data-stu-id="19155-107">Overlaying text on video and setting the text value (for example, the current date) at runtime for each input video.</span></span>
* <span data-ttu-id="19155-108">Přizpůsobení seznamu XML klip (tak, aby zadejte jednu nebo několik zdrojových souborů, s nebo bez oříznutí, atd.).</span><span class="sxs-lookup"><span data-stu-id="19155-108">Customizing the Clip List XML (to specify one or several source files, with or without trimming, etc.).</span></span>
* <span data-ttu-id="19155-109">Obrázek loga překrytí na vstup videa, při přehrávání videa je zakódován.</span><span class="sxs-lookup"><span data-stu-id="19155-109">Overlaying a logo image on the input video while the video is encoded.</span></span>
* <span data-ttu-id="19155-110">Více kódování zvuk jazyka.</span><span class="sxs-lookup"><span data-stu-id="19155-110">Multiple audio language encoding.</span></span>

<span data-ttu-id="19155-111">Umožníte **Media Encoder Premium pracovního postupu** vědět, že chcete změnit některé vlastnosti v pracovním postupu při vytvoření úlohy nebo odeslat více vstupní soubory, budete muset použít konfigurační řetězec, který obsahuje  **setRuntimeProperties** nebo **transcodeSource**.</span><span class="sxs-lookup"><span data-stu-id="19155-111">To let the **Media Encoder Premium Workflow** know that you are changing some properties in the workflow when you create the task or send multiple input files, you have to use a configuration string that contains **setRuntimeProperties** and/or **transcodeSource**.</span></span> <span data-ttu-id="19155-112">Toto téma vysvětluje, jak je používat.</span><span class="sxs-lookup"><span data-stu-id="19155-112">This topic explains how to use them.</span></span>

## <a name="configuration-string-syntax"></a><span data-ttu-id="19155-113">Konfigurace řetězec syntaxe</span><span class="sxs-lookup"><span data-stu-id="19155-113">Configuration string syntax</span></span>
<span data-ttu-id="19155-114">Konfigurace řetězec, který nastavit v kódování úloh používá dokument XML, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="19155-114">The configuration string to set in the encoding task uses an XML document that looks like this:</span></span>

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

<span data-ttu-id="19155-115">Následuje kód C#, který čte konfigurační soubor XML ze souboru, jej aktualizovat s právo video název souboru a předává je na úlohu v rámci úlohy:</span><span class="sxs-lookup"><span data-stu-id="19155-115">The following is the C# code that reads the XML configuration from a file, update it with the right video filename and passes it to the task in a job:</span></span>

```c#
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass to it the name of the 
// processor to use for the specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with the encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify the input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset to contain the results of the job. 
// This output is specified as AssetCreationOptions.None, which 
// means the output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a><span data-ttu-id="19155-116">Přizpůsobení vlastnosti komponenty</span><span class="sxs-lookup"><span data-stu-id="19155-116">Customizing component properties</span></span>
### <a name="property-with-a-simple-value"></a><span data-ttu-id="19155-117">Vlastnost s hodnotou jednoduché</span><span class="sxs-lookup"><span data-stu-id="19155-117">Property with a simple value</span></span>
<span data-ttu-id="19155-118">V některých případech je užitečné, chcete-li přizpůsobit vlastnosti součásti společně s soubor pracovního postupu, který chcete provést, v pracovním postupu Premium kodér médií.</span><span class="sxs-lookup"><span data-stu-id="19155-118">In some cases, it is useful to customize a component property together with the workflow file that is going to be executed by Media Encoder Premium Workflow.</span></span>

<span data-ttu-id="19155-119">Předpokládejme, že určený pracovního postupu tento text překryvy videa a text (například aktuální datum) by měla být nastavena v době běhu.</span><span class="sxs-lookup"><span data-stu-id="19155-119">Suppose you designed a workflow that overlays text on your videos, and the text (for example, the current date) is supposed to be set at runtime.</span></span> <span data-ttu-id="19155-120">To provedete odesláním text, který má být nastavená jako novou hodnotu pro vlastnost text překrytí součásti z úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="19155-120">You can do this by sending the text to be set as the new value for the text property of the overlay component from the encoding task.</span></span> <span data-ttu-id="19155-121">Tento mechanismus můžete změnit další vlastnosti komponenty v pracovním postupu (například pozici nebo barvu překrytí, přenosovou rychlostí AVC kodér atd.).</span><span class="sxs-lookup"><span data-stu-id="19155-121">You can use this mechanism to change other properties of a component in the workflow (such as the position or color of the overlay, the bitrate of the AVC encoder, etc.).</span></span>

<span data-ttu-id="19155-122">**setRuntimeProperties** se používá k přepsání na vlastnost ve součásti pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="19155-122">**setRuntimeProperties** is used to override a property in the components of the workflow.</span></span>

<span data-ttu-id="19155-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="19155-123">Example:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a><span data-ttu-id="19155-124">Vlastnost s hodnotou XML</span><span class="sxs-lookup"><span data-stu-id="19155-124">Property with an XML value</span></span>
<span data-ttu-id="19155-125">Pokud chcete nastavit vlastnost, která očekává hodnoty XML, zapouzdření pomocí `<![CDATA[ and ]]>`.</span><span class="sxs-lookup"><span data-stu-id="19155-125">To set a property that expects an XML value, encapsulate by using `<![CDATA[ and ]]>`.</span></span>

<span data-ttu-id="19155-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="19155-126">Example:</span></span>

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
> <span data-ttu-id="19155-127">Ujistěte se, není pro umístění znaků CR návratový právě po `<![CDATA[`.</span><span class="sxs-lookup"><span data-stu-id="19155-127">Make sure not to put a carriage return just after `<![CDATA[`.</span></span>

### <a name="propertypath-value"></a><span data-ttu-id="19155-128">Hodnota propertyPath</span><span class="sxs-lookup"><span data-stu-id="19155-128">propertyPath value</span></span>
<span data-ttu-id="19155-129">V předchozích příkladech propertyPath byla "/ souborů médií vstup/filename" nebo "/ inactiveTimeout" nebo "clipListXml".</span><span class="sxs-lookup"><span data-stu-id="19155-129">In the previous examples, the propertyPath was "/Media File Input/filename" or "/inactiveTimeout" or "clipListXml".</span></span>
<span data-ttu-id="19155-130">Toto je, obecně platí, názvu součásti a potom název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="19155-130">This is, in general, the name of the component, then the name of the property.</span></span> <span data-ttu-id="19155-131">Cesta může mít více nebo méně úrovně, jako je třeba "/ primarySourceFile" (protože vlastnost je v kořenovém adresáři pracovního postupu) nebo "/ Video zpracování nebo obrázek překrytí nebo krytí" (protože překrytí se nachází ve skupině).</span><span class="sxs-lookup"><span data-stu-id="19155-131">The path can have more or fewer levels, like "/primarySourceFile" (because the property is at the root of the workflow) or "/Video Processing/Graphic Overlay/Opacity" (because the Overlay is in a group).</span></span>    

<span data-ttu-id="19155-132">Chcete-li zkontrolovat název a cesta k vlastnosti, použijte tlačítko akce, která se nachází bezprostředně vedle každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="19155-132">To check the path and property name, use the action button that is immediately beside each property.</span></span> <span data-ttu-id="19155-133">Můžete kliknutím na toto tlačítko akce a vybrat **upravit**.</span><span class="sxs-lookup"><span data-stu-id="19155-133">You can click this action button and select **Edit**.</span></span> <span data-ttu-id="19155-134">To vám ukáže, skutečný název vlastnosti a okamžitě nad ním, obor názvů.</span><span class="sxs-lookup"><span data-stu-id="19155-134">This will show you the actual name of the property, and immediately above it, the namespace.</span></span>

![Akce či upravit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Vlastnost](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a><span data-ttu-id="19155-137">Více vstupní soubory</span><span class="sxs-lookup"><span data-stu-id="19155-137">Multiple input files</span></span>
<span data-ttu-id="19155-138">Každý úkol, který se odešle **Media Encoder Premium pracovního postupu** vyžaduje dva prostředky:</span><span class="sxs-lookup"><span data-stu-id="19155-138">Each task that you submit to the **Media Encoder Premium Workflow** requires two assets:</span></span>

* <span data-ttu-id="19155-139">První z nich je *pracovního postupu Asset* obsahující soubor pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="19155-139">The first one is a *Workflow Asset* that contains a workflow file.</span></span> <span data-ttu-id="19155-140">Soubory pracovního postupu můžete navrhnout pomocí [Návrhář postupu provádění](media-services-workflow-designer.md).</span><span class="sxs-lookup"><span data-stu-id="19155-140">You can design workflow files by using the [Workflow Designer](media-services-workflow-designer.md).</span></span>
* <span data-ttu-id="19155-141">Je druhá *média Asset* obsahující soubory médií, který chcete kódovat.</span><span class="sxs-lookup"><span data-stu-id="19155-141">The second one is a *Media Asset* that contains the media file(s) that you want to encode.</span></span>

<span data-ttu-id="19155-142">Pokud odesíláte média soubory, které mají **Media Encoder Premium pracovního postupu** kodér, platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="19155-142">When you're sending multiple media files to the **Media Encoder Premium Workflow** encoder, the following constraints apply:</span></span>

* <span data-ttu-id="19155-143">Všechny soubory média musí být ve stejné *média Asset*.</span><span class="sxs-lookup"><span data-stu-id="19155-143">All the media files must be in the same *Media Asset*.</span></span> <span data-ttu-id="19155-144">Použití více prostředků média není podporováno.</span><span class="sxs-lookup"><span data-stu-id="19155-144">Using multiple Media Assets is not supported.</span></span>
* <span data-ttu-id="19155-145">Je nutné nastavit primární soubor v tento prostředek média (v ideálním případě by toto je hlavní video soubor, který je kodér požadavku na zpracování).</span><span class="sxs-lookup"><span data-stu-id="19155-145">You must set the primary file in this Media Asset (ideally, this is the main video file that the encoder is asked to process).</span></span>
* <span data-ttu-id="19155-146">Je nutné předat konfigurační data, která zahrnuje **setRuntimeProperties** nebo **transcodeSource** element pro procesor.</span><span class="sxs-lookup"><span data-stu-id="19155-146">It is necessary to pass configuration data that includes the **setRuntimeProperties** and/or **transcodeSource** element to the processor.</span></span>
  * <span data-ttu-id="19155-147">**setRuntimeProperties** se používá k přepsání vlastnost název souboru nebo jinou vlastnost v součásti pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="19155-147">**setRuntimeProperties** is used to override the filename property or another property in the components of the workflow.</span></span>
  * <span data-ttu-id="19155-148">**transcodeSource** slouží k určení obsahu klip seznamu XML.</span><span class="sxs-lookup"><span data-stu-id="19155-148">**transcodeSource** is used to specify the Clip List XML content.</span></span>

<span data-ttu-id="19155-149">Připojení v pracovním postupu:</span><span class="sxs-lookup"><span data-stu-id="19155-149">Connections in the workflow:</span></span>

* <span data-ttu-id="19155-150">Pokud je použití jednoho nebo několika komponent vstupní soubor média a plánujete používat **setRuntimeProperties** Pokud chcete zadat název souboru, pak se nepřipojují pin součást primární soubor k nim.</span><span class="sxs-lookup"><span data-stu-id="19155-150">If you use one or several Media File Input components and plan to use **setRuntimeProperties** to specify the file name, then do not connect the primary file component pin to them.</span></span> <span data-ttu-id="19155-151">Ujistěte se, že neexistuje žádné připojení mezi objektem primární soubor a Input(s) soubor média.</span><span class="sxs-lookup"><span data-stu-id="19155-151">Make sure that there is no connection between the primary file object and the Media File Input(s).</span></span>
* <span data-ttu-id="19155-152">Pokud byste radši chtěli použít klip seznamu XML a jedna součást zdrojové médium, pak se můžete připojit i společně.</span><span class="sxs-lookup"><span data-stu-id="19155-152">If you prefer to use Clip List XML and one Media Source component, then you can connect both together.</span></span>

![Žádné připojení z primární zdrojový soubor pro vstupní soubor média](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

<span data-ttu-id="19155-154">*Neexistuje žádné připojení z primární soubor do součásti vstupní soubor média, pokud používáte setRuntimeProperties set pro vlastnost název souboru.*</span><span class="sxs-lookup"><span data-stu-id="19155-154">*There is no connection from the primary file to Media File Input component(s) if you use setRuntimeProperties to set the filename property.*</span></span>

![Připojení ze seznamu klip XML tak, aby v případě potřeby upraví zdrojového seznamu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

<span data-ttu-id="19155-156">*Můžete připojit klip seznamu XML pro zdroj média a použít transcodeSource.*</span><span class="sxs-lookup"><span data-stu-id="19155-156">*You can connect Clip List XML to Media Source and use transcodeSource.*</span></span>

### <a name="clip-list-xml-customization"></a><span data-ttu-id="19155-157">Oříznutí seznamu XML přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="19155-157">Clip List XML customization</span></span>
<span data-ttu-id="19155-158">Můžete zadat seznam XML klip v pracovním postupu za běhu pomocí **transcodeSource** v konfiguraci řetězce XML.</span><span class="sxs-lookup"><span data-stu-id="19155-158">You can specify the Clip List XML in the workflow at runtime by using **transcodeSource** in the configuration string XML.</span></span> <span data-ttu-id="19155-159">To vyžaduje PIN kód XML seznamu klip k připojení k komponentu zdroj média v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="19155-159">This requires the Clip List XML pin to be connected to the Media Source component in the workflow.</span></span>

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

<span data-ttu-id="19155-160">Pokud chcete určit /primarySourceFile pomocí této vlastnosti názvu výstupní soubory pomocí "Výrazy", pak doporučujeme předávání XML seznamu klip jako vlastnost *po* /primarySourceFile vlastnost, abyste nemuseli klip Seznam možné přepsat nastavení /primarySourceFile.</span><span class="sxs-lookup"><span data-stu-id="19155-160">If you want to specify /primarySourceFile to use this property to name the output files by using 'Expressions', then we recommend passing the Clip List XML as a property *after* the /primarySourceFile property, to avoid having the Clip List be overridden by the /primarySourceFile setting.</span></span>

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

<span data-ttu-id="19155-161">S další rámce přesné oříznutí:</span><span class="sxs-lookup"><span data-stu-id="19155-161">With additional frame-accurate trimming:</span></span>

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

## <a name="example-1--overlay-an-image-on-top-of-the-video"></a><span data-ttu-id="19155-162">Příklad 1: Překrytí bitovou kopii na video</span><span class="sxs-lookup"><span data-stu-id="19155-162">Example 1 : Overlay an image on top of the video</span></span>

### <a name="presentation"></a><span data-ttu-id="19155-163">Prezentace</span><span class="sxs-lookup"><span data-stu-id="19155-163">Presentation</span></span>
<span data-ttu-id="19155-164">Zvažte příklad, ve kterém chcete překrýt obrázek loga na vstup videa při přehrávání videa je zakódován.</span><span class="sxs-lookup"><span data-stu-id="19155-164">Consider an example in which you want to overlay a logo image on the input video while the video is encoded.</span></span> <span data-ttu-id="19155-165">V tomto příkladu vstupní video má název "Microsoft_HoloLens_Possibilities_816p24.mp4" a logo je s názvem "logo.png".</span><span class="sxs-lookup"><span data-stu-id="19155-165">In this example, the input video is named "Microsoft_HoloLens_Possibilities_816p24.mp4" and the logo is named "logo.png".</span></span> <span data-ttu-id="19155-166">Měli byste provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="19155-166">You should perform the following steps:</span></span>

* <span data-ttu-id="19155-167">Vytvoření prostředku pracovního postupu s soubor pracovního postupu (viz následující příklad).</span><span class="sxs-lookup"><span data-stu-id="19155-167">Create a Workflow Asset with the workflow file (see the following example).</span></span>
* <span data-ttu-id="19155-168">Vytvoření média prostředku, který obsahuje dva soubory: MyInputVideo.mp4 jako primární soubor a MyLogo.png.</span><span class="sxs-lookup"><span data-stu-id="19155-168">Create a Media Asset, which contains two files: MyInputVideo.mp4 as the primary file and MyLogo.png.</span></span>
* <span data-ttu-id="19155-169">Odeslat úlohu procesor médií Media Encoder Premium pracovního postupu s výše vstupní prostředky a zadat následující řetězec konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19155-169">Send a task to the Media Encoder Premium Workflow media processor with the above input assets and specify the following configuration string.</span></span>

<span data-ttu-id="19155-170">Konfigurace:</span><span class="sxs-lookup"><span data-stu-id="19155-170">Configuration:</span></span>

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

<span data-ttu-id="19155-171">V předchozím příkladu název souboru video posílá komponentu vstupní soubor média a vlastnost primarySourceFile.</span><span class="sxs-lookup"><span data-stu-id="19155-171">In the example above, the name of the video file is sent to the Media File Input component and the primarySourceFile property.</span></span> <span data-ttu-id="19155-172">Název souboru logo se odesílají do jiné vstup souboru média, která je připojena k součásti grafické překrytí.</span><span class="sxs-lookup"><span data-stu-id="19155-172">The name of the logo file is sent to another Media File Input that is connected to the graphic overlay component.</span></span>

> [!NOTE]
> <span data-ttu-id="19155-173">Název souboru videa se odesílají do primarySourceFile vlastnost.</span><span class="sxs-lookup"><span data-stu-id="19155-173">The video file name is sent to the primarySourceFile property.</span></span> <span data-ttu-id="19155-174">Důvodem je použití této vlastnosti v pracovním postupu pro vytváření pomocí výrazů, například název souboru správný výstup.</span><span class="sxs-lookup"><span data-stu-id="19155-174">The reason for this is to use this property in the workflow for building the correct output file name using Expressions, for example.</span></span>

### <a name="step-by-step-workflow-creation"></a><span data-ttu-id="19155-175">Vytvoření pracovního postupu krok za krokem</span><span class="sxs-lookup"><span data-stu-id="19155-175">Step-by-step workflow creation</span></span>
<span data-ttu-id="19155-176">Tady jsou kroky k vytvoření pracovního postupu, který přebírá dva soubory jako vstup: video a bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="19155-176">Here are the steps to create a workflow that takes two files as input: a video and an image.</span></span> <span data-ttu-id="19155-177">Ho bude překrytí bitovou kopii na video.</span><span class="sxs-lookup"><span data-stu-id="19155-177">It will overlay the image on top of the video.</span></span>

<span data-ttu-id="19155-178">Otevřete **Návrhář postupu provádění** a vyberte **soubor** > **nový pracovní prostor** > **převod plán, podle kterého**.</span><span class="sxs-lookup"><span data-stu-id="19155-178">Open **Workflow Designer** and select **File** > **New Workspace** > **Transcode Blueprint**.</span></span>

<span data-ttu-id="19155-179">Nový pracovní postup ukazuje tři prvky:</span><span class="sxs-lookup"><span data-stu-id="19155-179">The new workflow shows three elements:</span></span>

* <span data-ttu-id="19155-180">Primární zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="19155-180">Primary Source File</span></span>
* <span data-ttu-id="19155-181">Seznam klip ve formátu XML</span><span class="sxs-lookup"><span data-stu-id="19155-181">Clip List XML</span></span>
* <span data-ttu-id="19155-182">Výstupní soubor nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="19155-182">Output File/Asset</span></span>  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

<span data-ttu-id="19155-184">*Nový pracovní postup kódování*</span><span class="sxs-lookup"><span data-stu-id="19155-184">*New Encoding Workflow*</span></span>

<span data-ttu-id="19155-185">Aby bylo možné přijímat souboru vstupní média, začněte přidáte komponentu vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="19155-185">In order to accept the input media file, start with adding a Media File Input component.</span></span> <span data-ttu-id="19155-186">Chcete-li přidejte součást do pracovního postupu, podívejte se do vyhledávacího pole úložiště a přetáhněte na požadovanou položku na podokně návrháře.</span><span class="sxs-lookup"><span data-stu-id="19155-186">To add a component to the workflow, look for it in the Repository search box and drag the desired entry onto the designer pane.</span></span>

<span data-ttu-id="19155-187">V dalším kroku přidejte soubor video má být použit pro navrhování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="19155-187">Next, add the video file to be used for designing your workflow.</span></span> <span data-ttu-id="19155-188">Uděláte to tak, klikněte v podokně pozadí v Návrháři pracovních postupů a vyhledejte vlastnost primární zdrojový soubor v podokně pravém vlastnost.</span><span class="sxs-lookup"><span data-stu-id="19155-188">To do so, click the background pane in Workflow Designer and look for the Primary Source File property on the right-hand property pane.</span></span> <span data-ttu-id="19155-189">Klikněte na ikonu složky a vyberte příslušný soubor videa.</span><span class="sxs-lookup"><span data-stu-id="19155-189">Click the folder icon and select the appropriate video file.</span></span>

![Primární soubor zdroje](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

<span data-ttu-id="19155-191">*Primární soubor zdroje*</span><span class="sxs-lookup"><span data-stu-id="19155-191">*Primary File Source*</span></span>

<span data-ttu-id="19155-192">Potom zadejte soubor videa v komponentě vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="19155-192">Next, specify the video file in the Media File Input component.</span></span>   

![Vstupní zdroj souborů médií](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

<span data-ttu-id="19155-194">*Vstupní zdroj souborů médií*</span><span class="sxs-lookup"><span data-stu-id="19155-194">*Media File Input Source*</span></span>

<span data-ttu-id="19155-195">Jakmile to uděláte, bude komponentu vstupní soubor média zkontrolujte soubor a naplnit její výstup PIN tak, aby odrážela soubor, který ho prověřovány.</span><span class="sxs-lookup"><span data-stu-id="19155-195">As soon as this is done, the Media File Input component will inspect the file and populate its output pins to reflect the file that it inspected.</span></span>

<span data-ttu-id="19155-196">Dalším krokem je přidání "Video datový typ aktualizační" k určení barevný prostor pro Rec.709.</span><span class="sxs-lookup"><span data-stu-id="19155-196">The next step is to add a "Video Data Type Updater" to specify the color space to Rec.709.</span></span> <span data-ttu-id="19155-197">Přidání "Video formátu převaděč" nastavené na typ rozložení dat, rozvržení = konfigurovat planární.</span><span class="sxs-lookup"><span data-stu-id="19155-197">Add a "Video Format Converter" that is set to Data Layout/Layout type = Configurable Planar.</span></span> <span data-ttu-id="19155-198">Datový proud videa bude převedena do formátu, který může být přijata jako zdroj pro komponentu překrytí.</span><span class="sxs-lookup"><span data-stu-id="19155-198">This will convert the video stream to a format that can be taken as a source of the overlay component.</span></span>

![Video aktualizační datový typ a formát převaděč](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

<span data-ttu-id="19155-200">*Aktualizační video datový typ a formát převaděč*</span><span class="sxs-lookup"><span data-stu-id="19155-200">*Video Data Type Updater and Format Converter*</span></span>

![Typ rozložení = konfigurovat planární](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

<span data-ttu-id="19155-202">*Je-li konfigurovat planární typ rozložení*</span><span class="sxs-lookup"><span data-stu-id="19155-202">*Layout type is Configurable Planar*</span></span>

<span data-ttu-id="19155-203">V dalším kroku přidejte součást Video překrytí a připojení (nekomprimované) video kódu pin (nekomprimované) video připnete vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="19155-203">Next, add a Video Overlay component and connect the (uncompressed) video pin to the (uncompressed) video pin of the media file input.</span></span>

<span data-ttu-id="19155-204">Přidat další vstup soubor média (se načíst soubor loga), klikněte na tuto součást a přejmenujte ji na "Logo vstupní soubor média" a vyberte bitovou kopii (soubor například .png) ve vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="19155-204">Add another Media File Input (to load the logo file), click on this component and rename it to "Media File Input Logo", and select an image (a .png file for example) in the file property.</span></span> <span data-ttu-id="19155-205">PIN kód nekomprimované image se připojte k nekomprimované image pin překrytí.</span><span class="sxs-lookup"><span data-stu-id="19155-205">Connect the Uncompressed image pin to the Uncompressed image pin of the overlay.</span></span>

![Zdroj překrytí součásti a obrázkových souborů](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

<span data-ttu-id="19155-207">*Zdroj překrytí součásti a obrázkových souborů*</span><span class="sxs-lookup"><span data-stu-id="19155-207">*Overlay component and image file source*</span></span>

<span data-ttu-id="19155-208">Pokud chcete upravit pozici loga na video (například můžete chtít umístěte ho na 10 procent z levého horního rohu na video), zrušte zaškrtnutí políčka "Ruční vstup".</span><span class="sxs-lookup"><span data-stu-id="19155-208">If you want to modify the position of the logo on the video (for example, you might want to position it at 10 percent off of the top left corner of the video), clear the "Manual Input" check box.</span></span> <span data-ttu-id="19155-209">Lze provést, protože používáte vstup soubor média můžete zadat souboru logo součást překrytí.</span><span class="sxs-lookup"><span data-stu-id="19155-209">You can do this because you are using a Media File Input to provide the logo file to the overlay component.</span></span>

![Překrytí pozice](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

<span data-ttu-id="19155-211">*Překrytí pozice*</span><span class="sxs-lookup"><span data-stu-id="19155-211">*Overlay position*</span></span>

<span data-ttu-id="19155-212">Ke kódování datový proud videa na H.264, přidejte součásti kodér videa kodér AVC a AAC na plochu návrháře.</span><span class="sxs-lookup"><span data-stu-id="19155-212">To encode the video stream to H.264, add the AVC Video Encoder and AAC encoder components to the designer surface.</span></span> <span data-ttu-id="19155-213">Připojte kódy PIN.</span><span class="sxs-lookup"><span data-stu-id="19155-213">Connect the pins.</span></span>
<span data-ttu-id="19155-214">Nastavit kodér AAC a vybrat přednastavení převodu formátu zvuk: 2.0 (L, R).</span><span class="sxs-lookup"><span data-stu-id="19155-214">Set up the AAC encoder and select Audio Format Conversion/Preset : 2.0 (L, R).</span></span>

![Audio a Video kodéry](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

<span data-ttu-id="19155-216">*Audio a Video kodéry*</span><span class="sxs-lookup"><span data-stu-id="19155-216">*Audio and Video Encoders*</span></span>

<span data-ttu-id="19155-217">Nyní přidejte **ISO Mpeg-4 multiplexor** a **výstup souboru** součásti a připojte kódy PIN, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="19155-217">Now add the **ISO Mpeg-4 Multiplexer** and **File Output** components and connect the pins as shown.</span></span>

![MP4 multiplexor a výstupní soubor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

<span data-ttu-id="19155-219">*MP4 multiplexor a výstupní soubor*</span><span class="sxs-lookup"><span data-stu-id="19155-219">*MP4 multiplexer and file output*</span></span>

<span data-ttu-id="19155-220">Je nutné nastavit název souboru výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="19155-220">You need to set the name for the output file.</span></span> <span data-ttu-id="19155-221">Klikněte **výstup souboru** součásti a úpravy výraz pro soubor:</span><span class="sxs-lookup"><span data-stu-id="19155-221">Click the **File Output** component and edit the expression for the file:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Název výstupního souboru](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

<span data-ttu-id="19155-223">*Název výstupního souboru*</span><span class="sxs-lookup"><span data-stu-id="19155-223">*File output name*</span></span>

<span data-ttu-id="19155-224">Můžete spustit místně postup zkontrolujte, zda je správně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="19155-224">You can run the workflow locally to check that it is running correctly.</span></span>

<span data-ttu-id="19155-225">Po jejím dokončení, můžete ho spustit v Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="19155-225">After it finishes, you can run it in Azure Media Services.</span></span>

<span data-ttu-id="19155-226">Nejprve připravte ke dvěma souborům se prostředek ve službě Azure Media Services: soubor videa a logo.</span><span class="sxs-lookup"><span data-stu-id="19155-226">First, prepare an asset in Azure Media Services with two files in it: the video file and the logo.</span></span> <span data-ttu-id="19155-227">To provedete pomocí .NET nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="19155-227">You can do this by using the .NET or REST API.</span></span> <span data-ttu-id="19155-228">Můžete to provést taky pomocí portálu Azure nebo [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span><span class="sxs-lookup"><span data-stu-id="19155-228">You can also do this by using the Azure portal or [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span></span>

<span data-ttu-id="19155-229">V tomto kurzu se dozvíte, jak spravovat prostředky s AMSE.</span><span class="sxs-lookup"><span data-stu-id="19155-229">This tutorial shows you how to manage assets with AMSE.</span></span> <span data-ttu-id="19155-230">Existují dva způsoby, jak přidat soubory do prostředek:</span><span class="sxs-lookup"><span data-stu-id="19155-230">There are two ways to add files to an asset:</span></span>

* <span data-ttu-id="19155-231">Vytvořte místní složku, zkopírujte dva soubory v něm a přetažení složce **Asset** kartě.</span><span class="sxs-lookup"><span data-stu-id="19155-231">Create a local folder, copy the two files in it, and drag and drop the folder to the **Asset** tab.</span></span>
* <span data-ttu-id="19155-232">Nahrát videosoubor jako prostředek, se zobrazí informace o prostředku, přejděte na kartu soubory a odeslat další soubor (logo).</span><span class="sxs-lookup"><span data-stu-id="19155-232">Upload the video file as an asset, display the asset information, go to the files tab, and upload an additional file (logo).</span></span>

> [!NOTE]
> <span data-ttu-id="19155-233">Nezapomeňte nastavit primární soubor v prostředku (hlavní soubor video).</span><span class="sxs-lookup"><span data-stu-id="19155-233">Make sure to set a primary file in the asset (the main video file).</span></span>

![Soubory prostředků v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

<span data-ttu-id="19155-235">*Soubory prostředků v AMSE*</span><span class="sxs-lookup"><span data-stu-id="19155-235">*Asset files in AMSE*</span></span>

<span data-ttu-id="19155-236">Vyberte asset a rozhodnete Kódovat pomocí kodéru Premium.</span><span class="sxs-lookup"><span data-stu-id="19155-236">Select the asset and choose to encode it with Premium Encoder.</span></span> <span data-ttu-id="19155-237">Nahrát pracovního postupu a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="19155-237">Upload the workflow and select it.</span></span>

<span data-ttu-id="19155-238">Kliknutím na tlačítko předat data do procesoru a přidejte následující kód XML a nastavte vlastnosti modulu runtime:</span><span class="sxs-lookup"><span data-stu-id="19155-238">Click the button to pass data to the processor, and add the following XML to set the runtime properties:</span></span>

![Kodér úrovně Premium v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

<span data-ttu-id="19155-240">*Kodér úrovně Premium v AMSE*</span><span class="sxs-lookup"><span data-stu-id="19155-240">*Premium Encoder in AMSE*</span></span>

<span data-ttu-id="19155-241">Vložte následující data XML.</span><span class="sxs-lookup"><span data-stu-id="19155-241">Then, paste the following XML data.</span></span> <span data-ttu-id="19155-242">Je třeba zadat název souboru videa pro vstupní soubor média i primarySourceFile.</span><span class="sxs-lookup"><span data-stu-id="19155-242">You need to specify the name of the video file for both the Media File Input and primarySourceFile.</span></span> <span data-ttu-id="19155-243">Zadejte název název souboru pro logo příliš.</span><span class="sxs-lookup"><span data-stu-id="19155-243">Specify the name of the file name for the logo too.</span></span>

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

<span data-ttu-id="19155-245">*setRuntimeProperties*</span><span class="sxs-lookup"><span data-stu-id="19155-245">*setRuntimeProperties*</span></span>

<span data-ttu-id="19155-246">Je-li používat sadu .NET SDK k vytvoření a spuštění úlohy, tato data XML musí předat jako konfigurační řetězec.</span><span class="sxs-lookup"><span data-stu-id="19155-246">If you use the .NET SDK to create and run the task, this XML data has to be passed as the configuration string.</span></span>

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

<span data-ttu-id="19155-247">Po dokončení úlohy soubor MP4 ve výstupní asset zobrazí překrytí!</span><span class="sxs-lookup"><span data-stu-id="19155-247">After the job is complete, the MP4 file in the output asset displays the overlay!</span></span>

![Překrytí na video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

<span data-ttu-id="19155-249">*Překrytí na video*</span><span class="sxs-lookup"><span data-stu-id="19155-249">*Overlay on the video*</span></span>

<span data-ttu-id="19155-250">Můžete si stáhnout ukázkový pracovní postup z [Githubu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span><span class="sxs-lookup"><span data-stu-id="19155-250">You can download the sample workflow from [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span></span>

## <a name="example-2--multiple-audio-language-encoding"></a><span data-ttu-id="19155-251">Příklad 2: Více zvuk jazyk kódování</span><span class="sxs-lookup"><span data-stu-id="19155-251">Example 2 : Multiple audio language encoding</span></span>

<span data-ttu-id="19155-252">Příkladem zvuk vícejazyčné kódování workfkow je k dispozici v [Githubu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span><span class="sxs-lookup"><span data-stu-id="19155-252">An example of multiple audio language encoding workfkow is available in [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span></span>

<span data-ttu-id="19155-253">Tato složka obsahuje ukázkový pracovní postup, který můžete použít ke kódování souboru MXF k více majetku soubory MP4 s více zvukových stop.</span><span class="sxs-lookup"><span data-stu-id="19155-253">This folder contains a sample workflow which can be used to encode a MXF file to a multi MP4 files asset with multiple audio tracks.</span></span>

<span data-ttu-id="19155-254">Tento pracovní postup předpokládá, že soubor MXF obsahuje jeden zvuk sledovat; Další zvukových stop mají být předány jako samostatné zvukové soubory (WAV nebo MP4...).</span><span class="sxs-lookup"><span data-stu-id="19155-254">This workflow assumes that the MXF file contains one audio track ; the additional audio tracks should be passed as seperate audio files (WAV or MP4...).</span></span>

<span data-ttu-id="19155-255">Ke kódování, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="19155-255">To encode, please follow these steps:</span></span>

* <span data-ttu-id="19155-256">Vytvoření prostředku služby Media Services s MXF soubor a zvukové soubory (0 až 18 zvukové soubory).</span><span class="sxs-lookup"><span data-stu-id="19155-256">Create a Media Services asset with the MXF file and the Audio files (0 to 18 audio files).</span></span>
* <span data-ttu-id="19155-257">Ujistěte se, že v souboru MXF nastavena jako primární soubor.</span><span class="sxs-lookup"><span data-stu-id="19155-257">Make sure that the MXF file is set as a primary file.</span></span>
* <span data-ttu-id="19155-258">Vytvoření úlohy a úlohy pomocí procesoru kodér pracovního postupu úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="19155-258">Create a job and a task using the Premium Workflow Encoder processor.</span></span> <span data-ttu-id="19155-259">Použití pracovního postupu poskytuje (MultiMP4-1080p-19audio-v1.workflow).</span><span class="sxs-lookup"><span data-stu-id="19155-259">Use the workflow provided (MultiMP4-1080p-19audio-v1.workflow).</span></span>
* <span data-ttu-id="19155-260">Předávání dat setruntime.xml úlohy (Pokud používáte Azure Media Services Explorer, použijte tlačítko "předávání dat xml do pracovního postupu").</span><span class="sxs-lookup"><span data-stu-id="19155-260">Pass the setruntime.xml data to the task (if you use Azure Media Services Explorer, use the “pass xml data to the workflow” button).</span></span>
  * <span data-ttu-id="19155-261">Aktualizujte data XML a určete správný soubor jazyky a názvy značek.</span><span class="sxs-lookup"><span data-stu-id="19155-261">Please update the XML data to specify the correct file names and languages tags.</span></span>
  * <span data-ttu-id="19155-262">Pracovní postup obsahuje zvuk komponenty s názvem zvuk 1 na zvukové 18.</span><span class="sxs-lookup"><span data-stu-id="19155-262">The workflow has audio components named Audio 1 to Audio 18.</span></span>
  * <span data-ttu-id="19155-263">RFC5646 je podporována pro značku jazyka.</span><span class="sxs-lookup"><span data-stu-id="19155-263">RFC5646 is supported for the language tag.</span></span>

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

* <span data-ttu-id="19155-264">K zakódovanému assetu bude obsahovat více jazyk zvukových stop a musí být tyto sleduje vybrat v Azure Media Player.</span><span class="sxs-lookup"><span data-stu-id="19155-264">The encoded asset will contain multi language audio tracks and these tracks should be selectable in Azure Media Player.</span></span>

## <a name="see-also"></a><span data-ttu-id="19155-265">Viz také</span><span class="sxs-lookup"><span data-stu-id="19155-265">See also</span></span>
* [<span data-ttu-id="19155-266">Představení Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="19155-266">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="19155-267">Jak používat kódování Premium ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="19155-267">How to use Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="19155-268">Kódování obsahu na vyžádání pomocí Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="19155-268">Encoding on-demand content with Azure Media Services</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)
* [<span data-ttu-id="19155-269">Formáty Media Encoder Premium pracovního postupu a kodeky</span><span class="sxs-lookup"><span data-stu-id="19155-269">Media Encoder Premium Workflow formats and codecs</span></span>](media-services-premium-workflow-encoder-formats.md)
* [<span data-ttu-id="19155-270">Ukázkové soubory pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="19155-270">Sample workflow files</span></span>](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [<span data-ttu-id="19155-271">Nástroj pro Azure Media Services Explorer</span><span class="sxs-lookup"><span data-stu-id="19155-271">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="19155-272">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="19155-272">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="19155-273">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="19155-273">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
