---
title: "zprávy aaaEncode AS2 – Azure Logic Apps | Microsoft Docs"
description: "Jak toouse hello AS2 kodér v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 2b372c416512ffa9ea5dc50ce0f767bfd8aefbc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="65bf2-103">Kódování zprávy AS2 pro Azure Logic Apps s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="65bf2-103">Encode AS2 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="65bf2-104">tooestablish zabezpečení a spolehlivost při přenášení zpráv, pomocí konektoru serveru hello AS2 kódování zprávy.</span><span class="sxs-lookup"><span data-stu-id="65bf2-104">tooestablish security and reliability while transmitting messages, use hello Encode AS2 message connector.</span></span> <span data-ttu-id="65bf2-105">Tento konektor poskytuje digitální podpis, šifrování a potvrzení prostřednictvím zpráv dispozice oznámení (MDN), což také vede toosupport pro Non-Odvolatelnost.</span><span class="sxs-lookup"><span data-stu-id="65bf2-105">This connector provides digital signing, encryption, and acknowledgements through Message Disposition Notifications (MDN), which also leads toosupport for Non-Repudiation.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="65bf2-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="65bf2-106">Before you start</span></span>

<span data-ttu-id="65bf2-107">Tady je hello položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="65bf2-107">Here's hello items you need:</span></span>

* <span data-ttu-id="65bf2-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="65bf2-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="65bf2-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="65bf2-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="65bf2-110">Musíte mít konektoru integrace účet toouse hello AS2 kódování zprávy.</span><span class="sxs-lookup"><span data-stu-id="65bf2-110">You must have an integration account toouse hello Encode AS2 message connector.</span></span>
* <span data-ttu-id="65bf2-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="65bf2-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="65bf2-112">[Smlouvy AS2](logic-apps-enterprise-integration-as2.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="65bf2-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="encode-as2-messages"></a><span data-ttu-id="65bf2-113">Kódování zprávy AS2</span><span class="sxs-lookup"><span data-stu-id="65bf2-113">Encode AS2 messages</span></span>

1. <span data-ttu-id="65bf2-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="65bf2-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="65bf2-115">Hello AS2 kódování zpráv konektor nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="65bf2-115">hello Encode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="65bf2-116">V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="65bf2-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="65bf2-117">Hello vyhledávacího pole zadejte "AS2" filtru.</span><span class="sxs-lookup"><span data-stu-id="65bf2-117">In hello search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="65bf2-118">Vyberte **AS2 - AS2 kódování zpráv**.</span><span class="sxs-lookup"><span data-stu-id="65bf2-118">Select **AS2 - Encode AS2 message**.</span></span>
   
    ![Vyhledejte "AS2"](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. <span data-ttu-id="65bf2-120">Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="65bf2-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="65bf2-121">Název připojení a vyberte účet hello integrace, které chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="65bf2-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 
   
    ![Vytvořte účet toointegration připojení](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    <span data-ttu-id="65bf2-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="65bf2-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="65bf2-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="65bf2-124">Property</span></span> | <span data-ttu-id="65bf2-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="65bf2-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="65bf2-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="65bf2-126">Connection Name *</span></span> |<span data-ttu-id="65bf2-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="65bf2-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="65bf2-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="65bf2-128">Integration Account *</span></span> |<span data-ttu-id="65bf2-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="65bf2-129">Enter a name for your integration account.</span></span> <span data-ttu-id="65bf2-130">Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="65bf2-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="65bf2-131">Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis.</span><span class="sxs-lookup"><span data-stu-id="65bf2-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="65bf2-132">toofinish vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="65bf2-132">toofinish creating your connection, choose **Create**.</span></span>
   
    ![Podrobné informace o integraci připojení](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. <span data-ttu-id="65bf2-134">Po vytvoření připojení, jak je znázorněno v tomto příkladu, zadejte podrobnosti pro **AS2-z**, **AS2 tooidentifiers** podle konfigurace v smlouvě, a **textu**, což je Hello datovou část zprávy.</span><span class="sxs-lookup"><span data-stu-id="65bf2-134">After your connection is created, as shown in this example, provide details for **AS2-From**, **AS2-tooidentifiers** as configured in your agreement, and **Body**, which is hello message payload.</span></span>
   
    ![Zadejte povinné pole](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a><span data-ttu-id="65bf2-136">Podrobnosti kodér AS2</span><span class="sxs-lookup"><span data-stu-id="65bf2-136">AS2 encoder details</span></span>

<span data-ttu-id="65bf2-137">konektor kódování AS2 Hello provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="65bf2-137">hello Encode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="65bf2-138">Platí hlavičky AS2/HTTP</span><span class="sxs-lookup"><span data-stu-id="65bf2-138">Applies AS2/HTTP headers</span></span>
* <span data-ttu-id="65bf2-139">Přihlásí odchozí zprávy (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="65bf2-139">Signs outgoing messages (if configured)</span></span>
* <span data-ttu-id="65bf2-140">Šifruje odchozích zpráv (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="65bf2-140">Encrypts outgoing messages (if configured)</span></span>
* <span data-ttu-id="65bf2-141">Komprimuje uvítací zprávu (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="65bf2-141">Compresses hello message (if configured)</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="65bf2-142">Zkuste tuto ukázku</span><span class="sxs-lookup"><span data-stu-id="65bf2-142">Try this sample</span></span>

<span data-ttu-id="65bf2-143">tootry nasazení plně funkční logiku aplikace a ukázkové AS2 scénáři, najdete v části hello [AS2 šablona aplikace logiky a scénář](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="65bf2-143">tootry deploying a fully operational logic app and sample AS2 scenario, see hello [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="65bf2-144">Zobrazení hello swagger</span><span class="sxs-lookup"><span data-stu-id="65bf2-144">View hello swagger</span></span>
<span data-ttu-id="65bf2-145">V tématu hello [swagger podrobnosti](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="65bf2-145">See hello [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="65bf2-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65bf2-146">Next steps</span></span>
[<span data-ttu-id="65bf2-147">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="65bf2-147">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

