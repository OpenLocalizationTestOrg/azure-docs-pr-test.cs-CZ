---
title: "Cesta blobu aaaChange z výchozí hello | Microsoft Docs"
description: "Zjistěte, jak tooset si Azure funkce toorename cestu k objektu blob souboru (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 2c414603514223c701ab1a3bd0b81ee18f1af666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-a-blob-path-from-hello-default-path-private-preview"></a><span data-ttu-id="38aaf-103">Změňte cestu k objektu blob z hello výchozí cestu (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="38aaf-103">Change a blob path from hello default path (private preview)</span></span>

<span data-ttu-id="38aaf-104">Tento článek popisuje, jak tooset si Azure funkce toorename výchozí cestu souboru blob.</span><span class="sxs-lookup"><span data-stu-id="38aaf-104">This article describes how tooset up an Azure function toorename a default blob file path.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="38aaf-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="38aaf-105">Prerequisites</span></span>

<span data-ttu-id="38aaf-106">Ujistěte se, že máte definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="38aaf-106">Ensure that you have a job definition that has been correctly configured in a hybrid data resource within a resource group.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="38aaf-107">Vytvoření Azure funkce</span><span class="sxs-lookup"><span data-stu-id="38aaf-107">Create an Azure function</span></span>

<span data-ttu-id="38aaf-108">toocreate Azure funkce, hello následující:</span><span class="sxs-lookup"><span data-stu-id="38aaf-108">toocreate an Azure function, do hello following:</span></span>

1. <span data-ttu-id="38aaf-109">Přejděte toohello [portál Azure](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="38aaf-109">Go toohello [Azure portal](http://portal.azure.com/).</span></span>

2. <span data-ttu-id="38aaf-110">Hello horní části levého podokna hello, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-110">At hello top of hello left pane, click **New**.</span></span> 

3. <span data-ttu-id="38aaf-111">V hello **vyhledávání** zadejte **aplikaci funkce**, a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="38aaf-111">In hello **Search** box, type **Function App**, and then press Enter.</span></span>

    ![Zadejte "Funkce aplikace" hello vyhledávacího pole](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. <span data-ttu-id="38aaf-113">V hello **výsledky** seznamu, klikněte na tlačítko **aplikaci funkce**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-113">In hello **Results** list, click **Function App**.</span></span>

    ![Vyberte v seznamu výsledků hello prostředek aplikace hello – funkce](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    <span data-ttu-id="38aaf-115">Hello **aplikaci funkce** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="38aaf-115">hello **Function App** window opens.</span></span>

5. <span data-ttu-id="38aaf-116">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-116">Click **Create**.</span></span>

    ![tlačítko "Vytvořit" okno funkce aplikace Hello](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. <span data-ttu-id="38aaf-118">Na hello **aplikaci funkce** okno Konfigurace hello následující:</span><span class="sxs-lookup"><span data-stu-id="38aaf-118">On hello **Function App** configuration blade, do hello following:</span></span>

    <span data-ttu-id="38aaf-119">a.</span><span class="sxs-lookup"><span data-stu-id="38aaf-119">a.</span></span> <span data-ttu-id="38aaf-120">V hello **název aplikace** pole, zadejte název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="38aaf-120">In hello **App name** box, type hello app name.</span></span>
    
    <span data-ttu-id="38aaf-121">b.</span><span class="sxs-lookup"><span data-stu-id="38aaf-121">b.</span></span> <span data-ttu-id="38aaf-122">V hello **předplatné** pole, název typu hello hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="38aaf-122">In hello **Subscription** box, type hello name of hello subscription.</span></span>

    <span data-ttu-id="38aaf-123">c.</span><span class="sxs-lookup"><span data-stu-id="38aaf-123">c.</span></span> <span data-ttu-id="38aaf-124">V části **skupiny prostředků**, klikněte na tlačítko **vytvořit nový**a potom název typu hello hello skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="38aaf-124">Under **Resource Group**, click **Create new**, and then type hello name of hello resource group.</span></span>

    <span data-ttu-id="38aaf-125">d.</span><span class="sxs-lookup"><span data-stu-id="38aaf-125">d.</span></span> <span data-ttu-id="38aaf-126">V hello **hostování plánování** zadejte **plánování spotřeba**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-126">In hello **Hosting Plan** box, type **Consumption Plan**.</span></span>

    <span data-ttu-id="38aaf-127">e.</span><span class="sxs-lookup"><span data-stu-id="38aaf-127">e.</span></span> <span data-ttu-id="38aaf-128">V hello **umístění** pole typu hello umístění.</span><span class="sxs-lookup"><span data-stu-id="38aaf-128">In hello **Location** box, type hello location.</span></span>

    <span data-ttu-id="38aaf-129">f.</span><span class="sxs-lookup"><span data-stu-id="38aaf-129">f.</span></span> <span data-ttu-id="38aaf-130">V části **účet úložiště**vyberte stávající účet úložiště nebo vytvořte nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="38aaf-130">Under **Storage account**, select an existing storage account or create a new storage account.</span></span> <span data-ttu-id="38aaf-131">Účet úložiště se používá interně pro funkci hello.</span><span class="sxs-lookup"><span data-stu-id="38aaf-131">A storage account is used internally for hello function.</span></span>

    ![Zadejte novou aplikaci funkce konfigurační data](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. <span data-ttu-id="38aaf-133">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-133">Click **Create**.</span></span>  
    <span data-ttu-id="38aaf-134">Vytvoření aplikace Hello funkce.</span><span class="sxs-lookup"><span data-stu-id="38aaf-134">hello function app is created.</span></span>

8. <span data-ttu-id="38aaf-135">V levém podokně hello, klikněte na **další služby**a pak hello následující:</span><span class="sxs-lookup"><span data-stu-id="38aaf-135">In hello left pane, click **More services**, and then do hello following:</span></span>
    
    <span data-ttu-id="38aaf-136">a.</span><span class="sxs-lookup"><span data-stu-id="38aaf-136">a.</span></span> <span data-ttu-id="38aaf-137">V hello **filtru** zadejte **aplikační služby**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-137">In hello **Filter** box, type **App services**.</span></span>
    
    <span data-ttu-id="38aaf-138">b.</span><span class="sxs-lookup"><span data-stu-id="38aaf-138">b.</span></span> <span data-ttu-id="38aaf-139">Klikněte na tlačítko **aplikační služby**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-139">Click **App Services**.</span></span> 

    ![Odkaz "Další služby" v levém podokně hello](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. <span data-ttu-id="38aaf-141">Hello seznamu aplikační služby klikněte na název hello hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="38aaf-141">In hello list of app services, click hello name of hello function app.</span></span>  
    <span data-ttu-id="38aaf-142">Otevře se stránka aplikace Hello funkce.</span><span class="sxs-lookup"><span data-stu-id="38aaf-142">hello function app page opens.</span></span>

10. <span data-ttu-id="38aaf-143">V levém podokně hello, klikněte na **novou funkci**a pak hello následující:</span><span class="sxs-lookup"><span data-stu-id="38aaf-143">In hello left pane, click **New Function**, and then do hello following:</span></span> 

    <span data-ttu-id="38aaf-144">a.</span><span class="sxs-lookup"><span data-stu-id="38aaf-144">a.</span></span> <span data-ttu-id="38aaf-145">V hello **jazyk** seznamu, vyberte **C#**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-145">In hello **Language** list, select **C#**.</span></span>
    
    <span data-ttu-id="38aaf-146">b.</span><span class="sxs-lookup"><span data-stu-id="38aaf-146">b.</span></span> <span data-ttu-id="38aaf-147">V poli hello dlaždic šablony, vyberte **QueueTrigger CSharp**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-147">In hello array of template tiles, select **QueueTrigger-CSharp**.</span></span>

    <span data-ttu-id="38aaf-148">c.</span><span class="sxs-lookup"><span data-stu-id="38aaf-148">c.</span></span> <span data-ttu-id="38aaf-149">V hello **název funkce** pole, zadejte název funkce.</span><span class="sxs-lookup"><span data-stu-id="38aaf-149">In hello **Name your function** box, type a name for your function.</span></span>

    <span data-ttu-id="38aaf-150">d.</span><span class="sxs-lookup"><span data-stu-id="38aaf-150">d.</span></span> <span data-ttu-id="38aaf-151">V hello **název fronty** zadejte název definice transformaci dat úlohy.</span><span class="sxs-lookup"><span data-stu-id="38aaf-151">In hello **Queue name** box, type your data-transformation job definition name.</span></span>

    <span data-ttu-id="38aaf-152">e.</span><span class="sxs-lookup"><span data-stu-id="38aaf-152">e.</span></span> <span data-ttu-id="38aaf-153">V části **připojení účtu úložiště**, klikněte na tlačítko **nové**a potom vyberte hello účet, který odpovídá toohello transformaci dat úlohy.</span><span class="sxs-lookup"><span data-stu-id="38aaf-153">Under **Storage account connection**, click **new**, and then select hello account that corresponds toohello data-transformation job.</span></span>  
        <span data-ttu-id="38aaf-154">Poznamenejte si název připojení hello.</span><span class="sxs-lookup"><span data-stu-id="38aaf-154">Make a note of hello connection name.</span></span> <span data-ttu-id="38aaf-155">později v hello Azure funkce je vyžadován název Hello.</span><span class="sxs-lookup"><span data-stu-id="38aaf-155">hello name is required later in hello Azure function.</span></span>

       ![Vytvořte novou funkci jazyka C#](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    <span data-ttu-id="38aaf-157">f.</span><span class="sxs-lookup"><span data-stu-id="38aaf-157">f.</span></span> <span data-ttu-id="38aaf-158">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-158">Click **Create**.</span></span>  
    <span data-ttu-id="38aaf-159">Hello **funkce** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="38aaf-159">hello **Function** window opens.</span></span>

11. <span data-ttu-id="38aaf-160">V hello **funkce** okně Spustit _.csx_ souboru a pak hello následující:</span><span class="sxs-lookup"><span data-stu-id="38aaf-160">In hello **Function** window, run _.csx_ file, and then do hello following:</span></span>

    <span data-ttu-id="38aaf-161">a.</span><span class="sxs-lookup"><span data-stu-id="38aaf-161">a.</span></span> <span data-ttu-id="38aaf-162">Hello vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="38aaf-162">Paste hello following code:</span></span>

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create hello blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip toocopy hello blob toonew container, if source blob doesn't exist
            log.Info($"hello specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy toonew container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    <span data-ttu-id="38aaf-163">b.</span><span class="sxs-lookup"><span data-stu-id="38aaf-163">b.</span></span> <span data-ttu-id="38aaf-164">Nahraďte **STORAGE_CONNECTIONNAME** na řádek 11 s připojením k účtu úložiště (viz. bodu 8 c).</span><span class="sxs-lookup"><span data-stu-id="38aaf-164">Replace **STORAGE_CONNECTIONNAME** on line 11 with your storage account connection (refer point 8c).</span></span>

    <span data-ttu-id="38aaf-165">c.</span><span class="sxs-lookup"><span data-stu-id="38aaf-165">c.</span></span> <span data-ttu-id="38aaf-166">Hello nahoře vlevo, klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-166">At hello top left, click **Save**.</span></span>

    ![Uložit – funkce](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. <span data-ttu-id="38aaf-168">toocomplete hello funkce, přidejte jeden další soubor pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="38aaf-168">toocomplete hello function, add one more file by doing hello following:</span></span>

    <span data-ttu-id="38aaf-169">a.</span><span class="sxs-lookup"><span data-stu-id="38aaf-169">a.</span></span> <span data-ttu-id="38aaf-170">Klikněte na tlačítko **zobrazit soubory**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-170">Click **View files**.</span></span>

       ![odkaz Hello "zobrazení soubory.](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    <span data-ttu-id="38aaf-172">b.</span><span class="sxs-lookup"><span data-stu-id="38aaf-172">b.</span></span> <span data-ttu-id="38aaf-173">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-173">Click **Add**.</span></span>
    
    <span data-ttu-id="38aaf-174">c.</span><span class="sxs-lookup"><span data-stu-id="38aaf-174">c.</span></span> <span data-ttu-id="38aaf-175">Typ **project.json**, a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="38aaf-175">Type **project.json**, and then press Enter.</span></span>
    
    <span data-ttu-id="38aaf-176">d.</span><span class="sxs-lookup"><span data-stu-id="38aaf-176">d.</span></span> <span data-ttu-id="38aaf-177">V hello **project.json** souboru, vložte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="38aaf-177">In hello **project.json** file, paste hello following code:</span></span>

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    <span data-ttu-id="38aaf-178">e.</span><span class="sxs-lookup"><span data-stu-id="38aaf-178">e.</span></span> <span data-ttu-id="38aaf-179">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="38aaf-179">Click **Save**.</span></span>

<span data-ttu-id="38aaf-180">Vytvořili jste Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="38aaf-180">You have created an Azure function.</span></span> <span data-ttu-id="38aaf-181">Tato funkce se aktivuje vždy, když nový objekt blob je generován hello transformaci dat úlohy.</span><span class="sxs-lookup"><span data-stu-id="38aaf-181">This function is triggered each time a new blob is generated by hello data-transformation job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38aaf-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38aaf-182">Next steps</span></span>

[<span data-ttu-id="38aaf-183">Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat</span><span class="sxs-lookup"><span data-stu-id="38aaf-183">Use StorSimple Data Manager UI tootransform your data</span></span>](storsimple-data-manager-ui.md)
