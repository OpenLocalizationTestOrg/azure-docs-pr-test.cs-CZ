---
title: "aaaTutorial - použití hello Azure Batch Klientská knihovna pro .NET | Microsoft Docs"
description: "Informace hello základními koncepty Azure Batch a vytvoření jednoduché řešení pomocí rozhraní .NET."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 76cb9807-cbc1-405a-8136-d1e53e66e82b
ms.service: batch
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06062b3886a8081bd9a831824a981503ef55f9b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-building-solutions-with-hello-batch-client-library-for-net"></a>Začínáme sestavování řešení s použitím hello Batch Klientská knihovna pro .NET

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Další hello Základy [Azure Batch] [ azure_batch] a hello [Batch .NET] [ net_api] knihovny v tomto článku probereme C# ukázkové aplikace krok podle krok. Podíváme na tom, jak ukázková aplikace hello využívá tooprocess služby Batch hello paralelní úlohy v cloudu hello a jak komunikuje s [Azure Storage](../storage/common/storage-introduction.md) pro přípravě a načítání souborů. Budete další běžné pracovním postupem aplikací Batch a získáte základní přehled o hello hlavním součástem služby Batch například úlohách, úkolech, fondech a výpočetních uzlů.

![Pracovní postup řešení Batch (Basic)][11]<br/>

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte praktické znalosti jazyka C# a sady Visual Studio. Předpokládá také, že jste možnost toosatisfy hello účet vytvoření požadavky, které jsou uvedeny níže Azure a hello Batch a služby úložiště.

### <a name="accounts"></a>Účty
* **Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet Azure][azure_free_account].
* **Účet Batch**: Po pořízení předplatného Azure si [vytvořte účet Azure Batch](batch-account-create-portal.md).
* **Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/common/storage-create-storage-account.md).

> [!IMPORTANT]
> Batch aktuálně podporuje *pouze* hello **pro obecné účely** typ účtu úložiště, jak je popsáno v kroku #5 [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v [o Azure účty úložiště](../storage/common/storage-create-storage-account.md).
>
>

### <a name="visual-studio"></a>Visual Studio
Musíte mít **Visual Studio 2015 nebo novější** toobuild hello ukázkový projekt. Bezplatné a zkušební verze sady Visual Studio můžete najít v hello [přehled produktů Visual Studio][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Ukázka kódu *DotNetTutorial*
Hello [DotNetTutorial] [ github_dotnettutorial] ukázka je jednou z mnoha ukázek kódu služby Batch najít v hello hello [azure-batch-samples] [ github_samples] úložiště v GitHub. Všechny ukázky hello si můžete stáhnout kliknutím **klonovat nebo stáhnout > stáhnout ZIP** na domovskou stránku hello úložiště nebo kliknutím hello [azure-batch-samples-master.zip] [ github_samples_zip]přímý odkaz ke stažení. Po extrahování obsahu souboru ZIP hello hello, najdete v následující složce hello hello řešení:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Průzkumník Azure Batch (volitelné)
Hello [Průzkumník Azure Batch] [ github_batchexplorer] je bezplatný program, který je součástí hello [azure-batch-samples] [ github_samples] úložišti na Githubu. Při není požadováno toocomplete tohoto kurzu, může být užitečné při vývoji a ladění řešení Batch.

## <a name="dotnettutorial-sample-project-overview"></a>Přehled ukázkového projektu DotNetTutorial
Hello *DotNetTutorial* ukázka kódu je řešení sady Visual Studio, která se skládá ze dvou projektů: **DotNetTutorial** a **TaskApplication**.

* **DotNetTutorial** je hello klientskou aplikaci, která interaguje s tooexecute služby Batch a Storage hello paralelní úlohy na výpočetních uzlech (virtuálních počítačů). DotNetTutorial se spouští na místní pracovní stanici.
* **TaskApplication** je hello program, který běží na výpočetních uzlech v Azure tooperform hello samotnou práci. V ukázce hello `TaskApplication.exe` hello analyzuje text v souboru staženém ze služby Azure Storage (vstupní soubor hello). Pak se vytvoří textový soubor (hello výstupního souboru), obsahuje seznam hello první tři slova, která se zobrazují ve vstupním souboru hello. Po vytvoření výstupního souboru hello, TaskApplication odešle soubor tooAzure hello úložiště. Díky tomu je k dispozici toohello klientská aplikace ke stažení. TaskApplication běží paralelně v několika výpočetních uzlech v hello služby Batch.

Hello následující diagram znázorňuje primární operace hello, které provádí klientská aplikace hello, *DotNetTutorial*a hello aplikaci, kterou spouští úkoly hello *TaskApplication*. Tento základní pracovní postup je typický pro mnoho výpočetních řešení, která jsou vytvořená pomocí služby Batch. Když nepředvádí všechny funkce, které jsou k dispozici v hello služby Batch, téměř každý scénář Batch zahrnuje části tohoto pracovního postupu.

![Ukázkový pracovní postup služby Batch][8]<br/>

[**Krok 1.**](#step-1-create-storage-containers) Ve službě Azure Blob Storage vytvořte **kontejnery** .<br/>
[**Krok 2.**](#step-2-upload-task-application-and-data-files) Odesílání úloh aplikace soubory a vstupní soubory toocontainers.<br/>
[**Krok 3.**](#step-3-create-batch-pool) Vytvořte **fond** Batch.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Hello fondu **StartTask** stahování hello úkolů (TaskApplication) binární soubory toonodes připojí hello fondu.<br/>
[**Krok 4.**](#step-4-create-batch-job) Vytvořte **úlohu** Batch.<br/>
[**Krok 5.**](#step-5-add-tasks-to-job) Přidat **úlohy** toohello úlohy.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Hello úkoly jsou naplánované tooexecute na uzlech.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Každý úkol stáhne svoje vstupní data ze služby Azure Storage a potom zahájí spuštění.<br/>
[**Krok 6.**](#step-6-monitor-tasks) Sledujte úkoly.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Jako úkoly při dokončení odesílají svoje výstupní data tooAzure úložiště.<br/>
[**Krok 7.**](#step-7-download-task-output) Stáhněte si výstup úkolu ze služby Storage.

Jak je uvedeno, Ne každé řešení Batch provede právě tyto kroky a může obsahovat i mnohem víc, ale hello *DotNetTutorial* ukázkovou aplikaci předvádí běžné procesy, které jsou v řešení Batch se nenašly.

## <a name="build-hello-dotnettutorial-sample-project"></a>Sestavení hello *DotNetTutorial* ukázkový projekt
Předtím, než můžete úspěšně spustit ukázkový text hello, musíte zadat přihlašovací údaje účtu Batch a úložiště v hello *DotNetTutorial* projektu `Program.cs` souboru. Pokud jste tak ještě neučinili, otevřete hello řešení v sadě Visual Studio dvojitým kliknutím na soubor hello `DotNetTutorial.sln` soubor řešení. Nebo ho otevřete v sadě Visual Studio pomocí hello **soubor > Otevřít > projekt nebo řešení** nabídky.

Otevřete `Program.cs` v rámci hello *DotNetTutorial* projektu. Pak zadejte svoje přihlašovací údaje jako zadanou horní hello hello souboru:

```csharp
// Update hello Batch and Storage account credential strings below with hello values
// unique tooyour accounts. These are used when constructing connection strings
// for hello Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> Jak je uvedeno nahoře, je nutné zadat aktuálně hello přihlašovací údaje pro **pro obecné účely** účet úložiště ve službě Azure Storage. Aplikace Batch používat úložiště objektů blob v rámci hello **pro obecné účely** účet úložiště. Nezadávejte hello pověření pro účet úložiště, který byl vytvořen tak, že vyberete hello *úložiště objektů Blob* typ účtu.
>
>

Přihlašovací údaje účtu Batch a Storage v rámci hello okně účtu každé služby můžete najít v hello [portál Azure][azure_portal]:

![Přihlašovací údaje hello portálu služby batch][9]
![přihlašovací údaje Storage na portálu hello][10]<br/>

Teď, když aktualizujete hello projektu pomocí svých přihlašovacích údajů, klikněte pravým tlačítkem v Průzkumníku řešení hello a klikněte na tlačítko **sestavit řešení**. Pokud se zobrazí výzva, potvrďte hello obnovení všech balíčků NuGet.

> [!TIP]
> Pokud hello balíčky NuGet automaticky neobnoví, nebo pokud se zobrazí chyby o balíčcích hello toorestore selhání, ujistěte se, že máte hello [Správce balíčků NuGet] [ nuget_packagemgr] nainstalována. Potom povolte stažení chybějících balíčků hello. V tématu [povolení obnovy balíčků během sestavení] [ nuget_restore] tooenable stahování balíčku.
>
>

V následujících částech hello jsme rozdělit hello ukázkové aplikace hello kroky, které se provádí tooprocess úloh ve hello služby Batch a popisují tyto kroky si podrobně. Doporučujeme vám toorefer toohello otevřete řešení v sadě Visual Studio při procházení hello zbývající části tohoto článku, vzhledem k tomu, že každý jednotlivý řádek kódu v ukázce hello popsané.

Přejděte toohello horní části hello `MainAsync` metoda v hello *DotNetTutorial* projektu `Program.cs` souboru toostart s krokem 1. Každý krok níže, pak přibližně způsobem hello rozšiřování metoda volá `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Krok 1: Vytvoření kontejnerů služby Storage
![Vytvoření kontejnerů ve službě Azure Storage][1]
<br/>

Batch obsahuje vestavěnou podporu pro komunikaci se službou Azure Storage. Kontejnery v účtu úložiště bude poskytovat hello soubory potřebné hello úlohy, které běží v účtu Batch. Hello kontejnery také poskytují místě toostore hello výstupní data, která hello úkoly vytvářejí. Hello nejprve thing hello *DotNetTutorial* klientská aplikace se vytvoří tři kontejnery ve [Azure Blob Storage](../storage/common/storage-introduction.md):

* **aplikace**: Tento kontejner bude ukládat aplikace hello spuštěná hello úlohy, jakož i veškeré její závislé, například knihovny DLL.
* **vstupní**: budou úkoly stahovat z hello hello datové soubory tooprocess *vstupní* kontejneru.
* **výstup**: když úkoly dokončí zpracování vstupního souboru, odešlou výsledky toohello hello *výstup* kontejneru.

V pořadí toointeract s úložiště účtu a vytvoření kontejnerů používáme hello [Klientská knihovna pro úložiště Azure pro .NET][net_api_storage]. Vytvoříme účet toohello odkaz s [CloudStorageAccount][net_cloudstorageaccount]a z té vytvoříme [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct hello Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve hello storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create hello blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Používáme hello `blobClient` odkazovat v hello aplikaci a předáváme jako parametr metody tooseveral. Příkladem je v bloku kódu hello, který následuje hello výše, kde voláme `CreateContainerIfNotExistAsync` tooactually vytvoření kontejnerů hello.

```csharp
// Use hello blob client toocreate hello containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Po vytvoření kontejnerů hello hello aplikaci teď můžete nahrát hello soubory, které se použijí tak, že úlohy hello.

> [!TIP]
> [Jak toouse úložiště Blob z .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) nabízí pěkný přehled o práci s Azure Storage kontejnery a objekty BLOB ve službě. Musí být v hello horní části seznamu čtení jako začátek práce s Batch.
>
>

## <a name="step-2-upload-task-application-and-data-files"></a>Krok 2: Nahrání aplikačních a datových souborů úkolů
![Soubory toocontainers odeslání úloh aplikace a vstupních (data)][2]
<br/>

V souboru hello operace nahrávání *DotNetTutorial* nejdřív definuje kolekce **aplikace** a **vstupní** cesty k souborům, které jsou v místním počítači hello. Pak odešle tyto soubory toohello kontejnery, které jste vytvořili v předchozím kroku hello.

```csharp
// Paths toohello executable and its dependencies that will be executed by hello tasks
List<string> applicationFilePaths = new List<string>
{
    // hello DotNetTutorial project includes a project reference tooTaskApplication,
    // allowing us toodetermine hello path of hello task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// hello collection of data files that are toobe processed by hello tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload hello application and its dependencies tooAzure Storage. This is the
// application that will process hello data files, and will be executed by each
// of hello tasks on hello compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload hello data files. This is hello data that will be processed by each of
// hello tasks that are executed on hello compute nodes within hello pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Existují dvě metody v `Program.cs` které se účastní procesu nahrávání hello:

* `UploadFilesToContainerAsync`: Tato metoda vrátí kolekci [ResourceFile] [ net_resourcefile] (viz následující popis) a interně volá `UploadFileToContainerAsync` tooupload každý soubor, který je předán hello *filePaths* parametr.
* `UploadFileToContainerAsync`: Toto je hello metoda, která ve skutečnosti provádí nahrávání souborů hello a vytvoří hello [ResourceFile] [ net_resourcefile] objekty. Po nahrání souboru hello, získá sdílený přístupový podpis (SAS) pro soubor hello a vrátí objekt ResourceFile, který ho zastupuje. Sdílené přístupové podpisy jsou také popsány níže.

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} toocontainer [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath);

        // Set hello expiry time and permissions for hello blob shared access signature.
        // In this case, no start time is specified, so hello shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct hello SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles
A [ResourceFile] [ net_resourcefile] poskytuje úlohy v dávce s soubor tooa hello adresy URL ve službě Azure Storage, který je stažený tooa výpočetního uzlu před spuštěním této úlohy. Hello [ResourceFile.BlobSource] [ net_resourcefile_blobsource] vlastnost určuje úplnou adresu URL hello hello souboru, protože existuje ve službě Azure Storage. Adresa URL Hello mohou obsahovat také sdílený přístupový podpis (SAS), který poskytuje soubor toohello zabezpečený přístup. Většina typů úkolů v rámci v Batch .NET obsahuje vlastnost *ResourceFiles* včetně:

* [CloudTask][net_task]
* [StartTask][net_pool_starttask]
* [JobPreparationTask][net_jobpreptask]
* [JobReleaseTask][net_jobreltask]

Hello ukázková aplikace DotNetTutorial nepoužívá hello JobPreparationTask nebo JobReleaseTask typy úloh, ale si můžete přečíst více o nich [spustit přípravy a dokončení úlohy na Azure Batch výpočetních uzlech](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Sdílený přístupový podpis (SAS)
Sdílené přístupové podpisy jsou řetězce, které – když jsou součástí adresy URL – poskytnout zabezpečený přístup toocontainers a objekty BLOB ve službě Azure Storage. Hello aplikace DotNetTutorial používá objektu blob i a kontejner sdíleného přístupu podpis adresy URL a ukazuje, jak tooobtain tyto sdílený přístup k řetězce podpisu z hello služby úložiště.

* **Sdílené přístupové podpisy objektů blob**: StartTask fondu hello v aplikaci DotNetTutorial používá objekt blob sdílené přístupové podpisy při stahování aplikačních binárních souborů hello a vstupních datových souborů ze služby Storage (viz krok 3 níže). Hello `UploadFileToContainerAsync` metoda v DotNetTutorial `Program.cs` obsahuje hello kód, který získá sdílený přístupový podpis jednotlivých objektů blob. Dělá to tak, že volá [CloudBlob.GetSharedAccessSignature][net_sas_blob].
* **Sdílené přístupové podpisy kontejnerů**: když každý úkol dokončí svojí práci ve výpočetním uzlu hello, odešle svůj výstupní soubor toohello *výstup* kontejneru ve službě Azure Storage. toodo Ano, používá TaskApplication sdílený přístupový podpis kontejneru, který poskytuje přístup pro zápis toohello kontejneru jako součást hello cesty při nahrávání souboru hello. Získání sdíleného přístupového podpisu kontejneru hello se provádí podobným způsobem jako při získání objektu blob hello sdílený přístupový podpis. V aplikaci DotNetTutorial zjistíte, že hello `GetContainerSasUrl` volání metod helper [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] toodo tak. Další informace o tom, jak TaskApplication používá hello kontejneru sdíleného přístupového podpisu v "krok 6: sledování úkolů."

> [!TIP]
> Podívejte se na dvě části řady hello na sdílených přístupových podpisů [část 1: pochopení hello sdílené model podpis (SAS) přístupu](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a [část 2: vytvoření a používání sdíleného přístupového podpisu (SAS) pomocí úložiště objektů Blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn více o zajišťování bezpečného přístupu toodata ve vašem účtu úložiště.
>
>

## <a name="step-3-create-batch-pool"></a>Krok 3: Vytvoření fondu služby Batch
![Vytvoření fondu Batch][3]
<br/>

**Fond** Batch je kolekce výpočetních uzlů (virtuálních počítačů), na kterých služba Batch provádí úkoly z úlohy.

Po nahrání hello aplikace a data souborů toohello účet úložiště se rozhraní API úložiště Azure, *DotNetTutorial* zahájí provádění služba Batch toohello volání rozhraní API poskytované knihovny Batch .NET hello. Hello kód nejprve vytvoří [BatchClient][net_batchclient]:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

V dalším kroku hello ukázka vytvoří fond výpočetních uzlů v účtu Batch hello pomocí volání příliš`CreatePoolIfNotExistsAsync`. `CreatePoolIfNotExistsAsync`hello používá [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metoda toocreate nový fond v hello služby Batch:

```csharp
private static async Task CreatePoolIfNotExistAsync(BatchClient batchClient, string poolId, IList<ResourceFile> resourceFiles)
{
    CloudPool pool = null;
    try
    {
        Console.WriteLine("Creating pool [{0}]...", poolId);

        // Create hello unbound pool. Until we call CloudPool.Commit() or CommitAsync(), no pool is actually created in the
        // Batch service. This CloudPool instance is therefore considered "unbound," and we can modify its properties.
        pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicatedComputeNodes: 3,                                             // 3 compute nodes
            virtualMachineSize: "small",                                                // single-core, 1.75 GB memory, 225 GB disk
            cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));   // Windows Server 2012 R2

        // Create and assign hello StartTask that will be executed when compute nodes join hello pool.
        // In this case, we copy hello StartTask's resource files (that will be automatically downloaded
        // toohello node by hello StartTask) into hello shared directory that all tasks will have access to.
        pool.StartTask = new StartTask
        {
            // Specify a command line for hello StartTask that copies hello task application files toothe
            // node's shared directory. Every compute node in a Batch pool is configured with a number
            // of pre-defined environment variables that can be referenced by commands or applications
            // run by tasks.

            // Since a successful execution of robocopy can return a non-zero exit code (e.g. 1 when one or
            // more files were successfully copied) we need toomanually exit with a 0 for Batch toorecognize
            // StartTask execution success.
            CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
            ResourceFiles = resourceFiles,
            WaitForSuccess = true
        };

        await pool.CommitAsync();
    }
    catch (BatchException be)
    {
        // Swallow hello specific error code PoolExists since that is expected if hello pool already exists
        if (be.RequestInformation?.BatchError != null && be.RequestInformation.BatchError.Code == BatchErrorCodeStrings.PoolExists)
        {
            Console.WriteLine("hello pool {0} already existed when we tried toocreate it", poolId);
        }
        else
        {
            throw; // Any other exception is unexpected
        }
    }
}
```

Při vytváření fondu s [CreatePool][net_pool_create], můžete zadat několik parametrů, třeba hello počet výpočetních uzlů, hello [velikost uzlů hello](../cloud-services/cloud-services-sizes-specs.md), a hello operační uzlů systém. V *DotNetTutorial*, používáme [CloudServiceConfiguration] [ net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 z [cloudové služby](../cloud-services/cloud-services-guestos-update-matrix.md). 

Můžete také vytvářet fondy výpočetních uzlů, které jsou virtuální počítače (VM) Azure zadáním hello [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] fondu. Fond výpočetních uzlů virtuálních počítačů můžete vytvořit z [image systému Linux](batch-linux-nodes.md) nebo Windows. Hello zdroj pro vaše Image virtuálních počítačů může být buď:

- Hello [Marketplace virtuálních počítačů Azure][vm_marketplace], který poskytuje bitové kopie systému Windows a Linux, které jsou připravené k použití. 
- Vlastní image, kterou připravíte a poskytnete. Další informace o vlastních imagích najdete v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md#pool).

> [!IMPORTANT]
> Za výpočetní prostředky ve službě Batch vám budou účtované poplatky. můžete snížit náklady toominimize `targetDedicatedComputeNodes` too1 před spuštěním ukázka hello.
>
>

Spolu s těmito fyzickými vlastnostmi uzlu můžete určit také [StartTask] [ net_pool_starttask] hello fondu. Hello StartTask se spustí na každém uzlu jako takový uzel připojí hello fondu a pokaždé, když je uzel restartován. Hello StartTask je zvláště užitečná pro instalaci aplikací na výpočetní uzly předchozí toohello provedení úlohy. Například pokud vaše úlohy zpracování dat pomocí skriptů Python, můžete použít StartTask tooinstall Pythonu na výpočetní uzly hello.

V této ukázkové aplikaci hello StartTask zkopíruje hello soubory, které stáhne ze služby Storage (které jsou určené pomocí hello [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] vlastnost) z hello StartTask pracovní adresář toohello sdíleného adresáře, *všechny* úkoly spuštěné na uzlu hello přístup. V podstatě zkopíruje `TaskApplication.exe` a jeho závislosti toohello sdíleného adresáře v každém uzlu jako hello uzel připojí hello fondu, tak, aby všechny úlohy, které běží na uzlu hello k němu přístup.

> [!TIP]
> Hello **balíčky aplikací** funkce služby Azure Batch poskytuje jiný způsob tooget aplikaci na hello výpočetních uzlů ve fondu. V tématu [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md) podrobnosti.
>
>

Také je zajímavé ve fragmentu kódu hello výše hello použití dvou proměnných prostředí v hello *CommandLine* vlastnost hello StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` a `%AZ_BATCH_NODE_SHARED_DIR%`. Každý výpočetní uzel v rámci fondu Batch je automaticky nakonfigurovaný pomocí řady proměnných prostředí, které jsou specifické tooBatch. Jakýkoli proces spuštěný úkolem má přístup k proměnným prostředí toothese.

> [!TIP]
> toofind Další informace o hello proměnné prostředí, které jsou dostupné na výpočetní uzlech ve fondu Batch a informace o pracovních adresářích úkolu najdete v části hello [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) a [souborů a adresářů ](batch-api-basics.md#files-and-directories) části hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md).
>
>

## <a name="step-4-create-batch-job"></a>Krok 4: Vytvoření úlohy Batch
![Vytvoření úlohy Batch][4]<br/>

**Úloha** Batch je kolekcí úkolů a je přidružená k fondu výpočetních uzlů. Hello úlohy pro úlohu provést hello přidružené fondu výpočetních uzlů.

Úlohu můžete použít nejen k uspořádání a sledování úkolů v souvisejících úlohách, ale také pro nastavení určitých omezení – například maximálního runtime hello hello úlohy (a rozšíření, její úkoly) a také priority úloh ve vztahu tooother úloh v hello Batch účet. V tomto příkladu však hello úlohy je přidruženo pouze hello fondu, který byl vytvořen v kroku #3. Žádné další vlastnosti se nekonfigurují.

Všechny úlohy Batch jsou přidružené ke konkrétnímu fondu. Toto přidružení označuje uzly, které hello úlohy se spustí na. Toto určíte pomocí hello [CloudJob.PoolInformation] [ net_job_poolinfo] vlastnost, jak je znázorněno v následující fragment kódu hello.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Teď, když vytvořil úlohu úlohy se přidají pracovní tooperform hello.

## <a name="step-5-add-tasks-toojob"></a>Krok 5: Přidejte toojob úlohy
![Přidat toojob úlohy][5]<br/>
*(1) úkoly jsou přidány toohello úlohy, hello (2) úkoly jsou naplánované toorun na uzlech a hello (3) úkoly stahují hello data souborů tooprocess*

Batch **úlohy** jsou hello jednotlivé jednotky práce, které jsou spouštěny na hello výpočetních uzlů. Úkol má příkazový řádek a spouští hello skripty nebo spustitelné soubory, které zadáte v takovém příkazovém řádku.

tooactually práci, musí úkoly nejprve přidat úloha tooa. Každý [CloudTask] [ net_task] se konfiguruje pomocí vlastnosti příkazového řádku a [ResourceFiles] [ net_task_resourcefiles] (stejně jako u StartTask fondu hello), Hello úkol stáhne toohello uzlu předtím, než se jeho příkazový řádek automaticky spustí. V hello *DotNetTutorial* ukázkový projekt, každý úkol zpracovává jenom jeden soubor. Proto jeho kolekce ResourceFiles obsahuje jen jeden prvek.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks toojob [{1}]...", inputFiles.Count, jobId);

    // Create a collection toohold hello tasks that we'll be adding toohello job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of hello tasks. Because we copied hello task application toothe
    // node's shared directory with hello pool's StartTask, we can access it via
    // hello shared directory on hello node that hello task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add hello tasks as a collection, as opposed tooissuing a separate AddTask call
    // for each. Bulk task submission helps tooensure efficient underlying API calls
    // toohello Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> Když přistoupí k proměnným prostředí, jako například `%AZ_BATCH_NODE_SHARED_DIR%` nebo spuštění aplikace nebyla nalezena v uzlu hello `PATH`, musí příkazové řádky úkolu předponu `cmd /c`. Tato akce explicitně provést hello překladač příkazů a dostane pokyn tooterminate po provedení příkazu. Tento požadavek není nutný, pokud vaše úkoly spouští aplikace v uzlu hello `PATH` (například *robocopy.exe* nebo *powershell.exe*) a používají se žádné proměnné prostředí.
>
>

V rámci hello `foreach` ve smyčce ve fragmentu kódu hello výše, můžete vidět, že hello příkazového řádku pro úlohu hello je vytvořený tak, aby tři argumenty příkazového řádku se předávají příliš*TaskApplication.exe*:

1. Hello **první argument** hello cesta souboru tooprocess hello. Toto je hello místní cestu toohello soubor, protože existuje na uzlu hello. Když hello objekt ResourceFile v `UploadFileToContainerAsync` vytvořen vyšší, název souboru hello byl použit pro tuto vlastnost (jako parametr konstruktor ResourceFile toohello). To znamená, že hello soubor najdete v hello stejný adresář jako *TaskApplication.exe*.
2. Hello **druhý argument** Určuje, že hello horní *N* slova zasílány toohello výstupní soubor. V ukázce hello to je pevně zakódováno, aby hello nejčastějších tří slov jsou zapsány toohello výstupní soubor.
3. Hello **třetí argument** je hello sdílený přístupový podpis (SAS), který poskytuje přístup pro zápis toohello **výstup** kontejneru ve službě Azure Storage. *TaskApplication.exe* používá tento sdílený přístup k adrese URL podpisem při nahrávání hello výstupní soubor tooAzure úložiště. Hello kódu pro tento lze najít v hello `UploadFileToContainer` metoda v projektu TaskApplication hello `Program.cs` souboru:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference toohello container using hello SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload hello file (as a new blob) toohello container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when hello Batch service
                // sets hello CloudTask.ExecutionInformation.ExitCode for hello task that
                // executed this application, it properly indicates that there was a
                // problem with hello task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Krok 6: Sledování úkolů
![Sledujte úkoly.][6]<br/>
*Hello klienta aplikace (1) monitorování hello stav dokončení a úspěšnosti úkolů a (2) hello úlohy odeslání výsledek data tooAzure úložiště*

Pokud úkoly přidáte tooa úlohy, jsou automaticky zařazeny do fronty a naplánovaných pro spuštění na výpočetních uzlech ve fondu hello spojené s úlohou hello. Na základě hello nastavení, které zadáte, služba Batch zpracovává všechny služby Řízení front úloh, plánování, opakování a dalších úloh správy povinností za vás.

Toomonitoring provádění úkolů existuje mnoho přístupů. DotNetTutorial ukazuje jednoduchý příklad, který hlásí jenom dokončení a stavy úspěchu/neúspěchu úkolu. V rámci hello `MonitorTasks` metoda v DotNetTutorial `Program.cs`, existují tři koncepty Batch .NET, které místě prodiskutovat. Jsou uvedené níže v pořadí podle jejich výskytu:

1. **ODATADetailLevel**: Zadání [ODATADetailLevel][net_odatadetaillevel] v operaci vypsání seznamu (například získání seznamu úkolů úlohy) je důležité pro zajištění výkonu aplikace Batch. Přidat [efektivní dotazování služby Azure Batch hello](batch-efficient-list-queries.md) tooyour čtení seznamu, pokud máte v úmyslu provádět jakékoli řazení sledování stavu v aplikacích Batch.
2. **TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] poskytuje aplikacím Batch .NET pomocné nástroje ke sledování stavů úkolů. V `MonitorTasks`, *DotNetTutorial* čeká na všechny úlohy tooreach [TaskState.Completed] [ net_taskstate] v časovém limitu. Potom ukončí úlohu hello.
3. **TerminateJobAsync**: ukončení úlohy pomocí [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (nebo hello blokování JobOperations.TerminateJob) označí tuto úlohu jako dokončenou. Je nezbytné toodo, pokud vaše řešení Batch používá [JobReleaseTask][net_jobreltask]. Jedná se o zvláštní typ úkolu, který je popsaný v článku [Úkoly přípravy a dokončení úlohy](batch-job-prep-release.md).

Hello `MonitorTasks` metoda z *DotNetTutorial*na `Program.cs` se zobrazí níže:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed tooreach hello Completed state within hello timeout period.";

    // Obtain hello collection of tasks currently managed by hello job. Note that we use
    // a detail level too specify that only hello "id" property of each task should be
    // populated. Using a detail level for all list operations helps toolower
    // response time from hello Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor toomonitor hello state of our tasks. In this case, we
    // will wait for all tasks tooreach hello Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached hello "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property tooensure that it did not encounter a failure
    // or return a non-zero exit code.

    // Update hello detail level toopopulate only hello task id and executionInfo
    // properties. We refresh hello tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate hello task's properties with hello latest info from hello Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.Result == TaskExecutionResult.Failure)
        {
            // A task with failure information set indicates there was a problem with hello task. It is important toonote that
            // hello task's state can be "Completed," yet still have encountered a failure.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a failure: {1}", task.Id, task.ExecutionInformation.FailureInformation.Message);
            if (task.ExecutionInformation.ExitCode != 0)
            {
                // A non-zero exit code may indicate that hello application executed by hello task encountered an error
                // during execution. As not every application returns non-zero on failure by default (e.g. robocopy),
                // your implementation of error checking may differ from this example.

                Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
            }
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within hello specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Krok 7: Stažení výstupu úkolu
![Stažení výstupu úkolu ze služby Storage][7]<br/>

Teď, když hello dokončení úlohy hello výstup z úlohy hello si můžete stáhnout ze služby Azure Storage. To lze provést pomocí volání příliš`DownloadBlobsFromContainerAsync` v *DotNetTutorial*na `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference tooa previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all hello block blobs in hello specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference toohello current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents tooa file in hello specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded too{0}", directoryPath);
}
```

> [!NOTE]
> Hello volání příliš`DownloadBlobsFromContainerAsync` v hello *DotNetTutorial* aplikace určuje, zda text hello soubory by měly být stažené tooyour `%TEMP%` složky. Myslíte, že volné toomodify to výstupní umístění.
>
>

## <a name="step-8-delete-containers"></a>Krok 8: Odstranění kontejnerů
Vzhledem k tomu, že musíte platit za data uložená ve službě Azure Storage, je vždy tooremove vhodné objekty BLOB, které již nejsou potřebné pro úlohy Batch. V DotNetTutorial `Program.cs`, to provádí pomocí tří volání toohello Pomocná metoda `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Hello metoda sama jenom získá odkaz na kontejner toohello a potom zavolá [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a>Krok 9: Odstranění hello úlohy a fondu hello
V posledním kroku hello jste výzvami toodelete hello úlohy a hello fondu, které pocházejí z aplikace DotNetTutorial hello. I když se vám neúčtují poplatky za úlohy a úlohy samotné, *účtují* se vám poplatky za výpočetní uzly. Proto doporučujeme, abyste uzly přidělovali, jen když je to potřeba. Odstraňování nepoužívaných fondů by mělo být součástí vašeho standardního procesu údržby.

Hello BatchClient na [JobOperations] [ net_joboperations] a [PoolOperations] [ net_pooloperations] mají odpovídající metody odstranění, které se volají, pokud Hello uživatel potvrdí odstranění:

```csharp
// Clean up hello resources we've created in hello Batch account if hello user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [!IMPORTANT]
> Pamatujte, že se vám účtují poplatky za výpočetní prostředky, takže odstranění nepoužívaných fondů vám ušetří náklady. Mějte také, že odstraněním fondu odstraníte všechny výpočetní uzly v tomto fondu, a že všechna data na uzlech hello neopravitelné po odstranění fondu hello.
>
>

## <a name="run-hello-dotnettutorial-sample"></a>Spustit hello *DotNetTutorial* vzorku
Když spustíte hello ukázkovou aplikaci, bude výstup konzoly hello podobné toohello následující. Během provádění, dojde k pozastavení při `Awaiting task completion, timeout in 00:30:00...` při se spustí výpočetní uzly fondu hello. Použití hello [portál Azure] [ azure_portal] toomonitor fondu, výpočetních uzlů, úlohy a úkolů během a po spuštění. Použití hello [portál Azure] [ azure_portal] nebo hello [Azure Storage Explorer] [ storage_explorers] tooview hello prostředků služby Storage (kontejnerů a objektů BLOB) které jsou Vytvoření aplikace hello.

Typická doba provádění je **přibližně 5 minut** při spuštění aplikace hello ve výchozí konfiguraci.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe toocontainer [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll toocontainer [application]...
Uploading file ..\..\taskdata1.txt toocontainer [input]...
Uploading file ..\..\taskdata2.txt toocontainer [input]...
Uploading file ..\..\taskdata3.txt toocontainer [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks toojob [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within hello specified timeout period.
Downloading all files from container [output]...
All files downloaded tooC:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a>Další kroky
Působí volné toomake změny příliš*DotNetTutorial* a *TaskApplication* výpočetní tooexperiment jiné scénáře. Zkuste například přidat prodlevu provádění příliš*TaskApplication*, například stejně jako u [Thread.Sleep][net_thread_sleep], toosimulate dlouho běžící úlohy a monitorovat je hello portálu. Zkuste přidat další úkoly nebo upravte hello počet výpočetních uzlů. Přidejte logiku toocheck pro a povolit hello použití existující doba provádění toospeed fondu (*pomocný parametr*: Podívejte se na `ArticleHelpers.cs` v hello [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projektu v [azure-batch-samples][github_samples]).

Teď, když jste obeznámeni s hello základní pracovní postup řešení Batch, je čas toodig v toohello další funkce hello služby Batch.

* Zkontrolujte hello [funkcí přehled Azure Batch](batch-api-basics.md) článek, který doporučujeme, pokud jste novou službu toohello.
* Spuštění na hello další články vývoj Batch pod **vývoje** v hello [Batch studijní][batch_learning_path].
* Podívejte se na různé implementace zpracování úlohy hello "nejčastějších N slov" pomocí Batch v hello [TopNWords] [ github_topnwords] ukázka.
* Zkontrolujte hello Batch .NET [poznámky k verzi](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) hello nejnovější změny v hello knihovně.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/vs/
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Vytvoření kontejnerů ve službě Azure Storage"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Soubory toocontainers odeslání úloh aplikace a vstupních (data)"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Vytvoření fondu Batch"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Vytvoření úlohy Batch"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Přidat toojob úlohy"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Stažení výstupu úkolu ze služby Storage"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Pracovní postup řešení Batch (úplný diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Přihlašovací údaje Batch na portálu"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Přihlašovací údaje služby Storage na portálu"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Pracovní postup řešení Batch (minimální diagram)"
