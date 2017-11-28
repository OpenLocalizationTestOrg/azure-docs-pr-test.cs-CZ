---
title: "konektor OneDrive hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru OneDrive hello s parametry rozhraní REST API"
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
ms.openlocfilehash: 8303794bb3c2844de288f87f40639abb84c160fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-onedrive-connector"></a><span data-ttu-id="7447e-103">Začínáme s konektorem OneDrive hello</span><span class="sxs-lookup"><span data-stu-id="7447e-103">Get started with hello OneDrive connector</span></span>
<span data-ttu-id="7447e-104">Připojte tooOneDrive toomanage vaše soubory, včetně nahrávání, get, odstraňte soubory a další.</span><span class="sxs-lookup"><span data-stu-id="7447e-104">Connect tooOneDrive toomanage your files, including upload, get, delete files, and more.</span></span> 

<span data-ttu-id="7447e-105">S OneDrive můžete:</span><span class="sxs-lookup"><span data-stu-id="7447e-105">With OneDrive, you:</span></span> 

* <span data-ttu-id="7447e-106">Vytvoření pracovního postupu uložením souborů na Onedrivu, nebo aktualizovat existující soubory na OneDrive.</span><span class="sxs-lookup"><span data-stu-id="7447e-106">Build your workflow by storing files in OneDrive, or update existing files in OneDrive.</span></span> 
* <span data-ttu-id="7447e-107">Použijte toostart aktivuje pracovní postup, pokud soubor se vytvoří nebo aktualizuje v rámci OneDrive.</span><span class="sxs-lookup"><span data-stu-id="7447e-107">Use triggers toostart your workflow when a file is created or updated within your OneDrive.</span></span>
* <span data-ttu-id="7447e-108">Pomocí akce toocreate souboru, odstraňte soubor a další.</span><span class="sxs-lookup"><span data-stu-id="7447e-108">Use actions toocreate a file, delete a file, and more.</span></span> <span data-ttu-id="7447e-109">Například pokud byl přijat nový Office 365 e-mail s přílohou (aktivační události) vytvořte nový soubor ve Onedrivu (akce).</span><span class="sxs-lookup"><span data-stu-id="7447e-109">For example, when a new Office 365 email is received with an attachment (a trigger), create a new file in OneDrive (an action).</span></span>

<span data-ttu-id="7447e-110">Toto téma ukazuje, jak toouse hello konektoru aplikace logiky na OneDrive a také seznamy hello triggery a akce.</span><span class="sxs-lookup"><span data-stu-id="7447e-110">This topic shows you how toouse hello OneDrive connector in a logic app, and also lists hello triggers and actions.</span></span>

<span data-ttu-id="7447e-111">toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="7447e-111">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooonedrive"></a><span data-ttu-id="7447e-112">Připojit tooOneDrive</span><span class="sxs-lookup"><span data-stu-id="7447e-112">Connect tooOneDrive</span></span>
<span data-ttu-id="7447e-113">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="7447e-113">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="7447e-114">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="7447e-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="7447e-115">Například tooconnect tooOneDrive, musíte nejdřív OneDrive *připojení*.</span><span class="sxs-lookup"><span data-stu-id="7447e-115">For example, tooconnect tooOneDrive, you first need a OneDrive *connection*.</span></span> <span data-ttu-id="7447e-116">toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello službu, kterou chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="7447e-116">toocreate a connection, enter hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="7447e-117">Ano OneDrive, zadejte hello pověření tooyour OneDrive účet toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="7447e-117">So, with OneDrive, enter hello credentials tooyour OneDrive account  toocreate hello connection.</span></span>

### <a name="create-hello-connection"></a><span data-ttu-id="7447e-118">Vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="7447e-118">Create hello connection</span></span>
> [!INCLUDE [Steps toocreate a connection tooOneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="7447e-119">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="7447e-119">Use a trigger</span></span>
<span data-ttu-id="7447e-120">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="7447e-120">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="7447e-121">Aktivační události "dotazování" hello služby intervalu a frekvenci, který chcete.</span><span class="sxs-lookup"><span data-stu-id="7447e-121">Triggers "poll" hello service at an interval and frequency that you want.</span></span> <span data-ttu-id="7447e-122">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="7447e-122">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="7447e-123">V aplikaci logiky hello zadejte "onedrive" tooget seznam hello aktivační události:</span><span class="sxs-lookup"><span data-stu-id="7447e-123">In hello logic app, type "onedrive" tooget a list of hello triggers:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. <span data-ttu-id="7447e-124">Vyberte **změny souboru**.</span><span class="sxs-lookup"><span data-stu-id="7447e-124">Select **When a file is modified**.</span></span> <span data-ttu-id="7447e-125">Pokud připojení již existuje, pak vyberte hello zobrazit výběr tlačítko tooselect složku.</span><span class="sxs-lookup"><span data-stu-id="7447e-125">If a connection already exists, then select hello Show Picker button tooselect a folder.</span></span>
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    <span data-ttu-id="7447e-126">Pokud jste výzvami toosign v, zadejte přihlašovací hello podrobnosti toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="7447e-126">If you are prompted toosign in, then enter hello sign in details toocreate hello connection.</span></span> <span data-ttu-id="7447e-127">[Vytvoření připojení hello](connectors-create-api-onedrive.md#create-the-connection) v tomto tématu jsou uvedeny kroky hello.</span><span class="sxs-lookup"><span data-stu-id="7447e-127">[Create hello connection](connectors-create-api-onedrive.md#create-the-connection) in this topic lists hello steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="7447e-128">V tomto příkladu aplikace logiky hello spustí při otevření souboru ve složce hello, který zvolíte, se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="7447e-128">In this example, hello logic app runs when a file in hello folder you choose is updated.</span></span> <span data-ttu-id="7447e-129">toosee hello výsledky této aktivační události, přidejte další akci, která vám pošle e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7447e-129">toosee hello results of this trigger, add another action that sends you an email.</span></span> <span data-ttu-id="7447e-130">Například přidejte hello Office 365 Outlook *e-mailovou zprávu* akci, která odešle e-mail, když se aktualizuje soubor.</span><span class="sxs-lookup"><span data-stu-id="7447e-130">For example, add hello Office 365 Outlook *Send an email* action that emails you when a file is updated.</span></span> 

3. <span data-ttu-id="7447e-131">Vyberte hello **upravit** tlačítko a nastavte hello **frekvence** a **Interval** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7447e-131">Select hello **Edit** button and set hello **Frequency** and **Interval** values.</span></span> <span data-ttu-id="7447e-132">Například pokud chcete hello aktivační událost toopoll každých 15 minut, pak nastavte hello **frekvence** příliš**minutu**a sadu hello **Interval** příliš**15**.</span><span class="sxs-lookup"><span data-stu-id="7447e-132">For example, if you want hello trigger toopoll every 15 minutes, then set hello **Frequency** too**Minute**, and set hello **Interval** too**15**.</span></span> 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. <span data-ttu-id="7447e-133">**Uložit** změny (levém horním rohu hello panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="7447e-133">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="7447e-134">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="7447e-134">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="7447e-135">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="7447e-135">Use an action</span></span>
<span data-ttu-id="7447e-136">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="7447e-136">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="7447e-137">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="7447e-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="7447e-138">Vyberte znaménko plus hello.</span><span class="sxs-lookup"><span data-stu-id="7447e-138">Select hello plus sign.</span></span> <span data-ttu-id="7447e-139">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="7447e-139">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. <span data-ttu-id="7447e-140">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="7447e-140">Choose **Add an action**.</span></span>
3. <span data-ttu-id="7447e-141">Hello textového pole zadejte "onedrive" tooget seznam všechny dostupné akce hello.</span><span class="sxs-lookup"><span data-stu-id="7447e-141">In hello text box, type “onedrive” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. <span data-ttu-id="7447e-142">V našem příkladu zvolte **OneDrive – vytvoření souboru**.</span><span class="sxs-lookup"><span data-stu-id="7447e-142">In our example, choose **OneDrive - Create file**.</span></span> <span data-ttu-id="7447e-143">Pokud připojení již existuje, pak vyberte hello **cesta ke složce** tooput hello souboru, zadejte hello **název souboru**a zvolte hello **obsah souboru** chcete:</span><span class="sxs-lookup"><span data-stu-id="7447e-143">If a connection already exists, then select hello **Folder Path** tooput hello file, enter hello **File Name**, and choose hello **File Content** you want:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    <span data-ttu-id="7447e-144">Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="7447e-144">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="7447e-145">[Vytvoření připojení hello](connectors-create-api-onedrive.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7447e-145">[Create hello connection](connectors-create-api-onedrive.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="7447e-146">V tomto příkladu vytvoříme nový soubor ve složce OneDrive.</span><span class="sxs-lookup"><span data-stu-id="7447e-146">In this example, we create a new file in a OneDrive folder.</span></span> <span data-ttu-id="7447e-147">Můžete použít výstup z jiného aktivační událost toocreate hello OneDrive souboru.</span><span class="sxs-lookup"><span data-stu-id="7447e-147">You can use output from another trigger toocreate hello OneDrive file.</span></span> <span data-ttu-id="7447e-148">Například přidejte hello Office 365 Outlook *při doručení nových e-mailů* aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7447e-148">For example, add hello Office 365 Outlook *When a new email arrives* trigger.</span></span> <span data-ttu-id="7447e-149">Pak přidejte hello OneDrive *vytvořit soubor* akci, která používá hello příloh a pole Content-Type v rámci příkazu ForEach toocreate hello nový soubor ve Onedrivu.</span><span class="sxs-lookup"><span data-stu-id="7447e-149">Then add hello OneDrive *Create file* action that uses hello Attachments and Content-Type fields within a ForEach toocreate hello new file in OneDrive.</span></span> 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. <span data-ttu-id="7447e-150">**Uložit** změny (levém horním rohu hello panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="7447e-150">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="7447e-151">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="7447e-151">Your logic app is saved and may be automatically enabled.</span></span>


## <a name="connector-specific-details"></a><span data-ttu-id="7447e-152">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="7447e-152">Connector-specific details</span></span>

<span data-ttu-id="7447e-153">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/onedriveconnector/).</span><span class="sxs-lookup"><span data-stu-id="7447e-153">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/onedriveconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="7447e-154">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="7447e-154">More connectors</span></span>
<span data-ttu-id="7447e-155">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="7447e-155">Go back toohello [APIs list](apis-list.md).</span></span>
