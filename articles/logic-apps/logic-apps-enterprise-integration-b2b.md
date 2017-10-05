---
title: "Vytváření řešení B2B - Azure Logic Apps | Microsoft Docs"
description: "Přijímat data v aplikacích logiky pomocí funkcí B2B v podniku integračního balíčku"
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
ms.openlocfilehash: 0625787ddcbc0091e70b111f687e25929720ad15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="receive-data-in-logic-apps-with-the-b2b-features-in-the-enterprise-integration-pack"></a><span data-ttu-id="6a27b-103">Přijímat data v aplikacích logiky s funkcemi B2B v podniku integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="6a27b-103">Receive data in logic apps with the B2B features in the Enterprise Integration Pack</span></span>

<span data-ttu-id="6a27b-104">Po vytvoření účtu integrace s partnery a smlouvy, jste připraveni k vytvoření pracovního postupu obchodní – firmy (B2B) pro svou aplikaci logiky s [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a27b-104">After you create an integration account that has partners and agreements, you are ready to create a business to business (B2B) workflow for your logic app with the [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a27b-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a27b-105">Prerequisites</span></span>

<span data-ttu-id="6a27b-106">Použití AS2 a X12 akce, musí mít účet integrace podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="6a27b-106">To use the AS2 and X12 actions, you must have an Enterprise Integration Account.</span></span> <span data-ttu-id="6a27b-107">Další informace [postup vytvoření účtu integrace Enterprise](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="6a27b-107">Learn [how to create an Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

## <a name="create-a-logic-app-with-b2b-connectors"></a><span data-ttu-id="6a27b-108">Vytvoření aplikace logiky s konektory B2B</span><span class="sxs-lookup"><span data-stu-id="6a27b-108">Create a logic app with B2B connectors</span></span>

<span data-ttu-id="6a27b-109">Postupujte podle těchto kroků můžete vytvořit aplikaci logiky B2B, které používá AS2 a X12 akce přijímat data z obchodního partnera:</span><span class="sxs-lookup"><span data-stu-id="6a27b-109">Follow these steps to create a B2B logic app that uses the AS2 and X12 actions to receive data from a trading partner:</span></span>

1. <span data-ttu-id="6a27b-110">Vytvoření aplikace logiky, pak [propojit aplikaci se svým účtem integrace](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="6a27b-110">Create a logic app, then [link your app to your integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

2. <span data-ttu-id="6a27b-111">Přidat **požadavku - obdrží žádost HTTP při** aktivační svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="6a27b-111">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. <span data-ttu-id="6a27b-112">Chcete-li přidat **dekódovat AS2** akce, vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="6a27b-112">To add the **Decode AS2** action, select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. <span data-ttu-id="6a27b-113">Chcete-li všechny akce s klíčem, který chcete filtrovat, zadejte slovo **as2** do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6a27b-113">To filter all actions to the one that you want, enter the word **as2** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. <span data-ttu-id="6a27b-114">Vyberte **AS2 – zpráva dekódovat AS2** akce.</span><span class="sxs-lookup"><span data-stu-id="6a27b-114">Select the **AS2 - Decode AS2 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. <span data-ttu-id="6a27b-115">Přidat **textu** , kterou chcete použít jako vstup.</span><span class="sxs-lookup"><span data-stu-id="6a27b-115">Add the **Body** that you want to use as input.</span></span> <span data-ttu-id="6a27b-116">V tomto příkladu vyberte textu požadavku HTTP, která aktivuje aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="6a27b-116">In this example, select the body of the HTTP request that triggers the logic app.</span></span> <span data-ttu-id="6a27b-117">Nebo zadejte výraz, který vstup hlavičky v **HLAVIČKY** pole:</span><span class="sxs-lookup"><span data-stu-id="6a27b-117">Or enter an expression that inputs the headers in the **HEADERS** field:</span></span>

    <span data-ttu-id="6a27b-118">@triggerOutputs() [záhlaví]</span><span class="sxs-lookup"><span data-stu-id="6a27b-118">@triggerOutputs()['headers']</span></span>

7. <span data-ttu-id="6a27b-119">Přidejte požadované **hlavičky** pro AS2, které můžete najít v hlavičkách žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a27b-119">Add the required **Headers** for AS2, which you can find in the HTTP request headers.</span></span> <span data-ttu-id="6a27b-120">V tomto příkladu vyberte hlavičky požadavku HTTP, které spouštějí aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="6a27b-120">In this example, select the headers of the HTTP request that trigger the logic app.</span></span>

8. <span data-ttu-id="6a27b-121">Nyní přidejte akce dekódování X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="6a27b-121">Now add the Decode X12 message action.</span></span> <span data-ttu-id="6a27b-122">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="6a27b-122">Select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. <span data-ttu-id="6a27b-123">Chcete-li všechny akce s klíčem, který chcete filtrovat, zadejte slovo **x12** do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6a27b-123">To filter all actions to the one that you want, enter the word **x12** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. <span data-ttu-id="6a27b-124">Vyberte **X12-dekódovat X12 zpráva** akce.</span><span class="sxs-lookup"><span data-stu-id="6a27b-124">Select the **X12 - Decode X12 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. <span data-ttu-id="6a27b-125">Nyní je nutné určit vstup pro tuto akci.</span><span class="sxs-lookup"><span data-stu-id="6a27b-125">Now you must specify the input to this action.</span></span> <span data-ttu-id="6a27b-126">Tento vstup je výstup z předchozí akce AS2.</span><span class="sxs-lookup"><span data-stu-id="6a27b-126">This input is the output from the previous AS2 action.</span></span>

    <span data-ttu-id="6a27b-127">Zpráva skutečný obsah je v objektu JSON a s kódováním base64, musíte zadat výraz jako vstup.</span><span class="sxs-lookup"><span data-stu-id="6a27b-127">The actual message content is in a JSON object and is base64 encoded, so you must specify an expression as the input.</span></span> 
    <span data-ttu-id="6a27b-128">Zadejte následující výraz v **X12 s PLOCHOU dekódování na zprávu souboru** vstupní pole:</span><span class="sxs-lookup"><span data-stu-id="6a27b-128">Enter the following expression in the **X12 FLAT FILE MESSAGE TO DECODE** input field:</span></span>
    
    <span data-ttu-id="6a27b-129">@base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="6a27b-129">@base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])</span></span>

    <span data-ttu-id="6a27b-130">Nyní přidáte kroky pro dekódování X12 data přijata z obchodního partnera a výstupní položek v objektu JSON.</span><span class="sxs-lookup"><span data-stu-id="6a27b-130">Now add steps to decode the X12 data received from the trading partner and output items in a JSON object.</span></span> 
    <span data-ttu-id="6a27b-131">Oznámit partnerovi přijímání dat, můžete odeslat zpět odpověď obsahující AS2 zpráva dispozice oznámení (MDN) v akce odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a27b-131">To notify the partner that the data was received, you can send back a response containing the AS2 Message Disposition Notification (MDN) in an HTTP Response Action.</span></span>

12. <span data-ttu-id="6a27b-132">Chcete-li přidat **odpovědi** akce, zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="6a27b-132">To add the **Response** action, choose **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. <span data-ttu-id="6a27b-133">Chcete-li všechny akce s klíčem, který chcete filtrovat, zadejte slovo **odpovědi** do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6a27b-133">To filter all actions to the one that you want, enter the word **response** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. <span data-ttu-id="6a27b-134">Vyberte **odpovědi** akce.</span><span class="sxs-lookup"><span data-stu-id="6a27b-134">Select the **Response** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. <span data-ttu-id="6a27b-135">Pro přístup MDN z výstupu **dekódování X12 zpráva** akce, nastavte odpovědi **textu** pole s tímto výrazem:</span><span class="sxs-lookup"><span data-stu-id="6a27b-135">To access the MDN from the output of the **Decode X12 message** action, set the response **BODY** field with this expression:</span></span>

    <span data-ttu-id="6a27b-136">@base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="6a27b-136">@base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. <span data-ttu-id="6a27b-137">Uložte práci.</span><span class="sxs-lookup"><span data-stu-id="6a27b-137">Save your work.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

<span data-ttu-id="6a27b-138">Nyní dokončení nastavení aplikace logiky B2B.</span><span class="sxs-lookup"><span data-stu-id="6a27b-138">You are now done setting up your B2B logic app.</span></span> <span data-ttu-id="6a27b-139">V reálné aplikaci, můžete chtít uložit dekódované X12 dat v úložišti obchodní (LOB) aplikace nebo data.</span><span class="sxs-lookup"><span data-stu-id="6a27b-139">In a real world application, you might want to store the decoded X12 data in a line-of-business (LOB) app or data store.</span></span> <span data-ttu-id="6a27b-140">K připojení vlastní obchodní aplikace a použijte tato rozhraní API v aplikaci logiky, můžete přidat další akce nebo můžete zapsat vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6a27b-140">To connect your own LOB apps and use these APIs in your logic app, you can add further actions or write custom APIs.</span></span>

## <a name="features-and-use-cases"></a><span data-ttu-id="6a27b-141">Funkce a případy použití</span><span class="sxs-lookup"><span data-stu-id="6a27b-141">Features and use cases</span></span>

* <span data-ttu-id="6a27b-142">AS2 a X12 dekódovat a kódování akce umožňují výměnu dat mezi obchodních partnerů pomocí standardních protokolů v aplikacích logiky.</span><span class="sxs-lookup"><span data-stu-id="6a27b-142">The AS2 and X12 decode and encode actions let you exchange data between trading partners by using industry standard protocols in logic apps.</span></span>
* <span data-ttu-id="6a27b-143">Chcete-li vyměňovat data s obchodními partnery, můžete použít AS2 a X12 s nebo bez navzájem.</span><span class="sxs-lookup"><span data-stu-id="6a27b-143">To exchange data with trading partners, you can use AS2 and X12 with or without each other.</span></span>
* <span data-ttu-id="6a27b-144">Akce B2B můžete jednoduše vytvořit partnery a smluv ve vašem účtu integrace a využívat v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="6a27b-144">The B2B actions help you create partners and agreements easily in your integration account and consume them in a logic app.</span></span>
* <span data-ttu-id="6a27b-145">Když rozšíříte svou aplikaci logiky s další akce, můžete odesílat a přijímat data mezi jiným aplikacím a službám, jako je SalesForce.</span><span class="sxs-lookup"><span data-stu-id="6a27b-145">When you extend your logic app with other actions, you can send and receive data between other apps and services like SalesForce.</span></span>

## <a name="learn-more"></a><span data-ttu-id="6a27b-146">Další informace</span><span class="sxs-lookup"><span data-stu-id="6a27b-146">Learn more</span></span>
[<span data-ttu-id="6a27b-147">Další informace o Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="6a27b-147">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
