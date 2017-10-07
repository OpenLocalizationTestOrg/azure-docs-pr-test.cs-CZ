---
title: "aaaEnable veřejný přístup čtení pro kontejnery a objekty BLOB v Azure Blob storage | Microsoft Docs"
description: "Zjistěte, jak toomake kontejnery a objekty BLOB, které jsou dostupné pro anonymní přístup a jak tooaccess je prostřednictvím kódu programu."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: marsma
ms.openlocfilehash: 0675b5dc4d32a3a0a34376ae4c049542b07ba03a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-anonymous-read-access-toocontainers-and-blobs"></a>Správa toocontainers anonymní přístup pro čtení a objekty BLOB
Můžete povolit přístup pro čtení anonymní, veřejné tooa kontejner a jeho objekty BLOB v Azure Blob storage. Díky tomu můžete udělit oprávnění jen pro čtení toothese prostředky bez sdílení klíč účtu a bez nutnosti sdílený přístupový podpis (SAS).

Veřejný přístup pro čtení je nejvhodnější pro scénáře, kde chcete určité tooalways objekty BLOB nebudou dostupné pro anonymní přístup pro čtení. Pro větší kontrolu můžete vytvořit sdílený přístupový podpis. Sdílené přístupové podpisy povolit tooprovide omezený přístup pomocí různých oprávnění pro konkrétní časové období. Další informace o vytváření sdílený přístup podpisy, najdete v části [pomocí sdílené přístupové podpisy (SAS) ve službě Azure Storage](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-toocontainers-and-blobs"></a>Udělení oprávnění toocontainers anonymním uživatelům a objekty BLOB
Ve výchozím nastavení kontejner a všechny objekty BLOB v něm může být přístupné pouze vlastník hello hello účtu úložiště. toogive anonymní uživatelé oprávnění ke čtení tooa kontejner a jeho objekty BLOB, můžete nastavit hello kontejneru oprávnění tooallow veřejný přístup. Anonymní uživatelé mohou číst objekty BLOB do kontejneru, veřejně přístupný bez ověřování hello požadavku.

Kontejner můžete nakonfigurovat s hello následující oprávnění:

* **Žádný veřejný přístup pro čtení:** hello kontejner a jeho objekty BLOB je přístupný jenom vlastník účtu úložiště hello. Toto je výchozí hello pro všechny nové kontejnery.
* **Veřejný přístup pro objekty BLOB pouze pro čtení:** objektů BLOB v kontejneru hello mohou být přečteny anonymní žádost, ale kontejneru data nejsou k dispozici. Anonymní klienti nemůže vytvořit výčet hello objektů BLOB v kontejneru hello.
* **Úplný veřejný přístup pro čtení:** všechna data kontejnerů a objektů blob mohou být přečteny anonymní žádost. Klienti mohou anonymní žádosti o výčet objektů BLOB v kontejneru hello, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.

Můžete použít následující oprávnění kontejneru tooset hello:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blobs)
* [Azure CLI 2.0](storage-azure-cli.md#create-and-manage-blobs)
* Programově pomocí jedné z knihovny klienta úložiště hello nebo hello REST API

### <a name="set-container-permissions-in-hello-azure-portal"></a>Nastavte oprávnění kontejneru v hello portálu Azure
oprávnění ke kontejneru tooset hello [portál Azure](https://portal.azure.com), postupujte takto:

1. Otevřete váš **účet úložiště** okna portálu hello. Váš účet úložiště můžete najít tak, že vyberete **účty úložiště** v hlavní nabídce portálu okně hello.
1. V části **služby objektů BLOB** v okně hello nabídky, vyberte **kontejnery**.
1. Klikněte pravým tlačítkem na kontejner řádek hello nebo vyberte hello třemi tečkami tooopen hello kontejneru **kontextovou nabídku**.
1. Vyberte **zásada přístupu** v kontextové nabídce hello.
1. Vyberte **přistupovat typu** z hello rozevírací nabídce.

    ![Metadata kontejneru dialogové okno Upravit](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>Nastavte oprávnění kontejner s rozhraním .NET
tooset oprávnění pro kontejner pomocí C# a hello Klientská knihovna pro úložiště pro .NET, nejdřív načíst hello kontejneru existující oprávnění tak, že volání hello **GetPermissions** metoda. Potom sadu hello **PublicAccess** vlastnost hello **BlobContainerPermissions** objekt, který je vrácen hello **GetPermissions** metoda. Nakonec zavolejte hello **měli** metoda s hello aktualizovat oprávnění.

Následující ukázka Hello nastaví hello kontejneru oprávnění toofull veřejný přístup pro čtení. tooset oprávnění toopublic přístup pro čtení pro objekty BLOB, nastavte hello **PublicAccess** vlastnost příliš**BlobContainerPublicAccessType.Blob**. tooremove všechna oprávnění pro anonymní uživatele nastavit vlastnost hello příliš**BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Přístup k kontejnery a objekty BLOB anonymně
Konstruktory, které nevyžadují přihlašovací údaje můžete použít klienta, který přistupuje ke kontejnerům a objektům BLOB anonymně. Hello následující příklady ukazují několik různých způsobů anonymně tooreference prostředky služby objektů Blob.

### <a name="create-an-anonymous-client-object"></a>Vytvoření objektu anonymním klientem
Můžete vytvořit nový objekt klienta služby pro anonymní přístup tím, že koncový bod služby objektů Blob hello hello účtu. Nicméně je nutné znát hello název kontejneru v daném účtu, který je k dispozici pro anonymní přístup.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create hello client object using hello Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read hello container's properties. Note this is only possible when hello container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Odkaz kontejner anonymně
Pokud máte hello URL tooa kontejner, který je anonymně k dispozici, můžete ho tooreference hello kontejneru přímo.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in hello container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Referenční objekt blob anonymně
Pokud máte hello URL tooa blob, které jsou dostupné pro anonymní přístup, můžete odkazovat hello blob přímo pomocí této adresy URL:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-tooanonymous-users"></a>Uživatelé k dispozici tooanonymous funkce
Hello následující tabulka ukazuje operací, které může volat anonymní uživatelé, když kontejneru seznamu ACL tooallow veřejný přístup.

| Operace REST | Oprávnění Úplné veřejný přístup pro čtení | Oprávnění s veřejný přístup pro čtení pro pouze objekty BLOB |
| --- | --- | --- |
| Seznam kontejnery |Pouze vlastník |Pouze vlastník |
| Vytvoření kontejneru |Pouze vlastník |Pouze vlastník |
| Získat vlastnosti kontejneru |Všechny |Pouze vlastník |
| Získat Metadata kontejneru |Všechny |Pouze vlastník |
| Nastavit Metadata kontejneru |Pouze vlastník |Pouze vlastník |
| Získání kontejneru seznamu ACL |Pouze vlastník |Pouze vlastník |
| Nastavit kontejneru seznamu ACL |Pouze vlastník |Pouze vlastník |
| Odstranit kontejner |Pouze vlastník |Pouze vlastník |
| Seznam objektů BLOB |Všechny |Pouze vlastník |
| Uveďte objektů Blob |Pouze vlastník |Pouze vlastník |
| Získání objektu Blob |Všechny |Všechny |
| Získat vlastnosti objektu Blob |Všechny |Všechny |
| Nastavit vlastnosti objektů Blob |Pouze vlastník |Pouze vlastník |
| Získat Metadata objektu Blob |Všechny |Všechny |
| Nastavit Metadata objektu Blob |Pouze vlastník |Pouze vlastník |
| Uveďte bloku |Pouze vlastník |Pouze vlastník |
| Získat seznam blokovaných položek (pouze potvrdit bloky) |Všechny |Všechny |
| Získat seznam blokovaných (pouze nepotvrzené bloky nebo všechny bloky) |Pouze vlastník |Pouze vlastník |
| Uveďte seznam blokovaných položek |Pouze vlastník |Pouze vlastník |
| Odstranit objekt Blob |Pouze vlastník |Pouze vlastník |
| Zkopírování objektu Blob |Pouze vlastník |Pouze vlastník |
| Objekt Blob snímku |Pouze vlastník |Pouze vlastník |
| Objekt Blob zapůjčení |Pouze vlastník |Pouze vlastník |
| Uveďte stránky |Pouze vlastník |Pouze vlastník |
| Get rozsahů stránek |Všechny |Všechny |
| Append – objekt Blob |Pouze vlastník |Pouze vlastník |

## <a name="next-steps"></a>Další kroky

* [Ověřování pro hello služby úložiště Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md)
* [Delegování přístupu se sdíleným přístupovým podpisem](https://msdn.microsoft.com/library/azure/ee395415.aspx)
