---
title: "Vytvoření funkce aktivované zprávami ve frontě v Azure | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, kterou vyvolávají zprávy odeslané do fronty služby Azure Storage."
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
ms.openlocfilehash: 92a03154bf5a8945e2de9606afd138803c76fafe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a><span data-ttu-id="61271-103">Vytvoření funkce aktivované službou Azure Queue Storage</span><span class="sxs-lookup"><span data-stu-id="61271-103">Create a function triggered by Azure Queue storage</span></span>

<span data-ttu-id="61271-104">Zjistíte, jak vytvořit funkci, která se aktivuje při odeslání zpráv do fronty služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61271-104">Learn how to create a function triggered when messages are submitted to an Azure Storage queue.</span></span>

![Zobrazte si zprávy v protokolech.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="61271-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61271-106">Prerequisites</span></span>

- <span data-ttu-id="61271-107">Stáhnout a nainstalovat [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="61271-107">Download and install the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

- <span data-ttu-id="61271-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="61271-108">An Azure subscription.</span></span> <span data-ttu-id="61271-109">Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="61271-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="61271-110">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="61271-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="61271-112">Dál vytvoříte v nové aplikaci Function App funkci.</span><span class="sxs-lookup"><span data-stu-id="61271-112">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a><span data-ttu-id="61271-113">Vytvoření funkce aktivované frontou</span><span class="sxs-lookup"><span data-stu-id="61271-113">Create a Queue triggered function</span></span>

1. <span data-ttu-id="61271-114">Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**.</span><span class="sxs-lookup"><span data-stu-id="61271-114">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="61271-115">Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="61271-115">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="61271-116">Zobrazí se kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="61271-116">This displays the complete set of function templates.</span></span>

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. <span data-ttu-id="61271-118">Vyberte šablonu **QueueTrigger** pro požadovaný jazyk a potom použijte nastavení uvedené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="61271-118">Select the **QueueTrigger** template for your desired language, and  use the settings as specified in the table.</span></span>

    ![Vytvořte funkci spouštěnou frontou úložiště.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | <span data-ttu-id="61271-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="61271-120">Setting</span></span> | <span data-ttu-id="61271-121">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="61271-121">Suggested value</span></span> | <span data-ttu-id="61271-122">Popis</span><span class="sxs-lookup"><span data-stu-id="61271-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="61271-123">**Název fronty**</span><span class="sxs-lookup"><span data-stu-id="61271-123">**Queue name**</span></span>   | <span data-ttu-id="61271-124">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="61271-124">myqueue-items</span></span>    | <span data-ttu-id="61271-125">Název fronty, ke které se připojíte ve svém účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61271-125">Name of the queue to connect to in your Storage account.</span></span> |
    | <span data-ttu-id="61271-126">**Připojení k účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="61271-126">**Storage account connection**</span></span> | <span data-ttu-id="61271-127">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="61271-127">AzureWebJobStorage</span></span> | <span data-ttu-id="61271-128">Můžete použít připojení k účtu úložiště, které už používá vaše aplikace Function App, nebo můžete vytvořit nové.</span><span class="sxs-lookup"><span data-stu-id="61271-128">You can use the storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="61271-129">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="61271-129">**Name your function**</span></span> | <span data-ttu-id="61271-130">Jedinečný název v rámci aplikace Function App</span><span class="sxs-lookup"><span data-stu-id="61271-130">Unique in your function app</span></span> | <span data-ttu-id="61271-131">Název této funkce aktivované frontou.</span><span class="sxs-lookup"><span data-stu-id="61271-131">Name of this queue triggered function.</span></span> |

3. <span data-ttu-id="61271-132">Funkci vytvoříte kliknutím na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="61271-132">Click **Create** to create your function.</span></span>

<span data-ttu-id="61271-133">Teď se připojíte ke svému účtu služby Azure Storage a vytvoříte frontu úložiště **myqueue-items**.</span><span class="sxs-lookup"><span data-stu-id="61271-133">Next, you connect to your Azure Storage account and create the **myqueue-items** storage queue.</span></span>

## <a name="create-the-queue"></a><span data-ttu-id="61271-134">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="61271-134">Create the queue</span></span>

1. <span data-ttu-id="61271-135">Ve funkci klikněte na **Integrace**, rozbalte položku **Dokumentace**a zkopírujte údaje **Název účtu** a **Klíč účtu**.</span><span class="sxs-lookup"><span data-stu-id="61271-135">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="61271-136">Tyto přihlašovací údaje použijte k připojení k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="61271-136">You use these credentials to connect to the storage account.</span></span> <span data-ttu-id="61271-137">Pokud jste se už ke svému účtu úložiště připojili, přejděte ke kroku 4.</span><span class="sxs-lookup"><span data-stu-id="61271-137">If you have already connected your storage account, skip to step 4.</span></span>

    ![Získejte přihlašovací údaje účtu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)<span data-ttu-id="61271-139">v</span><span class="sxs-lookup"><span data-stu-id="61271-139">v</span></span>

1. <span data-ttu-id="61271-140">Spusťte nástroj [Microsoft Azure Storage Explorer](http://storageexplorer.com/), vlevo klikněte na ikonu připojení, zvolte **Use a storage account name and key** (Použít název a klíč účtu úložiště) a klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="61271-140">Run the [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click the connect icon on the left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Spusťte nástroj Průzkumník účtu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="61271-142">Zadejte **Název účtu** a **Klíč účtu** z kroku 1, klikněte na **Další** a potom klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="61271-142">Enter the **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span>

    ![Zadejte přihlašovací údaje úložiště a připojte se.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="61271-144">Rozbalte připojený účet úložiště, klikněte pravým tlačítkem na **Fronty**, klikněte na **Vytvořit frontu**, zadejte`myqueue-items` a potom stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="61271-144">Expand the attached storage account, right-click **Queues**, click **Create queue**, type `myqueue-items`, and then press enter.</span></span>

    ![Vytvořte frontu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

<span data-ttu-id="61271-146">Teď máte frontu úložiště a můžete funkci otestovat přidáním zprávy do fronty.</span><span class="sxs-lookup"><span data-stu-id="61271-146">Now that you have a storage queue, you can test the function by adding a message to the queue.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="61271-147">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="61271-147">Test the function</span></span>

1. <span data-ttu-id="61271-148">Zpátky na webu Azure Portal přejděte na svoji funkci, ve spodní části stránky rozbalte **Protokoly** a ujistěte se, že není pozastavené streamování protokolů.</span><span class="sxs-lookup"><span data-stu-id="61271-148">Back in the Azure portal, browse to your function expand the **Logs** at the bottom of the page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="61271-149">V Storage Exploreru rozbalte svůj účet úložiště, možnosti **Queues** (Fornty), a **myqueue-items** a potom klikněte na **Add message** (Přidat zprávu).</span><span class="sxs-lookup"><span data-stu-id="61271-149">In Storage Explorer, expand your storage account, **Queues**, and **myqueue-items**, then click **Add message**.</span></span>

    ![Přidejte do fronty zprávu.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. <span data-ttu-id="61271-151">Zadejte zprávu „Hello World!“</span><span class="sxs-lookup"><span data-stu-id="61271-151">Type your "Hello World!"</span></span> <span data-ttu-id="61271-152">do pole **Text zprávy** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="61271-152">message in **Message text** and click **OK**.</span></span>

1. <span data-ttu-id="61271-153">Několik sekund počkejte, potom se vraťte k protokolům funkce a zkontrolujte, jestli se nová zpráva z fronty přečetla.</span><span class="sxs-lookup"><span data-stu-id="61271-153">Wait for a few seconds, then go back to your function logs and verify that the new message has been read from the queue.</span></span>

    ![Zobrazte si zprávy v protokolech.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. <span data-ttu-id="61271-155">Zpátky v Storage Exploreru klikněte na **Refresh** (Aktualizovat) a zkontrolujte, jestli proběhlo zpracování zprávy a jestli zpráva zmizela z fronty.</span><span class="sxs-lookup"><span data-stu-id="61271-155">Back in Storage Explorer, click **Refresh** and verify that the message has been processed and is no longer in the queue.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="61271-156">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="61271-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="61271-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61271-157">Next steps</span></span>

<span data-ttu-id="61271-158">Vytvořili jste funkci, která se spustí při přidání zprávy do fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="61271-158">You have created a function that runs when a message is added to a storage queue.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="61271-159">Další informace o aktivačních událostech fronty úložiště najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="61271-159">For more information about Queue storage triggers, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span>