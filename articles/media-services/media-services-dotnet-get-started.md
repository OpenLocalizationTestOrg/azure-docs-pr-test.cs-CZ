---
title: "aaaGet začít s doručováním obsahu na vyžádání pomocí rozhraní .NET | Microsoft Docs"
description: "Tento kurz vás provede procesem hello kroky implementace aplikace pro doručování obsahu na vyžádání pomocí Azure Media Services pomocí rozhraní .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 4ca9394bd581e1d9062e5a008a410b2c058e017e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a><span data-ttu-id="75e45-103">Začínáme s doručováním obsahu na vyžádání pomocí sady SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="75e45-103">Get started with delivering content on demand using .NET SDK</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="75e45-104">Tento kurz vás provede procesem hello kroky implementace základní služby doručování obsahu vyžádání pomocí Azure Media Services (AMS) aplikace pomocí hello Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="75e45-104">This tutorial walks you through hello steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using hello Azure Media Services .NET SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75e45-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75e45-105">Prerequisites</span></span>

<span data-ttu-id="75e45-106">Hello následují požadované toocomplete hello kurzu:</span><span class="sxs-lookup"><span data-stu-id="75e45-106">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="75e45-107">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="75e45-107">An Azure account.</span></span> <span data-ttu-id="75e45-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75e45-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="75e45-109">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="75e45-109">A Media Services account.</span></span> <span data-ttu-id="75e45-110">toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="75e45-110">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="75e45-111">Rozhraní .NET 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="75e45-111">.NET Framework 4.0 or later.</span></span>
* <span data-ttu-id="75e45-112">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75e45-112">Visual Studio.</span></span>

<span data-ttu-id="75e45-113">Tento kurz zahrnuje hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="75e45-113">This tutorial includes hello following tasks:</span></span>

1. <span data-ttu-id="75e45-114">Spuštění (pomocí portálu Azure hello) koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="75e45-114">Start streaming endpoint (using hello Azure portal).</span></span>
2. <span data-ttu-id="75e45-115">Vytvoření a konfigurace projektu Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="75e45-115">Create and configure a Visual Studio project.</span></span>
3. <span data-ttu-id="75e45-116">Připojte toohello účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="75e45-116">Connect toohello Media Services account.</span></span>
2. <span data-ttu-id="75e45-117">Nahrání videosouboru</span><span class="sxs-lookup"><span data-stu-id="75e45-117">Upload a video file.</span></span>
3. <span data-ttu-id="75e45-118">Zakódujte hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="75e45-118">Encode hello source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="75e45-119">Publikujte hello asset a get streamování a progresivní stahování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="75e45-119">Publish hello asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="75e45-120">Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="75e45-120">Play your content.</span></span>

## <a name="overview"></a><span data-ttu-id="75e45-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="75e45-121">Overview</span></span>
<span data-ttu-id="75e45-122">Tento kurz vás provede procesem hello kroky implementace aplikace pro doručování obsahu na vyžádání pomocí Azure Media Services (AMS) sady SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="75e45-122">This tutorial walks you through hello steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) SDK for .NET.</span></span>

<span data-ttu-id="75e45-123">Hello kurz představuje hello základní pracovní postup služby Media Services a nejběžnější programovací objekty hello a úkoly vyžadované pro vývoj pro Media Services.</span><span class="sxs-lookup"><span data-stu-id="75e45-123">hello tutorial introduces hello basic Media Services workflow and hello most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="75e45-124">Na hello dokončení kurzu hello bude se mít toostream nebo progresivně stáhnout ukázkový mediální soubor, který nahráli, kódování a stáhnout.</span><span class="sxs-lookup"><span data-stu-id="75e45-124">At hello completion of hello tutorial, you will be able toostream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

### <a name="ams-model"></a><span data-ttu-id="75e45-125">Model AMS</span><span class="sxs-lookup"><span data-stu-id="75e45-125">AMS model</span></span>

<span data-ttu-id="75e45-126">Hello následující obrázek ukazuje některé objekty hello nejčastěji používaná při vývoji aplikace VoD vůči modelu hello Media Services OData.</span><span class="sxs-lookup"><span data-stu-id="75e45-126">hello following image shows some of hello most commonly used objects when developing VoD applications against hello Media Services OData model.</span></span>

<span data-ttu-id="75e45-127">Klikněte na tlačítko tooview hello bitové kopie je plná velikost.</span><span class="sxs-lookup"><span data-stu-id="75e45-127">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="75e45-128">Můžete zobrazit celý modelu hello [zde](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="75e45-128">You can view hello whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a><span data-ttu-id="75e45-129">Spustit pomocí portálu Azure hello koncové body streamování</span><span class="sxs-lookup"><span data-stu-id="75e45-129">Start streaming endpoints using hello Azure portal</span></span>

<span data-ttu-id="75e45-130">Při práci se službou Azure Media Services je jedním hello nejběžnějších scénářů doručování videa přes streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="75e45-130">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="75e45-131">Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver kódováním MP4 obsah ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) v běhu, aniž byste museli toostore předem zabalené vaší s adaptivní přenosovou rychlostí verze pro každý z těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="75e45-131">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="75e45-132">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="75e45-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="75e45-133">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="75e45-133">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="75e45-134">toostart hello koncový bod streamování, hello následující:</span><span class="sxs-lookup"><span data-stu-id="75e45-134">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="75e45-135">Přihlaste se na hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="75e45-135">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="75e45-136">V okně Nastavení hello klikněte Streaming koncové body.</span><span class="sxs-lookup"><span data-stu-id="75e45-136">In hello Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="75e45-137">Klikněte na tlačítko hello výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="75e45-137">Click hello default streaming endpoint.</span></span>

    <span data-ttu-id="75e45-138">Zobrazí se okno Hello výchozí podrobnosti koncový bod STREAMOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="75e45-138">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="75e45-139">Klikněte na ikonu Start hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-139">Click hello Start icon.</span></span>
5. <span data-ttu-id="75e45-140">Klikněte na tlačítko toosave tlačítko hello uložit provedené změny.</span><span class="sxs-lookup"><span data-stu-id="75e45-140">Click hello Save button toosave your changes.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="75e45-141">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="75e45-141">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="75e45-142">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="75e45-142">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="75e45-143">Vytvořte novou složku (složka může být kdekoli na místní disk) a zkopírujte soubor .mp4 má tooencode a datový proud nebo progresivně stahovat.</span><span class="sxs-lookup"><span data-stu-id="75e45-143">Create a new folder (folder can be anywhere on your local drive) and copy an .mp4 file that you want tooencode and stream or progressively download.</span></span> <span data-ttu-id="75e45-144">V tomto příkladu se používá cestu "C:\VideoFiles" hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-144">In this example, hello "C:\VideoFiles" path is used.</span></span>

## <a name="connect-toohello-media-services-account"></a><span data-ttu-id="75e45-145">Připojit toohello účtu Media Services</span><span class="sxs-lookup"><span data-stu-id="75e45-145">Connect toohello Media Services account</span></span>

<span data-ttu-id="75e45-146">Při použití Media Services pomocí rozhraní .NET, musíte použít hello **CloudMediaContext** třídy pro většinu programovacích úloh: připojení účtu tooMedia Services, vytváření, aktualizaci, přístup a odstraňování následující hello objektů: prostředky, soubory prostředků, úlohy, zásady přístupu, lokátory atd.</span><span class="sxs-lookup"><span data-stu-id="75e45-146">When using Media Services with .NET, you must use hello **CloudMediaContext** class for most Media Services programming tasks: connecting tooMedia Services account; creating, updating, accessing, and deleting hello following objects: assets, asset files, jobs, access policies, locators, etc.</span></span>

<span data-ttu-id="75e45-147">Přepište výchozí třídu Program hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="75e45-147">Overwrite hello default Program class with hello following code.</span></span> <span data-ttu-id="75e45-148">Hello kód ukazuje, jak hodnoty tooread hello připojení ze souboru App.config hello a jak toocreate hello **CloudMediaContext** objekt v pořadí tooconnect tooMedia služeb.</span><span class="sxs-lookup"><span data-stu-id="75e45-148">hello code demonstrates how tooread hello connection values from hello App.config file and how toocreate hello **CloudMediaContext** object in order tooconnect tooMedia Services.</span></span> <span data-ttu-id="75e45-149">Další informace najdete v tématu [připojení toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="75e45-149">For more information, see [connecting toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

<span data-ttu-id="75e45-150">Ujistěte se, že tooupdate hello soubor název a cesta toowhere, které máte mediálního souboru.</span><span class="sxs-lookup"><span data-stu-id="75e45-150">Make sure tooupdate hello file name and path toowhere you have your media file.</span></span>

<span data-ttu-id="75e45-151">Hello **hlavní** funkce volá metody, které si definujeme v této části.</span><span class="sxs-lookup"><span data-stu-id="75e45-151">hello **Main** function calls methods that will be defined further in this section.</span></span>

> [!NOTE]
> <span data-ttu-id="75e45-152">Se vám bude zobrazovat chyby při kompilaci dokud nepřidáte definice pro všechny funkce hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-152">You will be getting compilation errors until you add definitions for all hello functions.</span></span>

    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
        try
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Add calls toomethods defined in this section.
            // Make sure tooupdate hello file name and path toowhere you have your media file.
            IAsset inputAsset =
            UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

            IAsset encodedAsset =
            EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

            PublishAssetGetURLs(encodedAsset);
        }
        catch (Exception exception)
        {
            // Parse hello XML error message in hello Media Services response and create a new
            // exception with its content.
            exception = MediaServicesExceptionParser.Parse(exception);

            Console.Error.WriteLine(exception.Message);
        }
        finally
        {
            Console.ReadLine();
        }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a><span data-ttu-id="75e45-153">Vytvoření nového prostředku a odeslání videosouboru</span><span class="sxs-lookup"><span data-stu-id="75e45-153">Create a new asset and upload a video file</span></span>

<span data-ttu-id="75e45-154">Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku.</span><span class="sxs-lookup"><span data-stu-id="75e45-154">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="75e45-155">Hello **Asset** entita může obsahovat video, zvuk, bitové kopie, kolekci miniatur, textové stopy a titulků soubory (a hello metadata o těchto souborech.)  Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="75e45-155">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks, and closed caption files (and hello metadata about these files.)  Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span> <span data-ttu-id="75e45-156">soubory Hello v hello prostředku se nazývají **soubory prostředku**.</span><span class="sxs-lookup"><span data-stu-id="75e45-156">hello files in hello asset are called **Asset Files**.</span></span>

<span data-ttu-id="75e45-157">Hello **UploadFile** metoda definovaná níže volá metodu **CreateFromFile** (definovanou v rozšíření sady SDK pro .NET).</span><span class="sxs-lookup"><span data-stu-id="75e45-157">hello **UploadFile** method defined below calls **CreateFromFile** (defined in .NET SDK Extensions).</span></span> <span data-ttu-id="75e45-158">**CreateFromFile** vytvoří nový prostředek, do které hello zadaný zdrojový soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="75e45-158">**CreateFromFile** creates a new asset into which hello specified source file is uploaded.</span></span>

<span data-ttu-id="75e45-159">Hello **CreateFromFile** metoda trvá **AssetCreationOptions** která umožňuje určit jednu z následujících možností vytvoření prostředku hello:</span><span class="sxs-lookup"><span data-stu-id="75e45-159">hello **CreateFromFile** method takes **AssetCreationOptions** which lets you specify one of hello following asset creation options:</span></span>

* <span data-ttu-id="75e45-160">**Žádné** – nepoužívá se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="75e45-160">**None** - No encryption is used.</span></span> <span data-ttu-id="75e45-161">Toto je výchozí hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-161">This is hello default value.</span></span> <span data-ttu-id="75e45-162">Pamatujte, že při použití této možnosti není váš obsah chráněný během přenosu ani při umístění v úložišti.</span><span class="sxs-lookup"><span data-stu-id="75e45-162">Note that when using this option, your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="75e45-163">Tuto možnost použijte, pokud máte v plánu toodeliver MP4 pomocí progresivního stahování.</span><span class="sxs-lookup"><span data-stu-id="75e45-163">If you plan toodeliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="75e45-164">**StorageEncrypted** – použijte tuto možnost tooencrypt obsahu místně pomocí šifrování-256 bit Advanced Encryption (Standard AES), který odešle ho tooAzure úložiště, kde je uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="75e45-164">**StorageEncrypted** - Use this option tooencrypt your clear content locally using Advanced Encryption Standard (AES)-256 bit encryption, which then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="75e45-165">Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="75e45-165">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="75e45-166">Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure souborů vysoce kvalitními vstupními médii pomocí silného šifrování v klidovém stavu na disku.</span><span class="sxs-lookup"><span data-stu-id="75e45-166">hello primary use case for Storage Encryption is when you want toosecure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="75e45-167">**CommonEncryptionProtected** – tuto možnost použijte, pokud nahráváte zašifrovaný obsah chráněný běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chráněná pomocí DRM s technologií PlayReady).</span><span class="sxs-lookup"><span data-stu-id="75e45-167">**CommonEncryptionProtected** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="75e45-168">**EnvelopeEncryptionProtected** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES.</span><span class="sxs-lookup"><span data-stu-id="75e45-168">**EnvelopeEncryptionProtected** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="75e45-169">Všimněte si, že hello soubory musí být kódovaný a zašifrované pomocí Správce transformací.</span><span class="sxs-lookup"><span data-stu-id="75e45-169">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>

<span data-ttu-id="75e45-170">Hello **CreateFromFile** metoda kontrola umožňuje také určení zpětného volání v pořadí tooreport hello průběhu odesílání souboru hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-170">hello **CreateFromFile** method also lets you specify a callback in order tooreport hello upload progress of hello file.</span></span>

<span data-ttu-id="75e45-171">V následujícím příkladu hello, určíme **žádné** pro možnosti prostředku možnost hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-171">In hello following example, we specify **None** for hello asset options.</span></span>

<span data-ttu-id="75e45-172">Přidejte následující metody třídy Program toohello hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-172">Add hello following method toohello Program class.</span></span>

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


## <a name="encode-hello-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><span data-ttu-id="75e45-173">Kódování hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="75e45-173">Encode hello source file into a set of adaptive bitrate MP4 files</span></span>
<span data-ttu-id="75e45-174">Po zpracování prostředků ve službě Media Services, může být média zakódovat, transmuxovat, označit vodoznakem a tak dále, před jejich předáním tooclients.</span><span class="sxs-lookup"><span data-stu-id="75e45-174">After ingesting assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered tooclients.</span></span> <span data-ttu-id="75e45-175">Tyto aktivity jsou naplánované a spouštění více pozadí role instance tooensure vysoký výkon a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="75e45-175">These activities are scheduled and run against multiple background role instances tooensure high performance and availability.</span></span> <span data-ttu-id="75e45-176">Tyto aktivity se nazývají úlohy a každá úloha se skládá z atomických úloh, které hello samotnou práci na souboru Assetu hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-176">These activities are called Jobs, and each Job is composed of atomic Tasks that do hello actual work on hello Asset file.</span></span>

<span data-ttu-id="75e45-177">Jak již bylo zmíněno dříve, při práci se službou Azure Media Services, jedním hello nejběžnějších scénářů doručování adaptivní přenosové rychlosti streamování tooyour klientů.</span><span class="sxs-lookup"><span data-stu-id="75e45-177">As was mentioned earlier, when working with Azure Media Services, one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="75e45-178">Služba Media Services umí dynamicky balit sady souborů MP4 s adaptivní přenosovou rychlostí do jednoho z následujících formátů hello: HTTP Live Streaming (HLS), technologie Smooth Streaming a MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="75e45-178">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of hello following formats: HTTP Live Streaming (HLS), Smooth Streaming, and MPEG DASH.</span></span>

<span data-ttu-id="75e45-179">tootake výhod dynamického balení, potřebujete tooencode nebo převeďte soubor mezzanine (zdrojový) soubor do sady souborů MP4 nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="75e45-179">tootake advantage of dynamic packaging, you need tooencode or transcode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span>  

<span data-ttu-id="75e45-180">Hello následující kód ukazuje, jak toosubmit kódování úlohy.</span><span class="sxs-lookup"><span data-stu-id="75e45-180">hello following code shows how toosubmit an encoding job.</span></span> <span data-ttu-id="75e45-181">Hello úloha obsahuje jednu úlohu, která určuje souboru tootranscode hello mezzanine do sady s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi pomocí **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="75e45-181">hello job contains one task that specifies tootranscode hello mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="75e45-182">Kód Hello odešle hello úlohu a čeká, dokud nebude dokončena.</span><span class="sxs-lookup"><span data-stu-id="75e45-182">hello code submits hello job and waits until it is completed.</span></span>

<span data-ttu-id="75e45-183">Po dokončení úlohy hello by být možné toostream asset nebo progresivně stahovat soubory MP4, které jste vytvořili překódováním.</span><span class="sxs-lookup"><span data-stu-id="75e45-183">Once hello job is completed, you would be able toostream your asset or progressively download MP4 files that were created as a result of transcoding.</span></span>

<span data-ttu-id="75e45-184">Přidejte následující metody třídy Program toohello hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-184">Add hello following method toohello Program class.</span></span>

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task tootranscode hello specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "Adaptive Streaming",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit hello job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-hello-asset-and-get-urls-for-streaming-and-progressive-download"></a><span data-ttu-id="75e45-185">Publikujte hello asset a získání adres URL pro streamování a progresivní stahování</span><span class="sxs-lookup"><span data-stu-id="75e45-185">Publish hello asset and get URLs for streaming and progressive download</span></span>

<span data-ttu-id="75e45-186">toostream nebo stažení prostředek, je nejprve nutné příliš "publikovat" vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="75e45-186">toostream or download an asset, you first need too"publish" it by creating a locator.</span></span> <span data-ttu-id="75e45-187">Lokátory zajišťují přístup toofiles obsažené v hello asset.</span><span class="sxs-lookup"><span data-stu-id="75e45-187">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="75e45-188">Služba Media Services podporuje dva typy lokátorů: OnDemandOrigin lokátory použité toostream médií (například MPEG DASH, HLS nebo technologie Smooth Streaming) a přístupového podpisu (SAS), používá toodownload mediálních souborů (pro další informace o SAS lokátory najdete [to](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span><span class="sxs-lookup"><span data-stu-id="75e45-188">Media Services supports two types of locators: OnDemandOrigin locators, used toostream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used toodownload media files (for more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span></span>

### <a name="some-details-about-url-formats"></a><span data-ttu-id="75e45-189">Podrobnosti o formátech adres URL</span><span class="sxs-lookup"><span data-stu-id="75e45-189">Some details about URL formats</span></span>

<span data-ttu-id="75e45-190">Po vytvoření lokátorů hello, můžete vytvořit hello adresy URL, které by se použít toostream a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="75e45-190">After you create hello locators, you can build hello URLs that would be used toostream or download your files.</span></span> <span data-ttu-id="75e45-191">Ukázka Hello v tomto kurzu bude výstup adresy URL, které můžete vložit příslušné prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="75e45-191">hello sample in this tutorial will output URLs that you can paste in appropriate browsers.</span></span> <span data-ttu-id="75e45-192">V této části je pár příkladů, jak vypadají jiné formáty.</span><span class="sxs-lookup"><span data-stu-id="75e45-192">This section just gives short examples of what different formats look like.</span></span>

#### <a name="a-streaming-url-for-mpeg-dash-has-hello-following-format"></a><span data-ttu-id="75e45-193">Streamovací adresa URL pro MPEG DASH má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="75e45-193">A streaming URL for MPEG DASH has hello following format:</span></span>

<span data-ttu-id="75e45-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span><span class="sxs-lookup"><span data-stu-id="75e45-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span></span>

#### <a name="a-streaming-url-for-hls-has-hello-following-format"></a><span data-ttu-id="75e45-195">Streamovací adresa URL pro HLS má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="75e45-195">A streaming URL for HLS has hello following format:</span></span>

<span data-ttu-id="75e45-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span><span class="sxs-lookup"><span data-stu-id="75e45-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span></span>

#### <a name="a-streaming-url-for-smooth-streaming-has-hello-following-format"></a><span data-ttu-id="75e45-197">Streamovací adresa URL pro technologii Smooth Streaming má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="75e45-197">A streaming URL for Smooth Streaming has hello following format:</span></span>

<span data-ttu-id="75e45-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="75e45-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>


#### <a name="a-sas-url-used-toodownload-files-has-hello-following-format"></a><span data-ttu-id="75e45-199">SAS adresa URL používaná toodownload souborů má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="75e45-199">A SAS URL used toodownload files has hello following format:</span></span>

<span data-ttu-id="75e45-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span><span class="sxs-lookup"><span data-stu-id="75e45-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span></span>

<span data-ttu-id="75e45-201">Rozšíření sady Media Services .NET SDK poskytují užitečné pomocné metody, návratový formátu adresy URL pro hello publikovaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="75e45-201">Media Services .NET SDK extensions provide convenient helper methods that return formatted URLs for hello published asset.</span></span>

<span data-ttu-id="75e45-202">Hello následující kód používá rozšíření sady SDK pro .NET toocreate lokátory a tooget streamování a progresivního stahování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="75e45-202">hello following code uses .NET SDK Extensions toocreate locators and tooget streaming and progressive download URLs.</span></span> <span data-ttu-id="75e45-203">Hello kód také ukazuje, jak toodownload soubory tooa místní složky.</span><span class="sxs-lookup"><span data-stu-id="75e45-203">hello code also shows how toodownload files tooa local folder.</span></span>

<span data-ttu-id="75e45-204">Přidejte následující metody třídy Program toohello hello.</span><span class="sxs-lookup"><span data-stu-id="75e45-204">Add hello following method toohello Program class.</span></span>

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish hello output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get hello Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and hello Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get hello URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  hello streaming URLs.
        Console.WriteLine("Use hello following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display hello URLs for progressive download.
        Console.WriteLine("Use hello following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download hello output asset tooa local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files tooa local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a><span data-ttu-id="75e45-205">Testování přehráváním obsahu</span><span class="sxs-lookup"><span data-stu-id="75e45-205">Test by playing your content</span></span>

<span data-ttu-id="75e45-206">Po spuštění programu hello definovaného v předchozí části hello hello podobné následující toohello se zobrazí v okně konzoly hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="75e45-206">Once you run hello program defined in hello previous section, hello URLs similar toohello following will be displayed in hello console window.</span></span>

<span data-ttu-id="75e45-207">Adresa URL adaptivního streamování:</span><span class="sxs-lookup"><span data-stu-id="75e45-207">Adaptive streaming URLs:</span></span>

<span data-ttu-id="75e45-208">Technologie Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="75e45-208">Smooth Streaming</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="75e45-209">HLS</span><span class="sxs-lookup"><span data-stu-id="75e45-209">HLS</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="75e45-210">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="75e45-210">MPEG DASH</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

<span data-ttu-id="75e45-211">Adresa URL progresivního stahování (audio a video).</span><span class="sxs-lookup"><span data-stu-id="75e45-211">Progressive download URLs (audio and video).</span></span>

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


<span data-ttu-id="75e45-212">toostream videa, vložte adresu URL do pole Adresa URL hello v hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="75e45-212">toostream your video, paste your URL in hello URL textbox in hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="75e45-213">tootest progresivní stahování, vložte adresu URL do prohlížeče (například Internet Explorer, Chrome nebo Safari).</span><span class="sxs-lookup"><span data-stu-id="75e45-213">tootest progressive download, paste a URL into a browser (for example, Internet Explorer, Chrome, or Safari).</span></span>

<span data-ttu-id="75e45-214">Další informace najdete v tématu hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="75e45-214">For more information, see hello following topics:</span></span>

- [<span data-ttu-id="75e45-215">Přehrávání obsahu ve stávajících přehrávačích</span><span class="sxs-lookup"><span data-stu-id="75e45-215">Playing your content with existing players</span></span>](media-services-playback-content-with-existing-players.md)
- [<span data-ttu-id="75e45-216">Vývoj aplikací videopřehrávače</span><span class="sxs-lookup"><span data-stu-id="75e45-216">Develop video player applications</span></span>](media-services-develop-video-players.md)
- [<span data-ttu-id="75e45-217">Vložení videa adaptivního streamování MPEG-DASH do aplikace HTML5 se souborem DASH.js</span><span class="sxs-lookup"><span data-stu-id="75e45-217">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a><span data-ttu-id="75e45-218">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="75e45-218">Download sample</span></span>
<span data-ttu-id="75e45-219">Hello následující ukázka kódu obsahuje hello kód, který jste vytvořili v tomto kurzu: [ukázka](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="75e45-219">hello following code sample contains hello code that you created in this tutorial: [sample](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="75e45-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75e45-220">Next Steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="75e45-221">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="75e45-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
