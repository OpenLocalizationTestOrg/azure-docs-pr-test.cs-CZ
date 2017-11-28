---
title: "aaaCreate funkce v Azure aktivovány zprávy fronty | Microsoft Docs"
description: "Použití Azure Functions toocreate bez serveru funkce, které je vyvolána zprávy odeslané tooan fronty Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e9501ed336b502eaeee3fa62ec4ae085c76de0ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a><span data-ttu-id="afb5b-103">Vytvoření funkce aktivované službou Azure Queue Storage</span><span class="sxs-lookup"><span data-stu-id="afb5b-103">Create a function triggered by Azure Queue storage</span></span>

<span data-ttu-id="afb5b-104">Zjistěte, jak toocreate funkci aktivuje, když se zprávy odeslané tooan fronty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="afb5b-104">Learn how toocreate a function triggered when messages are submitted tooan Azure Storage queue.</span></span>

![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="afb5b-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="afb5b-106">Prerequisites</span></span>

- <span data-ttu-id="afb5b-107">Stáhněte a nainstalujte hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="afb5b-107">Download and install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

- <span data-ttu-id="afb5b-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="afb5b-108">An Azure subscription.</span></span> <span data-ttu-id="afb5b-109">Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="afb5b-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="afb5b-110">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="afb5b-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="afb5b-112">Dál vytvořte funkci v nové funkce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="afb5b-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a><span data-ttu-id="afb5b-113">Vytvoření funkce aktivované frontou</span><span class="sxs-lookup"><span data-stu-id="afb5b-113">Create a Queue triggered function</span></span>

1. <span data-ttu-id="afb5b-114">Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**.</span><span class="sxs-lookup"><span data-stu-id="afb5b-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="afb5b-115">Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="afb5b-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="afb5b-116">Zobrazí se hello kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="afb5b-116">This displays hello complete set of function templates.</span></span>

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. <span data-ttu-id="afb5b-118">Vyberte hello **QueueTrigger** šablonu pro požadovaný jazyk a použít hello nastavení uvedeného v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="afb5b-118">Select hello **QueueTrigger** template for your desired language, and  use hello settings as specified in hello table.</span></span>

    ![Vytvoření funkce aktivované fronty úložiště hello.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | <span data-ttu-id="afb5b-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="afb5b-120">Setting</span></span> | <span data-ttu-id="afb5b-121">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="afb5b-121">Suggested value</span></span> | <span data-ttu-id="afb5b-122">Popis</span><span class="sxs-lookup"><span data-stu-id="afb5b-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="afb5b-123">**Název fronty**</span><span class="sxs-lookup"><span data-stu-id="afb5b-123">**Queue name**</span></span>   | <span data-ttu-id="afb5b-124">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="afb5b-124">myqueue-items</span></span>    | <span data-ttu-id="afb5b-125">Název hello fronty tooconnect tooin účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="afb5b-125">Name of hello queue tooconnect tooin your Storage account.</span></span> |
    | <span data-ttu-id="afb5b-126">**Připojení k účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="afb5b-126">**Storage account connection**</span></span> | <span data-ttu-id="afb5b-127">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="afb5b-127">AzureWebJobStorage</span></span> | <span data-ttu-id="afb5b-128">Můžete použít připojení účtu úložiště hello již používá aplikace funkce nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="afb5b-128">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="afb5b-129">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="afb5b-129">**Name your function**</span></span> | <span data-ttu-id="afb5b-130">Jedinečný název v rámci aplikace Function App</span><span class="sxs-lookup"><span data-stu-id="afb5b-130">Unique in your function app</span></span> | <span data-ttu-id="afb5b-131">Název této funkce aktivované frontou.</span><span class="sxs-lookup"><span data-stu-id="afb5b-131">Name of this queue triggered function.</span></span> |

3. <span data-ttu-id="afb5b-132">Klikněte na tlačítko **vytvořit** toocreate funkce.</span><span class="sxs-lookup"><span data-stu-id="afb5b-132">Click **Create** toocreate your function.</span></span>

<span data-ttu-id="afb5b-133">V dalším kroku připojit tooyour účet úložiště Azure a vytvořit hello **Moje_fronta položky** fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="afb5b-133">Next, you connect tooyour Azure Storage account and create hello **myqueue-items** storage queue.</span></span>

## <a name="create-hello-queue"></a><span data-ttu-id="afb5b-134">Vytvořit frontu hello</span><span class="sxs-lookup"><span data-stu-id="afb5b-134">Create hello queue</span></span>

1. <span data-ttu-id="afb5b-135">Ve funkci klikněte na **Integrace**, rozbalte položku **Dokumentace**a zkopírujte údaje **Název účtu** a **Klíč účtu**.</span><span class="sxs-lookup"><span data-stu-id="afb5b-135">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="afb5b-136">Používáte účet úložiště toohello tooconnect tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="afb5b-136">You use these credentials tooconnect toohello storage account.</span></span> <span data-ttu-id="afb5b-137">Pokud už jste připojení účtu úložiště, přeskočte toostep 4.</span><span class="sxs-lookup"><span data-stu-id="afb5b-137">If you have already connected your storage account, skip toostep 4.</span></span>

    ![Účet hello úložiště získáte přihlašovací údaje pro připojení.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)<span data-ttu-id="afb5b-139">v</span><span class="sxs-lookup"><span data-stu-id="afb5b-139">v</span></span>

1. <span data-ttu-id="afb5b-140">Spustit hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) nástroje, klikněte na tlačítko hello připojit ikonu na levé straně hello, zvolte **použít název účtu úložiště a klíč**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="afb5b-140">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click hello connect icon on hello left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Spusťte nástroj hello Průzkumníka účet úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="afb5b-142">Zadejte hello **název účtu** a **klíč účtu** z kroku 1, klikněte na tlačítko **Další** a potom **Connect**.</span><span class="sxs-lookup"><span data-stu-id="afb5b-142">Enter hello **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span>

    ![Zadejte přihlašovací údaje storage hello a připojení.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="afb5b-144">Rozbalte účet úložiště hello připojit, klikněte pravým tlačítkem na **fronty**, klikněte na tlačítko **fronty vytvořit**, typ `myqueue-items`, a potom stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="afb5b-144">Expand hello attached storage account, right-click **Queues**, click **Create queue**, type `myqueue-items`, and then press enter.</span></span>

    ![Vytvořte frontu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

<span data-ttu-id="afb5b-146">Teď, když máte fronty úložiště, můžete otestovat hello funkce přidáním toohello front zpráv.</span><span class="sxs-lookup"><span data-stu-id="afb5b-146">Now that you have a storage queue, you can test hello function by adding a message toohello queue.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="afb5b-147">Testování funkce hello</span><span class="sxs-lookup"><span data-stu-id="afb5b-147">Test hello function</span></span>

1. <span data-ttu-id="afb5b-148">Zpět v hello portálu Azure, rozbalte procházet tooyour funkce hello **protokoly** dolnímu hello hello stránky a ujistěte se, že není pozastavená této protokolů streamování.</span><span class="sxs-lookup"><span data-stu-id="afb5b-148">Back in hello Azure portal, browse tooyour function expand hello **Logs** at hello bottom of hello page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="afb5b-149">V Storage Exploreru rozbalte svůj účet úložiště, možnosti **Queues** (Fornty), a **myqueue-items** a potom klikněte na **Add message** (Přidat zprávu).</span><span class="sxs-lookup"><span data-stu-id="afb5b-149">In Storage Explorer, expand your storage account, **Queues**, and **myqueue-items**, then click **Add message**.</span></span>

    ![Přidejte toohello front zpráv.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. <span data-ttu-id="afb5b-151">Zadejte zprávu „Hello World!“</span><span class="sxs-lookup"><span data-stu-id="afb5b-151">Type your "Hello World!"</span></span> <span data-ttu-id="afb5b-152">do pole **Text zprávy** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="afb5b-152">message in **Message text** and click **OK**.</span></span>

1. <span data-ttu-id="afb5b-153">Počkejte několik sekund poté přejděte zpět tooyour funkce protokoly a ověřte, že tuto novou zprávu hello byl načten z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="afb5b-153">Wait for a few seconds, then go back tooyour function logs and verify that hello new message has been read from hello queue.</span></span>

    ![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. <span data-ttu-id="afb5b-155">Zpět v Storage Explorer, klikněte na **aktualizovat** a ověřte, že uvítací zprávu zpracovává a již není ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="afb5b-155">Back in Storage Explorer, click **Refresh** and verify that hello message has been processed and is no longer in hello queue.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="afb5b-156">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="afb5b-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="afb5b-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="afb5b-157">Next steps</span></span>

<span data-ttu-id="afb5b-158">Vytvořili jste funkci, která spustí při přidání fronty úložiště tooa zprávu.</span><span class="sxs-lookup"><span data-stu-id="afb5b-158">You have created a function that runs when a message is added tooa storage queue.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="afb5b-159">Další informace o aktivačních událostech fronty úložiště najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="afb5b-159">For more information about Queue storage triggers, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span>