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
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a><span data-ttu-id="6f152-103">Použití více vstupní soubory a vlastnosti komponenty s kodér úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="6f152-103">Using multiple input files and component properties with Premium Encoder</span></span>
## <a name="overview"></a><span data-ttu-id="6f152-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6f152-104">Overview</span></span>
<span data-ttu-id="6f152-105">Existují scénáře, ve kterých může být nutné toocustomize vlastnosti komponenty, zadejte klip seznamu XML obsahu nebo odeslat více vstupních souborů při odesílání úlohy hello **Media Encoder Premium pracovního postupu** procesor médií.</span><span class="sxs-lookup"><span data-stu-id="6f152-105">There are scenarios in which you might need toocustomize component properties, specify Clip List XML content, or send multiple input files when you submit a task with hello **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="6f152-106">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="6f152-106">Some examples are:</span></span>

* <span data-ttu-id="6f152-107">Překrytí text na video a nastavení v době běhu pro každý vstupní video hello textové hodnoty (například hello aktuální datum).</span><span class="sxs-lookup"><span data-stu-id="6f152-107">Overlaying text on video and setting hello text value (for example, hello current date) at runtime for each input video.</span></span>
* <span data-ttu-id="6f152-108">Přizpůsobení hello klip seznamu XML (toospecify jednu nebo několik zdrojových souborů, s nebo bez oříznutí, atd.).</span><span class="sxs-lookup"><span data-stu-id="6f152-108">Customizing hello Clip List XML (toospecify one or several source files, with or without trimming, etc.).</span></span>
* <span data-ttu-id="6f152-109">Obrázek loga překrytí na hello vstupní video, při hello video je zakódován.</span><span class="sxs-lookup"><span data-stu-id="6f152-109">Overlaying a logo image on hello input video while hello video is encoded.</span></span>
* <span data-ttu-id="6f152-110">Více kódování zvuk jazyka.</span><span class="sxs-lookup"><span data-stu-id="6f152-110">Multiple audio language encoding.</span></span>

<span data-ttu-id="6f152-111">toolet hello **Media Encoder Premium pracovního postupu** vědět, že chcete změnit některé vlastnosti v pracovním postupu hello při vytvoření úlohy hello nebo odeslání více vstupní soubory, máte toouse konfigurace řetězec, který obsahuje ** setRuntimeProperties** nebo **transcodeSource**.</span><span class="sxs-lookup"><span data-stu-id="6f152-111">toolet hello **Media Encoder Premium Workflow** know that you are changing some properties in hello workflow when you create hello task or send multiple input files, you have toouse a configuration string that contains **setRuntimeProperties** and/or **transcodeSource**.</span></span> <span data-ttu-id="6f152-112">Toto téma vysvětluje, jak toouse je.</span><span class="sxs-lookup"><span data-stu-id="6f152-112">This topic explains how toouse them.</span></span>

## <a name="configuration-string-syntax"></a><span data-ttu-id="6f152-113">Konfigurace řetězec syntaxe</span><span class="sxs-lookup"><span data-stu-id="6f152-113">Configuration string syntax</span></span>
<span data-ttu-id="6f152-114">Hello konfigurační řetězec tooset v hello kódování úloh používá dokument XML, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6f152-114">hello configuration string tooset in hello encoding task uses an XML document that looks like this:</span></span>

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

<span data-ttu-id="6f152-115">Hello následuje hello C# kód, který čte konfigurační soubor XML hello ze souboru, aktualizujte jej s hello právo video filename a předává je toohello úloh pro úlohu:</span><span class="sxs-lookup"><span data-stu-id="6f152-115">hello following is hello C# code that reads hello XML configuration from a file, update it with hello right video filename and passes it toohello task in a job:</span></span>

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

## <a name="customizing-component-properties"></a><span data-ttu-id="6f152-116">Přizpůsobení vlastnosti komponenty</span><span class="sxs-lookup"><span data-stu-id="6f152-116">Customizing component properties</span></span>
### <a name="property-with-a-simple-value"></a><span data-ttu-id="6f152-117">Vlastnost s hodnotou jednoduché</span><span class="sxs-lookup"><span data-stu-id="6f152-117">Property with a simple value</span></span>
<span data-ttu-id="6f152-118">V některých případech je užitečné toocustomize vlastnost součásti společně s hello souboru pracovního postupu, který bude toobe provedený Media Encoder Premium pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6f152-118">In some cases, it is useful toocustomize a component property together with hello workflow file that is going toobe executed by Media Encoder Premium Workflow.</span></span>

<span data-ttu-id="6f152-119">Předpokládejme, že určený pracovního postupu tento text překryvy videa a textu hello (například hello aktuální data) by měla toobe sadu za běhu.</span><span class="sxs-lookup"><span data-stu-id="6f152-119">Suppose you designed a workflow that overlays text on your videos, and hello text (for example, hello current date) is supposed toobe set at runtime.</span></span> <span data-ttu-id="6f152-120">To provedete odesláním toobe textu hello nastavit jako hello novou hodnotu pro vlastnost textu hello hello překrytí součásti od hello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="6f152-120">You can do this by sending hello text toobe set as hello new value for hello text property of hello overlay component from hello encoding task.</span></span> <span data-ttu-id="6f152-121">Tento mechanismus toochange můžete použít další vlastnosti komponenty v pracovním postupu hello (například hello pozici nebo barvu hello překrytí, přenosovou rychlostí hello hello AVC kodér atd.).</span><span class="sxs-lookup"><span data-stu-id="6f152-121">You can use this mechanism toochange other properties of a component in hello workflow (such as hello position or color of hello overlay, hello bitrate of hello AVC encoder, etc.).</span></span>

<span data-ttu-id="6f152-122">**setRuntimeProperties** je použité toooverride vlastnost v součásti hello hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6f152-122">**setRuntimeProperties** is used toooverride a property in hello components of hello workflow.</span></span>

<span data-ttu-id="6f152-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6f152-123">Example:</span></span>

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

### <a name="property-with-an-xml-value"></a><span data-ttu-id="6f152-124">Vlastnost s hodnotou XML</span><span class="sxs-lookup"><span data-stu-id="6f152-124">Property with an XML value</span></span>
<span data-ttu-id="6f152-125">zapouzdření tooset vlastnost, která očekává hodnotu XML pomocí `<![CDATA[ and ]]>`.</span><span class="sxs-lookup"><span data-stu-id="6f152-125">tooset a property that expects an XML value, encapsulate by using `<![CDATA[ and ]]>`.</span></span>

<span data-ttu-id="6f152-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6f152-126">Example:</span></span>

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
> <span data-ttu-id="6f152-127">Zajistěte, aby tooput znaků CR vrátit pouze po `<![CDATA[`.</span><span class="sxs-lookup"><span data-stu-id="6f152-127">Make sure not tooput a carriage return just after `<![CDATA[`.</span></span>

### <a name="propertypath-value"></a><span data-ttu-id="6f152-128">Hodnota propertyPath</span><span class="sxs-lookup"><span data-stu-id="6f152-128">propertyPath value</span></span>
<span data-ttu-id="6f152-129">V předchozích příkladech hello, byla hello propertyPath "/ souborů médií vstup/filename" nebo "/ inactiveTimeout" nebo "clipListXml".</span><span class="sxs-lookup"><span data-stu-id="6f152-129">In hello previous examples, hello propertyPath was "/Media File Input/filename" or "/inactiveTimeout" or "clipListXml".</span></span>
<span data-ttu-id="6f152-130">Toto je, obecně hello názvu komponenty hello pak hello název vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-130">This is, in general, hello name of hello component, then hello name of hello property.</span></span> <span data-ttu-id="6f152-131">Hello cesta může mít více nebo méně úrovně, jako je třeba "/ primarySourceFile" (protože hello vlastnost je v kořenovém hello hello pracovního postupu) nebo "/ Video zpracování nebo obrázek překrytí nebo krytí" (protože hello překrytí se nachází ve skupině).</span><span class="sxs-lookup"><span data-stu-id="6f152-131">hello path can have more or fewer levels, like "/primarySourceFile" (because hello property is at hello root of hello workflow) or "/Video Processing/Graphic Overlay/Opacity" (because hello Overlay is in a group).</span></span>    

<span data-ttu-id="6f152-132">toocheck hello cesty a názvu vlastnosti, použijte hello akce tlačítka, které je okamžitě vedle každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6f152-132">toocheck hello path and property name, use hello action button that is immediately beside each property.</span></span> <span data-ttu-id="6f152-133">Můžete kliknutím na toto tlačítko akce a vybrat **upravit**.</span><span class="sxs-lookup"><span data-stu-id="6f152-133">You can click this action button and select **Edit**.</span></span> <span data-ttu-id="6f152-134">Tato rutina ukáže vám hello skutečný název vlastnosti hello a okamžitě nad ním, hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="6f152-134">This will show you hello actual name of hello property, and immediately above it, hello namespace.</span></span>

![Akce či upravit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Vlastnost](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a><span data-ttu-id="6f152-137">Více vstupní soubory</span><span class="sxs-lookup"><span data-stu-id="6f152-137">Multiple input files</span></span>
<span data-ttu-id="6f152-138">Každý úkol, odešlete toohello **Media Encoder Premium pracovního postupu** vyžaduje dva prostředky:</span><span class="sxs-lookup"><span data-stu-id="6f152-138">Each task that you submit toohello **Media Encoder Premium Workflow** requires two assets:</span></span>

* <span data-ttu-id="6f152-139">Hello nejprve jeden je *pracovního postupu Asset* obsahující soubor pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6f152-139">hello first one is a *Workflow Asset* that contains a workflow file.</span></span> <span data-ttu-id="6f152-140">Soubory pracovního postupu můžete navrhnout pomocí hello [Návrhář postupu provádění](media-services-workflow-designer.md).</span><span class="sxs-lookup"><span data-stu-id="6f152-140">You can design workflow files by using hello [Workflow Designer](media-services-workflow-designer.md).</span></span>
* <span data-ttu-id="6f152-141">je technologie Hello druhá *média Asset* obsahující soubory hello média, které chcete tooencode.</span><span class="sxs-lookup"><span data-stu-id="6f152-141">hello second one is a *Media Asset* that contains hello media file(s) that you want tooencode.</span></span>

<span data-ttu-id="6f152-142">Pokud odesíláte více toohello soubory médií **Media Encoder Premium pracovního postupu** použít kodér, hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="6f152-142">When you're sending multiple media files toohello **Media Encoder Premium Workflow** encoder, hello following constraints apply:</span></span>

* <span data-ttu-id="6f152-143">Všechna média hello soubory musí být ve stejné hello *média Asset*.</span><span class="sxs-lookup"><span data-stu-id="6f152-143">All hello media files must be in hello same *Media Asset*.</span></span> <span data-ttu-id="6f152-144">Použití více prostředků média není podporováno.</span><span class="sxs-lookup"><span data-stu-id="6f152-144">Using multiple Media Assets is not supported.</span></span>
* <span data-ttu-id="6f152-145">V tento prostředek média je nutné nastavit primární soubor hello (v ideálním případě jde hello hlavní soubor videa, který hello kodér se zobrazí výzva, tooprocess).</span><span class="sxs-lookup"><span data-stu-id="6f152-145">You must set hello primary file in this Media Asset (ideally, this is hello main video file that hello encoder is asked tooprocess).</span></span>
* <span data-ttu-id="6f152-146">Je nezbytné toopass konfigurační data, která zahrnuje hello **setRuntimeProperties** nebo **transcodeSource** element toohello procesoru.</span><span class="sxs-lookup"><span data-stu-id="6f152-146">It is necessary toopass configuration data that includes hello **setRuntimeProperties** and/or **transcodeSource** element toohello processor.</span></span>
  * <span data-ttu-id="6f152-147">**setRuntimeProperties** je vlastnost filename hello použité toooverride nebo jinou vlastnost v součásti hello hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6f152-147">**setRuntimeProperties** is used toooverride hello filename property or another property in hello components of hello workflow.</span></span>
  * <span data-ttu-id="6f152-148">**transcodeSource** je použité toospecify hello obsah klip seznamu XML.</span><span class="sxs-lookup"><span data-stu-id="6f152-148">**transcodeSource** is used toospecify hello Clip List XML content.</span></span>

<span data-ttu-id="6f152-149">Připojení v pracovním postupu hello:</span><span class="sxs-lookup"><span data-stu-id="6f152-149">Connections in hello workflow:</span></span>

* <span data-ttu-id="6f152-150">Pokud je použití jednoho nebo několika komponent vstupní soubor média a naplánujte toouse **setRuntimeProperties** toospecify hello název souboru a pak se nepřipojují hello primární soubor součást pin toothem.</span><span class="sxs-lookup"><span data-stu-id="6f152-150">If you use one or several Media File Input components and plan toouse **setRuntimeProperties** toospecify hello file name, then do not connect hello primary file component pin toothem.</span></span> <span data-ttu-id="6f152-151">Ujistěte se, že neexistuje žádné připojení mezi objektu hello primární soubor a hello Input(s) soubor média.</span><span class="sxs-lookup"><span data-stu-id="6f152-151">Make sure that there is no connection between hello primary file object and hello Media File Input(s).</span></span>
* <span data-ttu-id="6f152-152">Pokud dáváte přednost toouse klip seznamu XML a jedna součást zdrojové médium, pak se můžete připojit i společně.</span><span class="sxs-lookup"><span data-stu-id="6f152-152">If you prefer toouse Clip List XML and one Media Source component, then you can connect both together.</span></span>

![Žádné připojení z primární zdrojový soubor tooMedia vstupní soubor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

<span data-ttu-id="6f152-154">*Pokud použijete vlastnost filename hello tooset setRuntimeProperties neexistuje žádné připojení z hello primární soubor tooMedia vstupní soubor součásti.*</span><span class="sxs-lookup"><span data-stu-id="6f152-154">*There is no connection from hello primary file tooMedia File Input component(s) if you use setRuntimeProperties tooset hello filename property.*</span></span>

![Připojení ze seznamu XML klip tooClip zdrojového seznamu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

<span data-ttu-id="6f152-156">*Můžete připojit klip seznamu XML tooMedia zdroje a použít transcodeSource.*</span><span class="sxs-lookup"><span data-stu-id="6f152-156">*You can connect Clip List XML tooMedia Source and use transcodeSource.*</span></span>

### <a name="clip-list-xml-customization"></a><span data-ttu-id="6f152-157">Oříznutí seznamu XML přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="6f152-157">Clip List XML customization</span></span>
<span data-ttu-id="6f152-158">Můžete zadat hello klip seznamu XML v pracovním postupu hello za běhu pomocí **transcodeSource** v konfiguraci hello řetězce XML.</span><span class="sxs-lookup"><span data-stu-id="6f152-158">You can specify hello Clip List XML in hello workflow at runtime by using **transcodeSource** in hello configuration string XML.</span></span> <span data-ttu-id="6f152-159">To vyžaduje hello klip seznamu XML pin toobe připojené toohello zdrojové médium součást v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-159">This requires hello Clip List XML pin toobe connected toohello Media Source component in hello workflow.</span></span>

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

<span data-ttu-id="6f152-160">Pokud chcete toouse /primarySourceFile toospecify tento výstup hello tooname vlastnost soubory pomocí "Výrazy", pak doporučujeme předávání hello klip seznamu XML jako vlastnost *po* hello /primarySourceFile vlastnost, tooavoid s hello klip seznamu přepsat nastavení /primarySourceFile hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-160">If you want toospecify /primarySourceFile toouse this property tooname hello output files by using 'Expressions', then we recommend passing hello Clip List XML as a property *after* hello /primarySourceFile property, tooavoid having hello Clip List be overridden by hello /primarySourceFile setting.</span></span>

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

<span data-ttu-id="6f152-161">S další rámce přesné oříznutí:</span><span class="sxs-lookup"><span data-stu-id="6f152-161">With additional frame-accurate trimming:</span></span>

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

## <a name="example-1--overlay-an-image-on-top-of-hello-video"></a><span data-ttu-id="6f152-162">Příklad 1: Překrytí bitovou kopii nad hello video</span><span class="sxs-lookup"><span data-stu-id="6f152-162">Example 1 : Overlay an image on top of hello video</span></span>

### <a name="presentation"></a><span data-ttu-id="6f152-163">Prezentace</span><span class="sxs-lookup"><span data-stu-id="6f152-163">Presentation</span></span>
<span data-ttu-id="6f152-164">Zvažte příklad, ve kterém chcete toooverlay obrázek loga na hello vstupní video při hello video je zakódován.</span><span class="sxs-lookup"><span data-stu-id="6f152-164">Consider an example in which you want toooverlay a logo image on hello input video while hello video is encoded.</span></span> <span data-ttu-id="6f152-165">V tomto příkladu hello vstupní video má název "Microsoft_HoloLens_Possibilities_816p24.mp4" a hello logo je s názvem "logo.png".</span><span class="sxs-lookup"><span data-stu-id="6f152-165">In this example, hello input video is named "Microsoft_HoloLens_Possibilities_816p24.mp4" and hello logo is named "logo.png".</span></span> <span data-ttu-id="6f152-166">Měli byste provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f152-166">You should perform hello following steps:</span></span>

* <span data-ttu-id="6f152-167">Vytvoření prostředku pracovního postupu s soubor pracovního postupu hello (viz následující ukázka hello).</span><span class="sxs-lookup"><span data-stu-id="6f152-167">Create a Workflow Asset with hello workflow file (see hello following example).</span></span>
* <span data-ttu-id="6f152-168">Vytvoření média prostředku, který obsahuje dva soubory: MyInputVideo.mp4 jako hello primární soubor a MyLogo.png.</span><span class="sxs-lookup"><span data-stu-id="6f152-168">Create a Media Asset, which contains two files: MyInputVideo.mp4 as hello primary file and MyLogo.png.</span></span>
* <span data-ttu-id="6f152-169">Odesílat toohello úlohy pracovního postupu Premium Media Encoder média prostředky procesoru s hello výše vstup a zadejte následující konfigurační řetězec hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-169">Send a task toohello Media Encoder Premium Workflow media processor with hello above input assets and specify hello following configuration string.</span></span>

<span data-ttu-id="6f152-170">Konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6f152-170">Configuration:</span></span>

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

<span data-ttu-id="6f152-171">V předchozím příkladu hello je odeslána hello název hello videosoubor toohello vstupní soubor média součásti a hello primarySourceFile vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6f152-171">In hello example above, hello name of hello video file is sent toohello Media File Input component and hello primarySourceFile property.</span></span> <span data-ttu-id="6f152-172">Hello název souboru logo hello se odesílají tooanother vstup souboru média, který je připojený toohello součást grafické překrytí.</span><span class="sxs-lookup"><span data-stu-id="6f152-172">hello name of hello logo file is sent tooanother Media File Input that is connected toohello graphic overlay component.</span></span>

> [!NOTE]
> <span data-ttu-id="6f152-173">Název souboru videa Hello se odesílají toohello primarySourceFile vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6f152-173">hello video file name is sent toohello primarySourceFile property.</span></span> <span data-ttu-id="6f152-174">Hello důvodem je tato vlastnost v hello pracovního postupu pro vytváření název souboru správný výstup hello pomocí výrazů, například toouse.</span><span class="sxs-lookup"><span data-stu-id="6f152-174">hello reason for this is toouse this property in hello workflow for building hello correct output file name using Expressions, for example.</span></span>

### <a name="step-by-step-workflow-creation"></a><span data-ttu-id="6f152-175">Vytvoření pracovního postupu krok za krokem</span><span class="sxs-lookup"><span data-stu-id="6f152-175">Step-by-step workflow creation</span></span>
<span data-ttu-id="6f152-176">Tady jsou kroky toocreate hello pracovní postup, který přebírá dva soubory jako vstup: video a bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="6f152-176">Here are hello steps toocreate a workflow that takes two files as input: a video and an image.</span></span> <span data-ttu-id="6f152-177">Ho bude překrytí hello image nad hello videa.</span><span class="sxs-lookup"><span data-stu-id="6f152-177">It will overlay hello image on top of hello video.</span></span>

<span data-ttu-id="6f152-178">Otevřete **Návrhář postupu provádění** a vyberte **soubor** > **nový pracovní prostor** > **převod plán, podle kterého**.</span><span class="sxs-lookup"><span data-stu-id="6f152-178">Open **Workflow Designer** and select **File** > **New Workspace** > **Transcode Blueprint**.</span></span>

<span data-ttu-id="6f152-179">nový pracovní postup Hello ukazuje tři prvky:</span><span class="sxs-lookup"><span data-stu-id="6f152-179">hello new workflow shows three elements:</span></span>

* <span data-ttu-id="6f152-180">Primární zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="6f152-180">Primary Source File</span></span>
* <span data-ttu-id="6f152-181">Seznam klip ve formátu XML</span><span class="sxs-lookup"><span data-stu-id="6f152-181">Clip List XML</span></span>
* <span data-ttu-id="6f152-182">Výstupní soubor nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="6f152-182">Output File/Asset</span></span>  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

<span data-ttu-id="6f152-184">*Nový pracovní postup kódování*</span><span class="sxs-lookup"><span data-stu-id="6f152-184">*New Encoding Workflow*</span></span>

<span data-ttu-id="6f152-185">V pořadí tooaccept hello vstupními médii souboru začněte přidáte komponentu vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="6f152-185">In order tooaccept hello input media file, start with adding a Media File Input component.</span></span> <span data-ttu-id="6f152-186">tooadd toohello součást pracovního postupu, podívejte se do vyhledávacího pole úložiště hello a přetáhněte hello potřeby položku na návrháře podokně hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-186">tooadd a component toohello workflow, look for it in hello Repository search box and drag hello desired entry onto hello designer pane.</span></span>

<span data-ttu-id="6f152-187">Dál přidejte toobe videosoubor hello používá pro navrhování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6f152-187">Next, add hello video file toobe used for designing your workflow.</span></span> <span data-ttu-id="6f152-188">toodo tak, klikněte na podokno pozadí hello v Návrháři pracovních postupů a vyhledejte hello primární zdrojový soubor vlastnosti v podokně pravém vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-188">toodo so, click hello background pane in Workflow Designer and look for hello Primary Source File property on hello right-hand property pane.</span></span> <span data-ttu-id="6f152-189">Klikněte na ikonu hello složky a vyberte příslušné videosoubor hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-189">Click hello folder icon and select hello appropriate video file.</span></span>

![Primární soubor zdroje](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

<span data-ttu-id="6f152-191">*Primární soubor zdroje*</span><span class="sxs-lookup"><span data-stu-id="6f152-191">*Primary File Source*</span></span>

<span data-ttu-id="6f152-192">Potom zadejte hello videosoubor v komponentě hello vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="6f152-192">Next, specify hello video file in hello Media File Input component.</span></span>   

![Vstupní zdroj souborů médií](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

<span data-ttu-id="6f152-194">*Vstupní zdroj souborů médií*</span><span class="sxs-lookup"><span data-stu-id="6f152-194">*Media File Input Source*</span></span>

<span data-ttu-id="6f152-195">Jakmile to uděláte, bude součást vstupní soubor média hello zkontrolujte soubor hello a naplnit její výstup PIN tooreflect hello soubor, který ho prověřovány.</span><span class="sxs-lookup"><span data-stu-id="6f152-195">As soon as this is done, hello Media File Input component will inspect hello file and populate its output pins tooreflect hello file that it inspected.</span></span>

<span data-ttu-id="6f152-196">dalším krokem Hello je tooadd tooRec.709 místo k barva "Aktualizační Video datového typu" toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-196">hello next step is tooadd a "Video Data Type Updater" toospecify hello color space tooRec.709.</span></span> <span data-ttu-id="6f152-197">Přidání "Video formátu převaděč" nastavený typ rozložení a rozložení tooData = konfigurovat planární.</span><span class="sxs-lookup"><span data-stu-id="6f152-197">Add a "Video Format Converter" that is set tooData Layout/Layout type = Configurable Planar.</span></span> <span data-ttu-id="6f152-198">Tato funkce převede hello datový proud videa tooa formátu, který může být přijata jako zdroj hello překrytí součásti.</span><span class="sxs-lookup"><span data-stu-id="6f152-198">This will convert hello video stream tooa format that can be taken as a source of hello overlay component.</span></span>

![Video aktualizační datový typ a formát převaděč](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

<span data-ttu-id="6f152-200">*Aktualizační video datový typ a formát převaděč*</span><span class="sxs-lookup"><span data-stu-id="6f152-200">*Video Data Type Updater and Format Converter*</span></span>

![Typ rozložení = konfigurovat planární](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

<span data-ttu-id="6f152-202">*Je-li konfigurovat planární typ rozložení*</span><span class="sxs-lookup"><span data-stu-id="6f152-202">*Layout type is Configurable Planar*</span></span>

<span data-ttu-id="6f152-203">V dalším kroku přidejte součást Video překrytí a připojte hello (nekomprimované) video pin toohello (nekomprimované) video pin vstupní soubor média hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-203">Next, add a Video Overlay component and connect hello (uncompressed) video pin toohello (uncompressed) video pin of hello media file input.</span></span>

<span data-ttu-id="6f152-204">Přidejte jiný média vstupní soubor (tooload hello logo soubor), klikněte na tuto součást a přejmenujte ji příliš "Média souboru vstup Logo" a vyberte bitovou kopii (soubor .png např.) v souboru vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-204">Add another Media File Input (tooload hello logo file), click on this component and rename it too"Media File Input Logo", and select an image (a .png file for example) in hello file property.</span></span> <span data-ttu-id="6f152-205">Připojte hello nekomprimované image pin toohello nekomprimované image pin hello překrytí.</span><span class="sxs-lookup"><span data-stu-id="6f152-205">Connect hello Uncompressed image pin toohello Uncompressed image pin of hello overlay.</span></span>

![Zdroj překrytí součásti a obrázkových souborů](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

<span data-ttu-id="6f152-207">*Zdroj překrytí součásti a obrázkových souborů*</span><span class="sxs-lookup"><span data-stu-id="6f152-207">*Overlay component and image file source*</span></span>

<span data-ttu-id="6f152-208">Pokud chcete umístění hello toomodify hello logo na hello video (například chtít tooposition ho na 10 procent z horní části hello levého horního rohu videa hello), zrušte zaškrtnutí políčka "Ruční vstup" hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-208">If you want toomodify hello position of hello logo on hello video (for example, you might want tooposition it at 10 percent off of hello top left corner of hello video), clear hello "Manual Input" check box.</span></span> <span data-ttu-id="6f152-209">To můžete provést, protože používáte komponentu vstupní soubor média tooprovide hello logo souboru toohello překrytí.</span><span class="sxs-lookup"><span data-stu-id="6f152-209">You can do this because you are using a Media File Input tooprovide hello logo file toohello overlay component.</span></span>

![Překrytí pozice](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

<span data-ttu-id="6f152-211">*Překrytí pozice*</span><span class="sxs-lookup"><span data-stu-id="6f152-211">*Overlay position*</span></span>

<span data-ttu-id="6f152-212">tooencode hello tooH.264 datový proud videa, přidejte hello AVC kodér videa a AAC kodér součásti toohello návrháře prostor.</span><span class="sxs-lookup"><span data-stu-id="6f152-212">tooencode hello video stream tooH.264, add hello AVC Video Encoder and AAC encoder components toohello designer surface.</span></span> <span data-ttu-id="6f152-213">Připojte hello PIN.</span><span class="sxs-lookup"><span data-stu-id="6f152-213">Connect hello pins.</span></span>
<span data-ttu-id="6f152-214">Nastavit hello AAC kodér a vybrat přednastavení převodu formátu zvuk: 2.0 (L, R).</span><span class="sxs-lookup"><span data-stu-id="6f152-214">Set up hello AAC encoder and select Audio Format Conversion/Preset : 2.0 (L, R).</span></span>

![Audio a Video kodéry](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

<span data-ttu-id="6f152-216">*Audio a Video kodéry*</span><span class="sxs-lookup"><span data-stu-id="6f152-216">*Audio and Video Encoders*</span></span>

<span data-ttu-id="6f152-217">Nyní přidejte hello **ISO Mpeg-4 multiplexor** a **výstup souboru** součásti a připojte PIN hello, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="6f152-217">Now add hello **ISO Mpeg-4 Multiplexer** and **File Output** components and connect hello pins as shown.</span></span>

![MP4 multiplexor a výstupní soubor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

<span data-ttu-id="6f152-219">*MP4 multiplexor a výstupní soubor*</span><span class="sxs-lookup"><span data-stu-id="6f152-219">*MP4 multiplexer and file output*</span></span>

<span data-ttu-id="6f152-220">Je třeba název hello tooset hello výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="6f152-220">You need tooset hello name for hello output file.</span></span> <span data-ttu-id="6f152-221">Klikněte na tlačítko hello **výstup souboru** hello výraz součásti a úpravy souboru hello:</span><span class="sxs-lookup"><span data-stu-id="6f152-221">Click hello **File Output** component and edit hello expression for hello file:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Název výstupního souboru](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

<span data-ttu-id="6f152-223">*Název výstupního souboru*</span><span class="sxs-lookup"><span data-stu-id="6f152-223">*File output name*</span></span>

<span data-ttu-id="6f152-224">Můžete spustit pracovní postup hello místně toocheck, zda je správně spuštěn.</span><span class="sxs-lookup"><span data-stu-id="6f152-224">You can run hello workflow locally toocheck that it is running correctly.</span></span>

<span data-ttu-id="6f152-225">Po jejím dokončení, můžete ho spustit v Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="6f152-225">After it finishes, you can run it in Azure Media Services.</span></span>

<span data-ttu-id="6f152-226">Nejprve připravte ke dvěma souborům se prostředek ve službě Azure Media Services: hello videosoubor a hello logo.</span><span class="sxs-lookup"><span data-stu-id="6f152-226">First, prepare an asset in Azure Media Services with two files in it: hello video file and hello logo.</span></span> <span data-ttu-id="6f152-227">To provedete pomocí hello .NET nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="6f152-227">You can do this by using hello .NET or REST API.</span></span> <span data-ttu-id="6f152-228">Můžete to provést taky pomocí hello portál Azure nebo [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span><span class="sxs-lookup"><span data-stu-id="6f152-228">You can also do this by using hello Azure portal or [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span></span>

<span data-ttu-id="6f152-229">Tento kurz ukazuje, jak toomanage prostředky s AMSE.</span><span class="sxs-lookup"><span data-stu-id="6f152-229">This tutorial shows you how toomanage assets with AMSE.</span></span> <span data-ttu-id="6f152-230">Existují dva způsoby tooadd soubory tooan asset:</span><span class="sxs-lookup"><span data-stu-id="6f152-230">There are two ways tooadd files tooan asset:</span></span>

* <span data-ttu-id="6f152-231">Vytvořte místní složku, zkopírujte hello dva soubory v něm a přetažení hello složky toohello **Asset** kartě.</span><span class="sxs-lookup"><span data-stu-id="6f152-231">Create a local folder, copy hello two files in it, and drag and drop hello folder toohello **Asset** tab.</span></span>
* <span data-ttu-id="6f152-232">Nahrát videosoubor hello jako prostředek, zobrazit informace o evidenčním hello, přejděte toohello kartě soubory a odeslat další soubor (logo).</span><span class="sxs-lookup"><span data-stu-id="6f152-232">Upload hello video file as an asset, display hello asset information, go toohello files tab, and upload an additional file (logo).</span></span>

> [!NOTE]
> <span data-ttu-id="6f152-233">Ujistěte se, že tooset primární soubor v hello asset (hello hlavní video soubor).</span><span class="sxs-lookup"><span data-stu-id="6f152-233">Make sure tooset a primary file in hello asset (hello main video file).</span></span>

![Soubory prostředků v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

<span data-ttu-id="6f152-235">*Soubory prostředků v AMSE*</span><span class="sxs-lookup"><span data-stu-id="6f152-235">*Asset files in AMSE*</span></span>

<span data-ttu-id="6f152-236">Vyberte hello asset a zvolte tooencode ho pomocí kodéru Premium.</span><span class="sxs-lookup"><span data-stu-id="6f152-236">Select hello asset and choose tooencode it with Premium Encoder.</span></span> <span data-ttu-id="6f152-237">Nahrát hello pracovního postupu a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="6f152-237">Upload hello workflow and select it.</span></span>

<span data-ttu-id="6f152-238">Klikněte na tlačítko hello, toopass procesor toohello dat a přidejte následující XML tooset hello runtime vlastnosti hello:</span><span class="sxs-lookup"><span data-stu-id="6f152-238">Click hello button toopass data toohello processor, and add hello following XML tooset hello runtime properties:</span></span>

![Kodér úrovně Premium v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

<span data-ttu-id="6f152-240">*Kodér úrovně Premium v AMSE*</span><span class="sxs-lookup"><span data-stu-id="6f152-240">*Premium Encoder in AMSE*</span></span>

<span data-ttu-id="6f152-241">Vložte následující XML data hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-241">Then, paste hello following XML data.</span></span> <span data-ttu-id="6f152-242">Potřebujete název hello toospecify hello videosoubor hello vstupní soubor média a primarySourceFile.</span><span class="sxs-lookup"><span data-stu-id="6f152-242">You need toospecify hello name of hello video file for both hello Media File Input and primarySourceFile.</span></span> <span data-ttu-id="6f152-243">Zadejte název hello hello název souboru pro hello logo příliš.</span><span class="sxs-lookup"><span data-stu-id="6f152-243">Specify hello name of hello file name for hello logo too.</span></span>

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

<span data-ttu-id="6f152-245">*setRuntimeProperties*</span><span class="sxs-lookup"><span data-stu-id="6f152-245">*setRuntimeProperties*</span></span>

<span data-ttu-id="6f152-246">Pokud používáte toocreate hello .NET SDK a spusťte úlohu hello, má tato data XML toobe předány jako hello konfigurační řetězec.</span><span class="sxs-lookup"><span data-stu-id="6f152-246">If you use hello .NET SDK toocreate and run hello task, this XML data has toobe passed as hello configuration string.</span></span>

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

<span data-ttu-id="6f152-247">Po dokončení úlohy hello výstup hello MP4 souboru v hello asset zobrazí hello překrytí!</span><span class="sxs-lookup"><span data-stu-id="6f152-247">After hello job is complete, hello MP4 file in hello output asset displays hello overlay!</span></span>

![Překrytí na hello video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

<span data-ttu-id="6f152-249">*Překrytí na hello video*</span><span class="sxs-lookup"><span data-stu-id="6f152-249">*Overlay on hello video*</span></span>

<span data-ttu-id="6f152-250">Můžete si stáhnout hello ukázkového pracovního postupu z [Githubu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span><span class="sxs-lookup"><span data-stu-id="6f152-250">You can download hello sample workflow from [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span></span>

## <a name="example-2--multiple-audio-language-encoding"></a><span data-ttu-id="6f152-251">Příklad 2: Více zvuk jazyk kódování</span><span class="sxs-lookup"><span data-stu-id="6f152-251">Example 2 : Multiple audio language encoding</span></span>

<span data-ttu-id="6f152-252">Příkladem zvuk vícejazyčné kódování workfkow je k dispozici v [Githubu](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span><span class="sxs-lookup"><span data-stu-id="6f152-252">An example of multiple audio language encoding workfkow is available in [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span></span>

<span data-ttu-id="6f152-253">Tato složka obsahuje ukázkový pracovní postup, který lze použít tooencode MXF souboru tooa více MP4 soubory asset s více zvukových stop.</span><span class="sxs-lookup"><span data-stu-id="6f152-253">This folder contains a sample workflow which can be used tooencode a MXF file tooa multi MP4 files asset with multiple audio tracks.</span></span>

<span data-ttu-id="6f152-254">Tento pracovní postup předpokládá, že soubor MXF hello obsahuje jeden zvuk sledovat; Další zvukových stop Hello mají být předány jako samostatné zvukové soubory (WAV nebo MP4...).</span><span class="sxs-lookup"><span data-stu-id="6f152-254">This workflow assumes that hello MXF file contains one audio track ; hello additional audio tracks should be passed as seperate audio files (WAV or MP4...).</span></span>

<span data-ttu-id="6f152-255">tooencode, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="6f152-255">tooencode, please follow these steps:</span></span>

* <span data-ttu-id="6f152-256">Vytvoření prostředku služby Media Services s hello MXF souboru a hello zvukové soubory (0 zvukové soubory too18).</span><span class="sxs-lookup"><span data-stu-id="6f152-256">Create a Media Services asset with hello MXF file and hello Audio files (0 too18 audio files).</span></span>
* <span data-ttu-id="6f152-257">Ujistěte se, že tento soubor MXF hello je nastaven jako primární soubor.</span><span class="sxs-lookup"><span data-stu-id="6f152-257">Make sure that hello MXF file is set as a primary file.</span></span>
* <span data-ttu-id="6f152-258">Vytvoření úlohy a úlohy pomocí hello kodér úrovně Premium pracovní postup procesoru.</span><span class="sxs-lookup"><span data-stu-id="6f152-258">Create a job and a task using hello Premium Workflow Encoder processor.</span></span> <span data-ttu-id="6f152-259">Použití pracovního postupu hello zadaný (MultiMP4-1080p-19audio-v1.workflow).</span><span class="sxs-lookup"><span data-stu-id="6f152-259">Use hello workflow provided (MultiMP4-1080p-19audio-v1.workflow).</span></span>
* <span data-ttu-id="6f152-260">Předejte hello setruntime.xml data toohello úloh (Pokud používáte Azure Media Services Explorer, "předat pracovní postup xml data toohello" hello použití – tlačítko).</span><span class="sxs-lookup"><span data-stu-id="6f152-260">Pass hello setruntime.xml data toohello task (if you use Azure Media Services Explorer, use hello “pass xml data toohello workflow” button).</span></span>
  * <span data-ttu-id="6f152-261">Aktualizujte prosím hello data toospecify hello správný soubor názvy a jazyky značky XML.</span><span class="sxs-lookup"><span data-stu-id="6f152-261">Please update hello XML data toospecify hello correct file names and languages tags.</span></span>
  * <span data-ttu-id="6f152-262">pracovní postup Hello obsahuje zvuk komponenty s názvem tooAudio zvuk 1 18.</span><span class="sxs-lookup"><span data-stu-id="6f152-262">hello workflow has audio components named Audio 1 tooAudio 18.</span></span>
  * <span data-ttu-id="6f152-263">RFC5646 je podporována pro značky jazyka hello.</span><span class="sxs-lookup"><span data-stu-id="6f152-263">RFC5646 is supported for hello language tag.</span></span>

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

* <span data-ttu-id="6f152-264">Hello kódovaný asset bude obsahovat více jazyk zvukových stop a musí být tyto sleduje vybrat v Azure Media Player.</span><span class="sxs-lookup"><span data-stu-id="6f152-264">hello encoded asset will contain multi language audio tracks and these tracks should be selectable in Azure Media Player.</span></span>

## <a name="see-also"></a><span data-ttu-id="6f152-265">Viz také</span><span class="sxs-lookup"><span data-stu-id="6f152-265">See also</span></span>
* [<span data-ttu-id="6f152-266">Představení Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="6f152-266">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="6f152-267">Jak toouse Premium kódování ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="6f152-267">How toouse Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="6f152-268">Kódování obsahu na vyžádání pomocí Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="6f152-268">Encoding on-demand content with Azure Media Services</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)
* [<span data-ttu-id="6f152-269">Formáty Media Encoder Premium pracovního postupu a kodeky</span><span class="sxs-lookup"><span data-stu-id="6f152-269">Media Encoder Premium Workflow formats and codecs</span></span>](media-services-premium-workflow-encoder-formats.md)
* [<span data-ttu-id="6f152-270">Ukázkové soubory pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="6f152-270">Sample workflow files</span></span>](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [<span data-ttu-id="6f152-271">Nástroj pro Azure Media Services Explorer</span><span class="sxs-lookup"><span data-stu-id="6f152-271">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="6f152-272">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="6f152-272">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6f152-273">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="6f152-273">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
