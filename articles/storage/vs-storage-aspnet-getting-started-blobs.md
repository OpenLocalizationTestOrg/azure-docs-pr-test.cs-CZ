---
title: "Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Jak začít používat úložiště objektů blob v Azure po připojení k účtu úložiště pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: tarcher
ms.openlocfilehash: 8fd13efdbdd98c6d7dff1b88a6b232a08aa5a13d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled

Azure blob storage je služba, která ukládá Nestrukturovaná data v cloudu jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob storage se také nazývá úložiště objektů.

Tento kurz ukazuje, jak napsat kód ASP.NET pro některé běžné scénáře použití služby Azure blob storage. Scénáře zahrnují vytváření kontejner objektů blob a odesílání, výpis, stahování a odstraňování objektů BLOB.

##<a name="prerequisites"></a>Požadavky

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Účet služby Azure Storage](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Vytvořit řadič MVC 

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce vyberte **Přidat -> řadiče**.

    ![Přidat řadič do aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. Na **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Na **přidat kontroler** dialogové okno, názvu kontroleru *BlobsController*a vyberte **přidat**.

    ![Název řadiče MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Přidejte následující *pomocí* direktivy pro `BlobsController.cs` souboru:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Vytvořte kontejner objektů blob

Kontejner objektů blob je hierarchie vnořených objektů BLOB a složek. Následující kroky ukazují, jak vytvořit kontejner objektů blob:

> [!NOTE]
> 
> Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment). 

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem **CreateBlobContainer** , který vrací **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. V rámci **CreateBlobContainer** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použijte následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure. (Změnit  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název požadovaného objektu blob kontejneru. **CloudBlobClient.GetContainerReference** metoda neprovede požadavek úložiště objektů blob. Odkaz se vrátí, zda existuje kontejner objektů blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Volání **CloudBlobContainer.CreateIfNotExists** metodu pro vytvoření kontejneru, pokud ještě neexistuje. **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** Pokud kontejner neexistuje a je úspěšně vytvořen. V opačném **false** je vrácen.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Aktualizace **ViewBag** s názvem kontejneru objektů blob.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.

1. Na **přidat zobrazení** dialogové okno, zadejte **CreateBlobContainer** pro název zobrazení, vyberte **přidat**.

1. Otevřete `CreateBlobContainer.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Spusťte aplikaci a vyberte **vytvořit kontejner objektů Blob** a zobrazte výsledky podobné následujícím snímku obrazovky:
  
    ![Vytvoření kontejneru objektů blob](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Jak je uvedeno nahoře, **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** pouze když kontejner neexistuje, je vytvořena. Proto pokud spustíte aplikaci kontejneru existuje, vrátí metoda **false**. Aplikace je třeba spustit vícekrát, je nutné odstranit kontejner před spuštěním aplikace znovu. Odstranění kontejneru, můžete to udělat pomocí **CloudBlobContainer.Delete** metoda. Můžete také odstranit kontejner pomocí [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Nahrát objekt blob do kontejneru objektů blob

Jakmile jste [vytvořit kontejner objektů blob](#create-a-blob-container), mohou nahrávat soubory do tohoto kontejneru. Tato část vás provede nahráním místního souboru do kontejneru objektů blob. Postup předpokládá, že jste vytvořili kontejner objektů blob s názvem *test kontejneru objektů blob*. 

> [!NOTE]
> 
> Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment). 

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem **UploadBlob** , který vrací **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. V rámci **UploadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Jak je popsáno výše, Azure storage podporuje typy jiný objektu blob. Chcete-li získat odkaz na objekt blob stránky, zavolejte **CloudBlobContainer.GetPageBlobReference** metoda. Chcete-li získat odkaz na objekt blob bloku, zavolejte **CloudBlobContainer.GetBlockBlobReference** metoda. Typ k použití doporučuje je obvykle objekt blob bloku. (Změnit < název objektu blob > * k názvu chcete poskytnout jednou nahrát objekt blob.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat k němu voláním odkaz na objekt blob **UploadFromStream** metoda. **UploadFromStream** metoda vytvoří objekt blob, pokud neexistuje, nebo ho přepíše, pokud neexistuje. (Změnit  *&lt;nahrávání souborů >* na plně kvalifikovanou cestu k souboru, který chcete odeslat.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Spusťte aplikaci a vyberte **nahrávání blob**.  
  
V části - [seznamu objektů BLOB v kontejneru objektů blob](#list-the-blobs-in-a-blob-container) -znázorňuje, jak zobrazit seznam objektů BLOB v kontejneru objektů blob.    

## <a name="list-the-blobs-in-a-blob-container"></a>Seznam objektů BLOB v kontejneru objektů blob

Tato část ukazuje, jak získat seznam objektů BLOB v kontejneru objektů blob. Ukázka kódu odkazy *test kontejneru objektů blob* vytvořili v části, [vytvořte kontejner objektů blob](#create-a-blob-container).

> [!NOTE]
> 
> Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment). 

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem **ListBlobs** , který vrací **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. V rámci **ListBlobs** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. K zobrazení seznamu objektů BLOB v kontejneru objektů blob, použijte **CloudBlobContainer.ListBlobs** metoda. **CloudBlobContainer.ListBlobs** metoda vrátí **IListBlobItem** objekt, který jste přetypovat **CloudBlockBlob**, **CloudPageBlob**, nebo **CloudBlobDirectory** objektu. Následující fragment kódu vytvoří výčet všech objektů BLOB v kontejneru objektů blob. Každý objekt blob je převést na příslušný objekt na základě jeho typu a jeho název (nebo identifikátor URI u **CloudBlobDirectory**) se přidá do seznamu.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Kromě objekty BLOB kontejnery objektů blob může obsahovat adresáře. Předpokládejme, že máte kontejner objektů blob s názvem *test kontejneru objektů blob* s následující hierarchie:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Použitím předchozího příkladu kódu **objekty BLOB** řetězec seznam obsahuje hodnoty podobný následujícímu:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Jak vidíte, seznam obsahuje pouze nejvyšší úrovně entity; není vnořené ty (*bar.png* a *baz.png*). Pro zobrazení seznamu všech entit v kontejneru objektů blob, musí volat **CloudBlobContainer.ListBlobs** metoda a předejte jí **true** pro **useFlatBlobListing** parametr.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Nastavení **useFlatBlobListing** parametru **true** vrátí plochý seznam všech entit v kontejneru objektů blob a poskytne následující výsledky:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.

1. Na **přidat zobrazení** dialogové okno, zadejte **ListBlobs** pro název zobrazení, vyberte **přidat**.

1. Otevřete `ListBlobs.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Spusťte aplikaci a vyberte **seznamu objektů blob** a zobrazte výsledky podobné následujícím snímku obrazovky:
  
    ![Seznam objektů BLOB](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Stáhnout objekty blob

V této části ukazuje, jak stáhnout objekt blob a buď zachovat k místnímu úložišti nebo číst obsah do řetězce. Ukázka kódu odkazy *test kontejneru objektů blob* vytvořili v části, [vytvořte kontejner objektů blob](#create-a-blob-container).

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem **DownloadBlob** , který vrací **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. V rámci **DownloadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda. (Změnit  *&lt;název objektu blob >* k názvu objektu blob stahujete.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Chcete-li stáhnout objekt blob, použijte **CloudBlockBlob.DownloadToStream** nebo **CloudPageBlob.DownloadToStream** metoda, v závislosti na typu objektu blob. Následující kód používá fragment kódu **CloudBlockBlob.DownloadToStream** způsob přenosu obsahu objektu blob na objekt proudu, který je pak trvalé do místního souboru: (Změna  *&lt;místní název souboru >* k souboru plně kvalifikovaný název představující, kam chcete objekt blob stáhli.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Spusťte aplikaci a vyberte **stažení objektů blob** ke stažení objektu blob. Zadaný v objektu blob **CloudBlobContainer.GetBlockBlobReference** volání metody stáhne do umístění, zadejte v **File.OpenWrite** volání metody. 

## <a name="delete-blobs"></a>Odstranění objektů blob

Následující kroky ukazují, jak odstranit objekt blob:

> [!NOTE]
> 
> Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment). 

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem **DeleteBlob** , který vrací **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda. (Změnit  *&lt;název objektu blob >* k názvu objektu blob, který odstraňujete.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Spusťte aplikaci a vyberte **odstranit objekt blob** odstranit zadaný v objektu blob **CloudBlobContainer.GetBlockBlobReference** volání metody. 

## <a name="next-steps"></a>Další kroky
Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.

  * [Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)](./vs-storage-aspnet-getting-started-tables.md)
  * [Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)](./vs-storage-aspnet-getting-started-queues.md)
