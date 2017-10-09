---
title: "aaaCreate B2B solutions – Azure Logic Apps | Microsoft Docs"
description: "Přijímat data v aplikacích logiky pomocí funkcí hello B2B aplikace hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8f01318a0415d81c37b216f9b991c060edec2053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="receive-data-in-logic-apps-with-hello-b2b-features-in-hello-enterprise-integration-pack"></a><span data-ttu-id="7be91-103">Přijímat data v aplikacích logiky s funkcemi B2B hello v hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="7be91-103">Receive data in logic apps with hello B2B features in hello Enterprise Integration Pack</span></span>

<span data-ttu-id="7be91-104">Po vytvoření účtu integrace s partnery a smlouvy, jsou připravené toocreate pracovního postupu obchodní toobusiness (B2B) pro svou aplikaci logiky s hello [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7be91-104">After you create an integration account that has partners and agreements, you are ready toocreate a business toobusiness (B2B) workflow for your logic app with hello [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7be91-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7be91-105">Prerequisites</span></span>

<span data-ttu-id="7be91-106">toouse hello AS2 a X12 akce, musí mít účet integrace podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="7be91-106">toouse hello AS2 and X12 actions, you must have an Enterprise Integration Account.</span></span> <span data-ttu-id="7be91-107">Další informace [jak toocreate integrační Enterprise účet](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="7be91-107">Learn [how toocreate an Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

## <a name="create-a-logic-app-with-b2b-connectors"></a><span data-ttu-id="7be91-108">Vytvoření aplikace logiky s konektory B2B</span><span class="sxs-lookup"><span data-stu-id="7be91-108">Create a logic app with B2B connectors</span></span>

<span data-ttu-id="7be91-109">Postupujte podle těchto kroků toocreate B2B logiku aplikaci, která používá hello AS2 a X12 akce tooreceive data z obchodního partnera:</span><span class="sxs-lookup"><span data-stu-id="7be91-109">Follow these steps toocreate a B2B logic app that uses hello AS2 and X12 actions tooreceive data from a trading partner:</span></span>

1. <span data-ttu-id="7be91-110">Vytvoření aplikace logiky, pak [propojte si účet tooyour integrace aplikace](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="7be91-110">Create a logic app, then [link your app tooyour integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

2. <span data-ttu-id="7be91-111">Přidat **požadavku - obdrží žádost HTTP při** aplikace logiky tooyour aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7be91-111">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. <span data-ttu-id="7be91-112">tooadd hello **dekódovat AS2** akce, vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="7be91-112">tooadd hello **Decode AS2** action, select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. <span data-ttu-id="7be91-113">toofilter všechny toohello akce, který chcete, zadejte slovo hello **as2** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="7be91-113">toofilter all actions toohello one that you want, enter hello word **as2** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. <span data-ttu-id="7be91-114">Vyberte hello **AS2 – zpráva dekódovat AS2** akce.</span><span class="sxs-lookup"><span data-stu-id="7be91-114">Select hello **AS2 - Decode AS2 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. <span data-ttu-id="7be91-115">Přidat hello **textu** , které chcete toouse jako vstup.</span><span class="sxs-lookup"><span data-stu-id="7be91-115">Add hello **Body** that you want toouse as input.</span></span> <span data-ttu-id="7be91-116">V tomto příkladu vyberte hello textu hello požadavku protokolu HTTP, aktivační události hello aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="7be91-116">In this example, select hello body of hello HTTP request that triggers hello logic app.</span></span> <span data-ttu-id="7be91-117">Nebo zadejte výraz, který vstup hello hlavičky v hello **HLAVIČKY** pole:</span><span class="sxs-lookup"><span data-stu-id="7be91-117">Or enter an expression that inputs hello headers in hello **HEADERS** field:</span></span>

    <span data-ttu-id="7be91-118">@triggerOutputs() [záhlaví]</span><span class="sxs-lookup"><span data-stu-id="7be91-118">@triggerOutputs()['headers']</span></span>

7. <span data-ttu-id="7be91-119">Přidejte požadované hello **hlavičky** pro AS2, které můžete najít v hlavičkách žádosti hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7be91-119">Add hello required **Headers** for AS2, which you can find in hello HTTP request headers.</span></span> <span data-ttu-id="7be91-120">V tomto příkladu vyberte hello hlavičky požadavku HTTP hello aplikaci logiky hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7be91-120">In this example, select hello headers of hello HTTP request that trigger hello logic app.</span></span>

8. <span data-ttu-id="7be91-121">Nyní přidejte hello dekódování X12 zpráva akce.</span><span class="sxs-lookup"><span data-stu-id="7be91-121">Now add hello Decode X12 message action.</span></span> <span data-ttu-id="7be91-122">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="7be91-122">Select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. <span data-ttu-id="7be91-123">toofilter všechny toohello akce, který chcete, zadejte slovo hello **x12** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="7be91-123">toofilter all actions toohello one that you want, enter hello word **x12** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. <span data-ttu-id="7be91-124">Vyberte hello **X12-dekódovat X12 zpráva** akce.</span><span class="sxs-lookup"><span data-stu-id="7be91-124">Select hello **X12 - Decode X12 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. <span data-ttu-id="7be91-125">Nyní je nutné zadat akci vstupní toothis hello.</span><span class="sxs-lookup"><span data-stu-id="7be91-125">Now you must specify hello input toothis action.</span></span> <span data-ttu-id="7be91-126">Tento vstup je výstup hello z předchozí akce AS2 hello.</span><span class="sxs-lookup"><span data-stu-id="7be91-126">This input is hello output from hello previous AS2 action.</span></span>

    <span data-ttu-id="7be91-127">obsah zprávy skutečné Hello je v objektu JSON a s kódováním base64, musíte zadat výraz jako vstup hello.</span><span class="sxs-lookup"><span data-stu-id="7be91-127">hello actual message content is in a JSON object and is base64 encoded, so you must specify an expression as hello input.</span></span> 
    <span data-ttu-id="7be91-128">Zadejte následující výraz v hello hello **X12 s PLOCHOU zpráva souboru tooDECODE** vstupní pole:</span><span class="sxs-lookup"><span data-stu-id="7be91-128">Enter hello following expression in hello **X12 FLAT FILE MESSAGE tooDECODE** input field:</span></span>
    
    <span data-ttu-id="7be91-129">@base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="7be91-129">@base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])</span></span>

    <span data-ttu-id="7be91-130">Nyní přidejte kroky toodecode hello X12 data přijata z hello obchodního partnera a výstupní položek v objektu JSON.</span><span class="sxs-lookup"><span data-stu-id="7be91-130">Now add steps toodecode hello X12 data received from hello trading partner and output items in a JSON object.</span></span> 
    <span data-ttu-id="7be91-131">byla přijata toonotify hello partnera, který hello data, můžete odeslat zpět odpověď obsahující hello AS2 zpráva dispozice oznámení (MDN) v akce odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7be91-131">toonotify hello partner that hello data was received, you can send back a response containing hello AS2 Message Disposition Notification (MDN) in an HTTP Response Action.</span></span>

12. <span data-ttu-id="7be91-132">tooadd hello **odpovědi** akce, zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="7be91-132">tooadd hello **Response** action, choose **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. <span data-ttu-id="7be91-133">toofilter všechny toohello akce, který chcete, zadejte slovo hello **odpovědi** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="7be91-133">toofilter all actions toohello one that you want, enter hello word **response** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. <span data-ttu-id="7be91-134">Vyberte hello **odpovědi** akce.</span><span class="sxs-lookup"><span data-stu-id="7be91-134">Select hello **Response** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. <span data-ttu-id="7be91-135">tooaccess hello MDN z hello výstup hello **zpráva dekódování X12** akce, sada hello odpovědi **textu** pole s tímto výrazem:</span><span class="sxs-lookup"><span data-stu-id="7be91-135">tooaccess hello MDN from hello output of hello **Decode X12 message** action, set hello response **BODY** field with this expression:</span></span>

    <span data-ttu-id="7be91-136">@base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="7be91-136">@base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. <span data-ttu-id="7be91-137">Uložte práci.</span><span class="sxs-lookup"><span data-stu-id="7be91-137">Save your work.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

<span data-ttu-id="7be91-138">Nyní dokončení nastavení aplikace logiky B2B.</span><span class="sxs-lookup"><span data-stu-id="7be91-138">You are now done setting up your B2B logic app.</span></span> <span data-ttu-id="7be91-139">V reálné aplikaci, můžete toostore hello dekódovat X12 dat v úložišti obchodní (LOB) aplikace nebo data.</span><span class="sxs-lookup"><span data-stu-id="7be91-139">In a real world application, you might want toostore hello decoded X12 data in a line-of-business (LOB) app or data store.</span></span> <span data-ttu-id="7be91-140">tooconnect vlastní obchodní aplikace a použijte tato rozhraní API v aplikaci logiky, můžete přidat další akce nebo můžete zapsat vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7be91-140">tooconnect your own LOB apps and use these APIs in your logic app, you can add further actions or write custom APIs.</span></span>

## <a name="features-and-use-cases"></a><span data-ttu-id="7be91-141">Funkce a případy použití</span><span class="sxs-lookup"><span data-stu-id="7be91-141">Features and use cases</span></span>

* <span data-ttu-id="7be91-142">Hello AS2 a X12 dekódovat a kódování akce umožňují výměnu dat mezi obchodních partnerů pomocí standardních protokolů v aplikacích logiky.</span><span class="sxs-lookup"><span data-stu-id="7be91-142">hello AS2 and X12 decode and encode actions let you exchange data between trading partners by using industry standard protocols in logic apps.</span></span>
* <span data-ttu-id="7be91-143">tooexchange data s obchodními partnery, můžete použít AS2 a X12 s nebo bez navzájem.</span><span class="sxs-lookup"><span data-stu-id="7be91-143">tooexchange data with trading partners, you can use AS2 and X12 with or without each other.</span></span>
* <span data-ttu-id="7be91-144">Hello B2B akce můžete jednoduše vytvořit partnery a smluv ve vašem účtu integrace a využívat v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="7be91-144">hello B2B actions help you create partners and agreements easily in your integration account and consume them in a logic app.</span></span>
* <span data-ttu-id="7be91-145">Když rozšíříte svou aplikaci logiky s další akce, můžete odesílat a přijímat data mezi jiným aplikacím a službám, jako je SalesForce.</span><span class="sxs-lookup"><span data-stu-id="7be91-145">When you extend your logic app with other actions, you can send and receive data between other apps and services like SalesForce.</span></span>

## <a name="learn-more"></a><span data-ttu-id="7be91-146">Další informace</span><span class="sxs-lookup"><span data-stu-id="7be91-146">Learn more</span></span>
[<span data-ttu-id="7be91-147">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="7be91-147">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
