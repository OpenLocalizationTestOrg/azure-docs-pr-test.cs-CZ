---
title: "aaaAdd hello úložiště objektů blob Azure konektor ve vašich Logic Apps | Microsoft Docs"
description: "Začínáme a nakonfigurovat konektor úložiště objektů blob v Azure hello v aplikaci logiky"
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
ms.openlocfilehash: add61287ef1b2228ef9d3f54ce082807bad6858b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-blob-storage-connector-in-a-logic-app"></a><span data-ttu-id="ddb48-103">Použití konektoru úložiště objektů blob v Azure hello v aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="ddb48-103">Use hello Azure blob storage connector in a logic app</span></span>
<span data-ttu-id="ddb48-104">Použití hello Azure Blob storage konektor tooupload, aktualizace, získání a odstranění objektů BLOB v účtu úložiště, vše v rámci aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ddb48-104">Use hello Azure Blob storage connector tooupload, update, get, and delete blobs in your storage account, all within a logic app.</span></span>  

<span data-ttu-id="ddb48-105">S úložištěm Azure blob můžete:</span><span class="sxs-lookup"><span data-stu-id="ddb48-105">With Azure blob storage, you:</span></span>

* <span data-ttu-id="ddb48-106">Vytvořte pracovní postup odesílání nových projektů nebo získávání souborů, které byly nedávno aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="ddb48-106">Build your workflow by uploading new projects, or getting files that have been recently updated.</span></span>
* <span data-ttu-id="ddb48-107">Pomocí akce tooget soubor metadat, odstraňte soubor, kopírovat soubory a další.</span><span class="sxs-lookup"><span data-stu-id="ddb48-107">Use actions tooget file metadata, delete a file, copy files, and more.</span></span> <span data-ttu-id="ddb48-108">Například při aktualizaci nástroj v Azure webový server (aktivační událost), aktualizujte soubor v úložišti objektů blob (akce).</span><span class="sxs-lookup"><span data-stu-id="ddb48-108">For example, when a tool is updated in an Azure web site (a trigger), then update a file in blob storage (an action).</span></span> 

<span data-ttu-id="ddb48-109">Toto téma ukazuje, jak toouse hello objektu blob úložiště konektor v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ddb48-109">This topic shows you how toouse hello blob storage connector in a logic app.</span></span>

<span data-ttu-id="ddb48-110">toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ddb48-110">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<span data-ttu-id="ddb48-111">toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ddb48-111">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooazure-blob-storage"></a><span data-ttu-id="ddb48-112">Připojit tooAzure úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="ddb48-112">Connect tooAzure blob storage</span></span>
<span data-ttu-id="ddb48-113">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="ddb48-113">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="ddb48-114">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="ddb48-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="ddb48-115">Například účet úložiště tooa tooconnect, nejprve vytvoříte úložiště objektů blob *připojení*.</span><span class="sxs-lookup"><span data-stu-id="ddb48-115">For example, tooconnect tooa storage account, you first create a blob storage *connection*.</span></span> <span data-ttu-id="ddb48-116">toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello služby, ke kterému se připojujete.</span><span class="sxs-lookup"><span data-stu-id="ddb48-116">toocreate a connection, enter hello credentials you normally use tooaccess hello service you are connecting to.</span></span> <span data-ttu-id="ddb48-117">Proto s Azure storage, zadejte přihlašovací údaje hello tooyour úložiště účet toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="ddb48-117">So with Azure storage, enter hello credentials tooyour storage account toocreate hello connection.</span></span> 

#### <a name="create-hello-connection"></a><span data-ttu-id="ddb48-118">Vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="ddb48-118">Create hello connection</span></span>
> [!INCLUDE [Create a connection tooAzure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a><span data-ttu-id="ddb48-119">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="ddb48-119">Use a trigger</span></span>
<span data-ttu-id="ddb48-120">Tento konektor nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ddb48-120">This connector does not have any triggers.</span></span> <span data-ttu-id="ddb48-121">Použijte jiné aktivační události toostart hello aplikace logiky, jako jsou aktivační událost opakování, aktivační události Webhooku protokolu HTTP, aktivační události, které jsou k dispozici ostatní konektory a další.</span><span class="sxs-lookup"><span data-stu-id="ddb48-121">Use other triggers toostart hello logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="ddb48-122">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) poskytuje příklad.</span><span class="sxs-lookup"><span data-stu-id="ddb48-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="ddb48-123">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="ddb48-123">Use an action</span></span>
<span data-ttu-id="ddb48-124">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ddb48-124">An action is an operation carried out by hello workflow defined in a logic app.</span></span>

1. <span data-ttu-id="ddb48-125">Vyberte znaménko plus hello.</span><span class="sxs-lookup"><span data-stu-id="ddb48-125">Select hello plus sign.</span></span> <span data-ttu-id="ddb48-126">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="ddb48-126">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. <span data-ttu-id="ddb48-127">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="ddb48-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="ddb48-128">Hello textového pole zadejte "blob" tooget seznam všechny dostupné akce hello.</span><span class="sxs-lookup"><span data-stu-id="ddb48-128">In hello text box, type “blob” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. <span data-ttu-id="ddb48-129">V našem příkladu zvolte **AzureBlob - Get metadata souboru pomocí cesty**.</span><span class="sxs-lookup"><span data-stu-id="ddb48-129">In our example, choose **AzureBlob - Get file metadata using path**.</span></span> <span data-ttu-id="ddb48-130">Pokud připojení již existuje, pak vyberte hello **...** Tlačítko tooselect (Zobrazit výběr) soubor.</span><span class="sxs-lookup"><span data-stu-id="ddb48-130">If a connection already exists, then select hello **...** (Show Picker) button tooselect a file.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    <span data-ttu-id="ddb48-131">Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="ddb48-131">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="ddb48-132">[Vytvoření připojení hello](connectors-create-api-azureblobstorage.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ddb48-132">[Create hello connection](connectors-create-api-azureblobstorage.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ddb48-133">V tomto příkladu se nám získat hello metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="ddb48-133">In this example, we get hello metadata of a file.</span></span> <span data-ttu-id="ddb48-134">toosee hello metadata, přidejte další akci, která vytvoří nový soubor pomocí jiný konektor.</span><span class="sxs-lookup"><span data-stu-id="ddb48-134">toosee hello metadata, add another action that creates a new file using another connector.</span></span> <span data-ttu-id="ddb48-135">Například přidejte OneDrive akci, která vytvoří nový soubor "test" na základě metadat hello.</span><span class="sxs-lookup"><span data-stu-id="ddb48-135">For example, add a OneDrive action that creates a new "test" file based on hello metadata.</span></span> 


5. <span data-ttu-id="ddb48-136">**Uložit** změny (levém horním rohu hello panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="ddb48-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="ddb48-137">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="ddb48-137">Your logic app is saved and may be automatically enabled.</span></span>

> [!TIP]
> <span data-ttu-id="ddb48-138">[Storage Explorer](http://storageexplorer.com/) je skvělý nástroj příliš spravovat více účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="ddb48-138">[Storage Explorer](http://storageexplorer.com/) is a great tool too manage multiple storage accounts.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="ddb48-139">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="ddb48-139">Connector-specific details</span></span>

<span data-ttu-id="ddb48-140">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/azureblobconnector/).</span><span class="sxs-lookup"><span data-stu-id="ddb48-140">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/azureblobconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ddb48-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddb48-141">Next steps</span></span>
<span data-ttu-id="ddb48-142">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ddb48-142">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="ddb48-143">Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="ddb48-143">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

