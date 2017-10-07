---
title: "aaaGet začít s Azure blob storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí služby Azure blob storage po připojení účtu úložiště tooa pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
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
ms.openlocfilehash: eb38889f239a63852d6928e8be10c3d3f1746e9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled

Azure blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

Tento kurz ukazuje, jak kód toowrite ASP.NET pro některé běžné scénáře použití služby Azure blob storage. Scénáře zahrnují vytváření kontejner objektů blob a odesílání, výpis, stahování a odstraňování objektů BLOB.

##<a name="prerequisites"></a>Požadavky

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Účet služby Azure Storage](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Vytvořit řadič MVC 

1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce hello, vyberte **Přidat -> řadiče**.

    ![Přidat řadič tooan aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. Na hello **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Na hello **přidat kontroler** dialogové okno, název hello řadič *BlobsController*a vyberte **přidat**.

    ![Název hello MVC jsou řadič MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Přidejte následující hello *pomocí* toohello direktivy `BlobsController.cs` souboru:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Vytvořte kontejner objektů blob

Kontejner objektů blob je hierarchie vnořených objektů BLOB a složek. Hello následující kroky popisují jak toocreate kontejner objektů blob:

> [!NOTE]
> 
> Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `BlobsController.cs` souboru.

1. Přidejte metodu s názvem **CreateBlobContainer** , který vrací **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **CreateBlobContainer** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použijte následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello hello. (Změnit  *&lt;název účtu úložiště >* toohello název hello získáváte přístup k účtu úložiště Azure.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který reprezentuje název odkazu toohello požadovaný objekt blob kontejneru. Hello **CloudBlobClient.GetContainerReference** metoda neprovede požadavek úložiště objektů blob. referenční dokumentace Hello se vrátí, zda existuje kontejner objektů blob hello. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Volání hello **CloudBlobContainer.CreateIfNotExists** metoda toocreate hello kontejner, pokud ještě neexistuje. Hello **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** Pokud hello kontejner neexistuje a je úspěšně vytvořen. V opačném **false** je vrácen.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Aktualizace hello **ViewBag** s názvem hello hello kontejner objektů blob.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **CreateBlobContainer** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `CreateBlobContainer.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Spuštění aplikace hello a vyberte **vytvořit kontejner objektů Blob** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Vytvoření kontejneru objektů blob](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Jak je uvedeno nahoře, hello **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření kontejneru hello. Proto pokud hello aplikaci spustíte, když hello kontejner existuje, hello metoda vrátí **false**. aplikace hello toorun vícekrát, je nutné odstranit kontejner hello před spuštěním aplikace hello znovu. Odstraňování hello kontejneru, můžete to udělat pomocí hello **CloudBlobContainer.Delete** metoda. Můžete také odstranit kontejner hello pomocí hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Nahrát objekt blob do kontejneru objektů blob

Jakmile jste [vytvořit kontejner objektů blob](#create-a-blob-container), mohou nahrávat soubory do tohoto kontejneru. Tato část vás provede procesem odesílání kontejneru objektů blob tooa místního souboru. Postup Hello předpokládá jste vytvořili kontejner objektů blob s názvem *test kontejneru objektů blob*. 

> [!NOTE]
> 
> Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `BlobsController.cs` souboru.

1. Přidejte metodu s názvem **UploadBlob** , který vrací **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. V rámci hello **UploadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Jak je popsáno výše, Azure storage podporuje typy jiný objektu blob. tooretrieve objekt blob stránky tooa odkaz volání hello **CloudBlobContainer.GetPageBlobReference** metoda. tooretrieve objekt blob bloku tooa odkaz volání hello **CloudBlobContainer.GetBlockBlobReference** metoda. Objekt blob bloku je obvykle hello doporučená toouse typu. (Změnit < název objektu blob > * toohello název objektu blob hello toogive jednou nahráli.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Až budete mít odkaz na objekt blob, můžete nahrát všechny tooit datový proud dat voláním hello blob odkaz objektu **UploadFromStream** metoda. Hello **UploadFromStream** metoda vytvoří objekt blob hello, pokud neexistuje, nebo ho přepíše, pokud neexistuje. (Změnit  *&lt;nahrávání souborů >* tooa plně kvalifikovaný toohello cestě k souboru chcete tooupload.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Spuštění aplikace hello a vyberte **nahrávání blob**.  
  
Hello části - [seznamu hello objektů BLOB v kontejneru objektů blob](#list-the-blobs-in-a-blob-container) -ukazuje, jak toolist hello objektů BLOB v kontejneru objektů blob.  

## <a name="list-hello-blobs-in-a-blob-container"></a>Seznam hello objektů BLOB v kontejneru objektů blob

Tato část ukazuje, jak toolist hello objektů BLOB v kontejneru objektů blob. Ukázkový kód Hello odkazuje hello *test kontejneru objektů blob* vytvořili v části hello [vytvořte kontejner objektů blob](#create-a-blob-container).

> [!NOTE]
> 
> Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `BlobsController.cs` souboru.

1. Přidejte metodu s názvem **ListBlobs** , který vrací **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **ListBlobs** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. toolist hello objektů BLOB v kontejneru objektů blob, použijte hello **CloudBlobContainer.ListBlobs** metoda. Hello **CloudBlobContainer.ListBlobs** metoda vrátí **IListBlobItem** objektu, že vložíte tooa **CloudBlockBlob**, **CloudPageBlob**, nebo **CloudBlobDirectory** objektu. Hello následující fragment kódu vytvoří výčet všech hello objektů BLOB v kontejneru objektů blob. Každý objekt blob je přetypování toohello odpovídající objekt v závislosti na její typ a jeho název (nebo identifikátor URI v případě hello **CloudBlobDirectory**) je přidána tooa seznamu.

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

    V přidání tooblobs kontejnery objektů blob může obsahovat adresáře. Předpokládejme, že máte kontejner objektů blob s názvem *test kontejneru objektů blob* s hello následující hierarchie:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Pomocí hello předcházející příklad kódu, hello **objekty BLOB** řetězec seznam obsahuje podobné toohello následující hodnoty:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Jak vidíte, hello seznam obsahuje jenom hello nejvyšší úrovně entity; není hello těch, které jsou vnořené (*bar.png* a *baz.png*). toolist všechny hello entity v kontejneru objektů blob, musí volat hello **CloudBlobContainer.ListBlobs** metoda a předejte jí **true** pro hello **useFlatBlobListing** parametr.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Nastavení hello **useFlatBlobListing** parametr příliš**true** vrátí plochý seznam všech entit v kontejneru objektů blob hello a vypočítá hello následující výsledky:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **ListBlobs** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `ListBlobs.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

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

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Spuštění aplikace hello a vyberte **seznamu objektů blob** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Seznam objektů BLOB](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Stáhnout objekty blob

Tato část ukazuje, jak toodownload objekt blob a buď zachovat ho toolocal úložiště nebo pro čtení hello obsah do řetězce. Ukázkový kód Hello odkazuje hello *test kontejneru objektů blob* vytvořili v části hello [vytvořte kontejner objektů blob](#create-a-blob-container).

1. Otevřete hello `BlobsController.cs` souboru.

1. Přidejte metodu s názvem **DownloadBlob** , který vrací **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. V rámci hello **DownloadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda. (Změnit  *&lt;název objektu blob >* toohello název objektu blob hello stahujete.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. toodownload objekt blob, použijte hello **CloudBlockBlob.DownloadToStream** nebo **CloudPageBlob.DownloadToStream** metoda, v závislosti na typu objektu blob hello. Hello následující fragment kódu používá hello **CloudBlockBlob.DownloadToStream** tootransfer metoda datového proudu tooa objekt blob obsah objektu který je pak trvalé místního souboru tooa: (Změna  *&lt;název místního souboru >* toohello plně kvalifikovaný, místo, kam chcete stáhnout blob hello představující název souboru.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Spuštění aplikace hello a vyberte **stažení objektů blob** toodownload hello blob. Objekt blob Hello zadaný v hello **CloudBlobContainer.GetBlockBlobReference** volání metody stáhne toohello umístění v hello **File.OpenWrite** volání metody. 

## <a name="delete-blobs"></a>Odstranění objektů blob

Hello následující kroky popisují jak toodelete objekt blob:

> [!NOTE]
> 
> Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `BlobsController.cs` souboru.

1. Přidejte metodu s názvem **DeleteBlob** , který vrací **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda. (Změnit  *&lt;název objektu blob >* toohello název objektu blob hello odstraňujete.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. toodelete a blob, use hello **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Spuštění aplikace hello a vyberte **odstranit objekt blob** toodelete hello blob zadaný v hello **CloudBlobContainer.GetBlockBlobReference** volání metody. 

## <a name="next-steps"></a>Další kroky
Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.

  * [Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)](./vs-storage-aspnet-getting-started-tables.md)
  * [Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)](./vs-storage-aspnet-getting-started-queues.md)
