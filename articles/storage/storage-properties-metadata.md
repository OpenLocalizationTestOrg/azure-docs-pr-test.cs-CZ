---
title: "Nastavit a načíst vlastnosti objektu a metadata ve službě Azure Storage | Microsoft Docs"
description: "Ukládat vlastní metadata pro objekty v Azure Storage a nastavit a načíst vlastnosti systému."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: 6af66607478c58874f00bcf017a35abfc37888df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a><span data-ttu-id="e615e-103">Nastavení a načtení vlastností a metadat</span><span class="sxs-lookup"><span data-stu-id="e615e-103">Set and retrieve properties and metadata</span></span>

<span data-ttu-id="e615e-104">Objekty v vlastnosti podporu systému Azure Storage a metadata definovaná uživatelem, kromě dat, která obsahují.</span><span class="sxs-lookup"><span data-stu-id="e615e-104">Objects in Azure Storage support system properties and user-defined metadata, in addition to the data they contain.</span></span> <span data-ttu-id="e615e-105">Tento článek popisuje správu vlastnosti systému a metadata definovaná uživatelem s [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="e615e-105">This article discusses managing system properties and user-defined metadata with the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span>

* <span data-ttu-id="e615e-106">**Vlastnosti systému**: vlastnosti systému existovat na všechny prostředky úložiště.</span><span class="sxs-lookup"><span data-stu-id="e615e-106">**System properties**: System properties exist on each storage resource.</span></span> <span data-ttu-id="e615e-107">Některé z nich můžou číst nebo nastavit, zatímco jiné jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e615e-107">Some of them can be read or set, while others are read-only.</span></span> <span data-ttu-id="e615e-108">Některé vlastnosti systému skrytě, odpovídají určité standardní hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e615e-108">Under the covers, some system properties correspond to certain standard HTTP headers.</span></span> <span data-ttu-id="e615e-109">Klientská knihovna pro úložiště Azure udržuje to pro vás.</span><span class="sxs-lookup"><span data-stu-id="e615e-109">The Azure storage client library maintains these for you.</span></span>

* <span data-ttu-id="e615e-110">**Metadata definovaná uživatelem**: uživatelem definované metadata jsou metadata, která jste zadali na daný prostředek ve formě dvojice název hodnota.</span><span class="sxs-lookup"><span data-stu-id="e615e-110">**User-defined metadata**: User-defined metadata is metadata that you specify on a given resource in the form of a name-value pair.</span></span> <span data-ttu-id="e615e-111">Metadata můžete použít k uložení další hodnoty s prostředků úložiště.</span><span class="sxs-lookup"><span data-stu-id="e615e-111">You can use metadata to store additional values with a storage resource.</span></span> <span data-ttu-id="e615e-112">Tyto hodnoty další metadata jsou vlastní pouze pro účely a neovlivní chování prostředku.</span><span class="sxs-lookup"><span data-stu-id="e615e-112">These additional metadata values are for your own purposes only, and do not affect how the resource behaves.</span></span>

<span data-ttu-id="e615e-113">Načítání hodnot vlastností a metadat pro úložiště prostředek je ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="e615e-113">Retrieving property and metadata values for a storage resource is a two-step process.</span></span> <span data-ttu-id="e615e-114">Než si můžete přečíst tyto hodnoty, můžete musí explicitně načíst je voláním **FetchAttributes** metoda.</span><span class="sxs-lookup"><span data-stu-id="e615e-114">Before you can read these values, you must explicitly fetch them by calling the **FetchAttributes** method.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e615e-115">Hodnoty vlastnosti a metadat pro úložiště prostředků nejsou naplněny, pokud je volání jednoho z **FetchAttributes** metody.</span><span class="sxs-lookup"><span data-stu-id="e615e-115">Property and metadata values for a storage resource are not populated unless you call one of the **FetchAttributes** methods.</span></span>
>
> <span data-ttu-id="e615e-116">Zobrazí se `400 Bad Request` Pokud všechny dvojice název/hodnota obsahovat jiné znaky než ASCII.</span><span class="sxs-lookup"><span data-stu-id="e615e-116">You will receive a `400 Bad Request` if any name/value pairs contain non-ASCII characters.</span></span> <span data-ttu-id="e615e-117">Dvojice název/hodnota metadat jsou platné hlavičky protokolu HTTP a proto musí splňovat všechny omezení, kterými se řídí hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e615e-117">Metadata name/value pairs are valid HTTP headers, and so must adhere to all restrictions governing HTTP headers.</span></span> <span data-ttu-id="e615e-118">Proto se doporučuje používat kódování URL nebo pro názvy a hodnoty, který obsahuje jiné znaky než ASCII kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="e615e-118">It is therefore recommended that you use URL encoding or Base64 encoding for names and values containing non-ASCII characters.</span></span>
>

## <a name="setting-and-retrieving-properties"></a><span data-ttu-id="e615e-119">Nastavení nebo načtení vlastností</span><span class="sxs-lookup"><span data-stu-id="e615e-119">Setting and retrieving properties</span></span>
<span data-ttu-id="e615e-120">Chcete-li k získávání hodnot vlastností, zavolejte **FetchAttributes** metodu objektu blob nebo kontejneru k naplnění vlastností, přečtěte si hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e615e-120">To retrieve property values, call the **FetchAttributes** method on your blob or container to populate the properties, then read the values.</span></span>

<span data-ttu-id="e615e-121">Nastavení vlastností pro objekt, určete vlastnost hodnotu a pak volání **SetProperties –** metoda.</span><span class="sxs-lookup"><span data-stu-id="e615e-121">To set properties on an object, specify the property value, then call the **SetProperties** method.</span></span>

<span data-ttu-id="e615e-122">Následující příklad kódu vytvoří kontejner a některé jeho vlastnosti hodnoty zapisuje do okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="e615e-122">The following code example creates a container, then writes some of its property values to a console window.</span></span>

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a><span data-ttu-id="e615e-123">Nastavení a načítání metadat</span><span class="sxs-lookup"><span data-stu-id="e615e-123">Setting and retrieving metadata</span></span>
<span data-ttu-id="e615e-124">Metadata můžete zadat jako jeden nebo více dvojice název hodnota u objektů blob nebo kontejner prostředku.</span><span class="sxs-lookup"><span data-stu-id="e615e-124">You can specify metadata as one or more name-value pairs on a blob or container resource.</span></span> <span data-ttu-id="e615e-125">K nastavení metadat, přidat dvojice název hodnota k **Metadata** kolekce k prostředku, pak volání **SetMetadata** metody uložte hodnoty ke službě.</span><span class="sxs-lookup"><span data-stu-id="e615e-125">To set metadata, add name-value pairs to the **Metadata** collection on the resource, then call the **SetMetadata** method to save the values to the service.</span></span>

> [!NOTE]
> <span data-ttu-id="e615e-126">Zásady vytváření názvů pro identifikátory jazyka C# musí odpovídat názvu metadata.</span><span class="sxs-lookup"><span data-stu-id="e615e-126">The name of your metadata must conform to the naming conventions for C# identifiers.</span></span>
>
>

<span data-ttu-id="e615e-127">Následující příklad kódu nastaví metadata do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e615e-127">The following code example sets metadata on a container.</span></span> <span data-ttu-id="e615e-128">Jedna hodnota se nastavuje pomocí kolekce **přidat** metoda.</span><span class="sxs-lookup"><span data-stu-id="e615e-128">One value is set using the collection's **Add** method.</span></span> <span data-ttu-id="e615e-129">Další hodnota je nastavena pomocí syntaxe implicitní klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="e615e-129">The other value is set using implicit key/value syntax.</span></span> <span data-ttu-id="e615e-130">Obě jsou platné.</span><span class="sxs-lookup"><span data-stu-id="e615e-130">Both are valid.</span></span>

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set the container's metadata.
    container.SetMetadata();
}
```

<span data-ttu-id="e615e-131">K načtení metadat, volání **FetchAttributes** metodu objektu blob nebo kontejneru k naplnění **Metadata** kolekce, pak číst hodnoty, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e615e-131">To retrieve metadata, call the **FetchAttributes** method on your blob or container to populate the **Metadata** collection, then read the values, as shown in the example below.</span></span>

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order to populate the container's properties and metadata.
    container.FetchAttributes();

    //Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="e615e-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e615e-132">Next steps</span></span>
* [<span data-ttu-id="e615e-133">Klientská knihovna pro Azure Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="e615e-133">Azure Storage Client Library for .NET reference</span></span>](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [<span data-ttu-id="e615e-134">Azure Klientská knihovna pro úložiště pro balíček NuGet pro rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e615e-134">Azure Storage Client Library for .NET NuGet package</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
