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
# <a name="create-a-blob-snapshot"></a><span data-ttu-id="27779-104">Vytvoření snímku objektu blob</span><span class="sxs-lookup"><span data-stu-id="27779-104">Create a blob snapshot</span></span>

<span data-ttu-id="27779-105">Snímek je jen pro čtení verze objektu blob, který se pořídí na bod v čase.</span><span class="sxs-lookup"><span data-stu-id="27779-105">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="27779-106">Snímky jsou užitečné pro zálohování objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="27779-106">Snapshots are useful for backing up blobs.</span></span> <span data-ttu-id="27779-107">Po vytvoření snímku, číst, kopírovat nebo odstranit, ale nelze je změnit.</span><span class="sxs-lookup"><span data-stu-id="27779-107">After you create a snapshot, you can read, copy, or delete it, but you cannot modify it.</span></span>

<span data-ttu-id="27779-108">Snímek objektu blob je základní objekt blob identické tooits, s výjimkou tomuto objektu blob hello má identifikátor URI **data a času** hodnota připojena toohello blob URI tooindicate hello čas na které hello pořízení snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-108">A snapshot of a blob is identical tooits base blob, except that hello blob URI has a **DateTime** value appended toohello blob URI tooindicate hello time at which hello snapshot was taken.</span></span> <span data-ttu-id="27779-109">Například, pokud na stránce blob identifikátor URI je `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello snímku identifikátor URI je příliš podobné`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span><span class="sxs-lookup"><span data-stu-id="27779-109">For example, if a page blob URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello snapshot URI is similar too`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span></span>

> [!NOTE]
> <span data-ttu-id="27779-110">Všechny snímky sdílet hello základní je identifikátor URI objektu blob.</span><span class="sxs-lookup"><span data-stu-id="27779-110">All snapshots share hello base blob's URI.</span></span> <span data-ttu-id="27779-111">Hello jenom rozdíl mezi hello základní objekt blob a hello snímku je hello připojí **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27779-111">hello only distinction between hello base blob and hello snapshot is hello appended **DateTime** value.</span></span>
>

<span data-ttu-id="27779-112">Objekt blob může mít libovolný počet snímků.</span><span class="sxs-lookup"><span data-stu-id="27779-112">A blob can have any number of snapshots.</span></span> <span data-ttu-id="27779-113">Snímky uchová, dokud explicitně odstranit.</span><span class="sxs-lookup"><span data-stu-id="27779-113">Snapshots persist until they are explicitly deleted.</span></span> <span data-ttu-id="27779-114">Snímek nemůže outlive jeho základní objekt blob.</span><span class="sxs-lookup"><span data-stu-id="27779-114">A snapshot cannot outlive its base blob.</span></span> <span data-ttu-id="27779-115">Můžete vytvořit výčet přidružené hello základní objekt blob tootrack snímky hello vaše aktuální snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-115">You can enumerate hello snapshots associated with hello base blob tootrack your current snapshots.</span></span>

<span data-ttu-id="27779-116">Při vytváření snímku objektu blob hello vlastnosti systému objektu blob jsou zkopírované toohello snímku s hello stejné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27779-116">When you create a snapshot of a blob, hello blob's system properties are copied toohello snapshot with hello same values.</span></span> <span data-ttu-id="27779-117">Hello základní metadata objektu blob na je také zkopírovaný toohello snímku, pokud nezadáte samostatné metadata pro hello snímek při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="27779-117">hello base blob's metadata is also copied toohello snapshot, unless you specify separate metadata for hello snapshot when you create it.</span></span>

<span data-ttu-id="27779-118">Všechny zapůjčení přidružený objekt blob základní hello neovlivňují hello snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-118">Any leases associated with hello base blob do not affect hello snapshot.</span></span> <span data-ttu-id="27779-119">Nelze získat zapůjčení na snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-119">You cannot acquire a lease on a snapshot.</span></span>

<span data-ttu-id="27779-120">Soubor virtuálního pevného disku je použité toostore hello aktuální informace a stav pro disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="27779-120">A VHD file is used toostore hello current information and status for a VM disk.</span></span> <span data-ttu-id="27779-121">Můžete odpojit disk z v rámci hello virtuálního počítače nebo vypnout hello virtuálních počítačů a pak proveďte snímek jeho souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="27779-121">You can detach a disk from within hello VM or shut down hello VM, and then take a snapshot of its VHD file.</span></span> <span data-ttu-id="27779-122">Můžete použít tento soubor snímku novější tooretrieve hello virtuálního pevného disku soubor v tomto bodě v čase a znovu vytvořte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="27779-122">You can use that snapshot file later tooretrieve hello VHD file at that point in time and recreate hello VM.</span></span>

<span data-ttu-id="27779-123">Pokud je povolené šifrování služby úložiště (SSE) pro účet úložiště hello v které hello objekt blob se nachází, a potom zašifruje všechny snímky v úvahu tomuto objektu blob v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="27779-123">If Storage Service Encryption (SSE) is enabled for hello storage account in which hello blob resides, then any snapshots taken of that blob will be encrypted at rest.</span></span>

## <a name="create-a-snapshot"></a><span data-ttu-id="27779-124">Vytvoření snímku</span><span class="sxs-lookup"><span data-stu-id="27779-124">Create a snapshot</span></span>
<span data-ttu-id="27779-125">Hello následující příklad kódu ukazuje, jak toocreate snímek pomocí hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="27779-125">hello following code example shows how toocreate a snapshot by using hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span> <span data-ttu-id="27779-126">Tento příklad určuje dalších metadat pro vytvoření snímku hello při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="27779-126">This example specifies additional metadata for hello snapshot when it is created.</span></span>

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

## <a name="copy-snapshots"></a><span data-ttu-id="27779-127">Kopie snímků</span><span class="sxs-lookup"><span data-stu-id="27779-127">Copy snapshots</span></span>
<span data-ttu-id="27779-128">Operace kopírování zahrnující objekty BLOB a snímky postupujte podle těchto pravidel:</span><span class="sxs-lookup"><span data-stu-id="27779-128">Copy operations involving blobs and snapshots follow these rules:</span></span>

* <span data-ttu-id="27779-129">Snímek můžete zkopírovat přes jeho základní objekt blob.</span><span class="sxs-lookup"><span data-stu-id="27779-129">You can copy a snapshot over its base blob.</span></span> <span data-ttu-id="27779-130">Povýšení pozice toohello snímku hello základní objekt blob, můžete obnovit dřívější verzi objektu blob.</span><span class="sxs-lookup"><span data-stu-id="27779-130">By promoting a snapshot toohello position of hello base blob, you can restore an earlier version of a blob.</span></span> <span data-ttu-id="27779-131">Hello snímku zůstane, ale dojde k přepsání základní objekt blob hello kopii hello snímku s možností zápisu.</span><span class="sxs-lookup"><span data-stu-id="27779-131">hello snapshot remains, but hello base blob is overwritten with a writable copy of hello snapshot.</span></span>
* <span data-ttu-id="27779-132">Můžete kopírovat objekt blob pro cílový snímku tooa s jiným názvem.</span><span class="sxs-lookup"><span data-stu-id="27779-132">You can copy a snapshot tooa destination blob with a different name.</span></span> <span data-ttu-id="27779-133">Hello výsledné cílový objekt blob je s možností zápisu objektů blob a není snímek.</span><span class="sxs-lookup"><span data-stu-id="27779-133">hello resulting destination blob is a writable blob and not a snapshot.</span></span>
* <span data-ttu-id="27779-134">Po zkopírování zdrojový objekt blob nejsou cílové zkopírovaný toohello všechny snímky hello zdrojový objekt blob.</span><span class="sxs-lookup"><span data-stu-id="27779-134">When a source blob is copied, any snapshots of hello source blob are not copied toohello destination.</span></span> <span data-ttu-id="27779-135">Pokud cílový objekt blob je přepsat kopie, všechny snímky přidružené hello původní cílový objekt blob zůstanou beze změn.</span><span class="sxs-lookup"><span data-stu-id="27779-135">When a destination blob is overwritten with a copy, any snapshots associated with hello original destination blob remain intact.</span></span>
* <span data-ttu-id="27779-136">Při vytváření snímku objekt blob bloku potvrdit blokovaných hello blob je také zkopírovaný toohello snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-136">When you create a snapshot of a block blob, hello blob's committed block list is also copied toohello snapshot.</span></span> <span data-ttu-id="27779-137">Všechny nepotvrzené bloky nebudou zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="27779-137">Any uncommitted blocks are not copied.</span></span>

## <a name="specify-an-access-condition"></a><span data-ttu-id="27779-138">Zadejte podmínku přístup</span><span class="sxs-lookup"><span data-stu-id="27779-138">Specify an access condition</span></span>
<span data-ttu-id="27779-139">Při volání [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], můžete zadat podmínku přístup, tak, aby hello snímku se vytvoří pouze v případě, že je splněna podmínka.</span><span class="sxs-lookup"><span data-stu-id="27779-139">When you call [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], you can specify an access condition so that hello snapshot is created only if a condition is met.</span></span> <span data-ttu-id="27779-140">toospecify podmínku přístup pomocí hello [AccessCondition] [ dotnet_AccessCondition] parametr.</span><span class="sxs-lookup"><span data-stu-id="27779-140">toospecify an access condition, use hello [AccessCondition][dotnet_AccessCondition] parameter.</span></span> <span data-ttu-id="27779-141">Pokud zadaný hello není podmínka splněná, není-li vytvořit snímek hello a hello služby objektů Blob vrátí stavový kód [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.</span><span class="sxs-lookup"><span data-stu-id="27779-141">If hello specified condition is not met, hello snapshot is not created, and hello Blob service returns status code [HTTPStatusCode][dotnet_HTTPStatusCode].PreconditionFailed.</span></span>

## <a name="delete-snapshots"></a><span data-ttu-id="27779-142">Odstraňte snímky</span><span class="sxs-lookup"><span data-stu-id="27779-142">Delete snapshots</span></span>
<span data-ttu-id="27779-143">Objekt blob se snímky nelze odstranit, pokud budou odstraněny také hello snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-143">You can't delete a blob with snapshots unless hello snapshots are also deleted.</span></span> <span data-ttu-id="27779-144">Lze odstranit snímek jednotlivě, nebo určit, že při odstranění hello zdrojový objekt blob odstranit všechny snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-144">You can delete a snapshot individually, or specify that all snapshots be deleted when hello source blob is deleted.</span></span> <span data-ttu-id="27779-145">Když zkusíte toodelete objekt blob, který má stále snímky, bude výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="27779-145">If you attempt toodelete a blob that still has snapshots, an error results.</span></span>

<span data-ttu-id="27779-146">Následující příklad ukazuje kód jak Hello toodelete objekt blob a jeho snímky v rozhraní .NET, kde `blockBlob` je objekt typu [CloudBlockBlob][dotnet_CloudBlockBlob]:</span><span class="sxs-lookup"><span data-stu-id="27779-146">hello following code example shows how toodelete a blob and its snapshots in .NET, where `blockBlob` is an object of type [CloudBlockBlob][dotnet_CloudBlockBlob]:</span></span>

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a><span data-ttu-id="27779-147">Snímky s Azure Premium Storage</span><span class="sxs-lookup"><span data-stu-id="27779-147">Snapshots with Azure Premium Storage</span></span>
<span data-ttu-id="27779-148">Při používání snímky Storage úrovně Premium, platí následující pravidla hello:</span><span class="sxs-lookup"><span data-stu-id="27779-148">When using snapshots with Premium Storage, hello following rules apply:</span></span>

* <span data-ttu-id="27779-149">maximální počet snímků za objekt blob stránky v účtu úložiště premium Hello je 100.</span><span class="sxs-lookup"><span data-stu-id="27779-149">hello maximum number of snapshots per page blob in a premium storage account is 100.</span></span> <span data-ttu-id="27779-150">Pokud je tento limit překročen, hello snímku Blob operace vrátí kód chyby 409 (`SnapshotCountExceeded`).</span><span class="sxs-lookup"><span data-stu-id="27779-150">If that limit is exceeded, hello Snapshot Blob operation returns error code 409 (`SnapshotCountExceeded`).</span></span>
* <span data-ttu-id="27779-151">Snímek objekt blob stránky může trvat v účtu úložiště premium každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="27779-151">You can take a snapshot of a page blob in a premium storage account once every 10 minutes.</span></span> <span data-ttu-id="27779-152">Pokud dojde k překročení tohoto kurzu, hello snímku Blob operace vrátí kód chyby 409 (`SnapshotOperationRateExceeded`).</span><span class="sxs-lookup"><span data-stu-id="27779-152">If that rate is exceeded, hello Snapshot Blob operation returns error code 409 (`SnapshotOperationRateExceeded`).</span></span>
* <span data-ttu-id="27779-153">tooread snímek, můžete použít toocopy operace kopírování objektu Blob hello objekt blob stránky tooanother snímku v účtu hello.</span><span class="sxs-lookup"><span data-stu-id="27779-153">tooread a snapshot, you can use hello Copy Blob operation toocopy a snapshot tooanother page blob in hello account.</span></span> <span data-ttu-id="27779-154">Hello cílový objekt blob pro operace kopírování hello nesmí mít všechny existující snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-154">hello destination blob for hello copy operation must not have any existing snapshots.</span></span> <span data-ttu-id="27779-155">Pokud cílový objekt blob hello má snímky, pak hello operace kopírování objektu Blob vrátí kód chyby 409 (`SnapshotsPresent`).</span><span class="sxs-lookup"><span data-stu-id="27779-155">If hello destination blob does have snapshots, then hello Copy Blob operation returns error code 409 (`SnapshotsPresent`).</span></span>

## <a name="return-hello-absolute-uri-tooa-snapshot"></a><span data-ttu-id="27779-156">Vrátí hello absolutní identifikátor URI tooa snímku</span><span class="sxs-lookup"><span data-stu-id="27779-156">Return hello absolute URI tooa snapshot</span></span>
<span data-ttu-id="27779-157">Tento příklad kódu C# vytvoří snímek a zapíše se hello absolutní identifikátor URI pro primární umístění hello.</span><span class="sxs-lookup"><span data-stu-id="27779-157">This C# code example creates a snapshot and writes out hello absolute URI for hello primary location.</span></span>

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

## <a name="understand-how-snapshots-accrue-charges"></a><span data-ttu-id="27779-158">Pochopit, jak snímky nabíhat poplatky</span><span class="sxs-lookup"><span data-stu-id="27779-158">Understand how snapshots accrue charges</span></span>
<span data-ttu-id="27779-159">Vytvoření snímku, která je jen pro čtení kopie objektu blob, může mít za následek účet tooyour poplatky za úložiště další data.</span><span class="sxs-lookup"><span data-stu-id="27779-159">Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges tooyour account.</span></span> <span data-ttu-id="27779-160">Při navrhování vaší aplikace, je důležité vědět, jak může tyto poplatky nabíhat tak, aby můžete minimalizovat náklady toobe.</span><span class="sxs-lookup"><span data-stu-id="27779-160">When designing your application, it is important toobe aware of how these charges might accrue so that you can minimize costs.</span></span>

### <a name="important-billing-considerations"></a><span data-ttu-id="27779-161">Důležité aspekty fakturace</span><span class="sxs-lookup"><span data-stu-id="27779-161">Important billing considerations</span></span>
<span data-ttu-id="27779-162">Hello následující seznam obsahuje klíčové body tooconsider při vytváření snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-162">hello following list includes key points tooconsider when creating a snapshot.</span></span>

* <span data-ttu-id="27779-163">Váš účet úložiště způsobuje poplatky za jedinečný bloky nebo stránky, ať už v objektu blob hello nebo hello snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-163">Your storage account incurs charges for unique blocks or pages, whether they are in hello blob or in hello snapshot.</span></span> <span data-ttu-id="27779-164">Váš účet není způsobí nárůst nákladů pro snímky přidružený objekt blob, dokud neaktualizujete hello objektů blob, na kterém jsou založena.</span><span class="sxs-lookup"><span data-stu-id="27779-164">Your account does not incur additional charges for snapshots associated with a blob until you update hello blob on which they are based.</span></span> <span data-ttu-id="27779-165">Po aktualizaci základní objekt blob hello diverges z jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="27779-165">After you update hello base blob, it diverges from its snapshots.</span></span> <span data-ttu-id="27779-166">V takovém případě vám budou účtovat hello jedinečný bloky a stránky v jednotlivých objektů blob nebo snímek.</span><span class="sxs-lookup"><span data-stu-id="27779-166">When this happens, you are charged for hello unique blocks or pages in each blob or snapshot.</span></span>
* <span data-ttu-id="27779-167">Když nahradíte blok v rámci objekt blob bloku, tomto bloku je následně účtovat jako blok jedinečný.</span><span class="sxs-lookup"><span data-stu-id="27779-167">When you replace a block within a block blob, that block is subsequently charged as a unique block.</span></span> <span data-ttu-id="27779-168">To platí i v případě, že má hello bloku hello stejné blokovat ID a hello stejné dat, protože má v hello snímku.</span><span class="sxs-lookup"><span data-stu-id="27779-168">This is true even if hello block has hello same block ID and hello same data as it has in hello snapshot.</span></span> <span data-ttu-id="27779-169">Po hello blok je potvrzená znovu ji diverges od jeho protějšku v jakékoli snímek a vám bude účtována pro svoje data.</span><span class="sxs-lookup"><span data-stu-id="27779-169">After hello block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data.</span></span> <span data-ttu-id="27779-170">Hello to samé platí pro stránku v objekt blob stránky, který se aktualizuje identické daty.</span><span class="sxs-lookup"><span data-stu-id="27779-170">hello same holds true for a page in a page blob that's updated with identical data.</span></span>
* <span data-ttu-id="27779-171">Nahrazení objekt blob bloku pomocí volání hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], nebo [UploadFromByteArray] [ dotnet_UploadFromByteArray] metoda nahrazuje všechny bloky v objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="27779-171">Replacing a block blob by calling hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] method replaces all blocks in hello blob.</span></span> <span data-ttu-id="27779-172">Pokud máte snímek přidružené k tomuto objektu blob, teď odchýlit všechny bloky v hello základní objekt blob a snímků a vám bude účtována pro všechny bloky hello v obou objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="27779-172">If you have a snapshot associated with that blob, all blocks in hello base blob and snapshot now diverge, and you will be charged for all hello blocks in both blobs.</span></span> <span data-ttu-id="27779-173">To platí i v případě, že data hello v hello základní objekt blob a hello snímku zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="27779-173">This is true even if hello data in hello base blob and hello snapshot remain identical.</span></span>
* <span data-ttu-id="27779-174">Hello služby objektů Blob Azure nemá toodetermine znamená, zda dva bloky obsahují stejné údaje.</span><span class="sxs-lookup"><span data-stu-id="27779-174">hello Azure Blob service does not have a means toodetermine whether two blocks contain identical data.</span></span> <span data-ttu-id="27779-175">Každý blok, který je odeslán a potvrzené považuje jako jedinečné, i v případě, že se má hello stejným datům a hello stejné blokovat ID.</span><span class="sxs-lookup"><span data-stu-id="27779-175">Each block that is uploaded and committed is treated as unique, even if it has hello same data and hello same block ID.</span></span> <span data-ttu-id="27779-176">Pro jedinečný bloky nabíhat poplatky, proto je důležité tooconsider, který aktualizuje objekt blob, která obsahuje snímek za následek další jedinečné bloky a dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="27779-176">Because charges accrue for unique blocks, it's important tooconsider that updating a blob that has a snapshot results in additional unique blocks and additional charges.</span></span>

### <a name="minimize-cost-with-snapshot-management"></a><span data-ttu-id="27779-177">Minimalizovat náklady s Správa snímků</span><span class="sxs-lookup"><span data-stu-id="27779-177">Minimize cost with snapshot management</span></span>

<span data-ttu-id="27779-178">Doporučujeme pečlivě správě vaší snímků tooavoid další poplatky.</span><span class="sxs-lookup"><span data-stu-id="27779-178">We recommend managing your snapshots carefully tooavoid extra charges.</span></span> <span data-ttu-id="27779-179">Tyto doporučené postupy pro toohelp minimalizovat náklady hello způsobené hello úložiště vaše snímků můžete postupovat podle:</span><span class="sxs-lookup"><span data-stu-id="27779-179">You can follow these best practices toohelp minimize hello costs incurred by hello storage of your snapshots:</span></span>

* <span data-ttu-id="27779-180">Odstranit a znovu vytvořit snímky přidružený objekt blob při každé aktualizaci objektu blob hello, i když jsou aktualizace s stejné údaje, pokud váš návrh aplikace vyžaduje udržovat snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-180">Delete and re-create snapshots associated with a blob whenever you update hello blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots.</span></span> <span data-ttu-id="27779-181">Odstranit a znovu vytváření snímků hello blob, můžete zajistit, že není odchýlit hello blob a snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-181">By deleting and re-creating hello blob's snapshots, you can ensure that hello blob and snapshots do not diverge.</span></span>
* <span data-ttu-id="27779-182">Pokud udržujete snímků pro objekt blob, vyhněte se volání [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray] [ dotnet_UploadFromByteArray] tooupdate hello blob.</span><span class="sxs-lookup"><span data-stu-id="27779-182">If you are maintaining snapshots for a blob, avoid calling [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] tooupdate hello blob.</span></span> <span data-ttu-id="27779-183">Tyto metody nahraďte všechny bloky hello hello objektu BLOB, výrazně způsobuje objektu blob služby základní a jeho toodiverge snímky.</span><span class="sxs-lookup"><span data-stu-id="27779-183">These methods replace all of hello blocks in hello blob, causing your base blob and its snapshots toodiverge significantly.</span></span> <span data-ttu-id="27779-184">Místo toho aktualizace hello nejmenšího možný počet bloků s použitím hello [PutBlock] [ dotnet_PutBlock] a [PutBlockList] [ dotnet_PutBlockList] metody.</span><span class="sxs-lookup"><span data-stu-id="27779-184">Instead, update hello fewest possible number of blocks by using hello [PutBlock][dotnet_PutBlock] and [PutBlockList][dotnet_PutBlockList] methods.</span></span>

### <a name="snapshot-billing-scenarios"></a><span data-ttu-id="27779-185">Snímek fakturace scénáře</span><span class="sxs-lookup"><span data-stu-id="27779-185">Snapshot billing scenarios</span></span>
<span data-ttu-id="27779-186">Hello následující scénáře ukazují, jak nabíhat poplatky pro objekt blob bloku a jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="27779-186">hello following scenarios demonstrate how charges accrue for a block blob and its snapshots.</span></span>

<span data-ttu-id="27779-187">**Scénář 1**</span><span class="sxs-lookup"><span data-stu-id="27779-187">**Scenario 1**</span></span>

<span data-ttu-id="27779-188">Ve scénáři 1 základní objekt blob hello nebyla aktualizována po pořízení snímku hello, takže poplatky se vám neúčtují pouze pro jedinečný bloky 1, 2 a 3.</span><span class="sxs-lookup"><span data-stu-id="27779-188">In scenario 1, hello base blob has not been updated after hello snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

<span data-ttu-id="27779-190">**Scénář 2**</span><span class="sxs-lookup"><span data-stu-id="27779-190">**Scenario 2**</span></span>

<span data-ttu-id="27779-191">Ve scénáři 2 hello základní objekt blob se aktualizovalo, ale má hello snímku není.</span><span class="sxs-lookup"><span data-stu-id="27779-191">In scenario 2, hello base blob has been updated, but hello snapshot has not.</span></span> <span data-ttu-id="27779-192">Blok 3 byla aktualizována a to i v případě, že obsahuje hello stejná data a hello stejným ID, není stejný jako blokovat 3 v hello snímku hello.</span><span class="sxs-lookup"><span data-stu-id="27779-192">Block 3 was updated, and even though it contains hello same data and hello same ID, it is not hello same as block 3 in hello snapshot.</span></span> <span data-ttu-id="27779-193">V důsledku toho je účet hello účtovat čtyři bloky.</span><span class="sxs-lookup"><span data-stu-id="27779-193">As a result, hello account is charged for four blocks.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

<span data-ttu-id="27779-195">**Scénář 3**</span><span class="sxs-lookup"><span data-stu-id="27779-195">**Scenario 3**</span></span>

<span data-ttu-id="27779-196">Ve scénáři 3 hello základní objekt blob se aktualizovalo, ale má hello snímku není.</span><span class="sxs-lookup"><span data-stu-id="27779-196">In scenario 3, hello base blob has been updated, but hello snapshot has not.</span></span> <span data-ttu-id="27779-197">3 bloku byl nahrazen s blokem 4 v hello základní objekt blob, ale hello snímku odráží bloku 3.</span><span class="sxs-lookup"><span data-stu-id="27779-197">Block 3 was replaced with block 4 in hello base blob, but hello snapshot still reflects block 3.</span></span> <span data-ttu-id="27779-198">V důsledku toho je účet hello účtovat čtyři bloky.</span><span class="sxs-lookup"><span data-stu-id="27779-198">As a result, hello account is charged for four blocks.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

<span data-ttu-id="27779-200">**Scénář 4**</span><span class="sxs-lookup"><span data-stu-id="27779-200">**Scenario 4**</span></span>

<span data-ttu-id="27779-201">V případě 4 hello základní objekt blob se aktualizovala kompletně a obsahuje žádný z jeho původní bloky.</span><span class="sxs-lookup"><span data-stu-id="27779-201">In scenario 4, hello base blob has been completely updated and contains none of its original blocks.</span></span> <span data-ttu-id="27779-202">V důsledku toho je účtován hello účet pro všechny bloky osm jedinečný.</span><span class="sxs-lookup"><span data-stu-id="27779-202">As a result, hello account is charged for all eight unique blocks.</span></span> <span data-ttu-id="27779-203">Tato situace může nastat, pokud používáte metodu aktualizace, jako [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], nebo [UploadFromByteArray][dotnet_UploadFromByteArray], protože tyto metody nahrazovat všechny hello obsah objektu blob.</span><span class="sxs-lookup"><span data-stu-id="27779-203">This scenario can occur if you are using an update method such as [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray], because these methods replace all of hello contents of a blob.</span></span>

![Prostředky Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a><span data-ttu-id="27779-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27779-205">Next steps</span></span>

* <span data-ttu-id="27779-206">Můžete najít další informace o práci s snímků disku virtuálního počítače (VM) v [zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky](../../virtual-machines/windows/incremental-snapshots.md)</span><span class="sxs-lookup"><span data-stu-id="27779-206">You can find more information about working with virtual machine (VM) disk snapshots in [Back up Azure unmanaged VM disks with incremental snapshots](../../virtual-machines/windows/incremental-snapshots.md)</span></span>

* <span data-ttu-id="27779-207">Další příklady kódu pomocí úložiště objektů Blob, najdete v části [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span><span class="sxs-lookup"><span data-stu-id="27779-207">For additional code examples using Blob storage, see [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span></span> <span data-ttu-id="27779-208">Můžete stáhnout ukázkovou aplikaci a potom ho spusťte nebo přejít hello kódu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="27779-208">You can download a sample application and run it, or browse hello code on GitHub.</span></span>

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