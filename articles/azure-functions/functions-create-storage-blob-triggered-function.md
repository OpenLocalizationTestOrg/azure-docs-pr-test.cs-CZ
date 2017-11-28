---
title: "aaaCreate funkce v aktivovány úložiště objektů Blob v Azure | Microsoft Docs"
description: "Použití Azure Functions toocreate bez serveru funkce, která je volána, podle položek přidat tooAzure úložiště objektů Blob."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: acb7d29abb07a22da11d0e65d2ed54591f8e3f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a><span data-ttu-id="90cb1-103">Vytvoření funkce aktivované službou Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="90cb1-103">Create a function triggered by Azure Blob storage</span></span>

<span data-ttu-id="90cb1-104">Zjistěte, jak toocreate funkci aktivuje, když jsou soubory nahrát tooor aktualizovat v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="90cb1-104">Learn how toocreate a function triggered when files are uploaded tooor updated in Azure Blob storage.</span></span>

![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="90cb1-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90cb1-106">Prerequisites</span></span>

+ <span data-ttu-id="90cb1-107">Stáhněte a nainstalujte hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="90cb1-107">Download and install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
+ <span data-ttu-id="90cb1-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="90cb1-108">An Azure subscription.</span></span> <span data-ttu-id="90cb1-109">Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="90cb1-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="90cb1-110">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="90cb1-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="90cb1-112">Dál vytvořte funkci v nové funkce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="90cb1-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a><span data-ttu-id="90cb1-113">Vytvoření funkce aktivované službou Blob Storage</span><span class="sxs-lookup"><span data-stu-id="90cb1-113">Create a Blob storage triggered function</span></span>

1. <span data-ttu-id="90cb1-114">Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="90cb1-115">Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="90cb1-116">Zobrazí se hello kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="90cb1-116">This displays hello complete set of function templates.</span></span>

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. <span data-ttu-id="90cb1-118">Vyberte hello **BlobTrigger** šablonu pro požadovaný jazyk a použít hello nastavení uvedeného v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="90cb1-118">Select hello **BlobTrigger** template for your desired language, and use hello settings as specified in hello table.</span></span>

    ![Vytvoření funkce aktivované úložiště objektů Blob hello.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | <span data-ttu-id="90cb1-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="90cb1-120">Setting</span></span> | <span data-ttu-id="90cb1-121">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="90cb1-121">Suggested value</span></span> | <span data-ttu-id="90cb1-122">Popis</span><span class="sxs-lookup"><span data-stu-id="90cb1-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="90cb1-123">**Cesta**</span><span class="sxs-lookup"><span data-stu-id="90cb1-123">**Path**</span></span>   | <span data-ttu-id="90cb1-124">mycontainer/{name}</span><span class="sxs-lookup"><span data-stu-id="90cb1-124">mycontainer/{name}</span></span>    | <span data-ttu-id="90cb1-125">Monitorované umístění ve službě Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="90cb1-125">Location in Blob storage being monitored.</span></span> <span data-ttu-id="90cb1-126">Název souboru Hello objektu hello blob je předán hello vazbu jako hello _název_ parametr.</span><span class="sxs-lookup"><span data-stu-id="90cb1-126">hello file name of hello blob is passed in hello binding as hello _name_ parameter.</span></span>  |
    | <span data-ttu-id="90cb1-127">**Připojení k účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="90cb1-127">**Storage account connection**</span></span> | <span data-ttu-id="90cb1-128">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="90cb1-128">AzureWebJobStorage</span></span> | <span data-ttu-id="90cb1-129">Můžete použít připojení účtu úložiště hello již používá aplikace funkce nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="90cb1-129">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="90cb1-130">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="90cb1-130">**Name your function**</span></span> | <span data-ttu-id="90cb1-131">Jedinečný název v rámci aplikace Function App</span><span class="sxs-lookup"><span data-stu-id="90cb1-131">Unique in your function app</span></span> | <span data-ttu-id="90cb1-132">Název této funkce aktivované objektem blob.</span><span class="sxs-lookup"><span data-stu-id="90cb1-132">Name of this blob triggered function.</span></span> |

3. <span data-ttu-id="90cb1-133">Klikněte na tlačítko **vytvořit** toocreate funkce.</span><span class="sxs-lookup"><span data-stu-id="90cb1-133">Click **Create** toocreate your function.</span></span>

<span data-ttu-id="90cb1-134">V dalším kroku připojit tooyour účet úložiště Azure a vytvořit hello **můj_kontejner** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="90cb1-134">Next, you connect tooyour Azure Storage account and create hello **mycontainer** container.</span></span>

## <a name="create-hello-container"></a><span data-ttu-id="90cb1-135">Vytvoření kontejneru hello</span><span class="sxs-lookup"><span data-stu-id="90cb1-135">Create hello container</span></span>

1. <span data-ttu-id="90cb1-136">Ve funkci klikněte na **Integrace**, rozbalte položku **Dokumentace**a zkopírujte údaje **Název účtu** a **Klíč účtu**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-136">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="90cb1-137">Používáte účet úložiště toohello tooconnect tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="90cb1-137">You use these credentials tooconnect toohello storage account.</span></span> <span data-ttu-id="90cb1-138">Pokud už jste připojení účtu úložiště, přeskočte toostep 4.</span><span class="sxs-lookup"><span data-stu-id="90cb1-138">If you have already connected your storage account, skip toostep 4.</span></span>

    ![Účet hello úložiště získáte přihlašovací údaje pro připojení.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. <span data-ttu-id="90cb1-140">Spustit hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) nástroje, klikněte na tlačítko hello připojit ikonu na levé straně hello, zvolte **použít název účtu úložiště a klíč**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-140">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click hello connect icon on hello left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Spusťte nástroj hello Průzkumníka účet úložiště.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="90cb1-142">Zadejte hello **název účtu** a **klíč účtu** z kroku 1, klikněte na tlačítko **Další** a potom **Connect**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-142">Enter hello **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span> 

    ![Zadejte přihlašovací údaje storage hello a připojení.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="90cb1-144">Rozbalte účet úložiště hello připojit, klikněte pravým tlačítkem na **Blob kontejnery**, klikněte na tlačítko **vytvořit kontejner objektů blob**, typ `mycontainer`, a potom stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="90cb1-144">Expand hello attached storage account, right-click **Blob containers**, click **Create blob container**, type `mycontainer`, and then press enter.</span></span>

    ![Vytvořte frontu úložiště.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

<span data-ttu-id="90cb1-146">Teď, když máte kontejner objektů blob, můžete otestovat hello funkce tím, že nahrajete soubor toohello kontejner.</span><span class="sxs-lookup"><span data-stu-id="90cb1-146">Now that you have a blob container, you can test hello function by uploading a file toohello container.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="90cb1-147">Testování funkce hello</span><span class="sxs-lookup"><span data-stu-id="90cb1-147">Test hello function</span></span>

1. <span data-ttu-id="90cb1-148">Zpět v hello portálu Azure, rozbalte procházet tooyour funkce hello **protokoly** dolnímu hello hello stránky a ujistěte se, že není pozastavená této protokolů streamování.</span><span class="sxs-lookup"><span data-stu-id="90cb1-148">Back in hello Azure portal, browse tooyour function expand hello **Logs** at hello bottom of hello page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="90cb1-149">V Průzkumníku úložišť rozbalte účet úložiště, položku **Kontejnery objektů blob** a potom položku **mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-149">In Storage Explorer, expand your storage account, **Blob containers**, and **mycontainer**.</span></span> <span data-ttu-id="90cb1-150">Klikněte na **Odeslat** a potom na **Nahrát soubory…**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-150">Click **Upload** and then **Upload files...**.</span></span>

    ![Nahrajte soubor toohello kontejneru objektů blob.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. <span data-ttu-id="90cb1-152">V hello **nahrání souborů** dialogovém okně klikněte na hello **soubory** pole.</span><span class="sxs-lookup"><span data-stu-id="90cb1-152">In hello **Upload files** dialog box, click hello **Files** field.</span></span> <span data-ttu-id="90cb1-153">Vyhledejte soubor tooa v místním počítači, jako je soubor bitové kopie, vyberte ho a klikněte na tlačítko **otevřete** a potom **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="90cb1-153">Browse tooa file on your local computer, such as an image file, select it and click **Open** and then **Upload**.</span></span>

1. <span data-ttu-id="90cb1-154">Přejděte zpět tooyour funkce protokoly a ověřte, zda že byl načten tomuto objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="90cb1-154">Go back tooyour function logs and verify that hello blob has been read.</span></span>

   ![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > <span data-ttu-id="90cb1-156">Když vaše aplikace funkce běží v plánu spotřeby výchozí hello, mohou být zpoždění až tooseveral minut mezi hello objekt blob se přidán nebo aktualizován a hello funkce se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="90cb1-156">When your function app runs in hello default Consumption plan, there may be a delay of up tooseveral minutes between hello blob being added or updated and hello function being triggered.</span></span> <span data-ttu-id="90cb1-157">Pokud u funkcí aktivovaných objekty blob potřebujete nízkou latenci, zvažte spuštění aplikace Function App v rámci plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="90cb1-157">If you need low latency in your blob triggered functions, consider running your function app in an App Service plan.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="90cb1-158">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="90cb1-158">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="90cb1-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90cb1-159">Next steps</span></span>

<span data-ttu-id="90cb1-160">Vytvořili jste funkci, která se spustí v případě, že objekt blob je přidaný do úložiště objektů Blob tooor aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="90cb1-160">You have created a function that runs when a blob is added tooor updated in Blob storage.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="90cb1-161">Další informace o aktivačních událostech služby Blob Storage najdete v tématu [Vazby služby Azure Functions Blob Storage](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="90cb1-161">For more information about Blob storage triggers, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>
