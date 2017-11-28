---
title: "aaaCreate a používání sdíleného přístupového podpisu (SAS) s Azure Blob storage | Microsoft Docs"
description: "Tento kurz ukazuje, jak toocreate sdílené přístupové podpisy pro použití s úložištěm Blob a jak tooconsume je v klientských aplikacích."
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
ms.openlocfilehash: 32004d7d29a190a7ed7234513428c3c156b833b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a><span data-ttu-id="1ddca-103">Sdílené přístupové podpisy, část 2: Vytvoření a použití SAS s úložištěm Blob</span><span class="sxs-lookup"><span data-stu-id="1ddca-103">Shared Access Signatures, Part 2: Create and use a SAS with Blob storage</span></span>

<span data-ttu-id="1ddca-104">[Část 1](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) tohoto kurzu prozkoumali sdílené přístupové podpisy (SAS) a vysvětlení osvědčené postupy pro jejich použití.</span><span class="sxs-lookup"><span data-stu-id="1ddca-104">[Part 1](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) of this tutorial explored shared access signatures (SAS) and explained best practices for using them.</span></span> <span data-ttu-id="1ddca-105">Část 2 ukazuje, jak toogenerate a pak použít sdílené přístupové podpisy pomocí úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1ddca-105">Part 2 shows you how toogenerate and then use shared access signatures with Blob storage.</span></span> <span data-ttu-id="1ddca-106">Příklady Hello jsou napsané v C# a používají hello Klientská knihovna pro úložiště Azure pro .NET.</span><span class="sxs-lookup"><span data-stu-id="1ddca-106">hello examples are written in C# and use hello Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="1ddca-107">Hello příklady v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="1ddca-107">hello examples in this tutorial:</span></span>

* <span data-ttu-id="1ddca-108">Vygenerovat sdílený přístupový podpis do kontejneru</span><span class="sxs-lookup"><span data-stu-id="1ddca-108">Generate a shared access signature on a container</span></span>
* <span data-ttu-id="1ddca-109">Vygenerovat sdílený přístupový podpis na objekt blob</span><span class="sxs-lookup"><span data-stu-id="1ddca-109">Generate a shared access signature on a blob</span></span>
* <span data-ttu-id="1ddca-110">Vytvořit uložené přístup podpisy toomanage zásad na prostředky kontejneru</span><span class="sxs-lookup"><span data-stu-id="1ddca-110">Create a stored access policy toomanage signatures on a container's resources</span></span>
* <span data-ttu-id="1ddca-111">Testování hello sdílené přístupové podpisy v aplikaci klienta</span><span class="sxs-lookup"><span data-stu-id="1ddca-111">Test hello shared access signatures in a client application</span></span>

## <a name="about-this-tutorial"></a><span data-ttu-id="1ddca-112">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="1ddca-112">About this tutorial</span></span>
<span data-ttu-id="1ddca-113">V tomto kurzu vytvoříme dvě konzolové aplikace, které ukazují, vytvoření a použití sdílených přístupových podpisů pro kontejnery a objekty BLOB:</span><span class="sxs-lookup"><span data-stu-id="1ddca-113">In this tutorial, we create two console applications that demonstrate creating and using shared access signatures for containers and blobs:</span></span>

<span data-ttu-id="1ddca-114">**Aplikaci 1**: hello aplikace pro správu.</span><span class="sxs-lookup"><span data-stu-id="1ddca-114">**Application 1**: hello management application.</span></span> <span data-ttu-id="1ddca-115">Vygeneruje sdílený přístupový podpis kontejneru a objekt blob.</span><span class="sxs-lookup"><span data-stu-id="1ddca-115">Generates a shared access signature for a container and a blob.</span></span> <span data-ttu-id="1ddca-116">Zahrnuje přístupový klíč účtu úložiště hello ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="1ddca-116">Includes hello storage account access key in source code.</span></span>

<span data-ttu-id="1ddca-117">**Aplikace 2**: hello klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ddca-117">**Application 2**: hello client application.</span></span> <span data-ttu-id="1ddca-118">Přístupy kontejnerů a objektů blob prostředků pomocí hello sdílené přístupové podpisy vytvoření první aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-118">Accesses container and blob resources using hello shared access signatures created with hello first application.</span></span> <span data-ttu-id="1ddca-119">Používá pouze hello sdílené přístupové podpisy tooaccess kontejneru a prostředků blob – nemá *není* zahrnují hello přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1ddca-119">Uses only hello shared access signatures tooaccess container and blob resources--it does *not* include hello storage account access key.</span></span>

## <a name="part-1-create-a-console-application-toogenerate-shared-access-signatures"></a><span data-ttu-id="1ddca-120">Část 1: Vytvoření přístup ke konzole aplikace toogenerate sdílené podpisy</span><span class="sxs-lookup"><span data-stu-id="1ddca-120">Part 1: Create a console application toogenerate shared access signatures</span></span>
<span data-ttu-id="1ddca-121">Nejprve je třeba splnit hello Klientská knihovna pro úložiště Azure pro .NET nainstalované.</span><span class="sxs-lookup"><span data-stu-id="1ddca-121">First, ensure that you have hello Azure Storage Client Library for .NET installed.</span></span> <span data-ttu-id="1ddca-122">Můžete nainstalovat hello [balíček NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "balíček NuGet") obsahující hello nejaktuálnější sestavení pro knihovny klienta hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-122">You can install hello [NuGet package](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet package") containing hello most up-to-date assemblies for hello client library.</span></span> <span data-ttu-id="1ddca-123">Toto je doporučená metoda pro zajištění, že máte nejnovější opravy hello hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-123">This is hello recommended method for ensuring that you have hello most recent fixes.</span></span> <span data-ttu-id="1ddca-124">Klientská knihovna pro hello můžete také stáhnout jako součást hello nejnovější verzi hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="1ddca-124">You can also download hello client library as part of hello most recent version of hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="1ddca-125">V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **GenerateSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="1ddca-125">In Visual Studio, create a new Windows console application and name it **GenerateSharedAccessSignatures**.</span></span> <span data-ttu-id="1ddca-126">Přidejte odkazy na příliš[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) a [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) pomocí jedné z následujících postupů hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-126">Add references too[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) by using one of hello following approaches:</span></span>

* <span data-ttu-id="1ddca-127">Použití hello [Správce balíčků NuGet](https://docs.nuget.org/consume/installing-nuget) v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ddca-127">Use hello [NuGet package manager](https://docs.nuget.org/consume/installing-nuget) in Visual Studio.</span></span> <span data-ttu-id="1ddca-128">Vyberte **projektu** > **spravovat balíčky NuGet**, vyhledejte online každý balíček (Microsoft.WindowsAzure.ConfigurationManager a WindowsAzure.Storage) a instalovat je.</span><span class="sxs-lookup"><span data-stu-id="1ddca-128">Select **Project** > **Manage NuGet Packages**, search online for each package (Microsoft.WindowsAzure.ConfigurationManager and WindowsAzure.Storage) and install them.</span></span>
* <span data-ttu-id="1ddca-129">Můžete taky najít tyto sestavení v instalaci softwaru hello Azure SDK a přidejte toothem odkazy:</span><span class="sxs-lookup"><span data-stu-id="1ddca-129">Alternatively, locate these assemblies in your installation of hello Azure SDK and add references toothem:</span></span>
  * <span data-ttu-id="1ddca-130">Microsoft.WindowsAzure.Configuration.dll</span><span class="sxs-lookup"><span data-stu-id="1ddca-130">Microsoft.WindowsAzure.Configuration.dll</span></span>
  * <span data-ttu-id="1ddca-131">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="1ddca-131">Microsoft.WindowsAzure.Storage.dll</span></span>

<span data-ttu-id="1ddca-132">Hello horní části souboru Program.cs hello, přidejte následující hello **pomocí** direktivy:</span><span class="sxs-lookup"><span data-stu-id="1ddca-132">At hello top of hello Program.cs file, add hello following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="1ddca-133">Upravte soubor app.config hello, tak, aby obsahoval nastavení konfigurace s připojovacím řetězcem, který odkazuje tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1ddca-133">Edit hello app.config file so that it contains a configuration setting with a connection string that points tooyour storage account.</span></span> <span data-ttu-id="1ddca-134">Váš soubor app.config by měl vypadat podobně jako toothis jeden:</span><span class="sxs-lookup"><span data-stu-id="1ddca-134">Your app.config file should look similar toothis one:</span></span>

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

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a><span data-ttu-id="1ddca-135">Vygenerovat sdílený přístupový podpis identifikátor URI pro kontejner</span><span class="sxs-lookup"><span data-stu-id="1ddca-135">Generate a shared access signature URI for a container</span></span>
<span data-ttu-id="1ddca-136">toobegin s přidáme metoda toogenerate sdílený přístupový podpis na nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="1ddca-136">toobegin with, we add a method toogenerate a shared access signature on a new container.</span></span> <span data-ttu-id="1ddca-137">V takovém případě hello podpis není přidružený k zásadě uložené přístup, takže vykonává hello URI hello informace o tom jeho oprávnění hello a čas vypršení platnosti uděluje.</span><span class="sxs-lookup"><span data-stu-id="1ddca-137">In this case, hello signature is not associated with a stored access policy, so it carries on hello URI hello information indicating its expiry time and hello permissions it grants.</span></span>

<span data-ttu-id="1ddca-138">Nejprve přidejte kód toohello **Main()** metoda tooauthenticate přístup k účtu úložiště tooyour a vytvořit nový kontejner:</span><span class="sxs-lookup"><span data-stu-id="1ddca-138">First, add code toohello **Main()** method tooauthenticate access tooyour storage account and create a new container:</span></span>

```csharp
static void Main(string[] args)
{
    //Parse hello connection string and return a reference toohello storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create hello blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference tooa container toouse for hello sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls toohello methods created below here...

    //Require user input before closing hello console window.
    Console.ReadLine();
}
```

<span data-ttu-id="1ddca-139">V dalším kroku přidejte metodu, která generuje hello sdílený přístupový podpis kontejneru hello a vrátí podpis hello URI:</span><span class="sxs-lookup"><span data-stu-id="1ddca-139">Next, add a method that generates hello shared access signature for hello container and returns hello signature URI:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set hello expiry time and permissions for hello container.
    //In this case no start time is specified, so hello shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="1ddca-140">Přidejte následující řádky na konci hello hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, toocall **GetContainerSasUri()** a zápis hello okno konzoly URI toohello podpis:</span><span class="sxs-lookup"><span data-stu-id="1ddca-140">Add hello following lines at hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, toocall **GetContainerSasUri()** and write hello signature URI toohello console window:</span></span>

```csharp
//Generate a SAS URI for hello container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="1ddca-141">Zkompilování a spuštění toooutput hello sdílený přístupový podpis identifikátor URI pro nový kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-141">Compile and run toooutput hello shared access signature URI for hello new container.</span></span> <span data-ttu-id="1ddca-142">Hello URI bude podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="1ddca-142">hello URI will be similar toohello following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

<span data-ttu-id="1ddca-143">Když spustíte hello kód hello sdíleného přístupového podpisu, který jste vytvořili pro kontejner hello bude platit pro hello dalších 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="1ddca-143">Once you have run hello code, hello shared access signature you created for hello container will be valid for hello next 24 hours.</span></span> <span data-ttu-id="1ddca-144">podpis Hello uděluje oprávnění toolist objekty BLOB v kontejneru hello a toowrite nový kontejner objektů BLOB toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="1ddca-144">hello signature grants a client permission toolist blobs in hello container and toowrite new blobs toohello container.</span></span>

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a><span data-ttu-id="1ddca-145">Vygenerovat sdílený přístupový podpis identifikátor URI pro objekt blob</span><span class="sxs-lookup"><span data-stu-id="1ddca-145">Generate a shared access signature URI for a blob</span></span>
<span data-ttu-id="1ddca-146">V dalším kroku zápisu podobné toocreate kód nové objektů blob v kontejneru hello a vygenerovat sdílený přístupový podpis pro ni.</span><span class="sxs-lookup"><span data-stu-id="1ddca-146">Next, we write similar code toocreate a new blob within hello container and generate a shared access signature for it.</span></span> <span data-ttu-id="1ddca-147">Tento sdílený přístupový podpis není přidružený k zásadě uložené přístup tak, aby zahrnovala hello počáteční čas, čas vypršení platnosti a informace o oprávněních v hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="1ddca-147">This shared access signature is not associated with a stored access policy, so it includes hello start time, expiry time, and permission information in hello URI.</span></span>

<span data-ttu-id="1ddca-148">Přidejte novou metodu, která vytvoří nový objekt blob a zapíše některé tooit text a vygeneruje sdílený přístupový podpis a vrátí podpis hello URI:</span><span class="sxs-lookup"><span data-stu-id="1ddca-148">Add a new method that creates a new blob and writes some text tooit, then generates a shared access signature and returns hello signature URI:</span></span>

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference tooa blob within hello container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text toohello blob. If hello blob does not yet exist, it will be created.
    //If hello blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible tooclients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set hello expiry time and permissions for hello blob.
    //In this case, hello start time is specified as a few minutes in hello past, toomitigate clock skew.
    //hello shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate hello shared access signature on hello blob, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="1ddca-149">Na konci hello hello **Main()** metoda, přidejte následující řádky toocall hello **GetBlobSasUri()**, než hello volat příliš**Console.ReadLine()**a zápis hello sdílet okno pro přístup k podpisu URI toohello konzoly:</span><span class="sxs-lookup"><span data-stu-id="1ddca-149">At hello bottom of hello **Main()** method, add hello following lines toocall **GetBlobSasUri()**, before hello call too**Console.ReadLine()**, and write hello shared access signature URI toohello console window:</span></span>

```csharp
//Generate a SAS URI for a blob within hello container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="1ddca-150">Zkompilování a spuštění toooutput hello sdílený přístupový podpis identifikátor URI pro nový objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-150">Compile and run toooutput hello shared access signature URI for hello new blob.</span></span> <span data-ttu-id="1ddca-151">Hello URI bude podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="1ddca-151">hello URI will be similar toohello following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-hello-container"></a><span data-ttu-id="1ddca-152">Vytvoření zásady přístupu uložené na kontejneru hello</span><span class="sxs-lookup"><span data-stu-id="1ddca-152">Create a stored access policy on hello container</span></span>
<span data-ttu-id="1ddca-153">Nyní Pojďme vytvořit zásadu přístupu uložené na hello kontejneru, který bude definování hello omezení pro všechny sdílené přístupové podpisy, které jsou k ní přidružena.</span><span class="sxs-lookup"><span data-stu-id="1ddca-153">Now let's create a stored access policy on hello container, which will define hello constraints for any shared access signatures that are associated with it.</span></span>

<span data-ttu-id="1ddca-154">V předchozích příkladech hello jsme zadali hello čas spuštění (implicitně nebo explicitně), čas vypršení platnosti hello a hello oprávnění hello sdílený přístupový podpis URI sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1ddca-154">In hello previous examples, we specified hello start time (implicitly or explicitly), hello expiry time, and hello permissions on hello shared access signature URI itself.</span></span> <span data-ttu-id="1ddca-155">V následující příklady hello určíme tyto zásady přístupu hello uložené, nikoli hello sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="1ddca-155">In hello following examples, we specify these on hello stored access policy, not on hello shared access signature.</span></span> <span data-ttu-id="1ddca-156">Díky tomu tak umožňuje nám toochange těchto omezení bez opětovného vydání hello sdílený přístup k podpisu.</span><span class="sxs-lookup"><span data-stu-id="1ddca-156">Doing so enables us toochange these constraints without reissuing hello shared access signature.</span></span>

<span data-ttu-id="1ddca-157">Je možné toohave jeden nebo více hello omezení hello sdílený přístupový podpis a zbývající hello v zásadách přístupu hello uložené.</span><span class="sxs-lookup"><span data-stu-id="1ddca-157">It's possible toohave one or more of hello constraints on hello shared access signature, and hello remainder on hello stored access policy.</span></span> <span data-ttu-id="1ddca-158">Ale pouze můžete hello počáteční čas, čas vypršení platnosti a oprávnění v jednom místě nebo hello jiné.</span><span class="sxs-lookup"><span data-stu-id="1ddca-158">However, you can only specify hello start time, expiry time, and permissions in one place or hello other.</span></span> <span data-ttu-id="1ddca-159">Například nelze nastavit oprávnění na hello sdílený přístupový podpis a také zadejte je v zásadách přístupu hello uložené.</span><span class="sxs-lookup"><span data-stu-id="1ddca-159">For example, you can't specify permissions on hello shared access signature and also specify them on hello stored access policy.</span></span>

<span data-ttu-id="1ddca-160">Když přidáte tooa kontejner zásad uložené přístup, musí získat hello kontejneru existující oprávnění, přidat nové zásady přístupu hello a potom nastavte hello kontejneru oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1ddca-160">When you add a stored access policy tooa container, you must get hello container's existing permissions, add hello new access policy, and then set hello container's permissions.</span></span>

<span data-ttu-id="1ddca-161">Přidejte novou metodu, která vytvoří novou zásadu uložené přístup do kontejneru a vrátí hello název zásady hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-161">Add a new method that creates a new stored access policy on a container and returns hello name of hello policy:</span></span>

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get hello container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add hello new policy toohello container's permissions, and set hello container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

<span data-ttu-id="1ddca-162">Na konci hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, přidejte následující hello řádků toofirst vymazat všechny existující zásady přístupu a pak zavolají hello  **CreateSharedAccessPolicy()** metoda:</span><span class="sxs-lookup"><span data-stu-id="1ddca-162">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toofirst clear any existing access policies, and then call hello **CreateSharedAccessPolicy()** method:</span></span>

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on hello container, which may be optionally used tooprovide constraints for
//shared access signatures on hello container and hello blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

<span data-ttu-id="1ddca-163">Když zrušíte zaškrtnutí hello zásady přístupu do kontejneru, je nutné nejprve získat hello kontejneru existující oprávnění a potom zrušte hello oprávnění a potom znovu nastavit oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-163">When you clear hello access policies on a container, you must first get hello container's existing permissions, then clear hello permissions, then set hello permissions again.</span></span>

### <a name="generate-a-shared-access-signature-uri-on-hello-container-that-uses-an-access-policy"></a><span data-ttu-id="1ddca-164">Vygenerovat sdílený přístupový podpis URI hello kontejneru, který používá zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="1ddca-164">Generate a shared access signature URI on hello container that uses an access policy</span></span>
<span data-ttu-id="1ddca-165">V dalším kroku vytvoříme jiný sdílený přístupový podpis kontejneru hello, kterou jsme vytvořili předtím, ale momentálně jsme přidružit hello podpis hello uložené zásady přístupu, které jsme vytvořili v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-165">Next, we create another shared access signature for hello container that we created earlier, but this time we associate hello signature with hello stored access policy we created in hello previous example.</span></span>

<span data-ttu-id="1ddca-166">Přidejte nové toogenerate metoda jiný sdílený přístupový podpis kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-166">Add a new method toogenerate another shared access signature for hello container:</span></span>

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate hello shared access signature on hello container. In this case, all of hello constraints for the
    //shared access signature are specified on hello stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="1ddca-167">Na konci hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, přidejte následující řádky toocall hello hello **GetContainerSasUriWithPolicy** – metoda :</span><span class="sxs-lookup"><span data-stu-id="1ddca-167">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toocall hello **GetContainerSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-hello-blob-that-uses-an-access-policy"></a><span data-ttu-id="1ddca-168">Umožňuje vygenerovat URI podpis sdíleného přístupu na hello objektů Blob, aby používal zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="1ddca-168">Generate a Shared Access Signature URI on hello Blob That Uses an Access Policy</span></span>
<span data-ttu-id="1ddca-169">Nakonec přidejte podobné toocreate metoda jiný objekt blob a vygenerovat sdílený přístupový podpis, který má přidružené k zásadě uložené přístup.</span><span class="sxs-lookup"><span data-stu-id="1ddca-169">Finally, we add a similar method toocreate another blob and generate a shared access signature that's associated with a stored access policy.</span></span>

<span data-ttu-id="1ddca-170">Přidejte nové toocreate metoda objektu blob a vygenerovat sdílený přístupový podpis:</span><span class="sxs-lookup"><span data-stu-id="1ddca-170">Add a new method toocreate a blob and generate a shared access signature:</span></span>

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference tooa blob within hello container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text toohello blob. If hello blob does not yet exist, it will be created.
    //If hello blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible tooclients via a shared access signature. " +
    "A stored access policy defines hello constraints for hello signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate hello shared access signature on hello blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="1ddca-171">Na konci hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, přidejte následující řádky toocall hello hello **GetBlobSasUriWithPolicy** metoda:</span><span class="sxs-lookup"><span data-stu-id="1ddca-171">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toocall hello **GetBlobSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

<span data-ttu-id="1ddca-172">Hello **Main()** metoda by měl nyní vypadat jako v celé jeho šíři.</span><span class="sxs-lookup"><span data-stu-id="1ddca-172">hello **Main()** method should now look like this in its entirety.</span></span> <span data-ttu-id="1ddca-173">Spustit toowrite hello sdílený přístupový podpis okna konzoly toohello identifikátory URI, pak zkopírujte a vložte je do textového souboru pro použití v hello druhé části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1ddca-173">Run it toowrite hello shared access signature URIs toohello console window, then copy and paste them into a text file for use in hello second part of this tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    //Parse hello connection string and return a reference toohello storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create hello blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference tooa container toouse for hello sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for hello container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within hello container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on hello container, which may be optionally used tooprovide constraints for
    //shared access signatures on hello container and hello blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

<span data-ttu-id="1ddca-174">Když spustíte hello GenerateSharedAccessSignatures konzolovou aplikaci, uvidíte výstup podobný toohello následující.</span><span class="sxs-lookup"><span data-stu-id="1ddca-174">When you run hello GenerateSharedAccessSignatures console application, you'll see output similar toohello following.</span></span> <span data-ttu-id="1ddca-175">Jedná se o hello sdílené přístupové podpisy, které můžete použít v rámci 2 hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="1ddca-175">These are hello shared access signatures you use in Part 2 of hello tutorial.</span></span>

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-tootest-hello-shared-access-signatures"></a><span data-ttu-id="1ddca-176">Část 2: Vytvoření tootest hello sdílený přístup z konzole aplikace podpisů</span><span class="sxs-lookup"><span data-stu-id="1ddca-176">Part 2: Create a console application tootest hello shared access signatures</span></span>
<span data-ttu-id="1ddca-177">tootest hello sdílené přístupové podpisy vytvořili v předchozích příkladech hello, vytvoříme druhý konzolovou aplikaci, která používá hello podpisy tooperform operací na hello kontejneru a na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="1ddca-177">tootest hello shared access signatures created in hello previous examples, we create a second console application that uses hello signatures tooperform operations on hello container and on a blob.</span></span>

> [!NOTE]
> <span data-ttu-id="1ddca-178">Pokud víc než 24 hodin předané vzhledem k tomu, že jste dokončili první část kurzu hello hello, hello podpisy, které jste vygenerovali již nebude platný.</span><span class="sxs-lookup"><span data-stu-id="1ddca-178">If more than 24 hours have passed since you completed hello first part of hello tutorial, hello signatures you generated will no longer be valid.</span></span> <span data-ttu-id="1ddca-179">V takovém případě byste měli spustit hello kódu v hello první konzole aplikace toogenerate čerstvé sdílené přístupové podpisy pro použití v druhé části kurzu hello hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-179">In this case, you should run hello code in hello first console application toogenerate fresh shared access signatures for use in hello second part of hello tutorial.</span></span>
>

<span data-ttu-id="1ddca-180">V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **ConsumeSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="1ddca-180">In Visual Studio, create a new Windows console application and name it **ConsumeSharedAccessSignatures**.</span></span> <span data-ttu-id="1ddca-181">Přidejte odkazy na příliš[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) a [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), protože jste provedli dříve.</span><span class="sxs-lookup"><span data-stu-id="1ddca-181">Add references too[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), as you did previously.</span></span>

<span data-ttu-id="1ddca-182">Hello horní části souboru Program.cs hello, přidejte následující hello **pomocí** direktivy:</span><span class="sxs-lookup"><span data-stu-id="1ddca-182">At hello top of hello Program.cs file, add hello following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="1ddca-183">V textu hello hello **Main()** metody přidat hello následující řetězcové konstanty, změna jejich hodnoty toohello sdílené přístupové podpisy generované v části 1 hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="1ddca-183">In hello body of hello **Main()** method, add hello following string constants, changing their values toohello shared access signatures you generated in part 1 of hello tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-tootry-container-operations-using-a-shared-access-signature"></a><span data-ttu-id="1ddca-184">Přidání operací metoda tootry kontejneru pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="1ddca-184">Add a method tootry container operations using a shared access signature</span></span>
<span data-ttu-id="1ddca-185">Potom přidáme metodu, která se testuje některé operace kontejneru pomocí sdíleného přístupového podpisu kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-185">Next, we add a method that tests some container operations using a shared access signature for hello container.</span></span> <span data-ttu-id="1ddca-186">sdílený přístupový podpis Hello je použité tooreturn kontejner toohello odkaz ověřování kontejneru toohello přístup podle hello podpis samostatně.</span><span class="sxs-lookup"><span data-stu-id="1ddca-186">hello shared access signature is used tooreturn a reference toohello container, authenticating access toohello container based on hello signature alone.</span></span>

<span data-ttu-id="1ddca-187">Přidejte následující metodu tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-187">Add hello following method tooProgram.cs:</span></span>

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with hello SAS provided.

    //Return a reference toohello container using hello SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list toostore blob URIs returned by a listing operation on hello container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob toohello container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions toohello container. ";
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

    //List operation: List hello blobs in hello container.
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

    //Read operation: Get a reference tooone of hello blobs in hello container and read it.
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

    //Delete operation: Delete a blob in hello container.
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

<span data-ttu-id="1ddca-188">Aktualizace hello **Main()** metoda toocall **UseContainerSAS()** s oběma hello sdílené přístupové podpisy, které jste vytvořili v kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-188">Update hello **Main()** method toocall **UseContainerSAS()** with both of hello shared access signatures you created on hello container:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call hello test methods with hello shared access signatures created on hello container, with and without hello access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-tootry-blob-operations-using-a-shared-access-signature"></a><span data-ttu-id="1ddca-189">Přidání operací metoda tootry blob pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="1ddca-189">Add a method tootry blob operations using a shared access signature</span></span>
<span data-ttu-id="1ddca-190">Nakonec přidáme metodu, která se testuje některé operace objektů blob pomocí sdíleného přístupového podpisu u objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="1ddca-190">Finally, we add a method that tests some blob operations using a shared access signature on hello blob.</span></span> <span data-ttu-id="1ddca-191">V tomto případě používáme hello konstruktor **CloudBlockBlob(String)**a předejte hello sdílený přístupový podpis tooreturn referenční toohello objekt blob.</span><span class="sxs-lookup"><span data-stu-id="1ddca-191">In this case, we use hello constructor **CloudBlockBlob(String)**, passing in hello shared access signature, tooreturn a reference toohello blob.</span></span> <span data-ttu-id="1ddca-192">Další ověřování není vyžadováno; je založena na hello podpis samostatně.</span><span class="sxs-lookup"><span data-stu-id="1ddca-192">No other authentication is required; it's based on hello signature alone.</span></span>

<span data-ttu-id="1ddca-193">Přidejte následující metodu tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-193">Add hello following method tooProgram.cs:</span></span>

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using hello SAS provided.

    //Return a reference toohello blob using hello SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob toohello container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions toohello blob. ";
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

    //Read operation: Read hello contents of hello blob.
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

    //Delete operation: Delete hello blob.
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

<span data-ttu-id="1ddca-194">Aktualizace hello **Main()** metoda toocall **UseBlobSAS()** s oběma hello sdílené přístupové podpisy, které jste vytvořili na objekt blob hello:</span><span class="sxs-lookup"><span data-stu-id="1ddca-194">Update hello **Main()** method toocall **UseBlobSAS()** with both of hello shared access signatures that you created on hello blob:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call hello test methods with hello shared access signatures created on hello container, with and without hello access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call hello test methods with hello shared access signatures created on hello blob, with and without hello access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

<span data-ttu-id="1ddca-195">Spusťte hello konzolové aplikace a sledovat toosee výstup hello operací, které jsou povoleny pro které podpisy.</span><span class="sxs-lookup"><span data-stu-id="1ddca-195">Run hello console application and observe hello output toosee which operations are permitted for which signatures.</span></span> <span data-ttu-id="1ddca-196">výstup Hello v okně konzoly hello bude vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="1ddca-196">hello output in hello console window will look similar toohello following:</span></span>

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a><span data-ttu-id="1ddca-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ddca-197">Next Steps</span></span>

* [<span data-ttu-id="1ddca-198">Sdílené přístupové podpisy, část 1: Vysvětlení modelu SAS hello</span><span class="sxs-lookup"><span data-stu-id="1ddca-198">Shared Access Signatures, Part 1: Understanding hello SAS Model</span></span>](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="1ddca-199">Správa toocontainers anonymní přístup pro čtení a objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="1ddca-199">Manage anonymous read access toocontainers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="1ddca-200">Delegování přístupu k pomocí sdíleného přístupového podpisu (REST API)</span><span class="sxs-lookup"><span data-stu-id="1ddca-200">Delegating access with a shared access signature (REST API)</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="1ddca-201">Představení tabulky a fronty SAS</span><span class="sxs-lookup"><span data-stu-id="1ddca-201">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
