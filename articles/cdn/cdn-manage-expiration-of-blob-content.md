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
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Spravovat konec platnosti objektů BLOB Azure Storage v Azure CDN
> [!div class="op_single_selector"]
> * [Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Služba Azure blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

Hello [služba objektů blob](../storage/common/storage-introduction.md#blob-storage) v [Azure Storage](../storage/common/storage-introduction.md) je jedním z několika Azure na základě původu integrované s Azure CDN.  Veškerý obsah, veřejně přístupná objektů blob můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).  Hello TTL je dáno hello [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi hello HTTP z úložiště Azure.

> [!TIP]
> Můžete se rozhodnout tooset žádné TTL na objekt blob.  V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.
> 
> Další informace o tom, jak funguje Azure CDN toospeed tooblobs přístupu a další soubory, najdete v části hello [přehled CDN Azure](cdn-overview.md).
> 
> Další podrobnosti o hello služby objektů blob Azure Storage najdete v tématu [koncepty služby objektů Blob](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Tento kurz představuje několik způsobů hello TTL můžete nastavit u objektu blob ve službě Azure Storage.  

## <a name="azure-powershell"></a>Azure PowerShell
[Prostředí Azure PowerShell](/powershell/azure/overview) je jedním z hello nejrychlejší, nejúčinnějších způsobů tooadminister služeb Azure.  Použití hello `Get-AzureStorageBlob` rutiny tooget toohello odkaz objektu blob, nastavte hello `.ICloudBlob.Properties.CacheControl` vlastnost. 

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
> Můžete taky použít PowerShell příliš[spravovat koncové body a profilů CDN](cdn-manage-powershell.md).
> 
> 

## <a name="azure-storage-client-library-for-net"></a>Klientská knihovna pro úložiště Azure pro .NET
tooset objekt blob je TTL pomocí rozhraní .NET, použijte hello [Klientská knihovna pro úložiště Azure pro .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) vlastnost.

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
> Nejsou k dispozici v hello mnoho další ukázky kódu .NET [ukázky úložiště objektů Blob Azure pro .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Ostatní metody
* [Rozhraní příkazového řádku Azure](../cli-install-nodejs.md)
  
    Pokud chcete nahrát objekt blob hello, nastavit hello *cacheControl* vlastnost pomocí hello `-p` přepínače.  Tento příklad nastaví hello TTL tooone hodinu (3600 sekund).
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Explicitně sadu hello *x-ms-blob-cache-control* vlastnost na [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [uvést seznam blokovaných](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), nebo [nastavit vlastnosti objektů Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) požadavku.
* Nástroje pro správu úložiště třetí strany.
  
    Některé nástroje pro správu Azure úložiště jiných výrobců umožňují tooset hello *CacheControl* vlastnost na objekty BLOB. 

## <a name="testing-hello-cache-control-header"></a>Testování hello *Cache-Control* záhlaví
Snadno můžete ověřit hello TTL objektů BLOB.  Pomocí prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), testování, aby se objektu blob služby včetně hello *Cache-Control* hlavičky odpovědi.  Můžete použít také nástroje, jako je **wget**, [Postman](https://www.getpostman.com/), nebo [Fiddler](http://www.telerik.com/fiddler) hlavičky odpovědi tooexamine hello.

## <a name="next-steps"></a>Další kroky
* [Přečtěte si informace o hello *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Zjistěte, jak toomanage vypršení platnosti obsahu cloudové služby v Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

