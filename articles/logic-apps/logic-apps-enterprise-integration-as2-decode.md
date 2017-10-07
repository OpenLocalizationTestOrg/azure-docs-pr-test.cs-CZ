---
title: "zprávy aaaDecode AS2 – Azure Logic Apps | Microsoft Docs"
description: "Jak toouse hello AS2 decoder v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 2406e5ec68e0906700fad97d60cb83ef0d106cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="77ff1-103">Dekódovat AS2 zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="77ff1-103">Decode AS2 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span> 

<span data-ttu-id="77ff1-104">tooestablish zabezpečení a spolehlivost při přenášení zpráv pomocí konektoru zpráva dekódovat AS2 hello.</span><span class="sxs-lookup"><span data-stu-id="77ff1-104">tooestablish security and reliability while transmitting messages, use hello Decode AS2 message connector.</span></span> <span data-ttu-id="77ff1-105">Tento konektor poskytuje digitální podpis, dešifrování a potvrzení prostřednictvím zpráv dispozice oznámení (MDN).</span><span class="sxs-lookup"><span data-stu-id="77ff1-105">This connector provides digital signing, decryption, and acknowledgements through Message Disposition Notifications (MDN).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="77ff1-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="77ff1-106">Before you start</span></span>

<span data-ttu-id="77ff1-107">Tady je hello položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="77ff1-107">Here's hello items you need:</span></span>

* <span data-ttu-id="77ff1-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="77ff1-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="77ff1-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="77ff1-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="77ff1-110">Musíte mít konektoru integrace účet toouse hello dekódovat AS2 zprávy.</span><span class="sxs-lookup"><span data-stu-id="77ff1-110">You must have an integration account toouse hello Decode AS2 message connector.</span></span>
* <span data-ttu-id="77ff1-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="77ff1-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="77ff1-112">[Smlouvy AS2](logic-apps-enterprise-integration-as2.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="77ff1-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="decode-as2-messages"></a><span data-ttu-id="77ff1-113">Dekódovat AS2 zprávy</span><span class="sxs-lookup"><span data-stu-id="77ff1-113">Decode AS2 messages</span></span>

1. <span data-ttu-id="77ff1-114">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="77ff1-114">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="77ff1-115">Hello dekódovat AS2 zpráva konektor nemá aktivačních událostí, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="77ff1-115">hello Decode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="77ff1-116">V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="77ff1-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="77ff1-117">Hello vyhledávacího pole zadejte "AS2" filtru.</span><span class="sxs-lookup"><span data-stu-id="77ff1-117">In hello search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="77ff1-118">Vyberte **AS2 - dekódovat AS2 zpráva**.</span><span class="sxs-lookup"><span data-stu-id="77ff1-118">Select **AS2 - Decode AS2 message**.</span></span>
   
    ![Vyhledejte "AS2"](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. <span data-ttu-id="77ff1-120">Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="77ff1-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="77ff1-121">Název připojení a vyberte účet hello integrace, které chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="77ff1-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>
   
    ![Vytvoření připojení integrace](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    <span data-ttu-id="77ff1-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="77ff1-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="77ff1-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="77ff1-124">Property</span></span> | <span data-ttu-id="77ff1-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="77ff1-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="77ff1-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="77ff1-126">Connection Name *</span></span> |<span data-ttu-id="77ff1-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="77ff1-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="77ff1-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="77ff1-128">Integration Account *</span></span> |<span data-ttu-id="77ff1-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="77ff1-129">Enter a name for your integration account.</span></span> <span data-ttu-id="77ff1-130">Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="77ff1-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="77ff1-131">Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis.</span><span class="sxs-lookup"><span data-stu-id="77ff1-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="77ff1-132">toofinish vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="77ff1-132">toofinish creating your connection, choose **Create**.</span></span>

    ![Podrobné informace o integraci připojení](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. <span data-ttu-id="77ff1-134">Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte **textu** a **hlavičky** z hello požadavku výstupy.</span><span class="sxs-lookup"><span data-stu-id="77ff1-134">After your connection is created, as shown in this example, select **Body** and **Headers** from hello Request outputs.</span></span>
   
    ![Vytvoření připojení integrace](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    <span data-ttu-id="77ff1-136">Například:</span><span class="sxs-lookup"><span data-stu-id="77ff1-136">For example:</span></span>

    ![Vyberte textu a hlaviček ze žádosti výstupy](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a><span data-ttu-id="77ff1-138">Podrobnosti decoder AS2</span><span class="sxs-lookup"><span data-stu-id="77ff1-138">AS2 decoder details</span></span>

<span data-ttu-id="77ff1-139">konektor dekódovat AS2 Hello provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="77ff1-139">hello Decode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="77ff1-140">Zpracuje hlavičky AS2/HTTP</span><span class="sxs-lookup"><span data-stu-id="77ff1-140">Processes AS2/HTTP headers</span></span>
* <span data-ttu-id="77ff1-141">Ověří podpis hello (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="77ff1-141">Verifies hello signature (if configured)</span></span>
* <span data-ttu-id="77ff1-142">Dešifruje hello zprávy (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="77ff1-142">Decrypts hello messages (if configured)</span></span>
* <span data-ttu-id="77ff1-143">Dekomprimuje uvítací zprávu (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="77ff1-143">Decompresses hello message (if configured)</span></span>
* <span data-ttu-id="77ff1-144">Sloučí přijaté MDN s původní odchozí zprávy hello</span><span class="sxs-lookup"><span data-stu-id="77ff1-144">Reconciles a received MDN with hello original outbound message</span></span>
* <span data-ttu-id="77ff1-145">Aktualizace a korelaci záznamy v databázi nepopiratelnosti hello</span><span class="sxs-lookup"><span data-stu-id="77ff1-145">Updates and correlates records in hello non-repudiation database</span></span>
* <span data-ttu-id="77ff1-146">Zapíše záznamy pro generování sestav o stavu AS2</span><span class="sxs-lookup"><span data-stu-id="77ff1-146">Writes records for AS2 status reporting</span></span>
* <span data-ttu-id="77ff1-147">Hello výstupní datovou část obsahu jsou kódováním base64</span><span class="sxs-lookup"><span data-stu-id="77ff1-147">hello output payload contents are base64 encoded</span></span>
* <span data-ttu-id="77ff1-148">Určuje, jestli MDN je nutné a jestli hello MDN by měla být synchronní nebo asynchronní na základě konfigurace smlouvy AS2</span><span class="sxs-lookup"><span data-stu-id="77ff1-148">Determines whether an MDN is required, and whether hello MDN should be synchronous or asynchronous based on configuration in AS2 agreement</span></span>
* <span data-ttu-id="77ff1-149">Generuje synchronní nebo asynchronní MDN (podle konfigurace smlouvy)</span><span class="sxs-lookup"><span data-stu-id="77ff1-149">Generates a synchronous or asynchronous MDN (based on agreement configurations)</span></span>
* <span data-ttu-id="77ff1-150">Nastaví na hello MDN hello korelace tokeny a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="77ff1-150">Sets hello correlation tokens and properties on hello MDN</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="77ff1-151">Zkuste tuto ukázku</span><span class="sxs-lookup"><span data-stu-id="77ff1-151">Try this sample</span></span>

<span data-ttu-id="77ff1-152">tootry nasazení plně funkční logiku aplikace a ukázkové AS2 scénáři, najdete v části hello [AS2 šablona aplikace logiky a scénář](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="77ff1-152">tootry deploying a fully operational logic app and sample AS2 scenario, see hello [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="77ff1-153">Zobrazení hello swagger</span><span class="sxs-lookup"><span data-stu-id="77ff1-153">View hello swagger</span></span>
<span data-ttu-id="77ff1-154">V tématu hello [swagger podrobnosti](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="77ff1-154">See hello [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="77ff1-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77ff1-155">Next steps</span></span>
[<span data-ttu-id="77ff1-156">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="77ff1-156">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md) 

