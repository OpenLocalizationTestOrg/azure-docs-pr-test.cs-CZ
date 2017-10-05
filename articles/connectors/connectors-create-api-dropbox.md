---
title: Konektor dropbox v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojení k Dropboxu ke správě souborů. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v Dropbox."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0d09580c60fd620811b539147439d0922839fe7e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-dropbox-connector"></a><span data-ttu-id="1fb99-105">Začínáme s konektorem Dropbox</span><span class="sxs-lookup"><span data-stu-id="1fb99-105">Get started with the Dropbox connector</span></span>
<span data-ttu-id="1fb99-106">Připojení k Dropboxu ke správě souborů.</span><span class="sxs-lookup"><span data-stu-id="1fb99-106">Connect to Dropbox to manage your files.</span></span> <span data-ttu-id="1fb99-107">Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v Dropbox.</span><span class="sxs-lookup"><span data-stu-id="1fb99-107">You can perform various actions such as upload, update, get, and delete files in Dropbox.</span></span>

<span data-ttu-id="1fb99-108">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1fb99-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="1fb99-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1fb99-109">You can get started by [creating a Logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-dropbox"></a><span data-ttu-id="1fb99-110">Připojení k Dropboxu</span><span class="sxs-lookup"><span data-stu-id="1fb99-110">Connect to Dropbox</span></span>
<span data-ttu-id="1fb99-111">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="1fb99-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="1fb99-112">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="1fb99-112">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="1fb99-113">Například, aby bylo možné připojit k Dropbox, musíte nejdřív Dropbox *připojení*.</span><span class="sxs-lookup"><span data-stu-id="1fb99-113">For example, in order to connect to Dropbox, you first need a Dropbox *connection*.</span></span> <span data-ttu-id="1fb99-114">Vytvoření připojení, musíte zadat přihlašovací údaje, které standardně používáte k přístupu ke službě, který chcete připojit k.</span><span class="sxs-lookup"><span data-stu-id="1fb99-114">To create a connection, you would need to provide the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="1fb99-115">Ano v příkladu Dropbox, potřebovali byste přihlašovací údaje ke svému účtu Dropboxu k vytvoření připojení k Dropboxu.</span><span class="sxs-lookup"><span data-stu-id="1fb99-115">So, in the Dropbox example, you would need the credentials to your Dropbox account in order to create the connection to Dropbox.</span></span> [<span data-ttu-id="1fb99-116">Další informace o připojení</span><span class="sxs-lookup"><span data-stu-id="1fb99-116">Learn more about connections</span></span>]()

### <a name="create-a-connection-to-dropbox"></a><span data-ttu-id="1fb99-117">Umožňuje vytvořit připojení k Dropboxu</span><span class="sxs-lookup"><span data-stu-id="1fb99-117">Create a connection to Dropbox</span></span>
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a><span data-ttu-id="1fb99-118">Použít aktivační událost Dropbox</span><span class="sxs-lookup"><span data-stu-id="1fb99-118">Use a Dropbox trigger</span></span>
<span data-ttu-id="1fb99-119">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1fb99-119">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="1fb99-120">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="1fb99-120">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="1fb99-121">V tomto příkladu použijeme **vytvoření souboru** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="1fb99-121">In this example, we will use the **When a file is created** trigger.</span></span> <span data-ttu-id="1fb99-122">Když dojde k této aktivační události, můžeme vám zavoláme **získat obsah souboru pomocí cesty** Dropbox akce.</span><span class="sxs-lookup"><span data-stu-id="1fb99-122">When this trigger occurs, we will call the **Get file content using path** Dropbox action.</span></span> 

1. <span data-ttu-id="1fb99-123">Zadejte *dropbox* do vyhledávacího pole v designeru Logic Apps, pak vyberte **Dropboxu - Pokud dojde k vytvoření souboru** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="1fb99-123">Enter *dropbox* in the search box on the Logic Apps designer, then select the **Dropbox - When a file is created** trigger.</span></span>      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. <span data-ttu-id="1fb99-124">Vyberte složku, ve kterém chcete sledovat vytváření souborů.</span><span class="sxs-lookup"><span data-stu-id="1fb99-124">Select the folder in which you want to track file creation.</span></span> <span data-ttu-id="1fb99-125">Vyberte... (označená červeným rámečkem) a přejděte do složky, kterou chcete vybrat pro vstup aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="1fb99-125">Select ... (identified in the red box) and browse to the folder you wish to select for the trigger's input.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a><span data-ttu-id="1fb99-126">Pomocí akce Dropbox</span><span class="sxs-lookup"><span data-stu-id="1fb99-126">Use a Dropbox action</span></span>
<span data-ttu-id="1fb99-127">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="1fb99-127">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="1fb99-128">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="1fb99-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="1fb99-129">Teď, když byla přidána aktivační událost, použijte následující postup přidání akce, která bude získat nový soubor obsahu.</span><span class="sxs-lookup"><span data-stu-id="1fb99-129">Now that the trigger has been added, follow these steps to add an action that will get the new file's content.</span></span>

1. <span data-ttu-id="1fb99-130">Vyberte **+ nový krok** přidat akci chcete provést, když je vytvořen nový soubor.</span><span class="sxs-lookup"><span data-stu-id="1fb99-130">Select **+ New Step** to add the action you would like to take when a new file is created.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. <span data-ttu-id="1fb99-131">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="1fb99-131">Select **Add an action**.</span></span> <span data-ttu-id="1fb99-132">Otevře se tato, které chcete do vyhledávacího pole, kde můžete vyhledat všechny akce můžete provést.</span><span class="sxs-lookup"><span data-stu-id="1fb99-132">This opens the search box where you can search for any action you would like to take.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. <span data-ttu-id="1fb99-133">Zadejte *dropbox* k vyhledání akcí souvisejících s Dropbox.</span><span class="sxs-lookup"><span data-stu-id="1fb99-133">Enter *dropbox* to search for actions related to Dropbox.</span></span>  
4. <span data-ttu-id="1fb99-134">Vyberte **Dropbox - Get obsah souboru pomocí cesty** jako pro případ, kdy je vytvořen nový soubor ve vybrané složce Dropbox.</span><span class="sxs-lookup"><span data-stu-id="1fb99-134">Select **Dropbox - Get file content using path** as the action to take when a new file is created in the selected Dropbox folder.</span></span> <span data-ttu-id="1fb99-135">Otevře se řídicí blok akce.</span><span class="sxs-lookup"><span data-stu-id="1fb99-135">The action control block opens.</span></span> <span data-ttu-id="1fb99-136">Zobrazí se výzva k autorizaci aplikace logiky pro přístup k účtu Dropbox, pokud jste tak dosud neučinili dříve.</span><span class="sxs-lookup"><span data-stu-id="1fb99-136">You will be prompted to authorize your logic app to access your Dropbox account if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. <span data-ttu-id="1fb99-137">Vyberte... (nachází na pravé straně **cesta k souboru** ovládací prvek) a přejděte do cesty k souboru se má použít.</span><span class="sxs-lookup"><span data-stu-id="1fb99-137">Select ... (located at the right side of the **File Path** control) and browse to the file path you would like to use.</span></span> <span data-ttu-id="1fb99-138">Nebo použijte **cesta k souboru** tokenu urychlit vytváření vašeho logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fb99-138">Or, use the **file path** token to speed up your logic app creation.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. <span data-ttu-id="1fb99-139">Uložte si práci a vytvořte nový soubor v Dropbox aktivovat pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="1fb99-139">Save your work and create a new file in Dropbox to activate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="1fb99-140">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="1fb99-140">Connector-specific details</span></span>

<span data-ttu-id="1fb99-141">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/dropbox/).</span><span class="sxs-lookup"><span data-stu-id="1fb99-141">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/dropbox/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="1fb99-142">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="1fb99-142">More connectors</span></span>
<span data-ttu-id="1fb99-143">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1fb99-143">Go back to the [APIs list](apis-list.md).</span></span>