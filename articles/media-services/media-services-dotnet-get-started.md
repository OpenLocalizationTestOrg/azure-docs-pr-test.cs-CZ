---
title: "Začínáme s doručováním obsahu na vyžádání pomocí technologie .NET | Dokumentace Microsoftu"
description: "Tento kurz vás provede jednotlivými kroky implementace aplikace pro doručování obsahu na vyžádání pomocí služeb Azure Media Services a .NET."
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
ms.openlocfilehash: f0be787ba1ccee067fb1d7e6a6554be32f886089
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a><span data-ttu-id="33921-103">Začínáme s doručováním obsahu na vyžádání pomocí sady SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="33921-103">Get started with delivering content on demand using .NET SDK</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="33921-104">Tento kurz vás provede jednotlivými kroky implementace základní aplikace pro doručování videa na vyžádání (Video-on-Demand) pomocí služby Azure Media Services (AMS) s použitím sady SDK služby Azure Media Services .NET.</span><span class="sxs-lookup"><span data-stu-id="33921-104">This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using the Azure Media Services .NET SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33921-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="33921-105">Prerequisites</span></span>

<span data-ttu-id="33921-106">K dokončení kurzu potřebujete následující:</span><span class="sxs-lookup"><span data-stu-id="33921-106">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="33921-107">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="33921-107">An Azure account.</span></span> <span data-ttu-id="33921-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="33921-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="33921-109">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="33921-109">A Media Services account.</span></span> <span data-ttu-id="33921-110">Pokud chcete vytvořit účet Media Services, přečtěte si článek [Jak vytvořit účet Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="33921-110">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="33921-111">Rozhraní .NET 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="33921-111">.NET Framework 4.0 or later.</span></span>
* <span data-ttu-id="33921-112">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33921-112">Visual Studio.</span></span>

<span data-ttu-id="33921-113">Tento kurz sestává z následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="33921-113">This tutorial includes the following tasks:</span></span>

1. <span data-ttu-id="33921-114">Spuštění koncového bodu streamování (pomocí webu Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="33921-114">Start streaming endpoint (using the Azure portal).</span></span>
2. <span data-ttu-id="33921-115">Vytvoření a konfigurace projektu Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="33921-115">Create and configure a Visual Studio project.</span></span>
3. <span data-ttu-id="33921-116">Připojení k účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="33921-116">Connect to the Media Services account.</span></span>
2. <span data-ttu-id="33921-117">Nahrání videosouboru</span><span class="sxs-lookup"><span data-stu-id="33921-117">Upload a video file.</span></span>
3. <span data-ttu-id="33921-118">Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="33921-118">Encode the source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="33921-119">Publikování assetu a získání adres URL streamování a progresivního stahování</span><span class="sxs-lookup"><span data-stu-id="33921-119">Publish the asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="33921-120">Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="33921-120">Play your content.</span></span>

## <a name="overview"></a><span data-ttu-id="33921-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="33921-121">Overview</span></span>
<span data-ttu-id="33921-122">Tento kurz vás provede jednotlivými kroky implementace aplikace pro doručování videa na vyžádání (VoD, Video-on-Demand) pomocí sady SDK služby Azure Media Services (AMS) pro .NET.</span><span class="sxs-lookup"><span data-stu-id="33921-122">This tutorial walks you through the steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) SDK for .NET.</span></span>

<span data-ttu-id="33921-123">Kurz představuje základní pracovní postup služby Media Services a nejběžnější programovací objekty a úkoly, které Media Services vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="33921-123">The tutorial introduces the basic Media Services workflow and the most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="33921-124">Po dokončení kurzu bude umět streamovat nebo progresivně stáhnout ukázkový multimediální soubor, který jste odeslali, zakódovali a stáhli.</span><span class="sxs-lookup"><span data-stu-id="33921-124">At the completion of the tutorial, you will be able to stream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

### <a name="ams-model"></a><span data-ttu-id="33921-125">Model AMS</span><span class="sxs-lookup"><span data-stu-id="33921-125">AMS model</span></span>

<span data-ttu-id="33921-126">Následující obrázek ukazuje některé z nejčastěji používaných objektů při vývoji aplikace VoD na základě modelu Media Services OData.</span><span class="sxs-lookup"><span data-stu-id="33921-126">The following image shows some of the most commonly used objects when developing VoD applications against the Media Services OData model.</span></span>

<span data-ttu-id="33921-127">Kliknutím na obrázek zobrazíte jeho plnou velikost.</span><span class="sxs-lookup"><span data-stu-id="33921-127">Click the image to view it full size.</span></span>  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="33921-128">Celý model můžete zobrazit [zde](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="33921-128">You can view the whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="start-streaming-endpoints-using-the-azure-portal"></a><span data-ttu-id="33921-129">Spuštění koncového bodu streamování pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33921-129">Start streaming endpoints using the Azure portal</span></span>

<span data-ttu-id="33921-130">Při práci se službou Azure Media Services je jedním z nejběžnější scénářů doručování videa prostřednictvím streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="33921-130">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="33921-131">Služba Media Services poskytuje dynamické balení, které umožňuje doručovat obsah s adaptivní přenosovou rychlostí s kódováním MP4 ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming). není přitom potřeba ukládat předem zabalené verze pro každý z těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="33921-131">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="33921-132">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="33921-132">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="33921-133">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="33921-133">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

<span data-ttu-id="33921-134">Pokud chcete spustit koncový bod streamování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="33921-134">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="33921-135">Přihlaste se na [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="33921-135">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="33921-136">V okně Nastavení klikněte na Koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="33921-136">In the Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="33921-137">Klikněte na výchozí koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="33921-137">Click the default streaming endpoint.</span></span>

    <span data-ttu-id="33921-138">Zobrazí se okno VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI.</span><span class="sxs-lookup"><span data-stu-id="33921-138">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="33921-139">Klikněte na ikonu Spustit.</span><span class="sxs-lookup"><span data-stu-id="33921-139">Click the Start icon.</span></span>
5. <span data-ttu-id="33921-140">Kliknutím na tlačítko Uložit uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="33921-140">Click the Save button to save your changes.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="33921-141">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="33921-141">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="33921-142">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="33921-142">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="33921-143">Vytvořte novou složku (kdekoli na místním disku) a zkopírujte si soubor .mp4, který chcete kódovat a streamovat nebo progresivně stahovat.</span><span class="sxs-lookup"><span data-stu-id="33921-143">Create a new folder (folder can be anywhere on your local drive) and copy an .mp4 file that you want to encode and stream or progressively download.</span></span> <span data-ttu-id="33921-144">V tomto příkladu používáme cestu „C:\VideoFiles“.</span><span class="sxs-lookup"><span data-stu-id="33921-144">In this example, the "C:\VideoFiles" path is used.</span></span>

## <a name="connect-to-the-media-services-account"></a><span data-ttu-id="33921-145">Připojení k účtu Media Services</span><span class="sxs-lookup"><span data-stu-id="33921-145">Connect to the Media Services account</span></span>

<span data-ttu-id="33921-146">Když službu Media Services používáte s rozhraním .NET, musíte třídu **CloudMediaContext** používat pro většinu programovacích úloh: připojení k účtu Media Services, vytváření, aktualizace, otevírání a odstraňování následujících objektů: prostředky, soubory prostředků, úlohy, zásady přístupu, lokátory atd.</span><span class="sxs-lookup"><span data-stu-id="33921-146">When using Media Services with .NET, you must use the **CloudMediaContext** class for most Media Services programming tasks: connecting to Media Services account; creating, updating, accessing, and deleting the following objects: assets, asset files, jobs, access policies, locators, etc.</span></span>

<span data-ttu-id="33921-147">Přepište výchozí třídu Program následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="33921-147">Overwrite the default Program class with the following code.</span></span> <span data-ttu-id="33921-148">Kód ukazuje, jak číst hodnoty připojení ze souboru App.config a jak vytvořit objekt **CloudMediaContext**, abyste se mohli připojit ke službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="33921-148">The code demonstrates how to read the connection values from the App.config file and how to create the **CloudMediaContext** object in order to connect to Media Services.</span></span> <span data-ttu-id="33921-149">Další informace najdete v tématu popisujícím [připojení k rozhraní API služby Media Services](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="33921-149">For more information, see [connecting to the Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

<span data-ttu-id="33921-150">Nezapomeňte aktualizovat název souboru a cestu podle umístění multimediálního souboru.</span><span class="sxs-lookup"><span data-stu-id="33921-150">Make sure to update the file name and path to where you have your media file.</span></span>

<span data-ttu-id="33921-151">Funkce **Main** volá metody, které si definujeme v této části.</span><span class="sxs-lookup"><span data-stu-id="33921-151">The **Main** function calls methods that will be defined further in this section.</span></span>

> [!NOTE]
> <span data-ttu-id="33921-152">Dokud nepřidáte definice pro všechny funkce, budou se vám zobrazovat chyby kompilace.</span><span class="sxs-lookup"><span data-stu-id="33921-152">You will be getting compilation errors until you add definitions for all the functions.</span></span>

    class Program
    {
        // Read values from the App.config file.
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

            // Add calls to methods defined in this section.
            // Make sure to update the file name and path to where you have your media file.
            IAsset inputAsset =
            UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

            IAsset encodedAsset =
            EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

            PublishAssetGetURLs(encodedAsset);
        }
        catch (Exception exception)
        {
            // Parse the XML error message in the Media Services response and create a new
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

## <a name="create-a-new-asset-and-upload-a-video-file"></a><span data-ttu-id="33921-153">Vytvoření nového prostředku a odeslání videosouboru</span><span class="sxs-lookup"><span data-stu-id="33921-153">Create a new asset and upload a video file</span></span>

<span data-ttu-id="33921-154">Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku.</span><span class="sxs-lookup"><span data-stu-id="33921-154">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="33921-155">Entita **Prostředek** může obsahovat video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků (a metadata o těchto souborech.)  Jakmile soubory odešlete, bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="33921-155">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks, and closed caption files (and the metadata about these files.)  Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span> <span data-ttu-id="33921-156">Soubory v prostředku se nazývají **soubory prostředku**.</span><span class="sxs-lookup"><span data-stu-id="33921-156">The files in the asset are called **Asset Files**.</span></span>

<span data-ttu-id="33921-157">Metoda **UploadFile** definovaná níže volá metodu **CreateFromFile** (definovanou v rozšíření sady SDK pro .NET).</span><span class="sxs-lookup"><span data-stu-id="33921-157">The **UploadFile** method defined below calls **CreateFromFile** (defined in .NET SDK Extensions).</span></span> <span data-ttu-id="33921-158">**CreateFromFile** vytvoří nový prostředek, do kterého se zadaný zdrojový soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="33921-158">**CreateFromFile** creates a new asset into which the specified source file is uploaded.</span></span>

<span data-ttu-id="33921-159">Metoda **CreateFromFile** přijímá metodu **AssetCreationOptions**, která vám umožňuje určit jednu z následujících možností vytvoření prostředku:</span><span class="sxs-lookup"><span data-stu-id="33921-159">The **CreateFromFile** method takes **AssetCreationOptions** which lets you specify one of the following asset creation options:</span></span>

* <span data-ttu-id="33921-160">**Žádné** – nepoužívá se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="33921-160">**None** - No encryption is used.</span></span> <span data-ttu-id="33921-161">Toto je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="33921-161">This is the default value.</span></span> <span data-ttu-id="33921-162">Pamatujte, že při použití této možnosti není váš obsah chráněný během přenosu ani při umístění v úložišti.</span><span class="sxs-lookup"><span data-stu-id="33921-162">Note that when using this option, your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="33921-163">Pokud chcete pomocí progresivního stahování dodávat obsah ve formátu MP4, použijte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="33921-163">If you plan to deliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="33921-164">**StorageEncrypted** – tuto možnost použijte k místnímu šifrování nešifrovaného obsahu pomocí 256bitového šifrování AES (Advanced Encryption Standard). Obsah je poté odeslán do služby Azure Storage, kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="33921-164">**StorageEncrypted** - Use this option to encrypt your clear content locally using Advanced Encryption Standard (AES)-256 bit encryption, which then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="33921-165">Prostředky chráněné pomocí šifrování úložiště jsou před kódováním automaticky bez šifrování umístěny do systému souborů EFS a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="33921-165">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="33921-166">Případem primárního použití šifrování úložiště je situace, kdy chcete zabezpečit soubory s vysoce kvalitními vstupními multimediálními soubory pomocí silného šifrování na disku.</span><span class="sxs-lookup"><span data-stu-id="33921-166">The primary use case for Storage Encryption is when you want to secure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="33921-167">**CommonEncryptionProtected** – tuto možnost použijte, pokud nahráváte zašifrovaný obsah chráněný běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chráněná pomocí DRM s technologií PlayReady).</span><span class="sxs-lookup"><span data-stu-id="33921-167">**CommonEncryptionProtected** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="33921-168">**EnvelopeEncryptionProtected** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES.</span><span class="sxs-lookup"><span data-stu-id="33921-168">**EnvelopeEncryptionProtected** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="33921-169">Pamatujte, že soubory musí být zakódované a zašifrované pomocí správce transformací.</span><span class="sxs-lookup"><span data-stu-id="33921-169">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>

<span data-ttu-id="33921-170">Metoda **CreateFromFile** umožňuje také určení zpětného volání za účelem hlášení průběhu odesílání souboru.</span><span class="sxs-lookup"><span data-stu-id="33921-170">The **CreateFromFile** method also lets you specify a callback in order to report the upload progress of the file.</span></span>

<span data-ttu-id="33921-171">V následujícím příkladu určíme pro možnosti prostředku možnost **Žádné**.</span><span class="sxs-lookup"><span data-stu-id="33921-171">In the following example, we specify **None** for the asset options.</span></span>

<span data-ttu-id="33921-172">Přidejte následující metodu do třídy Program.</span><span class="sxs-lookup"><span data-stu-id="33921-172">Add the following method to the Program class.</span></span>

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


## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><span data-ttu-id="33921-173">Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="33921-173">Encode the source file into a set of adaptive bitrate MP4 files</span></span>
<span data-ttu-id="33921-174">Po zpracování prostředků ve službě Media Services a před jejich předáním klientům můžete média zakódovat, transmuxovat, označit vodoznakem a tak dále.</span><span class="sxs-lookup"><span data-stu-id="33921-174">After ingesting assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered to clients.</span></span> <span data-ttu-id="33921-175">Tyto aktivity se plánují a spouštějí s několika instancemi role na pozadí, abyste měli zajištěný vysoký výkon a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="33921-175">These activities are scheduled and run against multiple background role instances to ensure high performance and availability.</span></span> <span data-ttu-id="33921-176">Tyto aktivity se nazývají úlohy a každá úloha se skládá z atomických úloh, které vykonávají samotnou práci na souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="33921-176">These activities are called Jobs, and each Job is composed of atomic Tasks that do the actual work on the Asset file.</span></span>

<span data-ttu-id="33921-177">Jak jsem už zmínili dřív, při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům.</span><span class="sxs-lookup"><span data-stu-id="33921-177">As was mentioned earlier, when working with Azure Media Services, one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="33921-178">Služba Media Services umí dynamicky balit sady souborů MP4 s adaptivní přenosovou rychlostí do následujících formátů: HTTP Live Streaming (HLS), technologie Smooth Streaming a MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="33921-178">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of the following formats: HTTP Live Streaming (HLS), Smooth Streaming, and MPEG DASH.</span></span>

<span data-ttu-id="33921-179">Pokud chcete využít výhod dynamického balení, musíte soubor mezzanine (zdrojový soubor) zakódovat nebo převést na sadu souborů MP4 nebo Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="33921-179">To take advantage of dynamic packaging, you need to encode or transcode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span>  

<span data-ttu-id="33921-180">Následující kód ukazuje, jak se odeslat kódovací úlohu.</span><span class="sxs-lookup"><span data-stu-id="33921-180">The following code shows how to submit an encoding job.</span></span> <span data-ttu-id="33921-181">Úloha obsahuje jednu úlohu, která určuje převod souboru mezzanine do sady souborů MP4 s adaptivní přenosovou rychlostí pomocí **standardu pro kodér médií**.</span><span class="sxs-lookup"><span data-stu-id="33921-181">The job contains one task that specifies to transcode the mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="33921-182">Kód předá úlohu a čeká na její dokončení.</span><span class="sxs-lookup"><span data-stu-id="33921-182">The code submits the job and waits until it is completed.</span></span>

<span data-ttu-id="33921-183">Po dokončení úlohy budete moct streamovat prostředek nebo progresivně stahovat soubory MP4, které jste vytvořili překódováním.</span><span class="sxs-lookup"><span data-stu-id="33921-183">Once the job is completed, you would be able to stream your asset or progressively download MP4 files that were created as a result of transcoding.</span></span>

<span data-ttu-id="33921-184">Přidejte následující metodu do třídy Program.</span><span class="sxs-lookup"><span data-stu-id="33921-184">Add the following method to the Program class.</span></span>

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "Adaptive Streaming",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
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

## <a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a><span data-ttu-id="33921-185">Publikování prostředku a získání adres URL pro streamování a progresivní stahování</span><span class="sxs-lookup"><span data-stu-id="33921-185">Publish the asset and get URLs for streaming and progressive download</span></span>

<span data-ttu-id="33921-186">Pokud chcete prostředek streamovat nebo stáhnout, musíte ho nejdřív „publikovat“ vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="33921-186">To stream or download an asset, you first need to "publish" it by creating a locator.</span></span> <span data-ttu-id="33921-187">Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu.</span><span class="sxs-lookup"><span data-stu-id="33921-187">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="33921-188">Media Services podporuje dva typy lokátorů: lokátory OnDemandOrigin, používané ke streamování médií (například MPEG DASH, HLS nebo technologie Smooth Streaming), a lokátory s přístupovým podpisem (SAS), používané ke stahování multimediálních souborů (další informace o lokátorech SAS najdete na [tomto](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogu).</span><span class="sxs-lookup"><span data-stu-id="33921-188">Media Services supports two types of locators: OnDemandOrigin locators, used to stream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used to download media files (for more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span></span>

### <a name="some-details-about-url-formats"></a><span data-ttu-id="33921-189">Podrobnosti o formátech adres URL</span><span class="sxs-lookup"><span data-stu-id="33921-189">Some details about URL formats</span></span>

<span data-ttu-id="33921-190">Po vytvoření lokátorů můžete sestavit adresy URL, které budou sloužit ke streamování a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="33921-190">After you create the locators, you can build the URLs that would be used to stream or download your files.</span></span> <span data-ttu-id="33921-191">Ukázka v tomto kurzu budou výstupy adresy URL, které můžete vložit do příslušných prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="33921-191">The sample in this tutorial will output URLs that you can paste in appropriate browsers.</span></span> <span data-ttu-id="33921-192">V této části je pár příkladů, jak vypadají jiné formáty.</span><span class="sxs-lookup"><span data-stu-id="33921-192">This section just gives short examples of what different formats look like.</span></span>

#### <a name="a-streaming-url-for-mpeg-dash-has-the-following-format"></a><span data-ttu-id="33921-193">Streamovací adresa URL pro MPEG DASH má následující formát:</span><span class="sxs-lookup"><span data-stu-id="33921-193">A streaming URL for MPEG DASH has the following format:</span></span>

<span data-ttu-id="33921-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span><span class="sxs-lookup"><span data-stu-id="33921-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span></span>

#### <a name="a-streaming-url-for-hls-has-the-following-format"></a><span data-ttu-id="33921-195">Streamovací adresa URL pro HLS má následující formát:</span><span class="sxs-lookup"><span data-stu-id="33921-195">A streaming URL for HLS has the following format:</span></span>

<span data-ttu-id="33921-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span><span class="sxs-lookup"><span data-stu-id="33921-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span></span>

#### <a name="a-streaming-url-for-smooth-streaming-has-the-following-format"></a><span data-ttu-id="33921-197">Streamovací adresa URL pro technologii Smooth Streaming má následující formát:</span><span class="sxs-lookup"><span data-stu-id="33921-197">A streaming URL for Smooth Streaming has the following format:</span></span>

<span data-ttu-id="33921-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="33921-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>


#### <a name="a-sas-url-used-to-download-files-has-the-following-format"></a><span data-ttu-id="33921-199">SAS adresa URL používaná ke stahování souborů má následující formát:</span><span class="sxs-lookup"><span data-stu-id="33921-199">A SAS URL used to download files has the following format:</span></span>

<span data-ttu-id="33921-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span><span class="sxs-lookup"><span data-stu-id="33921-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span></span>

<span data-ttu-id="33921-201">Rozšíření sady SDK služby Media Services pro .NET  nabízejí užitečné pomocné metody, které vracejí formátované adresy URL publikovaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="33921-201">Media Services .NET SDK extensions provide convenient helper methods that return formatted URLs for the published asset.</span></span>

<span data-ttu-id="33921-202">Následující kód používá rozšíření sady SDK pro .NET k vytvoření lokátorů a k získání adres URL pro streamování a progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="33921-202">The following code uses .NET SDK Extensions to create locators and to get streaming and progressive download URLs.</span></span> <span data-ttu-id="33921-203">Kód také ukazuje způsob stahování souborů do místní složky.</span><span class="sxs-lookup"><span data-stu-id="33921-203">The code also shows how to download files to a local folder.</span></span>

<span data-ttu-id="33921-204">Přidejte následující metodu do třídy Program.</span><span class="sxs-lookup"><span data-stu-id="33921-204">Add the following method to the Program class.</span></span>

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
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

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a><span data-ttu-id="33921-205">Testování přehráváním obsahu</span><span class="sxs-lookup"><span data-stu-id="33921-205">Test by playing your content</span></span>

<span data-ttu-id="33921-206">Po spuštění programu definovaného v předchozí části se v okně konzoly zobrazí adresy URL, které se budou podobat následujícím.</span><span class="sxs-lookup"><span data-stu-id="33921-206">Once you run the program defined in the previous section, the URLs similar to the following will be displayed in the console window.</span></span>

<span data-ttu-id="33921-207">Adresa URL adaptivního streamování:</span><span class="sxs-lookup"><span data-stu-id="33921-207">Adaptive streaming URLs:</span></span>

<span data-ttu-id="33921-208">Technologie Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="33921-208">Smooth Streaming</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="33921-209">HLS</span><span class="sxs-lookup"><span data-stu-id="33921-209">HLS</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="33921-210">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="33921-210">MPEG DASH</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

<span data-ttu-id="33921-211">Adresa URL progresivního stahování (audio a video).</span><span class="sxs-lookup"><span data-stu-id="33921-211">Progressive download URLs (audio and video).</span></span>

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


<span data-ttu-id="33921-212">Abyste mohli streamovat video, vložte adresu URL do textového pole URL v [přehrávači Azure Media Services](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="33921-212">To stream your video, paste your URL in the URL textbox in the [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="33921-213">Pokud chcete otestovat progresivní stahování, vložte adresu URL do prohlížeče (například Internet Exploreru, Chromu nebo Safari).</span><span class="sxs-lookup"><span data-stu-id="33921-213">To test progressive download, paste a URL into a browser (for example, Internet Explorer, Chrome, or Safari).</span></span>

<span data-ttu-id="33921-214">Další informace najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="33921-214">For more information, see the following topics:</span></span>

- [<span data-ttu-id="33921-215">Přehrávání obsahu ve stávajících přehrávačích</span><span class="sxs-lookup"><span data-stu-id="33921-215">Playing your content with existing players</span></span>](media-services-playback-content-with-existing-players.md)
- [<span data-ttu-id="33921-216">Vývoj aplikací videopřehrávače</span><span class="sxs-lookup"><span data-stu-id="33921-216">Develop video player applications</span></span>](media-services-develop-video-players.md)
- [<span data-ttu-id="33921-217">Vložení videa adaptivního streamování MPEG-DASH do aplikace HTML5 se souborem DASH.js</span><span class="sxs-lookup"><span data-stu-id="33921-217">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a><span data-ttu-id="33921-218">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="33921-218">Download sample</span></span>
<span data-ttu-id="33921-219">Následující ukázka kódu obsahuje kód, který jste vytvořili v tomto kurzu: [ukázka](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="33921-219">The following code sample contains the code that you created in this tutorial: [sample](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="33921-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33921-220">Next Steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="33921-221">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="33921-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
