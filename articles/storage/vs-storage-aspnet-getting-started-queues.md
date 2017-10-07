---
title: "aaaGet začít s Azure queue storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí fronty Azure storage po připojení účtu úložiště tooa pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
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
ms.openlocfilehash: a9d6ecb1e8d61d75f59658d0ea3fa63d26fd7354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="c7c0b-103">Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-103">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="c7c0b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c7c0b-104">Overview</span></span>

<span data-ttu-id="c7c0b-105">Azure queue storage poskytuje cloudu zasílání zpráv mezi součástmi aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-105">Azure queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="c7c0b-106">Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-106">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="c7c0b-107">Fronty úložiště poskytuje asynchronní zasílání zpráv pro komunikaci mezi součástmi aplikací, zda jsou spuštěny v hello cloudu, na ploše hello, na místním serveru nebo na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-107">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in hello cloud, on hello desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="c7c0b-108">Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-108">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

<span data-ttu-id="c7c0b-109">Tento kurz ukazuje, jak kód toowrite ASP.NET pro některé běžné scénáře použití fronty Azure storage entity.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-109">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure queue storage entities.</span></span> <span data-ttu-id="c7c0b-110">Mezi tyto scénáře patří běžné úkoly, jako je například vytváření fronty Azure a přidání, úprava, čtení a odebrání zprávy do fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-110">These scenarios include common tasks such as creating an Azure queue, and adding, modifying, reading, and removing queue messages.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="c7c0b-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c7c0b-111">Prerequisites</span></span>

* [<span data-ttu-id="c7c0b-112">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7c0b-112">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="c7c0b-113">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c7c0b-113">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="c7c0b-114">Vytvořit řadič MVC</span><span class="sxs-lookup"><span data-stu-id="c7c0b-114">Create an MVC controller</span></span> 

1. <span data-ttu-id="c7c0b-115">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce hello, vyberte **Přidat -> řadiče**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-115">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![Přidat řadič tooan aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. <span data-ttu-id="c7c0b-117">Na hello **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-117">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. <span data-ttu-id="c7c0b-119">Na hello **přidat kontroler** dialogové okno, název hello řadič *QueuesController*a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-119">On hello **Add Controller** dialog, name hello controller *QueuesController*, and select **Add**.</span></span>

    ![Název hello MVC jsou řadič MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. <span data-ttu-id="c7c0b-121">Přidejte následující hello *pomocí* toohello direktivy `QueuesController.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-121">Add hello following *using* directives toohello `QueuesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a><span data-ttu-id="c7c0b-122">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="c7c0b-122">Create a queue</span></span>

<span data-ttu-id="c7c0b-123">Hello následující kroky popisují jak toocreate fronty:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-123">hello following steps illustrate how toocreate a queue:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="c7c0b-124">V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-124">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="c7c0b-125">Otevřete hello `QueuesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-125">Open hello `QueuesController.cs` file.</span></span> 

1. <span data-ttu-id="c7c0b-126">Přidejte metodu s názvem **CreateQueue** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-126">Add a method called **CreateQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="c7c0b-127">V rámci hello **CreateQueue** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-127">Within hello **CreateQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="c7c0b-128">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-128">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="c7c0b-129">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-129">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. <span data-ttu-id="c7c0b-130">Získání **CloudQueue** objekt, který reprezentuje název požadované fronty toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-130">Get a **CloudQueue** object that represents a reference toohello desired queue name.</span></span> <span data-ttu-id="c7c0b-131">Hello **CloudQueueClient.GetQueueReference** metoda neprovede požadavek fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-131">hello **CloudQueueClient.GetQueueReference** method does not make a request against queue storage.</span></span> <span data-ttu-id="c7c0b-132">odkaz Hello se vrátí, zda text hello fronta existuje.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-132">hello reference is returned whether or not hello queue exists.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="c7c0b-133">Volání hello **CloudQueue.CreateIfNotExists** metoda toocreate hello fronty, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-133">Call hello **CloudQueue.CreateIfNotExists** method toocreate hello queue if it does not yet exist.</span></span> <span data-ttu-id="c7c0b-134">Hello **CloudQueue.CreateIfNotExists** metoda vrátí **true** Pokud hello fronta neexistuje a je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-134">hello **CloudQueue.CreateIfNotExists** method returns **true** if hello queue does not exist, and is successfully created.</span></span> <span data-ttu-id="c7c0b-135">V opačném **false** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-135">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. <span data-ttu-id="c7c0b-136">Aktualizace hello **ViewBag** s názvem hello hello fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-136">Update hello **ViewBag** with hello name of hello queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. <span data-ttu-id="c7c0b-137">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-137">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="c7c0b-138">Na hello **přidat zobrazení** dialogové okno, zadejte **CreateQueue** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-138">On hello **Add View** dialog, enter **CreateQueue** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="c7c0b-139">Otevřete `CreateQueue.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-139">Open `CreateQueue.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="c7c0b-140">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-140">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="c7c0b-141">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-141">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. <span data-ttu-id="c7c0b-142">Spuštění aplikace hello a vyberte **fronty vytvořit** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-142">Run hello application, and select **Create queue** toosee results similar toohello following screen shot:</span></span>
  
    ![Vytvoření fronty](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    <span data-ttu-id="c7c0b-144">Jak je uvedeno nahoře, hello **CloudQueue.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-144">As mentioned previously, hello **CloudQueue.CreateIfNotExists** method returns **true** only when hello queue doesn't exist and is created.</span></span> <span data-ttu-id="c7c0b-145">Proto pokud hello aplikaci spustíte, když hello fronta existuje, hello metoda vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-145">Therefore, if you run hello app when hello queue exists, hello method returns **false**.</span></span> <span data-ttu-id="c7c0b-146">aplikace hello toorun vícekrát, je nutné odstranit hello fronty před spuštěním aplikace hello znovu.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-146">toorun hello app multiple times, you must delete hello queue before running hello app again.</span></span> <span data-ttu-id="c7c0b-147">Odstraňování hello fronty, můžete to udělat pomocí hello **CloudQueue.Delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-147">Deleting hello queue can be done via hello **CloudQueue.Delete** method.</span></span> <span data-ttu-id="c7c0b-148">Můžete také odstranit hello fronty pomocí hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-148">You can also delete hello queue using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="c7c0b-149">Přidat tooa fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="c7c0b-149">Add a message tooa queue</span></span>

<span data-ttu-id="c7c0b-150">Jakmile jste [vytvořili frontu](#create-a-queue), můžete přidat toothat fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-150">Once you've [created a queue](#create-a-queue), you can add messages toothat queue.</span></span> <span data-ttu-id="c7c0b-151">Tato část vás provede procesem přidání fronty zpráv tooa *zkušební fronty*.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-151">This section walks you through adding a message tooa queue *test-queue*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="c7c0b-152">V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-152">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="c7c0b-153">Otevřete hello `QueuesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-153">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="c7c0b-154">Přidejte metodu s názvem **AddMessage** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-154">Add a method called **AddMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="c7c0b-155">V rámci hello **AddMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-155">Within hello **AddMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="c7c0b-156">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-156">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="c7c0b-157">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-157">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="c7c0b-158">Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-158">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="c7c0b-159">Vytvoření hello **CloudQueueMessage** objekt reprezentující uvítací zprávu chcete tooadd toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-159">Create hello **CloudQueueMessage** object representing hello message you want tooadd toohello queue.</span></span> <span data-ttu-id="c7c0b-160">A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-160">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. <span data-ttu-id="c7c0b-161">Volání hello **CloudQueue.AddMessage** metoda tooadd hello messaged toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-161">Call hello **CloudQueue.AddMessage** method tooadd hello messaged toohello queue.</span></span>

    ```csharp
    queue.AddMessage(message);
    ```

1. <span data-ttu-id="c7c0b-162">Vytvoření a nastavení několika **ViewBag** vlastnosti pro zobrazení v zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-162">Create and set a couple of **ViewBag** properties for display in hello view.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. <span data-ttu-id="c7c0b-163">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-163">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="c7c0b-164">Na hello **přidat zobrazení** dialogové okno, zadejte **AddMessage** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-164">On hello **Add View** dialog, enter **AddMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="c7c0b-165">Otevřete `AddMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-165">Open `AddMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    hello message '@ViewBag.Message' was added toohello queue '@ViewBag.QueueName'.
    ```

1. <span data-ttu-id="c7c0b-166">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-166">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="c7c0b-167">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-167">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. <span data-ttu-id="c7c0b-168">Spuštění aplikace hello a vyberte **přidat zprávu** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-168">Run hello application, and select **Add message** toosee results similar toohello following screen shot:</span></span>
  
    ![Přidat zprávu](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

<span data-ttu-id="c7c0b-170">Hello dva oddíly - [přečte zprávu z fronty bez odebráním](#read-a-message-from-a-queue-without-removing-it) a [pro čtení a odebere zprávu z fronty](#read-and-remove-a-message-from-a-queue) -ilustrují, jak tooread zpráv z fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-170">hello two sections - [Read a message from a queue without removing it](#read-a-message-from-a-queue-without-removing-it) and [Read and remove a message from a queue](#read-and-remove-a-message-from-a-queue) - illustrate how tooread messages from a queue.</span></span>  

## <a name="read-a-message-from-a-queue-without-removing-it"></a><span data-ttu-id="c7c0b-171">Přečte zprávu z fronty bez odebráním</span><span class="sxs-lookup"><span data-stu-id="c7c0b-171">Read a message from a queue without removing it</span></span>

<span data-ttu-id="c7c0b-172">Tato část ukazuje způsob toopeek na zprávu ve frontě (první zprávy pro čtení hello bez odebráním).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-172">This section illustrates how toopeek at a queued message (read hello first message without removing it).</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="c7c0b-173">V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-173">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="c7c0b-174">Otevřete hello `QueuesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-174">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="c7c0b-175">Přidejte metodu s názvem **PeekMessage** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-175">Add a method called **PeekMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult PeekMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="c7c0b-176">V rámci hello **PeekMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-176">Within hello **PeekMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="c7c0b-177">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-177">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="c7c0b-178">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-178">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="c7c0b-179">Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-179">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="c7c0b-180">Volání hello **CloudQueue.PeekMessage** metoda tooread hello první zprávu ve frontě hello bez odebere ji z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-180">Call hello **CloudQueue.PeekMessage** method tooread hello first message in hello queue without removing it from hello queue.</span></span> 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. <span data-ttu-id="c7c0b-181">Aktualizace hello **ViewBag** se dvěma hodnotami: název fronty hello a uvítací zprávu, která byla načtena.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-181">Update hello **ViewBag** with two values: hello queue name and hello message that was read.</span></span> <span data-ttu-id="c7c0b-182">Hello **CloudQueueMessage** objekt poskytuje dvě vlastnosti pro získání hodnoty objektu hello: **CloudQueueMessage.AsBytes** a **CloudQueueMessage.AsString**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-182">hello **CloudQueueMessage** object exposes two properties for getting hello object's value: **CloudQueueMessage.AsBytes** and **CloudQueueMessage.AsString**.</span></span> <span data-ttu-id="c7c0b-183">**AsString** (používá se v tomto příkladu) vrátí řetězec, zatímco **AsBytes** vrátí pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-183">**AsString** (used in this example) returns a string, while **AsBytes** returns a byte array.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. <span data-ttu-id="c7c0b-184">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-184">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="c7c0b-185">Na hello **přidat zobrazení** dialogové okno, zadejte **PeekMessage** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-185">On hello **Add View** dialog, enter **PeekMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="c7c0b-186">Otevřete `PeekMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-186">Open `PeekMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="c7c0b-187">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-187">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="c7c0b-188">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-188">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. <span data-ttu-id="c7c0b-189">Spuštění aplikace hello a vyberte **funkce Náhled zpráva** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-189">Run hello application, and select **Peek message** toosee results similar toohello following screen shot:</span></span>
  
    ![Prohlížení zpráv](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a><span data-ttu-id="c7c0b-191">Přečtěte si a odebrání zprávy z fronty</span><span class="sxs-lookup"><span data-stu-id="c7c0b-191">Read and remove a message from a queue</span></span>

<span data-ttu-id="c7c0b-192">V této části se dozvíte, jak tooread a odebrání zprávy z fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-192">In this section, you learn how tooread and remove a message from a queue.</span></span>   

> [!NOTE]
> 
> <span data-ttu-id="c7c0b-193">V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-193">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="c7c0b-194">Otevřete hello `QueuesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-194">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="c7c0b-195">Přidejte metodu s názvem **ReadMessage** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-195">Add a method called **ReadMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ReadMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="c7c0b-196">V rámci hello **ReadMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-196">Within hello **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="c7c0b-197">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-197">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="c7c0b-198">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-198">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="c7c0b-199">Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-199">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="c7c0b-200">Volání hello **CloudQueue.GetMessage** metoda tooread hello první zprávu ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-200">Call hello **CloudQueue.GetMessage** method tooread hello first message in hello queue.</span></span> <span data-ttu-id="c7c0b-201">Hello **CloudQueue.GetMessage** metoda umožňuje hello jiný kód čtení zpráv tak, aby žádný jiný kód, můžete upravit nebo odstranit uvítací zprávu při vaší zpracování, je neviditelné zpráva pro tooany 30 sekund (ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-201">hello **CloudQueue.GetMessage** method makes hello message invisible for 30 seconds (by default) tooany other code reading messages so that no other code can modify or delete hello message while your processing it.</span></span> <span data-ttu-id="c7c0b-202">toochange hello množství času uvítací zprávu je neviditelná, je třeba změnit hello **visibilityTimeout** parametr předávány toohello **CloudQueue.GetMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-202">toochange hello amount of time hello message is invisible, modify hello **visibilityTimeout** parameter being passed toohello **CloudQueue.GetMessage** method.</span></span>

    ```csharp
    // This message will be invisible tooother code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. <span data-ttu-id="c7c0b-203">Volání hello **CloudQueueMessage.Delete** metoda toodelete uvítací zprávu z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-203">Call hello **CloudQueueMessage.Delete** method toodelete hello message from hello queue.</span></span>

    ```csharp
    queue.DeleteMessage(message);
    ```

1. <span data-ttu-id="c7c0b-204">Aktualizace hello **ViewBag** s hello zprávu odstranit a hello název fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-204">Update hello **ViewBag** with hello message deleted, and hello name of hello queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. <span data-ttu-id="c7c0b-205">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-205">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="c7c0b-206">Na hello **přidat zobrazení** dialogové okno, zadejte **ReadMessage** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-206">On hello **Add View** dialog, enter **ReadMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="c7c0b-207">Otevřete `ReadMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-207">Open `ReadMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="c7c0b-208">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-208">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="c7c0b-209">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-209">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. <span data-ttu-id="c7c0b-210">Spuštění aplikace hello a vyberte **pro čtení nebo odstranění zprávy** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-210">Run hello application, and select **Read/Delete message** toosee results similar toohello following screen shot:</span></span>
  
    ![Čtení a odstraňování zpráv](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-hello-queue-length"></a><span data-ttu-id="c7c0b-212">Získání délky fronty hello</span><span class="sxs-lookup"><span data-stu-id="c7c0b-212">Get hello queue length</span></span>

<span data-ttu-id="c7c0b-213">Tato část ukazuje, jak tooget hello délka fronty (počet zpráv).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-213">This section illustrates how tooget hello queue length (number of messages).</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="c7c0b-214">V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-214">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="c7c0b-215">Otevřete hello `QueuesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-215">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="c7c0b-216">Přidejte metodu s názvem **GetQueueLength** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-216">Add a method called **GetQueueLength** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetQueueLength()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="c7c0b-217">V rámci hello **ReadMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-217">Within hello **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="c7c0b-218">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-218">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="c7c0b-219">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-219">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="c7c0b-220">Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-220">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="c7c0b-221">Volání hello **CloudQueue.FetchAttributes** atributy metoda tooretrieve hello fronty (včetně jeho délky).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-221">Call hello **CloudQueue.FetchAttributes** method tooretrieve hello queue's attributes (including its length).</span></span> 

    ```csharp
    queue.FetchAttributes();
    ```

6. <span data-ttu-id="c7c0b-222">Přístup hello **CloudQueue.ApproximateMessageCount** délka vlastnost tooget hello fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-222">Access hello **CloudQueue.ApproximateMessageCount** property tooget hello queue's length.</span></span>
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. <span data-ttu-id="c7c0b-223">Aktualizace hello **ViewBag** s názvem hello hello fronty a jeho délka.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-223">Update hello **ViewBag** with hello name of hello queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. <span data-ttu-id="c7c0b-224">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-224">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="c7c0b-225">Na hello **přidat zobrazení** dialogové okno, zadejte **GetQueueLength** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-225">On hello **Add View** dialog, enter **GetQueueLength** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="c7c0b-226">Otevřete `GetQueueLengthMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-226">Open `GetQueueLengthMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    hello queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. <span data-ttu-id="c7c0b-227">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-227">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="c7c0b-228">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-228">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. <span data-ttu-id="c7c0b-229">Spuštění aplikace hello a vyberte **získat délka fronty** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-229">Run hello application, and select **Get queue length** toosee results similar toohello following screen shot:</span></span>
  
    ![Získání délky fronty](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a><span data-ttu-id="c7c0b-231">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="c7c0b-231">Delete a queue</span></span>
<span data-ttu-id="c7c0b-232">Tato část ukazuje způsob toodelete fronty.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-232">This section illustrates how toodelete a queue.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="c7c0b-233">V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="c7c0b-233">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="c7c0b-234">Otevřete hello `QueuesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-234">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="c7c0b-235">Přidejte metodu s názvem **DeleteQueue** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-235">Add a method called **DeleteQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="c7c0b-236">V rámci hello **DeleteQueue** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-236">Within hello **DeleteQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="c7c0b-237">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-237">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="c7c0b-238">Získání **CloudQueueClient** objekt představuje klienta služby front.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-238">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="c7c0b-239">Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-239">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="c7c0b-240">Volání hello **CloudQueue.Delete** metoda toodelete hello fronty reprezentována hello **CloudQueue** objektu.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-240">Call hello **CloudQueue.Delete** method toodelete hello queue represented by hello **CloudQueue** object.</span></span>

    ```csharp
    queue.Delete();
    ```

1. <span data-ttu-id="c7c0b-241">Aktualizace hello **ViewBag** s názvem hello hello fronty a jeho délka.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-241">Update hello **ViewBag** with hello name of hello queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. <span data-ttu-id="c7c0b-242">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-242">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="c7c0b-243">Na hello **přidat zobrazení** dialogové okno, zadejte **DeleteQueue** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-243">On hello **Add View** dialog, enter **DeleteQueue** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="c7c0b-244">Otevřete `DeleteQueue.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-244">Open `DeleteQueue.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. <span data-ttu-id="c7c0b-245">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-245">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="c7c0b-246">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-246">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. <span data-ttu-id="c7c0b-247">Spuštění aplikace hello a vyberte **získat délka fronty** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c7c0b-247">Run hello application, and select **Get queue length** toosee results similar toohello following screen shot:</span></span>
  
    ![Odstranění fronty](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a><span data-ttu-id="c7c0b-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7c0b-249">Next steps</span></span>
<span data-ttu-id="c7c0b-250">Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="c7c0b-250">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="c7c0b-251">Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-251">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="c7c0b-252">Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="c7c0b-252">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-tables.md)
