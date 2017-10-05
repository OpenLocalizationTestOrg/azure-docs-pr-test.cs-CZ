---
title: "Přidejte konektor OneDrive ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru OneDrive s parametry rozhraní REST API"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 63bd33bf4e09b98aa53dcfec9fcc4a0109204952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-onedrive-connector"></a><span data-ttu-id="2684c-103">Začínáme s konektorem OneDrive</span><span class="sxs-lookup"><span data-stu-id="2684c-103">Get started with the OneDrive connector</span></span>
<span data-ttu-id="2684c-104">Připojte se ke Onedrivu spravovat vaše soubory, včetně nahrávání, získat, odstraňte soubory a další.</span><span class="sxs-lookup"><span data-stu-id="2684c-104">Connect to OneDrive to manage your files, including upload, get, delete files, and more.</span></span> 

<span data-ttu-id="2684c-105">S OneDrive můžete:</span><span class="sxs-lookup"><span data-stu-id="2684c-105">With OneDrive, you:</span></span> 

* <span data-ttu-id="2684c-106">Vytvoření pracovního postupu uložením souborů na Onedrivu, nebo aktualizovat existující soubory na OneDrive.</span><span class="sxs-lookup"><span data-stu-id="2684c-106">Build your workflow by storing files in OneDrive, or update existing files in OneDrive.</span></span> 
* <span data-ttu-id="2684c-107">Spustit pracovní postup v případě, že soubor se vytvoří nebo aktualizuje v rámci OneDrive pomocí aktivační události.</span><span class="sxs-lookup"><span data-stu-id="2684c-107">Use triggers to start your workflow when a file is created or updated within your OneDrive.</span></span>
* <span data-ttu-id="2684c-108">Použijte k vytvoření souboru, odstraňte soubor a další akce.</span><span class="sxs-lookup"><span data-stu-id="2684c-108">Use actions to create a file, delete a file, and more.</span></span> <span data-ttu-id="2684c-109">Například pokud byl přijat nový Office 365 e-mail s přílohou (aktivační události) vytvořte nový soubor ve Onedrivu (akce).</span><span class="sxs-lookup"><span data-stu-id="2684c-109">For example, when a new Office 365 email is received with an attachment (a trigger), create a new file in OneDrive (an action).</span></span>

<span data-ttu-id="2684c-110">Toto téma ukazuje, jak k používání konektoru OneDrive v aplikaci logiky a taky seznam triggery a akce.</span><span class="sxs-lookup"><span data-stu-id="2684c-110">This topic shows you how to use the OneDrive connector in a logic app, and also lists the triggers and actions.</span></span>

<span data-ttu-id="2684c-111">Další informace o Logic Apps najdete v tématu [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2684c-111">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-onedrive"></a><span data-ttu-id="2684c-112">Připojte se ke Onedrivu</span><span class="sxs-lookup"><span data-stu-id="2684c-112">Connect to OneDrive</span></span>
<span data-ttu-id="2684c-113">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="2684c-113">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="2684c-114">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="2684c-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="2684c-115">Například pokud chcete připojit k Onedrivu, musíte nejdřív OneDrive *připojení*.</span><span class="sxs-lookup"><span data-stu-id="2684c-115">For example, to connect to OneDrive, you first need a OneDrive *connection*.</span></span> <span data-ttu-id="2684c-116">Chcete-li vytvořit připojení, zadejte přihlašovací údaje, které standardně používáte k přístupu ke službě, který chcete připojit k.</span><span class="sxs-lookup"><span data-stu-id="2684c-116">To create a connection, enter the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="2684c-117">Ano s OneDrive, zadejte přihlašovací údaje ke svému účtu Onedrivu k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="2684c-117">So, with OneDrive, enter the credentials to your OneDrive account  to create the connection.</span></span>

### <a name="create-the-connection"></a><span data-ttu-id="2684c-118">Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="2684c-118">Create the connection</span></span>
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="2684c-119">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="2684c-119">Use a trigger</span></span>
<span data-ttu-id="2684c-120">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="2684c-120">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="2684c-121">Aktivační události "dotazování" službu v intervalem a frekvenci, který chcete.</span><span class="sxs-lookup"><span data-stu-id="2684c-121">Triggers "poll" the service at an interval and frequency that you want.</span></span> <span data-ttu-id="2684c-122">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2684c-122">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="2684c-123">V aplikaci logiky zadejte "onedrive" získat seznam aktivačních událostí:</span><span class="sxs-lookup"><span data-stu-id="2684c-123">In the logic app, type "onedrive" to get a list of the triggers:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. <span data-ttu-id="2684c-124">Vyberte **změny souboru**.</span><span class="sxs-lookup"><span data-stu-id="2684c-124">Select **When a file is modified**.</span></span> <span data-ttu-id="2684c-125">Pokud připojení již existuje, pak vyberte tlačítko Zobrazit výběr vyberte složku.</span><span class="sxs-lookup"><span data-stu-id="2684c-125">If a connection already exists, then select the Show Picker button to select a folder.</span></span>
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    <span data-ttu-id="2684c-126">Pokud budete vyzváni k přihlášení, pak zadejte přihlašovací údaje pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="2684c-126">If you are prompted to sign in, then enter the sign in details to create the connection.</span></span> <span data-ttu-id="2684c-127">[Vytvoření připojení](connectors-create-api-onedrive.md#create-the-connection) v tomto tématu jsou uvedeny kroky.</span><span class="sxs-lookup"><span data-stu-id="2684c-127">[Create the connection](connectors-create-api-onedrive.md#create-the-connection) in this topic lists the steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="2684c-128">V tomto příkladu spustí aplikaci logiky při otevření souboru ve složce, kterou zvolíte, se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="2684c-128">In this example, the logic app runs when a file in the folder you choose is updated.</span></span> <span data-ttu-id="2684c-129">Pokud chcete zobrazit výsledky této aktivační události, přidejte další akci, která vám pošle e-mailu.</span><span class="sxs-lookup"><span data-stu-id="2684c-129">To see the results of this trigger, add another action that sends you an email.</span></span> <span data-ttu-id="2684c-130">Například přidejte Office 365 Outlook *e-mailovou zprávu* akci, která odešle e-mail, když se aktualizuje soubor.</span><span class="sxs-lookup"><span data-stu-id="2684c-130">For example, add the Office 365 Outlook *Send an email* action that emails you when a file is updated.</span></span> 

3. <span data-ttu-id="2684c-131">Vyberte **upravit** tlačítko a nastavte **frekvence** a **Interval** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2684c-131">Select the **Edit** button and set the **Frequency** and **Interval** values.</span></span> <span data-ttu-id="2684c-132">Například pokud chcete, aby aktivační událost pro cyklické dotazování každých 15 minut, nastavte **frekvence** k **minutu**a nastavte **Interval** k **15**.</span><span class="sxs-lookup"><span data-stu-id="2684c-132">For example, if you want the trigger to poll every 15 minutes, then set the **Frequency** to **Minute**, and set the **Interval** to **15**.</span></span> 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. <span data-ttu-id="2684c-133">**Uložit** změny (levém horním rohu panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="2684c-133">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="2684c-134">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="2684c-134">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="2684c-135">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="2684c-135">Use an action</span></span>
<span data-ttu-id="2684c-136">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="2684c-136">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="2684c-137">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2684c-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="2684c-138">Vyberte znaménko plus.</span><span class="sxs-lookup"><span data-stu-id="2684c-138">Select the plus sign.</span></span> <span data-ttu-id="2684c-139">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="2684c-139">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. <span data-ttu-id="2684c-140">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="2684c-140">Choose **Add an action**.</span></span>
3. <span data-ttu-id="2684c-141">Do textového pole zadejte "onedrive" získat seznam všechny dostupné akce.</span><span class="sxs-lookup"><span data-stu-id="2684c-141">In the text box, type “onedrive” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. <span data-ttu-id="2684c-142">V našem příkladu zvolte **OneDrive – vytvoření souboru**.</span><span class="sxs-lookup"><span data-stu-id="2684c-142">In our example, choose **OneDrive - Create file**.</span></span> <span data-ttu-id="2684c-143">Pokud připojení již existuje, pak vyberte **cesta ke složce** uvést soubor, zadejte **název souboru**a vyberte **obsah souboru** chcete:</span><span class="sxs-lookup"><span data-stu-id="2684c-143">If a connection already exists, then select the **Folder Path** to put the file, enter the **File Name**, and choose the **File Content** you want:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    <span data-ttu-id="2684c-144">Pokud se zobrazí výzva pro informace o připojení, zadejte podrobnosti k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="2684c-144">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="2684c-145">[Vytvoření připojení](connectors-create-api-onedrive.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2684c-145">[Create the connection](connectors-create-api-onedrive.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="2684c-146">V tomto příkladu vytvoříme nový soubor ve složce OneDrive.</span><span class="sxs-lookup"><span data-stu-id="2684c-146">In this example, we create a new file in a OneDrive folder.</span></span> <span data-ttu-id="2684c-147">Výstup z jiného aktivační událost můžete použít k vytvoření souboru OneDrive.</span><span class="sxs-lookup"><span data-stu-id="2684c-147">You can use output from another trigger to create the OneDrive file.</span></span> <span data-ttu-id="2684c-148">Například přidejte Office 365 Outlook *při doručení nových e-mailů* aktivační události.</span><span class="sxs-lookup"><span data-stu-id="2684c-148">For example, add the Office 365 Outlook *When a new email arrives* trigger.</span></span> <span data-ttu-id="2684c-149">Pak přidejte Onedrivu *vytvořit soubor* akci, která používá příloh a Content-Type pole v rámci příkazu ForEach k vytvoření nového souboru na Onedrivu.</span><span class="sxs-lookup"><span data-stu-id="2684c-149">Then add the OneDrive *Create file* action that uses the Attachments and Content-Type fields within a ForEach to create the new file in OneDrive.</span></span> 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. <span data-ttu-id="2684c-150">**Uložit** změny (levém horním rohu panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="2684c-150">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="2684c-151">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="2684c-151">Your logic app is saved and may be automatically enabled.</span></span>


## <a name="connector-specific-details"></a><span data-ttu-id="2684c-152">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="2684c-152">Connector-specific details</span></span>

<span data-ttu-id="2684c-153">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/onedriveconnector/).</span><span class="sxs-lookup"><span data-stu-id="2684c-153">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/onedriveconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="2684c-154">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="2684c-154">More connectors</span></span>
<span data-ttu-id="2684c-155">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="2684c-155">Go back to the [APIs list](apis-list.md).</span></span>