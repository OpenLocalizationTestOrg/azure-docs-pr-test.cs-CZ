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
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="e0fca-103">Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e0fca-103">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="e0fca-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e0fca-104">Overview</span></span>

<span data-ttu-id="e0fca-105">Azure blob storage je služba, která ukládá Nestrukturovaná data v cloudu jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="e0fca-105">Azure blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="e0fca-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0fca-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="e0fca-107">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="e0fca-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="e0fca-108">Tento kurz ukazuje, jak napsat kód ASP.NET pro některé běžné scénáře použití služby Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="e0fca-108">This tutorial shows how to write ASP.NET code for some common scenarios using Azure blob storage.</span></span> <span data-ttu-id="e0fca-109">Scénáře zahrnují vytváření kontejner objektů blob a odesílání, výpis, stahování a odstraňování objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="e0fca-109">Scenarios include creating a blob container, and uploading, listing, downloading, and deleting blobs.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="e0fca-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0fca-110">Prerequisites</span></span>

* [<span data-ttu-id="e0fca-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0fca-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="e0fca-112">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e0fca-112">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="e0fca-113">Vytvořit řadič MVC</span><span class="sxs-lookup"><span data-stu-id="e0fca-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="e0fca-114">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce vyberte **Přidat -> řadiče**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-114">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Přidat řadič do aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. <span data-ttu-id="e0fca-116">Na **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-116">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. <span data-ttu-id="e0fca-118">Na **přidat kontroler** dialogové okno, názvu kontroleru *BlobsController*a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-118">On the **Add Controller** dialog, name the controller *BlobsController*, and select **Add**.</span></span>

    ![Název řadiče MVC](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. <span data-ttu-id="e0fca-120">Přidejte následující *pomocí* direktivy pro `BlobsController.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="e0fca-120">Add the following *using* directives to the `BlobsController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a><span data-ttu-id="e0fca-121">Vytvořte kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="e0fca-121">Create a blob container</span></span>

<span data-ttu-id="e0fca-122">Kontejner objektů blob je hierarchie vnořených objektů BLOB a složek.</span><span class="sxs-lookup"><span data-stu-id="e0fca-122">A blob container is a nested hierarchy of blobs and folders.</span></span> <span data-ttu-id="e0fca-123">Následující kroky ukazují, jak vytvořit kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="e0fca-123">The following steps illustrate how to create a blob container:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="e0fca-124">Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="e0fca-124">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="e0fca-125">Otevřete soubor `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-125">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="e0fca-126">Přidejte metodu s názvem **CreateBlobContainer** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-126">Add a method called **CreateBlobContainer** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="e0fca-127">V rámci **CreateBlobContainer** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0fca-127">Within the **CreateBlobContainer** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="e0fca-128">Použijte následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e0fca-128">Use the following code to get the storage connection string and storage account information from the Azure service configuration.</span></span> <span data-ttu-id="e0fca-129">(Změnit  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-129">(Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="e0fca-130">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-130">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="e0fca-131">Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název požadovaného objektu blob kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e0fca-131">Get a **CloudBlobContainer** object that represents a reference to the desired blob container name.</span></span> <span data-ttu-id="e0fca-132">**CloudBlobClient.GetContainerReference** metoda neprovede požadavek úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-132">The **CloudBlobClient.GetContainerReference** method does not make a request against blob storage.</span></span> <span data-ttu-id="e0fca-133">Odkaz se vrátí, zda existuje kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-133">The reference is returned whether or not the blob container exists.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="e0fca-134">Volání **CloudBlobContainer.CreateIfNotExists** metodu pro vytvoření kontejneru, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e0fca-134">Call the **CloudBlobContainer.CreateIfNotExists** method to create the container if it does not yet exist.</span></span> <span data-ttu-id="e0fca-135">**CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** Pokud kontejner neexistuje a je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="e0fca-135">The **CloudBlobContainer.CreateIfNotExists** method returns **true** if the container does not exist, and is successfully created.</span></span> <span data-ttu-id="e0fca-136">V opačném **false** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="e0fca-136">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. <span data-ttu-id="e0fca-137">Aktualizace **ViewBag** s názvem kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-137">Update the **ViewBag** with the name of the blob container.</span></span>

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. <span data-ttu-id="e0fca-138">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-138">In the **Solution Explorer**, expand the **Views** folder, right-click **Blobs**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="e0fca-139">Na **přidat zobrazení** dialogové okno, zadejte **CreateBlobContainer** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-139">On the **Add View** dialog, enter **CreateBlobContainer** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="e0fca-140">Otevřete `CreateBlobContainer.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="e0fca-140">Open `CreateBlobContainer.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="e0fca-141">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-141">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="e0fca-142">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="e0fca-142">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. <span data-ttu-id="e0fca-143">Spusťte aplikaci a vyberte **vytvořit kontejner objektů Blob** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e0fca-143">Run the application, and select **Create Blob Container** to see results similar to the following screen shot:</span></span>
  
    ![Vytvoření kontejneru objektů blob](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    <span data-ttu-id="e0fca-145">Jak je uvedeno nahoře, **CloudBlobContainer.CreateIfNotExists** metoda vrátí **true** pouze když kontejner neexistuje, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="e0fca-145">As mentioned previously, the **CloudBlobContainer.CreateIfNotExists** method returns **true** only when the container doesn't exist and is created.</span></span> <span data-ttu-id="e0fca-146">Proto pokud spustíte aplikaci kontejneru existuje, vrátí metoda **false**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-146">Therefore, if you run the app when the container exists, the method returns **false**.</span></span> <span data-ttu-id="e0fca-147">Aplikace je třeba spustit vícekrát, je nutné odstranit kontejner před spuštěním aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="e0fca-147">To run the app multiple times, you must delete the container before running the app again.</span></span> <span data-ttu-id="e0fca-148">Odstranění kontejneru, můžete to udělat pomocí **CloudBlobContainer.Delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-148">Deleting the container can be done via the **CloudBlobContainer.Delete** method.</span></span> <span data-ttu-id="e0fca-149">Můžete také odstranit kontejner pomocí [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="e0fca-149">You can also delete the container using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="upload-a-blob-into-a-blob-container"></a><span data-ttu-id="e0fca-150">Nahrát objekt blob do kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="e0fca-150">Upload a blob into a blob container</span></span>

<span data-ttu-id="e0fca-151">Jakmile jste [vytvořit kontejner objektů blob](#create-a-blob-container), mohou nahrávat soubory do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e0fca-151">Once you've [created a blob container](#create-a-blob-container), you can upload files into that container.</span></span> <span data-ttu-id="e0fca-152">Tato část vás provede nahráním místního souboru do kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-152">This section walks you through uploading a local file to a blob container.</span></span> <span data-ttu-id="e0fca-153">Postup předpokládá, že jste vytvořili kontejner objektů blob s názvem *test kontejneru objektů blob*.</span><span class="sxs-lookup"><span data-stu-id="e0fca-153">The steps assume you've created a blob container named *test-blob-container*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="e0fca-154">Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="e0fca-154">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="e0fca-155">Otevřete soubor `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-155">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="e0fca-156">Přidejte metodu s názvem **UploadBlob** , který vrací **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-156">Add a method called **UploadBlob** that returns an **EmptyResult**.</span></span>

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="e0fca-157">V rámci **UploadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0fca-157">Within the **UploadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="e0fca-158">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-158">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="e0fca-159">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-159">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="e0fca-160">Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-160">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="e0fca-161">Jak je popsáno výše, Azure storage podporuje typy jiný objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-161">As explained earlier, Azure storage supports different blob types.</span></span> <span data-ttu-id="e0fca-162">Chcete-li získat odkaz na objekt blob stránky, zavolejte **CloudBlobContainer.GetPageBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-162">To retrieve a reference to a page blob, call the **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="e0fca-163">Chcete-li získat odkaz na objekt blob bloku, zavolejte **CloudBlobContainer.GetBlockBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-163">To retrieve a reference to a block blob, call the **CloudBlobContainer.GetBlockBlobReference** method.</span></span> <span data-ttu-id="e0fca-164">Typ k použití doporučuje je obvykle objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="e0fca-164">Usually, block blob is the recommended type to use.</span></span> <span data-ttu-id="e0fca-165">(Změnit < název objektu blob > * k názvu chcete poskytnout jednou nahrát objekt blob.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-165">(Change <blob-name>* to the name you want to give the blob once uploaded.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="e0fca-166">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat k němu voláním odkaz na objekt blob **UploadFromStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-166">Once you have a blob reference, you can upload any data stream to it by calling the blob reference object's **UploadFromStream** method.</span></span> <span data-ttu-id="e0fca-167">**UploadFromStream** metoda vytvoří objekt blob, pokud neexistuje, nebo ho přepíše, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e0fca-167">The **UploadFromStream** method creates the blob if it doesn't exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="e0fca-168">(Změnit  *&lt;nahrávání souborů >* na plně kvalifikovanou cestu k souboru, který chcete odeslat.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-168">(Change *&lt;file-to-upload>* to a fully qualified path to the file you want to upload.)</span></span>

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. <span data-ttu-id="e0fca-169">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-169">In the **Solution Explorer**, expand the **Views** folder, right-click **Blobs**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="e0fca-170">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-170">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="e0fca-171">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="e0fca-171">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="e0fca-172">Spusťte aplikaci a vyberte **nahrávání blob**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-172">Run the application, and select **Upload blob**.</span></span>  
  
<span data-ttu-id="e0fca-173">V části - [seznamu objektů BLOB v kontejneru objektů blob](#list-the-blobs-in-a-blob-container) -znázorňuje, jak zobrazit seznam objektů BLOB v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-173">The section - [List the blobs in a blob container](#list-the-blobs-in-a-blob-container) - illustrates how to list the blobs in a blob container.</span></span>    

## <a name="list-the-blobs-in-a-blob-container"></a><span data-ttu-id="e0fca-174">Seznam objektů BLOB v kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="e0fca-174">List the blobs in a blob container</span></span>

<span data-ttu-id="e0fca-175">Tato část ukazuje, jak získat seznam objektů BLOB v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-175">This section illustrates how to list the blobs in a blob container.</span></span> <span data-ttu-id="e0fca-176">Ukázka kódu odkazy *test kontejneru objektů blob* vytvořili v části, [vytvořte kontejner objektů blob](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="e0fca-176">The sample code references the *test-blob-container* created in the section, [Create a blob container](#create-a-blob-container).</span></span>

> [!NOTE]
> 
> <span data-ttu-id="e0fca-177">Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="e0fca-177">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="e0fca-178">Otevřete soubor `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-178">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="e0fca-179">Přidejte metodu s názvem **ListBlobs** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-179">Add a method called **ListBlobs** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="e0fca-180">V rámci **ListBlobs** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0fca-180">Within the **ListBlobs** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="e0fca-181">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-181">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="e0fca-182">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-182">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="e0fca-183">Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-183">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="e0fca-184">K zobrazení seznamu objektů BLOB v kontejneru objektů blob, použijte **CloudBlobContainer.ListBlobs** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-184">To list the blobs in a blob container, use the **CloudBlobContainer.ListBlobs** method.</span></span> <span data-ttu-id="e0fca-185">**CloudBlobContainer.ListBlobs** metoda vrátí **IListBlobItem** objekt, který jste přetypovat **CloudBlockBlob**, **CloudPageBlob**, nebo **CloudBlobDirectory** objektu.</span><span class="sxs-lookup"><span data-stu-id="e0fca-185">The **CloudBlobContainer.ListBlobs** method returns an **IListBlobItem** object that you cast to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="e0fca-186">Následující fragment kódu vytvoří výčet všech objektů BLOB v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-186">The following code snippet enumerates all the blobs in a blob container.</span></span> <span data-ttu-id="e0fca-187">Každý objekt blob je převést na příslušný objekt na základě jeho typu a jeho název (nebo identifikátor URI u **CloudBlobDirectory**) se přidá do seznamu.</span><span class="sxs-lookup"><span data-stu-id="e0fca-187">Each blob is cast to the appropriate object based on its type, and its name (or URI in the case of a **CloudBlobDirectory**) is added to a list.</span></span>

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

    <span data-ttu-id="e0fca-188">Kromě objekty BLOB kontejnery objektů blob může obsahovat adresáře.</span><span class="sxs-lookup"><span data-stu-id="e0fca-188">In addition to blobs, blob containers can contain directories.</span></span> <span data-ttu-id="e0fca-189">Předpokládejme, že máte kontejner objektů blob s názvem *test kontejneru objektů blob* s následující hierarchie:</span><span class="sxs-lookup"><span data-stu-id="e0fca-189">Let's suppose you have a blob container called *test-blob-container* with the following hierarchy:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

    <span data-ttu-id="e0fca-190">Použitím předchozího příkladu kódu **objekty BLOB** řetězec seznam obsahuje hodnoty podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="e0fca-190">Using the preceding code example, the **blobs** string list contains values similar to the following:</span></span>

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    <span data-ttu-id="e0fca-191">Jak vidíte, seznam obsahuje pouze nejvyšší úrovně entity; není vnořené ty (*bar.png* a *baz.png*).</span><span class="sxs-lookup"><span data-stu-id="e0fca-191">As you can see, the list includes only the top-level entities; not the nested ones (*bar.png* and *baz.png*).</span></span> <span data-ttu-id="e0fca-192">Pro zobrazení seznamu všech entit v kontejneru objektů blob, musí volat **CloudBlobContainer.ListBlobs** metoda a předejte jí **true** pro **useFlatBlobListing** parametr.</span><span class="sxs-lookup"><span data-stu-id="e0fca-192">To list all the entities within a blob container, you must call the **CloudBlobContainer.ListBlobs** method and pass **true** for the **useFlatBlobListing** parameter.</span></span>    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    <span data-ttu-id="e0fca-193">Nastavení **useFlatBlobListing** parametru **true** vrátí plochý seznam všech entit v kontejneru objektů blob a poskytne následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="e0fca-193">Setting the **useFlatBlobListing** parameter to **true** returns a flat listing of all entities in the blob container, and yields the following results:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

1. <span data-ttu-id="e0fca-194">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-194">In the **Solution Explorer**, expand the **Views** folder, right-click **Blobs**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="e0fca-195">Na **přidat zobrazení** dialogové okno, zadejte **ListBlobs** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-195">On the **Add View** dialog, enter **ListBlobs** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="e0fca-196">Otevřete `ListBlobs.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="e0fca-196">Open `ListBlobs.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="e0fca-197">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-197">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="e0fca-198">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="e0fca-198">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. <span data-ttu-id="e0fca-199">Spusťte aplikaci a vyberte **seznamu objektů blob** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e0fca-199">Run the application, and select **List blobs** to see results similar to the following screen shot:</span></span>
  
    ![Seznam objektů BLOB](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a><span data-ttu-id="e0fca-201">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="e0fca-201">Download blobs</span></span>

<span data-ttu-id="e0fca-202">V této části ukazuje, jak stáhnout objekt blob a buď zachovat k místnímu úložišti nebo číst obsah do řetězce.</span><span class="sxs-lookup"><span data-stu-id="e0fca-202">This section illustrates how to download a blob and either persist it to local storage or read the contents into a string.</span></span> <span data-ttu-id="e0fca-203">Ukázka kódu odkazy *test kontejneru objektů blob* vytvořili v části, [vytvořte kontejner objektů blob](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="e0fca-203">The sample code references the *test-blob-container* created in the section, [Create a blob container](#create-a-blob-container).</span></span>

1. <span data-ttu-id="e0fca-204">Otevřete soubor `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-204">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="e0fca-205">Přidejte metodu s názvem **DownloadBlob** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-205">Add a method called **DownloadBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="e0fca-206">V rámci **DownloadBlob** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0fca-206">Within the **DownloadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="e0fca-207">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-207">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="e0fca-208">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-208">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="e0fca-209">Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-209">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="e0fca-210">Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-210">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="e0fca-211">(Změnit  *&lt;název objektu blob >* k názvu objektu blob stahujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-211">(Change *&lt;blob-name>* to the name of the blob you are downloading.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="e0fca-212">Chcete-li stáhnout objekt blob, použijte **CloudBlockBlob.DownloadToStream** nebo **CloudPageBlob.DownloadToStream** metoda, v závislosti na typu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-212">To download a blob, use the **CloudBlockBlob.DownloadToStream** or **CloudPageBlob.DownloadToStream** method, depending on the blob type.</span></span> <span data-ttu-id="e0fca-213">Následující kód používá fragment kódu **CloudBlockBlob.DownloadToStream** způsob přenosu obsahu objektu blob na objekt proudu, který je pak trvalé do místního souboru: (Změna  *&lt;místní název souboru >* k souboru plně kvalifikovaný název představující, kam chcete objekt blob stáhli.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-213">The following code snippet uses the **CloudBlockBlob.DownloadToStream** method to transfer a blob's contents to a stream object that is then persisted to a local file: (Change *&lt;local-file-name>* to the fully qualified file name representing where you want the blob downloaded.)</span></span> 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. <span data-ttu-id="e0fca-214">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-214">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="e0fca-215">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="e0fca-215">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="e0fca-216">Spusťte aplikaci a vyberte **stažení objektů blob** ke stažení objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-216">Run the application, and select **Download blob** to download the blob.</span></span> <span data-ttu-id="e0fca-217">Zadaný v objektu blob **CloudBlobContainer.GetBlockBlobReference** volání metody stáhne do umístění, zadejte v **File.OpenWrite** volání metody.</span><span class="sxs-lookup"><span data-stu-id="e0fca-217">The blob specified in the **CloudBlobContainer.GetBlockBlobReference** method call downloads to the location you specify in the **File.OpenWrite** method call.</span></span> 

## <a name="delete-blobs"></a><span data-ttu-id="e0fca-218">Odstranění objektů blob</span><span class="sxs-lookup"><span data-stu-id="e0fca-218">Delete blobs</span></span>

<span data-ttu-id="e0fca-219">Následující kroky ukazují, jak odstranit objekt blob:</span><span class="sxs-lookup"><span data-stu-id="e0fca-219">The following steps illustrate how to delete a blob:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="e0fca-220">Kód v této části se předpokládá, že jste dokončili kroky v části [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="e0fca-220">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="e0fca-221">Otevřete soubor `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-221">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="e0fca-222">Přidejte metodu s názvem **DeleteBlob** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e0fca-222">Add a method called **DeleteBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. <span data-ttu-id="e0fca-223">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0fca-223">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="e0fca-224">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-224">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="e0fca-225">Získání **CloudBlobClient** objekt představuje klienta služby objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-225">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="e0fca-226">Získání **CloudBlobContainer** objekt, který reprezentuje odkaz na název kontejneru objektu blob.</span><span class="sxs-lookup"><span data-stu-id="e0fca-226">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="e0fca-227">Získat odkaz na objekt blob voláním **CloudBlobContainer.GetBlockBlobReference** nebo **CloudBlobContainer.GetPageBlobReference** metoda.</span><span class="sxs-lookup"><span data-stu-id="e0fca-227">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="e0fca-228">(Změnit  *&lt;název objektu blob >* k názvu objektu blob, který odstraňujete.)</span><span class="sxs-lookup"><span data-stu-id="e0fca-228">(Change *&lt;blob-name>* to the name of the blob you are deleting.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. <span data-ttu-id="e0fca-229">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="e0fca-229">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="e0fca-230">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="e0fca-230">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="e0fca-231">Spusťte aplikaci a vyberte **odstranit objekt blob** odstranit zadaný v objektu blob **CloudBlobContainer.GetBlockBlobReference** volání metody.</span><span class="sxs-lookup"><span data-stu-id="e0fca-231">Run the application, and select **Delete blob** to delete the blob specified in the **CloudBlobContainer.GetBlockBlobReference** method call.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e0fca-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0fca-232">Next steps</span></span>
<span data-ttu-id="e0fca-233">Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="e0fca-233">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="e0fca-234">Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e0fca-234">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-tables.md)
  * [<span data-ttu-id="e0fca-235">Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e0fca-235">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-queues.md)
