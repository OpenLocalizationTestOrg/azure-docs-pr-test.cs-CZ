---
title: "Použití dynamické šifrování AES-128 a doručení klíče služby | Microsoft Docs"
description: "Microsoft Azure Media Services umožňuje doručovat obsah šifrován šifrovacích klíčů AES 128 bitů. Služba Media Services také poskytuje službu doručování klíč, který doručí šifrovací klíče na oprávněné uživatele. Toto téma ukazuje, jak dynamicky šifrovat pomocí standardu AES-128 a používat službu doručení klíče."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: ae1b36c26e688e74eb8fcc1a4cdbd3be0c014c08
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Použití dynamické šifrování AES-128 a doručení klíče služby
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Přehled
> [!NOTE]
> V tématu [to](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video přehled o tom, jak chránit obsah vašeho média s šifrování AES.
> 
> 

Microsoft Azure Media Services umožňuje doručovat Http-Live-Streaming (HLS) a funkce Smooth Streams šifrované s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování). Služba Media Services také poskytuje službu doručování klíč, který doručí šifrovací klíče na oprávněné uživatele. Pokud chcete použít pro Media Services k šifrování prostředek, budete muset přidružit šifrovací klíč assetu a taky nakonfigurovat zásady autorizace pro klíč. Datový proud je žádost přehrávač, Media Services používá k zadanému klíči dynamicky šifrovat obsah pomocí šifrování AES. K dešifrování datového proudu, bude přehrávač požadovat klíč ze služby doručení klíče. Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.

Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč. Zásady autorizace pro klíč k obsahu mohou obsahovat jedno nebo více omezení autorizace: otevřené omezení nebo omezení s tokenem. Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS). Služba Media Services podporuje tokeny ve  formátu [jednoduchých webových tokenů](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a [webových tokenů JSON](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Další informace najdete v tématu [nakonfigurujte zásady autorizace klíče obsahu](media-services-protect-with-aes128.md#configure_key_auth_policy).

Abyste mohli využívat dynamické šifrování, je třeba mít asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming s více přenosovými rychlostmi. Také musíte nakonfigurovat zásady doručení pro asset (popsáno dále v tomto tématu). Potom, na základě formátu určeného v adrese URL streamování, server streamingu na vyžádání zajistí, aby byl datový proud doručen v protokolu podle vašeho výběru. Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Toto téma bude užitečné pro vývojáře pracující na aplikacích, které doručují média chráněná. Téma ukazuje, jak nakonfigurovat službu doručení klíče pomocí zásad autorizace tak, aby pouze autorizovaní klienti mohli dostávat šifrovací klíče. Také ukazuje, jak používat dynamické šifrování.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dynamické šifrování AES-128 a doručení klíče služby pracovního postupu

Níže jsou uvedeny základní kroky, které je třeba provést při šifrování vaše prostředky pomocí standardu AES, pomocí služby Media Services doručení klíče a také používáte dynamické šifrování.

1. [Vytvoření assetu a nahrání souborů do assetu](media-services-protect-with-aes128.md#create_asset).
2. [Zakódujte asset obsahující soubor do sady souborů MP4 s adaptivní přenosovou](media-services-protect-with-aes128.md#encode_asset).
3. [Vytvořte klíč obsahu a přidružte ji k zakódovanému assetu](media-services-protect-with-aes128.md#create_contentkey). Klíč k obsahu ve službě Media Services obsahuje šifrovací klíč assetu.
4. [Nakonfigurujte zásady autorizace klíče obsahu](media-services-protect-with-aes128.md#configure_key_auth_policy). Zásady autorizace pro klíč k obsahu musíte vy nakonfigurovat a klient splnit, aby bylo možné mu klíč k obsahu doručit.
5. [Konfigurace zásad doručení pro asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Konfigurace zásad doručení zahrnuje: klíčů adresu URL pro získání a inicializační vektor (IV) (AES 128 vyžaduje stejné IV je nutné zadat při šifrování a dešifrování), doručovací protokol (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), typ dynamického šifrování (například obálky nebo žádné dynamické šifrování).

    Na každý protokol stejného assetu můžete použít jiné zásady. Můžete například použít šifrování PlayReady na protokol Smooth/DASH a šifrování pomocí standardu AES Envelope na protokol HLS. Veškeré protokoly, které nejsou v zásadách doručení definovány (například když přidáte jedinou zásadu, která jako protokol určuje pouze HLS), budou při streamování blokovány. Výjimkou je, pokud nemáte definovány vůbec žádné zásady doručení assetu. Pak budou všechny protokoly povolené v nešifrované podobě.

6. [Vytvořit lokátor OnDemand](media-services-protect-with-aes128.md#create_locator) Chcete-li získat adresu URL streamování.

Téma také ukazuje [jak klientská aplikace požádejte o klíč ze služby doručení klíče](media-services-protect-with-aes128.md#client_request).

Najdete kompletní .NET [příklad](media-services-protect-with-aes128.md#example) na konci tohoto tématu.

Následující obrázek znázorňuje pracovní postup popsaný výše. Zde se k ověření používá token.

![Ochrana pomocí AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Zbývající část tohoto tématu poskytuje podrobné vysvětlení, ukázky kódů a odkazy na témata, která ukazují, jak dokončit výše popsané úlohy.

## <a name="current-limitations"></a>Aktuální omezení
Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.

## <a id="create_asset"></a>Vytvoření assetu a nahrání souborů do assetu
Aby bylo možné spravovat, kódovat a streamovat videa, musíte nejprve nahrát obsah do služby Microsoft Azure Media Services. Po nahrání bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování. 

Podrobné informace najdete v článku o [nahrání souborů do účtu služby Media Services](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>Zakódujte asset obsahující soubor s adaptivní přenosovou rychlostí sady souborů MP4
V případě dynamického šifrování je třeba pouze vytvořit asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming s více přenosovými rychlostmi. Pak na základě formátu určeného v manifestu nebo fragmentu požadavek streamingu na vyžádání serveru zajistí datový proud obdrželi v protokolu, kterou jste vybrali. Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů. Další informace najdete v tématu [Přehled dynamického balení](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**. Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**. 
>
>Abyste mohli používat dynamické balením a dynamickým šifrováním také váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.

Pokyny ke kódování najdete v článku o [kódování assetu pomocí kodéru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Vytvoření klíče k obsahu a jeho přiřazení k zakódovanému assetu
Klíč k obsahu ve službě Media Services obsahuje klíč, kterým chcete asset šifrovat.

Podrobné informace najdete v tématu [Vytvoření klíče k obsahu](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>Konfigurace zásad autorizace klíče obsahu
Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč. Zásady autorizace pro klíč k obsahu musíte vy nakonfigurovat a klient (přehrávač) splnit, aby bylo možné mu klíč doručit. Zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevřete, token omezení nebo omezení IP adres.

Podrobnější informace najdete v tématu [Konfigurace zásad autorizace pro klíč k obsahu](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení assetu
Nakonfigurujte zásady doručení pro asset. Konfigurace zásad doručení assetu zahrnuje následující položky:

* Adresa URL získávání klíčů. 
* Inicializace vektoru (IV) má použít pro šifrování obálku. AES 128 vyžaduje stejné IV je nutné zadat při šifrování a dešifrování. 
* Doručovací protokol assetu (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny).
* Typ dynamického šifrování (například pomocí standardu AES envelope) nebo žádné dynamické šifrování. 

Podrobné informace najdete v tématu [Konfigurace zásad doručení assetu ](media-services-rest-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Pokud chcete získat adresu URL streamování, vytvořte lokátor streamování OnDemand.
Uživateli je třeba poskytnout adresu URL streamování pro protokol Smooth, DASH nebo HLS.

> [!NOTE]
> Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.
> 
> 

Pokyny k publikování assetu a vytvoření adresy URL streamování najdete v článku o [vytvoření adresy URL streamování](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Získání testovacího tokenu
Získejte testovací token na základě omezení s tokenem, které se používá v zásadách autorizace klíče.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

K testování datového proudu můžete použít [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a id="client_request"></a>Jak můžete vašeho klienta požadavku klíč ze služby doručení klíče?
V předchozím kroku sestavit adresu URL, která odkazuje na soubor manifestu. Váš klient je potřeba extrahovat nezbytné informace z datových proudů souborů manifestu aby bylo možné žádost službě doručení klíče.

### <a name="manifest-files"></a>Soubory manifestu
Klient musí k extrakci adresu URL (který také obsahuje klíč obsahu Id (dětský)) hodnotu ze souboru manifestu. Klient se potom pokusí získat šifrovací klíč ze služby doručení klíče. Klient musí také k extrakci IV hodnota a použít ho dešifrovat datového proudu. Následující fragment kódu ukazuje <Protection> element manifestu technologie Smooth Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

V případě HLS kořenové manifest je rozděleno do segmentu souborů. 

Například kořenový manifest je: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) a obsahuje seznam názvů souborů segmentu.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Pokud je jeden ze souborů segment otevřete v textovém editoru (například http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), měl by obsahovat #EXT-X-KEY, který označuje, že soubor je šifrován.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Pokud máte v úmyslu přehrání AES šifrovat HLS v prohlížeči Safari najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>Klíč žádat službu doručení klíče

Následující kód ukazuje, jak odeslat žádost o doručení klíče službě Media Services pomocí doručení klíče identifikátor Uri (extrahovaný z manifestu) a token (v tomto tématu není mluvit o tom, jak získat jednoduchých webových tokenů od služby tokenů zabezpečení).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a>Chránit obsah s AES-128 pomocí rozhraní .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

1. Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md). 
2. Do části **appSettings** definované ve vašem souboru app.config přidejte následující elementy:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Příklad

Přepište kód v souboru Program.cs kódem zobrazeným v této části.
 
>[!NOTE]
>Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad. Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

Nezapomeňte aktualizovat proměnné tak, aby odkazovaly do složek, ve kterých jsou umístěné vaše vstupní soubory.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace AESDynamicEncryptionAndKeyDeliverySvc
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing the issuer of the token.  
        // Must match the value in the token for the token to be considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // The Audience or Scope of the token.  
        // Must match the value in the token for the token to be considered valid.
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //The GenerateTestToken method returns the token without the word “Bearer” in front
            //so you have to add it in front of the token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the bit.ly/aesplayer Flash player to test the URL 
            // (with open authorization policy). 
            // Paste the URL and click the Update button to play the video. 
            //
            string URL = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
            Console.WriteLine();
            Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
            Console.WriteLine();

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);


            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }

        static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
        {
            // Create envelope encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                keyId,
                contentKey,
                "ContentKey",
                ContentKeyType.EnvelopeEncryption);

            // Associate the key with the asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions 
            // and create authorization policy             
            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Open Authorization Policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
            Name = "HLS Open Authorization Policy",
            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
            Requirements = null // no requirements needed for HLS
        };

            restrictions.Add(restriction);

            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            "");

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy to ContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("HLS token restricted authorization policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "Token Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString
            };

            restrictions.Add(restriction);

            //You could have multiple options 
            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "Token option for HLS",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            null  // no key delivery data is needed for HLS
            );

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy to ContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose to associate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

            // The following policy configuration specifies: 
            // key url that will have KID=<Guid> appended to the envelope and
            // the Initialization Vector (IV) to use for the envelope encryption.

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
            };

            IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin. 
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file. 
            return originLocator.Path + assetFile.Name;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int size)
        {
            byte[] randomBytes = new byte[size];
            using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
            {
            rng.GetBytes(randomBytes);
            }

            return randomBytes;
        }
        }
    }


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

