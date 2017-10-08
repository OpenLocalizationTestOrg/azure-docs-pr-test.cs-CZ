---
title: "aaaGet začít s Azure queue storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí fronty Azure storage po připojení účtu úložiště tooa pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: kraigb
ms.openlocfilehash: 415a437c4ce60b1e2e328f8e937c73b0d5c50e78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled

Azure queue storage poskytuje cloudu zasílání zpráv mezi součástmi aplikace. Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle. Fronty úložiště poskytuje asynchronní zasílání zpráv pro komunikaci mezi součástmi aplikací, zda jsou spuštěny v hello cloudu, na ploše hello, na místním serveru nebo na mobilním zařízení. Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.

Tento kurz ukazuje, jak kód toowrite ASP.NET pro některé běžné scénáře použití fronty Azure storage entity. Mezi tyto scénáře patří běžné úkoly, jako je například vytváření fronty Azure a přidání, úprava, čtení a odebrání zprávy do fronty.

##<a name="prerequisites"></a>Požadavky

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Účet služby Azure Storage](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Vytvořit řadič MVC 

1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce hello, vyberte **Přidat -> řadiče**.

    ![Přidat řadič tooan aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. Na hello **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. Na hello **přidat kontroler** dialogové okno, název hello řadič *QueuesController*a vyberte **přidat**.

    ![Název hello MVC jsou řadič MVC](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Přidejte následující hello *pomocí* toohello direktivy `QueuesController.cs` souboru:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a>Vytvoření fronty

Hello následující kroky popisují jak toocreate fronty:

> [!NOTE]
> 
> V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `QueuesController.cs` souboru. 

1. Přidejte metodu s názvem **CreateQueue** , který vrací **ActionResult**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **CreateQueue** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudQueueClient** objekt představuje klienta služby front.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Získání **CloudQueue** objekt, který reprezentuje název požadované fronty toohello odkaz. Hello **CloudQueueClient.GetQueueReference** metoda neprovede požadavek fronty úložiště. odkaz Hello se vrátí, zda text hello fronta existuje. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Volání hello **CloudQueue.CreateIfNotExists** metoda toocreate hello fronty, pokud ještě neexistuje. Hello **CloudQueue.CreateIfNotExists** metoda vrátí **true** Pokud hello fronta neexistuje a je úspěšně vytvořen. V opačném **false** je vrácen.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Aktualizace hello **ViewBag** s názvem hello hello fronty.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **CreateQueue** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `CreateQueue.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Spuštění aplikace hello a vyberte **fronty vytvořit** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Vytvoření fronty](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Jak je uvedeno nahoře, hello **CloudQueue.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření fronty hello. Proto pokud hello aplikaci spustíte, když hello fronta existuje, hello metoda vrátí **false**. aplikace hello toorun vícekrát, je nutné odstranit hello fronty před spuštěním aplikace hello znovu. Odstraňování hello fronty, můžete to udělat pomocí hello **CloudQueue.Delete** metoda. Můžete také odstranit hello fronty pomocí hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-tooa-queue"></a>Přidat tooa fronty zpráv

Jakmile jste [vytvořili frontu](#create-a-queue), můžete přidat toothat fronty zpráv. Tato část vás provede procesem přidání fronty zpráv tooa *zkušební fronty*. 

> [!NOTE]
> 
> V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `QueuesController.cs` souboru.

1. Přidejte metodu s názvem **AddMessage** , který vrací **ActionResult**.

    ```csharp
    public ActionResult AddMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **AddMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudQueueClient** objekt představuje klienta služby front.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Vytvoření hello **CloudQueueMessage** objekt reprezentující uvítací zprávu chcete tooadd toohello fronty. A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Volání hello **CloudQueue.AddMessage** metoda tooadd hello messaged toohello fronty.

    ```csharp
    queue.AddMessage(message);
    ```

1. Vytvoření a nastavení několika **ViewBag** vlastnosti pro zobrazení v zobrazení hello.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **AddMessage** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `AddMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    hello message '@ViewBag.Message' was added toohello queue '@ViewBag.QueueName'.
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Spuštění aplikace hello a vyberte **přidat zprávu** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Přidat zprávu](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

Hello dva oddíly - [přečte zprávu z fronty bez odebráním](#read-a-message-from-a-queue-without-removing-it) a [pro čtení a odebere zprávu z fronty](#read-and-remove-a-message-from-a-queue) -ilustrují, jak tooread zpráv z fronty.  

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Přečte zprávu z fronty bez odebráním

Tato část ukazuje způsob toopeek na zprávu ve frontě (první zprávy pro čtení hello bez odebráním).  

> [!NOTE]
> 
> V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `QueuesController.cs` souboru.

1. Přidejte metodu s názvem **PeekMessage** , který vrací **ActionResult**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **PeekMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudQueueClient** objekt představuje klienta služby front.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Volání hello **CloudQueue.PeekMessage** metoda tooread hello první zprávu ve frontě hello bez odebere ji z fronty hello. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Aktualizace hello **ViewBag** se dvěma hodnotami: název fronty hello a uvítací zprávu, která byla načtena. Hello **CloudQueueMessage** objekt poskytuje dvě vlastnosti pro získání hodnoty objektu hello: **CloudQueueMessage.AsBytes** a **CloudQueueMessage.AsString**. **AsString** (používá se v tomto příkladu) vrátí řetězec, zatímco **AsBytes** vrátí pole bajtů.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **PeekMessage** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `PeekMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

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

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Spuštění aplikace hello a vyberte **funkce Náhled zpráva** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Prohlížení zpráv](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Přečtěte si a odebrání zprávy z fronty

V této části se dozvíte, jak tooread a odebrání zprávy z fronty.   

> [!NOTE]
> 
> V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `QueuesController.cs` souboru.

1. Přidejte metodu s názvem **ReadMessage** , který vrací **ActionResult**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **ReadMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudQueueClient** objekt představuje klienta služby front.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Volání hello **CloudQueue.GetMessage** metoda tooread hello první zprávu ve frontě hello. Hello **CloudQueue.GetMessage** metoda umožňuje hello jiný kód čtení zpráv tak, aby žádný jiný kód, můžete upravit nebo odstranit uvítací zprávu při vaší zpracování, je neviditelné zpráva pro tooany 30 sekund (ve výchozím nastavení). toochange hello množství času uvítací zprávu je neviditelná, je třeba změnit hello **visibilityTimeout** parametr předávány toohello **CloudQueue.GetMessage** metoda.

    ```csharp
    // This message will be invisible tooother code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Volání hello **CloudQueueMessage.Delete** metoda toodelete uvítací zprávu z fronty hello.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Aktualizace hello **ViewBag** s hello zprávu odstranit a hello název fronty hello.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **ReadMessage** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `ReadMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

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

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Spuštění aplikace hello a vyberte **pro čtení nebo odstranění zprávy** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Čtení a odstraňování zpráv](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-hello-queue-length"></a>Získání délky fronty hello

Tato část ukazuje, jak tooget hello délka fronty (počet zpráv). 

> [!NOTE]
> 
> V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `QueuesController.cs` souboru.

1. Přidejte metodu s názvem **GetQueueLength** , který vrací **ActionResult**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **ReadMessage** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudQueueClient** objekt představuje klienta služby front.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Volání hello **CloudQueue.FetchAttributes** atributy metoda tooretrieve hello fronty (včetně jeho délky). 

    ```csharp
    queue.FetchAttributes();
    ```

6. Přístup hello **CloudQueue.ApproximateMessageCount** délka vlastnost tooget hello fronty.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Aktualizace hello **ViewBag** s názvem hello hello fronty a jeho délka.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **GetQueueLength** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `GetQueueLengthMessage.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    hello queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Spuštění aplikace hello a vyberte **získat délka fronty** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Získání délky fronty](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Odstranění fronty
Tato část ukazuje způsob toodelete fronty. 

> [!NOTE]
> 
> V této části se předpokládá dokončení kroků hello [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `QueuesController.cs` souboru.

1. Přidejte metodu s názvem **DeleteQueue** , který vrací **ActionResult**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. V rámci hello **DeleteQueue** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Získání **CloudQueueClient** objekt představuje klienta služby front.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Získání **CloudQueueContainer** objekt, který reprezentuje fronty toohello odkaz. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Volání hello **CloudQueue.Delete** metoda toodelete hello fronty reprezentována hello **CloudQueue** objektu.

    ```csharp
    queue.Delete();
    ```

1. Aktualizace hello **ViewBag** s názvem hello hello fronty a jeho délka.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **fronty**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **DeleteQueue** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `DeleteQueue.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Spuštění aplikace hello a vyberte **získat délka fronty** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Odstranění fronty](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Další kroky
Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.

  * [Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
