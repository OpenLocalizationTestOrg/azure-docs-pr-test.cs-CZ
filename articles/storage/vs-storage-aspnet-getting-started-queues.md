---
title: "Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Jak začít používat fronty Azure storage v projektu ASP.NET v sadě Visual Studio po připojení k účtu úložiště pomocí Visual Studio připojené Services"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: tarcher
ms.openlocfilehash: 76b0d5e270e16a317ce8a7b424c06c867b537a8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="f1218-103">Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="f1218-103">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="f1218-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f1218-104">Overview</span></span>

<span data-ttu-id="f1218-105">Azure queue storage poskytuje cloudu zasílání zpráv mezi součástmi aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1218-105">Azure queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="f1218-106">Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="f1218-106">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="f1218-107">Queue Storage zajišťuje asynchronní přenos zpráv pro komunikaci mezi součástmi aplikace bez ohledu na to, jestli běží v cloudu, na desktopu, na místním serveru nebo na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="f1218-107">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in the cloud, on the desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="f1218-108">Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.</span><span class="sxs-lookup"><span data-stu-id="f1218-108">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

<span data-ttu-id="f1218-109">Tento kurz ukazuje, jak napsat kód ASP.NET pro některé běžné scénáře použití fronty Azure storage entity.</span><span class="sxs-lookup"><span data-stu-id="f1218-109">This tutorial shows how to write ASP.NET code for some common scenarios using Azure queue storage entities.</span></span> <span data-ttu-id="f1218-110">Mezi tyto scénáře patří běžné úkoly, jako je například vytváření fronty Azure a přidání, úprava, čtení a odebrání zprávy do fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-110">These scenarios include common tasks such as creating an Azure queue, and adding, modifying, reading, and removing queue messages.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="f1218-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f1218-111">Prerequisites</span></span>

* [<span data-ttu-id="f1218-112">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1218-112">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="f1218-113">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f1218-113">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="f1218-114">Vytvořit řadič MVC</span><span class="sxs-lookup"><span data-stu-id="f1218-114">Create an MVC controller</span></span> 

1. <span data-ttu-id="f1218-115">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce vyberte **Přidat -> řadiče**.</span><span class="sxs-lookup"><span data-stu-id="f1218-115">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Přidat řadič do aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. <span data-ttu-id="f1218-117">Na **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-117">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. <span data-ttu-id="f1218-119">Na **přidat kontroler** dialogové okno, názvu kontroleru *QueuesController*a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-119">On the **Add Controller** dialog, name the controller *QueuesController*, and select **Add**.</span></span>

    ![Název řadiče MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. <span data-ttu-id="f1218-121">Přidejte následující *pomocí* direktivy pro `QueuesController.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="f1218-121">Add the following *using* directives to the `QueuesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a><span data-ttu-id="f1218-122">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="f1218-122">Create a queue</span></span>

<span data-ttu-id="f1218-123">Následující kroky ukazují, jak vytvořit frontu:</span><span class="sxs-lookup"><span data-stu-id="f1218-123">The following steps illustrate how to create a queue:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="f1218-124">V této části předpokládá, že jste dokončili postup [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="f1218-124">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="f1218-125">Otevřete soubor `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1218-125">Open the `QueuesController.cs` file.</span></span> 

1. <span data-ttu-id="f1218-126">Přidejte metodu s názvem **CreateQueue** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f1218-126">Add a method called **CreateQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="f1218-127">V rámci **CreateQueue** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-127">Within the **CreateQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="f1218-128">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="f1218-128">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="f1218-129">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="f1218-129">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. <span data-ttu-id="f1218-130">Získání **CloudQueue** objekt, který reprezentuje odkaz na název požadovaného fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-130">Get a **CloudQueue** object that represents a reference to the desired queue name.</span></span> <span data-ttu-id="f1218-131">**CloudQueueClient.GetQueueReference** metoda neprovede požadavek fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-131">The **CloudQueueClient.GetQueueReference** method does not make a request against queue storage.</span></span> <span data-ttu-id="f1218-132">Odkaz se vrátí, zda fronta existuje.</span><span class="sxs-lookup"><span data-stu-id="f1218-132">The reference is returned whether or not the queue exists.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="f1218-133">Volání **CloudQueue.CreateIfNotExists** metodu pro vytvoření fronty, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f1218-133">Call the **CloudQueue.CreateIfNotExists** method to create the queue if it does not yet exist.</span></span> <span data-ttu-id="f1218-134">**CloudQueue.CreateIfNotExists** metoda vrátí **true** Pokud fronta neexistuje a je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="f1218-134">The **CloudQueue.CreateIfNotExists** method returns **true** if the queue does not exist, and is successfully created.</span></span> <span data-ttu-id="f1218-135">V opačném **false** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="f1218-135">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. <span data-ttu-id="f1218-136">Aktualizace **ViewBag** s názvem fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-136">Update the **ViewBag** with the name of the queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. <span data-ttu-id="f1218-137">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1218-137">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="f1218-138">Na **přidat zobrazení** dialogové okno, zadejte **CreateQueue** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-138">On the **Add View** dialog, enter **CreateQueue** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="f1218-139">Otevřete `CreateQueue.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f1218-139">Open `CreateQueue.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="f1218-140">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f1218-140">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="f1218-141">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="f1218-141">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. <span data-ttu-id="f1218-142">Spusťte aplikaci a vyberte **fronty vytvořit** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f1218-142">Run the application, and select **Create queue** to see results similar to the following screen shot:</span></span>
  
    ![Vytvoření fronty](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    <span data-ttu-id="f1218-144">Jak je uvedeno nahoře, **CloudQueue.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-144">As mentioned previously, the **CloudQueue.CreateIfNotExists** method returns **true** only when the queue doesn't exist and is created.</span></span> <span data-ttu-id="f1218-145">Proto pokud spustíte aplikaci, když fronta existuje, vrátí metoda **false**.</span><span class="sxs-lookup"><span data-stu-id="f1218-145">Therefore, if you run the app when the queue exists, the method returns **false**.</span></span> <span data-ttu-id="f1218-146">Aplikace je třeba spustit vícekrát, je nutné odstranit frontu před spuštěním aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="f1218-146">To run the app multiple times, you must delete the queue before running the app again.</span></span> <span data-ttu-id="f1218-147">Odstranění fronty, můžete to udělat pomocí **CloudQueue.Delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="f1218-147">Deleting the queue can be done via the **CloudQueue.Delete** method.</span></span> <span data-ttu-id="f1218-148">Můžete také odstranit pomocí fronty [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="f1218-148">You can also delete the queue using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="f1218-149">Přidání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="f1218-149">Add a message to a queue</span></span>

<span data-ttu-id="f1218-150">Jakmile jste [vytvořili frontu](#create-a-queue), můžete taky přidat zprávy do této fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-150">Once you've [created a queue](#create-a-queue), you can add messages to that queue.</span></span> <span data-ttu-id="f1218-151">Tato část vás provede procesem přidání zprávu do fronty *zkušební fronty*.</span><span class="sxs-lookup"><span data-stu-id="f1218-151">This section walks you through adding a message to a queue *test-queue*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="f1218-152">V této části předpokládá, že jste dokončili postup [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="f1218-152">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="f1218-153">Otevřete soubor `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1218-153">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="f1218-154">Přidejte metodu s názvem **AddMessage** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f1218-154">Add a method called **AddMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="f1218-155">V rámci **AddMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-155">Within the **AddMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="f1218-156">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="f1218-156">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="f1218-157">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="f1218-157">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="f1218-158">Získání **CloudQueueContainer** objekt, který reprezentuje odkaz na frontu.</span><span class="sxs-lookup"><span data-stu-id="f1218-158">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="f1218-159">Vytvořte **CloudQueueMessage** objektu, který představuje zprávu, kterou chcete přidat do fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-159">Create the **CloudQueueMessage** object representing the message you want to add to the queue.</span></span> <span data-ttu-id="f1218-160">A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="f1218-160">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. <span data-ttu-id="f1218-161">Volání **CloudQueue.AddMessage** metody přidat messaged do fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-161">Call the **CloudQueue.AddMessage** method to add the messaged to the queue.</span></span>

    ```csharp
    queue.AddMessage(message);
    ```

1. <span data-ttu-id="f1218-162">Vytvoření a nastavení několika **ViewBag** vlastnosti pro zobrazení v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f1218-162">Create and set a couple of **ViewBag** properties for display in the view.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. <span data-ttu-id="f1218-163">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1218-163">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="f1218-164">Na **přidat zobrazení** dialogové okno, zadejte **AddMessage** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-164">On the **Add View** dialog, enter **AddMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="f1218-165">Otevřete `AddMessage.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f1218-165">Open `AddMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. <span data-ttu-id="f1218-166">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f1218-166">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="f1218-167">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="f1218-167">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. <span data-ttu-id="f1218-168">Spusťte aplikaci a vyberte **přidat zprávu** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f1218-168">Run the application, and select **Add message** to see results similar to the following screen shot:</span></span>
  
    ![Přidat zprávu](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

<span data-ttu-id="f1218-170">Dva oddíly - [přečte zprávu z fronty bez odebráním](#read-a-message-from-a-queue-without-removing-it) a [pro čtení a odebere zprávu z fronty](#read-and-remove-a-message-from-a-queue) -ukazují, jak číst zprávy z fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-170">The two sections - [Read a message from a queue without removing it](#read-a-message-from-a-queue-without-removing-it) and [Read and remove a message from a queue](#read-and-remove-a-message-from-a-queue) - illustrate how to read messages from a queue.</span></span>    

## <a name="read-a-message-from-a-queue-without-removing-it"></a><span data-ttu-id="f1218-171">Přečte zprávu z fronty bez odebráním</span><span class="sxs-lookup"><span data-stu-id="f1218-171">Read a message from a queue without removing it</span></span>

<span data-ttu-id="f1218-172">Tato část ukazuje postup prohlížet zprávu ve frontě (první zprávu přečíst bez odebráním).</span><span class="sxs-lookup"><span data-stu-id="f1218-172">This section illustrates how to peek at a queued message (read the first message without removing it).</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="f1218-173">V této části předpokládá, že jste dokončili postup [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="f1218-173">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="f1218-174">Otevřete soubor `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1218-174">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="f1218-175">Přidejte metodu s názvem **PeekMessage** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f1218-175">Add a method called **PeekMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="f1218-176">V rámci **PeekMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-176">Within the **PeekMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="f1218-177">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="f1218-177">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="f1218-178">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="f1218-178">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="f1218-179">Získání **CloudQueueContainer** objekt, který reprezentuje odkaz na frontu.</span><span class="sxs-lookup"><span data-stu-id="f1218-179">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="f1218-180">Volání **CloudQueue.PeekMessage** metoda číst první zprávu ve frontě bez odebere ji z fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-180">Call the **CloudQueue.PeekMessage** method to read the first message in the queue without removing it from the queue.</span></span> 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. <span data-ttu-id="f1218-181">Aktualizace **ViewBag** se dvěma hodnotami: název fronty a zprávy, která byla načtena.</span><span class="sxs-lookup"><span data-stu-id="f1218-181">Update the **ViewBag** with two values: the queue name and the message that was read.</span></span> <span data-ttu-id="f1218-182">**CloudQueueMessage** objekt poskytuje dvě vlastnosti pro získání hodnota objektu: **CloudQueueMessage.AsBytes** a **CloudQueueMessage.AsString**.</span><span class="sxs-lookup"><span data-stu-id="f1218-182">The **CloudQueueMessage** object exposes two properties for getting the object's value: **CloudQueueMessage.AsBytes** and **CloudQueueMessage.AsString**.</span></span> <span data-ttu-id="f1218-183">**AsString** (používá se v tomto příkladu) vrátí řetězec, zatímco **AsBytes** vrátí pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="f1218-183">**AsString** (used in this example) returns a string, while **AsBytes** returns a byte array.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. <span data-ttu-id="f1218-184">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1218-184">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="f1218-185">Na **přidat zobrazení** dialogové okno, zadejte **PeekMessage** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-185">On the **Add View** dialog, enter **PeekMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="f1218-186">Otevřete `PeekMessage.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f1218-186">Open `PeekMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "PeekMessage";
    }
    
    <h2>Peek Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Peeked Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>    
    ```

1. <span data-ttu-id="f1218-187">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f1218-187">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="f1218-188">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="f1218-188">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. <span data-ttu-id="f1218-189">Spusťte aplikaci a vyberte **funkce Náhled zpráva** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f1218-189">Run the application, and select **Peek message** to see results similar to the following screen shot:</span></span>
  
    ![Prohlížení zpráv](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a><span data-ttu-id="f1218-191">Přečtěte si a odebrání zprávy z fronty</span><span class="sxs-lookup"><span data-stu-id="f1218-191">Read and remove a message from a queue</span></span>

<span data-ttu-id="f1218-192">V této části se dozvíte, jak ke čtení a odebrání zprávy z fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-192">In this section, you learn how to read and remove a message from a queue.</span></span>   

> [!NOTE]
> 
> <span data-ttu-id="f1218-193">V této části předpokládá, že jste dokončili postup [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="f1218-193">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="f1218-194">Otevřete soubor `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1218-194">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="f1218-195">Přidejte metodu s názvem **ReadMessage** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f1218-195">Add a method called **ReadMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="f1218-196">V rámci **ReadMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-196">Within the **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="f1218-197">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="f1218-197">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="f1218-198">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="f1218-198">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="f1218-199">Získání **CloudQueueContainer** objekt, který reprezentuje odkaz na frontu.</span><span class="sxs-lookup"><span data-stu-id="f1218-199">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="f1218-200">Volání **CloudQueue.GetMessage** metoda první zprávu ve frontě číst.</span><span class="sxs-lookup"><span data-stu-id="f1218-200">Call the **CloudQueue.GetMessage** method to read the first message in the queue.</span></span> <span data-ttu-id="f1218-201">**CloudQueue.GetMessage** metoda usnadňuje zpráva neviditelná pro jakýkoli jiný kód čtení zpráv tak, aby žádný jiný kód, můžete upravit nebo odstranit zpráva při vaší zpracování, je 30 sekund (ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="f1218-201">The **CloudQueue.GetMessage** method makes the message invisible for 30 seconds (by default) to any other code reading messages so that no other code can modify or delete the message while your processing it.</span></span> <span data-ttu-id="f1218-202">Chcete-li změnit dobu zpráva je neviditelná, změňte **visibilityTimeout** parametr předána **CloudQueue.GetMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="f1218-202">To change the amount of time the message is invisible, modify the **visibilityTimeout** parameter being passed to the **CloudQueue.GetMessage** method.</span></span>

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. <span data-ttu-id="f1218-203">Volání **CloudQueueMessage.Delete** metoda odstranění zprávy z fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-203">Call the **CloudQueueMessage.Delete** method to delete the message from the queue.</span></span>

    ```csharp
    queue.DeleteMessage(message);
    ```

1. <span data-ttu-id="f1218-204">Aktualizace **ViewBag** zprávu odstranit a s názvem fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-204">Update the **ViewBag** with the message deleted, and the name of the queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. <span data-ttu-id="f1218-205">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1218-205">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="f1218-206">Na **přidat zobrazení** dialogové okno, zadejte **ReadMessage** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-206">On the **Add View** dialog, enter **ReadMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="f1218-207">Otevřete `ReadMessage.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f1218-207">Open `ReadMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "ReadMessage";
    }
    
    <h2>Read Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Read (and Deleted) Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>
    ```

1. <span data-ttu-id="f1218-208">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f1218-208">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="f1218-209">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="f1218-209">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. <span data-ttu-id="f1218-210">Spusťte aplikaci a vyberte **pro čtení nebo odstranění zprávy** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f1218-210">Run the application, and select **Read/Delete message** to see results similar to the following screen shot:</span></span>
  
    ![Čtení a odstraňování zpráv](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a><span data-ttu-id="f1218-212">Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="f1218-212">Get the queue length</span></span>

<span data-ttu-id="f1218-213">Tato část ukazuje postup získání délky fronty (počet zpráv).</span><span class="sxs-lookup"><span data-stu-id="f1218-213">This section illustrates how to get the queue length (number of messages).</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="f1218-214">V této části předpokládá, že jste dokončili postup [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="f1218-214">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="f1218-215">Otevřete soubor `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1218-215">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="f1218-216">Přidejte metodu s názvem **GetQueueLength** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f1218-216">Add a method called **GetQueueLength** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="f1218-217">V rámci **ReadMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-217">Within the **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="f1218-218">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="f1218-218">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="f1218-219">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="f1218-219">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="f1218-220">Získání **CloudQueueContainer** objekt, který reprezentuje odkaz na frontu.</span><span class="sxs-lookup"><span data-stu-id="f1218-220">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="f1218-221">Volání **CloudQueue.FetchAttributes** metoda načtení atributů fronty (včetně jeho délky).</span><span class="sxs-lookup"><span data-stu-id="f1218-221">Call the **CloudQueue.FetchAttributes** method to retrieve the queue's attributes (including its length).</span></span> 

    ```csharp
    queue.FetchAttributes();
    ```

6. <span data-ttu-id="f1218-222">Přístup **CloudQueue.ApproximateMessageCount** vlastnost k získání délky fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-222">Access the **CloudQueue.ApproximateMessageCount** property to get the queue's length.</span></span>
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. <span data-ttu-id="f1218-223">Aktualizace **ViewBag** s názvem fronty a jeho délka.</span><span class="sxs-lookup"><span data-stu-id="f1218-223">Update the **ViewBag** with the name of the queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. <span data-ttu-id="f1218-224">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1218-224">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="f1218-225">Na **přidat zobrazení** dialogové okno, zadejte **GetQueueLength** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-225">On the **Add View** dialog, enter **GetQueueLength** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="f1218-226">Otevřete `GetQueueLengthMessage.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f1218-226">Open `GetQueueLengthMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. <span data-ttu-id="f1218-227">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f1218-227">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="f1218-228">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="f1218-228">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. <span data-ttu-id="f1218-229">Spusťte aplikaci a vyberte **získat délka fronty** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f1218-229">Run the application, and select **Get queue length** to see results similar to the following screen shot:</span></span>
  
    ![Získání délky fronty](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a><span data-ttu-id="f1218-231">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="f1218-231">Delete a queue</span></span>
<span data-ttu-id="f1218-232">Tato část ukazuje postup odstranění fronty.</span><span class="sxs-lookup"><span data-stu-id="f1218-232">This section illustrates how to delete a queue.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="f1218-233">V této části předpokládá, že jste dokončili postup [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="f1218-233">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="f1218-234">Otevřete soubor `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1218-234">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="f1218-235">Přidejte metodu s názvem **DeleteQueue** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f1218-235">Add a method called **DeleteQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="f1218-236">V rámci **DeleteQueue** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1218-236">Within the **DeleteQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="f1218-237">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="f1218-237">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="f1218-238">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="f1218-238">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="f1218-239">Získání **CloudQueueContainer** objekt, který reprezentuje odkaz na frontu.</span><span class="sxs-lookup"><span data-stu-id="f1218-239">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="f1218-240">Volání **CloudQueue.Delete** metoda odstranění fronty reprezentována **CloudQueue** objektu.</span><span class="sxs-lookup"><span data-stu-id="f1218-240">Call the **CloudQueue.Delete** method to delete the queue represented by the **CloudQueue** object.</span></span>

    ```csharp
    queue.Delete();
    ```

1. <span data-ttu-id="f1218-241">Aktualizace **ViewBag** s názvem fronty a jeho délka.</span><span class="sxs-lookup"><span data-stu-id="f1218-241">Update the **ViewBag** with the name of the queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. <span data-ttu-id="f1218-242">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1218-242">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="f1218-243">Na **přidat zobrazení** dialogové okno, zadejte **DeleteQueue** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1218-243">On the **Add View** dialog, enter **DeleteQueue** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="f1218-244">Otevřete `DeleteQueue.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f1218-244">Open `DeleteQueue.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. <span data-ttu-id="f1218-245">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f1218-245">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="f1218-246">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="f1218-246">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. <span data-ttu-id="f1218-247">Spusťte aplikaci a vyberte **získat délka fronty** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f1218-247">Run the application, and select **Get queue length** to see results similar to the following screen shot:</span></span>
  
    ![Odstranění fronty](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a><span data-ttu-id="f1218-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1218-249">Next steps</span></span>
<span data-ttu-id="f1218-250">Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="f1218-250">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="f1218-251">Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="f1218-251">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="f1218-252">Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="f1218-252">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-tables.md)
