---
title: "Vytvoření funkce připojující se ke službám Azure | Dokumentace Microsoftu"
description: "Pomocí Azure Functions vytvořte aplikaci bez serveru, která se připojuje k jiným službám Azure."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 65964a322f0adab4f648fb350bedb77b46bf9054
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a><span data-ttu-id="3503f-104">Pomocí Azure Functions vytvořte funkci, která se připojuje k jiným službám Azure.</span><span class="sxs-lookup"><span data-stu-id="3503f-104">Use Azure Functions to create a function that connects to other Azure services</span></span>

<span data-ttu-id="3503f-105">Toto téma ukazuje, jak vytvořit funkci v Azure Functions, která naslouchá zprávám ve frontě služby Azure Storage a kopíruje zprávy do řádků v tabulce Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3503f-105">This topic shows you how to create a function in Azure Functions that listens to messages on an Azure Storage queue and copies the messages to rows in an Azure Storage table.</span></span> <span data-ttu-id="3503f-106">Funkce aktivovaná časovačem slouží k načítání zpráv do fronty.</span><span class="sxs-lookup"><span data-stu-id="3503f-106">A timer triggered function is used to load messages into the queue.</span></span> <span data-ttu-id="3503f-107">Druhá funkce čte z fronty a zapisuje zprávy do tabulky.</span><span class="sxs-lookup"><span data-stu-id="3503f-107">A second function reads from the queue and writes messages to the table.</span></span> <span data-ttu-id="3503f-108">Frontu i tabulku za vás vytvoří služba Azure Functions na základě definic vazeb.</span><span class="sxs-lookup"><span data-stu-id="3503f-108">Both the queue and the table are created for you by Azure Functions based on the binding definitions.</span></span> 

<span data-ttu-id="3503f-109">Aby to bylo zajímavější, jedna funkce je napsaná v JavaScriptu, zatímco druhá je skript v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="3503f-109">To make things more interesting, one function is written in JavaScript and the other is written in C# script.</span></span> <span data-ttu-id="3503f-110">To ukazuje, jak aplikace Function App může obsahovat funkce v různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="3503f-110">This demonstrates how a function app can have functions in various languages.</span></span> 

<span data-ttu-id="3503f-111">Předvedení tohoto scénáře zobrazuje [video na webu Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span><span class="sxs-lookup"><span data-stu-id="3503f-111">You can see this scenario demonstrated in a [video on Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span></span>

## <a name="create-a-function-that-writes-to-the-queue"></a><span data-ttu-id="3503f-112">Vytvoření funkce, která zapisuje do fronty</span><span class="sxs-lookup"><span data-stu-id="3503f-112">Create a function that writes to the queue</span></span>

<span data-ttu-id="3503f-113">Než se budete moci připojit k frontě úložiště, je nutné vytvořit funkci, která načítá frontu zpráv.</span><span class="sxs-lookup"><span data-stu-id="3503f-113">Before you can connect to a storage queue, you need to create a function that loads the message queue.</span></span> <span data-ttu-id="3503f-114">Tato funkce JavaScriptu využívá trigger časovače, který každých 10 sekund zapisuje zprávu do fronty.</span><span class="sxs-lookup"><span data-stu-id="3503f-114">This JavaScript function uses a timer trigger that writes a message to the queue every 10 seconds.</span></span> <span data-ttu-id="3503f-115">Pokud ještě nemáte účet Azure, [vyzkoušejte si službu Azure Functions](https://functions.azure.com/try) nebo [si vytvořte bezplatný účet Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="3503f-115">If you don't already have an Azure account, check out the [Try Azure Functions](https://functions.azure.com/try) experience, or [create your free Azure account](https://azure.microsoft.com/free/).</span></span>

1. <span data-ttu-id="3503f-116">Přejděte na Azure Portal a vyhledejte aplikaci Function App.</span><span class="sxs-lookup"><span data-stu-id="3503f-116">Go to the Azure portal and locate your function app.</span></span>

2. <span data-ttu-id="3503f-117">Klikněte na **Nová funkce** > **TimerTrigger-JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="3503f-117">Click **New Function** > **TimerTrigger-JavaScript**.</span></span> 

3. <span data-ttu-id="3503f-118">Pojmenujte funkci **FunctionsBindingsDemo1**, jako **Plán** zadejte hodnotu výrazu Cron `0/10 * * * * *` a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3503f-118">Name the function **FunctionsBindingsDemo1**, enter a cron expression value of `0/10 * * * * *` for **Schedule**, and then click **Create**.</span></span>
   
    ![Přidání funkce aktivované časovačem](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    <span data-ttu-id="3503f-120">Nyní jste vytvořili funkci aktivovanou časovačem, která se spouští každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="3503f-120">You have now created a timer triggered function that runs every 10 seconds.</span></span>

5. <span data-ttu-id="3503f-121">Na kartě **Vývoj** klikněte na **Protokoly** a zobrazte aktivitu v protokolu.</span><span class="sxs-lookup"><span data-stu-id="3503f-121">On the **Develop** tab, click **Logs** and view the activity in the log.</span></span> <span data-ttu-id="3503f-122">Jak vidíte, každých 10 sekund se zapíše položka protokolu.</span><span class="sxs-lookup"><span data-stu-id="3503f-122">You see a log entry written every 10 seconds.</span></span>
   
    ![Zobrazení protokolu pro kontrolu funkčnosti funkce](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a><span data-ttu-id="3503f-124">Přidání výstupní vazby fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="3503f-124">Add a message queue output binding</span></span>

1. <span data-ttu-id="3503f-125">Na kartě **Integrace** zvolte **Nový výstup** > **Azure Queue Storage** > **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="3503f-125">On the **Integrate** tab, choose **New Output** > **Azure Queue Storage** > **Select**.</span></span>

    ![Přidání funkce časovače triggeru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. <span data-ttu-id="3503f-127">Zadejte `myQueueItem` jako **Název parametru zprávy** a `functions-bindings` jako **Název fronty**, vyberte existující **Připojení účtu úložiště** nebo kliknutím na **nový** vytvořte připojení účtu úložiště a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3503f-127">Enter `myQueueItem` for **Message parameter name** and `functions-bindings` for **Queue name**, select an existing **Storage account connection** or click **new** to create a storage account connection, and then click **Save**.</span></span>  

    ![Vytvoření výstupní vazby na frontu úložiště](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. <span data-ttu-id="3503f-129">Na kartě **Vývoj** přidejte k funkci následující kód:</span><span class="sxs-lookup"><span data-stu-id="3503f-129">Back in the **Develop** tab, append the following code to the function:</span></span>
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. <span data-ttu-id="3503f-130">Vyhledejte výraz *if* kolem řádku 9 funkce a vložte za tento výraz následující kód.</span><span class="sxs-lookup"><span data-stu-id="3503f-130">Locate the *if* statement around line 9 of the function, and insert the following code after that statement.</span></span>
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    <span data-ttu-id="3503f-131">Tento kód vytvoří **myQueueItem** a nastaví vlastnost **time** (čas) na aktuální časové razítko.</span><span class="sxs-lookup"><span data-stu-id="3503f-131">This code creates a **myQueueItem** and sets its **time** property to the current timeStamp.</span></span> <span data-ttu-id="3503f-132">Pak novou položku fronty přidá do vazby **myQueueItem** podle kontextu.</span><span class="sxs-lookup"><span data-stu-id="3503f-132">It then adds the new queue item to the context's **myQueueItem** binding.</span></span>

3. <span data-ttu-id="3503f-133">Klikněte na **Uložit a spustit**.</span><span class="sxs-lookup"><span data-stu-id="3503f-133">Click **Save and Run**.</span></span>

## <a name="view-storage-updates-by-using-storage-explorer"></a><span data-ttu-id="3503f-134">Zobrazení aktualizací úložiště pomocí Storage Exploreru</span><span class="sxs-lookup"><span data-stu-id="3503f-134">View storage updates by using Storage Explorer</span></span>
<span data-ttu-id="3503f-135">Fungování vaší funkce můžete ověřit zobrazením zpráv ve frontě, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3503f-135">You can verify that your function is working by viewing messages in the queue you created.</span></span>  <span data-ttu-id="3503f-136">Můžete se připojit k vaší frontě úložiště pomocí Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3503f-136">You can connect to your storage queue by using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="3503f-137">Portál však umožňuje snadné připojení k účtu úložiště pomocí Microsoft Azure Storage Exploreru.</span><span class="sxs-lookup"><span data-stu-id="3503f-137">However, the portal makes it easy to connect to your storage account by using Microsoft Azure Storage Explorer.</span></span>

1. <span data-ttu-id="3503f-138">Na kartě **Integrace** klikněte na výstupní vazbu fronty > **Dokumentace**, zobrazte Připojovací řetězec pro účet úložiště a zkopírujte jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3503f-138">In the **Integrate** tab, click your queue output binding > **Documentation**, then unhide the Connection String for your storage account and copy the value.</span></span> <span data-ttu-id="3503f-139">Tuto hodnotu použijete pro připojení k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3503f-139">You use this value to connect to your storage account.</span></span>

    ![Stáhnutí Azure Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. <span data-ttu-id="3503f-141">Pokud jste tak již neučinili, stáhněte a nainstalujte [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="3503f-141">If you haven't already done so, download and install [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span></span> 
 
3. <span data-ttu-id="3503f-142">Ve Storage Exploreru klikněte na ikonu připojit ke službě Azure Storage, do pole vložte připojovací řetězec a dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="3503f-142">In Storage Explorer, click the connect to Azure Storage icon, paste the connection string in the field, and complete the wizard.</span></span>

    ![Přidání připojení ve Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. <span data-ttu-id="3503f-144">V části **Místní a připojené** rozbalte **Účty úložiště** > váš účet úložiště > **Fronty** > **functions-bindings** a ověřte, že se zprávy zapisují do fronty.</span><span class="sxs-lookup"><span data-stu-id="3503f-144">Under **Local and attached**, expand **Storage Accounts** > your storage account > **Queues** > **functions-bindings** and verify that messages are written to the queue.</span></span>

    ![Zobrazení zpráv ve frontě](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    <span data-ttu-id="3503f-146">Pokud fronta neexistuje nebo je prázdná, pravděpodobně je nějaký problém s vazbami nebo kódem vaší funkce.</span><span class="sxs-lookup"><span data-stu-id="3503f-146">If the queue does not exist or is empty, there is most likely a problem with your function binding or code.</span></span>

## <a name="create-a-function-that-reads-from-the-queue"></a><span data-ttu-id="3503f-147">Vytvoření funkce, která čte z fronty</span><span class="sxs-lookup"><span data-stu-id="3503f-147">Create a function that reads from the queue</span></span>

<span data-ttu-id="3503f-148">Teď, když se zprávy přidávají do fronty, můžete vytvořit další funkci, která neustále čte z fronty a zapisuje zprávy do tabulky Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3503f-148">Now that you have messages being added to the queue, you can create another function that reads from the queue and writes the messages permanently to an Azure Storage table.</span></span>

1. <span data-ttu-id="3503f-149">Klikněte na **Nová funkce** > **QueueTrigger-CSharp**.</span><span class="sxs-lookup"><span data-stu-id="3503f-149">Click **New Function** > **QueueTrigger-CSharp**.</span></span> 
 
2. <span data-ttu-id="3503f-150">Pojmenujte funkci `FunctionsBindingsDemo2`, do pole **Název fronty** zadejte **functions-bindings**, vyberte existující účet úložiště nebo vytvořte nový, a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3503f-150">Name the function `FunctionsBindingsDemo2`, enter **functions-bindings** in the **Queue name** field, select an existing storage account or create one, and then click **Create**.</span></span>

    ![Přidání funkce časovače výstupní fronty](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. <span data-ttu-id="3503f-152">(Volitelné) Fungování nové funkce můžete ověřit zobrazením nové fronty ve Storage Exploreru stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="3503f-152">(Optional) You can verify that the new function works by viewing the new queue in Storage Explorer as before.</span></span> <span data-ttu-id="3503f-153">Můžete také použít Průzkumník cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3503f-153">You can also use Cloud Explorer in Visual Studio.</span></span>  

4. <span data-ttu-id="3503f-154">(Volitelné) Obnovte frontu **functions-bindings** a všimněte si, že z fronty byly odebrány položky.</span><span class="sxs-lookup"><span data-stu-id="3503f-154">(Optional) Refresh the **functions-bindings** queue and notice that items have been removed from the queue.</span></span> <span data-ttu-id="3503f-155">K odebrání dochází, protože je funkce vázaná na frontu **functions-bindings** jako vstupní trigger a tato funkce čte z fronty.</span><span class="sxs-lookup"><span data-stu-id="3503f-155">The removal occurs because the function is bound to the **functions-bindings** queue as an input trigger and the function reads the queue.</span></span> 
 
## <a name="add-a-table-output-binding"></a><span data-ttu-id="3503f-156">Přidání výstupní vazby tabulky</span><span class="sxs-lookup"><span data-stu-id="3503f-156">Add a table output binding</span></span>

1. <span data-ttu-id="3503f-157">V aplikaci FunctionsBindingsDemo2 klikněte na **Integrace** > **Nový výstup** > **Azure Table Storage** > **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="3503f-157">In FunctionsBindingsDemo2, click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

    ![Přidání vazby na tabulku Azure Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. <span data-ttu-id="3503f-159">Zadejte `functionbindings` jako **Název tabulky** a `myTable` jako **Název parametru tabulky**, zvolte **Připojení účtu úložiště** nebo vytvořte nové, a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3503f-159">Enter `functionbindings` for **Table name** and `myTable` for **Table parameter name**, choose a **Storage account connection** or create a new one, and then click **Save**.</span></span>

    ![Konfigurace vazby tabulky Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. <span data-ttu-id="3503f-161">Na kartě **Vývoj** nahraďte stávající kód funkce následujícím:</span><span class="sxs-lookup"><span data-stu-id="3503f-161">In the **Develop** tab, replace the existing function code with the following:</span></span>
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    <span data-ttu-id="3503f-162">Třída **TableItem** reprezentuje řádek v tabulce úložiště a položky přidáváte do kolekce `myTable` objektů **TableItem**.</span><span class="sxs-lookup"><span data-stu-id="3503f-162">The **TableItem** class represents a row in the storage table, and you add the item to the `myTable` collection of **TableItem** objects.</span></span> <span data-ttu-id="3503f-163">Abyste mohli vkládat do tabulky, je třeba nastavit vlastnosti **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3503f-163">You must set the **PartitionKey** and **RowKey** properties to be able to insert into the table.</span></span>

4. <span data-ttu-id="3503f-164">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3503f-164">Click **Save**.</span></span>  <span data-ttu-id="3503f-165">Nakonec můžete ověřit fungování funkce zobrazením tabulky ve Storage Exploreru nebo v Průzkumníku cloudu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3503f-165">Finally, you can verify the function works by viewing the table in Storage explorer or Visual Studio Cloud Explorer.</span></span>

5. <span data-ttu-id="3503f-166">(Volitelné) Ve svém účtu úložiště ve Storage Exploreru rozbalte **Tabulky** > **functionsbindings** a ověřte, že se do tabulky přidávají řádky.</span><span class="sxs-lookup"><span data-stu-id="3503f-166">(Optional) In your storage account in Storage Explorer, expand **Tables** > **functionsbindings** and verify that rows are added to the table.</span></span> <span data-ttu-id="3503f-167">To samé můžete udělat v Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3503f-167">You can do the same in Cloud Explorer in Visual Studio.</span></span>

    ![Zobrazení řádků v tabulce](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    <span data-ttu-id="3503f-169">Pokud tabulka neexistuje nebo je prázdná, pravděpodobně je nějaký problém s vazbami nebo kódem vaší funkce.</span><span class="sxs-lookup"><span data-stu-id="3503f-169">If the table does not exist or is empty, there is most likely a problem with your function binding or code.</span></span> 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a><span data-ttu-id="3503f-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3503f-170">Next steps</span></span>
<span data-ttu-id="3503f-171">Další informace o službě Azure Functions najdete v těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="3503f-171">For more information about Azure Functions, see the following topics:</span></span>

* [<span data-ttu-id="3503f-172">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3503f-172">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="3503f-173">Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="3503f-173">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="3503f-174">Testování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3503f-174">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="3503f-175">Toto téma popisuje různé nástroje a techniky pro testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="3503f-175">Describes various tools and techniques for testing your functions.</span></span>
* [<span data-ttu-id="3503f-176">Postup škálování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3503f-176">How to scale Azure Functions</span></span>](functions-scale.md)  
  <span data-ttu-id="3503f-177">Toto téma popisuje plány služby, které jsou dostupné se službou Azure Functions (včetně plánu hostování Consumption), a výběr správného plánu.</span><span class="sxs-lookup"><span data-stu-id="3503f-177">Discusses service plans available with Azure Functions, including the Consumption hosting plan, and how to choose the right plan.</span></span> 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

