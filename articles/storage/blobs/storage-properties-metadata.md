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
# <a name="set-and-retrieve-properties-and-metadata"></a>Nastavení a načtení vlastností a metadat

Objekty v vlastnosti podporu systému Azure Storage a metadata definovaná uživatelem, kromě toohello data, která obsahují. Tento článek popisuje správu vlastnosti systému a metadata definovaná uživatelem s hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **Vlastnosti systému**: vlastnosti systému existovat na všechny prostředky úložiště. Některé z nich můžou číst nebo nastavit, zatímco jiné jsou jen pro čtení. V části hello zahrnuje některé vlastnosti systému odpovídají toocertain standardní hlavičky protokolu HTTP. Klientská knihovna pro úložiště Azure Hello udržuje to pro vás.

* **Metadata definovaná uživatelem**: uživatelem definované metadata jsou metadata, která jste zadali na daný prostředek v podobě hello dvojic název hodnota. Můžete vytvořit další hodnoty metadata toostore s prostředků úložiště. Tyto hodnoty další metadata jsou vlastní pouze pro účely a neovlivní chování hello prostředků.

Načítání hodnot vlastností a metadat pro úložiště prostředek je ve dvou krocích. Než si můžete přečíst tyto hodnoty, můžete musí explicitně načíst je ve volání hello **FetchAttributes** metoda.

> [!IMPORTANT]
> Hodnoty vlastnosti a metadat pro úložiště prostředků nejsou naplněny, pokud je volání jednoho z hello **FetchAttributes** metody.
>
> Zobrazí se `400 Bad Request` Pokud všechny dvojice název/hodnota obsahovat jiné znaky než ASCII. Dvojice název/hodnota metadata jsou platné hlavičky protokolu HTTP a proto musí splňovat tooall omezení, kterými se řídí hlavičky protokolu HTTP. Proto se doporučuje používat kódování URL nebo pro názvy a hodnoty, který obsahuje jiné znaky než ASCII kódování Base64.
>

## <a name="setting-and-retrieving-properties"></a>Nastavení nebo načtení vlastností
hodnoty vlastností tooretrieve, volání hello **FetchAttributes** metoda vaší objektů blob nebo kontejneru toopopulate hello vlastností, přečtěte si hello hodnoty.

Zadejte hodnotu vlastnosti hello tooset vlastnosti v objektu, a potom volání hello **SetProperties –** metoda.

Hello následující příklad kódu vytvoří kontejner a pak zapíše některé z okna konzoly tooa hodnoty vlastnosti.

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

## <a name="setting-and-retrieving-metadata"></a>Nastavení a načítání metadat
Metadata můžete zadat jako jeden nebo více dvojice název hodnota u objektů blob nebo kontejner prostředku. tooset metadata přidat dvojice název hodnota toohello **Metadata** kolekce na hello prostředku, pak zavolají hello **SetMetadata** metoda toosave hello hodnoty toohello služby.

> [!NOTE]
> Název Hello metadata musí odpovídat toohello zásady vytváření názvů pro identifikátory jazyka C#.
>
>

Hello následující příklad kódu nastaví metadata do kontejneru. Jedna hodnota je nastavena pomocí kolekce hello **přidat** metoda. Hello jiná hodnota je nastavena pomocí syntaxe implicitní klíč/hodnota. Obě jsou platné.

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

metadata tooretrieve, volání hello **FetchAttributes** metoda na vaše objektů blob nebo kontejneru hello toopopulate **Metadata** kolekce, pak přečte hello hodnoty, jak je znázorněno v následujícím příkladu hello.

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

## <a name="next-steps"></a>Další kroky
* [Klientská knihovna pro Azure Storage pro .NET – referenční informace](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [Azure Klientská knihovna pro úložiště pro balíček NuGet pro rozhraní .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
