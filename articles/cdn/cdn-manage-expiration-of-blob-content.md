---
title: "Spravovat konec platnosti objektů BLOB Azure Storage v Azure CDN | Microsoft Docs"
description: "Informace o možnostech řízení time to live pro objekty BLOB v Azure CDN ukládání do mezipaměti."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a><span data-ttu-id="6a166-103">Spravovat konec platnosti objektů BLOB Azure Storage v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="6a166-103">Manage expiration of Azure Storage blobs in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a166-104">Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS</span><span class="sxs-lookup"><span data-stu-id="6a166-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="6a166-105">Služba Azure blob Storage</span><span class="sxs-lookup"><span data-stu-id="6a166-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="6a166-106">[Služba objektů blob](../storage/common/storage-introduction.md#blob-storage) v [Azure Storage](../storage/common/storage-introduction.md) je jedním z několika Azure na základě původu integrované s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="6a166-106">The [blob service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is one of several Azure-based origins integrated with Azure CDN.</span></span>  <span data-ttu-id="6a166-107">Veškerý obsah, veřejně přístupná objektů blob můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).</span><span class="sxs-lookup"><span data-stu-id="6a166-107">Any publicly accessible blob content can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="6a166-108">Hodnota TTL je dáno [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6a166-108">The TTL is determined by the [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in the HTTP response from Azure Storage.</span></span>

> [!TIP]
> <span data-ttu-id="6a166-109">Můžete nastavit žádné TTL na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="6a166-109">You may choose to set no TTL on a blob.</span></span>  <span data-ttu-id="6a166-110">V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.</span><span class="sxs-lookup"><span data-stu-id="6a166-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="6a166-111">Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k objektům BLOB a další soubory, najdete v článku [přehled CDN Azure](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a166-111">For more information about how Azure CDN works to speed up access to blobs and other files, see the [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> <span data-ttu-id="6a166-112">Další informace o objektu blob služby Azure Storage najdete v tématu [koncepty služby objektů Blob](https://msdn.microsoft.com/library/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a166-112">For more details on the Azure Storage blob service, see [Blob Service Concepts](https://msdn.microsoft.com/library/dd179376.aspx).</span></span> 
> 
> 

<span data-ttu-id="6a166-113">Tento kurz představuje několik způsobů, které můžete nastavit hodnotu TTL u objektu blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6a166-113">This tutorial demonstrates several ways that you can set the TTL on a blob in Azure Storage.</span></span>  

## <a name="azure-powershell"></a><span data-ttu-id="6a166-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a166-114">Azure PowerShell</span></span>
<span data-ttu-id="6a166-115">[Prostředí Azure PowerShell](/powershell/azure/overview) je jedním z nejrychlejší, nejúčinnějších způsobů, jak spravovat služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="6a166-115">[Azure PowerShell](/powershell/azure/overview) is one of the quickest, most powerful ways to administer your Azure services.</span></span>  <span data-ttu-id="6a166-116">Použití `Get-AzureStorageBlob` rutiny odkazovat na objekt blob, nastavte `.ICloudBlob.Properties.CacheControl` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6a166-116">Use the `Get-AzureStorageBlob` cmdlet to get a reference to the blob, then set the `.ICloudBlob.Properties.CacheControl` property.</span></span> 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> <span data-ttu-id="6a166-117">Můžete taky použít PowerShell k [spravovat koncové body a profilů CDN](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6a166-117">You can also use PowerShell to [manage your CDN profiles and endpoints](cdn-manage-powershell.md).</span></span>
> 
> 

## <a name="azure-storage-client-library-for-net"></a><span data-ttu-id="6a166-118">Klientská knihovna pro úložiště Azure pro .NET</span><span class="sxs-lookup"><span data-stu-id="6a166-118">Azure Storage Client Library for .NET</span></span>
<span data-ttu-id="6a166-119">Pokud chcete nastavit hodnotu TTL objekt blob pomocí rozhraní .NET, použijte [Klientská knihovna pro úložiště Azure pro .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) nastavit [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6a166-119">To set a blob's TTL using .NET, use the [Azure Storage Client Library for .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) to set the [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) property.</span></span>

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> <span data-ttu-id="6a166-120">Nejsou k dispozici v mnoha další ukázek kódu .NET [ukázky úložiště objektů Blob Azure pro .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="6a166-120">There are many more .NET code samples available in the [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span>
> 
> 

## <a name="other-methods"></a><span data-ttu-id="6a166-121">Ostatní metody</span><span class="sxs-lookup"><span data-stu-id="6a166-121">Other methods</span></span>
* [<span data-ttu-id="6a166-122">Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6a166-122">Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)
  
    <span data-ttu-id="6a166-123">Pokud chcete nahrát objekt blob, nastavit *cacheControl* pomocí vlastnosti `-p` přepínače.</span><span class="sxs-lookup"><span data-stu-id="6a166-123">When uploading the blob, set the *cacheControl* property using the `-p` switch.</span></span>  <span data-ttu-id="6a166-124">Tento příklad nastaví hodnotu TTL na jednu hodinu (3600 sekund).</span><span class="sxs-lookup"><span data-stu-id="6a166-124">This example sets the TTL to one hour (3600 seconds).</span></span>
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [<span data-ttu-id="6a166-125">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6a166-125">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    <span data-ttu-id="6a166-126">Explicitně nastavit *x-ms-blob-cache-control* vlastnost na [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [uvést seznam blokovaných](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), nebo [nastavit vlastnosti objektu Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) požadavku.</span><span class="sxs-lookup"><span data-stu-id="6a166-126">Explicitly set the *x-ms-blob-cache-control* property on a [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), or [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) request.</span></span>
* <span data-ttu-id="6a166-127">Nástroje pro správu úložiště třetí strany.</span><span class="sxs-lookup"><span data-stu-id="6a166-127">Third-party storage management tools</span></span>
  
    <span data-ttu-id="6a166-128">Některé nástroje pro správu Azure úložiště jiných výrobců povolit nastavení *CacheControl* vlastnost na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="6a166-128">Some third-party Azure Storage management tools allow you to set the *CacheControl* property on blobs.</span></span> 

## <a name="testing-the-cache-control-header"></a><span data-ttu-id="6a166-129">Testování *Cache-Control* záhlaví</span><span class="sxs-lookup"><span data-stu-id="6a166-129">Testing the *Cache-Control* header</span></span>
<span data-ttu-id="6a166-130">Snadno můžete ověřit hodnoty TTL objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="6a166-130">You can easily verify the TTL of your blobs.</span></span>  <span data-ttu-id="6a166-131">Pomocí prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, který je včetně objektu blob služby *Cache-Control* hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6a166-131">Using your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob is including the *Cache-Control* response header.</span></span>  <span data-ttu-id="6a166-132">Můžete použít také nástroje, jako je **wget**, [Postman](https://www.getpostman.com/), nebo [Fiddler](http://www.telerik.com/fiddler) a prověří hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6a166-132">You can also use a tool like **wget**, [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) to examine the response headers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a166-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a166-133">Next Steps</span></span>
* [<span data-ttu-id="6a166-134">Přečtěte si informace o *Cache-Control* záhlaví</span><span class="sxs-lookup"><span data-stu-id="6a166-134">Read about the *Cache-Control* header</span></span>](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="6a166-135">Zjistěte, jak spravovat platnost obsahu cloudové služby v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="6a166-135">Learn how to manage expiration of Cloud Service content in Azure CDN</span></span>](cdn-manage-expiration-of-cloud-service-content.md)

