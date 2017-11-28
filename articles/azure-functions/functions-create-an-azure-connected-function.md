---
title: "aaaCreate funkce, která se připojuje tooAzure služby | Microsoft Docs"
description: "Azure Functions toocreate aplikaci bez serveru, který se připojuje tooother Azure pomocí služby."
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
ms.openlocfilehash: 9d1f7d3b236f8d2c1a404c76aee410f6d458fb7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-toocreate-a-function-that-connects-tooother-azure-services"></a><span data-ttu-id="6e5ad-104">Azure Functions toocreate funkce, která se připojuje tooother Azure pomocí služby</span><span class="sxs-lookup"><span data-stu-id="6e5ad-104">Use Azure Functions toocreate a function that connects tooother Azure services</span></span>

<span data-ttu-id="6e5ad-105">Toto téma ukazuje, jak toocreate funkce v Azure Functions, která naslouchá toomessages na Azure Storage fronty a kopie hello zprávy toorows v tabulce Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-105">This topic shows you how toocreate a function in Azure Functions that listens toomessages on an Azure Storage queue and copies hello messages toorows in an Azure Storage table.</span></span> <span data-ttu-id="6e5ad-106">Funkce spustí časovač je použité tooload zprávy do fronty hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-106">A timer triggered function is used tooload messages into hello queue.</span></span> <span data-ttu-id="6e5ad-107">Druhý funkce přečte z fronty hello a zapíše zprávy toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-107">A second function reads from hello queue and writes messages toohello table.</span></span> <span data-ttu-id="6e5ad-108">Hello fronty a tabulky hello jsou vytvořené pro vás Azure Functions podle definice vazby hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-108">Both hello queue and hello table are created for you by Azure Functions based on hello binding definitions.</span></span> 

<span data-ttu-id="6e5ad-109">věcí toomake zajímavějšího, jednu funkci je napsána v jazyce JavaScript a hello jiných je napsán v C# skript.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-109">toomake things more interesting, one function is written in JavaScript and hello other is written in C# script.</span></span> <span data-ttu-id="6e5ad-110">To ukazuje, jak aplikace Function App může obsahovat funkce v různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-110">This demonstrates how a function app can have functions in various languages.</span></span> 

<span data-ttu-id="6e5ad-111">Předvedení tohoto scénáře zobrazuje [video na webu Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span><span class="sxs-lookup"><span data-stu-id="6e5ad-111">You can see this scenario demonstrated in a [video on Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span></span>

## <a name="create-a-function-that-writes-toohello-queue"></a><span data-ttu-id="6e5ad-112">Vytvoří funkci, která zapisuje toohello fronty</span><span class="sxs-lookup"><span data-stu-id="6e5ad-112">Create a function that writes toohello queue</span></span>

<span data-ttu-id="6e5ad-113">Před připojením tooa fronty úložiště, musíte toocreate funkci, která načte fronta zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-113">Before you can connect tooa storage queue, you need toocreate a function that loads hello message queue.</span></span> <span data-ttu-id="6e5ad-114">Tato funkce JavaScript, která používá aktivaci časovačem, který zapíše zprávu fronty toohello každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-114">This JavaScript function uses a timer trigger that writes a message toohello queue every 10 seconds.</span></span> <span data-ttu-id="6e5ad-115">Pokud nemáte účet Azure, podívejte se na hello [zkuste Azure Functions](https://functions.azure.com/try) prostředí, nebo [vytvořit účet Azure zdarma](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6e5ad-115">If you don't already have an Azure account, check out hello [Try Azure Functions](https://functions.azure.com/try) experience, or [create your free Azure account](https://azure.microsoft.com/free/).</span></span>

1. <span data-ttu-id="6e5ad-116">Přejděte toohello portál Azure a vyhledejte aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-116">Go toohello Azure portal and locate your function app.</span></span>

2. <span data-ttu-id="6e5ad-117">Klikněte na **Nová funkce** > **TimerTrigger-JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-117">Click **New Function** > **TimerTrigger-JavaScript**.</span></span> 

3. <span data-ttu-id="6e5ad-118">Název funkce hello **FunctionsBindingsDemo1**, zadejte hodnotu výrazu cron `0/10 * * * * *` pro **plán**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-118">Name hello function **FunctionsBindingsDemo1**, enter a cron expression value of `0/10 * * * * *` for **Schedule**, and then click **Create**.</span></span>
   
    ![Přidání funkce aktivované časovačem](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    <span data-ttu-id="6e5ad-120">Nyní jste vytvořili funkci aktivovanou časovačem, která se spouští každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-120">You have now created a timer triggered function that runs every 10 seconds.</span></span>

5. <span data-ttu-id="6e5ad-121">Na hello **vývoj** , klikněte na **protokoly** a zobrazit hello aktivity v protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-121">On hello **Develop** tab, click **Logs** and view hello activity in hello log.</span></span> <span data-ttu-id="6e5ad-122">Jak vidíte, každých 10 sekund se zapíše položka protokolu.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-122">You see a log entry written every 10 seconds.</span></span>
   
    ![Zobrazení hello protokolu tooverify hello funkce funguje](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a><span data-ttu-id="6e5ad-124">Přidání výstupní vazby fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="6e5ad-124">Add a message queue output binding</span></span>

1. <span data-ttu-id="6e5ad-125">Na hello **integrací** , zvolte **nový výstupní** > **Azure Queue Storage** > **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-125">On hello **Integrate** tab, choose **New Output** > **Azure Queue Storage** > **Select**.</span></span>

    ![Přidání funkce časovače triggeru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. <span data-ttu-id="6e5ad-127">Zadejte `myQueueItem` pro **název parametru zpráva** a `functions-bindings` pro **název fronty**, vyberte existující **připojení účtu úložiště** nebo klikněte na tlačítko **nové** toocreate úložiště účtu připojení a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-127">Enter `myQueueItem` for **Message parameter name** and `functions-bindings` for **Queue name**, select an existing **Storage account connection** or click **new** toocreate a storage account connection, and then click **Save**.</span></span>  

    ![Vytvoření fronty úložiště toohello vazby výstup hello](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. <span data-ttu-id="6e5ad-129">Zpět v hello **vývoj** kartě, doplňovací hello následující funkce toohello kódu:</span><span class="sxs-lookup"><span data-stu-id="6e5ad-129">Back in hello **Develop** tab, append hello following code toohello function:</span></span>
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. <span data-ttu-id="6e5ad-130">Vyhledejte hello *Pokud* příkaz přibližně řádek 9 hello funkce a vložení hello následující kód po tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-130">Locate hello *if* statement around line 9 of hello function, and insert hello following code after that statement.</span></span>
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    <span data-ttu-id="6e5ad-131">Tento kód vytvoří **myQueueItem** a nastaví její **čas** vlastnost toohello aktuální časové razítko.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-131">This code creates a **myQueueItem** and sets its **time** property toohello current timeStamp.</span></span> <span data-ttu-id="6e5ad-132">Potom přidá hello nové fronty položky toohello kontextu **myQueueItem** vazby.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-132">It then adds hello new queue item toohello context's **myQueueItem** binding.</span></span>

3. <span data-ttu-id="6e5ad-133">Klikněte na **Uložit a spustit**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-133">Click **Save and Run**.</span></span>

## <a name="view-storage-updates-by-using-storage-explorer"></a><span data-ttu-id="6e5ad-134">Zobrazení aktualizací úložiště pomocí Storage Exploreru</span><span class="sxs-lookup"><span data-stu-id="6e5ad-134">View storage updates by using Storage Explorer</span></span>
<span data-ttu-id="6e5ad-135">Můžete ověřit, že funkce funguje zobrazením zprávy ve frontě hello, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-135">You can verify that your function is working by viewing messages in hello queue you created.</span></span>  <span data-ttu-id="6e5ad-136">Tooyour fronty úložiště můžete připojit pomocí Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-136">You can connect tooyour storage queue by using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="6e5ad-137">Ale hello portál je účet úložiště snadno tooconnect tooyour pomocí Microsoft Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-137">However, hello portal makes it easy tooconnect tooyour storage account by using Microsoft Azure Storage Explorer.</span></span>

1. <span data-ttu-id="6e5ad-138">V hello **integrací** , klikněte na frontu výstup vazby > **dokumentace**, odkrýt hello připojovací řetězec pro váš účet úložiště a zkopírujte hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-138">In hello **Integrate** tab, click your queue output binding > **Documentation**, then unhide hello Connection String for your storage account and copy hello value.</span></span> <span data-ttu-id="6e5ad-139">Můžete použít tento účet úložiště tooyour tooconnect hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-139">You use this value tooconnect tooyour storage account.</span></span>

    ![Stáhnutí Azure Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. <span data-ttu-id="6e5ad-141">Pokud jste tak již neučinili, stáhněte a nainstalujte [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="6e5ad-141">If you haven't already done so, download and install [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span></span> 
 
3. <span data-ttu-id="6e5ad-142">Ve Storage Exploreru, klikněte na tlačítko hello připojení ikona úložiště tooAzure, vložte hello připojovací řetězec v poli hello a dokončete průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-142">In Storage Explorer, click hello connect tooAzure Storage icon, paste hello connection string in hello field, and complete hello wizard.</span></span>

    ![Přidání připojení ve Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. <span data-ttu-id="6e5ad-144">V části **místní a připojené**, rozbalte položku **účty úložiště** > váš účet úložiště > **fronty** > **funkce vazby**a ověřte, zda jsou zprávy zapisovány toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-144">Under **Local and attached**, expand **Storage Accounts** > your storage account > **Queues** > **functions-bindings** and verify that messages are written toohello queue.</span></span>

    ![Zobrazení zpráv ve frontě hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    <span data-ttu-id="6e5ad-146">Pokud hello fronta neexistuje nebo je prázdný, je pravděpodobně problém s vaší vazba funkce nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-146">If hello queue does not exist or is empty, there is most likely a problem with your function binding or code.</span></span>

## <a name="create-a-function-that-reads-from-hello-queue"></a><span data-ttu-id="6e5ad-147">Vytvoří funkci, která čte z fronty hello</span><span class="sxs-lookup"><span data-stu-id="6e5ad-147">Create a function that reads from hello queue</span></span>

<span data-ttu-id="6e5ad-148">Teď, když máte zprávy přidávané toohello fronty, můžete vytvořit další funkce, která čte z fronty hello a zápisy hello zprávy trvale tooan tabulky Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-148">Now that you have messages being added toohello queue, you can create another function that reads from hello queue and writes hello messages permanently tooan Azure Storage table.</span></span>

1. <span data-ttu-id="6e5ad-149">Klikněte na **Nová funkce** > **QueueTrigger-CSharp**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-149">Click **New Function** > **QueueTrigger-CSharp**.</span></span> 
 
2. <span data-ttu-id="6e5ad-150">Název funkce hello `FunctionsBindingsDemo2`, zadejte **funkce vazby** v hello **název fronty** pole, vybrat existující účet úložiště nebo vytvořit a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-150">Name hello function `FunctionsBindingsDemo2`, enter **functions-bindings** in hello **Queue name** field, select an existing storage account or create one, and then click **Create**.</span></span>

    ![Přidání funkce časovače výstupní fronty](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. <span data-ttu-id="6e5ad-152">(Volitelné) Můžete ověřit, že nové funkce hello funguje zobrazením hello novou frontu na Storage Explorer jako před.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-152">(Optional) You can verify that hello new function works by viewing hello new queue in Storage Explorer as before.</span></span> <span data-ttu-id="6e5ad-153">Můžete také použít Průzkumník cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-153">You can also use Cloud Explorer in Visual Studio.</span></span>  

4. <span data-ttu-id="6e5ad-154">(Volitelné) Aktualizujte hello **funkce vazby** ve frontě a Všimněte si, že položky byly odebrány z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-154">(Optional) Refresh hello **functions-bindings** queue and notice that items have been removed from hello queue.</span></span> <span data-ttu-id="6e5ad-155">Hello odebrání dochází, je funkce hello vázané toohello **funkce vazby** jako vstupní funkce aktivační události a hello čte hello fronty ve frontě.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-155">hello removal occurs because hello function is bound toohello **functions-bindings** queue as an input trigger and hello function reads hello queue.</span></span> 
 
## <a name="add-a-table-output-binding"></a><span data-ttu-id="6e5ad-156">Přidání výstupní vazby tabulky</span><span class="sxs-lookup"><span data-stu-id="6e5ad-156">Add a table output binding</span></span>

1. <span data-ttu-id="6e5ad-157">V aplikaci FunctionsBindingsDemo2 klikněte na **Integrace** > **Nový výstup** > **Azure Table Storage** > **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-157">In FunctionsBindingsDemo2, click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

    ![Přidat tabulku vazby tooan Azure Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. <span data-ttu-id="6e5ad-159">Zadejte `functionbindings` jako **Název tabulky** a `myTable` jako **Název parametru tabulky**, zvolte **Připojení účtu úložiště** nebo vytvořte nové, a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-159">Enter `functionbindings` for **Table name** and `myTable` for **Table parameter name**, choose a **Storage account connection** or create a new one, and then click **Save**.</span></span>

    ![Nakonfigurujte vazbu tabulky úložiště hello](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. <span data-ttu-id="6e5ad-161">V hello **vývoj** kartě, nahraďte existující kód funkce hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e5ad-161">In hello **Develop** tab, replace hello existing function code with hello following:</span></span>
   
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
        
        // Add hello item toohello table binding collection.
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
    <span data-ttu-id="6e5ad-162">Hello **TableItem** třída reprezentuje řádek v tabulce hello úložiště a přidejte položku toohello hello `myTable` kolekce **TableItem** objekty.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-162">hello **TableItem** class represents a row in hello storage table, and you add hello item toohello `myTable` collection of **TableItem** objects.</span></span> <span data-ttu-id="6e5ad-163">Je nutné nastavit hello **PartitionKey** a **RowKey** vlastnosti toobe možné tooinsert do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-163">You must set hello **PartitionKey** and **RowKey** properties toobe able tooinsert into hello table.</span></span>

4. <span data-ttu-id="6e5ad-164">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-164">Click **Save**.</span></span>  <span data-ttu-id="6e5ad-165">Nakonec můžete ověřit hello funkce funguje zobrazením hello tabulky v Průzkumníka úložiště nebo cloudu Průzkumníka Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-165">Finally, you can verify hello function works by viewing hello table in Storage explorer or Visual Studio Cloud Explorer.</span></span>

5. <span data-ttu-id="6e5ad-166">(Volitelné) Ve vašem účtu úložiště v Storage Explorer rozbalte **tabulky** > **functionsbindings** a ověřte, zda jsou řádky přidán toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-166">(Optional) In your storage account in Storage Explorer, expand **Tables** > **functionsbindings** and verify that rows are added toohello table.</span></span> <span data-ttu-id="6e5ad-167">Můžete provést stejný hello v Průzkumníku cloudu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-167">You can do hello same in Cloud Explorer in Visual Studio.</span></span>

    ![Zobrazení řádků v tabulce hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    <span data-ttu-id="6e5ad-169">Pokud hello tabulka neexistuje nebo je prázdný, je pravděpodobně problém s vaší vazba funkce nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-169">If hello table does not exist or is empty, there is most likely a problem with your function binding or code.</span></span> 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a><span data-ttu-id="6e5ad-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e5ad-170">Next steps</span></span>
<span data-ttu-id="6e5ad-171">Další informace o Azure Functions najdete v tématu hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="6e5ad-171">For more information about Azure Functions, see hello following topics:</span></span>

* [<span data-ttu-id="6e5ad-172">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="6e5ad-172">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="6e5ad-173">Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-173">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="6e5ad-174">Testování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="6e5ad-174">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="6e5ad-175">Toto téma popisuje různé nástroje a techniky pro testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-175">Describes various tools and techniques for testing your functions.</span></span>
* [<span data-ttu-id="6e5ad-176">Jak tooscale Azure Functions</span><span class="sxs-lookup"><span data-stu-id="6e5ad-176">How tooscale Azure Functions</span></span>](functions-scale.md)  
  <span data-ttu-id="6e5ad-177">Popisuje plány služby, které jsou dostupné s Azure Functions, včetně hello spotřeba hostingový plán a jak toochoose hello správného plánu.</span><span class="sxs-lookup"><span data-stu-id="6e5ad-177">Discusses service plans available with Azure Functions, including hello Consumption hosting plan, and how toochoose hello right plan.</span></span> 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

