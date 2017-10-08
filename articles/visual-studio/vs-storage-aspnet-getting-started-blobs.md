---
title: "aaaGet začít s Azure blob storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí služby Azure blob storage po připojení účtu úložiště tooa pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraig
ms.openlocfilehash: 7b3e160da5bb95967ca4650b124afb8e867c03d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="1717a-103">Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="1717a-103">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="1717a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1717a-104">Overview</span></span>

<span data-ttu-id="1717a-105">Azure blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="1717a-105">Azure blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="1717a-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="1717a-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="1717a-107">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1717a-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="1717a-108">Tento kurz ukazuje, jak kód toowrite ASP.NET pro některé běžné scénáře použití služby Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="1717a-108">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure blob storage.</span></span> <span data-ttu-id="1717a-109">Scénáře zahrnují vytváření kontejner objektů blob a odesílání, výpis, stahování a odstraňování objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="1717a-109">Scenarios include creating a blob container, and uploading, listing, downloading, and deleting blobs.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="1717a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1717a-110">Prerequisites</span></span>

* [<span data-ttu-id="1717a-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1717a-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="1717a-112">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1717a-112">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="1717a-113">Vytvořit řadič MVC</span><span class="sxs-lookup"><span data-stu-id="1717a-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="1717a-114">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce hello, vyberte **Přidat -> řadiče**.</span><span class="sxs-lookup"><span data-stu-id="1717a-114">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![Přidat řadič tooan aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. <span data-ttu-id="1717a-116">Na hello **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1717a-116">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. <span data-ttu-id="1717a-118">Na hello **přidat kontroler** dialogové okno, název hello řadič *BlobsController*a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1717a-118">On hello **Add Controller** dialog, name hello controller *BlobsController*, and select **Add**.</span></span>

    ![Název hello MVC jsou řadič MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. <span data-ttu-id="1717a-120">Přidejte následující hello *pomocí* toohello direktivy `BlobsController.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="1717a-120">Add hello following *using* directives toohello `BlobsController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a><span data-ttu-id="1717a-121">Vytvořte kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="1717a-121">Create a blob container</span></span>

<span data-ttu-id="1717a-122">Kontejner objektů blob je hierarchie vnořených objektů BLOB a složek.</span><span class="sxs-lookup"><span data-stu-id="1717a-122">A blob container is a nested hierarchy of blobs and folders.</span></span> <span data-ttu-id="1717a-123">Hello následující kroky popisují jak toocreate kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="1717a-123">hello following steps illustrate how toocreate a blob container:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="1717a-124">Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="1717a-124">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="1717a-125">Otevřete hello `BlobsController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="1717a-125">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="1717a-126">Přidejte metodu s názvem **CreateBlobContainer** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1717a-126">Add a method called **CreateBlobContainer** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="1717a-127">V rámci hello **CreateBlobContainer** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1717a-127">Within hello **CreateBlobContainer** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="1717a-128">Použijte následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="1717a-128">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration.</span></span> <span data-ttu-id="1717a-129">(Změnit  *&lt;název účtu úložiště >* toohello název hello získáváte přístup k účtu úložiště Azure.)</span><span class="sxs-lookup"><span data-stu-id="1717a-129">(Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="1717a-130">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-130">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="1717a-131">Získání **CloudBlobContainer** objekt, který reprezentuje název odkazu toohello požadovaný objekt blob kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1717a-131">Get a **CloudBlobContainer** object that represents a reference toohello desired blob container name.</span></span> <span data-ttu-id="1717a-132">Hello **CloudBlobClient.GetContainerReference** metoda neprovede požadavek úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-132">hello **CloudBlobClient.GetContainerReference** method does not make a request against blob storage.</span></span> <span data-ttu-id="1717a-133">referenční dokumentace Hello se vrátí, zda existuje kontejner objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="1717a-133">hello reference is returned whether or not hello blob container exists.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="1717a-134">Volání hello **CloudBlobContainer.CreateIfNotExists** metoda toocreate hello kontejner, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="1717a-134">Call hello **CloudBlobContainer.CreateIfNotExists** method toocreate hello container if it does not yet exist.</span></span> <span data-ttu-id="1717a-135">Hello **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** Pokud hello kontejner neexistuje a je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="1717a-135">hello **CloudBlobContainer.CreateIfNotExists** method returns **true** if hello container does not exist, and is successfully created.</span></span> <span data-ttu-id="1717a-136">V opačném **false** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="1717a-136">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. <span data-ttu-id="1717a-137">Aktualizace hello **ViewBag** s názvem hello hello kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-137">Update hello **ViewBag** with hello name of hello blob container.</span></span>

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. <span data-ttu-id="1717a-138">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1717a-138">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="1717a-139">Na hello **přidat zobrazení** dialogové okno, zadejte **CreateBlobContainer** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1717a-139">On hello **Add View** dialog, enter **CreateBlobContainer** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="1717a-140">Otevřete `CreateBlobContainer.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1717a-140">Open `CreateBlobContainer.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="1717a-141">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1717a-141">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="1717a-142">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="1717a-142">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. <span data-ttu-id="1717a-143">Spuštění aplikace hello a vyberte **vytvořit kontejner objektů Blob** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="1717a-143">Run hello application, and select **Create Blob Container** toosee results similar toohello following screen shot:</span></span>
  
    ![Vytvoření kontejneru objektů blob](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    <span data-ttu-id="1717a-145">Jak je uvedeno nahoře, hello **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="1717a-145">As mentioned previously, hello **CloudBlobContainer.CreateIfNotExists** method returns **true** only when hello container doesn't exist and is created.</span></span> <span data-ttu-id="1717a-146">Proto pokud hello aplikaci spustíte, když hello kontejner existuje, hello metoda vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="1717a-146">Therefore, if you run hello app when hello container exists, hello method returns **false**.</span></span> <span data-ttu-id="1717a-147">aplikace hello toorun vícekrát, je nutné odstranit kontejner hello před spuštěním aplikace hello znovu.</span><span class="sxs-lookup"><span data-stu-id="1717a-147">toorun hello app multiple times, you must delete hello container before running hello app again.</span></span> <span data-ttu-id="1717a-148">Odstraňování hello kontejneru, můžete to udělat pomocí hello **CloudBlobContainer.Delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-148">Deleting hello container can be done via hello **CloudBlobContainer.Delete** method.</span></span> <span data-ttu-id="1717a-149">Můžete také odstranit kontejner hello pomocí hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="1717a-149">You can also delete hello container using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="upload-a-blob-into-a-blob-container"></a><span data-ttu-id="1717a-150">Nahrát objekt blob do kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="1717a-150">Upload a blob into a blob container</span></span>

<span data-ttu-id="1717a-151">Jakmile jste [vytvořit kontejner objektů blob](#create-a-blob-container), mohou nahrávat soubory do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1717a-151">Once you've [created a blob container](#create-a-blob-container), you can upload files into that container.</span></span> <span data-ttu-id="1717a-152">Tato část vás provede procesem odesílání kontejneru objektů blob tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="1717a-152">This section walks you through uploading a local file tooa blob container.</span></span> <span data-ttu-id="1717a-153">Postup Hello předpokládá jste vytvořili kontejner objektů blob s názvem *test kontejneru objektů blob*.</span><span class="sxs-lookup"><span data-stu-id="1717a-153">hello steps assume you've created a blob container named *test-blob-container*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="1717a-154">Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="1717a-154">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="1717a-155">Otevřete hello `BlobsController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="1717a-155">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="1717a-156">Přidejte metodu s názvem **UploadBlob** , který vrací **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="1717a-156">Add a method called **UploadBlob** that returns an **EmptyResult**.</span></span>

    ```csharp
    public EmptyResult UploadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="1717a-157">V rámci hello **UploadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1717a-157">Within hello **UploadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="1717a-158">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="1717a-158">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="1717a-159">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-159">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="1717a-160">Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="1717a-160">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="1717a-161">Jak je popsáno výše, Azure storage podporuje typy jiný objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-161">As explained earlier, Azure storage supports different blob types.</span></span> <span data-ttu-id="1717a-162">tooretrieve objekt blob stránky tooa odkaz volání hello **CloudBlobContainer.GetPageBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-162">tooretrieve a reference tooa page blob, call hello **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="1717a-163">tooretrieve objekt blob bloku tooa odkaz volání hello **CloudBlobContainer.GetBlockBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-163">tooretrieve a reference tooa block blob, call hello **CloudBlobContainer.GetBlockBlobReference** method.</span></span> <span data-ttu-id="1717a-164">Objekt blob bloku je obvykle hello doporučená toouse typu.</span><span class="sxs-lookup"><span data-stu-id="1717a-164">Usually, block blob is hello recommended type toouse.</span></span> <span data-ttu-id="1717a-165">(Změnit < název objektu blob > * toohello název objektu blob hello toogive jednou nahráli.)</span><span class="sxs-lookup"><span data-stu-id="1717a-165">(Change <blob-name>* toohello name you want toogive hello blob once uploaded.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="1717a-166">Až budete mít odkaz na objekt blob, můžete nahrát všechny tooit datový proud dat voláním hello blob odkaz objektu **UploadFromStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-166">Once you have a blob reference, you can upload any data stream tooit by calling hello blob reference object's **UploadFromStream** method.</span></span> <span data-ttu-id="1717a-167">Hello **UploadFromStream** metoda vytvoří objekt blob hello, pokud neexistuje, nebo ho přepíše, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="1717a-167">hello **UploadFromStream** method creates hello blob if it doesn't exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="1717a-168">(Změnit  *&lt;nahrávání souborů >* tooa plně kvalifikovaný toohello cestě k souboru chcete tooupload.)</span><span class="sxs-lookup"><span data-stu-id="1717a-168">(Change *&lt;file-to-upload>* tooa fully qualified path toohello file you want tooupload.)</span></span>

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. <span data-ttu-id="1717a-169">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1717a-169">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="1717a-170">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1717a-170">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="1717a-171">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="1717a-171">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="1717a-172">Spuštění aplikace hello a vyberte **nahrávání blob**.</span><span class="sxs-lookup"><span data-stu-id="1717a-172">Run hello application, and select **Upload blob**.</span></span>  
  
<span data-ttu-id="1717a-173">Hello části - [seznamu hello objektů BLOB v kontejneru objektů blob](#list-the-blobs-in-a-blob-container) -ukazuje, jak toolist hello objektů BLOB v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-173">hello section - [List hello blobs in a blob container](#list-the-blobs-in-a-blob-container) - illustrates how toolist hello blobs in a blob container.</span></span>  

## <a name="list-hello-blobs-in-a-blob-container"></a><span data-ttu-id="1717a-174">Seznam hello objektů BLOB v kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="1717a-174">List hello blobs in a blob container</span></span>

<span data-ttu-id="1717a-175">Tato část ukazuje, jak toolist hello objektů BLOB v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-175">This section illustrates how toolist hello blobs in a blob container.</span></span> <span data-ttu-id="1717a-176">Ukázkový kód Hello odkazuje hello *test kontejneru objektů blob* vytvořili v části hello [vytvořte kontejner objektů blob](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="1717a-176">hello sample code references hello *test-blob-container* created in hello section, [Create a blob container](#create-a-blob-container).</span></span>

> [!NOTE]
> 
> <span data-ttu-id="1717a-177">Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="1717a-177">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="1717a-178">Otevřete hello `BlobsController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="1717a-178">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="1717a-179">Přidejte metodu s názvem **ListBlobs** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1717a-179">Add a method called **ListBlobs** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ListBlobs()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="1717a-180">V rámci hello **ListBlobs** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1717a-180">Within hello **ListBlobs** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="1717a-181">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="1717a-181">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="1717a-182">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-182">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="1717a-183">Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="1717a-183">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="1717a-184">toolist hello objektů BLOB v kontejneru objektů blob, použijte hello **CloudBlobContainer.ListBlobs** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-184">toolist hello blobs in a blob container, use hello **CloudBlobContainer.ListBlobs** method.</span></span> <span data-ttu-id="1717a-185">Hello **CloudBlobContainer.ListBlobs** metoda vrátí **IListBlobItem** objektu, že vložíte tooa **CloudBlockBlob**, **CloudPageBlob**, nebo **CloudBlobDirectory** objektu.</span><span class="sxs-lookup"><span data-stu-id="1717a-185">hello **CloudBlobContainer.ListBlobs** method returns an **IListBlobItem** object that you cast tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="1717a-186">Hello následující fragment kódu vytvoří výčet všech hello objektů BLOB v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-186">hello following code snippet enumerates all hello blobs in a blob container.</span></span> <span data-ttu-id="1717a-187">Každý objekt blob je přetypování toohello odpovídající objekt v závislosti na její typ a jeho název (nebo identifikátor URI v případě hello **CloudBlobDirectory**) je přidána tooa seznamu.</span><span class="sxs-lookup"><span data-stu-id="1717a-187">Each blob is cast toohello appropriate object based on its type, and its name (or URI in hello case of a **CloudBlobDirectory**) is added tooa list.</span></span>

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

    <span data-ttu-id="1717a-188">V přidání tooblobs kontejnery objektů blob může obsahovat adresáře.</span><span class="sxs-lookup"><span data-stu-id="1717a-188">In addition tooblobs, blob containers can contain directories.</span></span> <span data-ttu-id="1717a-189">Předpokládejme, že máte kontejner objektů blob s názvem *test kontejneru objektů blob* s hello následující hierarchie:</span><span class="sxs-lookup"><span data-stu-id="1717a-189">Let's suppose you have a blob container called *test-blob-container* with hello following hierarchy:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

    <span data-ttu-id="1717a-190">Pomocí hello předcházející příklad kódu, hello **objekty BLOB** řetězec seznam obsahuje podobné toohello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1717a-190">Using hello preceding code example, hello **blobs** string list contains values similar toohello following:</span></span>

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    <span data-ttu-id="1717a-191">Jak vidíte, hello seznam obsahuje jenom hello nejvyšší úrovně entity; není hello těch, které jsou vnořené (*bar.png* a *baz.png*).</span><span class="sxs-lookup"><span data-stu-id="1717a-191">As you can see, hello list includes only hello top-level entities; not hello nested ones (*bar.png* and *baz.png*).</span></span> <span data-ttu-id="1717a-192">toolist všechny hello entity v kontejneru objektů blob, musí volat hello **CloudBlobContainer.ListBlobs** metoda a předejte jí **true** pro hello **useFlatBlobListing** parametr.</span><span class="sxs-lookup"><span data-stu-id="1717a-192">toolist all hello entities within a blob container, you must call hello **CloudBlobContainer.ListBlobs** method and pass **true** for hello **useFlatBlobListing** parameter.</span></span>    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    <span data-ttu-id="1717a-193">Nastavení hello **useFlatBlobListing** parametr příliš**true** vrátí plochý seznam všech entit v kontejneru objektů blob hello a vypočítá hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="1717a-193">Setting hello **useFlatBlobListing** parameter too**true** returns a flat listing of all entities in hello blob container, and yields hello following results:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

1. <span data-ttu-id="1717a-194">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1717a-194">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="1717a-195">Na hello **přidat zobrazení** dialogové okno, zadejte **ListBlobs** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1717a-195">On hello **Add View** dialog, enter **ListBlobs** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="1717a-196">Otevřete `ListBlobs.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1717a-196">Open `ListBlobs.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="1717a-197">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1717a-197">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="1717a-198">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="1717a-198">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. <span data-ttu-id="1717a-199">Spuštění aplikace hello a vyberte **seznamu objektů blob** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="1717a-199">Run hello application, and select **List blobs** toosee results similar toohello following screen shot:</span></span>
  
    ![Seznam objektů BLOB](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a><span data-ttu-id="1717a-201">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="1717a-201">Download blobs</span></span>

<span data-ttu-id="1717a-202">Tato část ukazuje, jak toodownload objekt blob a buď zachovat ho toolocal úložiště nebo pro čtení hello obsah do řetězce.</span><span class="sxs-lookup"><span data-stu-id="1717a-202">This section illustrates how toodownload a blob and either persist it toolocal storage or read hello contents into a string.</span></span> <span data-ttu-id="1717a-203">Ukázkový kód Hello odkazuje hello *test kontejneru objektů blob* vytvořili v části hello [vytvořte kontejner objektů blob](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="1717a-203">hello sample code references hello *test-blob-container* created in hello section, [Create a blob container](#create-a-blob-container).</span></span>

1. <span data-ttu-id="1717a-204">Otevřete hello `BlobsController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="1717a-204">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="1717a-205">Přidejte metodu s názvem **DownloadBlob** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1717a-205">Add a method called **DownloadBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="1717a-206">V rámci hello **DownloadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1717a-206">Within hello **DownloadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="1717a-207">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="1717a-207">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="1717a-208">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-208">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="1717a-209">Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="1717a-209">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="1717a-210">Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-210">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="1717a-211">(Změnit  *&lt;název objektu blob >* toohello název objektu blob hello stahujete.)</span><span class="sxs-lookup"><span data-stu-id="1717a-211">(Change *&lt;blob-name>* toohello name of hello blob you are downloading.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="1717a-212">toodownload objekt blob, použijte hello **CloudBlockBlob.DownloadToStream** nebo **CloudPageBlob.DownloadToStream** metoda, v závislosti na typu objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="1717a-212">toodownload a blob, use hello **CloudBlockBlob.DownloadToStream** or **CloudPageBlob.DownloadToStream** method, depending on hello blob type.</span></span> <span data-ttu-id="1717a-213">Hello následující fragment kódu používá hello **CloudBlockBlob.DownloadToStream** tootransfer metoda datového proudu tooa objekt blob obsah objektu který je pak trvalé místního souboru tooa: (Změna  *&lt;název místního souboru >* toohello plně kvalifikovaný, místo, kam chcete stáhnout blob hello představující název souboru.)</span><span class="sxs-lookup"><span data-stu-id="1717a-213">hello following code snippet uses hello **CloudBlockBlob.DownloadToStream** method tootransfer a blob's contents tooa stream object that is then persisted tooa local file: (Change *&lt;local-file-name>* toohello fully qualified file name representing where you want hello blob downloaded.)</span></span> 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. <span data-ttu-id="1717a-214">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1717a-214">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="1717a-215">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="1717a-215">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="1717a-216">Spuštění aplikace hello a vyberte **stažení objektů blob** toodownload hello blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-216">Run hello application, and select **Download blob** toodownload hello blob.</span></span> <span data-ttu-id="1717a-217">Objekt blob Hello zadaný v hello **CloudBlobContainer.GetBlockBlobReference** volání metody stáhne toohello umístění v hello **File.OpenWrite** volání metody.</span><span class="sxs-lookup"><span data-stu-id="1717a-217">hello blob specified in hello **CloudBlobContainer.GetBlockBlobReference** method call downloads toohello location you specify in hello **File.OpenWrite** method call.</span></span> 

## <a name="delete-blobs"></a><span data-ttu-id="1717a-218">Odstranění objektů blob</span><span class="sxs-lookup"><span data-stu-id="1717a-218">Delete blobs</span></span>

<span data-ttu-id="1717a-219">Hello následující kroky popisují jak toodelete objekt blob:</span><span class="sxs-lookup"><span data-stu-id="1717a-219">hello following steps illustrate how toodelete a blob:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="1717a-220">Hello kódu v této části se předpokládá, že jste dokončili hello kroky v části hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="1717a-220">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="1717a-221">Otevřete hello `BlobsController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="1717a-221">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="1717a-222">Přidejte metodu s názvem **DeleteBlob** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1717a-222">Add a method called **DeleteBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```

1. <span data-ttu-id="1717a-223">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1717a-223">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="1717a-224">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="1717a-224">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="1717a-225">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1717a-225">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="1717a-226">Získání **CloudBlobContainer** objekt, který představuje název kontejneru objektu blob toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="1717a-226">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="1717a-227">Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="1717a-227">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="1717a-228">(Změnit  *&lt;název objektu blob >* toohello název objektu blob hello odstraňujete.)</span><span class="sxs-lookup"><span data-stu-id="1717a-228">(Change *&lt;blob-name>* toohello name of hello blob you are deleting.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. toodelete a blob, use hello **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. <span data-ttu-id="1717a-229">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1717a-229">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="1717a-230">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="1717a-230">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="1717a-231">Spuštění aplikace hello a vyberte **odstranit objekt blob** toodelete hello blob zadaný v hello **CloudBlobContainer.GetBlockBlobReference** volání metody.</span><span class="sxs-lookup"><span data-stu-id="1717a-231">Run hello application, and select **Delete blob** toodelete hello blob specified in hello **CloudBlobContainer.GetBlockBlobReference** method call.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1717a-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1717a-232">Next steps</span></span>
<span data-ttu-id="1717a-233">Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="1717a-233">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="1717a-234">Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="1717a-234">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-tables.md)
  * [<span data-ttu-id="1717a-235">Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="1717a-235">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-queues.md)
