---
title: "aaaCreate funkce v Azure aktivovány zprávy fronty | Microsoft Docs"
description: "Použití Azure Functions toocreate bez serveru funkce, které je vyvolána zprávy odeslané tooan fronty Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/17/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 44db90fa80bf77e31bf53dddabd7136de5800b11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-messages-tooan-azure-storage-queue-using-functions"></a><span data-ttu-id="c3fb0-103">Přidat fronty Azure Storage tooan zprávy pomocí funkce</span><span class="sxs-lookup"><span data-stu-id="c3fb0-103">Add messages tooan Azure Storage queue using Functions</span></span>

<span data-ttu-id="c3fb0-104">V Azure Functions poskytují vstupní a výstupní vazby dat deklarativní způsob tooconnect tooexternal služby z funkce.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-104">In Azure Functions, input and output bindings provide a declarative way tooconnect tooexternal service data from your function.</span></span> <span data-ttu-id="c3fb0-105">V tomto tématu zjistěte, jak tooupdate existující funkce přidáním výstup vazby, která odesílá zprávy tooAzure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-105">In this topic, learn how tooupdate an existing function by adding an output binding that sends messages tooAzure Queue storage.</span></span>  

![Zobrazení zpráv v protokolech hello.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a><span data-ttu-id="c3fb0-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c3fb0-107">Prerequisites</span></span> 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* <span data-ttu-id="c3fb0-108">Nainstalujte hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="c3fb0-108">Install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <span data-ttu-id="c3fb0-109"><a name="add-binding"></a>Přidání výstupní vazby</span><span class="sxs-lookup"><span data-stu-id="c3fb0-109"><a name="add-binding"></a>Add an output binding</span></span>
 
1. <span data-ttu-id="c3fb0-110">Rozbalte aplikaci Function App i funkci.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-110">Expand both your function app and your function.</span></span>

2. <span data-ttu-id="c3fb0-111">Vyberte **integrací** a **+ nový výstupní**, zvolte **Azure Queue storage** a zvolte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-111">Select **Integrate** and **+ New output**, then choose **Azure Queue storage** and choose **Select**.</span></span>
    
    ![Přidejte funkci tooa vazby výstupní fronty úložiště v hello portálu Azure.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. <span data-ttu-id="c3fb0-113">Použijte hello nastavení uvedeného v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="c3fb0-113">Use hello settings as specified in hello table:</span></span> 

    ![Přidejte funkci tooa vazby výstupní fronty úložiště v hello portálu Azure.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | <span data-ttu-id="c3fb0-115">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c3fb0-115">Setting</span></span>      |  <span data-ttu-id="c3fb0-116">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="c3fb0-116">Suggested value</span></span>   | <span data-ttu-id="c3fb0-117">Popis</span><span class="sxs-lookup"><span data-stu-id="c3fb0-117">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="c3fb0-118">**Název fronty**</span><span class="sxs-lookup"><span data-stu-id="c3fb0-118">**Queue name**</span></span>   | <span data-ttu-id="c3fb0-119">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="c3fb0-119">myqueue-items</span></span>    | <span data-ttu-id="c3fb0-120">Název Hello hello fronty tooconnect tooin účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-120">hello name of hello queue tooconnect tooin your Storage account.</span></span> |
    | <span data-ttu-id="c3fb0-121">**Připojení k účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="c3fb0-121">**Storage account connection**</span></span> | <span data-ttu-id="c3fb0-122">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="c3fb0-122">AzureWebJobStorage</span></span> | <span data-ttu-id="c3fb0-123">Můžete použít připojení účtu úložiště hello již používá aplikace funkce nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-123">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="c3fb0-124">**Název parametru zprávy**</span><span class="sxs-lookup"><span data-stu-id="c3fb0-124">**Message parameter name**</span></span> | <span data-ttu-id="c3fb0-125">outputQueueItem</span><span class="sxs-lookup"><span data-stu-id="c3fb0-125">outputQueueItem</span></span> | <span data-ttu-id="c3fb0-126">Název Hello hello výstupní parametr vazby.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-126">hello name of hello output binding parameter.</span></span> | 

4. <span data-ttu-id="c3fb0-127">Klikněte na tlačítko **Uložit** tooadd hello vazby.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-127">Click **Save** tooadd hello binding.</span></span>
 
<span data-ttu-id="c3fb0-128">Teď, když máte vazbu výstup, který je definován, je nutné tooupdate hello kód toouse hello vazby tooadd tooa fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-128">Now that you have an output binding defined, you need tooupdate hello code toouse hello binding tooadd messages tooa queue.</span></span>  

## <a name="update-hello-function-code"></a><span data-ttu-id="c3fb0-129">Aktualizovat kód funkce hello</span><span class="sxs-lookup"><span data-stu-id="c3fb0-129">Update hello function code</span></span>

1. <span data-ttu-id="c3fb0-130">Vyberte funkce toodisplay hello funkce kódu v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-130">Select your function toodisplay hello function code in hello editor.</span></span> 

2. <span data-ttu-id="c3fb0-131">Pro C# funkci, aktualizovat svou definici funkce takto tooadd hello **outputQueueItem** úložiště parametr vazby.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-131">For a C# function, update your function definition as follows tooadd hello **outputQueueItem** storage binding parameter.</span></span> <span data-ttu-id="c3fb0-132">V případě funkce v jazyce JavaScript tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-132">Skip this step for a JavaScript function.</span></span>

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. <span data-ttu-id="c3fb0-133">Přidejte následující kód toohello funkce těsně před hello metoda vrátí hello.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-133">Add hello following code toohello function just before hello method returns.</span></span> <span data-ttu-id="c3fb0-134">Použití hello odpovídající fragment kódu pro jazyk hello funkce.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-134">Use hello appropriate snippet for hello language of your function.</span></span>

    ```javascript
    context.bindings.outputQueueItem = "Name passed toohello function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed toohello function: " + name);     
    ```

4. <span data-ttu-id="c3fb0-135">Vyberte **Uložit** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-135">Select **Save** toosave changes.</span></span>

<span data-ttu-id="c3fb0-136">Aktivace protokolu HTTP toohello předaná hodnota Hello je součástí přidané toohello front zpráv.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-136">hello value passed toohello HTTP trigger is included in a message added toohello queue.</span></span>
 
## <a name="test-hello-function"></a><span data-ttu-id="c3fb0-137">Testování funkce hello</span><span class="sxs-lookup"><span data-stu-id="c3fb0-137">Test hello function</span></span> 

1. <span data-ttu-id="c3fb0-138">Po uložení změn kódu hello vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-138">After hello code changes are saved, select **Run**.</span></span> 

    ![Přidejte funkci tooa vazby výstupní fronty úložiště v hello portálu Azure.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. <span data-ttu-id="c3fb0-140">Zkontrolujte toomake protokoly hello se, zda text hello funkce proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-140">Check hello logs toomake sure that hello function succeeded.</span></span> <span data-ttu-id="c3fb0-141">Novou frontu s názvem **outqueue** hello runtime Functions při hello výstup vazby se používá nejprve vytvoří ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-141">A new queue named **outqueue** is created in your Storage account by hello Functions runtime when hello output binding is first used.</span></span>

<span data-ttu-id="c3fb0-142">V dalším kroku se můžete připojit tooyour úložiště účet tooverify hello novou frontu a uvítací zprávu, že jste přidali tooit.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-142">Next, you can connect tooyour storage account tooverify hello new queue and hello message you added tooit.</span></span> 

## <a name="connect-toohello-queue"></a><span data-ttu-id="c3fb0-143">Připojit toohello fronty</span><span class="sxs-lookup"><span data-stu-id="c3fb0-143">Connect toohello queue</span></span>

<span data-ttu-id="c3fb0-144">Přeskočit hello první tři kroky, pokud je již nainstalován Storage Explorer a připojení se účet úložiště tooyour.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-144">Skip hello first three steps if you have already installed Storage Explorer and connected it tooyour storage account.</span></span>    

1. <span data-ttu-id="c3fb0-145">Ve funkci, zvolte **integrací** a hello nové **Azure Queue storage** výstup vazby, pak rozbalte **dokumentaci**.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-145">In your function, choose **Integrate** and hello new **Azure Queue storage** output binding, then expand **Documentation**.</span></span> <span data-ttu-id="c3fb0-146">Zkopírujte nastavení **Název účtu** i **Klíč účtu**.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-146">Copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="c3fb0-147">Používáte účet úložiště toohello tooconnect tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-147">You use these credentials tooconnect toohello storage account.</span></span>
 
    ![Účet hello úložiště získáte přihlašovací údaje pro připojení.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. <span data-ttu-id="c3fb0-149">Spustit hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) nástroj, vyberte hello připojit ikonu na levé straně hello, zvolte **použít název účtu úložiště a klíč**a vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-149">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, select hello connect icon on hello left, choose **Use a storage account name and key**, and select **Next**.</span></span>

    ![Spusťte nástroj hello Průzkumníka účet úložiště.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. <span data-ttu-id="c3fb0-151">Vložení hello **název účtu** a **klíč účtu** z kroku 1 do jejich odpovídající pole a pak vyberte **Další**, a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-151">Paste hello **Account name** and **Account key** from step 1 into their corresponding fields, then select **Next**, and **Connect**.</span></span> 
  
    ![Vložte hello úložiště přihlašovací údaje a připojení.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. <span data-ttu-id="c3fb0-153">Rozbalte účet úložiště hello připojené, rozbalte položku **fronty** a ověřte, že frontu s názvem **Moje_fronta položky** existuje.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-153">Expand hello attached storage account, expand **Queues** and verify that a queue named **myqueue-items** exists.</span></span> <span data-ttu-id="c3fb0-154">Měli byste taky vidět zprávu již ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-154">You should also see a message already in hello queue.</span></span>  
 
    ![Vytvořte frontu úložiště.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a><span data-ttu-id="c3fb0-156">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="c3fb0-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="c3fb0-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3fb0-157">Next steps</span></span>

<span data-ttu-id="c3fb0-158">Jste přidali tooan vazby výstup pro existující funkce.</span><span class="sxs-lookup"><span data-stu-id="c3fb0-158">You have added an output binding tooan existing function.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="c3fb0-159">Další informace o vazby tooQueue úložiště najdete v tématu [vazby fronty Azure Storage funkce](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="c3fb0-159">For more information about binding tooQueue storage, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span> 



