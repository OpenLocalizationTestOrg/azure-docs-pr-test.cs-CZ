---
title: aaaDevelop pro Azure File storage s .NET | Microsoft Docs
description: "Zjistěte, jak toodevelop .NET aplikací a služeb, které používají Azure File storage toostore souborová data."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 79855f178111483edea13014b8eeecc3376dd4e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a><span data-ttu-id="0ccab-103">Vývoj pro službu Azure File Storage pomocí .NET</span><span class="sxs-lookup"><span data-stu-id="0ccab-103">Develop for Azure File storage with .NET</span></span> 
> [!NOTE]
> <span data-ttu-id="0ccab-104">Tento článek ukazuje, jak toomanage Azure File storage s kód .NET.</span><span class="sxs-lookup"><span data-stu-id="0ccab-104">This article shows how toomanage Azure File storage with .NET code.</span></span> <span data-ttu-id="0ccab-105">toolearn Další informace o Azure File storage najdete v tématu hello [Úvod tooAzure úložiště File](storage-files-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0ccab-105">toolearn more about Azure File storage, please see hello [Introduction tooAzure File storage](storage-files-introduction.md).</span></span>
>

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="0ccab-106">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="0ccab-106">About this tutorial</span></span>
<span data-ttu-id="0ccab-107">V tomto kurzu se ukazují hello základy používání služby, které používají Azure File storage toostore souborová data nebo toodevelop aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="0ccab-107">This tutorial will demonstrate hello basics of using .NET toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="0ccab-108">V tomto kurzu bude vytvoření jednoduché konzolové aplikace a zobrazit jak tooperform základní operace s .NET a Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="0ccab-108">In this tutorial, we will create a simple console application and show how tooperform basic actions with .NET and Azure File storage:</span></span>

* <span data-ttu-id="0ccab-109">Získat hello obsah souboru</span><span class="sxs-lookup"><span data-stu-id="0ccab-109">Get hello contents of a file</span></span>
* <span data-ttu-id="0ccab-110">Nastavit hello kvótu (maximální velikost) pro sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-110">Set hello quota (maximum size) for hello file share.</span></span>
* <span data-ttu-id="0ccab-111">Vytvořte sdílený přístupový podpis (SAS klíč) pro soubor, který používá definovaný ve sdílené složce hello zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="0ccab-111">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on hello share.</span></span>
* <span data-ttu-id="0ccab-112">Zkopírujte soubor tooanother souboru hello stejný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-112">Copy a file tooanother file in hello same storage account.</span></span>
* <span data-ttu-id="0ccab-113">Zkopírujte soubor tooa objekt blob v hello stejný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-113">Copy a file tooa blob in hello same storage account.</span></span>
* <span data-ttu-id="0ccab-114">Použijte Azure Storage Metrics pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0ccab-114">Use Azure Storage Metrics for troubleshooting</span></span>

> [!Note]  
> <span data-ttu-id="0ccab-115">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure sdílenou složku pomocí hello standardní System.IO třídy pro vstupně-výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="0ccab-115">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard System.IO classes for File I/O.</span></span> <span data-ttu-id="0ccab-116">Tento článek popisuje, jak toowrite aplikace, které používají hello .NET SDK úložiště Azure, který používá hello [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.</span><span class="sxs-lookup"><span data-stu-id="0ccab-116">This article will describe how toowrite applications that use hello Azure Storage .NET SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span> 


## <a name="create-hello-console-application-and-obtain-hello-assembly"></a><span data-ttu-id="0ccab-117">Vytvoření konzolové aplikace hello a získání sestavení hello</span><span class="sxs-lookup"><span data-stu-id="0ccab-117">Create hello console application and obtain hello assembly</span></span>
<span data-ttu-id="0ccab-118">V sadě Visual Studio vytvořte novou konzolovou aplikaci pro Windows.</span><span class="sxs-lookup"><span data-stu-id="0ccab-118">In Visual Studio, create a new Windows console application.</span></span> <span data-ttu-id="0ccab-119">Hello následující kroky vám ukážou, jak toocreate konzolovou aplikaci v sadě Visual Studio 2017, ale hello kroky jsou podobné v jiných verzích sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ccab-119">hello following steps show you how toocreate a console application in Visual Studio 2017, however, hello steps are similar in other versions of Visual Studio.</span></span>

1. <span data-ttu-id="0ccab-120">Vyberte **Soubor**  >  **Nový**  >  **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0ccab-120">Select **File** > **New** > **Project**</span></span>
2. <span data-ttu-id="0ccab-121">Vyberte **Instalováno** > **Šablony** > **Visual C#** > **Klasická plocha Windows**.</span><span class="sxs-lookup"><span data-stu-id="0ccab-121">Select **Installed** > **Templates** > **Visual C#** > **Windows Classic Desktop**</span></span>
3. <span data-ttu-id="0ccab-122">Vyberte **Aplikace konzoly (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="0ccab-122">Select **Console App (.NET Framework)**</span></span>
4. <span data-ttu-id="0ccab-123">Zadejte název pro vaši aplikaci ve hello **název:** pole</span><span class="sxs-lookup"><span data-stu-id="0ccab-123">Enter a name for your application in hello **Name:** field</span></span>
5. <span data-ttu-id="0ccab-124">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ccab-124">Select **OK**</span></span>

<span data-ttu-id="0ccab-125">Všechny příklady kódu v tomto kurzu lze přidat toohello `Main()` metoda konzolové aplikace `Program.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="0ccab-125">All code examples in this tutorial can be added toohello `Main()` method of your console application's `Program.cs` file.</span></span>

<span data-ttu-id="0ccab-126">Hello Klientská knihovna pro úložiště Azure můžete použít v libovolného typu aplikace .NET, včetně Azure cloud service nebo webovou aplikaci a stolní počítače a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ccab-126">You can use hello Azure Storage Client Library in any type of .NET application, including an Azure cloud service or web app, and desktop and mobile applications.</span></span> <span data-ttu-id="0ccab-127">V této příručce použijeme konzolovou aplikaci kvůli zjednodušení.</span><span class="sxs-lookup"><span data-stu-id="0ccab-127">In this guide, we use a console application for simplicity.</span></span>

## <a name="use-nuget-tooinstall-hello-required-packages"></a><span data-ttu-id="0ccab-128">Použití NuGet tooinstall hello požadované balíčky</span><span class="sxs-lookup"><span data-stu-id="0ccab-128">Use NuGet tooinstall hello required packages</span></span>
<span data-ttu-id="0ccab-129">Existují dva balíčky, které potřebujete tooreference ve vašem projektu toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="0ccab-129">There are two packages you need tooreference in your project toocomplete this tutorial:</span></span>

* <span data-ttu-id="0ccab-130">[Microsoft Azure Storage Client Library pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Tento balíček zajišťuje programový přístup toodata prostředky ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-130">[Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): This package provides programmatic access toodata resources in your storage account.</span></span>
* <span data-ttu-id="0ccab-131">[Microsoft Azure Configuration Manager library for .NET:](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) Tento balíček poskytuje třídu pro potřeby analýzy připojovacího řetězce v konfiguračním souboru bez ohledu na to, kde je aplikace spuštěná.</span><span class="sxs-lookup"><span data-stu-id="0ccab-131">[Microsoft Azure Configuration Manager library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): This package provides a class for parsing a connection string in a configuration file, regardless of where your application is running.</span></span>

<span data-ttu-id="0ccab-132">Můžete vytvořit NuGet tooobtain oba balíčky.</span><span class="sxs-lookup"><span data-stu-id="0ccab-132">You can use NuGet tooobtain both packages.</span></span> <span data-ttu-id="0ccab-133">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="0ccab-133">Follow these steps:</span></span>

1. <span data-ttu-id="0ccab-134">Klikněte v **Průzkumníku řešení** pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0ccab-134">Right-click your project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="0ccab-135">Vyhledejte online text "WindowsAzure.Storage" a klikněte na tlačítko **nainstalovat** tooinstall hello Klientská knihovna pro úložiště a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="0ccab-135">Search online for "WindowsAzure.Storage" and click **Install** tooinstall hello Storage Client Library and its dependencies.</span></span>
3. <span data-ttu-id="0ccab-136">Online hledání "WindowsAzure.ConfigurationManager" a klikněte na tlačítko **nainstalovat** tooinstall hello Azure Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0ccab-136">Search online for "WindowsAzure.ConfigurationManager" and click **Install** tooinstall hello Azure Configuration Manager.</span></span>

## <a name="save-your-storage-account-credentials-toohello-appconfig-file"></a><span data-ttu-id="0ccab-137">Uložte soubor app.config toohello přihlašovací údaje účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="0ccab-137">Save your storage account credentials toohello app.config file</span></span>
<span data-ttu-id="0ccab-138">Dál uložte svoje přihlašovací údaje do souboru app.config vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="0ccab-138">Next, save your credentials in your project's app.config file.</span></span> <span data-ttu-id="0ccab-139">Upravte soubor app.config hello tak, aby se podobné toohello následující ukázka, nahraďte `myaccount` s názvem svého účtu úložiště a `mykey` nahraďte svým klíčem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-139">Edit hello app.config file so that it appears similar toohello following example, replacing `myaccount` with your storage account name, and `mykey` with your storage account key.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> nejnovější verze emulátoru úložiště Azure hello Hello nepodporuje Azure File storage. <span data-ttu-id="0ccab-141">Připojovací řetězec musí být účet úložiště Azure v cloudu toowork hello s Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="0ccab-141">Your connection string must target an Azure Storage Account in hello cloud toowork with Azure File storage.</span></span>

## <a name="add-using-directives"></a><span data-ttu-id="0ccab-142">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="0ccab-142">Add using directives</span></span>
<span data-ttu-id="0ccab-143">Otevřete hello `Program.cs` soubor v Průzkumníku řešení a přidejte následující hello pomocí direktivy toohello horní části souboru hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-143">Open hello `Program.cs` file from Solution Explorer, and add hello following using directives toohello top of hello file.</span></span>

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-hello-file-share-programmatically"></a><span data-ttu-id="0ccab-144">Sdílení souborů hello přístup prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="0ccab-144">Access hello file share programmatically</span></span>
<span data-ttu-id="0ccab-145">Dál přidejte následující kód toohello hello `Main()` – metoda (po výše uvedeném kódu hello) tooretrieve hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="0ccab-145">Next, add hello following code toohello `Main()` method (after hello code shown above) tooretrieve hello connection string.</span></span> <span data-ttu-id="0ccab-146">Tento kód získá odkaz na soubor toohello, které jsme vytvořili předtím a výstupy okna konzoly toohello jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="0ccab-146">This code gets a reference toohello file we created earlier and outputs its contents toohello console window.</span></span>

```csharp
// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello file exists.
        if (file.Exists())
        {
            // Write hello contents of hello file toohello console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

<span data-ttu-id="0ccab-147">Spusťte výstup hello hello konzoly aplikace toosee.</span><span class="sxs-lookup"><span data-stu-id="0ccab-147">Run hello console application toosee hello output.</span></span>

## <a name="set-hello-maximum-size-for-a-file-share"></a><span data-ttu-id="0ccab-148">Nastavit maximální velikost hello sdílené složky</span><span class="sxs-lookup"><span data-stu-id="0ccab-148">Set hello maximum size for a file share</span></span>
<span data-ttu-id="0ccab-149">Od verze 5.x hello Klientská knihovna pro úložiště Azure, můžete nastavit sady hello kvóty (nebo maximální velikost) pro sdílenou složku, v gigabajtech.</span><span class="sxs-lookup"><span data-stu-id="0ccab-149">Beginning with version 5.x of hello Azure Storage Client Library, you can set set hello quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="0ccab-150">Můžete také zkontrolovat toosee množství dat, které jsou aktuálně uložené ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-150">You can also check toosee how much data is currently stored on hello share.</span></span>

<span data-ttu-id="0ccab-151">Pomocí nastavení hello kvótu sdílené složky můžete omezit hello celková velikost souborů hello uložené ve sdílené složce hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-151">By setting hello quota for a share, you can limit hello total size of hello files stored on hello share.</span></span> <span data-ttu-id="0ccab-152">Pokud celková velikost souborů ve sdílené složce hello hello překročí kvótu hello nastavte ve sdílené složce hello a pak klienti budou nelze tooincrease hello velikost existující soubory nebo vytvořit nové soubory, pokud tyto soubory jsou prázdné.</span><span class="sxs-lookup"><span data-stu-id="0ccab-152">If hello total size of files on hello share exceeds hello quota set on hello share, then clients will be unable tooincrease hello size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="0ccab-153">Následující příklad Hello ukazuje, jak toocheck hello aktuální využití sdílené složky a jak tooset hello kvótu pro sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-153">hello example below shows how toocheck hello current usage for a share and how tooset hello quota for hello share.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Check current usage stats for hello share.
    // Note that hello ShareStats object is part of hello protocol layer for hello File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify hello maximum size of hello share, in GB.
    // This line sets hello quota toobe 10 GB greater than hello current usage of hello share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check hello quota for hello share. Call FetchAttributes() toopopulate hello share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="0ccab-154">Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku</span><span class="sxs-lookup"><span data-stu-id="0ccab-154">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="0ccab-155">Od verze 5.x hello Klientská knihovna pro úložiště Azure, můžete vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo pro konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="0ccab-155">Beginning with version 5.x of hello Azure Storage Client Library, you can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="0ccab-156">Můžete také vytvořit zásady sdíleného přístupu na toomanage sdílený přístup ke sdílení souborů signatur.</span><span class="sxs-lookup"><span data-stu-id="0ccab-156">You can also create a shared access policy on a file share toomanage shared access signatures.</span></span> <span data-ttu-id="0ccab-157">Vytvoření zásady sdíleného přístupu se doporučuje, protože nabízí způsob odvolání hello SAS, pokud by měl být dojde k ohrožení.</span><span class="sxs-lookup"><span data-stu-id="0ccab-157">Creating a shared access policy is recommended, as it provides a means of revoking hello SAS if it should be compromised.</span></span>

<span data-ttu-id="0ccab-158">Hello následující ukázka vytvoří zásada sdíleného přístupu ve sdílené složce a pak použije, že zásady omezení tooprovide hello pro SAS na souboru ve hello sdílet.</span><span class="sxs-lookup"><span data-stu-id="0ccab-158">hello following example creates a shared access policy on a share, and then uses that policy tooprovide hello constraints for a SAS on a file in hello share.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for hello share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add hello shared access policy toohello share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in hello share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

<span data-ttu-id="0ccab-159">Další informace o vytváření a používání sdílených přístupových podpisů najdete v tématech [Použití sdílených přístupových podpisů (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) a [Vytvoření a použití SAS s objekty blob Azure](../blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="0ccab-159">For more information about creating and using shared access signatures, see [Using Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) and [Create and use a SAS with Azure Blobs](../blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span>

## <a name="copy-files"></a><span data-ttu-id="0ccab-160">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="0ccab-160">Copy files</span></span>
<span data-ttu-id="0ccab-161">Od verze 5.x hello Klientská knihovna pro úložiště Azure, můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0ccab-161">Beginning with version 5.x of hello Azure Storage Client Library, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="0ccab-162">V dalších částech hello, ukážeme, jak se tooperform tyto zkopírovat operations prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="0ccab-162">In hello next sections, we demonstrate how tooperform these copy operations programmatically.</span></span>

<span data-ttu-id="0ccab-163">Můžete také použít AzCopy toocopy jeden soubor tooanother nebo toocopy naopak objektů blob tooa soubor nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="0ccab-163">You can also use AzCopy toocopy one file tooanother or toocopy a blob tooa file or vice versa.</span></span> <span data-ttu-id="0ccab-164">V tématu [přenos dat pomocí příkazového řádku Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0ccab-164">See [Transfer data with hello AzCopy Command-Line Utility](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="0ccab-165">Pokud kopírujete objekt blob tooa soubor nebo soubor tooa objekt blob, musíte použít sdílený přístupový podpis (SAS) tooauthenticate hello zdrojový objekt, i když kopírujete v rámci hello stejný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-165">If you are copying a blob tooa file, or a file tooa blob, you must use a shared access signature (SAS) tooauthenticate hello source object, even if you are copying within hello same storage account.</span></span>
> 
> 

<span data-ttu-id="0ccab-166">**Zkopírujte soubor tooanother souboru** hello následujícím příkladu se zkopíruje soubor tooanother souboru v hello téže sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="0ccab-166">**Copy a file tooanother file** hello following example copies a file tooanother file in hello same share.</span></span> <span data-ttu-id="0ccab-167">Protože tato operace kopírování kopíruje soubory v hello stejný účet úložiště, můžete použít sdílený klíč ověřování tooperform hello kopírování.</span><span class="sxs-lookup"><span data-stu-id="0ccab-167">Because this copy operation copies between files in hello same storage account, you can use Shared Key authentication tooperform hello copy.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference toohello destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start hello copy operation.
            destFile.StartCopy(sourceFile);

            // Write hello contents of hello destination file toohello console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

<span data-ttu-id="0ccab-168">**Zkopírujte soubor tooa objekt blob** hello následující příklad vytvoří soubor a zkopíruje ho tooa objektů blob v rámci hello stejný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-168">**Copy a file tooa blob** hello following example creates a file and copies it tooa blob within hello same storage account.</span></span> <span data-ttu-id="0ccab-169">Hello příklad vytvoří SAS pro hello zdrojový soubor, který služba hello používá tooauthenticate přístup toohello zdrojového souboru během operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-169">hello example creates a SAS for hello source file, which hello service uses tooauthenticate access toohello source file during hello copy operation.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in hello root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in hello root directory.");

// Get a reference toohello blob toowhich hello file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for hello file that's valid for 24 hours.
// Note that when you are copying a file tooa blob, or a blob tooa file, you must use a SAS
// tooauthenticate access toohello source object, even if you are copying within hello same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for hello source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct hello URI toohello source file, including hello SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy hello file toohello blob.
destBlob.StartCopy(fileSasUri);

// Write hello contents of hello file toohello console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

<span data-ttu-id="0ccab-170">Můžete kopírovat soubor tooa objektů blob v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="0ccab-170">You can copy a blob tooa file in hello same way.</span></span> <span data-ttu-id="0ccab-171">Pokud hello zdrojový objekt, který je objekt blob, vytvořte objekt blob SAS tooauthenticate přístup toothat během operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-171">If hello source object is a blob, then create a SAS tooauthenticate access toothat blob during hello copy operation.</span></span>

## <a name="troubleshooting-azure-file-storage-using-metrics"></a><span data-ttu-id="0ccab-172">Řešení potíží se službou Azure File Storage pomocí metrik</span><span class="sxs-lookup"><span data-stu-id="0ccab-172">Troubleshooting Azure File storage using metrics</span></span>
<span data-ttu-id="0ccab-173">Azure Storage Analytics teď podporuje metriky pro službu Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="0ccab-173">Azure Storage Analytics now supports metrics for Azure File storage.</span></span> <span data-ttu-id="0ccab-174">S údaji z metriky můžete sledovat žádosti a diagnostikovat potíže.</span><span class="sxs-lookup"><span data-stu-id="0ccab-174">With metrics data, you can trace requests and diagnose issues.</span></span>


<span data-ttu-id="0ccab-175">Můžete povolit metriky pro úložiště Azure File hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ccab-175">You can enable metrics for Azure File storage from hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="0ccab-176">Můžete také povolit metriky programově pomocí volání hello operace Set File Service Properties přes hello REST API nebo některou z podobných operací v hello Klientská knihovna pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ccab-176">You can also enable metrics programmatically by calling hello Set File Service Properties operation via hello REST API, or one of its analogues in hello Storage Client Library.</span></span>


<span data-ttu-id="0ccab-177">Hello následující příklad kódu ukazuje, jak toouse hello Klientská knihovna pro úložiště pro .NET tooenable metriky pro úložiště Azure File.</span><span class="sxs-lookup"><span data-stu-id="0ccab-177">hello following code example shows how toouse hello Storage Client Library for .NET tooenable metrics for Azure File storage.</span></span>

<span data-ttu-id="0ccab-178">Nejprve přidejte následující hello `using` tooyour direktivy `Program.cs` souboru, kromě toothose jste přidali výše:</span><span class="sxs-lookup"><span data-stu-id="0ccab-178">First, add hello following `using` directives tooyour `Program.cs` file, in addition toothose you added above:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

<span data-ttu-id="0ccab-179">Pamatujte, že při Azure BLOB, tabulka Azure a Azure používají hello sdílené `ServiceProperties` typu v hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` obor názvů, úložiště Azure File používá vlastní typ hello `FileServiceProperties` typu v hello `Microsoft.WindowsAzure.Storage.File.Protocol` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="0ccab-179">Note that while Azure Blobs, Azure Table, and Azure Queues use hello shared `ServiceProperties` type in hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` namespace, Azure File storage uses its own type, hello `FileServiceProperties` type in hello `Microsoft.WindowsAzure.Storage.File.Protocol` namespace.</span></span> <span data-ttu-id="0ccab-180">Oba obory názvů musí odkazovat z vašeho kódu, ale pro následující kód toocompile hello.</span><span class="sxs-lookup"><span data-stu-id="0ccab-180">Both namespaces must be referenced from your code, however, for hello following code toocompile.</span></span>

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create hello File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that hello File service currently uses its own service properties type,
// available in hello Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read hello metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

<span data-ttu-id="0ccab-181">Navíc můžete odkazovat příliš[Azure File storage článku Poradce při potížích s](storage-troubleshoot-windows-file-connection-problems.md) pro pokyny k odstraňování problémů začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="0ccab-181">Also, you can refer too[Azure File storage Troubleshooting Article](storage-troubleshoot-windows-file-connection-problems.md) for end-to-end troubleshooting guidance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ccab-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ccab-182">Next steps</span></span>
<span data-ttu-id="0ccab-183">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="0ccab-183">See these links for more information about Azure File storage.</span></span>

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="0ccab-184">Koncepční články a videa</span><span class="sxs-lookup"><span data-stu-id="0ccab-184">Conceptual articles and videos</span></span>
* [<span data-ttu-id="0ccab-185">Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux</span><span class="sxs-lookup"><span data-stu-id="0ccab-185">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="0ccab-186">Jak toouse Azure File storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="0ccab-186">How toouse Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a><span data-ttu-id="0ccab-187">Podpora nástrojů pro úložiště File</span><span class="sxs-lookup"><span data-stu-id="0ccab-187">Tooling support for File storage</span></span>
* [<span data-ttu-id="0ccab-188">Jak toouse AzCopy s Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0ccab-188">How toouse AzCopy with Microsoft Azure Storage</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [<span data-ttu-id="0ccab-189">Použití hello rozhraní příkazového řádku Azure s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0ccab-189">Using hello Azure CLI with Azure Storage</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [<span data-ttu-id="0ccab-190">Řešení potíží se službou Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="0ccab-190">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a><span data-ttu-id="0ccab-191">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="0ccab-191">Reference</span></span>
* [<span data-ttu-id="0ccab-192">Klientská knihovna Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="0ccab-192">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="0ccab-193">REST API služby File – referenční informace</span><span class="sxs-lookup"><span data-stu-id="0ccab-193">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a><span data-ttu-id="0ccab-194">Příspěvky na blozích</span><span class="sxs-lookup"><span data-stu-id="0ccab-194">Blog posts</span></span>
* [<span data-ttu-id="0ccab-195">Úložiště Azure File je nyní dostupné pro veřejnost</span><span class="sxs-lookup"><span data-stu-id="0ccab-195">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="0ccab-196">Uvnitř Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="0ccab-196">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="0ccab-197">Představujeme službu Microsoft Azure File</span><span class="sxs-lookup"><span data-stu-id="0ccab-197">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="0ccab-198">Zachování připojení tooMicrosoft Azure File storage</span><span class="sxs-lookup"><span data-stu-id="0ccab-198">Persisting connections tooMicrosoft Azure File storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
