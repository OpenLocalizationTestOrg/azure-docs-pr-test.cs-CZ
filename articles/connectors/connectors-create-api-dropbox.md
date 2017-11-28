---
title: konektor aaaDropbox v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte tooDropbox toomanage vaše soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v Dropbox."
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
ms.openlocfilehash: 1f307477836104c0bc0008341604a1400860987f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-dropbox-connector"></a><span data-ttu-id="3905a-105">Začínáme s konektorem Dropbox hello</span><span class="sxs-lookup"><span data-stu-id="3905a-105">Get started with hello Dropbox connector</span></span>
<span data-ttu-id="3905a-106">Připojte tooDropbox toomanage vaše soubory.</span><span class="sxs-lookup"><span data-stu-id="3905a-106">Connect tooDropbox toomanage your files.</span></span> <span data-ttu-id="3905a-107">Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v Dropbox.</span><span class="sxs-lookup"><span data-stu-id="3905a-107">You can perform various actions such as upload, update, get, and delete files in Dropbox.</span></span>

<span data-ttu-id="3905a-108">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3905a-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="3905a-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3905a-109">You can get started by [creating a Logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toodropbox"></a><span data-ttu-id="3905a-110">Připojit tooDropbox</span><span class="sxs-lookup"><span data-stu-id="3905a-110">Connect tooDropbox</span></span>
<span data-ttu-id="3905a-111">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="3905a-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="3905a-112">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="3905a-112">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="3905a-113">Například v pořadí tooconnect tooDropbox, musíte nejdřív Dropbox *připojení*.</span><span class="sxs-lookup"><span data-stu-id="3905a-113">For example, in order tooconnect tooDropbox, you first need a Dropbox *connection*.</span></span> <span data-ttu-id="3905a-114">toocreate připojení, bude třeba tooprovide hello přihlašovací údaje, obvykle použijete tooaccess hello službu, kterou chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="3905a-114">toocreate a connection, you would need tooprovide hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="3905a-115">Ano v příkladu hello Dropbox, bude třeba tooyour hello přihlašovací údaje účtu Dropboxu v pořadí toocreate hello připojení tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="3905a-115">So, in hello Dropbox example, you would need hello credentials tooyour Dropbox account in order toocreate hello connection tooDropbox.</span></span> [<span data-ttu-id="3905a-116">Další informace o připojení</span><span class="sxs-lookup"><span data-stu-id="3905a-116">Learn more about connections</span></span>]()

### <a name="create-a-connection-toodropbox"></a><span data-ttu-id="3905a-117">Vytvoření připojení tooDropbox</span><span class="sxs-lookup"><span data-stu-id="3905a-117">Create a connection tooDropbox</span></span>
> [!INCLUDE [Steps toocreate a connection tooDropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a><span data-ttu-id="3905a-118">Použít aktivační událost Dropbox</span><span class="sxs-lookup"><span data-stu-id="3905a-118">Use a Dropbox trigger</span></span>
<span data-ttu-id="3905a-119">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3905a-119">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="3905a-120">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="3905a-120">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="3905a-121">V tomto příkladu použijeme hello **vytvoření souboru** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="3905a-121">In this example, we will use hello **When a file is created** trigger.</span></span> <span data-ttu-id="3905a-122">Když dojde k této aktivační události, říkáme hello **získat obsah souboru pomocí cesty** Dropbox akce.</span><span class="sxs-lookup"><span data-stu-id="3905a-122">When this trigger occurs, we will call hello **Get file content using path** Dropbox action.</span></span> 

1. <span data-ttu-id="3905a-123">Zadejte *dropbox* hello vyhledávacího pole v designeru hello Logic Apps, pak vyberte hello **Dropboxu - Pokud dojde k vytvoření souboru** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="3905a-123">Enter *dropbox* in hello search box on hello Logic Apps designer, then select hello **Dropbox - When a file is created** trigger.</span></span>      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. <span data-ttu-id="3905a-124">Vyberte hello složku, ve kterém chcete tootrack vytváření souborů.</span><span class="sxs-lookup"><span data-stu-id="3905a-124">Select hello folder in which you want tootrack file creation.</span></span> <span data-ttu-id="3905a-125">Vyberte... (označená červenou hello pole) a procházet toohello složku, vstup tooselect hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="3905a-125">Select ... (identified in hello red box) and browse toohello folder you wish tooselect for hello trigger's input.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a><span data-ttu-id="3905a-126">Pomocí akce Dropbox</span><span class="sxs-lookup"><span data-stu-id="3905a-126">Use a Dropbox action</span></span>
<span data-ttu-id="3905a-127">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3905a-127">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="3905a-128">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="3905a-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="3905a-129">Teď, když hello aktivační událost byla přidána, postupujte podle těchto kroků tooadd akci, která bude získat obsah hello nový soubor.</span><span class="sxs-lookup"><span data-stu-id="3905a-129">Now that hello trigger has been added, follow these steps tooadd an action that will get hello new file's content.</span></span>

1. <span data-ttu-id="3905a-130">Vyberte **+ nový krok** tooadd hello akce chcete tootake při vytvoření nového souboru.</span><span class="sxs-lookup"><span data-stu-id="3905a-130">Select **+ New Step** tooadd hello action you would like tootake when a new file is created.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. <span data-ttu-id="3905a-131">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="3905a-131">Select **Add an action**.</span></span> <span data-ttu-id="3905a-132">Pole pro vyhledávání otevře hello kde můžete vyhledat všechny akce jste chtěli tootake.</span><span class="sxs-lookup"><span data-stu-id="3905a-132">This opens hello search box where you can search for any action you would like tootake.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. <span data-ttu-id="3905a-133">Zadejte *dropbox* toosearch pro akce související tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="3905a-133">Enter *dropbox* toosearch for actions related tooDropbox.</span></span>  
4. <span data-ttu-id="3905a-134">Vyberte **Dropbox - Get obsah souboru pomocí cesty** jako hello tootake akce při vytvoření nového souboru v hello vybrané složce Dropbox.</span><span class="sxs-lookup"><span data-stu-id="3905a-134">Select **Dropbox - Get file content using path** as hello action tootake when a new file is created in hello selected Dropbox folder.</span></span> <span data-ttu-id="3905a-135">Otevře se řídicí blok Hello akce.</span><span class="sxs-lookup"><span data-stu-id="3905a-135">hello action control block opens.</span></span> <span data-ttu-id="3905a-136">Můžete se výzvami tooauthorize vaše tooaccess aplikace logiky vaší schránky účtu, pokud jste tak dosud neučinili dříve.</span><span class="sxs-lookup"><span data-stu-id="3905a-136">You will be prompted tooauthorize your logic app tooaccess your Dropbox account if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. <span data-ttu-id="3905a-137">Vyberte... (nachází na pravé straně hello hello **cesta k souboru** ovládací prvek) a cesta k souboru toohello toouse chcete procházet.</span><span class="sxs-lookup"><span data-stu-id="3905a-137">Select ... (located at hello right side of hello **File Path** control) and browse toohello file path you would like toouse.</span></span> <span data-ttu-id="3905a-138">Nebo použijte hello **cesta k souboru** tokenu toospeed si vaše vytváření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3905a-138">Or, use hello **file path** token toospeed up your logic app creation.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. <span data-ttu-id="3905a-139">Uložte si práci a vytvořte nový soubor v Dropbox tooactivate pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="3905a-139">Save your work and create a new file in Dropbox tooactivate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="3905a-140">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="3905a-140">Connector-specific details</span></span>

<span data-ttu-id="3905a-141">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/dropbox/).</span><span class="sxs-lookup"><span data-stu-id="3905a-141">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/dropbox/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="3905a-142">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="3905a-142">More connectors</span></span>
<span data-ttu-id="3905a-143">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="3905a-143">Go back toohello [APIs list](apis-list.md).</span></span>
