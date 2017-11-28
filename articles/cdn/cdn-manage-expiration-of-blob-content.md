---
title: "vypršení platnosti aaaManage objektů BLOB Azure Storage v Azure CDN | Microsoft Docs"
description: "Informace o možnostech hello řízení time to live pro objekty BLOB v Azure CDN ukládání do mezipaměti."
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
ms.openlocfilehash: 9fecae9639deb28977da7f851e1da4a823ddc4e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a><span data-ttu-id="fce56-103">Spravovat konec platnosti objektů BLOB Azure Storage v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="fce56-103">Manage expiration of Azure Storage blobs in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fce56-104">Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS</span><span class="sxs-lookup"><span data-stu-id="fce56-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="fce56-105">Služba Azure blob Storage</span><span class="sxs-lookup"><span data-stu-id="fce56-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="fce56-106">Hello [služba objektů blob](../storage/common/storage-introduction.md#blob-storage) v [Azure Storage](../storage/common/storage-introduction.md) je jedním z několika Azure na základě původu integrované s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="fce56-106">hello [blob service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is one of several Azure-based origins integrated with Azure CDN.</span></span>  <span data-ttu-id="fce56-107">Veškerý obsah, veřejně přístupná objektů blob můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).</span><span class="sxs-lookup"><span data-stu-id="fce56-107">Any publicly accessible blob content can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="fce56-108">Hello TTL je dáno hello [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi hello HTTP z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="fce56-108">hello TTL is determined by hello [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in hello HTTP response from Azure Storage.</span></span>

> [!TIP]
> <span data-ttu-id="fce56-109">Můžete se rozhodnout tooset žádné TTL na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="fce56-109">You may choose tooset no TTL on a blob.</span></span>  <span data-ttu-id="fce56-110">V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.</span><span class="sxs-lookup"><span data-stu-id="fce56-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="fce56-111">Další informace o tom, jak funguje Azure CDN toospeed tooblobs přístupu a další soubory, najdete v části hello [přehled CDN Azure](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fce56-111">For more information about how Azure CDN works toospeed up access tooblobs and other files, see hello [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> <span data-ttu-id="fce56-112">Další podrobnosti o hello služby objektů blob Azure Storage najdete v tématu [koncepty služby objektů Blob](https://msdn.microsoft.com/library/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="fce56-112">For more details on hello Azure Storage blob service, see [Blob Service Concepts](https://msdn.microsoft.com/library/dd179376.aspx).</span></span> 
> 
> 

<span data-ttu-id="fce56-113">Tento kurz představuje několik způsobů hello TTL můžete nastavit u objektu blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fce56-113">This tutorial demonstrates several ways that you can set hello TTL on a blob in Azure Storage.</span></span>  

## <a name="azure-powershell"></a><span data-ttu-id="fce56-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fce56-114">Azure PowerShell</span></span>
<span data-ttu-id="fce56-115">[Prostředí Azure PowerShell](/powershell/azure/overview) je jedním z hello nejrychlejší, nejúčinnějších způsobů tooadminister služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="fce56-115">[Azure PowerShell](/powershell/azure/overview) is one of hello quickest, most powerful ways tooadminister your Azure services.</span></span>  <span data-ttu-id="fce56-116">Použití hello `Get-AzureStorageBlob` rutiny tooget toohello odkaz objektu blob, nastavte hello `.ICloudBlob.Properties.CacheControl` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fce56-116">Use hello `Get-AzureStorageBlob` cmdlet tooget a reference toohello blob, then set hello `.ICloudBlob.Properties.CacheControl` property.</span></span> 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference toohello blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set hello CacheControl property tooexpire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send hello update toohello cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> <span data-ttu-id="fce56-117">Můžete taky použít PowerShell příliš[spravovat koncové body a profilů CDN](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fce56-117">You can also use PowerShell too[manage your CDN profiles and endpoints](cdn-manage-powershell.md).</span></span>
> 
> 

## <a name="azure-storage-client-library-for-net"></a><span data-ttu-id="fce56-118">Klientská knihovna pro úložiště Azure pro .NET</span><span class="sxs-lookup"><span data-stu-id="fce56-118">Azure Storage Client Library for .NET</span></span>
<span data-ttu-id="fce56-119">tooset objekt blob je TTL pomocí rozhraní .NET, použijte hello [Klientská knihovna pro úložiště Azure pro .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fce56-119">tooset a blob's TTL using .NET, use hello [Azure Storage Client Library for .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) property.</span></span>

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with hello blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference toohello container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference toohello blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set hello CacheControl property tooexpire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update hello blob's properties in hello cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> <span data-ttu-id="fce56-120">Nejsou k dispozici v hello mnoho další ukázky kódu .NET [ukázky úložiště objektů Blob Azure pro .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="fce56-120">There are many more .NET code samples available in hello [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span>
> 
> 

## <a name="other-methods"></a><span data-ttu-id="fce56-121">Ostatní metody</span><span class="sxs-lookup"><span data-stu-id="fce56-121">Other methods</span></span>
* [<span data-ttu-id="fce56-122">Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="fce56-122">Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)
  
    <span data-ttu-id="fce56-123">Pokud chcete nahrát objekt blob hello, nastavit hello *cacheControl* vlastnost pomocí hello `-p` přepínače.</span><span class="sxs-lookup"><span data-stu-id="fce56-123">When uploading hello blob, set hello *cacheControl* property using hello `-p` switch.</span></span>  <span data-ttu-id="fce56-124">Tento příklad nastaví hello TTL tooone hodinu (3600 sekund).</span><span class="sxs-lookup"><span data-stu-id="fce56-124">This example sets hello TTL tooone hour (3600 seconds).</span></span>
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [<span data-ttu-id="fce56-125">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="fce56-125">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    <span data-ttu-id="fce56-126">Explicitně sadu hello *x-ms-blob-cache-control* vlastnost na [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [uvést seznam blokovaných](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), nebo [nastavit vlastnosti objektů Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) požadavku.</span><span class="sxs-lookup"><span data-stu-id="fce56-126">Explicitly set hello *x-ms-blob-cache-control* property on a [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), or [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) request.</span></span>
* <span data-ttu-id="fce56-127">Nástroje pro správu úložiště třetí strany.</span><span class="sxs-lookup"><span data-stu-id="fce56-127">Third-party storage management tools</span></span>
  
    <span data-ttu-id="fce56-128">Některé nástroje pro správu Azure úložiště jiných výrobců umožňují tooset hello *CacheControl* vlastnost na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="fce56-128">Some third-party Azure Storage management tools allow you tooset hello *CacheControl* property on blobs.</span></span> 

## <a name="testing-hello-cache-control-header"></a><span data-ttu-id="fce56-129">Testování hello *Cache-Control* záhlaví</span><span class="sxs-lookup"><span data-stu-id="fce56-129">Testing hello *Cache-Control* header</span></span>
<span data-ttu-id="fce56-130">Snadno můžete ověřit hello TTL objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="fce56-130">You can easily verify hello TTL of your blobs.</span></span>  <span data-ttu-id="fce56-131">Pomocí prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), testování, aby se objektu blob služby včetně hello *Cache-Control* hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="fce56-131">Using your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob is including hello *Cache-Control* response header.</span></span>  <span data-ttu-id="fce56-132">Můžete použít také nástroje, jako je **wget**, [Postman](https://www.getpostman.com/), nebo [Fiddler](http://www.telerik.com/fiddler) hlavičky odpovědi tooexamine hello.</span><span class="sxs-lookup"><span data-stu-id="fce56-132">You can also use a tool like **wget**, [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) tooexamine hello response headers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fce56-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fce56-133">Next Steps</span></span>
* [<span data-ttu-id="fce56-134">Přečtěte si informace o hello *Cache-Control* záhlaví</span><span class="sxs-lookup"><span data-stu-id="fce56-134">Read about hello *Cache-Control* header</span></span>](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="fce56-135">Zjistěte, jak toomanage vypršení platnosti obsahu cloudové služby v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="fce56-135">Learn how toomanage expiration of Cloud Service content in Azure CDN</span></span>](cdn-manage-expiration-of-cloud-service-content.md)

