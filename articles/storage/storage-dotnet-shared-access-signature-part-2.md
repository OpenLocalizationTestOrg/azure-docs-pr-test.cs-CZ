---
title: "Vytváření a používání sdíleného přístupového podpisu (SAS) s Azure Blob storage | Microsoft Docs"
description: "Tento kurz ukazuje, jak vytvořit sdílené přístupové podpisy pro použití s úložištěm Blob a jak využívat v klientských aplikacích."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 491e0b3c-76d4-4149-9a80-bbbd683b1f3e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: ba78dd2bbcc68ffffeba59b1623891126baf656f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a><span data-ttu-id="df40e-103">Sdílené přístupové podpisy, část 2: Vytvoření a použití SAS s úložištěm Blob</span><span class="sxs-lookup"><span data-stu-id="df40e-103">Shared Access Signatures, Part 2: Create and use a SAS with Blob storage</span></span>

<span data-ttu-id="df40e-104">[Část 1](storage-dotnet-shared-access-signature-part-1.md) tohoto kurzu prozkoumali sdílené přístupové podpisy (SAS) a vysvětlení osvědčené postupy pro jejich použití.</span><span class="sxs-lookup"><span data-stu-id="df40e-104">[Part 1](storage-dotnet-shared-access-signature-part-1.md) of this tutorial explored shared access signatures (SAS) and explained best practices for using them.</span></span> <span data-ttu-id="df40e-105">Část 2 ukazuje, jak vygenerovat a pak použít sdílené přístupové podpisy úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="df40e-105">Part 2 shows you how to generate and then use shared access signatures with Blob storage.</span></span> <span data-ttu-id="df40e-106">Příklady jsou napsané v jazyce C# a použít knihovnu klienta služby Azure Storage pro .NET.</span><span class="sxs-lookup"><span data-stu-id="df40e-106">The examples are written in C# and use the Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="df40e-107">Příklady v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="df40e-107">The examples in this tutorial:</span></span>

* <span data-ttu-id="df40e-108">Vygenerovat sdílený přístupový podpis do kontejneru</span><span class="sxs-lookup"><span data-stu-id="df40e-108">Generate a shared access signature on a container</span></span>
* <span data-ttu-id="df40e-109">Vygenerovat sdílený přístupový podpis na objekt blob</span><span class="sxs-lookup"><span data-stu-id="df40e-109">Generate a shared access signature on a blob</span></span>
* <span data-ttu-id="df40e-110">Vytvoření zásady uložené přístupu ke správě podpisů v kontejneru na prostředky</span><span class="sxs-lookup"><span data-stu-id="df40e-110">Create a stored access policy to manage signatures on a container's resources</span></span>
* <span data-ttu-id="df40e-111">Testovací sdílené přístupové podpisy v aplikaci klienta</span><span class="sxs-lookup"><span data-stu-id="df40e-111">Test the shared access signatures in a client application</span></span>

## <a name="about-this-tutorial"></a><span data-ttu-id="df40e-112">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="df40e-112">About this tutorial</span></span>
<span data-ttu-id="df40e-113">V tomto kurzu vytvoříme dvě konzolové aplikace, které ukazují, vytvoření a použití sdílených přístupových podpisů pro kontejnery a objekty BLOB:</span><span class="sxs-lookup"><span data-stu-id="df40e-113">In this tutorial, we create two console applications that demonstrate creating and using shared access signatures for containers and blobs:</span></span>

<span data-ttu-id="df40e-114">**Aplikaci 1**: aplikace pro správu.</span><span class="sxs-lookup"><span data-stu-id="df40e-114">**Application 1**: The management application.</span></span> <span data-ttu-id="df40e-115">Vygeneruje sdílený přístupový podpis kontejneru a objekt blob.</span><span class="sxs-lookup"><span data-stu-id="df40e-115">Generates a shared access signature for a container and a blob.</span></span> <span data-ttu-id="df40e-116">Přístupový klíč účtu úložiště zahrnuje ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="df40e-116">Includes the storage account access key in source code.</span></span>

<span data-ttu-id="df40e-117">**Aplikace 2**: klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="df40e-117">**Application 2**: The client application.</span></span> <span data-ttu-id="df40e-118">Přístupy kontejnerů a objektů blob prostředky použití sdílených přístupových podpisů vytvoření první aplikace.</span><span class="sxs-lookup"><span data-stu-id="df40e-118">Accesses container and blob resources using the shared access signatures created with the first application.</span></span> <span data-ttu-id="df40e-119">Používá sdílené přístupové podpisy k přístupu kontejneru a prostředkům blob – provede *není* zahrnují přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="df40e-119">Uses only the shared access signatures to access container and blob resources--it does *not* include the storage account access key.</span></span>

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a><span data-ttu-id="df40e-120">Část 1: Vytvořte konzolovou aplikaci pro generování podpisů sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="df40e-120">Part 1: Create a console application to generate shared access signatures</span></span>
<span data-ttu-id="df40e-121">Nejprve je třeba splnit Klientská knihovna pro úložiště Azure pro .NET nainstalované.</span><span class="sxs-lookup"><span data-stu-id="df40e-121">First, ensure that you have the Azure Storage Client Library for .NET installed.</span></span> <span data-ttu-id="df40e-122">Můžete nainstalovat [balíček NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "balíček NuGet") obsahující nejaktuálnější sestavení pro knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="df40e-122">You can install the [NuGet package](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet package") containing the most up-to-date assemblies for the client library.</span></span> <span data-ttu-id="df40e-123">Toto je doporučená metoda pro zajištění, že máte nejnovější opravy.</span><span class="sxs-lookup"><span data-stu-id="df40e-123">This is the recommended method for ensuring that you have the most recent fixes.</span></span> <span data-ttu-id="df40e-124">Klientská knihovna můžete také stáhnout jako součást nejnovější verzi [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="df40e-124">You can also download the client library as part of the most recent version of the [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="df40e-125">V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **GenerateSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="df40e-125">In Visual Studio, create a new Windows console application and name it **GenerateSharedAccessSignatures**.</span></span> <span data-ttu-id="df40e-126">Přidejte odkazy na [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) a [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) pomocí jedné z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="df40e-126">Add references to [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) by using one of the following approaches:</span></span>

* <span data-ttu-id="df40e-127">Použití [Správce balíčků NuGet](https://docs.nuget.org/consume/installing-nuget) v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df40e-127">Use the [NuGet package manager](https://docs.nuget.org/consume/installing-nuget) in Visual Studio.</span></span> <span data-ttu-id="df40e-128">Vyberte **projektu** > **spravovat balíčky NuGet**, vyhledejte online každý balíček (Microsoft.WindowsAzure.ConfigurationManager a WindowsAzure.Storage) a instalovat je.</span><span class="sxs-lookup"><span data-stu-id="df40e-128">Select **Project** > **Manage NuGet Packages**, search online for each package (Microsoft.WindowsAzure.ConfigurationManager and WindowsAzure.Storage) and install them.</span></span>
* <span data-ttu-id="df40e-129">Můžete taky najít tyto sestavení v instalaci sady Azure SDK a přidejte odkazy na tyto:</span><span class="sxs-lookup"><span data-stu-id="df40e-129">Alternatively, locate these assemblies in your installation of the Azure SDK and add references to them:</span></span>
  * <span data-ttu-id="df40e-130">Microsoft.WindowsAzure.Configuration.dll</span><span class="sxs-lookup"><span data-stu-id="df40e-130">Microsoft.WindowsAzure.Configuration.dll</span></span>
  * <span data-ttu-id="df40e-131">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="df40e-131">Microsoft.WindowsAzure.Storage.dll</span></span>

<span data-ttu-id="df40e-132">Na začátku souboru Program.cs přidejte následující **pomocí** direktivy:</span><span class="sxs-lookup"><span data-stu-id="df40e-132">At the top of the Program.cs file, add the following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="df40e-133">Upravte soubor app.config tak, aby obsahoval nastavení konfigurace s připojovacím řetězcem, který odkazuje na účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="df40e-133">Edit the app.config file so that it contains a configuration setting with a connection string that points to your storage account.</span></span> <span data-ttu-id="df40e-134">Váš soubor app.config by měl vypadat podobně jako k tomuto:</span><span class="sxs-lookup"><span data-stu-id="df40e-134">Your app.config file should look similar to this one:</span></span>

```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
  </appSettings>
</configuration>
```

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a><span data-ttu-id="df40e-135">Vygenerovat sdílený přístupový podpis identifikátor URI pro kontejner</span><span class="sxs-lookup"><span data-stu-id="df40e-135">Generate a shared access signature URI for a container</span></span>
<span data-ttu-id="df40e-136">Přidáme způsob, jak vygenerovat sdílený přístupový podpis na nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="df40e-136">To begin with, we add a method to generate a shared access signature on a new container.</span></span> <span data-ttu-id="df40e-137">V takovém případě podpis není přidružený k zásadě uložené přístup, takže představuje v identifikátoru URI informace o jeho čas vypršení platnosti a oprávnění, která uděluje.</span><span class="sxs-lookup"><span data-stu-id="df40e-137">In this case, the signature is not associated with a stored access policy, so it carries on the URI the information indicating its expiry time and the permissions it grants.</span></span>

<span data-ttu-id="df40e-138">Nejprve přidejte kód, který **Main()** metoda ověření přístupu k účtu úložiště a vytvořit nový kontejner:</span><span class="sxs-lookup"><span data-stu-id="df40e-138">First, add code to the **Main()** method to authenticate access to your storage account and create a new container:</span></span>

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls to the methods created below here...

    //Require user input before closing the console window.
    Console.ReadLine();
}
```

<span data-ttu-id="df40e-139">V dalším kroku přidejte metodu, která generuje sdílený přístupový podpis kontejneru a vrátí podpis URI:</span><span class="sxs-lookup"><span data-stu-id="df40e-139">Next, add a method that generates the shared access signature for the container and returns the signature URI:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set the expiry time and permissions for the container.
    //In this case no start time is specified, so the shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="df40e-140">Přidejte následující řádky na konci **Main()** metoda před voláním **Console.ReadLine()**, aby volal **GetContainerSasUri()** a zápis podpis URI v okně konzoly:</span><span class="sxs-lookup"><span data-stu-id="df40e-140">Add the following lines at the bottom of the **Main()** method, before the call to **Console.ReadLine()**, to call **GetContainerSasUri()** and write the signature URI to the console window:</span></span>

```csharp
//Generate a SAS URI for the container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="df40e-141">Zkompilování a spuštění výstup sdílený přístupový podpis identifikátor URI pro nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="df40e-141">Compile and run to output the shared access signature URI for the new container.</span></span> <span data-ttu-id="df40e-142">Identifikátor URI bude podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="df40e-142">The URI will be similar to the following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

<span data-ttu-id="df40e-143">Jakmile spustíte kód, bude sdílený přístupový podpis, který jste vytvořili pro kontejner platný dobu následujících 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="df40e-143">Once you have run the code, the shared access signature you created for the container will be valid for the next 24 hours.</span></span> <span data-ttu-id="df40e-144">Podpis uděluje oprávnění klienta k seznamu objektů BLOB v kontejneru a vytvářet nové objekty BLOB do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="df40e-144">The signature grants a client permission to list blobs in the container and to write new blobs to the container.</span></span>

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a><span data-ttu-id="df40e-145">Vygenerovat sdílený přístupový podpis identifikátor URI pro objekt blob</span><span class="sxs-lookup"><span data-stu-id="df40e-145">Generate a shared access signature URI for a blob</span></span>
<span data-ttu-id="df40e-146">Dále napište nám podobný kód k vytvoření nového objektu blob v kontejneru a vygenerovat sdílený přístupový podpis pro ni.</span><span class="sxs-lookup"><span data-stu-id="df40e-146">Next, we write similar code to create a new blob within the container and generate a shared access signature for it.</span></span> <span data-ttu-id="df40e-147">Tento sdílený přístupový podpis není přidružený k zásadě uložené přístup tak, aby zahrnovala čas spuštění, čas vypršení platnosti a informace o oprávněních v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="df40e-147">This shared access signature is not associated with a stored access policy, so it includes the start time, expiry time, and permission information in the URI.</span></span>

<span data-ttu-id="df40e-148">Přidejte novou metodu, která vytvoří nový objekt blob a zapíše text, pak vygeneruje sdílený přístupový podpis a vrátí podpis URI:</span><span class="sxs-lookup"><span data-stu-id="df40e-148">Add a new method that creates a new blob and writes some text to it, then generates a shared access signature and returns the signature URI:</span></span>

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set the expiry time and permissions for the blob.
    //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
    //The shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="df40e-149">V dolní části **Main()** metoda, přidejte následující řádky k volání **GetBlobSasUri()**, před voláním **Console.ReadLine()**, a zápis sdílený přístupový podpis URI v okně konzoly:</span><span class="sxs-lookup"><span data-stu-id="df40e-149">At the bottom of the **Main()** method, add the following lines to call **GetBlobSasUri()**, before the call to **Console.ReadLine()**, and write the shared access signature URI to the console window:</span></span>

```csharp
//Generate a SAS URI for a blob within the container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="df40e-150">Zkompilování a spuštění výstup sdílený přístupový podpis identifikátor URI pro tento nový objekt blob.</span><span class="sxs-lookup"><span data-stu-id="df40e-150">Compile and run to output the shared access signature URI for the new blob.</span></span> <span data-ttu-id="df40e-151">Identifikátor URI bude podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="df40e-151">The URI will be similar to the following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-the-container"></a><span data-ttu-id="df40e-152">Vytvoření zásady přístupu uložené na kontejneru</span><span class="sxs-lookup"><span data-stu-id="df40e-152">Create a stored access policy on the container</span></span>
<span data-ttu-id="df40e-153">Nyní Pojďme vytvořit zásadu přístupu uložené na kontejneru, který bude definování omezení pro všechny sdílené přístupové podpisy, které jsou k ní přidružena.</span><span class="sxs-lookup"><span data-stu-id="df40e-153">Now let's create a stored access policy on the container, which will define the constraints for any shared access signatures that are associated with it.</span></span>

<span data-ttu-id="df40e-154">V předchozích příkladech jsme určený čas spuštění (implicitně nebo explicitně), čas vypršení platnosti a oprávnění na sdílený přístupový podpis URI sám sebe.</span><span class="sxs-lookup"><span data-stu-id="df40e-154">In the previous examples, we specified the start time (implicitly or explicitly), the expiry time, and the permissions on the shared access signature URI itself.</span></span> <span data-ttu-id="df40e-155">V následujících příkladech jsme specifikovat v zásadách přístupu uložené, nikoli na sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="df40e-155">In the following examples, we specify these on the stored access policy, not on the shared access signature.</span></span> <span data-ttu-id="df40e-156">Díky tomu nám změnit bez opětovného vydání sdílený přístupový podpis těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="df40e-156">Doing so enables us to change these constraints without reissuing the shared access signature.</span></span>

<span data-ttu-id="df40e-157">Je možné, že jeden nebo více omezení u sdílený přístupový podpis a zbytek na zásadách uložené přístupu.</span><span class="sxs-lookup"><span data-stu-id="df40e-157">It's possible to have one or more of the constraints on the shared access signature, and the remainder on the stored access policy.</span></span> <span data-ttu-id="df40e-158">Však je můžete jenom zadat čas zahájení, čas vypršení platnosti a oprávnění v jednom místě nebo dalších.</span><span class="sxs-lookup"><span data-stu-id="df40e-158">However, you can only specify the start time, expiry time, and permissions in one place or the other.</span></span> <span data-ttu-id="df40e-159">Například nelze nastavit oprávnění na sdílený přístupový podpis a také zadejte je v zásadách přístupu uložené.</span><span class="sxs-lookup"><span data-stu-id="df40e-159">For example, you can't specify permissions on the shared access signature and also specify them on the stored access policy.</span></span>

<span data-ttu-id="df40e-160">Při přidání zásad uložené přístup do kontejneru, musíte získat oprávnění existující kontejneru, přidat nové zásady přístupu a potom nastavte kontejneru oprávnění.</span><span class="sxs-lookup"><span data-stu-id="df40e-160">When you add a stored access policy to a container, you must get the container's existing permissions, add the new access policy, and then set the container's permissions.</span></span>

<span data-ttu-id="df40e-161">Přidejte novou metodu, která vytvoří novou zásadu uložené přístup do kontejneru a vrací název zásady:</span><span class="sxs-lookup"><span data-stu-id="df40e-161">Add a new method that creates a new stored access policy on a container and returns the name of the policy:</span></span>

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get the container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

<span data-ttu-id="df40e-162">V dolní části **Main()** metoda před voláním **Console.ReadLine()**, přidejte následující řádky první zrušte všechny existující zásady přístupu a pak zavolají **CreateSharedAccessPolicy()** metoda:</span><span class="sxs-lookup"><span data-stu-id="df40e-162">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to first clear any existing access policies, and then call the **CreateSharedAccessPolicy()** method:</span></span>

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on the container, which may be optionally used to provide constraints for
//shared access signatures on the container and the blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

<span data-ttu-id="df40e-163">Když zrušíte zaškrtnutí zásady přístupu do kontejneru, musíte nejprve získat kontejneru existující oprávnění, poté zrušte oprávnění a znovu nastavit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="df40e-163">When you clear the access policies on a container, you must first get the container's existing permissions, then clear the permissions, then set the permissions again.</span></span>

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a><span data-ttu-id="df40e-164">Vygenerovat sdílený přístupový podpis URI na kontejneru, který používá zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="df40e-164">Generate a shared access signature URI on the container that uses an access policy</span></span>
<span data-ttu-id="df40e-165">V dalším kroku vytvoříme jiný sdílený přístupový podpis kontejneru, který jsme vytvořili výše, ale tentokrát uložené přístup zásadu, kterou jsme vytvořili v předchozím příkladu jsme přidružit podpis.</span><span class="sxs-lookup"><span data-stu-id="df40e-165">Next, we create another shared access signature for the container that we created earlier, but this time we associate the signature with the stored access policy we created in the previous example.</span></span>

<span data-ttu-id="df40e-166">Přidání nové metody pro generování jiný sdílený přístupový podpis kontejneru:</span><span class="sxs-lookup"><span data-stu-id="df40e-166">Add a new method to generate another shared access signature for the container:</span></span>

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate the shared access signature on the container. In this case, all of the constraints for the
    //shared access signature are specified on the stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="df40e-167">V dolní části **Main()** metoda před voláním **Console.ReadLine()**, přidejte následující řádky k volání **GetContainerSasUriWithPolicy** metoda:</span><span class="sxs-lookup"><span data-stu-id="df40e-167">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to call the **GetContainerSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a><span data-ttu-id="df40e-168">Vygenerovat sdílený přístupový podpis URI u objektu Blob, který používá zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="df40e-168">Generate a Shared Access Signature URI on the Blob That Uses an Access Policy</span></span>
<span data-ttu-id="df40e-169">Nakonec přidáme podobné metody vytvoření jiný objekt blob a vygenerovat sdílený přístupový podpis, který má přidružené k zásadě uložené přístup.</span><span class="sxs-lookup"><span data-stu-id="df40e-169">Finally, we add a similar method to create another blob and generate a shared access signature that's associated with a stored access policy.</span></span>

<span data-ttu-id="df40e-170">Přidání nové metody pro vytvoření objektu blob a vygenerovat sdílený přístupový podpis:</span><span class="sxs-lookup"><span data-stu-id="df40e-170">Add a new method to create a blob and generate a shared access signature:</span></span>

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature. " +
    "A stored access policy defines the constraints for the signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate the shared access signature on the blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="df40e-171">V dolní části **Main()** metoda před voláním **Console.ReadLine()**, přidejte následující řádky k volání **GetBlobSasUriWithPolicy** metoda:</span><span class="sxs-lookup"><span data-stu-id="df40e-171">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to call the **GetBlobSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

<span data-ttu-id="df40e-172">**Main()** metoda by měl nyní vypadat jako v celé jeho šíři.</span><span class="sxs-lookup"><span data-stu-id="df40e-172">The **Main()** method should now look like this in its entirety.</span></span> <span data-ttu-id="df40e-173">Spusťte ji k zápisu sdílený přístupový podpis identifikátory URI v okně konzoly pak zkopírujte a vložte je do textového souboru pro použití v druhé části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="df40e-173">Run it to write the shared access signature URIs to the console window, then copy and paste them into a text file for use in the second part of this tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

<span data-ttu-id="df40e-174">Při spuštění GenerateSharedAccessSignatures konzolové aplikace, zobrazí se výstup podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="df40e-174">When you run the GenerateSharedAccessSignatures console application, you'll see output similar to the following.</span></span> <span data-ttu-id="df40e-175">Jedná se o sdílené přístupové podpisy, které můžete použít v rámci 2 kurzu.</span><span class="sxs-lookup"><span data-stu-id="df40e-175">These are the shared access signatures you use in Part 2 of the tutorial.</span></span>

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a><span data-ttu-id="df40e-176">Část 2: Vytvořte konzolovou aplikaci otestovat sdílených přístupových podpisů</span><span class="sxs-lookup"><span data-stu-id="df40e-176">Part 2: Create a console application to test the shared access signatures</span></span>
<span data-ttu-id="df40e-177">K testování sdílené přístupové podpisy vytvořili v předchozích příkladech, vytvoříme druhý konzolovou aplikaci, která používá podpisů k provádění operací na kontejneru a na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="df40e-177">To test the shared access signatures created in the previous examples, we create a second console application that uses the signatures to perform operations on the container and on a blob.</span></span>

> [!NOTE]
> <span data-ttu-id="df40e-178">Pokud víc než 24 hodin předané vzhledem k tomu, že jste dokončili první část kurzu, podpisy, které jste vygenerovali již nebude platný.</span><span class="sxs-lookup"><span data-stu-id="df40e-178">If more than 24 hours have passed since you completed the first part of the tutorial, the signatures you generated will no longer be valid.</span></span> <span data-ttu-id="df40e-179">V takovém případě byste měli spustit kód v první aplikaci konzoly vygenerovat novou sdílené přístupové podpisy pro použití v druhé části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="df40e-179">In this case, you should run the code in the first console application to generate fresh shared access signatures for use in the second part of the tutorial.</span></span>
>

<span data-ttu-id="df40e-180">V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **ConsumeSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="df40e-180">In Visual Studio, create a new Windows console application and name it **ConsumeSharedAccessSignatures**.</span></span> <span data-ttu-id="df40e-181">Přidejte odkazy na [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) a [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), protože jste provedli dříve.</span><span class="sxs-lookup"><span data-stu-id="df40e-181">Add references to [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), as you did previously.</span></span>

<span data-ttu-id="df40e-182">Na začátku souboru Program.cs přidejte následující **pomocí** direktivy:</span><span class="sxs-lookup"><span data-stu-id="df40e-182">At the top of the Program.cs file, add the following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="df40e-183">V textu **Main()** metoda, přidejte následující konstanty řetězec, změna jejich hodnoty na sdílené přístupové podpisy generované v části 1 kurzu.</span><span class="sxs-lookup"><span data-stu-id="df40e-183">In the body of the **Main()** method, add the following string constants, changing their values to the shared access signatures you generated in part 1 of the tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a><span data-ttu-id="df40e-184">Přidání metody pokusit kontejneru operace pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="df40e-184">Add a method to try container operations using a shared access signature</span></span>
<span data-ttu-id="df40e-185">Potom přidáme metodu, která se testuje některé operace kontejneru pomocí sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="df40e-185">Next, we add a method that tests some container operations using a shared access signature for the container.</span></span> <span data-ttu-id="df40e-186">Sdílený přístupový podpis se používá k vrácení odkaz na kontejner, ověřování přístupu ke kontejneru podle podpis samostatně.</span><span class="sxs-lookup"><span data-stu-id="df40e-186">The shared access signature is used to return a reference to the container, authenticating access to the container based on the signature alone.</span></span>

<span data-ttu-id="df40e-187">Do souboru Program.cs přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="df40e-187">Add the following method to Program.cs:</span></span>

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with the SAS provided.

    //Return a reference to the container using the SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list to store blob URIs returned by a listing operation on the container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob to the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
        blob.UploadText(blobContent);

        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //List operation: List the blobs in the container.
    try
    {
        foreach (ICloudBlob blob in container.ListBlobs())
        {
            blobList.Add(blob);
        }
        Console.WriteLine("List operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("List operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Get a reference to one of the blobs in the container and read it.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        MemoryStream msRead = new MemoryStream();
        msRead.Position = 0;
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            Console.WriteLine(msRead.Length);
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    Console.WriteLine();

    //Delete operation: Delete a blob in the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="df40e-188">Aktualizace **Main()** metoda k volání **UseContainerSAS()** s oběma sdílené přístupové podpisy, které jste vytvořili v kontejneru:</span><span class="sxs-lookup"><span data-stu-id="df40e-188">Update the **Main()** method to call **UseContainerSAS()** with both of the shared access signatures you created on the container:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a><span data-ttu-id="df40e-189">Přidání metody pokusit operace objektů blob pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="df40e-189">Add a method to try blob operations using a shared access signature</span></span>
<span data-ttu-id="df40e-190">Nakonec přidáme metodu, která se testuje některé operace objektů blob pomocí sdíleného přístupového podpisu u objektu blob.</span><span class="sxs-lookup"><span data-stu-id="df40e-190">Finally, we add a method that tests some blob operations using a shared access signature on the blob.</span></span> <span data-ttu-id="df40e-191">V tomto případě používáme konstruktoru **CloudBlockBlob(String)**a předejte sdílený přístupový podpis vrací odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="df40e-191">In this case, we use the constructor **CloudBlockBlob(String)**, passing in the shared access signature, to return a reference to the blob.</span></span> <span data-ttu-id="df40e-192">Další ověřování není vyžadováno; je založena na podpis samostatně.</span><span class="sxs-lookup"><span data-stu-id="df40e-192">No other authentication is required; it's based on the signature alone.</span></span>

<span data-ttu-id="df40e-193">Do souboru Program.cs přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="df40e-193">Add the following method to Program.cs:</span></span>

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using the SAS provided.

    //Return a reference to the blob using the SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob to the container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            blob.UploadFromStream(msWrite);
        }
        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Read the contents of the blob.
    try
    {
        MemoryStream msRead = new MemoryStream();
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            msRead.Position = 0;
            using (StreamReader reader = new StreamReader(msRead, true))
            {
                string line;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Delete operation: Delete the blob.
    try
    {
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="df40e-194">Aktualizace **Main()** metoda k volání **UseBlobSAS()** s oběma sdílené přístupové podpisy, které jste vytvořili na objekt blob:</span><span class="sxs-lookup"><span data-stu-id="df40e-194">Update the **Main()** method to call **UseBlobSAS()** with both of the shared access signatures that you created on the blob:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

<span data-ttu-id="df40e-195">Spusťte konzolovou aplikaci a sledovat výstup zobrazíte operací, které jsou povoleny pro které podpisy.</span><span class="sxs-lookup"><span data-stu-id="df40e-195">Run the console application and observe the output to see which operations are permitted for which signatures.</span></span> <span data-ttu-id="df40e-196">Výstup v okně konzoly bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="df40e-196">The output in the console window will look similar to the following:</span></span>

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a><span data-ttu-id="df40e-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df40e-197">Next Steps</span></span>

* [<span data-ttu-id="df40e-198">Sdílené přístupové podpisy, část 1: Vysvětlení modelu SAS</span><span class="sxs-lookup"><span data-stu-id="df40e-198">Shared Access Signatures, Part 1: Understanding the SAS Model</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="df40e-199">Správa anonymního přístupu pro čtení ke kontejnerům a objektům BLOB</span><span class="sxs-lookup"><span data-stu-id="df40e-199">Manage anonymous read access to containers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="df40e-200">Delegování přístupu k pomocí sdíleného přístupového podpisu (REST API)</span><span class="sxs-lookup"><span data-stu-id="df40e-200">Delegating access with a shared access signature (REST API)</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="df40e-201">Představení tabulky a fronty SAS</span><span class="sxs-lookup"><span data-stu-id="df40e-201">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
