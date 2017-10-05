---
title: "Přenos dat pomocí knihovna pro přesun dat úložiště Microsoft Azure | Microsoft Docs"
description: "Knihovna pro přesun dat pomocí přesunutí nebo zkopírování dat do nebo z objektu blob a obsahu souborů. Kopírování dat do úložiště Azure z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Snadno migrujte data do úložiště Azure."
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
ms.openlocfilehash: 2ba94e4dd931b6d385101c7dadccfa3583b5296e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a><span data-ttu-id="1efa7-105">Přenos dat pomocí knihovna pro přesun dat úložiště Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1efa7-105">Transfer Data with the Microsoft Azure Storage Data Movement Library</span></span>

## <a name="overview"></a><span data-ttu-id="1efa7-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="1efa7-106">Overview</span></span>
<span data-ttu-id="1efa7-107">Knihovna pro přesun dat aplikace Microsoft Azure Storage je knihovna napříč platformami s otevřeným zdrojem určená pro vysoký výkon odesílání, stahování a kopírování souborů a objektů BLOB služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1efa7-107">The Microsoft Azure Storage Data Movement Library is a cross-platform open source library that is designed for high performance uploading, downloading, and copying of Azure Storage Blobs and Files.</span></span> <span data-ttu-id="1efa7-108">Tato knihovna je základní framework přesun dat, která pohání [AzCopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="1efa7-108">This library is the core data movement framework that powers [AzCopy](storage-use-azcopy.md).</span></span> <span data-ttu-id="1efa7-109">Knihovna pro přesun dat poskytuje praktické metody, které nejsou k dispozici v našem tradiční [.NET Klientská knihovna pro úložiště Azure](storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="1efa7-109">The Data Movement Library provides convenient methods that aren't available in our traditional [.NET Azure Storage Client Library](storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="1efa7-110">To zahrnuje možnost nastavit počet paralelních operací, sledovat průběh přenosu, snadno obnovit zrušené přenos a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="1efa7-110">This includes the ability to set the number of parallel operations, track transfer progress, easily resume a canceled transfer, and much more.</span></span>  

<span data-ttu-id="1efa7-111">Tuto knihovnu používá také .NET Core, což znamená, že při vytváření aplikace .NET pro Windows, Linux a systému macOS, můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="1efa7-111">This library also uses .NET Core, which means you can use it when building .NET apps for Windows, Linux and macOS.</span></span> <span data-ttu-id="1efa7-112">Další informace o .NET Core, naleznete [.NET Core dokumentaci](https://dotnet.github.io/).</span><span class="sxs-lookup"><span data-stu-id="1efa7-112">To learn more about .NET Core, refer to the [.NET Core documentation](https://dotnet.github.io/).</span></span> <span data-ttu-id="1efa7-113">Tato knihovna funguje i pro tradiční aplikace pro rozhraní .NET Framework pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="1efa7-113">This library also works for traditional .NET Framework apps for Windows.</span></span> 

<span data-ttu-id="1efa7-114">Tento dokument ukazuje postup vytvoření aplikace konzoly .NET Core, který, který běží v systému Windows, Linux a systému macOS a provede následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="1efa7-114">This document demonstrates how to create a .NET Core console application that that runs on Windows, Linux, and macOS and performs the following scenarios:</span></span>

- <span data-ttu-id="1efa7-115">Nahrávání souborů a adresářů do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1efa7-115">Upload files and directories to Blob Storage.</span></span>
- <span data-ttu-id="1efa7-116">Při přenosu dat, zadejte počet paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="1efa7-116">Define the number of parallel operations when transferring data.</span></span>
- <span data-ttu-id="1efa7-117">Probíhá přenos dat sledování.</span><span class="sxs-lookup"><span data-stu-id="1efa7-117">Track data transfer progress.</span></span>
- <span data-ttu-id="1efa7-118">Přenos dat obnovení zrušeny.</span><span class="sxs-lookup"><span data-stu-id="1efa7-118">Resume canceled data transfer.</span></span> 
- <span data-ttu-id="1efa7-119">Zkopírujte soubor do úložiště objektů Blob z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1efa7-119">Copy file from URL to Blob Storage.</span></span> 
- <span data-ttu-id="1efa7-120">Zkopírujte z úložiště objektů Blob do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1efa7-120">Copy from Blob Storage to Blob Storage.</span></span>

<span data-ttu-id="1efa7-121">**Co potřebujete:**</span><span class="sxs-lookup"><span data-stu-id="1efa7-121">**What you need:**</span></span>

* [<span data-ttu-id="1efa7-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1efa7-122">Visual Studio Code</span></span>](https://code.visualstudio.com/)
* <span data-ttu-id="1efa7-123">[Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="1efa7-123">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

> [!NOTE]
> <span data-ttu-id="1efa7-124">Tato příručka předpokládá, že jste již obeznámeni s [Azure Storage](https://azure.microsoft.com/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="1efa7-124">This guide assumes that you are already familiar with [Azure Storage](https://azure.microsoft.com/services/storage/).</span></span> <span data-ttu-id="1efa7-125">Pokud ne, čtení [Úvod do Azure Storage](storage-introduction.md) dokumentace je užitečné.</span><span class="sxs-lookup"><span data-stu-id="1efa7-125">If not, reading the [Introduction to Azure Storage](storage-introduction.md) documentation is helpful.</span></span> <span data-ttu-id="1efa7-126">Co je nejdůležitější, budete muset [vytvořit účet úložiště](storage-create-storage-account.md#create-a-storage-account) začít používat knihovna pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="1efa7-126">Most importantly, you need to [create a Storage account](storage-create-storage-account.md#create-a-storage-account) to start using the Data Movement Library.</span></span>
> 
> 

## <a name="setup"></a><span data-ttu-id="1efa7-127">Nastavení</span><span class="sxs-lookup"><span data-stu-id="1efa7-127">Setup</span></span>  

1. <span data-ttu-id="1efa7-128">Přejděte [Průvodce instalací rozhraní .NET Core](https://www.microsoft.com/net/core) k instalaci .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1efa7-128">Visit the [.NET Core Installation Guide](https://www.microsoft.com/net/core) to install .NET Core.</span></span> <span data-ttu-id="1efa7-129">Když vyberete prostředí, vyberte tuto možnost příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1efa7-129">When selecting your environment, choose the command-line option.</span></span> 
2. <span data-ttu-id="1efa7-130">Z příkazového řádku vytvořte adresář pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="1efa7-130">From the command line, create a directory for your project.</span></span> <span data-ttu-id="1efa7-131">Přejděte do tohoto adresáře, zadejte `dotnet new` k vytvoření konzoly projektu C#.</span><span class="sxs-lookup"><span data-stu-id="1efa7-131">Navigate into this directory, then type `dotnet new` to create a C# console project.</span></span>
3. <span data-ttu-id="1efa7-132">Otevřete tento adresář v kódu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1efa7-132">Open this directory in Visual Studio Code.</span></span> <span data-ttu-id="1efa7-133">Tento krok lze rychle provést prostřednictvím příkazového řádku zadáním `code .`.</span><span class="sxs-lookup"><span data-stu-id="1efa7-133">This step can be quickly done via the command line by typing `code .`.</span></span>  
4. <span data-ttu-id="1efa7-134">Nainstalujte [C# rozšíření](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) z Visual Studio Marketplace kódu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-134">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) from the Visual Studio Code Marketplace.</span></span> <span data-ttu-id="1efa7-135">Restartujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1efa7-135">Restart Visual Studio Code.</span></span> 
5. <span data-ttu-id="1efa7-136">V tomto okamžiku byste měli vidět dvě výzvy.</span><span class="sxs-lookup"><span data-stu-id="1efa7-136">At this point, you should see two prompts.</span></span> <span data-ttu-id="1efa7-137">Jeden je pro přidání "požadované prostředky pro sestavení a ladění."</span><span class="sxs-lookup"><span data-stu-id="1efa7-137">One is for adding "required assets to build and debug."</span></span> <span data-ttu-id="1efa7-138">Klikněte na tlačítko Ano..</span><span class="sxs-lookup"><span data-stu-id="1efa7-138">Click "yes."</span></span> <span data-ttu-id="1efa7-139">Další výzva je určena pro obnovení nerozpoznané závislosti.</span><span class="sxs-lookup"><span data-stu-id="1efa7-139">Another prompt is for restoring unresolved dependencies.</span></span> <span data-ttu-id="1efa7-140">Klikněte na možnost "obnovit."</span><span class="sxs-lookup"><span data-stu-id="1efa7-140">Click "restore."</span></span>
6. <span data-ttu-id="1efa7-141">Aplikace by měl nyní obsahovat `launch.json` souboru pod `.vscode` adresáře.</span><span class="sxs-lookup"><span data-stu-id="1efa7-141">Your application should now contain a `launch.json` file under the `.vscode` directory.</span></span> <span data-ttu-id="1efa7-142">V tomto souboru změnit `externalConsole` hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="1efa7-142">In this file, change the `externalConsole` value to `true`.</span></span>
7. <span data-ttu-id="1efa7-143">Visual Studio Code umožňuje ladit aplikace .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1efa7-143">Visual Studio Code allows you to debug .NET Core applications.</span></span> <span data-ttu-id="1efa7-144">Stiskněte tlačítko `F5` spusťte aplikaci a ověřte, zda je funkční vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="1efa7-144">Hit `F5` to run your application and verify that your setup is working.</span></span> <span data-ttu-id="1efa7-145">Měli byste vidět "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="1efa7-145">You should see "Hello World!"</span></span> <span data-ttu-id="1efa7-146">vytištěny ke konzole.</span><span class="sxs-lookup"><span data-stu-id="1efa7-146">printed to the console.</span></span> 

## <a name="add-data-movement-library-to-your-project"></a><span data-ttu-id="1efa7-147">Přidejte knihovna pro přesun dat do projektu</span><span class="sxs-lookup"><span data-stu-id="1efa7-147">Add Data Movement Library to your project</span></span>

1. <span data-ttu-id="1efa7-148">Přidat nejnovější verzi knihovna pro přesun dat do `dependencies` část vaší `project.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="1efa7-148">Add the latest version of the Data Movement Library to the `dependencies` section of your `project.json` file.</span></span> <span data-ttu-id="1efa7-149">V době psaní bude tato verze`"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span><span class="sxs-lookup"><span data-stu-id="1efa7-149">At the time of writing, this version would be `"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span></span> 
2. <span data-ttu-id="1efa7-150">Přidat `"portable-net45+win8"` k `imports` oddílu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-150">Add `"portable-net45+win8"` to the `imports` section.</span></span> 
3. <span data-ttu-id="1efa7-151">K obnovení projektu by měl zobrazovat výzvu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-151">A prompt should display to restore your project.</span></span> <span data-ttu-id="1efa7-152">Klikněte na tlačítko "restore".</span><span class="sxs-lookup"><span data-stu-id="1efa7-152">Click the "restore" button.</span></span> <span data-ttu-id="1efa7-153">Můžete také obnovit projekt z příkazového řádku zadáním příkazu `dotnet restore` v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-153">You can also restore your project from the command line by typing the command `dotnet restore` in the root of your project directory.</span></span>

<span data-ttu-id="1efa7-154">Upravit `project.json`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-154">Modify `project.json`:</span></span>

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

## <a name="set-up-the-skeleton-of-your-application"></a><span data-ttu-id="1efa7-155">Nastavit kostru aplikace</span><span class="sxs-lookup"><span data-stu-id="1efa7-155">Set up the skeleton of your application</span></span>
<span data-ttu-id="1efa7-156">První věcí, kterou provedeme nastavení "kostra" kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="1efa7-156">The first thing we do is set up the "skeleton" code of our application.</span></span> <span data-ttu-id="1efa7-157">Tento kód vyzve nám pro klíč účet a název účtu úložiště a použije tyto přihlašovací údaje pro vytvoření `CloudStorageAccount` objektu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-157">This code prompts us for a Storage account name and account key and uses those credentials to create a `CloudStorageAccount` object.</span></span> <span data-ttu-id="1efa7-158">Tento objekt se používá k interakci se naše účet úložiště ve všech scénářích přenosu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-158">This object is used to interact with our Storage account in all transfer scenarios.</span></span> <span data-ttu-id="1efa7-159">Kód výzvy také nám vyberte typ operace přenosu, kterou jsme chtěli provést.</span><span class="sxs-lookup"><span data-stu-id="1efa7-159">The code also prompts us to choose the type of transfer operation we would like to execute.</span></span> 

<span data-ttu-id="1efa7-160">Upravit `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-160">Modify `Program.cs`:</span></span>

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
            Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
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

## <a name="transfer-local-file-to-azure-blob"></a><span data-ttu-id="1efa7-161">Přenos místní soubor do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="1efa7-161">Transfer local file to Azure Blob</span></span>
<span data-ttu-id="1efa7-162">Přidejte metody `GetSourcePath` a `GetBlob` k `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-162">Add the methods `GetSourcePath` and `GetBlob` to `Program.cs`:</span></span>

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

<span data-ttu-id="1efa7-163">Změnit `TransferLocalFileToAzureBlob` metoda:</span><span class="sxs-lookup"><span data-stu-id="1efa7-163">Modify the `TransferLocalFileToAzureBlob` method:</span></span>

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

<span data-ttu-id="1efa7-164">Tento kód nám vyzve k zadání cesty do místního souboru, název nové nebo existující kontejneru a název nového objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1efa7-164">This code prompts us for the path to a local file, the name of a new or existing container, and the name of a new blob.</span></span> <span data-ttu-id="1efa7-165">`TransferManager.UploadAsync` Metoda provádí nahrávání na základě těchto informací.</span><span class="sxs-lookup"><span data-stu-id="1efa7-165">The `TransferManager.UploadAsync` method performs the upload using this information.</span></span> 

<span data-ttu-id="1efa7-166">Stiskněte tlačítko `F5` spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1efa7-166">Hit `F5` to run your application.</span></span> <span data-ttu-id="1efa7-167">Můžete ověřit, že došlo k nahrávání zobrazením účtu úložiště s [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="1efa7-167">You can verify that the upload occurred by viewing your Storage account with the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="set-number-of-parallel-operations"></a><span data-ttu-id="1efa7-168">Sada počet paralelních operací</span><span class="sxs-lookup"><span data-stu-id="1efa7-168">Set number of parallel operations</span></span>
<span data-ttu-id="1efa7-169">Skvělé funkce, které nabízí knihovna pro přesun dat je možnost nastavit počet paralelních operací, pokud chcete zvýšit propustnost dat přenosu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-169">A great feature offered by the Data Movement Library is the ability to set the number of parallel operations to increase the data transfer throughput.</span></span> <span data-ttu-id="1efa7-170">Knihovna pro přesun dat ve výchozím nastavení, nastaví počet paralelních operací na 8 * počet jader na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="1efa7-170">By default, the Data Movement Library sets the number of parallel operations to 8 * the number of cores on your machine.</span></span> 

<span data-ttu-id="1efa7-171">Uvědomte si, že mnoho paralelních operací v prostředí s malou šířkou pásma může zahlcovat síťové připojení a ve skutečnosti zabránit v plně dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="1efa7-171">Keep in mind that many parallel operations in a low-bandwidth environment may overwhelm the network connection and actually prevent operations from fully completing.</span></span> <span data-ttu-id="1efa7-172">Budete muset Vyzkoušejte toto nastavení k určení, co funguje nejlépe závislosti na vaší dostupnou šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="1efa7-172">You'll need to experiment with this setting to determine what works best based on your available network bandwidth.</span></span> 

<span data-ttu-id="1efa7-173">Přidejme nějaký kód, který umožňuje nastavit počet paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="1efa7-173">Let's add some code that allows us to set the number of parallel operations.</span></span> <span data-ttu-id="1efa7-174">Můžeme také přidat kód, který krát, jak dlouho trvá pro přenos do dokončení.</span><span class="sxs-lookup"><span data-stu-id="1efa7-174">Let's also add code that times how long it takes for the transfer to complete.</span></span>

<span data-ttu-id="1efa7-175">Přidat `SetNumberOfParallelOperations` metodu `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-175">Add a `SetNumberOfParallelOperations` method to `Program.cs`:</span></span>

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

<span data-ttu-id="1efa7-176">Změnit `ExecuteChoice` metodu použít `SetNumberOfParallelOperations`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-176">Modify the `ExecuteChoice` method to use `SetNumberOfParallelOperations`:</span></span>

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
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

<span data-ttu-id="1efa7-177">Změnit `TransferLocalFileToAzureBlob` metodu použít časovač:</span><span class="sxs-lookup"><span data-stu-id="1efa7-177">Modify the `TransferLocalFileToAzureBlob` method to use a timer:</span></span>

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

## <a name="track-transfer-progress"></a><span data-ttu-id="1efa7-178">Sledování průběhu přenosu</span><span class="sxs-lookup"><span data-stu-id="1efa7-178">Track transfer progress</span></span>
<span data-ttu-id="1efa7-179">Zároveň budete vědět, jak dlouho trvalo pro naše data k přenosu je velmi užitečná.</span><span class="sxs-lookup"><span data-stu-id="1efa7-179">Knowing how long it took for our data to transfer is great.</span></span> <span data-ttu-id="1efa7-180">Však bude možné sledovat průběh naše přenos *během* operace přenosu by být ještě lepší.</span><span class="sxs-lookup"><span data-stu-id="1efa7-180">However, being able to see the progress of our transfer *during* the transfer operation would be even better.</span></span> <span data-ttu-id="1efa7-181">K dosažení tohoto scénáře, je nutné vytvořit `TransferContext` objektu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-181">To achieve this scenario, we need to create a `TransferContext` object.</span></span> <span data-ttu-id="1efa7-182">`TransferContext` Objekt pochází ve dvou formách: `SingleTransferContext` a `DirectoryTransferContext`.</span><span class="sxs-lookup"><span data-stu-id="1efa7-182">The `TransferContext` object comes in two forms: `SingleTransferContext` and `DirectoryTransferContext`.</span></span> <span data-ttu-id="1efa7-183">První je při přenosu jednoho souboru (což je co jsme vaše změny teď) a je pro přenos adresář soubory (které přidáváme později).</span><span class="sxs-lookup"><span data-stu-id="1efa7-183">The former is for transferring a single file (which is what we're doing now) and the latter is for transferring a directory of files (which we are adding later).</span></span>

<span data-ttu-id="1efa7-184">Přidejte metody `GetSingleTransferContext` a `GetDirectoryTransferContext` k `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-184">Add the methods `GetSingleTransferContext` and `GetDirectoryTransferContext` to `Program.cs`:</span></span> 

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

<span data-ttu-id="1efa7-185">Změnit `TransferLocalFileToAzureBlob` metodu použít `GetSingleTransferContext`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-185">Modify the `TransferLocalFileToAzureBlob` method to use `GetSingleTransferContext`:</span></span>

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

## <a name="resume-a-canceled-transfer"></a><span data-ttu-id="1efa7-186">Obnovit zrušené přenosu</span><span class="sxs-lookup"><span data-stu-id="1efa7-186">Resume a canceled transfer</span></span>
<span data-ttu-id="1efa7-187">Další pohodlný funkcí, které nabízí knihovna pro přesun dat je možnost obnovit zrušené přenosu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-187">Another convenient feature offered by the Data Movement Library is the ability to resume a canceled transfer.</span></span> <span data-ttu-id="1efa7-188">Přidejme nějaký kód, který umožňuje dočasně zrušit přenos zadáním `c`a poté obnovit přenos 3 sekund později.</span><span class="sxs-lookup"><span data-stu-id="1efa7-188">Let's add some code that allows us to temporarily cancel the transfer by typing `c`, and then resume the transfer 3 seconds later.</span></span>

<span data-ttu-id="1efa7-189">Upravit `TransferLocalFileToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-189">Modify `TransferLocalFileToAzureBlob`:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="1efa7-190">Až se zatím naše `checkpoint` vždy byla nastavena hodnota `null`.</span><span class="sxs-lookup"><span data-stu-id="1efa7-190">Up until now, our `checkpoint` value has always been set to `null`.</span></span> <span data-ttu-id="1efa7-191">Nyní pokud zrušení přenos, jsme načíst do posledního kontrolního bodu naše přenos a potom používat tento nový kontrolní bod v našem kontext přenosu.</span><span class="sxs-lookup"><span data-stu-id="1efa7-191">Now, if we cancel the transfer, we retrieve the last checkpoint of our transfer, then use this new checkpoint in our transfer context.</span></span> 

## <a name="transfer-local-directory-to-azure-blob-directory"></a><span data-ttu-id="1efa7-192">Přenos místního adresáře do Azure Blob adresáře</span><span class="sxs-lookup"><span data-stu-id="1efa7-192">Transfer local directory to Azure Blob directory</span></span>
<span data-ttu-id="1efa7-193">By být neuspokojivé, pokud knihovna pro přesun dat může přenášet jenom jeden soubor současně.</span><span class="sxs-lookup"><span data-stu-id="1efa7-193">It would be disappointing if the Data Movement Library could only transfer one file at a time.</span></span> <span data-ttu-id="1efa7-194">Naštěstí to tak není.</span><span class="sxs-lookup"><span data-stu-id="1efa7-194">Luckily, this is not the case.</span></span> <span data-ttu-id="1efa7-195">Knihovna pro přesun dat umožňuje přenos souborů a všech jeho podadresářích adresáře.</span><span class="sxs-lookup"><span data-stu-id="1efa7-195">The Data Movement Library provides the ability to transfer a directory of files and all of its subdirectories.</span></span> <span data-ttu-id="1efa7-196">Přidejme nějaký kód, který umožňuje nám takový postup.</span><span class="sxs-lookup"><span data-stu-id="1efa7-196">Let's add some code that allows us to do just that.</span></span>

<span data-ttu-id="1efa7-197">Nejprve přidejte metodu `GetBlobDirectory` k `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-197">First, add the method `GetBlobDirectory` to `Program.cs`:</span></span>

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

<span data-ttu-id="1efa7-198">Potom upravte `TransferLocalDirectoryToAzureBlobDirectory`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-198">Then, modify `TransferLocalDirectoryToAzureBlobDirectory`:</span></span>

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{ 
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account); 
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="1efa7-199">Existuje několik rozdílů mezi touto metodou a metodu pro nahrávání jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="1efa7-199">There are a few differences between this method and the method for uploading a single file.</span></span> <span data-ttu-id="1efa7-200">Teď používáme `TransferManager.UploadDirectoryAsync` a `getDirectoryTransferContext` jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="1efa7-200">We're now using `TransferManager.UploadDirectoryAsync` and the `getDirectoryTransferContext` method we created earlier.</span></span> <span data-ttu-id="1efa7-201">Kromě toho teď poskytujeme `options` hodnotu naše operace nahrávání, která umožňuje znamenat, že chceme zahrnout do našich nahrávání podadresáře.</span><span class="sxs-lookup"><span data-stu-id="1efa7-201">In addition, we now provide an `options` value to our upload operation, which allows us to indicate that we want to include subdirectories in our upload.</span></span> 

## <a name="copy-file-from-url-to-azure-blob"></a><span data-ttu-id="1efa7-202">Zkopírujte soubor do objektu Blob Azure z adresy URL</span><span class="sxs-lookup"><span data-stu-id="1efa7-202">Copy file from URL to Azure Blob</span></span>
<span data-ttu-id="1efa7-203">Nyní Pojďme přidat kód, který umožňuje zkopírovat soubor z adresy URL do objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="1efa7-203">Now, let's add code that allows us to copy a file from a URL to an Azure Blob.</span></span> 

<span data-ttu-id="1efa7-204">Upravit `TransferUrlToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-204">Modify `TransferUrlToAzureBlob`:</span></span>

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="1efa7-205">Jeden případ použití důležité pro tuto funkci je, když potřebujete přesunout data z jiné cloudové služby (např. AWS) do Azure.</span><span class="sxs-lookup"><span data-stu-id="1efa7-205">One important use case for this feature is when you need to move data from another cloud service (e.g. AWS) to Azure.</span></span> <span data-ttu-id="1efa7-206">Tak dlouho, dokud máte adresu URL, která umožňuje přístup k prostředku, můžete snadno přesunutí prostředku do objektů BLOB služby Azure pomocí `TransferManager.CopyAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="1efa7-206">As long as you have a URL that gives you access to the resource, you can easily move that resource into Azure Blobs by using the `TransferManager.CopyAsync` method.</span></span> <span data-ttu-id="1efa7-207">Tato metoda také zavádí nový parametr typu boolean.</span><span class="sxs-lookup"><span data-stu-id="1efa7-207">This method also introduces a new boolean parameter.</span></span> <span data-ttu-id="1efa7-208">Nastavení tohoto parametru na `true` označuje, že chceme provést asynchronní kopie na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1efa7-208">Setting this parameter to `true` indicates that we want to do an asynchronous server-side copy.</span></span> <span data-ttu-id="1efa7-209">Nastavení tohoto parametru na `false` označuje synchronní kopie – tj. prostředek se nejprve stáhne do našich v místním počítači, pak je nahrán do objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="1efa7-209">Setting this parameter to `false` indicates a synchronous copy - meaning the resource is downloaded to our local machine first, then uploaded to Azure Blob.</span></span> <span data-ttu-id="1efa7-210">Však synchronní kopie je nyní k dispozici pouze pro kopírování z jednoho prostředku Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1efa7-210">However, synchronous copy is currently only available for copying from one Azure Storage resource to another.</span></span> 

## <a name="transfer-azure-blob-to-azure-blob"></a><span data-ttu-id="1efa7-211">Přenos objektů Blob Azure do Azure Blob</span><span class="sxs-lookup"><span data-stu-id="1efa7-211">Transfer Azure Blob to Azure Blob</span></span>
<span data-ttu-id="1efa7-212">Další funkcí, které jednoznačně poskytuje knihovna pro přesun dat je možnost Kopírovat z jednoho prostředku Azure Storage do jiného.</span><span class="sxs-lookup"><span data-stu-id="1efa7-212">Another feature that's uniquely provided by the Data Movement Library is the ability to copy from one Azure Storage resource to another.</span></span> 

<span data-ttu-id="1efa7-213">Upravit `TransferAzureBlobToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="1efa7-213">Modify `TransferAzureBlobToAzureBlob`:</span></span>

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="1efa7-214">V tomto příkladu jsme nastavený logického parametru `TransferManager.CopyAsync` k `false` indikující, že nám chcete synchronní kopie.</span><span class="sxs-lookup"><span data-stu-id="1efa7-214">In this example, we set the boolean parameter in `TransferManager.CopyAsync` to `false` to indicate that we want to do a synchronous copy.</span></span> <span data-ttu-id="1efa7-215">To znamená, že je prostředek nejprve stáhne do našich v místním počítači a potom nahrán do objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="1efa7-215">This means that the resource is downloaded to our local machine first, then uploaded to Azure Blob.</span></span> <span data-ttu-id="1efa7-216">Možnost synchronní kopie je skvělým způsobem, jak zajistěte, aby vaše operace kopírování byla konzistentní rychlost.</span><span class="sxs-lookup"><span data-stu-id="1efa7-216">The synchronous copy option is a great way to ensure that your copy operation has a consistent speed.</span></span> <span data-ttu-id="1efa7-217">Rychlost asynchronní kopie serverové spočívá v tom, závisí na dostupnou šířku pásma sítě na serveru, který můžete kolísá.</span><span class="sxs-lookup"><span data-stu-id="1efa7-217">In contrast, the speed of an asynchronous server-side copy is dependent on the available network bandwidth on the server, which can fluctuate.</span></span> <span data-ttu-id="1efa7-218">Synchronní kopie však může generovat další odchozí nákladů ve srovnání s asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="1efa7-218">However, synchronous copy may generate additional egress cost compared to asynchronous copy.</span></span> <span data-ttu-id="1efa7-219">Doporučený postup je použít synchronní kopie ve virtuálním počítači Azure, který je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="1efa7-219">The recommended approach is to use synchronous copy in an Azure VM that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="conclusion"></a><span data-ttu-id="1efa7-220">Závěr</span><span class="sxs-lookup"><span data-stu-id="1efa7-220">Conclusion</span></span>
<span data-ttu-id="1efa7-221">Naše aplikace přesun dat je nyní dokončen.</span><span class="sxs-lookup"><span data-stu-id="1efa7-221">Our data movement application is now complete.</span></span> <span data-ttu-id="1efa7-222">[Ukázka úplného kódu je dostupná na Githubu](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span><span class="sxs-lookup"><span data-stu-id="1efa7-222">[The full code sample is available on GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1efa7-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1efa7-223">Next steps</span></span>
<span data-ttu-id="1efa7-224">V této příručce Začínáme, jsme vytvořili aplikaci, která komunikuje s úložištěm Azure a běží na systému Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="1efa7-224">In this getting started, we created an application that interacts with Azure Storage and runs on Windows, Linux, and macOS.</span></span> <span data-ttu-id="1efa7-225">Toto úvodní zaměřuje na úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1efa7-225">This getting started focused on Blob Storage.</span></span> <span data-ttu-id="1efa7-226">Tato stejné znalosti však lze použít k úložišti souborů.</span><span class="sxs-lookup"><span data-stu-id="1efa7-226">However, this same knowledge can be applied to File Storage.</span></span> <span data-ttu-id="1efa7-227">Další informace, podívejte se na [knihovna pro přesun dat úložiště Azure referenční dokumentaci k nástroji](https://azure.github.io/azure-storage-net-data-movement).</span><span class="sxs-lookup"><span data-stu-id="1efa7-227">To learn more, check out [Azure Storage Data Movement Library reference documentation](https://azure.github.io/azure-storage-net-data-movement).</span></span>

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]




