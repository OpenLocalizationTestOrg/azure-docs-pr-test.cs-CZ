---
title: "aaaCreate snímek jen pro čtení objektů blob v Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toocreate snímek blob tooback data objektů blob v daném okamžiku v čase. Pochopit, jak se účtují snímky a toouse je toominimize kapacity poplatky."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 3710705d-e127-4b01-8d0f-29853fb06d0d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: marsma
ms.openlocfilehash: 57f2e76b8899b8a513688bf148dd13673141d5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-blob-snapshot"></a>Vytvoření snímku objektu blob

Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase. Snímky jsou užitečné pro zálohování objekty BLOB. Po vytvoření snímku, číst, kopírovat nebo odstranit, ale nelze je změnit.

Snímek objektu blob je základní objekt blob identické tooits, s výjimkou tomuto objektu blob hello má identifikátor URI **data a času** hodnota připojena toohello blob URI tooindicate hello čas na které hello pořízení snímku. Například, pokud na stránce blob identifikátor URI je `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello snímku identifikátor URI je příliš podobné`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Všechny snímky sdílet hello základní je identifikátor URI objektu blob. Hello jenom rozdíl mezi hello základní objekt blob a hello snímku je hello připojí **data a času** hodnotu.
>

Objekt blob může mít libovolný počet snímků. Snímky uchová, dokud explicitně odstranit. Snímek nemůže outlive jeho základní objekt blob. Můžete vytvořit výčet přidružené hello základní objekt blob tootrack snímky hello vaše aktuální snímky.

Při vytváření snímku objektu blob hello vlastnosti systému objektu blob jsou zkopírované toohello snímku s hello stejné hodnoty. Hello základní metadata objektu blob na je také zkopírovaný toohello snímku, pokud nezadáte samostatné metadata pro hello snímek při jeho vytvoření.

Všechny zapůjčení přidružený objekt blob základní hello neovlivňují hello snímku. Nelze získat zapůjčení na snímku.

Soubor virtuálního pevného disku je použité toostore hello aktuální informace a stav pro disk virtuálního počítače. Můžete odpojit disk z v rámci hello virtuálního počítače nebo vypnout hello virtuálních počítačů a pak proveďte snímek jeho souboru virtuálního pevného disku. Můžete použít tento soubor snímku novější tooretrieve hello virtuálního pevného disku soubor v tomto bodě v čase a znovu vytvořte hello virtuálních počítačů.

Pokud je povolené šifrování služby úložiště (SSE) pro účet úložiště hello v které hello objekt blob se nachází, a potom zašifruje všechny snímky v úvahu tomuto objektu blob v klidovém stavu.

## <a name="create-a-snapshot"></a>Vytvoření snímku
Hello následující příklad kódu ukazuje, jak toocreate snímek pomocí hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Tento příklad určuje dalších metadat pro vytvoření snímku hello při jeho vytvoření.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in hello container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload hello blob toocreate it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of hello base blob.
        // Specify metadata at hello time that hello snapshot is created toospecify unique metadata for hello snapshot.
        // If no metadata is specified when hello snapshot is created, hello base blob's metadata is copied toohello snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="copy-snapshots"></a>Kopie snímků
Operace kopírování zahrnující objekty BLOB a snímky postupujte podle těchto pravidel:

* Snímek můžete zkopírovat přes jeho základní objekt blob. Povýšení pozice toohello snímku hello základní objekt blob, můžete obnovit dřívější verzi objektu blob. Hello snímku zůstane, ale dojde k přepsání základní objekt blob hello kopii hello snímku s možností zápisu.
* Můžete kopírovat objekt blob pro cílový snímku tooa s jiným názvem. Hello výsledné cílový objekt blob je s možností zápisu objektů blob a není snímek.
* Po zkopírování zdrojový objekt blob nejsou cílové zkopírovaný toohello všechny snímky hello zdrojový objekt blob. Pokud cílový objekt blob je přepsat kopie, všechny snímky přidružené hello původní cílový objekt blob zůstanou beze změn.
* Při vytváření snímku objekt blob bloku potvrdit blokovaných hello blob je také zkopírovaný toohello snímku. Všechny nepotvrzené bloky nebudou zkopírovány.

## <a name="specify-an-access-condition"></a>Zadejte podmínku přístup
Při volání [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], můžete zadat podmínku přístup, tak, aby hello snímku se vytvoří pouze v případě, že je splněna podmínka. toospecify podmínku přístup pomocí hello [AccessCondition] [ dotnet_AccessCondition] parametr. Pokud zadaný hello není podmínka splněná, není-li vytvořit snímek hello a hello služby objektů Blob vrátí stavový kód [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Odstraňte snímky
Objekt blob se snímky nelze odstranit, pokud budou odstraněny také hello snímky. Lze odstranit snímek jednotlivě, nebo určit, že při odstranění hello zdrojový objekt blob odstranit všechny snímky. Když zkusíte toodelete objekt blob, který má stále snímky, bude výsledkem chyba.

Následující příklad ukazuje kód jak Hello toodelete objekt blob a jeho snímky v rozhraní .NET, kde `blockBlob` je objekt typu [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Snímky s Azure Premium Storage
Při používání snímky Storage úrovně Premium, platí následující pravidla hello:

* maximální počet snímků za objekt blob stránky v účtu úložiště premium Hello je 100. Pokud je tento limit překročen, hello snímku Blob operace vrátí kód chyby 409 (`SnapshotCountExceeded`).
* Snímek objekt blob stránky může trvat v účtu úložiště premium každých 10 minut. Pokud dojde k překročení tohoto kurzu, hello snímku Blob operace vrátí kód chyby 409 (`SnapshotOperationRateExceeded`).
* tooread snímek, můžete použít toocopy operace kopírování objektu Blob hello objekt blob stránky tooanother snímku v účtu hello. Hello cílový objekt blob pro operace kopírování hello nesmí mít všechny existující snímky. Pokud cílový objekt blob hello má snímky, pak hello operace kopírování objektu Blob vrátí kód chyby 409 (`SnapshotsPresent`).

## <a name="return-hello-absolute-uri-tooa-snapshot"></a>Vrátí hello absolutní identifikátor URI tooa snímku
Tento příklad kódu C# vytvoří snímek a zapíše se hello absolutní identifikátor URI pro primární umístění hello.

```csharp
//Create hello blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference tooa blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of hello blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Pochopit, jak snímky nabíhat poplatky
Vytvoření snímku, která je jen pro čtení kopie objektu blob, může mít za následek účet tooyour poplatky za úložiště další data. Při navrhování vaší aplikace, je důležité vědět, jak může tyto poplatky nabíhat tak, aby můžete minimalizovat náklady toobe.

### <a name="important-billing-considerations"></a>Důležité aspekty fakturace
Hello následující seznam obsahuje klíčové body tooconsider při vytváření snímku.

* Váš účet úložiště způsobuje poplatky za jedinečný bloky nebo stránky, ať už v objektu blob hello nebo hello snímku. Váš účet není způsobí nárůst nákladů pro snímky přidružený objekt blob, dokud neaktualizujete hello objektů blob, na kterém jsou založena. Po aktualizaci základní objekt blob hello diverges z jeho snímků. V takovém případě vám budou účtovat hello jedinečný bloky a stránky v jednotlivých objektů blob nebo snímek.
* Když nahradíte blok v rámci objekt blob bloku, tomto bloku je následně účtovat jako blok jedinečný. To platí i v případě, že má hello bloku hello stejné blokovat ID a hello stejné dat, protože má v hello snímku. Po hello blok je potvrzená znovu ji diverges od jeho protějšku v jakékoli snímek a vám bude účtována pro svoje data. Hello to samé platí pro stránku v objekt blob stránky, který se aktualizuje identické daty.
* Nahrazení objekt blob bloku pomocí volání hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], nebo [UploadFromByteArray] [ dotnet_UploadFromByteArray] metoda nahrazuje všechny bloky v objektu blob hello. Pokud máte snímek přidružené k tomuto objektu blob, teď odchýlit všechny bloky v hello základní objekt blob a snímků a vám bude účtována pro všechny bloky hello v obou objekty BLOB. To platí i v případě, že data hello v hello základní objekt blob a hello snímku zůstávají stejné.
* Hello služby objektů Blob Azure nemá toodetermine znamená, zda dva bloky obsahují stejné údaje. Každý blok, který je odeslán a potvrzené považuje jako jedinečné, i v případě, že se má hello stejným datům a hello stejné blokovat ID. Pro jedinečný bloky nabíhat poplatky, proto je důležité tooconsider, který aktualizuje objekt blob, která obsahuje snímek za následek další jedinečné bloky a dalších poplatků.

### <a name="minimize-cost-with-snapshot-management"></a>Minimalizovat náklady s Správa snímků

Doporučujeme pečlivě správě vaší snímků tooavoid další poplatky. Tyto doporučené postupy pro toohelp minimalizovat náklady hello způsobené hello úložiště vaše snímků můžete postupovat podle:

* Odstranit a znovu vytvořit snímky přidružený objekt blob při každé aktualizaci objektu blob hello, i když jsou aktualizace s stejné údaje, pokud váš návrh aplikace vyžaduje udržovat snímky. Odstranit a znovu vytváření snímků hello blob, můžete zajistit, že není odchýlit hello blob a snímky.
* Pokud udržujete snímků pro objekt blob, vyhněte se volání [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray] [ dotnet_UploadFromByteArray] tooupdate hello blob. Tyto metody nahraďte všechny bloky hello hello objektu BLOB, výrazně způsobuje objektu blob služby základní a jeho toodiverge snímky. Místo toho aktualizace hello nejmenšího možný počet bloků s použitím hello [PutBlock] [ dotnet_PutBlock] a [PutBlockList] [ dotnet_PutBlockList] metody.

### <a name="snapshot-billing-scenarios"></a>Snímek fakturace scénáře
Hello následující scénáře ukazují, jak nabíhat poplatky pro objekt blob bloku a jeho snímků.

**Scénář 1**

Ve scénáři 1 základní objekt blob hello nebyla aktualizována po pořízení snímku hello, takže poplatky se vám neúčtují pouze pro jedinečný bloky 1, 2 a 3.

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Scénář 2**

Ve scénáři 2 hello základní objekt blob se aktualizovalo, ale má hello snímku není. Blok 3 byla aktualizována a to i v případě, že obsahuje hello stejná data a hello stejným ID, není stejný jako blokovat 3 v hello snímku hello. V důsledku toho je účet hello účtovat čtyři bloky.

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Scénář 3**

Ve scénáři 3 hello základní objekt blob se aktualizovalo, ale má hello snímku není. 3 bloku byl nahrazen s blokem 4 v hello základní objekt blob, ale hello snímku odráží bloku 3. V důsledku toho je účet hello účtovat čtyři bloky.

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Scénář 4**

V případě 4 hello základní objekt blob se aktualizovala kompletně a obsahuje žádný z jeho původní bloky. V důsledku toho je účtován hello účet pro všechny bloky osm jedinečný. Tato situace může nastat, pokud používáte metodu aktualizace, jako [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray][dotnet_UploadFromByteArray], protože tyto metody nahrazovat všechny hello obsah objektu blob.

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Další kroky

* Můžete najít další informace o práci s snímků disku virtuálního počítače (VM) v [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md)

* Další příklady kódu pomocí úložiště objektů Blob, najdete v části [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Můžete stáhnout ukázkovou aplikaci a potom ho spusťte nebo přejít hello kódu na Githubu.

[dotnet_AccessCondition]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.accesscondition.aspx
[dotnet_CloudBlockBlob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx
[dotnet_CreateSnapshotAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.createsnapshotasync.aspx
[dotnet_HTTPStatusCode]: https://msdn.microsoft.com/library/system.net.httpstatuscode(v=vs.110).aspx
[dotnet_PutBlockList]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblocklist.aspx
[dotnet_PutBlock]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblock.aspx
[dotnet_UploadFromByteArray]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfrombytearray.aspx
[dotnet_UploadFromFile]: https://msdn.microsoft.com/library/azure/mt705654.aspx
[dotnet_UploadFromStream]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstream.aspx
[dotnet_UploadText]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadtext.aspx