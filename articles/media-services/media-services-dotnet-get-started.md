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
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Začínáme s doručováním obsahu na vyžádání pomocí sady SDK pro .NET
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Tento kurz vás provede procesem hello kroky implementace základní služby doručování obsahu vyžádání pomocí Azure Media Services (AMS) aplikace pomocí hello Azure Media Services .NET SDK.

## <a name="prerequisites"></a>Požadavky

Hello následují požadované toocomplete hello kurzu:

* Účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* Účet Media Services. toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).
* Rozhraní .NET 4.0 nebo novější.
* Visual Studio.

Tento kurz zahrnuje hello následující úlohy:

1. Spuštění (pomocí portálu Azure hello) koncový bod streamování.
2. Vytvoření a konfigurace projektu Visual Studia.
3. Připojte toohello účtu Media Services.
2. Nahrání videosouboru
3. Zakódujte hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.
4. Publikujte hello asset a get streamování a progresivní stahování adresy URL.  
5. Přehrání obsahu

## <a name="overview"></a>Přehled
Tento kurz vás provede procesem hello kroky implementace aplikace pro doručování obsahu na vyžádání pomocí Azure Media Services (AMS) sady SDK pro .NET.

Hello kurz představuje hello základní pracovní postup služby Media Services a nejběžnější programovací objekty hello a úkoly vyžadované pro vývoj pro Media Services. Na hello dokončení kurzu hello bude se mít toostream nebo progresivně stáhnout ukázkový mediální soubor, který nahráli, kódování a stáhnout.

### <a name="ams-model"></a>Model AMS

Hello následující obrázek ukazuje některé objekty hello nejčastěji používaná při vývoji aplikace VoD vůči modelu hello Media Services OData.

Klikněte na tlačítko tooview hello bitové kopie je plná velikost.  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

Můžete zobrazit celý modelu hello [zde](https://media.windows.net/API/$metadata?api-version=2.15).  

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a>Spustit pomocí portálu Azure hello koncové body streamování

Při práci se službou Azure Media Services je jedním hello nejběžnějších scénářů doručování videa přes streamování s adaptivní přenosovou rychlostí. Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver kódováním MP4 obsah ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) v běhu, aniž byste museli toostore předem zabalené vaší s adaptivní přenosovou rychlostí verze pro každý z těchto formátů streamování.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.

toostart hello koncový bod streamování, hello následující:

1. Přihlaste se na hello [portál Azure](https://portal.azure.com/).
2. V okně Nastavení hello klikněte Streaming koncové body.
3. Klikněte na tlačítko hello výchozího koncového bodu streamování.

    Zobrazí se okno Hello výchozí podrobnosti koncový bod STREAMOVÁNÍ.

4. Klikněte na ikonu Start hello.
5. Klikněte na tlačítko toosave tlačítko hello uložit provedené změny.

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

1. Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 
2. Vytvořte novou složku (složka může být kdekoli na místní disk) a zkopírujte soubor .mp4 má tooencode a datový proud nebo progresivně stahovat. V tomto příkladu se používá cestu "C:\VideoFiles" hello.

## <a name="connect-toohello-media-services-account"></a>Připojit toohello účtu Media Services

Při použití Media Services pomocí rozhraní .NET, musíte použít hello **CloudMediaContext** třídy pro většinu programovacích úloh: připojení účtu tooMedia Services, vytváření, aktualizaci, přístup a odstraňování následující hello objektů: prostředky, soubory prostředků, úlohy, zásady přístupu, lokátory atd.

Přepište výchozí třídu Program hello hello následující kód. Hello kód ukazuje, jak hodnoty tooread hello připojení ze souboru App.config hello a jak toocreate hello **CloudMediaContext** objekt v pořadí tooconnect tooMedia služeb. Další informace najdete v tématu [připojení toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).

Ujistěte se, že tooupdate hello soubor název a cesta toowhere, které máte mediálního souboru.

Hello **hlavní** funkce volá metody, které si definujeme v této části.

> [!NOTE]
> Se vám bude zobrazovat chyby při kompilaci dokud nepřidáte definice pro všechny funkce hello.

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

## <a name="create-a-new-asset-and-upload-a-video-file"></a>Vytvoření nového prostředku a odeslání videosouboru

Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku. Hello **Asset** entita může obsahovat video, zvuk, bitové kopie, kolekci miniatur, textové stopy a titulků soubory (a hello metadata o těchto souborech.)  Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování. soubory Hello v hello prostředku se nazývají **soubory prostředku**.

Hello **UploadFile** metoda definovaná níže volá metodu **CreateFromFile** (definovanou v rozšíření sady SDK pro .NET). **CreateFromFile** vytvoří nový prostředek, do které hello zadaný zdrojový soubor odešle.

Hello **CreateFromFile** metoda trvá **AssetCreationOptions** která umožňuje určit jednu z následujících možností vytvoření prostředku hello:

* **Žádné** – nepoužívá se žádné šifrování. Toto je výchozí hodnota hello. Pamatujte, že při použití této možnosti není váš obsah chráněný během přenosu ani při umístění v úložišti.
  Tuto možnost použijte, pokud máte v plánu toodeliver MP4 pomocí progresivního stahování.
* **StorageEncrypted** – použijte tuto možnost tooencrypt obsahu místně pomocí šifrování-256 bit Advanced Encryption (Standard AES), který odešle ho tooAzure úložiště, kde je uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku. Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure souborů vysoce kvalitními vstupními médii pomocí silného šifrování v klidovém stavu na disku.
* **CommonEncryptionProtected** – tuto možnost použijte, pokud nahráváte zašifrovaný obsah chráněný běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chráněná pomocí DRM s technologií PlayReady).
* **EnvelopeEncryptionProtected** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES. Všimněte si, že hello soubory musí být kódovaný a zašifrované pomocí Správce transformací.

Hello **CreateFromFile** metoda kontrola umožňuje také určení zpětného volání v pořadí tooreport hello průběhu odesílání souboru hello.

V následujícím příkladu hello, určíme **žádné** pro možnosti prostředku možnost hello.

Přidejte následující metody třídy Program toohello hello.

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


## <a name="encode-hello-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Kódování hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí
Po zpracování prostředků ve službě Media Services, může být média zakódovat, transmuxovat, označit vodoznakem a tak dále, před jejich předáním tooclients. Tyto aktivity jsou naplánované a spouštění více pozadí role instance tooensure vysoký výkon a dostupnost. Tyto aktivity se nazývají úlohy a každá úloha se skládá z atomických úloh, které hello samotnou práci na souboru Assetu hello.

Jak již bylo zmíněno dříve, při práci se službou Azure Media Services, jedním hello nejběžnějších scénářů doručování adaptivní přenosové rychlosti streamování tooyour klientů. Služba Media Services umí dynamicky balit sady souborů MP4 s adaptivní přenosovou rychlostí do jednoho z následujících formátů hello: HTTP Live Streaming (HLS), technologie Smooth Streaming a MPEG DASH.

tootake výhod dynamického balení, potřebujete tooencode nebo převeďte soubor mezzanine (zdrojový) soubor do sady souborů MP4 nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.  

Hello následující kód ukazuje, jak toosubmit kódování úlohy. Hello úloha obsahuje jednu úlohu, která určuje souboru tootranscode hello mezzanine do sady s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi pomocí **Media Encoder Standard**. Kód Hello odešle hello úlohu a čeká, dokud nebude dokončena.

Po dokončení úlohy hello by být možné toostream asset nebo progresivně stahovat soubory MP4, které jste vytvořili překódováním.

Přidejte následující metody třídy Program toohello hello.

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

## <a name="publish-hello-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publikujte hello asset a získání adres URL pro streamování a progresivní stahování

toostream nebo stažení prostředek, je nejprve nutné příliš "publikovat" vytvořením lokátoru. Lokátory zajišťují přístup toofiles obsažené v hello asset. Služba Media Services podporuje dva typy lokátorů: OnDemandOrigin lokátory použité toostream médií (například MPEG DASH, HLS nebo technologie Smooth Streaming) a přístupového podpisu (SAS), používá toodownload mediálních souborů (pro další informace o SAS lokátory najdete [to](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).

### <a name="some-details-about-url-formats"></a>Podrobnosti o formátech adres URL

Po vytvoření lokátorů hello, můžete vytvořit hello adresy URL, které by se použít toostream a stahování souborů. Ukázka Hello v tomto kurzu bude výstup adresy URL, které můžete vložit příslušné prohlížeče. V této části je pár příkladů, jak vypadají jiné formáty.

#### <a name="a-streaming-url-for-mpeg-dash-has-hello-following-format"></a>Streamovací adresa URL pro MPEG DASH má následující formát hello:

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**

#### <a name="a-streaming-url-for-hls-has-hello-following-format"></a>Streamovací adresa URL pro HLS má následující formát hello:

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**

#### <a name="a-streaming-url-for-smooth-streaming-has-hello-following-format"></a>Streamovací adresa URL pro technologii Smooth Streaming má následující formát hello:

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest


#### <a name="a-sas-url-used-toodownload-files-has-hello-following-format"></a>SAS adresa URL používaná toodownload souborů má hello následující formát:

{blob container name}/{asset name}/{file name}/{SAS signature}

Rozšíření sady Media Services .NET SDK poskytují užitečné pomocné metody, návratový formátu adresy URL pro hello publikovaný prostředek.

Hello následující kód používá rozšíření sady SDK pro .NET toocreate lokátory a tooget streamování a progresivního stahování adresy URL. Hello kód také ukazuje, jak toodownload soubory tooa místní složky.

Přidejte následující metody třídy Program toohello hello.

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

## <a name="test-by-playing-your-content"></a>Testování přehráváním obsahu

Po spuštění programu hello definovaného v předchozí části hello hello podobné následující toohello se zobrazí v okně konzoly hello adresy URL.

Adresa URL adaptivního streamování:

Technologie Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Adresa URL progresivního stahování (audio a video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


toostream videa, vložte adresu URL do pole Adresa URL hello v hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

tootest progresivní stahování, vložte adresu URL do prohlížeče (například Internet Explorer, Chrome nebo Safari).

Další informace najdete v tématu hello následující témata:

- [Přehrávání obsahu ve stávajících přehrávačích](media-services-playback-content-with-existing-players.md)
- [Vývoj aplikací videopřehrávače](media-services-develop-video-players.md)
- [Vložení videa adaptivního streamování MPEG-DASH do aplikace HTML5 se souborem DASH.js](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a>Stažení ukázky
Hello následující ukázka kódu obsahuje hello kód, který jste vytvořili v tomto kurzu: [ukázka](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="next-steps"></a>Další kroky

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
