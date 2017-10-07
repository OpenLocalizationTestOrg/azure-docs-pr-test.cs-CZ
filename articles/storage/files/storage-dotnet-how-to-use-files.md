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
# <a name="develop-for-azure-file-storage-with-net"></a>Vývoj pro službu Azure File Storage pomocí .NET 
> [!NOTE]
> Tento článek ukazuje, jak toomanage Azure File storage s kód .NET. toolearn Další informace o Azure File storage najdete v tématu hello [Úvod tooAzure úložiště File](storage-files-introduction.md).
>

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu se ukazují hello základy používání služby, které používají Azure File storage toostore souborová data nebo toodevelop aplikace .NET. V tomto kurzu bude vytvoření jednoduché konzolové aplikace a zobrazit jak tooperform základní operace s .NET a Azure File storage:

* Získat hello obsah souboru
* Nastavit hello kvótu (maximální velikost) pro sdílenou složku hello.
* Vytvořte sdílený přístupový podpis (SAS klíč) pro soubor, který používá definovaný ve sdílené složce hello zásady sdíleného přístupu.
* Zkopírujte soubor tooanother souboru hello stejný účet úložiště.
* Zkopírujte soubor tooa objekt blob v hello stejný účet úložiště.
* Použijte Azure Storage Metrics pro řešení potíží.

> [!Note]  
> Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure sdílenou složku pomocí hello standardní System.IO třídy pro vstupně-výstupní soubor. Tento článek popisuje, jak toowrite aplikace, které používají hello .NET SDK úložiště Azure, který používá hello [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File. 


## <a name="create-hello-console-application-and-obtain-hello-assembly"></a>Vytvoření konzolové aplikace hello a získání sestavení hello
V sadě Visual Studio vytvořte novou konzolovou aplikaci pro Windows. Hello následující kroky vám ukážou, jak toocreate konzolovou aplikaci v sadě Visual Studio 2017, ale hello kroky jsou podobné v jiných verzích sady Visual Studio.

1. Vyberte **Soubor**  >  **Nový**  >  **Projekt**.
2. Vyberte **Instalováno** > **Šablony** > **Visual C#** > **Klasická plocha Windows**.
3. Vyberte **Aplikace konzoly (.NET Framework)**.
4. Zadejte název pro vaši aplikaci ve hello **název:** pole
5. Vyberte **OK**.

Všechny příklady kódu v tomto kurzu lze přidat toohello `Main()` metoda konzolové aplikace `Program.cs` souboru.

Hello Klientská knihovna pro úložiště Azure můžete použít v libovolného typu aplikace .NET, včetně Azure cloud service nebo webovou aplikaci a stolní počítače a mobilní aplikace. V této příručce použijeme konzolovou aplikaci kvůli zjednodušení.

## <a name="use-nuget-tooinstall-hello-required-packages"></a>Použití NuGet tooinstall hello požadované balíčky
Existují dva balíčky, které potřebujete tooreference ve vašem projektu toocomplete v tomto kurzu:

* [Microsoft Azure Storage Client Library pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Tento balíček zajišťuje programový přístup toodata prostředky ve vašem účtu úložiště.
* [Microsoft Azure Configuration Manager library for .NET:](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) Tento balíček poskytuje třídu pro potřeby analýzy připojovacího řetězce v konfiguračním souboru bez ohledu na to, kde je aplikace spuštěná.

Můžete vytvořit NuGet tooobtain oba balíčky. Postupujte následovně:

1. Klikněte v **Průzkumníku řešení** pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet**.
2. Vyhledejte online text "WindowsAzure.Storage" a klikněte na tlačítko **nainstalovat** tooinstall hello Klientská knihovna pro úložiště a jeho závislosti.
3. Online hledání "WindowsAzure.ConfigurationManager" a klikněte na tlačítko **nainstalovat** tooinstall hello Azure Configuration Manager.

## <a name="save-your-storage-account-credentials-toohello-appconfig-file"></a>Uložte soubor app.config toohello přihlašovací údaje účtu úložiště
Dál uložte svoje přihlašovací údaje do souboru app.config vašeho projektu. Upravte soubor app.config hello tak, aby se podobné toohello následující ukázka, nahraďte `myaccount` s názvem svého účtu úložiště a `mykey` nahraďte svým klíčem účtu úložiště.

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
> nejnovější verze emulátoru úložiště Azure hello Hello nepodporuje Azure File storage. Připojovací řetězec musí být účet úložiště Azure v cloudu toowork hello s Azure File storage.

## <a name="add-using-directives"></a>Přidání direktiv using
Otevřete hello `Program.cs` soubor v Průzkumníku řešení a přidejte následující hello pomocí direktivy toohello horní části souboru hello.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-hello-file-share-programmatically"></a>Sdílení souborů hello přístup prostřednictvím kódu programu
Dál přidejte následující kód toohello hello `Main()` – metoda (po výše uvedeném kódu hello) tooretrieve hello připojovací řetězec. Tento kód získá odkaz na soubor toohello, které jsme vytvořili předtím a výstupy okna konzoly toohello jeho obsah.

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

Spusťte výstup hello hello konzoly aplikace toosee.

## <a name="set-hello-maximum-size-for-a-file-share"></a>Nastavit maximální velikost hello sdílené složky
Od verze 5.x hello Klientská knihovna pro úložiště Azure, můžete nastavit sady hello kvóty (nebo maximální velikost) pro sdílenou složku, v gigabajtech. Můžete také zkontrolovat toosee množství dat, které jsou aktuálně uložené ve složce hello.

Pomocí nastavení hello kvótu sdílené složky můžete omezit hello celková velikost souborů hello uložené ve sdílené složce hello. Pokud celková velikost souborů ve sdílené složce hello hello překročí kvótu hello nastavte ve sdílené složce hello a pak klienti budou nelze tooincrease hello velikost existující soubory nebo vytvořit nové soubory, pokud tyto soubory jsou prázdné.

Následující příklad Hello ukazuje, jak toocheck hello aktuální využití sdílené složky a jak tooset hello kvótu pro sdílenou složku hello.

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

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku
Od verze 5.x hello Klientská knihovna pro úložiště Azure, můžete vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo pro konkrétní soubor. Můžete také vytvořit zásady sdíleného přístupu na toomanage sdílený přístup ke sdílení souborů signatur. Vytvoření zásady sdíleného přístupu se doporučuje, protože nabízí způsob odvolání hello SAS, pokud by měl být dojde k ohrožení.

Hello následující ukázka vytvoří zásada sdíleného přístupu ve sdílené složce a pak použije, že zásady omezení tooprovide hello pro SAS na souboru ve hello sdílet.

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

Další informace o vytváření a používání sdílených přístupových podpisů najdete v tématech [Použití sdílených přístupových podpisů (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) a [Vytvoření a použití SAS s objekty blob Azure](../blobs/storage-dotnet-shared-access-signature-part-2.md).

## <a name="copy-files"></a>Kopírování souborů
Od verze 5.x hello Klientská knihovna pro úložiště Azure, můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob. V dalších částech hello, ukážeme, jak se tooperform tyto zkopírovat operations prostřednictvím kódu programu.

Můžete také použít AzCopy toocopy jeden soubor tooanother nebo toocopy naopak objektů blob tooa soubor nebo naopak. V tématu [přenos dat pomocí příkazového řádku Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

> [!NOTE]
> Pokud kopírujete objekt blob tooa soubor nebo soubor tooa objekt blob, musíte použít sdílený přístupový podpis (SAS) tooauthenticate hello zdrojový objekt, i když kopírujete v rámci hello stejný účet úložiště.
> 
> 

**Zkopírujte soubor tooanother souboru** hello následujícím příkladu se zkopíruje soubor tooanother souboru v hello téže sdílené složky. Protože tato operace kopírování kopíruje soubory v hello stejný účet úložiště, můžete použít sdílený klíč ověřování tooperform hello kopírování.

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

**Zkopírujte soubor tooa objekt blob** hello následující příklad vytvoří soubor a zkopíruje ho tooa objektů blob v rámci hello stejný účet úložiště. Hello příklad vytvoří SAS pro hello zdrojový soubor, který služba hello používá tooauthenticate přístup toohello zdrojového souboru během operace kopírování hello.

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

Můžete kopírovat soubor tooa objektů blob v hello stejným způsobem. Pokud hello zdrojový objekt, který je objekt blob, vytvořte objekt blob SAS tooauthenticate přístup toothat během operace kopírování hello.

## <a name="troubleshooting-azure-file-storage-using-metrics"></a>Řešení potíží se službou Azure File Storage pomocí metrik
Azure Storage Analytics teď podporuje metriky pro službu Azure File Storage. S údaji z metriky můžete sledovat žádosti a diagnostikovat potíže.


Můžete povolit metriky pro úložiště Azure File hello [portálu Azure](https://portal.azure.com). Můžete také povolit metriky programově pomocí volání hello operace Set File Service Properties přes hello REST API nebo některou z podobných operací v hello Klientská knihovna pro úložiště.


Hello následující příklad kódu ukazuje, jak toouse hello Klientská knihovna pro úložiště pro .NET tooenable metriky pro úložiště Azure File.

Nejprve přidejte následující hello `using` tooyour direktivy `Program.cs` souboru, kromě toothose jste přidali výše:

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Pamatujte, že při Azure BLOB, tabulka Azure a Azure používají hello sdílené `ServiceProperties` typu v hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` obor názvů, úložiště Azure File používá vlastní typ hello `FileServiceProperties` typu v hello `Microsoft.WindowsAzure.Storage.File.Protocol` oboru názvů. Oba obory názvů musí odkazovat z vašeho kódu, ale pro následující kód toocompile hello.

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

Navíc můžete odkazovat příliš[Azure File storage článku Poradce při potížích s](storage-troubleshoot-windows-file-connection-problems.md) pro pokyny k odstraňování problémů začátku do konce.

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Podpora nástrojů pro úložiště File
* [Jak toouse AzCopy s Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Použití hello rozhraní příkazového řádku Azure s Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Řešení potíží se službou Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Zachování připojení tooMicrosoft Azure File storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
