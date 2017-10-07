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
ms.openlocfilehash: 629f5c0aee3f41115a0d514a2010d8cc0187126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Sdílené přístupové podpisy, část 2: Vytvoření a použití SAS s úložištěm Blob

[Část 1](storage-dotnet-shared-access-signature-part-1.md) tohoto kurzu prozkoumali sdílené přístupové podpisy (SAS) a vysvětlení osvědčené postupy pro jejich použití. Část 2 ukazuje, jak toogenerate a pak použít sdílené přístupové podpisy pomocí úložiště objektů Blob. Příklady Hello jsou napsané v C# a používají hello Klientská knihovna pro úložiště Azure pro .NET. Hello příklady v tomto kurzu:

* Vygenerovat sdílený přístupový podpis do kontejneru
* Vygenerovat sdílený přístupový podpis na objekt blob
* Vytvořit uložené přístup podpisy toomanage zásad na prostředky kontejneru
* Testování hello sdílené přístupové podpisy v aplikaci klienta

## <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu vytvoříme dvě konzolové aplikace, které ukazují, vytvoření a použití sdílených přístupových podpisů pro kontejnery a objekty BLOB:

**Aplikaci 1**: hello aplikace pro správu. Vygeneruje sdílený přístupový podpis kontejneru a objekt blob. Zahrnuje přístupový klíč účtu úložiště hello ve zdrojovém kódu.

**Aplikace 2**: hello klientské aplikace. Přístupy kontejnerů a objektů blob prostředků pomocí hello sdílené přístupové podpisy vytvoření první aplikace hello. Používá pouze hello sdílené přístupové podpisy tooaccess kontejneru a prostředků blob – nemá *není* zahrnují hello přístupový klíč účtu úložiště.

## <a name="part-1-create-a-console-application-toogenerate-shared-access-signatures"></a>Část 1: Vytvoření přístup ke konzole aplikace toogenerate sdílené podpisy
Nejprve je třeba splnit hello Klientská knihovna pro úložiště Azure pro .NET nainstalované. Můžete nainstalovat hello [balíček NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "balíček NuGet") obsahující hello nejaktuálnější sestavení pro knihovny klienta hello. Toto je doporučená metoda pro zajištění, že máte nejnovější opravy hello hello. Klientská knihovna pro hello můžete také stáhnout jako součást hello nejnovější verzi hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/).

V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **GenerateSharedAccessSignatures**. Přidejte odkazy na příliš[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) a [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) pomocí jedné z následujících postupů hello:

* Použití hello [Správce balíčků NuGet](https://docs.nuget.org/consume/installing-nuget) v sadě Visual Studio. Vyberte **projektu** > **spravovat balíčky NuGet**, vyhledejte online každý balíček (Microsoft.WindowsAzure.ConfigurationManager a WindowsAzure.Storage) a instalovat je.
* Můžete taky najít tyto sestavení v instalaci softwaru hello Azure SDK a přidejte toothem odkazy:
  * Microsoft.WindowsAzure.Configuration.dll
  * Microsoft.WindowsAzure.Storage.dll

Hello horní části souboru Program.cs hello, přidejte následující hello **pomocí** direktivy:

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Upravte soubor app.config hello, tak, aby obsahoval nastavení konfigurace s připojovacím řetězcem, který odkazuje tooyour účet úložiště. Váš soubor app.config by měl vypadat podobně jako toothis jeden:

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

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Vygenerovat sdílený přístupový podpis identifikátor URI pro kontejner
toobegin s přidáme metoda toogenerate sdílený přístupový podpis na nový kontejner. V takovém případě hello podpis není přidružený k zásadě uložené přístup, takže vykonává hello URI hello informace o tom jeho oprávnění hello a čas vypršení platnosti uděluje.

Nejprve přidejte kód toohello **Main()** metoda tooauthenticate přístup k účtu úložiště tooyour a vytvořit nový kontejner:

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

V dalším kroku přidejte metodu, která generuje hello sdílený přístupový podpis kontejneru hello a vrátí podpis hello URI:

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

Přidejte následující řádky na konci hello hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, toocall **GetContainerSasUri()** a zápis hello okno konzoly URI toohello podpis:

```csharp
//Generate a SAS URI for hello container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

Zkompilování a spuštění toooutput hello sdílený přístupový podpis identifikátor URI pro nový kontejner hello. Hello URI bude podobné toohello následující:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

Když spustíte hello kód hello sdíleného přístupového podpisu, který jste vytvořili pro kontejner hello bude platit pro hello dalších 24 hodin. podpis Hello uděluje oprávnění toolist objekty BLOB v kontejneru hello a toowrite nový kontejner objektů BLOB toohello klienta.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Vygenerovat sdílený přístupový podpis identifikátor URI pro objekt blob
V dalším kroku zápisu podobné toocreate kód nové objektů blob v kontejneru hello a vygenerovat sdílený přístupový podpis pro ni. Tento sdílený přístupový podpis není přidružený k zásadě uložené přístup tak, aby zahrnovala hello počáteční čas, čas vypršení platnosti a informace o oprávněních v hello identifikátor URI.

Přidejte novou metodu, která vytvoří nový objekt blob a zapíše některé tooit text a vygeneruje sdílený přístupový podpis a vrátí podpis hello URI:

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

Na konci hello hello **Main()** metoda, přidejte následující řádky toocall hello **GetBlobSasUri()**, než hello volat příliš**Console.ReadLine()**a zápis hello sdílet okno pro přístup k podpisu URI toohello konzoly:

```csharp
//Generate a SAS URI for a blob within hello container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

Zkompilování a spuštění toooutput hello sdílený přístupový podpis identifikátor URI pro nový objekt blob hello. Hello URI bude podobné toohello následující:

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-hello-container"></a>Vytvoření zásady přístupu uložené na kontejneru hello
Nyní Pojďme vytvořit zásadu přístupu uložené na hello kontejneru, který bude definování hello omezení pro všechny sdílené přístupové podpisy, které jsou k ní přidružena.

V předchozích příkladech hello jsme zadali hello čas spuštění (implicitně nebo explicitně), čas vypršení platnosti hello a hello oprávnění hello sdílený přístupový podpis URI sám sebe. V následující příklady hello určíme tyto zásady přístupu hello uložené, nikoli hello sdílený přístupový podpis. Díky tomu tak umožňuje nám toochange těchto omezení bez opětovného vydání hello sdílený přístup k podpisu.

Je možné toohave jeden nebo více hello omezení hello sdílený přístupový podpis a zbývající hello v zásadách přístupu hello uložené. Ale pouze můžete hello počáteční čas, čas vypršení platnosti a oprávnění v jednom místě nebo hello jiné. Například nelze nastavit oprávnění na hello sdílený přístupový podpis a také zadejte je v zásadách přístupu hello uložené.

Když přidáte tooa kontejner zásad uložené přístup, musí získat hello kontejneru existující oprávnění, přidat nové zásady přístupu hello a potom nastavte hello kontejneru oprávnění.

Přidejte novou metodu, která vytvoří novou zásadu uložené přístup do kontejneru a vrátí hello název zásady hello:

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

Na konci hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, přidejte následující hello řádků toofirst vymazat všechny existující zásady přístupu a pak zavolají hello  **CreateSharedAccessPolicy()** metoda:

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

Když zrušíte zaškrtnutí hello zásady přístupu do kontejneru, je nutné nejprve získat hello kontejneru existující oprávnění a potom zrušte hello oprávnění a potom znovu nastavit oprávnění hello.

### <a name="generate-a-shared-access-signature-uri-on-hello-container-that-uses-an-access-policy"></a>Vygenerovat sdílený přístupový podpis URI hello kontejneru, který používá zásady přístupu
V dalším kroku vytvoříme jiný sdílený přístupový podpis kontejneru hello, kterou jsme vytvořili předtím, ale momentálně jsme přidružit hello podpis hello uložené zásady přístupu, které jsme vytvořili v předchozím příkladu hello.

Přidejte nové toogenerate metoda jiný sdílený přístupový podpis kontejneru hello:

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

Na konci hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, přidejte následující řádky toocall hello hello **GetContainerSasUriWithPolicy** – metoda :

```csharp
//Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-hello-blob-that-uses-an-access-policy"></a>Umožňuje vygenerovat URI podpis sdíleného přístupu na hello objektů Blob, aby používal zásady přístupu
Nakonec přidejte podobné toocreate metoda jiný objekt blob a vygenerovat sdílený přístupový podpis, který má přidružené k zásadě uložené přístup.

Přidejte nové toocreate metoda objektu blob a vygenerovat sdílený přístupový podpis:

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

Na konci hello hello **Main()** metoda, než hello volat příliš**Console.ReadLine()**, přidejte následující řádky toocall hello hello **GetBlobSasUriWithPolicy** metoda:

```csharp
//Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

Hello **Main()** metoda by měl nyní vypadat jako v celé jeho šíři. Spustit toowrite hello sdílený přístupový podpis okna konzoly toohello identifikátory URI, pak zkopírujte a vložte je do textového souboru pro použití v hello druhé části tohoto kurzu.

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

Když spustíte hello GenerateSharedAccessSignatures konzolovou aplikaci, uvidíte výstup podobný toohello následující. Jedná se o hello sdílené přístupové podpisy, které můžete použít v rámci 2 hello kurzu.

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-tootest-hello-shared-access-signatures"></a>Část 2: Vytvoření tootest hello sdílený přístup z konzole aplikace podpisů
tootest hello sdílené přístupové podpisy vytvořili v předchozích příkladech hello, vytvoříme druhý konzolovou aplikaci, která používá hello podpisy tooperform operací na hello kontejneru a na objekt blob.

> [!NOTE]
> Pokud víc než 24 hodin předané vzhledem k tomu, že jste dokončili první část kurzu hello hello, hello podpisy, které jste vygenerovali již nebude platný. V takovém případě byste měli spustit hello kódu v hello první konzole aplikace toogenerate čerstvé sdílené přístupové podpisy pro použití v druhé části kurzu hello hello.
>

V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **ConsumeSharedAccessSignatures**. Přidejte odkazy na příliš[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) a [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), protože jste provedli dříve.

Hello horní části souboru Program.cs hello, přidejte následující hello **pomocí** direktivy:

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

V textu hello hello **Main()** metody přidat hello následující řetězcové konstanty, změna jejich hodnoty toohello sdílené přístupové podpisy generované v části 1 hello kurzu.

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-tootry-container-operations-using-a-shared-access-signature"></a>Přidání operací metoda tootry kontejneru pomocí sdíleného přístupového podpisu
Potom přidáme metodu, která se testuje některé operace kontejneru pomocí sdíleného přístupového podpisu kontejneru hello. sdílený přístupový podpis Hello je použité tooreturn kontejner toohello odkaz ověřování kontejneru toohello přístup podle hello podpis samostatně.

Přidejte následující metodu tooProgram.cs hello:

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

Aktualizace hello **Main()** metoda toocall **UseContainerSAS()** s oběma hello sdílené přístupové podpisy, které jste vytvořili v kontejneru hello:

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

### <a name="add-a-method-tootry-blob-operations-using-a-shared-access-signature"></a>Přidání operací metoda tootry blob pomocí sdíleného přístupového podpisu
Nakonec přidáme metodu, která se testuje některé operace objektů blob pomocí sdíleného přístupového podpisu u objektu blob hello. V tomto případě používáme hello konstruktor **CloudBlockBlob(String)**a předejte hello sdílený přístupový podpis tooreturn referenční toohello objekt blob. Další ověřování není vyžadováno; je založena na hello podpis samostatně.

Přidejte následující metodu tooProgram.cs hello:

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

Aktualizace hello **Main()** metoda toocall **UseBlobSAS()** s oběma hello sdílené přístupové podpisy, které jste vytvořili na objekt blob hello:

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

Spusťte hello konzolové aplikace a sledovat toosee výstup hello operací, které jsou povoleny pro které podpisy. výstup Hello v okně konzoly hello bude vypadat podobně jako toohello následující:

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a>Další kroky

* [Sdílené přístupové podpisy, část 1: Vysvětlení modelu SAS hello](storage-dotnet-shared-access-signature-part-1.md)
* [Správa toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md)
* [Delegování přístupu k pomocí sdíleného přístupového podpisu (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Představení tabulky a fronty SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
