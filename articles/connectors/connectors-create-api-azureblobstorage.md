---
title: "Přidejte konektor Azure blob storage ve vašich Logic Apps | Microsoft Docs"
description: "Začínáme a nakonfigurovat konektor úložiště objektů blob v Azure v aplikaci logiky"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: bc7908868828bd1628633cf9e57f8c44f8000827
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a><span data-ttu-id="82eeb-103">Použití konektoru úložiště objektů blob v Azure v aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="82eeb-103">Use the Azure blob storage connector in a logic app</span></span>
<span data-ttu-id="82eeb-104">Pomocí konektoru úložiště objektů Blob v Azure nahrát, aktualizace, získání a odstranění objektů BLOB v účtu úložiště, vše v rámci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="82eeb-104">Use the Azure Blob storage connector to upload, update, get, and delete blobs in your storage account, all within a logic app.</span></span>  

<span data-ttu-id="82eeb-105">S úložištěm Azure blob můžete:</span><span class="sxs-lookup"><span data-stu-id="82eeb-105">With Azure blob storage, you:</span></span>

* <span data-ttu-id="82eeb-106">Vytvořte pracovní postup odesílání nových projektů nebo získávání souborů, které byly nedávno aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="82eeb-106">Build your workflow by uploading new projects, or getting files that have been recently updated.</span></span>
* <span data-ttu-id="82eeb-107">Pomocí akcí můžete získat metadata souboru, odstraňte soubor, kopírovat soubory a další.</span><span class="sxs-lookup"><span data-stu-id="82eeb-107">Use actions to get file metadata, delete a file, copy files, and more.</span></span> <span data-ttu-id="82eeb-108">Například při aktualizaci nástroj v Azure webový server (aktivační událost), aktualizujte soubor v úložišti objektů blob (akce).</span><span class="sxs-lookup"><span data-stu-id="82eeb-108">For example, when a tool is updated in an Azure web site (a trigger), then update a file in blob storage (an action).</span></span> 

<span data-ttu-id="82eeb-109">Toto téma ukazuje, jak používat konektor úložiště objektů blob v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="82eeb-109">This topic shows you how to use the blob storage connector in a logic app.</span></span>

<span data-ttu-id="82eeb-110">Další informace o Logic Apps najdete v tématu [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="82eeb-110">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<span data-ttu-id="82eeb-111">Další informace o Logic Apps najdete v tématu [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="82eeb-111">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-azure-blob-storage"></a><span data-ttu-id="82eeb-112">Připojení do Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="82eeb-112">Connect to Azure blob storage</span></span>
<span data-ttu-id="82eeb-113">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="82eeb-113">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="82eeb-114">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="82eeb-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="82eeb-115">Například pokud chcete připojit k účtu úložiště, nejprve vytvoříte úložiště objektů blob *připojení*.</span><span class="sxs-lookup"><span data-stu-id="82eeb-115">For example, to connect to a storage account, you first create a blob storage *connection*.</span></span> <span data-ttu-id="82eeb-116">Chcete-li vytvořit připojení, zadejte přihlašovací údaje, které standardně používáte k přístupu ke službě, ke kterému se připojujete.</span><span class="sxs-lookup"><span data-stu-id="82eeb-116">To create a connection, enter the credentials you normally use to access the service you are connecting to.</span></span> <span data-ttu-id="82eeb-117">Proto s Azure storage, zadejte přihlašovací údaje k účtu úložiště k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="82eeb-117">So with Azure storage, enter the credentials to your storage account to create the connection.</span></span> 

#### <a name="create-the-connection"></a><span data-ttu-id="82eeb-118">Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="82eeb-118">Create the connection</span></span>
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a><span data-ttu-id="82eeb-119">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="82eeb-119">Use a trigger</span></span>
<span data-ttu-id="82eeb-120">Tento konektor nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="82eeb-120">This connector does not have any triggers.</span></span> <span data-ttu-id="82eeb-121">Spusťte aplikaci logiky, jako jsou aktivační událost opakování, aktivační události Webhooku protokolu HTTP, aktivační události, které jsou k dispozici ostatní konektory a další pomocí jiných aktivační události.</span><span class="sxs-lookup"><span data-stu-id="82eeb-121">Use other triggers to start the logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="82eeb-122">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) poskytuje příklad.</span><span class="sxs-lookup"><span data-stu-id="82eeb-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="82eeb-123">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="82eeb-123">Use an action</span></span>
<span data-ttu-id="82eeb-124">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="82eeb-124">An action is an operation carried out by the workflow defined in a logic app.</span></span>

1. <span data-ttu-id="82eeb-125">Vyberte znaménko plus.</span><span class="sxs-lookup"><span data-stu-id="82eeb-125">Select the plus sign.</span></span> <span data-ttu-id="82eeb-126">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="82eeb-126">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. <span data-ttu-id="82eeb-127">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="82eeb-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="82eeb-128">Do textového pole zadejte "blob" získat seznam všechny dostupné akce.</span><span class="sxs-lookup"><span data-stu-id="82eeb-128">In the text box, type “blob” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. <span data-ttu-id="82eeb-129">V našem příkladu zvolte **AzureBlob - Get metadata souboru pomocí cesty**.</span><span class="sxs-lookup"><span data-stu-id="82eeb-129">In our example, choose **AzureBlob - Get file metadata using path**.</span></span> <span data-ttu-id="82eeb-130">Pokud připojení již existuje, pak vyberte **...** Tlačítko (Zobrazit výběr) a vyberte soubor.</span><span class="sxs-lookup"><span data-stu-id="82eeb-130">If a connection already exists, then select the **...** (Show Picker) button to select a file.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    <span data-ttu-id="82eeb-131">Pokud se zobrazí výzva pro informace o připojení, zadejte podrobnosti k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="82eeb-131">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="82eeb-132">[Vytvoření připojení](connectors-create-api-azureblobstorage.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="82eeb-132">[Create the connection](connectors-create-api-azureblobstorage.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="82eeb-133">V tomto příkladu se nám získat metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="82eeb-133">In this example, we get the metadata of a file.</span></span> <span data-ttu-id="82eeb-134">Pokud chcete zobrazit metadata, přidejte akci, která se vytvoří nový soubor pomocí jiný konektor.</span><span class="sxs-lookup"><span data-stu-id="82eeb-134">To see the metadata, add another action that creates a new file using another connector.</span></span> <span data-ttu-id="82eeb-135">Například přidejte OneDrive akci, která vytvoří nový soubor "test" založené na metadatech.</span><span class="sxs-lookup"><span data-stu-id="82eeb-135">For example, add a OneDrive action that creates a new "test" file based on the metadata.</span></span> 


5. <span data-ttu-id="82eeb-136">**Uložit** změny (levém horním rohu panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="82eeb-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="82eeb-137">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="82eeb-137">Your logic app is saved and may be automatically enabled.</span></span>

> [!TIP]
> <span data-ttu-id="82eeb-138">[Storage Explorer](http://storageexplorer.com/) je skvělý nástroj pro správu více účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="82eeb-138">[Storage Explorer](http://storageexplorer.com/) is a great tool to  manage multiple storage accounts.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="82eeb-139">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="82eeb-139">Connector-specific details</span></span>

<span data-ttu-id="82eeb-140">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/azureblobconnector/).</span><span class="sxs-lookup"><span data-stu-id="82eeb-140">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/azureblobconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="82eeb-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82eeb-141">Next steps</span></span>
<span data-ttu-id="82eeb-142">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="82eeb-142">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="82eeb-143">Prozkoumejte dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="82eeb-143">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

