---
title: "Vytvoření snímku jen pro čtení objektu blob ve službě Azure Storage | Microsoft Docs"
description: "Naučte se vytvořit snímek objektů blob k zálohování dat objektů blob v daném okamžiku v čase. Pochopit, jak se účtují snímky a jak pomocí nich můžete minimalizovat náklady na kapacitu."
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
ms.openlocfilehash: 6ebb77f4ec5f1887e5c55bf1c8c64224a9233654
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-blob-snapshot"></a><span data-ttu-id="2003a-104">Vytvoření snímku objektu blob</span><span class="sxs-lookup"><span data-stu-id="2003a-104">Create a blob snapshot</span></span>

<span data-ttu-id="2003a-105">Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase.</span><span class="sxs-lookup"><span data-stu-id="2003a-105">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="2003a-106">Snímky jsou užitečné pro zálohování objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="2003a-106">Snapshots are useful for backing up blobs.</span></span> <span data-ttu-id="2003a-107">Po vytvoření snímku, číst, kopírovat nebo odstranit, ale nelze je změnit.</span><span class="sxs-lookup"><span data-stu-id="2003a-107">After you create a snapshot, you can read, copy, or delete it, but you cannot modify it.</span></span>

<span data-ttu-id="2003a-108">Snímek objektu blob je stejný jako jeho základní objekt blob, s tím rozdílem, že je identifikátor URI objektu blob **data a času** hodnota připojí k objektu blob identifikátor URI, který označuje datum a čas, kdy pořízení snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-108">A snapshot of a blob is identical to its base blob, except that the blob URI has a **DateTime** value appended to the blob URI to indicate the time at which the snapshot was taken.</span></span> <span data-ttu-id="2003a-109">Například, pokud na stránce blob identifikátor URI je `http://storagesample.core.blob.windows.net/mydrives/myvhd`, snímku identifikátor URI je podobná `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span><span class="sxs-lookup"><span data-stu-id="2003a-109">For example, if a page blob URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, the snapshot URI is similar to `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span></span>

> [!NOTE]
> <span data-ttu-id="2003a-110">Všechny snímky sdílet identifikátor URI základní objektu blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-110">All snapshots share the base blob's URI.</span></span> <span data-ttu-id="2003a-111">Mezi základní objekt blob a snímku jenom rozdíl je připojených **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2003a-111">The only distinction between the base blob and the snapshot is the appended **DateTime** value.</span></span>
>

<span data-ttu-id="2003a-112">Objekt blob může mít libovolný počet snímků.</span><span class="sxs-lookup"><span data-stu-id="2003a-112">A blob can have any number of snapshots.</span></span> <span data-ttu-id="2003a-113">Snímky uchová, dokud explicitně odstranit.</span><span class="sxs-lookup"><span data-stu-id="2003a-113">Snapshots persist until they are explicitly deleted.</span></span> <span data-ttu-id="2003a-114">Snímek nemůže outlive jeho základní objekt blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-114">A snapshot cannot outlive its base blob.</span></span> <span data-ttu-id="2003a-115">Můžete vytvořit výčet snímků přidružených základní objekt blob sledovat vaše aktuální snímky.</span><span class="sxs-lookup"><span data-stu-id="2003a-115">You can enumerate the snapshots associated with the base blob to track your current snapshots.</span></span>

<span data-ttu-id="2003a-116">Při vytváření snímku objektu blob vlastnosti systému objektu blob se zkopírují do snímku se stejnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2003a-116">When you create a snapshot of a blob, the blob's system properties are copied to the snapshot with the same values.</span></span> <span data-ttu-id="2003a-117">Metadata základní objektu blob je také zkopírován do snímku, pokud nezadáte samostatné metadata pro snímek při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="2003a-117">The base blob's metadata is also copied to the snapshot, unless you specify separate metadata for the snapshot when you create it.</span></span>

<span data-ttu-id="2003a-118">Všechny zapůjčení přidružené základní objekt blob neovlivňují snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-118">Any leases associated with the base blob do not affect the snapshot.</span></span> <span data-ttu-id="2003a-119">Nelze získat zapůjčení na snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-119">You cannot acquire a lease on a snapshot.</span></span>

<span data-ttu-id="2003a-120">Soubor virtuálního pevného disku se používá k uložení aktuální informace a stav pro disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2003a-120">A VHD file is used to store the current information and status for a VM disk.</span></span> <span data-ttu-id="2003a-121">Můžete odpojit disk z virtuálního počítače nebo vypněte virtuální počítač a potom pořízení snímku jeho souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="2003a-121">You can detach a disk from within the VM or shut down the VM, and then take a snapshot of its VHD file.</span></span> <span data-ttu-id="2003a-122">Tento soubor snímku můžete použít později k načtení souboru virtuálního pevného disku v tomto bodě v čase a znovu vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2003a-122">You can use that snapshot file later to retrieve the VHD file at that point in time and recreate the VM.</span></span>

<span data-ttu-id="2003a-123">Pokud šifrování služby úložiště (SSE) je povolen pro účet úložiště, ve kterém se nachází objekt blob, budou všechny snímky v úvahu tomuto objektu blob zašifrovaná přinejmenším.</span><span class="sxs-lookup"><span data-stu-id="2003a-123">If Storage Service Encryption (SSE) is enabled for the storage account in which the blob resides, then any snapshots taken of that blob will be encrypted at rest.</span></span>

## <a name="create-a-snapshot"></a><span data-ttu-id="2003a-124">Vytvoření snímku</span><span class="sxs-lookup"><span data-stu-id="2003a-124">Create a snapshot</span></span>
<span data-ttu-id="2003a-125">Následující příklad kódu ukazuje, jak vytvořit snímek pomocí [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="2003a-125">The following code example shows how to create a snapshot by using the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span> <span data-ttu-id="2003a-126">Tento příklad určuje další metadata pro snímek, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="2003a-126">This example specifies additional metadata for the snapshot when it is created.</span></span>

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
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

## <a name="copy-snapshots"></a><span data-ttu-id="2003a-127">Kopie snímků</span><span class="sxs-lookup"><span data-stu-id="2003a-127">Copy snapshots</span></span>
<span data-ttu-id="2003a-128">Operace kopírování zahrnující objekty BLOB a snímky postupujte podle těchto pravidel:</span><span class="sxs-lookup"><span data-stu-id="2003a-128">Copy operations involving blobs and snapshots follow these rules:</span></span>

* <span data-ttu-id="2003a-129">Snímek můžete zkopírovat přes jeho základní objekt blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-129">You can copy a snapshot over its base blob.</span></span> <span data-ttu-id="2003a-130">Povýšení snímku na pozici základní objekt blob, můžete obnovit dřívější verzi objektu blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-130">By promoting a snapshot to the position of the base blob, you can restore an earlier version of a blob.</span></span> <span data-ttu-id="2003a-131">Snímek zůstane, ale základní dojde k přepsání objektů blob s možností zápisu kopie snímků.</span><span class="sxs-lookup"><span data-stu-id="2003a-131">The snapshot remains, but the base blob is overwritten with a writable copy of the snapshot.</span></span>
* <span data-ttu-id="2003a-132">Snímek můžete zkopírovat do cílového objektu blob s jiným názvem.</span><span class="sxs-lookup"><span data-stu-id="2003a-132">You can copy a snapshot to a destination blob with a different name.</span></span> <span data-ttu-id="2003a-133">Výsledný cílový objekt blob je s možností zápisu objektů blob a není snímek.</span><span class="sxs-lookup"><span data-stu-id="2003a-133">The resulting destination blob is a writable blob and not a snapshot.</span></span>
* <span data-ttu-id="2003a-134">Po zkopírování zdrojový objekt blob nebudou zkopírovány všechny snímky zdrojový objekt blob do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="2003a-134">When a source blob is copied, any snapshots of the source blob are not copied to the destination.</span></span> <span data-ttu-id="2003a-135">Pokud cílový objekt blob je přepsat kopie, všechny snímky přidružené k původní cílový objekt blob zůstanou beze změn.</span><span class="sxs-lookup"><span data-stu-id="2003a-135">When a destination blob is overwritten with a copy, any snapshots associated with the original destination blob remain intact.</span></span>
* <span data-ttu-id="2003a-136">Při vytváření snímku objekt blob bloku potvrdit blokovaných objektu blob je také zkopírován do snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-136">When you create a snapshot of a block blob, the blob's committed block list is also copied to the snapshot.</span></span> <span data-ttu-id="2003a-137">Všechny nepotvrzené bloky nebudou zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="2003a-137">Any uncommitted blocks are not copied.</span></span>

## <a name="specify-an-access-condition"></a><span data-ttu-id="2003a-138">Zadejte podmínku přístup</span><span class="sxs-lookup"><span data-stu-id="2003a-138">Specify an access condition</span></span>
<span data-ttu-id="2003a-139">Při volání [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], můžete zadat podmínku přístup, tak, aby snímku se vytvoří pouze v případě, že je splněna podmínka.</span><span class="sxs-lookup"><span data-stu-id="2003a-139">When you call [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], you can specify an access condition so that the snapshot is created only if a condition is met.</span></span> <span data-ttu-id="2003a-140">Pokud chcete zadat podmínku přístup, použijte [AccessCondition] [ dotnet_AccessCondition] parametr.</span><span class="sxs-lookup"><span data-stu-id="2003a-140">To specify an access condition, use the [AccessCondition][dotnet_AccessCondition] parameter.</span></span> <span data-ttu-id="2003a-141">Pokud není splněna zadaná podmínka, není-li vytvořit snímek a služby objektů Blob vrátí stavový kód [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.</span><span class="sxs-lookup"><span data-stu-id="2003a-141">If the specified condition is not met, the snapshot is not created, and the Blob service returns status code [HTTPStatusCode][dotnet_HTTPStatusCode].PreconditionFailed.</span></span>

## <a name="delete-snapshots"></a><span data-ttu-id="2003a-142">Odstraňte snímky</span><span class="sxs-lookup"><span data-stu-id="2003a-142">Delete snapshots</span></span>
<span data-ttu-id="2003a-143">Objekt blob se snímky nelze odstranit, pokud budou odstraněny také tyto snímky.</span><span class="sxs-lookup"><span data-stu-id="2003a-143">You can't delete a blob with snapshots unless the snapshots are also deleted.</span></span> <span data-ttu-id="2003a-144">Lze odstranit snímek jednotlivě, nebo určit, že všechny snímky odstranit, pokud je zdrojový objekt blob se odstraní.</span><span class="sxs-lookup"><span data-stu-id="2003a-144">You can delete a snapshot individually, or specify that all snapshots be deleted when the source blob is deleted.</span></span> <span data-ttu-id="2003a-145">Pokud se pokusíte odstranit objekt blob, který má stále snímky, bude výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="2003a-145">If you attempt to delete a blob that still has snapshots, an error results.</span></span>

<span data-ttu-id="2003a-146">Následující příklad kódu ukazuje, jak odstranit objekt blob a jeho snímky v rozhraní .NET, kde `blockBlob` je objekt typu [CloudBlockBlob][dotnet_CloudBlockBlob]:</span><span class="sxs-lookup"><span data-stu-id="2003a-146">The following code example shows how to delete a blob and its snapshots in .NET, where `blockBlob` is an object of type [CloudBlockBlob][dotnet_CloudBlockBlob]:</span></span>

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a><span data-ttu-id="2003a-147">Snímky s Azure Premium Storage</span><span class="sxs-lookup"><span data-stu-id="2003a-147">Snapshots with Azure Premium Storage</span></span>
<span data-ttu-id="2003a-148">Při použití snímků Storage úrovně Premium, platí následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="2003a-148">When using snapshots with Premium Storage, the following rules apply:</span></span>

* <span data-ttu-id="2003a-149">Maximální počet snímků za objekt blob stránky v účtu úložiště premium je 100.</span><span class="sxs-lookup"><span data-stu-id="2003a-149">The maximum number of snapshots per page blob in a premium storage account is 100.</span></span> <span data-ttu-id="2003a-150">Pokud je tento limit překročen, operace snímku Blob vrátí kód chyby 409 (`SnapshotCountExceeded`).</span><span class="sxs-lookup"><span data-stu-id="2003a-150">If that limit is exceeded, the Snapshot Blob operation returns error code 409 (`SnapshotCountExceeded`).</span></span>
* <span data-ttu-id="2003a-151">Snímek objekt blob stránky může trvat v účtu úložiště premium každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="2003a-151">You can take a snapshot of a page blob in a premium storage account once every 10 minutes.</span></span> <span data-ttu-id="2003a-152">Pokud dojde k překročení tohoto kurzu, objektů Blob snímku operaci vrátí kód chyby 409 (`SnapshotOperationRateExceeded`).</span><span class="sxs-lookup"><span data-stu-id="2003a-152">If that rate is exceeded, the Snapshot Blob operation returns error code 409 (`SnapshotOperationRateExceeded`).</span></span>
* <span data-ttu-id="2003a-153">Číst snímku, můžete kopírovat objekt Blob operaci kopírování snímku na jiný objekt blob stránky v účtu.</span><span class="sxs-lookup"><span data-stu-id="2003a-153">To read a snapshot, you can use the Copy Blob operation to copy a snapshot to another page blob in the account.</span></span> <span data-ttu-id="2003a-154">Cílový objekt blob pro kopírování nesmí mít všechny existující snímky.</span><span class="sxs-lookup"><span data-stu-id="2003a-154">The destination blob for the copy operation must not have any existing snapshots.</span></span> <span data-ttu-id="2003a-155">Pokud cílový objekt blob má snímky a pak operaci kopírování objektu Blob vrátí kód chyby 409 (`SnapshotsPresent`).</span><span class="sxs-lookup"><span data-stu-id="2003a-155">If the destination blob does have snapshots, then the Copy Blob operation returns error code 409 (`SnapshotsPresent`).</span></span>

## <a name="return-the-absolute-uri-to-a-snapshot"></a><span data-ttu-id="2003a-156">Vrátí absolutní identifikátor URI na snímek</span><span class="sxs-lookup"><span data-stu-id="2003a-156">Return the absolute URI to a snapshot</span></span>
<span data-ttu-id="2003a-157">Tento příklad kódu C# vytvoří snímek a zapisuje na absolutní identifikátor URI pro primární umístění.</span><span class="sxs-lookup"><span data-stu-id="2003a-157">This C# code example creates a snapshot and writes out the absolute URI for the primary location.</span></span>

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a><span data-ttu-id="2003a-158">Pochopit, jak snímky nabíhat poplatky</span><span class="sxs-lookup"><span data-stu-id="2003a-158">Understand how snapshots accrue charges</span></span>
<span data-ttu-id="2003a-159">Vytvoření snímku, která je jen pro čtení kopie objektu blob, může způsobit další data poplatky za úložiště k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="2003a-159">Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges to your account.</span></span> <span data-ttu-id="2003a-160">Při navrhování vaší aplikace, je důležité si uvědomit o tom, jak může tyto poplatky nabíhat tak, aby můžete minimalizovat náklady.</span><span class="sxs-lookup"><span data-stu-id="2003a-160">When designing your application, it is important to be aware of how these charges might accrue so that you can minimize costs.</span></span>

### <a name="important-billing-considerations"></a><span data-ttu-id="2003a-161">Důležité aspekty fakturace</span><span class="sxs-lookup"><span data-stu-id="2003a-161">Important billing considerations</span></span>
<span data-ttu-id="2003a-162">Následující seznam obsahuje klíčové body, které je třeba zvážit při vytváření snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-162">The following list includes key points to consider when creating a snapshot.</span></span>

* <span data-ttu-id="2003a-163">Váš účet úložiště způsobuje poplatky za jedinečný bloky nebo stránky, ať už jsou v objektu blob nebo ve snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-163">Your storage account incurs charges for unique blocks or pages, whether they are in the blob or in the snapshot.</span></span> <span data-ttu-id="2003a-164">Váš účet není způsobí nárůst nákladů pro snímky přidružený objekt blob, dokud neaktualizujete objektů blob, na kterém jsou založena.</span><span class="sxs-lookup"><span data-stu-id="2003a-164">Your account does not incur additional charges for snapshots associated with a blob until you update the blob on which they are based.</span></span> <span data-ttu-id="2003a-165">Po dokončení aktualizace základní objekt blob se diverges z jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="2003a-165">After you update the base blob, it diverges from its snapshots.</span></span> <span data-ttu-id="2003a-166">V takovém případě vám budou účtovat jedinečný bloky nebo stránky v jednotlivých objektů blob nebo snímek.</span><span class="sxs-lookup"><span data-stu-id="2003a-166">When this happens, you are charged for the unique blocks or pages in each blob or snapshot.</span></span>
* <span data-ttu-id="2003a-167">Když nahradíte blok v rámci objekt blob bloku, tomto bloku je následně účtovat jako blok jedinečný.</span><span class="sxs-lookup"><span data-stu-id="2003a-167">When you replace a block within a block blob, that block is subsequently charged as a unique block.</span></span> <span data-ttu-id="2003a-168">To platí i v případě, že bloku má stejné ID bloku a stejná data, protože má ve snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-168">This is true even if the block has the same block ID and the same data as it has in the snapshot.</span></span> <span data-ttu-id="2003a-169">Po blok je potvrzená znovu ji diverges od jeho protějšku v jakékoli snímek a vám bude účtována pro svoje data.</span><span class="sxs-lookup"><span data-stu-id="2003a-169">After the block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data.</span></span> <span data-ttu-id="2003a-170">To samé platí pro stránku v objekt blob stránky, který se aktualizuje identické daty.</span><span class="sxs-lookup"><span data-stu-id="2003a-170">The same holds true for a page in a page blob that's updated with identical data.</span></span>
* <span data-ttu-id="2003a-171">Nahrazení objekt blob bloku voláním [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray] [ dotnet_UploadFromByteArray] metoda nahrazuje všechny bloky v objektu blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-171">Replacing a block blob by calling the [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] method replaces all blocks in the blob.</span></span> <span data-ttu-id="2003a-172">Pokud máte snímek přidružené k tomuto objektu blob, teď odchýlit v základní objekt blob a snímek všech bloků a vám bude účtována pro všechny bloky v obou objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="2003a-172">If you have a snapshot associated with that blob, all blocks in the base blob and snapshot now diverge, and you will be charged for all the blocks in both blobs.</span></span> <span data-ttu-id="2003a-173">To platí i v případě, že data v základní objekt blob a snímku zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="2003a-173">This is true even if the data in the base blob and the snapshot remain identical.</span></span>
* <span data-ttu-id="2003a-174">Služba objektů Blob v Azure nemá prostředek k určení, zda dva bloky obsahují stejné údaje.</span><span class="sxs-lookup"><span data-stu-id="2003a-174">The Azure Blob service does not have a means to determine whether two blocks contain identical data.</span></span> <span data-ttu-id="2003a-175">Každý blok, který je odeslán a potvrzené považuje jako jedinečné, i když má stejná data a stejné ID bloku.</span><span class="sxs-lookup"><span data-stu-id="2003a-175">Each block that is uploaded and committed is treated as unique, even if it has the same data and the same block ID.</span></span> <span data-ttu-id="2003a-176">Protože pro jedinečný bloky nabíhat poplatky, je důležité vzít v úvahu, aktualizován objekt blob, který má snímku výsledky v další jedinečné bloky a další poplatky.</span><span class="sxs-lookup"><span data-stu-id="2003a-176">Because charges accrue for unique blocks, it's important to consider that updating a blob that has a snapshot results in additional unique blocks and additional charges.</span></span>

### <a name="minimize-cost-with-snapshot-management"></a><span data-ttu-id="2003a-177">Minimalizovat náklady s Správa snímků</span><span class="sxs-lookup"><span data-stu-id="2003a-177">Minimize cost with snapshot management</span></span>

<span data-ttu-id="2003a-178">Doporučujeme vám, správě vaší snímků pečlivě, aby se zabránilo další poplatky.</span><span class="sxs-lookup"><span data-stu-id="2003a-178">We recommend managing your snapshots carefully to avoid extra charges.</span></span> <span data-ttu-id="2003a-179">Můžete postupovat podle těchto osvědčené postupy minimalizovat náklady na úložiště vaše snímky:</span><span class="sxs-lookup"><span data-stu-id="2003a-179">You can follow these best practices to help minimize the costs incurred by the storage of your snapshots:</span></span>

* <span data-ttu-id="2003a-180">Odstranit a znovu vytvořit snímky, které jsou přidružené k objektu blob při každé aktualizaci objektu blob, i když jsou aktualizace s stejné údaje, pokud váš návrh aplikace vyžaduje udržet si snímky.</span><span class="sxs-lookup"><span data-stu-id="2003a-180">Delete and re-create snapshots associated with a blob whenever you update the blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots.</span></span> <span data-ttu-id="2003a-181">Odstranit a znovu vytváření snímků objektu blob, můžete zajistit, že není odchýlit objektů blob a snímky.</span><span class="sxs-lookup"><span data-stu-id="2003a-181">By deleting and re-creating the blob's snapshots, you can ensure that the blob and snapshots do not diverge.</span></span>
* <span data-ttu-id="2003a-182">Pokud udržujete snímků pro objekt blob, vyhněte se volání [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray] [ dotnet_UploadFromByteArray] k aktualizaci objektu blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-182">If you are maintaining snapshots for a blob, avoid calling [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] to update the blob.</span></span> <span data-ttu-id="2003a-183">Tyto metody nahraďte všechny bloky v objektu blob, které způsobuje objektu blob služby základní a jeho snímky o odchylce výrazně.</span><span class="sxs-lookup"><span data-stu-id="2003a-183">These methods replace all of the blocks in the blob, causing your base blob and its snapshots to diverge significantly.</span></span> <span data-ttu-id="2003a-184">Místo toho aktualizovat pomocí nejmenšího počtu bloků [PutBlock] [ dotnet_PutBlock] a [PutBlockList] [ dotnet_PutBlockList] metody.</span><span class="sxs-lookup"><span data-stu-id="2003a-184">Instead, update the fewest possible number of blocks by using the [PutBlock][dotnet_PutBlock] and [PutBlockList][dotnet_PutBlockList] methods.</span></span>

### <a name="snapshot-billing-scenarios"></a><span data-ttu-id="2003a-185">Snímek fakturace scénáře</span><span class="sxs-lookup"><span data-stu-id="2003a-185">Snapshot billing scenarios</span></span>
<span data-ttu-id="2003a-186">Následující scénáře ukazují, jak nabíhat poplatky pro objekt blob bloku a jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="2003a-186">The following scenarios demonstrate how charges accrue for a block blob and its snapshots.</span></span>

<span data-ttu-id="2003a-187">**Scénář 1**</span><span class="sxs-lookup"><span data-stu-id="2003a-187">**Scenario 1**</span></span>

<span data-ttu-id="2003a-188">Ve scénáři 1 základní objekt blob nebyla aktualizována po pořízení snímku, takže poplatky se vám neúčtují pouze pro jedinečný bloky 1, 2 a 3.</span><span class="sxs-lookup"><span data-stu-id="2003a-188">In scenario 1, the base blob has not been updated after the snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

<span data-ttu-id="2003a-190">**Scénář 2**</span><span class="sxs-lookup"><span data-stu-id="2003a-190">**Scenario 2**</span></span>

<span data-ttu-id="2003a-191">Ve scénáři 2 základní objekt blob se aktualizovalo, ale má snímku není.</span><span class="sxs-lookup"><span data-stu-id="2003a-191">In scenario 2, the base blob has been updated, but the snapshot has not.</span></span> <span data-ttu-id="2003a-192">Blok 3 byla aktualizována, a to i v případě, že obsahuje stejná data a stejným ID, není stejný jako blokovat 3 ve snímku.</span><span class="sxs-lookup"><span data-stu-id="2003a-192">Block 3 was updated, and even though it contains the same data and the same ID, it is not the same as block 3 in the snapshot.</span></span> <span data-ttu-id="2003a-193">Účet je v důsledku toho účtovat čtyři bloky.</span><span class="sxs-lookup"><span data-stu-id="2003a-193">As a result, the account is charged for four blocks.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

<span data-ttu-id="2003a-195">**Scénář 3**</span><span class="sxs-lookup"><span data-stu-id="2003a-195">**Scenario 3**</span></span>

<span data-ttu-id="2003a-196">Ve scénáři 3 základní objekt blob se aktualizovalo, ale má snímku není.</span><span class="sxs-lookup"><span data-stu-id="2003a-196">In scenario 3, the base blob has been updated, but the snapshot has not.</span></span> <span data-ttu-id="2003a-197">Blok 3 byla nahrazena blok 4 v základní objektu blob, ale snímku odráží bloku 3.</span><span class="sxs-lookup"><span data-stu-id="2003a-197">Block 3 was replaced with block 4 in the base blob, but the snapshot still reflects block 3.</span></span> <span data-ttu-id="2003a-198">Účet je v důsledku toho účtovat čtyři bloky.</span><span class="sxs-lookup"><span data-stu-id="2003a-198">As a result, the account is charged for four blocks.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

<span data-ttu-id="2003a-200">**Scénář 4**</span><span class="sxs-lookup"><span data-stu-id="2003a-200">**Scenario 4**</span></span>

<span data-ttu-id="2003a-201">V případě 4 základní objekt blob se aktualizovala kompletně a obsahuje žádný z jeho původní bloky.</span><span class="sxs-lookup"><span data-stu-id="2003a-201">In scenario 4, the base blob has been completely updated and contains none of its original blocks.</span></span> <span data-ttu-id="2003a-202">Účet je v důsledku toho účtovat všechny bloky osm jedinečný.</span><span class="sxs-lookup"><span data-stu-id="2003a-202">As a result, the account is charged for all eight unique blocks.</span></span> <span data-ttu-id="2003a-203">Tato situace může nastat, pokud používáte metodu aktualizace, jako [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray][dotnet_UploadFromByteArray], protože tyto metody nahradit veškerý obsah objektu blob.</span><span class="sxs-lookup"><span data-stu-id="2003a-203">This scenario can occur if you are using an update method such as [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray], because these methods replace all of the contents of a blob.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a><span data-ttu-id="2003a-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2003a-205">Next steps</span></span>

* <span data-ttu-id="2003a-206">Můžete najít další informace o práci s snímků disku virtuálního počítače (VM) v [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](storage-incremental-snapshots.md)</span><span class="sxs-lookup"><span data-stu-id="2003a-206">You can find more information about working with virtual machine (VM) disk snapshots in [Back up Azure unmanaged VM disks with incremental snapshots](storage-incremental-snapshots.md)</span></span>

* <span data-ttu-id="2003a-207">Další příklady kódu pomocí úložiště objektů Blob, najdete v části [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span><span class="sxs-lookup"><span data-stu-id="2003a-207">For additional code examples using Blob storage, see [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span></span> <span data-ttu-id="2003a-208">Můžete stáhnout ukázkovou aplikaci a potom ho spusťte nebo procházet kód na Githubu.</span><span class="sxs-lookup"><span data-stu-id="2003a-208">You can download a sample application and run it, or browse the code on GitHub.</span></span>

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