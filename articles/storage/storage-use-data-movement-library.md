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
# <a name="transfer-data-with-hello-microsoft-azure-storage-data-movement-library"></a><span data-ttu-id="66704-105">Přenos dat s hello knihovna pro přesun dat Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="66704-105">Transfer Data with hello Microsoft Azure Storage Data Movement Library</span></span>

## <a name="overview"></a><span data-ttu-id="66704-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="66704-106">Overview</span></span>
<span data-ttu-id="66704-107">Hello knihovna pro přesun dat Microsoft Azure Storage je knihovna napříč platformami s otevřeným zdrojem určená pro vysoký výkon odesílání, stahování a kopírování souborů a objektů BLOB služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="66704-107">hello Microsoft Azure Storage Data Movement Library is a cross-platform open source library that is designed for high performance uploading, downloading, and copying of Azure Storage Blobs and Files.</span></span> <span data-ttu-id="66704-108">Tato knihovna je hello přesun základních dat která pohání [AzCopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="66704-108">This library is hello core data movement framework that powers [AzCopy](storage-use-azcopy.md).</span></span> <span data-ttu-id="66704-109">Hello knihovna pro přesun dat poskytuje praktické metody, které nejsou k dispozici v našem tradiční [.NET Klientská knihovna pro úložiště Azure](storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="66704-109">hello Data Movement Library provides convenient methods that aren't available in our traditional [.NET Azure Storage Client Library](storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="66704-110">To zahrnuje hello možnost tooset hello počet paralelních operací, sledovat průběhu přenosu, snadno obnovit zrušené přenos a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="66704-110">This includes hello ability tooset hello number of parallel operations, track transfer progress, easily resume a canceled transfer, and much more.</span></span>  

<span data-ttu-id="66704-111">Tuto knihovnu používá také .NET Core, což znamená, že při vytváření aplikace .NET pro Windows, Linux a systému macOS, můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="66704-111">This library also uses .NET Core, which means you can use it when building .NET apps for Windows, Linux and macOS.</span></span> <span data-ttu-id="66704-112">toolearn Další informace o .NET Core odkazovat toohello [.NET Core dokumentaci](https://dotnet.github.io/).</span><span class="sxs-lookup"><span data-stu-id="66704-112">toolearn more about .NET Core, refer toohello [.NET Core documentation](https://dotnet.github.io/).</span></span> <span data-ttu-id="66704-113">Tato knihovna funguje i pro tradiční aplikace pro rozhraní .NET Framework pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="66704-113">This library also works for traditional .NET Framework apps for Windows.</span></span> 

<span data-ttu-id="66704-114">Tento dokument ukazuje, jak toocreate .NET Core Konzolová aplikace, která běží na systému Windows, Linux a systému macOS a provede hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="66704-114">This document demonstrates how toocreate a .NET Core console application that that runs on Windows, Linux, and macOS and performs hello following scenarios:</span></span>

- <span data-ttu-id="66704-115">Odeslání souborů a adresářů tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="66704-115">Upload files and directories tooBlob Storage.</span></span>
- <span data-ttu-id="66704-116">Při přenosu dat, definujte hello počet paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="66704-116">Define hello number of parallel operations when transferring data.</span></span>
- <span data-ttu-id="66704-117">Probíhá přenos dat sledování.</span><span class="sxs-lookup"><span data-stu-id="66704-117">Track data transfer progress.</span></span>
- <span data-ttu-id="66704-118">Přenos dat obnovení zrušeny.</span><span class="sxs-lookup"><span data-stu-id="66704-118">Resume canceled data transfer.</span></span> 
- <span data-ttu-id="66704-119">Zkopírujte soubor z adresy URL tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="66704-119">Copy file from URL tooBlob Storage.</span></span> 
- <span data-ttu-id="66704-120">Zkopírujte z úložiště objektů Blob tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="66704-120">Copy from Blob Storage tooBlob Storage.</span></span>

<span data-ttu-id="66704-121">**Co potřebujete:**</span><span class="sxs-lookup"><span data-stu-id="66704-121">**What you need:**</span></span>

* [<span data-ttu-id="66704-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="66704-122">Visual Studio Code</span></span>](https://code.visualstudio.com/)
* <span data-ttu-id="66704-123">[Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="66704-123">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

> [!NOTE]
> <span data-ttu-id="66704-124">Tato příručka předpokládá, že jste již obeznámeni s [Azure Storage](https://azure.microsoft.com/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="66704-124">This guide assumes that you are already familiar with [Azure Storage](https://azure.microsoft.com/services/storage/).</span></span> <span data-ttu-id="66704-125">Pokud ne, čtení hello [Úvod tooAzure úložiště](storage-introduction.md) dokumentace je užitečné.</span><span class="sxs-lookup"><span data-stu-id="66704-125">If not, reading hello [Introduction tooAzure Storage](storage-introduction.md) documentation is helpful.</span></span> <span data-ttu-id="66704-126">Co je nejdůležitější, budete potřebovat příliš[vytvořit účet úložiště](storage-create-storage-account.md#create-a-storage-account) pomocí toostart hello knihovna pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="66704-126">Most importantly, you need too[create a Storage account](storage-create-storage-account.md#create-a-storage-account) toostart using hello Data Movement Library.</span></span>
> 
> 

## <a name="setup"></a><span data-ttu-id="66704-127">Nastavení</span><span class="sxs-lookup"><span data-stu-id="66704-127">Setup</span></span>  

1. <span data-ttu-id="66704-128">Navštivte hello [Průvodce instalací rozhraní .NET Core](https://www.microsoft.com/net/core) tooinstall .NET Core.</span><span class="sxs-lookup"><span data-stu-id="66704-128">Visit hello [.NET Core Installation Guide](https://www.microsoft.com/net/core) tooinstall .NET Core.</span></span> <span data-ttu-id="66704-129">Když vyberete prostředí, zvolte možnost příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="66704-129">When selecting your environment, choose hello command-line option.</span></span> 
2. <span data-ttu-id="66704-130">Z příkazového řádku hello vytvořte adresář pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="66704-130">From hello command line, create a directory for your project.</span></span> <span data-ttu-id="66704-131">Přejděte do tohoto adresáře, zadejte `dotnet new` projekt konzolové toocreate C#.</span><span class="sxs-lookup"><span data-stu-id="66704-131">Navigate into this directory, then type `dotnet new` toocreate a C# console project.</span></span>
3. <span data-ttu-id="66704-132">Otevřete tento adresář v kódu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66704-132">Open this directory in Visual Studio Code.</span></span> <span data-ttu-id="66704-133">Tento krok lze rychle provést prostřednictvím hello příkazového řádku zadáním `code .`.</span><span class="sxs-lookup"><span data-stu-id="66704-133">This step can be quickly done via hello command line by typing `code .`.</span></span>  
4. <span data-ttu-id="66704-134">Nainstalujte hello [C# rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) z hello Visual Studio Code Marketplace.</span><span class="sxs-lookup"><span data-stu-id="66704-134">Install hello [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) from hello Visual Studio Code Marketplace.</span></span> <span data-ttu-id="66704-135">Restartujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="66704-135">Restart Visual Studio Code.</span></span> 
5. <span data-ttu-id="66704-136">V tomto okamžiku byste měli vidět dvě výzvy.</span><span class="sxs-lookup"><span data-stu-id="66704-136">At this point, you should see two prompts.</span></span> <span data-ttu-id="66704-137">Jeden je pro přidání "toobuild požadované prostředky a ladění."</span><span class="sxs-lookup"><span data-stu-id="66704-137">One is for adding "required assets toobuild and debug."</span></span> <span data-ttu-id="66704-138">Klikněte na tlačítko Ano..</span><span class="sxs-lookup"><span data-stu-id="66704-138">Click "yes."</span></span> <span data-ttu-id="66704-139">Další výzva je určena pro obnovení nerozpoznané závislosti.</span><span class="sxs-lookup"><span data-stu-id="66704-139">Another prompt is for restoring unresolved dependencies.</span></span> <span data-ttu-id="66704-140">Klikněte na možnost "obnovit."</span><span class="sxs-lookup"><span data-stu-id="66704-140">Click "restore."</span></span>
6. <span data-ttu-id="66704-141">Aplikace by měl nyní obsahovat `launch.json` souboru pod hello `.vscode` adresáře.</span><span class="sxs-lookup"><span data-stu-id="66704-141">Your application should now contain a `launch.json` file under hello `.vscode` directory.</span></span> <span data-ttu-id="66704-142">V tomto souboru změnit hello `externalConsole` hodnota příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="66704-142">In this file, change hello `externalConsole` value too`true`.</span></span>
7. <span data-ttu-id="66704-143">Visual Studio Code umožňuje toodebug aplikace .NET Core.</span><span class="sxs-lookup"><span data-stu-id="66704-143">Visual Studio Code allows you toodebug .NET Core applications.</span></span> <span data-ttu-id="66704-144">Stiskněte tlačítko `F5` toorun vaší aplikace a ověřte, zda je funkční vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="66704-144">Hit `F5` toorun your application and verify that your setup is working.</span></span> <span data-ttu-id="66704-145">Měli byste vidět "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="66704-145">You should see "Hello World!"</span></span> <span data-ttu-id="66704-146">Konzola na tištěných toohello.</span><span class="sxs-lookup"><span data-stu-id="66704-146">printed toohello console.</span></span> 

## <a name="add-data-movement-library-tooyour-project"></a><span data-ttu-id="66704-147">Přidání projektu tooyour knihovna pro přesun dat</span><span class="sxs-lookup"><span data-stu-id="66704-147">Add Data Movement Library tooyour project</span></span>

1. <span data-ttu-id="66704-148">Přidat hello nejnovější verzi hello knihovna pro přesun dat toohello `dependencies` část vaší `project.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="66704-148">Add hello latest version of hello Data Movement Library toohello `dependencies` section of your `project.json` file.</span></span> <span data-ttu-id="66704-149">V době hello zápis bude tato verze`"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span><span class="sxs-lookup"><span data-stu-id="66704-149">At hello time of writing, this version would be `"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span></span> 
2. <span data-ttu-id="66704-150">Přidat `"portable-net45+win8"` toohello `imports` části.</span><span class="sxs-lookup"><span data-stu-id="66704-150">Add `"portable-net45+win8"` toohello `imports` section.</span></span> 
3. <span data-ttu-id="66704-151">Na řádku by měl zobrazit toorestore projektu.</span><span class="sxs-lookup"><span data-stu-id="66704-151">A prompt should display toorestore your project.</span></span> <span data-ttu-id="66704-152">Klikněte na tlačítko "obnovení" hello.</span><span class="sxs-lookup"><span data-stu-id="66704-152">Click hello "restore" button.</span></span> <span data-ttu-id="66704-153">Můžete také obnovit projekt z příkazového řádku hello zadáním příkazu hello `dotnet restore` v kořenovém adresáři projektu hello.</span><span class="sxs-lookup"><span data-stu-id="66704-153">You can also restore your project from hello command line by typing hello command `dotnet restore` in hello root of your project directory.</span></span>

<span data-ttu-id="66704-154">Upravit `project.json`:</span><span class="sxs-lookup"><span data-stu-id="66704-154">Modify `project.json`:</span></span>

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

## <a name="set-up-hello-skeleton-of-your-application"></a><span data-ttu-id="66704-155">Nastavit hello kostru aplikace</span><span class="sxs-lookup"><span data-stu-id="66704-155">Set up hello skeleton of your application</span></span>
<span data-ttu-id="66704-156">Hello první věc, kterou provedeme nastavení hello "kostra" kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="66704-156">hello first thing we do is set up hello "skeleton" code of our application.</span></span> <span data-ttu-id="66704-157">Tento kód vyzve nám pro klíč účet a název účtu úložiště a použije tyto přihlašovací údaje toocreate `CloudStorageAccount` objektu.</span><span class="sxs-lookup"><span data-stu-id="66704-157">This code prompts us for a Storage account name and account key and uses those credentials toocreate a `CloudStorageAccount` object.</span></span> <span data-ttu-id="66704-158">Tento objekt je použité toointeract s náš účet úložiště ve všech scénářích přenosu.</span><span class="sxs-lookup"><span data-stu-id="66704-158">This object is used toointeract with our Storage account in all transfer scenarios.</span></span> <span data-ttu-id="66704-159">Hello kód také vyzve nám toochoose hello typ operace přenosu rádi bychom znali tooexecute.</span><span class="sxs-lookup"><span data-stu-id="66704-159">hello code also prompts us toochoose hello type of transfer operation we would like tooexecute.</span></span> 

<span data-ttu-id="66704-160">Upravit `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="66704-160">Modify `Program.cs`:</span></span>

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

## <a name="transfer-local-file-tooazure-blob"></a><span data-ttu-id="66704-161">Přenos místního souboru tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="66704-161">Transfer local file tooAzure Blob</span></span>
<span data-ttu-id="66704-162">Přidejte metody hello `GetSourcePath` a `GetBlob` příliš`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="66704-162">Add hello methods `GetSourcePath` and `GetBlob` too`Program.cs`:</span></span>

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

<span data-ttu-id="66704-163">Upravit hello `TransferLocalFileToAzureBlob` metoda:</span><span class="sxs-lookup"><span data-stu-id="66704-163">Modify hello `TransferLocalFileToAzureBlob` method:</span></span>

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

<span data-ttu-id="66704-164">Tento kód nám vyzve k zadání místního souboru tooa hello cestu, hello název nové nebo existující kontejner a hello název nového objektu blob.</span><span class="sxs-lookup"><span data-stu-id="66704-164">This code prompts us for hello path tooa local file, hello name of a new or existing container, and hello name of a new blob.</span></span> <span data-ttu-id="66704-165">Hello `TransferManager.UploadAsync` metoda provádí nahrávání hello na základě těchto informací.</span><span class="sxs-lookup"><span data-stu-id="66704-165">hello `TransferManager.UploadAsync` method performs hello upload using this information.</span></span> 

<span data-ttu-id="66704-166">Stiskněte tlačítko `F5` toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="66704-166">Hit `F5` toorun your application.</span></span> <span data-ttu-id="66704-167">Můžete ověřit, že hello nahrávání došlo k zobrazením účtu úložiště s hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="66704-167">You can verify that hello upload occurred by viewing your Storage account with hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="set-number-of-parallel-operations"></a><span data-ttu-id="66704-168">Sada počet paralelních operací</span><span class="sxs-lookup"><span data-stu-id="66704-168">Set number of parallel operations</span></span>
<span data-ttu-id="66704-169">Skvělé funkce nabízené sítěmi hello knihovna pro přesun dat je hello možnost tooset hello počet propustnost přenosu dat hello tooincrease paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="66704-169">A great feature offered by hello Data Movement Library is hello ability tooset hello number of parallel operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="66704-170">Ve výchozím nastavení, nastaví hello knihovna pro přesun dat hello počet paralelních operací too8 * hello počet jader na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="66704-170">By default, hello Data Movement Library sets hello number of parallel operations too8 * hello number of cores on your machine.</span></span> 

<span data-ttu-id="66704-171">Uvědomte si, že mnoho paralelních operací v prostředí s malou šířkou pásma může zahlcovat hello síťové připojení a ve skutečnosti zabránit v plně dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="66704-171">Keep in mind that many parallel operations in a low-bandwidth environment may overwhelm hello network connection and actually prevent operations from fully completing.</span></span> <span data-ttu-id="66704-172">Budete potřebovat tooexperiment s toodetermine tato nastavení co funguje nejlépe závislosti na vaší dostupnou šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="66704-172">You'll need tooexperiment with this setting toodetermine what works best based on your available network bandwidth.</span></span> 

<span data-ttu-id="66704-173">Přidejme nějaký kód, který umožňuje nám tooset hello počet paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="66704-173">Let's add some code that allows us tooset hello number of parallel operations.</span></span> <span data-ttu-id="66704-174">Můžeme také přidat kód, který krát, jak dlouho trvá pro přenos toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="66704-174">Let's also add code that times how long it takes for hello transfer toocomplete.</span></span>

<span data-ttu-id="66704-175">Přidat `SetNumberOfParallelOperations` metoda příliš`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="66704-175">Add a `SetNumberOfParallelOperations` method too`Program.cs`:</span></span>

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like toouse?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

<span data-ttu-id="66704-176">Upravit hello `ExecuteChoice` toouse metoda `SetNumberOfParallelOperations`:</span><span class="sxs-lookup"><span data-stu-id="66704-176">Modify hello `ExecuteChoice` method toouse `SetNumberOfParallelOperations`:</span></span>

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

<span data-ttu-id="66704-177">Upravit hello `TransferLocalFileToAzureBlob` metoda toouse časovač:</span><span class="sxs-lookup"><span data-stu-id="66704-177">Modify hello `TransferLocalFileToAzureBlob` method toouse a timer:</span></span>

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

## <a name="track-transfer-progress"></a><span data-ttu-id="66704-178">Sledování průběhu přenosu</span><span class="sxs-lookup"><span data-stu-id="66704-178">Track transfer progress</span></span>
<span data-ttu-id="66704-179">Zároveň budete vědět, jak dlouho trvalo pro naše data tootransfer je velmi užitečná.</span><span class="sxs-lookup"><span data-stu-id="66704-179">Knowing how long it took for our data tootransfer is great.</span></span> <span data-ttu-id="66704-180">Ale je možné toosee hello průběh naše přenos *během* operace přenosu hello by být ještě lepší.</span><span class="sxs-lookup"><span data-stu-id="66704-180">However, being able toosee hello progress of our transfer *during* hello transfer operation would be even better.</span></span> <span data-ttu-id="66704-181">tooachieve tento scénář, potřebujeme toocreate `TransferContext` objektu.</span><span class="sxs-lookup"><span data-stu-id="66704-181">tooachieve this scenario, we need toocreate a `TransferContext` object.</span></span> <span data-ttu-id="66704-182">Hello `TransferContext` objekt pochází ve dvou formách: `SingleTransferContext` a `DirectoryTransferContext`.</span><span class="sxs-lookup"><span data-stu-id="66704-182">hello `TransferContext` object comes in two forms: `SingleTransferContext` and `DirectoryTransferContext`.</span></span> <span data-ttu-id="66704-183">Hello bývalé je při přenosu jednoho souboru (což je co jsme vaše změny teď) a pozdější hello je přenos adresář soubory (které přidáváme později).</span><span class="sxs-lookup"><span data-stu-id="66704-183">hello former is for transferring a single file (which is what we're doing now) and hello latter is for transferring a directory of files (which we are adding later).</span></span>

<span data-ttu-id="66704-184">Přidejte metody hello `GetSingleTransferContext` a `GetDirectoryTransferContext` příliš`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="66704-184">Add hello methods `GetSingleTransferContext` and `GetDirectoryTransferContext` too`Program.cs`:</span></span> 

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

<span data-ttu-id="66704-185">Upravit hello `TransferLocalFileToAzureBlob` toouse metoda `GetSingleTransferContext`:</span><span class="sxs-lookup"><span data-stu-id="66704-185">Modify hello `TransferLocalFileToAzureBlob` method toouse `GetSingleTransferContext`:</span></span>

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

## <a name="resume-a-canceled-transfer"></a><span data-ttu-id="66704-186">Obnovit zrušené přenosu</span><span class="sxs-lookup"><span data-stu-id="66704-186">Resume a canceled transfer</span></span>
<span data-ttu-id="66704-187">Další pohodlný funkcí, které nabízí hello knihovna pro přesun dat je hello možnost tooresume zrušené přenosu.</span><span class="sxs-lookup"><span data-stu-id="66704-187">Another convenient feature offered by hello Data Movement Library is hello ability tooresume a canceled transfer.</span></span> <span data-ttu-id="66704-188">Přidejme nějaký kód, který umožňuje nám tootemporarily Storno hello přenos zadáním `c`a poté obnovit přenos hello později 3 sekund.</span><span class="sxs-lookup"><span data-stu-id="66704-188">Let's add some code that allows us tootemporarily cancel hello transfer by typing `c`, and then resume hello transfer 3 seconds later.</span></span>

<span data-ttu-id="66704-189">Upravit `TransferLocalFileToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="66704-189">Modify `TransferLocalFileToAzureBlob`:</span></span>

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

<span data-ttu-id="66704-190">Až se zatím naše `checkpoint` vždy byla nastavena hodnota příliš`null`.</span><span class="sxs-lookup"><span data-stu-id="66704-190">Up until now, our `checkpoint` value has always been set too`null`.</span></span> <span data-ttu-id="66704-191">Nyní pokud zrušení hello přenos, jsme načíst hello posledního kontrolního bodu naše přenos a potom používat tento nový kontrolní bod v našem kontext přenosu.</span><span class="sxs-lookup"><span data-stu-id="66704-191">Now, if we cancel hello transfer, we retrieve hello last checkpoint of our transfer, then use this new checkpoint in our transfer context.</span></span> 

## <a name="transfer-local-directory-tooazure-blob-directory"></a><span data-ttu-id="66704-192">Přenos místního adresáře tooAzure Blob adresáře</span><span class="sxs-lookup"><span data-stu-id="66704-192">Transfer local directory tooAzure Blob directory</span></span>
<span data-ttu-id="66704-193">By být neuspokojivé, pokud hello knihovna pro přesun dat může přenášet jenom jeden soubor současně.</span><span class="sxs-lookup"><span data-stu-id="66704-193">It would be disappointing if hello Data Movement Library could only transfer one file at a time.</span></span> <span data-ttu-id="66704-194">Naštěstí se nejedná o hello případ.</span><span class="sxs-lookup"><span data-stu-id="66704-194">Luckily, this is not hello case.</span></span> <span data-ttu-id="66704-195">Hello knihovna pro přesun dat poskytuje možnost tootransfer hello adresář souborů a všechny jeho podadresářů.</span><span class="sxs-lookup"><span data-stu-id="66704-195">hello Data Movement Library provides hello ability tootransfer a directory of files and all of its subdirectories.</span></span> <span data-ttu-id="66704-196">Přidejme nějaký kód, který umožňuje nám toodo právě který.</span><span class="sxs-lookup"><span data-stu-id="66704-196">Let's add some code that allows us toodo just that.</span></span>

<span data-ttu-id="66704-197">Nejprve přidejte hello metoda `GetBlobDirectory` příliš`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="66704-197">First, add hello method `GetBlobDirectory` too`Program.cs`:</span></span>

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

<span data-ttu-id="66704-198">Potom upravte `TransferLocalDirectoryToAzureBlobDirectory`:</span><span class="sxs-lookup"><span data-stu-id="66704-198">Then, modify `TransferLocalDirectoryToAzureBlobDirectory`:</span></span>

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

<span data-ttu-id="66704-199">Existuje několik rozdílů mezi tato metoda a metoda hello nahrát jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="66704-199">There are a few differences between this method and hello method for uploading a single file.</span></span> <span data-ttu-id="66704-200">Teď používáme `TransferManager.UploadDirectoryAsync` a hello `getDirectoryTransferContext` jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="66704-200">We're now using `TransferManager.UploadDirectoryAsync` and hello `getDirectoryTransferContext` method we created earlier.</span></span> <span data-ttu-id="66704-201">Kromě toho teď poskytujeme `options` hodnotu tooour nahrávání operaci, která umožňuje nám tooindicate chceme tooinclude podadresáře v našem nahrávání.</span><span class="sxs-lookup"><span data-stu-id="66704-201">In addition, we now provide an `options` value tooour upload operation, which allows us tooindicate that we want tooinclude subdirectories in our upload.</span></span> 

## <a name="copy-file-from-url-tooazure-blob"></a><span data-ttu-id="66704-202">Zkopírujte soubor z adresy URL tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="66704-202">Copy file from URL tooAzure Blob</span></span>
<span data-ttu-id="66704-203">Nyní Pojďme přidat kód, který umožňuje nám toocopy soubor z adresy URL tooan objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="66704-203">Now, let's add code that allows us toocopy a file from a URL tooan Azure Blob.</span></span> 

<span data-ttu-id="66704-204">Upravit `TransferUrlToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="66704-204">Modify `TransferUrlToAzureBlob`:</span></span>

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

<span data-ttu-id="66704-205">Jeden případ použití důležité pro tuto funkci je, když potřebujete toomove data z jiného tooAzure cloudové služby (např. AWS).</span><span class="sxs-lookup"><span data-stu-id="66704-205">One important use case for this feature is when you need toomove data from another cloud service (e.g. AWS) tooAzure.</span></span> <span data-ttu-id="66704-206">Tak dlouho, dokud máte adresu URL, která poskytuje přístup toohello prostředků, tento prostředek můžou snadno přesunout do objektů BLOB služby Azure pomocí hello `TransferManager.CopyAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="66704-206">As long as you have a URL that gives you access toohello resource, you can easily move that resource into Azure Blobs by using hello `TransferManager.CopyAsync` method.</span></span> <span data-ttu-id="66704-207">Tato metoda také zavádí nový parametr typu boolean.</span><span class="sxs-lookup"><span data-stu-id="66704-207">This method also introduces a new boolean parameter.</span></span> <span data-ttu-id="66704-208">Nastavení tohoto parametru příliš`true` označuje, že má být kopie toodo asynchronní straně serveru.</span><span class="sxs-lookup"><span data-stu-id="66704-208">Setting this parameter too`true` indicates that we want toodo an asynchronous server-side copy.</span></span> <span data-ttu-id="66704-209">Nastavení tohoto parametru příliš`false` označuje synchronní kopie - znamená hello prostředků je místní počítač stažené tooour nejdřív poté odeslány tooAzure objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="66704-209">Setting this parameter too`false` indicates a synchronous copy - meaning hello resource is downloaded tooour local machine first, then uploaded tooAzure Blob.</span></span> <span data-ttu-id="66704-210">Ale synchronní kopie je aktuálně k dispozici pouze pro kopírování z jednoho tooanother prostředků úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="66704-210">However, synchronous copy is currently only available for copying from one Azure Storage resource tooanother.</span></span> 

## <a name="transfer-azure-blob-tooazure-blob"></a><span data-ttu-id="66704-211">Přenos tooAzure objektů Blob v Azure Blob</span><span class="sxs-lookup"><span data-stu-id="66704-211">Transfer Azure Blob tooAzure Blob</span></span>
<span data-ttu-id="66704-212">Další funkcí, které jednoznačně poskytuje hello knihovna pro přesun dat je hello možnost toocopy z jednoho tooanother prostředků úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="66704-212">Another feature that's uniquely provided by hello Data Movement Library is hello ability toocopy from one Azure Storage resource tooanother.</span></span> 

<span data-ttu-id="66704-213">Upravit `TransferAzureBlobToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="66704-213">Modify `TransferAzureBlobToAzureBlob`:</span></span>

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

<span data-ttu-id="66704-214">V tomto příkladu jsme nastavený hello logického parametru `TransferManager.CopyAsync` příliš`false` tooindicate, že má být toodo synchronní kopie.</span><span class="sxs-lookup"><span data-stu-id="66704-214">In this example, we set hello boolean parameter in `TransferManager.CopyAsync` too`false` tooindicate that we want toodo a synchronous copy.</span></span> <span data-ttu-id="66704-215">To znamená, že stažené tooour místní počítač, který je nejprve hello prostředků a potom nahrán tooAzure objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="66704-215">This means that hello resource is downloaded tooour local machine first, then uploaded tooAzure Blob.</span></span> <span data-ttu-id="66704-216">možnost synchronní kopie Hello je skvělým způsobem tooensure, že má vaše operace kopírování konzistentní rychlost.</span><span class="sxs-lookup"><span data-stu-id="66704-216">hello synchronous copy option is a great way tooensure that your copy operation has a consistent speed.</span></span> <span data-ttu-id="66704-217">Rychlost hello asynchronní kopie serverové spočívá v tom, závisí na hello dostupnou šířku pásma sítě na hello serveru, který můžete kolísá.</span><span class="sxs-lookup"><span data-stu-id="66704-217">In contrast, hello speed of an asynchronous server-side copy is dependent on hello available network bandwidth on hello server, which can fluctuate.</span></span> <span data-ttu-id="66704-218">Synchronní kopie však může generovat další odchozí náklady porovnání tooasynchronous kopie.</span><span class="sxs-lookup"><span data-stu-id="66704-218">However, synchronous copy may generate additional egress cost compared tooasynchronous copy.</span></span> <span data-ttu-id="66704-219">Hello doporučený postup je toouse synchronní kopie ve virtuálním počítači Azure, který je v hello stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="66704-219">hello recommended approach is toouse synchronous copy in an Azure VM that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="conclusion"></a><span data-ttu-id="66704-220">Závěr</span><span class="sxs-lookup"><span data-stu-id="66704-220">Conclusion</span></span>
<span data-ttu-id="66704-221">Naše aplikace přesun dat je nyní dokončen.</span><span class="sxs-lookup"><span data-stu-id="66704-221">Our data movement application is now complete.</span></span> <span data-ttu-id="66704-222">[Hello úplného kódu ukázka je dostupná na Githubu](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span><span class="sxs-lookup"><span data-stu-id="66704-222">[hello full code sample is available on GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="66704-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66704-223">Next steps</span></span>
<span data-ttu-id="66704-224">V této příručce Začínáme, jsme vytvořili aplikaci, která komunikuje s úložištěm Azure a běží na systému Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="66704-224">In this getting started, we created an application that interacts with Azure Storage and runs on Windows, Linux, and macOS.</span></span> <span data-ttu-id="66704-225">Toto úvodní zaměřuje na úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="66704-225">This getting started focused on Blob Storage.</span></span> <span data-ttu-id="66704-226">Tato stejné znalosti však může být použité tooFile úložiště.</span><span class="sxs-lookup"><span data-stu-id="66704-226">However, this same knowledge can be applied tooFile Storage.</span></span> <span data-ttu-id="66704-227">toolearn víc, podívejte se na [knihovna pro přesun dat úložiště Azure referenční dokumentaci k nástroji](https://azure.github.io/azure-storage-net-data-movement).</span><span class="sxs-lookup"><span data-stu-id="66704-227">toolearn more, check out [Azure Storage Data Movement Library reference documentation](https://azure.github.io/azure-storage-net-data-movement).</span></span>

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]




