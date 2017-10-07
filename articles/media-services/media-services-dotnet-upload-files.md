---
title: "aaaUpload souborů do účtu Media Services pomocí rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak tooget média obsahu ve službě Media Services pomocí vytvoření a odeslání prostředky."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: 11c8a359b09efe04b54490fd48ac0cd7c366f8b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>Nahrání souborů do účtu Media Services pomocí rozhraní .NET
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Azure Portal](media-services-portal-upload-files.md)
> 
> 

Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku. Hello **Asset** entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.)  Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.

soubory Hello v hello prostředku se nazývají **soubory prostředku**. Hello **AssetFile** instance a hello samotný mediální soubor jsou dva odlišné objekty. Hello AssetFile instance obsahuje metadata o hello soubor média, zatímco soubor média hello obsahuje hello samotný mediální obsah.

> [!NOTE]
> použít Hello následující aspekty:
> 
> * Služba Media Services použije hello hodnotu hello IAssetFile.Name vlastnost při sestavování adresy URL pro hello streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech. Hello hodnotu hello **název** vlastnost nemůže mít žádné z následujících hello [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Navíc může existovat pouze jedna '.' pro příponu názvu souboru hello.
> * Délka Hello hello názvu by neměla být větší než 260 znaků.
> * Existuje limit toohello maximální velikost souboru pro zpracování ve službě Media Services podporována. Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.
> * Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady). Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.
> 

Při vytváření prostředků, můžete zadat následující možnosti šifrování hello. 

* **Žádné** – nepoužívá se žádné šifrování. Toto je výchozí hodnota hello. Všimněte si, že při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.
  Tuto možnost použijte, pokud máte v plánu toodeliver MP4 pomocí progresivního stahování. 
* **CommonEncryption** – tuto možnost použijte, pokud nahráváte obsah, který byl zašifrován a chráněný běžným šifrováním nebo DRM s technologií PlayReady (například Smooth Streaming chráněná pomocí DRM s technologií PlayReady).
* **EnvelopeEncrypted** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES. Všimněte si, že hello soubory musí být kódovaný a zašifrované pomocí Správce transformací.
* **StorageEncrypted** – šifruje vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a odešle ho tooAzure úložiště, kde je uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku. Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure rest souborů vysoké kvality vstupními médii pomocí silného šifrování na disku.
  
    Služba Media Services poskytuje šifrování úložiště na disku pro vaše prostředky, ne přes přenosu jako správce digitální práv (DRM).
  
    Pokud váš asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu. Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).

Pokud zadáte pro váš asset toobe šifrován **CommonEncrypted** možnost, nebo **EnvelopeEncypted** možnost, budete potřebovat tooassociate asset s **ContentKey**. Další informace najdete v tématu [jak toocreate ContentKey](media-services-dotnet-create-contentkey.md). 

Pokud zadáte pro váš asset toobe šifrován **StorageEncrypted** možnost, hello sady Media Services SDK pro .NET vytvoří **StorateEncrypted** **ContentKey** pro vaše Asset.

Toto téma ukazuje, jak toouse Media Services .NET SDK, jakož i sady Media Services .NET SDK rozšíření tooupload soubory do asset Media Services.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Nahrát jeden soubor pomocí sady Media Services .NET SDK
Hello následující ukázkový kód používá .NET SDK tooupload jeden soubor. Hello AccessPolicy a Lokátor vytvořen a zničen funkcí nahrávání hello. 


        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Uložení více souborů pomocí sady Media Services .NET SDK
Následující kód ukazuje, jak Hello toocreate prostředek a uložení více souborů.

Kód Hello hello následující:

* Vytvoří prázdný majetku pomocí metody CreateEmptyAsset hello definované v předchozím kroku hello.
* Vytvoří **AccessPolicy** instanci, která definuje hello oprávnění a dobu trvání asset toohello přístup.
* Vytvoří **Lokátor** instanci, která poskytuje přístup toohello asset.
* Vytvoří **BlobTransferClient** instance. Tento typ reprezentuje klienta, který funguje na hello objektů BLOB Azure. V tomto příkladu používáme průběhu odesílání hello klienta toomonitor hello. 
* Vytvoří výčet prostřednictvím určeného adresáře hello soubory a vytvoří **AssetFile** instance pro každý soubor.
* Nahrávání hello soubory ve službě Media Services pomocí hello **UploadAsync** metoda. 

> [!NOTE]
> Použijte tooensure hello UploadAsync metoda, která hello volání neblokují a hello soubory jsou odeslány paralelně.
> 
> 

        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended toovalidate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading hello files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as hello upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Při nahrávání velký počet prostředků, zvažte následující hello.

* Vytvořte novou **CloudMediaContext** objektu na vlákno. Hello **CloudMediaContext** třída není bezpečná pro přístup z více vláken.
* Zvýšit NumberOfConcurrentTransfers z hello výchozí hodnotu 2 vyšší hodnota tooa, jako je 5. Nastavení této vlastnosti ovlivní všechny instance **CloudMediaContext**. 
* Zachovat ParallelTransferThreadCount v hello výchozí hodnota je 10.

## <a id="ingest_in_bulk"></a>Příjem prostředky hromadně pomocí sady Media Services .NET SDK
Nahrávání souborů velké prostředek může být kritický bod během vytváření asset. Příjem prostředky v hromadné nebo "Hromadné příjem", zahrnuje oddělení asset vytvoření z procesu nahrávání hello. toouse hromadné ingesting přístup, vytvořte manifestu (IngestManifest), který popisuje hello asset a jeho přidružené soubory. Pak použijte hello nahrávání metodu výběru tooupload hello přidružené soubory toohello manifest pro kontejner objektů blob. Microsoft Azure Media Services sleduje kontejneru objektů blob hello přidruženého k manifestu hello. Kontejner objektů blob nahrané toohello po soubor Microsoft Azure Media Services dokončí vytváření asset hello na základě konfigurace hello hello majetku v manifestu hello (IngestManifestAsset).

toocreate nové IngestManifest volat metodu Create hello vystavené hello IngestManifests kolekce na hello CloudMediaContext. Tato metoda vytvoří nový IngestManifest hello manifestu názvem, který zadáte.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Vytvořte hello prostředky, které budou přidruženy k hromadné hello IngestManifest. Konfigurujte možnosti šifrování hello potřeby na hello asset pro příjem hromadně.

    // Create hello assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

IngestManifestAsset přidruží hromadné IngestManifest pro příjem hromadné prostředek. Také přidruží hello AssetFiles, které budou použity k vytvoření každého prostředku. toocreate IngestManifestAsset, použijte metodu Create hello v kontextu server hello.

Hello následující příklad ukazuje přidání dva nové IngestManifestAssets, které spojují hello dva prostředky vytvořili hromadné toohello ingestování manifestu. Každý IngestManifestAsset také přidruží sadu souborů, které budou odeslány, pro každý prostředek během hromadné příjem.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

Můžete použít libovolná aplikace klienta vysokorychlostní schopná odesílání hello asset soubory toohello kontejner úložiště objektů blob URI poskytované hello **IIngestManifest.BlobStorageUriForUpload** vlastnost hello IngestManifest. Je jedna služba nahrávání významné vysokorychlostní [Aspera na vyžádání pro aplikaci Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Je také možné zapsat kód tooupload hello prostředky soubory jak ukazuje následující příklad kódu hello.

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }

Hello kód pro nahrávání souborů hello asset pro ukázku hello použitým v tomto tématu je uveden v hello následující ukázka kódu.

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


Můžete určit hello průběh hello hromadné příjem pro všechny prostředky přidružené **IngestManifest** pomocí cyklického dotazování hello statistiky vlastnost hello **IngestManifest**. V pořadí informace o průběhu tooupdate, je třeba použít novou **CloudMediaContext** pokaždé, když dotazovat vlastnost statistiky hello.

Hello následující příklad ukazuje, dotazování IngestManifest podle jeho **Id**.

    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }



## <a name="upload-files-using-net-sdk-extensions"></a>Nahrát soubory pomocí rozšíření sady SDK pro .NET
Následující příklad Hello ukazuje, jak tooupload jednu souboru pomocí rozšíření sady SDK pro .NET. V takovém případě hello **CreateFromFile** metoda se používá, ale je k dispozici také asynchronní verzi hello (**CreateFromFileAsync**). Hello **CreateFromFile** metoda slouží k určení názvu souboru text hello, možnost šifrování a zpětného volání v pořadí tooreport hello nahrát průběh hello souboru.

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

Hello následující příklad volá funkci UploadFile a určuje šifrování úložiště jako možnost vytvoření asset hello.  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a>Další kroky

Nyní můžete kódovat nahrané assety. Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).

Můžete také použít Azure Functions tootrigger úlohu kódování na základě souboru přicházejících do kontejneru hello nakonfigurované. Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Další krok
Teď, když jste nahráli prostředek tooMedia služby, přejděte toohello [jak tooGet procesor médií] [ How tooGet a Media Processor] tématu.

[How tooGet a Media Processor]: media-services-get-media-processor.md

