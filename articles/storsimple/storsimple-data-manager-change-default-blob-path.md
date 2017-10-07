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
# <a name="change-a-blob-path-from-hello-default-path-private-preview"></a>Změňte cestu k objektu blob z hello výchozí cestu (soukromém náhledu).

Tento článek popisuje, jak tooset si Azure funkce toorename výchozí cestu souboru blob. 

## <a name="prerequisites"></a>Požadavky

Ujistěte se, že máte definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.

## <a name="create-an-azure-function"></a>Vytvoření Azure funkce

toocreate Azure funkce, hello následující:

1. Přejděte toohello [portál Azure](http://portal.azure.com/).

2. Hello horní části levého podokna hello, klikněte na tlačítko **nový**. 

3. V hello **vyhledávání** zadejte **aplikaci funkce**, a stiskněte klávesu Enter.

    ![Zadejte "Funkce aplikace" hello vyhledávacího pole](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. V hello **výsledky** seznamu, klikněte na tlačítko **aplikaci funkce**.

    ![Vyberte v seznamu výsledků hello prostředek aplikace hello – funkce](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    Hello **aplikaci funkce** otevře se okno.

5. Klikněte na možnost **Vytvořit**.

    ![tlačítko "Vytvořit" okno funkce aplikace Hello](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. Na hello **aplikaci funkce** okno Konfigurace hello následující:

    a. V hello **název aplikace** pole, zadejte název aplikace hello.
    
    b. V hello **předplatné** pole, název typu hello hello předplatného.

    c. V části **skupiny prostředků**, klikněte na tlačítko **vytvořit nový**a potom název typu hello hello skupiny prostředků.

    d. V hello **hostování plánování** zadejte **plánování spotřeba**.

    e. V hello **umístění** pole typu hello umístění.

    f. V části **účet úložiště**vyberte stávající účet úložiště nebo vytvořte nový účet úložiště. Účet úložiště se používá interně pro funkci hello.

    ![Zadejte novou aplikaci funkce konfigurační data](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Klikněte na možnost **Vytvořit**.  
    Vytvoření aplikace Hello funkce.

8. V levém podokně hello, klikněte na **další služby**a pak hello následující:
    
    a. V hello **filtru** zadejte **aplikační služby**.
    
    b. Klikněte na tlačítko **aplikační služby**. 

    ![Odkaz "Další služby" v levém podokně hello](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. Hello seznamu aplikační služby klikněte na název hello hello funkce aplikace.  
    Otevře se stránka aplikace Hello funkce.

10. V levém podokně hello, klikněte na **novou funkci**a pak hello následující: 

    a. V hello **jazyk** seznamu, vyberte **C#**.
    
    b. V poli hello dlaždic šablony, vyberte **QueueTrigger CSharp**.

    c. V hello **název funkce** pole, zadejte název funkce.

    d. V hello **název fronty** zadejte název definice transformaci dat úlohy.

    e. V části **připojení účtu úložiště**, klikněte na tlačítko **nové**a potom vyberte hello účet, který odpovídá toohello transformaci dat úlohy.  
        Poznamenejte si název připojení hello. později v hello Azure funkce je vyžadován název Hello.

       ![Vytvořte novou funkci jazyka C#](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Klikněte na možnost **Vytvořit**.  
    Hello **funkce** otevře se okno.

11. V hello **funkce** okně Spustit _.csx_ souboru a pak hello následující:

    a. Hello vložte následující kód:

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

    b. Nahraďte **STORAGE_CONNECTIONNAME** na řádek 11 s připojením k účtu úložiště (viz. bodu 8 c).

    c. Hello nahoře vlevo, klikněte na **Uložit**.

    ![Uložit – funkce](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. toocomplete hello funkce, přidejte jeden další soubor pomocí tohoto postupu hello následující:

    a. Klikněte na tlačítko **zobrazit soubory**.

       ![odkaz Hello "zobrazení soubory.](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Klikněte na tlačítko **Přidat**.
    
    c. Typ **project.json**, a stiskněte klávesu Enter.
    
    d. V hello **project.json** souboru, vložte následující kód hello:

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

    e. Klikněte na **Uložit**.

Vytvořili jste Azure funkce. Tato funkce se aktivuje vždy, když nový objekt blob je generován hello transformaci dat úlohy.

## <a name="next-steps"></a>Další kroky

[Pomocí uživatelského rozhraní Správce dat StorSimple tootransform dat](storsimple-data-manager-ui.md)
