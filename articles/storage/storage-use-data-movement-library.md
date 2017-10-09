---
title: "aaaTransfer Data s hello knihovna pro přesun dat Microsoft Azure Storage | Microsoft Docs"
description: "Použijte hello knihovna pro přesun dat toomove nebo kopírování dat tooor z objektu blob a soubor obsahu. Kopírování dat tooAzure úložiště z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Vaše data tooAzure úložiště snadno migrujte."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/22/2017
ms.author: seguler
ms.openlocfilehash: 5de902d132565a8eafdc672f7a1a18e1a2db3a06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-microsoft-azure-storage-data-movement-library"></a>Přenos dat s hello knihovna pro přesun dat Microsoft Azure Storage

## <a name="overview"></a>Přehled
Hello knihovna pro přesun dat Microsoft Azure Storage je knihovna napříč platformami s otevřeným zdrojem určená pro vysoký výkon odesílání, stahování a kopírování souborů a objektů BLOB služby Azure Storage. Tato knihovna je hello přesun základních dat která pohání [AzCopy](storage-use-azcopy.md). Hello knihovna pro přesun dat poskytuje praktické metody, které nejsou k dispozici v našem tradiční [.NET Klientská knihovna pro úložiště Azure](storage-dotnet-how-to-use-blobs.md). To zahrnuje hello možnost tooset hello počet paralelních operací, sledovat průběhu přenosu, snadno obnovit zrušené přenos a mnoho dalšího.  

Tuto knihovnu používá také .NET Core, což znamená, že při vytváření aplikace .NET pro Windows, Linux a systému macOS, můžete ji použít. toolearn Další informace o .NET Core odkazovat toohello [.NET Core dokumentaci](https://dotnet.github.io/). Tato knihovna funguje i pro tradiční aplikace pro rozhraní .NET Framework pro systém Windows. 

Tento dokument ukazuje, jak toocreate .NET Core Konzolová aplikace, která běží na systému Windows, Linux a systému macOS a provede hello následující scénáře:

- Odeslání souborů a adresářů tooBlob úložiště.
- Při přenosu dat, definujte hello počet paralelních operací.
- Probíhá přenos dat sledování.
- Přenos dat obnovení zrušeny. 
- Zkopírujte soubor z adresy URL tooBlob úložiště. 
- Zkopírujte z úložiště objektů Blob tooBlob úložiště.

**Co potřebujete:**

* [Visual Studio Code](https://code.visualstudio.com/)
* [Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)

> [!NOTE]
> Tato příručka předpokládá, že jste již obeznámeni s [Azure Storage](https://azure.microsoft.com/services/storage/). Pokud ne, čtení hello [Úvod tooAzure úložiště](storage-introduction.md) dokumentace je užitečné. Co je nejdůležitější, budete potřebovat příliš[vytvořit účet úložiště](storage-create-storage-account.md#create-a-storage-account) pomocí toostart hello knihovna pro přesun dat.
> 
> 

## <a name="setup"></a>Nastavení  

1. Navštivte hello [Průvodce instalací rozhraní .NET Core](https://www.microsoft.com/net/core) tooinstall .NET Core. Když vyberete prostředí, zvolte možnost příkazového řádku hello. 
2. Z příkazového řádku hello vytvořte adresář pro váš projekt. Přejděte do tohoto adresáře, zadejte `dotnet new` projekt konzolové toocreate C#.
3. Otevřete tento adresář v kódu Visual Studio. Tento krok lze rychle provést prostřednictvím hello příkazového řádku zadáním `code .`.  
4. Nainstalujte hello [C# rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) z hello Visual Studio Code Marketplace. Restartujte Visual Studio Code. 
5. V tomto okamžiku byste měli vidět dvě výzvy. Jeden je pro přidání "toobuild požadované prostředky a ladění." Klikněte na tlačítko Ano.. Další výzva je určena pro obnovení nerozpoznané závislosti. Klikněte na možnost "obnovit."
6. Aplikace by měl nyní obsahovat `launch.json` souboru pod hello `.vscode` adresáře. V tomto souboru změnit hello `externalConsole` hodnota příliš`true`.
7. Visual Studio Code umožňuje toodebug aplikace .NET Core. Stiskněte tlačítko `F5` toorun vaší aplikace a ověřte, zda je funkční vašeho nastavení. Měli byste vidět "Hello, World!" Konzola na tištěných toohello. 

## <a name="add-data-movement-library-tooyour-project"></a>Přidání projektu tooyour knihovna pro přesun dat

1. Přidat hello nejnovější verzi hello knihovna pro přesun dat toohello `dependencies` část vaší `project.json` souboru. V době hello zápis bude tato verze`"Microsoft.Azure.Storage.DataMovement": "0.5.0"` 
2. Přidat `"portable-net45+win8"` toohello `imports` části. 
3. Na řádku by měl zobrazit toorestore projektu. Klikněte na tlačítko "obnovení" hello. Můžete také obnovit projekt z příkazového řádku hello zadáním příkazu hello `dotnet restore` v kořenovém adresáři projektu hello.

Upravit `project.json`:

    {
      "version": "1.0.0-*",
      "buildOptions": {
        "debugType": "portable",
        "emitEntryPoint": true
      },
      "dependencies": {
        "Microsoft.Azure.Storage.DataMovement": "0.5.0"
      },
      "frameworks": {
        "netcoreapp1.1": {
          "dependencies": {
            "Microsoft.NETCore.App": {
              "type": "platform",
              "version": "1.1.0"
            }
          },
          "imports": [
            "dnxcore50",
            "portable-net45+win8"
          ]
        }
      }
    }

## <a name="set-up-hello-skeleton-of-your-application"></a>Nastavit hello kostru aplikace
Hello první věc, kterou provedeme nastavení hello "kostra" kód aplikace. Tento kód vyzve nám pro klíč účet a název účtu úložiště a použije tyto přihlašovací údaje toocreate `CloudStorageAccount` objektu. Tento objekt je použité toointeract s náš účet úložiště ve všech scénářích přenosu. Hello kód také vyzve nám toochoose hello typ operace přenosu rádi bychom znali tooexecute. 

Upravit `Program.cs`:

```csharp
using System;
using System.Threading;
using System.Diagnostics;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.DataMovement;

namespace DMLibSample
{
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("Enter Storage account name:");           
            string accountName = Console.ReadLine();

            Console.WriteLine("\nEnter Storage account key:");           
            string accountKey = Console.ReadLine();

            string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + accountName + ";AccountKey=" + accountKey;
            CloudStorageAccount account = CloudStorageAccount.Parse(storageConnectionString);

            ExecuteChoice(account);
        }

        public static void ExecuteChoice(CloudStorageAccount account)
        {
            Console.WriteLine("\nWhat type of transfer would you like tooexecute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
            int choice = int.Parse(Console.ReadLine());

            if(choice == 1)
            {
                TransferLocalFileToAzureBlob(account).Wait();
            }
            else if(choice == 2)
            {
                TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
            }
            else if(choice == 3)
            {
                TransferUrlToAzureBlob(account).Wait();
            }
            else if(choice == 4)
            {
                TransferAzureBlobToAzureBlob(account).Wait();
            }
        }

        public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
        { 
            
        }

        public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
        { 
            
        }

        public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
        {

        }
    }
}
```

## <a name="transfer-local-file-tooazure-blob"></a>Přenos místního souboru tooAzure objektů Blob
Přidejte metody hello `GetSourcePath` a `GetBlob` příliš`Program.cs`:

```csharp
public static string GetSourcePath()
{
    Console.WriteLine("\nProvide path for source:");
    string sourcePath = Console.ReadLine();

    return sourcePath;
}

public static CloudBlockBlob GetBlob(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    Console.WriteLine("\nProvide name of new Blob:");
    string blobName = Console.ReadLine();
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    return blob;
}
```

Upravit hello `TransferLocalFileToAzureBlob` metoda:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    await TransferManager.UploadAsync(localFilePath, blob);
    Console.WriteLine("\nTransfer operation complete.");
    ExecuteChoice(account);
}
```

Tento kód nám vyzve k zadání místního souboru tooa hello cestu, hello název nové nebo existující kontejner a hello název nového objektu blob. Hello `TransferManager.UploadAsync` metoda provádí nahrávání hello na základě těchto informací. 

Stiskněte tlačítko `F5` toorun vaší aplikace. Můžete ověřit, že hello nahrávání došlo k zobrazením účtu úložiště s hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Sada počet paralelních operací
Skvělé funkce nabízené sítěmi hello knihovna pro přesun dat je hello možnost tooset hello počet propustnost přenosu dat hello tooincrease paralelních operací. Ve výchozím nastavení, nastaví hello knihovna pro přesun dat hello počet paralelních operací too8 * hello počet jader na váš počítač. 

Uvědomte si, že mnoho paralelních operací v prostředí s malou šířkou pásma může zahlcovat hello síťové připojení a ve skutečnosti zabránit v plně dokončení operace. Budete potřebovat tooexperiment s toodetermine tato nastavení co funguje nejlépe závislosti na vaší dostupnou šířku pásma sítě. 

Přidejme nějaký kód, který umožňuje nám tooset hello počet paralelních operací. Můžeme také přidat kód, který krát, jak dlouho trvá pro přenos toocomplete hello.

Přidat `SetNumberOfParallelOperations` metoda příliš`Program.cs`:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like toouse?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Upravit hello `ExecuteChoice` toouse metoda `SetNumberOfParallelOperations`:

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like tooexecute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
    int choice = int.Parse(Console.ReadLine());

    SetNumberOfParallelOperations();

    if(choice == 1)
    {
        TransferLocalFileToAzureBlob(account).Wait();
    }
    else if(choice == 2)
    {
        TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
    }
    else if(choice == 3)
    {
        TransferUrlToAzureBlob(account).Wait();
    }
    else if(choice == 4)
    {
        TransferAzureBlobToAzureBlob(account).Wait();
    }
}
```

Upravit hello `TransferLocalFileToAzureBlob` metoda toouse časovač:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="track-transfer-progress"></a>Sledování průběhu přenosu
Zároveň budete vědět, jak dlouho trvalo pro naše data tootransfer je velmi užitečná. Ale je možné toosee hello průběh naše přenos *během* operace přenosu hello by být ještě lepší. tooachieve tento scénář, potřebujeme toocreate `TransferContext` objektu. Hello `TransferContext` objekt pochází ve dvou formách: `SingleTransferContext` a `DirectoryTransferContext`. Hello bývalé je při přenosu jednoho souboru (což je co jsme vaše změny teď) a pozdější hello je přenos adresář soubory (které přidáváme později).

Přidejte metody hello `GetSingleTransferContext` a `GetDirectoryTransferContext` příliš`Program.cs`: 

```csharp
public static SingleTransferContext GetSingleTransferContext(TransferCheckpoint checkpoint)
{
    SingleTransferContext context = new SingleTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });
    
    return context;
}

public static DirectoryTransferContext GetDirectoryTransferContext(TransferCheckpoint checkpoint)
{
    DirectoryTransferContext context = new DirectoryTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });
    
    return context;
}
```

Upravit hello `TransferLocalFileToAzureBlob` toouse metoda `GetSingleTransferContext`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    Console.WriteLine("\nTransfer started...\n");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob, null, context);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="resume-a-canceled-transfer"></a>Obnovit zrušené přenosu
Další pohodlný funkcí, které nabízí hello knihovna pro přesun dat je hello možnost tooresume zrušené přenosu. Přidejme nějaký kód, který umožňuje nám tootemporarily Storno hello přenos zadáním `c`a poté obnovit přenos hello později 3 sekund.

Upravit `TransferLocalFileToAzureBlob`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.UploadAsync(localFilePath, blob, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadAsync(localFilePath, blob, null, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Až se zatím naše `checkpoint` vždy byla nastavena hodnota příliš`null`. Nyní pokud zrušení hello přenos, jsme načíst hello posledního kontrolního bodu naše přenos a potom používat tento nový kontrolní bod v našem kontext přenosu. 

## <a name="transfer-local-directory-tooazure-blob-directory"></a>Přenos místního adresáře tooAzure Blob adresáře
By být neuspokojivé, pokud hello knihovna pro přesun dat může přenášet jenom jeden soubor současně. Naštěstí se nejedná o hello případ. Hello knihovna pro přesun dat poskytuje možnost tootransfer hello adresář souborů a všechny jeho podadresářů. Přidejme nějaký kód, který umožňuje nám toodo právě který.

Nejprve přidejte hello metoda `GetBlobDirectory` příliš`Program.cs`:

```csharp
public static CloudBlobDirectory GetBlobDirectory(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container. This can be a new or existing Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    CloudBlobDirectory blobDirectory = container.GetDirectoryReference("");

    return blobDirectory;
}
```

Potom upravte `TransferLocalDirectoryToAzureBlobDirectory`:

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{ 
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account); 
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    UploadDirectoryOptions options = new UploadDirectoryOptions()
    {
        Recursive = true
    };

    try
    {
        task = TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetDirectoryTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Existuje několik rozdílů mezi tato metoda a metoda hello nahrát jeden soubor. Teď používáme `TransferManager.UploadDirectoryAsync` a hello `getDirectoryTransferContext` jsme vytvořili předtím. Kromě toho teď poskytujeme `options` hodnotu tooour nahrávání operaci, která umožňuje nám tooindicate chceme tooinclude podadresáře v našem nahrávání. 

## <a name="copy-file-from-url-tooazure-blob"></a>Zkopírujte soubor z adresy URL tooAzure objektů Blob
Nyní Pojďme přidat kód, který umožňuje nám toocopy soubor z adresy URL tooan objektů Blob v Azure. 

Upravit `TransferUrlToAzureBlob`:

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Jeden případ použití důležité pro tuto funkci je, když potřebujete toomove data z jiného tooAzure cloudové služby (např. AWS). Tak dlouho, dokud máte adresu URL, která poskytuje přístup toohello prostředků, tento prostředek můžou snadno přesunout do objektů BLOB služby Azure pomocí hello `TransferManager.CopyAsync` metoda. Tato metoda také zavádí nový parametr typu boolean. Nastavení tohoto parametru příliš`true` označuje, že má být kopie toodo asynchronní straně serveru. Nastavení tohoto parametru příliš`false` označuje synchronní kopie - znamená hello prostředků je místní počítač stažené tooour nejdřív poté odeslány tooAzure objektů Blob. Ale synchronní kopie je aktuálně k dispozici pouze pro kopírování z jednoho tooanother prostředků úložiště Azure. 

## <a name="transfer-azure-blob-tooazure-blob"></a>Přenos tooAzure objektů Blob v Azure Blob
Další funkcí, které jednoznačně poskytuje hello knihovna pro přesun dat je hello možnost toocopy z jednoho tooanother prostředků úložiště Azure. 

Upravit `TransferAzureBlobToAzureBlob`:

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(sourceBlob, destinationBlob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(sourceBlob, destinationBlob, false, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

V tomto příkladu jsme nastavený hello logického parametru `TransferManager.CopyAsync` příliš`false` tooindicate, že má být toodo synchronní kopie. To znamená, že stažené tooour místní počítač, který je nejprve hello prostředků a potom nahrán tooAzure objektů Blob. možnost synchronní kopie Hello je skvělým způsobem tooensure, že má vaše operace kopírování konzistentní rychlost. Rychlost hello asynchronní kopie serverové spočívá v tom, závisí na hello dostupnou šířku pásma sítě na hello serveru, který můžete kolísá. Synchronní kopie však může generovat další odchozí náklady porovnání tooasynchronous kopie. Hello doporučený postup je toouse synchronní kopie ve virtuálním počítači Azure, který je v hello stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.

## <a name="conclusion"></a>Závěr
Naše aplikace přesun dat je nyní dokončen. [Hello úplného kódu ukázka je dostupná na Githubu](https://github.com/azure-samples/storage-dotnet-data-movement-library-app). 

## <a name="next-steps"></a>Další kroky
V této příručce Začínáme, jsme vytvořili aplikaci, která komunikuje s úložištěm Azure a běží na systému Windows, Linux a systému macOS. Toto úvodní zaměřuje na úložiště objektů Blob. Tato stejné znalosti však může být použité tooFile úložiště. toolearn víc, podívejte se na [knihovna pro přesun dat úložiště Azure referenční dokumentaci k nástroji](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]




