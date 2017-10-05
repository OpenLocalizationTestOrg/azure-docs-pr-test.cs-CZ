---
title: "Vývoj pro Azure File Storage pomocí .NET | Dokumentace Microsoftu"
description: "Zjistěte, jak vyvíjet aplikace a služby .NET, které používají službu Azure File Storage k ukládání dat souborů."
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
ms.openlocfilehash: 7b94e70619324bb8dc8e7f8306f00f06e7476c1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a><span data-ttu-id="4a0b3-103">Vývoj pro službu Azure File Storage pomocí .NET</span><span class="sxs-lookup"><span data-stu-id="4a0b3-103">Develop for Azure File storage with .NET</span></span> 
> [!NOTE]
> <span data-ttu-id="4a0b3-104">Tento článek ukazuje, jak spravovat službu Azure File Storage pomocí kódu .NET.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-104">This article shows how to manage Azure File storage with .NET code.</span></span> <span data-ttu-id="4a0b3-105">Další informace o službě Azure File Storage najdete v tématu [Úvod do služby Azure File Storage](storage-files-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4a0b3-105">To learn more about Azure File storage, please see the [Introduction to Azure File storage](storage-files-introduction.md).</span></span>
>

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="4a0b3-106">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="4a0b3-106">About this tutorial</span></span>
<span data-ttu-id="4a0b3-107">Tento kurz vám ukáže základy používání technologie .NET k vývoji aplikací a služeb, které používají službu Azure File Storage k ukládání dat souborů.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-107">This tutorial will demonstrate the basics of using .NET to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="4a0b3-108">V tomto kurzu vytvoříme jednoduchou konzolovou aplikaci a ukážeme si, jak provádět základní akce s technologií .NET a službou Azure File Storage:</span><span class="sxs-lookup"><span data-stu-id="4a0b3-108">In this tutorial, we will create a simple console application and show how to perform basic actions with .NET and Azure File storage:</span></span>

* <span data-ttu-id="4a0b3-109">Získání obsahu souboru</span><span class="sxs-lookup"><span data-stu-id="4a0b3-109">Get the contents of a file</span></span>
* <span data-ttu-id="4a0b3-110">Nastavte kvótu (maximální velikost) pro sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-110">Set the quota (maximum size) for the file share.</span></span>
* <span data-ttu-id="4a0b3-111">Vytvořte sdílený přístupový podpis (klíč SAS) pro soubor, který používá zásady sdíleného přístupu definované ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-111">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on the share.</span></span>
* <span data-ttu-id="4a0b3-112">Zkopírujte soubor do jiného souboru ve stejném účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-112">Copy a file to another file in the same storage account.</span></span>
* <span data-ttu-id="4a0b3-113">Zkopírujte soubor do objektu blob ve stejném účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-113">Copy a file to a blob in the same storage account.</span></span>
* <span data-ttu-id="4a0b3-114">Použijte Azure Storage Metrics pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-114">Use Azure Storage Metrics for troubleshooting</span></span>

> [!Note]  
> <span data-ttu-id="4a0b3-115">Protože je služba Azure File Storage přístupná přes protokol SMB, je možné psát jednoduché aplikace, které přistupují ke sdílené složce Azure pomocí standardních tříd System.IO pro vstupně-výstupní operace se soubory.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-115">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard System.IO classes for File I/O.</span></span> <span data-ttu-id="4a0b3-116">Tento článek popisuje, jak psát aplikace, které používají sadu .NET SDK pro Azure Storage, která se službou Azure File Storage komunikuje pomocí rozhraní [REST API pro Azure File Storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="4a0b3-116">This article will describe how to write applications that use the Azure Storage .NET SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span> 


## <a name="create-the-console-application-and-obtain-the-assembly"></a><span data-ttu-id="4a0b3-117">Vytvoření konzolové aplikace a získání sestavení</span><span class="sxs-lookup"><span data-stu-id="4a0b3-117">Create the console application and obtain the assembly</span></span>
<span data-ttu-id="4a0b3-118">V sadě Visual Studio vytvořte novou konzolovou aplikaci pro Windows.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-118">In Visual Studio, create a new Windows console application.</span></span> <span data-ttu-id="4a0b3-119">Následující kroky ukazují, jak vytvořit konzolovou aplikaci v sadě Visual Studio 2017, ale kroky v jiných verzích sady Visual Studio se podobají.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-119">The following steps show you how to create a console application in Visual Studio 2017, however, the steps are similar in other versions of Visual Studio.</span></span>

1. <span data-ttu-id="4a0b3-120">Vyberte **Soubor**  >  **Nový**  >  **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-120">Select **File** > **New** > **Project**</span></span>
2. <span data-ttu-id="4a0b3-121">Vyberte **Instalováno** > **Šablony** > **Visual C#** > **Klasická plocha Windows**.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-121">Select **Installed** > **Templates** > **Visual C#** > **Windows Classic Desktop**</span></span>
3. <span data-ttu-id="4a0b3-122">Vyberte **Aplikace konzoly (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-122">Select **Console App (.NET Framework)**</span></span>
4. <span data-ttu-id="4a0b3-123">Do pole **Název** zadejte název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-123">Enter a name for your application in the **Name:** field</span></span>
5. <span data-ttu-id="4a0b3-124">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-124">Select **OK**</span></span>

<span data-ttu-id="4a0b3-125">Všechny příklady kódu v tomto kurzu můžete přidat do metody `Main()` v souboru `Program.cs` vaší konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-125">All code examples in this tutorial can be added to the `Main()` method of your console application's `Program.cs` file.</span></span>

<span data-ttu-id="4a0b3-126">Můžete použít knihovnu klienta služby Azure Storage z libovolného typu aplikace .NET, včetně webové aplikace nebo cloudové služby Azure, desktopové nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-126">You can use the Azure Storage Client Library in any type of .NET application, including an Azure cloud service or web app, and desktop and mobile applications.</span></span> <span data-ttu-id="4a0b3-127">V této příručce použijeme konzolovou aplikaci kvůli zjednodušení.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-127">In this guide, we use a console application for simplicity.</span></span>

## <a name="use-nuget-to-install-the-required-packages"></a><span data-ttu-id="4a0b3-128">Použití balíčku NuGet k instalaci požadovaných balíčků</span><span class="sxs-lookup"><span data-stu-id="4a0b3-128">Use NuGet to install the required packages</span></span>
<span data-ttu-id="4a0b3-129">Abyste mohli tento kurz dokončit, potřebujete ze svého projektu odkazovat na dva balíčky:</span><span class="sxs-lookup"><span data-stu-id="4a0b3-129">There are two packages you need to reference in your project to complete this tutorial:</span></span>

* <span data-ttu-id="4a0b3-130">[Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Tento balíček zajišťuje programový přístup k datovým prostředkům na účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-130">[Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): This package provides programmatic access to data resources in your storage account.</span></span>
* <span data-ttu-id="4a0b3-131">[Microsoft Azure Configuration Manager library for .NET:](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) Tento balíček poskytuje třídu pro potřeby analýzy připojovacího řetězce v konfiguračním souboru bez ohledu na to, kde je aplikace spuštěná.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-131">[Microsoft Azure Configuration Manager library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): This package provides a class for parsing a connection string in a configuration file, regardless of where your application is running.</span></span>

<span data-ttu-id="4a0b3-132">K získání obou balíčků můžete použít balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-132">You can use NuGet to obtain both packages.</span></span> <span data-ttu-id="4a0b3-133">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="4a0b3-133">Follow these steps:</span></span>

1. <span data-ttu-id="4a0b3-134">Klikněte v **Průzkumníku řešení** pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-134">Right-click your project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="4a0b3-135">Proveďte online hledání textu „WindowsAzure.Storage“ a kliknutím na **Instalovat** nainstalujte balíček knihovny klienta úložiště a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-135">Search online for "WindowsAzure.Storage" and click **Install** to install the Storage Client Library and its dependencies.</span></span>
3. <span data-ttu-id="4a0b3-136">Proveďte online hledání textu „WindowsAzure.ConfigurationManager“ a kliknutím na **Instalovat** nainstalujete správce konfigurace Azure.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-136">Search online for "WindowsAzure.ConfigurationManager" and click **Install** to install the Azure Configuration Manager.</span></span>

## <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a><span data-ttu-id="4a0b3-137">Uložení přihlašovacích údajů pro účet úložiště do souboru app.config</span><span class="sxs-lookup"><span data-stu-id="4a0b3-137">Save your storage account credentials to the app.config file</span></span>
<span data-ttu-id="4a0b3-138">Dál uložte svoje přihlašovací údaje do souboru app.config vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-138">Next, save your credentials in your project's app.config file.</span></span> <span data-ttu-id="4a0b3-139">Upravte soubor app.config tak, aby vypadal podobně jako v následujícím příkladu, `myaccount` nahraďte názvem svého účtu úložiště a `mykey` nahraďte svým klíčem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-139">Edit the app.config file so that it appears similar to the following example, replacing `myaccount` with your storage account name, and `mykey` with your storage account key.</span></span>

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
> Nejnovější verze emulátoru úložiště Azure nepodporuje službu Azure File Storage. <span data-ttu-id="4a0b3-141">Aby váš připojovací řetězec mohl pracovat se službou Azure File Storage, musí jako cíl mít účet služby Azure Storage v cloudu.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-141">Your connection string must target an Azure Storage Account in the cloud to work with Azure File storage.</span></span>

## <a name="add-using-directives"></a><span data-ttu-id="4a0b3-142">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="4a0b3-142">Add using directives</span></span>
<span data-ttu-id="4a0b3-143">V Průzkumníku řešení otevřete soubor `Program.cs` a na začátek souboru přidejte následující direktivy using.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-143">Open the `Program.cs` file from Solution Explorer, and add the following using directives to the top of the file.</span></span>

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-the-file-share-programmatically"></a><span data-ttu-id="4a0b3-144">Programový přístup ke sdílené složce</span><span class="sxs-lookup"><span data-stu-id="4a0b3-144">Access the file share programmatically</span></span>
<span data-ttu-id="4a0b3-145">Dál přidejte následující kód k metodě `Main()` (po výše uvedeném kódu) pro získání připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-145">Next, add the following code to the `Main()` method (after the code shown above) to retrieve the connection string.</span></span> <span data-ttu-id="4a0b3-146">Tento kód získá odkaz na soubor, který jsme vytvořili předtím, a vypíše obsah do okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-146">This code gets a reference to the file we created earlier and outputs its contents to the console window.</span></span>

```csharp
// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

<span data-ttu-id="4a0b3-147">Výstup zobrazíte spuštěním konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-147">Run the console application to see the output.</span></span>

## <a name="set-the-maximum-size-for-a-file-share"></a><span data-ttu-id="4a0b3-148">Nastavení maximální velikosti sdílené složky</span><span class="sxs-lookup"><span data-stu-id="4a0b3-148">Set the maximum size for a file share</span></span>
<span data-ttu-id="4a0b3-149">Klientská knihovna pro úložiště Azure od verze 5.x umožňuje nastavit kvótu (maximální velikost) sdílené složky v gigabajtech.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-149">Beginning with version 5.x of the Azure Storage Client Library, you can set set the quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="4a0b3-150">Můžete se taky podívat, kolik data je aktuálně uloženo ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-150">You can also check to see how much data is currently stored on the share.</span></span>

<span data-ttu-id="4a0b3-151">Pokud nastavíte kvótu sdílené složky, můžete omezit celkovou velikost souborů uložených ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-151">By setting the quota for a share, you can limit the total size of the files stored on the share.</span></span> <span data-ttu-id="4a0b3-152">Pokud celková velikost souborů ve sdílené složce překročí kvótu nastavenou pro sdílenou složku, klienti nebudou moct zvyšovat velikost existujících souborů, s výjimkou situace, když je velikost souborů nulová.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-152">If the total size of files on the share exceeds the quota set on the share, then clients will be unable to increase the size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="4a0b3-153">Dole uvedený příklad ukazuje, jak můžete zkontrolovat aktuální využití sdílené složky a jak nastavit kvótu pro sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-153">The example below shows how to check the current usage for a share and how to set the quota for the share.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="4a0b3-154">Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku</span><span class="sxs-lookup"><span data-stu-id="4a0b3-154">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="4a0b3-155">Klientská knihovna pro úložiště Azure od verze 5.x umožňuje vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-155">Beginning with version 5.x of the Azure Storage Client Library, you can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="4a0b3-156">Můžete taky vytvořit sdílené zásady přístupu pro sdílenou složku ke správě sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-156">You can also create a shared access policy on a file share to manage shared access signatures.</span></span> <span data-ttu-id="4a0b3-157">Vytvoření sdílených zásad přístupu se doporučuje, protože se tím nabízí způsob odvolání SAS v případě narušení jeho integrity nebo důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-157">Creating a shared access policy is recommended, as it provides a means of revoking the SAS if it should be compromised.</span></span>

<span data-ttu-id="4a0b3-158">V následujícím příkladu se vytvoří sdílená zásada přístupu pro sdílenou složku a ta se pak použije k omezení pro SAS na souboru ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-158">The following example creates a shared access policy on a share, and then uses that policy to provide the constraints for a SAS on a file in the share.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

<span data-ttu-id="4a0b3-159">Další informace o vytváření a používání sdílených přístupových podpisů najdete v tématech [Použití sdílených přístupových podpisů (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) a [Vytvoření a použití SAS s objekty blob Azure](../blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="4a0b3-159">For more information about creating and using shared access signatures, see [Using Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) and [Create and use a SAS with Azure Blobs](../blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span>

## <a name="copy-files"></a><span data-ttu-id="4a0b3-160">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="4a0b3-160">Copy files</span></span>
<span data-ttu-id="4a0b3-161">Klientská knihovna pro úložiště Azure od verze 5.x umožňuje kopírovat soubor do jiného souboru, soubor do objektu nebo objekt blob do souboru.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-161">Beginning with version 5.x of the Azure Storage Client Library, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="4a0b3-162">V dalších kapitolách ukážeme, jak se operace kopírovaní dají provést programově.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-162">In the next sections, we demonstrate how to perform these copy operations programmatically.</span></span>

<span data-ttu-id="4a0b3-163">Pomocí nástroje AzCopy taky můžete zkopírovat jeden soubor do jiného nebo zkopírovat objekt blob do souboru a obráceně.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-163">You can also use AzCopy to copy one file to another or to copy a blob to a file or vice versa.</span></span> <span data-ttu-id="4a0b3-164">Viz téma [Přenos dat pomocí nástroje příkazového řádku AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4a0b3-164">See [Transfer data with the AzCopy Command-Line Utility](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="4a0b3-165">Pokud kopírujete objekt blob do souboru nebo soubor do objektu blob, musíte použít sdílený přístupový klíč (SAS) k ověření zdroje objektu, a to i když kopírujete v rámci jednoho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-165">If you are copying a blob to a file, or a file to a blob, you must use a shared access signature (SAS) to authenticate the source object, even if you are copying within the same storage account.</span></span>
> 
> 

<span data-ttu-id="4a0b3-166">**Kopírování souboru do jiného souboru** V následujícím příkladu se zkopíruje soubor do jiného souboru ve stejné sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-166">**Copy a file to another file** The following example copies a file to another file in the same share.</span></span> <span data-ttu-id="4a0b3-167">Protože tato operace kopírování kopíruje soubory ve stejném účtu úložiště, můžete pro kopírování použít ověření Sdíleným klíčem.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-167">Because this copy operation copies between files in the same storage account, you can use Shared Key authentication to perform the copy.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

<span data-ttu-id="4a0b3-168">**Kopírování souboru do objektu blob** V následujícím příkladu se vytvoří soubor a zkopíruje se do objektu blob ve stejném účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-168">**Copy a file to a blob** The following example creates a file and copies it to a blob within the same storage account.</span></span> <span data-ttu-id="4a0b3-169">V příkladu se vytvoří SAS pro zdrojový soubor a služba ho při kopírování použije k ověření přístupu ke zdrojovému souboru.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-169">The example creates a SAS for the source file, which the service uses to authenticate access to the source file during the copy operation.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authenticate access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

<span data-ttu-id="4a0b3-170">Stejným způsobem můžete kopírovat objekt blob do souboru.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-170">You can copy a blob to a file in the same way.</span></span> <span data-ttu-id="4a0b3-171">Pokud je zdrojovým objektem objekt blob, vytvořte SAS k ověření přístupu k tomuto objektu blob při kopírování.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-171">If the source object is a blob, then create a SAS to authenticate access to that blob during the copy operation.</span></span>

## <a name="troubleshooting-azure-file-storage-using-metrics"></a><span data-ttu-id="4a0b3-172">Řešení potíží se službou Azure File Storage pomocí metrik</span><span class="sxs-lookup"><span data-stu-id="4a0b3-172">Troubleshooting Azure File storage using metrics</span></span>
<span data-ttu-id="4a0b3-173">Azure Storage Analytics teď podporuje metriky pro službu Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-173">Azure Storage Analytics now supports metrics for Azure File storage.</span></span> <span data-ttu-id="4a0b3-174">S údaji z metriky můžete sledovat žádosti a diagnostikovat potíže.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-174">With metrics data, you can trace requests and diagnose issues.</span></span>


<span data-ttu-id="4a0b3-175">Metriky pro službu Azure File Storage můžete povolit na webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a0b3-175">You can enable metrics for Azure File storage from the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="4a0b3-176">Metriky taky můžete zapnout programově zavoláním operace Set File Service Properties přes REST API nebo některou z podobných operací v Klientské knihovně pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-176">You can also enable metrics programmatically by calling the Set File Service Properties operation via the REST API, or one of its analogues in the Storage Client Library.</span></span>


<span data-ttu-id="4a0b3-177">Následující příklad kódu ukazuje, jak můžete použít Klientskou knihovnu pro úložiště pro .NET k zapnutí metrik pro službu Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-177">The following code example shows how to use the Storage Client Library for .NET to enable metrics for Azure File storage.</span></span>

<span data-ttu-id="4a0b3-178">Nejdříve do souboru `Program.cs` mimo těch, které jste přidali výše, přidejte následující direktivy `using`:</span><span class="sxs-lookup"><span data-stu-id="4a0b3-178">First, add the following `using` directives to your `Program.cs` file, in addition to those you added above:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

<span data-ttu-id="4a0b3-179">Pamatujte, že zatímco objekty blob Azure, Tabulky Azure a Fronty Azure používají sdílený typ `ServiceProperties` v oboru názvů `Microsoft.WindowsAzure.Storage.Shared.Protocol`, Azure File Storage používá vlastní typ `FileServiceProperties` v oboru názvů `Microsoft.WindowsAzure.Storage.File.Protocol`.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-179">Note that while Azure Blobs, Azure Table, and Azure Queues use the shared `ServiceProperties` type in the `Microsoft.WindowsAzure.Storage.Shared.Protocol` namespace, Azure File storage uses its own type, the `FileServiceProperties` type in the `Microsoft.WindowsAzure.Storage.File.Protocol` namespace.</span></span> <span data-ttu-id="4a0b3-180">Aby se ale následující kód mohl zkompilovat, musí se z vašeho kódu odkazovat oba obory názvů.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-180">Both namespaces must be referenced from your code, however, for the following code to compile.</span></span>

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
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

// Read the metrics properties we just set.
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

<span data-ttu-id="4a0b3-181">Podrobné pokyny, jak postupovat při řešení potíží, najdete v článku [Azure File storage Troubleshooting Article](storage-troubleshoot-windows-file-connection-problems.md) (Azure File Storage – řešení problémů).</span><span class="sxs-lookup"><span data-stu-id="4a0b3-181">Also, you can refer to [Azure File storage Troubleshooting Article](storage-troubleshoot-windows-file-connection-problems.md) for end-to-end troubleshooting guidance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a0b3-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a0b3-182">Next steps</span></span>
<span data-ttu-id="4a0b3-183">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="4a0b3-183">See these links for more information about Azure File storage.</span></span>

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="4a0b3-184">Koncepční články a videa</span><span class="sxs-lookup"><span data-stu-id="4a0b3-184">Conceptual articles and videos</span></span>
* [<span data-ttu-id="4a0b3-185">Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux</span><span class="sxs-lookup"><span data-stu-id="4a0b3-185">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="4a0b3-186">Jak používat Azure File Storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4a0b3-186">How to use Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a><span data-ttu-id="4a0b3-187">Podpora nástrojů pro úložiště File</span><span class="sxs-lookup"><span data-stu-id="4a0b3-187">Tooling support for File storage</span></span>
* [<span data-ttu-id="4a0b3-188">Použití nástroje AzCopy s Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4a0b3-188">How to use AzCopy with Microsoft Azure Storage</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [<span data-ttu-id="4a0b3-189">Použití Azure CLI s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4a0b3-189">Using the Azure CLI with Azure Storage</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [<span data-ttu-id="4a0b3-190">Řešení potíží se službou Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="4a0b3-190">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a><span data-ttu-id="4a0b3-191">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="4a0b3-191">Reference</span></span>
* [<span data-ttu-id="4a0b3-192">Klientská knihovna Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="4a0b3-192">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="4a0b3-193">REST API služby File – referenční informace</span><span class="sxs-lookup"><span data-stu-id="4a0b3-193">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a><span data-ttu-id="4a0b3-194">Příspěvky na blozích</span><span class="sxs-lookup"><span data-stu-id="4a0b3-194">Blog posts</span></span>
* [<span data-ttu-id="4a0b3-195">Úložiště Azure File je nyní dostupné pro veřejnost</span><span class="sxs-lookup"><span data-stu-id="4a0b3-195">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="4a0b3-196">Uvnitř Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="4a0b3-196">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="4a0b3-197">Představujeme službu Microsoft Azure File</span><span class="sxs-lookup"><span data-stu-id="4a0b3-197">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="4a0b3-198">Nastavení trvalých připojení ke službě Microsoft Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="4a0b3-198">Persisting connections to Microsoft Azure File storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)