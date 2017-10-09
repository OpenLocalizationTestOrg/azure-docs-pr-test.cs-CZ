---
title: "aaaSet a načíst objekt vlastnosti a metadata ve službě Azure Storage | Microsoft Docs"
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
ms.openlocfilehash: 44f9243183014845964f337b476a6b0069dc0902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a><span data-ttu-id="37de4-103">Nastavení a načtení vlastností a metadat</span><span class="sxs-lookup"><span data-stu-id="37de4-103">Set and retrieve properties and metadata</span></span>

<span data-ttu-id="37de4-104">Objekty v vlastnosti podporu systému Azure Storage a metadata definovaná uživatelem, kromě toohello data, která obsahují.</span><span class="sxs-lookup"><span data-stu-id="37de4-104">Objects in Azure Storage support system properties and user-defined metadata, in addition toohello data they contain.</span></span> <span data-ttu-id="37de4-105">Tento článek popisuje správu vlastnosti systému a metadata definovaná uživatelem s hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="37de4-105">This article discusses managing system properties and user-defined metadata with hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span>

* <span data-ttu-id="37de4-106">**Vlastnosti systému**: vlastnosti systému existovat na všechny prostředky úložiště.</span><span class="sxs-lookup"><span data-stu-id="37de4-106">**System properties**: System properties exist on each storage resource.</span></span> <span data-ttu-id="37de4-107">Některé z nich můžou číst nebo nastavit, zatímco jiné jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="37de4-107">Some of them can be read or set, while others are read-only.</span></span> <span data-ttu-id="37de4-108">V části hello zahrnuje některé vlastnosti systému odpovídají toocertain standardní hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="37de4-108">Under hello covers, some system properties correspond toocertain standard HTTP headers.</span></span> <span data-ttu-id="37de4-109">Klientská knihovna pro úložiště Azure Hello udržuje to pro vás.</span><span class="sxs-lookup"><span data-stu-id="37de4-109">hello Azure storage client library maintains these for you.</span></span>

* <span data-ttu-id="37de4-110">**Metadata definovaná uživatelem**: uživatelem definované metadata jsou metadata, která jste zadali na daný prostředek v podobě hello dvojic název hodnota.</span><span class="sxs-lookup"><span data-stu-id="37de4-110">**User-defined metadata**: User-defined metadata is metadata that you specify on a given resource in hello form of a name-value pair.</span></span> <span data-ttu-id="37de4-111">Můžete vytvořit další hodnoty metadata toostore s prostředků úložiště.</span><span class="sxs-lookup"><span data-stu-id="37de4-111">You can use metadata toostore additional values with a storage resource.</span></span> <span data-ttu-id="37de4-112">Tyto hodnoty další metadata jsou vlastní pouze pro účely a neovlivní chování hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="37de4-112">These additional metadata values are for your own purposes only, and do not affect how hello resource behaves.</span></span>

<span data-ttu-id="37de4-113">Načítání hodnot vlastností a metadat pro úložiště prostředek je ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="37de4-113">Retrieving property and metadata values for a storage resource is a two-step process.</span></span> <span data-ttu-id="37de4-114">Než si můžete přečíst tyto hodnoty, můžete musí explicitně načíst je ve volání hello **FetchAttributes** metoda.</span><span class="sxs-lookup"><span data-stu-id="37de4-114">Before you can read these values, you must explicitly fetch them by calling hello **FetchAttributes** method.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37de4-115">Hodnoty vlastnosti a metadat pro úložiště prostředků nejsou naplněny, pokud je volání jednoho z hello **FetchAttributes** metody.</span><span class="sxs-lookup"><span data-stu-id="37de4-115">Property and metadata values for a storage resource are not populated unless you call one of hello **FetchAttributes** methods.</span></span>
>
> <span data-ttu-id="37de4-116">Zobrazí se `400 Bad Request` Pokud všechny dvojice název/hodnota obsahovat jiné znaky než ASCII.</span><span class="sxs-lookup"><span data-stu-id="37de4-116">You will receive a `400 Bad Request` if any name/value pairs contain non-ASCII characters.</span></span> <span data-ttu-id="37de4-117">Dvojice název/hodnota metadata jsou platné hlavičky protokolu HTTP a proto musí splňovat tooall omezení, kterými se řídí hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="37de4-117">Metadata name/value pairs are valid HTTP headers, and so must adhere tooall restrictions governing HTTP headers.</span></span> <span data-ttu-id="37de4-118">Proto se doporučuje používat kódování URL nebo pro názvy a hodnoty, který obsahuje jiné znaky než ASCII kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="37de4-118">It is therefore recommended that you use URL encoding or Base64 encoding for names and values containing non-ASCII characters.</span></span>
>

## <a name="setting-and-retrieving-properties"></a><span data-ttu-id="37de4-119">Nastavení nebo načtení vlastností</span><span class="sxs-lookup"><span data-stu-id="37de4-119">Setting and retrieving properties</span></span>
<span data-ttu-id="37de4-120">hodnoty vlastností tooretrieve, volání hello **FetchAttributes** metoda vaší objektů blob nebo kontejneru toopopulate hello vlastností, přečtěte si hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="37de4-120">tooretrieve property values, call hello **FetchAttributes** method on your blob or container toopopulate hello properties, then read hello values.</span></span>

<span data-ttu-id="37de4-121">Zadejte hodnotu vlastnosti hello tooset vlastnosti v objektu, a potom volání hello **SetProperties –** metoda.</span><span class="sxs-lookup"><span data-stu-id="37de4-121">tooset properties on an object, specify hello property value, then call hello **SetProperties** method.</span></span>

<span data-ttu-id="37de4-122">Hello následující příklad kódu vytvoří kontejner a pak zapíše některé z okna konzoly tooa hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="37de4-122">hello following code example creates a container, then writes some of its property values tooa console window.</span></span>

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create hello service client object for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a><span data-ttu-id="37de4-123">Nastavení a načítání metadat</span><span class="sxs-lookup"><span data-stu-id="37de4-123">Setting and retrieving metadata</span></span>
<span data-ttu-id="37de4-124">Metadata můžete zadat jako jeden nebo více dvojice název hodnota u objektů blob nebo kontejner prostředku.</span><span class="sxs-lookup"><span data-stu-id="37de4-124">You can specify metadata as one or more name-value pairs on a blob or container resource.</span></span> <span data-ttu-id="37de4-125">tooset metadata přidat dvojice název hodnota toohello **Metadata** kolekce na hello prostředku, pak zavolají hello **SetMetadata** metoda toosave hello hodnoty toohello služby.</span><span class="sxs-lookup"><span data-stu-id="37de4-125">tooset metadata, add name-value pairs toohello **Metadata** collection on hello resource, then call hello **SetMetadata** method toosave hello values toohello service.</span></span>

> [!NOTE]
> <span data-ttu-id="37de4-126">Název Hello metadata musí odpovídat toohello zásady vytváření názvů pro identifikátory jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="37de4-126">hello name of your metadata must conform toohello naming conventions for C# identifiers.</span></span>
>
>

<span data-ttu-id="37de4-127">Hello následující příklad kódu nastaví metadata do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="37de4-127">hello following code example sets metadata on a container.</span></span> <span data-ttu-id="37de4-128">Jedna hodnota je nastavena pomocí kolekce hello **přidat** metoda.</span><span class="sxs-lookup"><span data-stu-id="37de4-128">One value is set using hello collection's **Add** method.</span></span> <span data-ttu-id="37de4-129">Hello jiná hodnota je nastavena pomocí syntaxe implicitní klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="37de4-129">hello other value is set using implicit key/value syntax.</span></span> <span data-ttu-id="37de4-130">Obě jsou platné.</span><span class="sxs-lookup"><span data-stu-id="37de4-130">Both are valid.</span></span>

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata toohello container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set hello container's metadata.
    container.SetMetadata();
}
```

<span data-ttu-id="37de4-131">metadata tooretrieve, volání hello **FetchAttributes** metoda na vaše objektů blob nebo kontejneru hello toopopulate **Metadata** kolekce, pak přečte hello hodnoty, jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="37de4-131">tooretrieve metadata, call hello **FetchAttributes** method on your blob or container toopopulate hello **Metadata** collection, then read hello values, as shown in hello example below.</span></span>

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order toopopulate hello container's properties and metadata.
    container.FetchAttributes();

    //Enumerate hello container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="37de4-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37de4-132">Next steps</span></span>
* [<span data-ttu-id="37de4-133">Klientská knihovna pro Azure Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="37de4-133">Azure Storage Client Library for .NET reference</span></span>](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [<span data-ttu-id="37de4-134">Azure Klientská knihovna pro úložiště pro balíček NuGet pro rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="37de4-134">Azure Storage Client Library for .NET NuGet package</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
