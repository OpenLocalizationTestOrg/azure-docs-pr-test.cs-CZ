---
title: "Dekódovat AS2 zprávy – Azure Logic Apps | Microsoft Docs"
description: "Jak používat dekodér AS2 v podniku integrační balíček pro Azure Logic Apps"
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
ms.openlocfilehash: a7920b2509fe368c6f7d55e17fe0bf0020c4562c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="bd4bb-103">Dekódovat AS2 zprávy pro Azure Logic Apps s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="bd4bb-103">Decode AS2 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span> 

<span data-ttu-id="bd4bb-104">K vytvoření zabezpečení a spolehlivost při přenosu zprávy, pomocí konektoru zpráva dekódovat AS2.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-104">To establish security and reliability while transmitting messages, use the Decode AS2 message connector.</span></span> <span data-ttu-id="bd4bb-105">Tento konektor poskytuje digitální podpis, dešifrování a potvrzení prostřednictvím zpráv dispozice oznámení (MDN).</span><span class="sxs-lookup"><span data-stu-id="bd4bb-105">This connector provides digital signing, decryption, and acknowledgements through Message Disposition Notifications (MDN).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="bd4bb-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="bd4bb-106">Before you start</span></span>

<span data-ttu-id="bd4bb-107">Tady je položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="bd4bb-107">Here's the items you need:</span></span>

* <span data-ttu-id="bd4bb-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="bd4bb-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="bd4bb-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="bd4bb-110">Musí mít účet integrace k používání konektoru zpráva dekódovat AS2.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-110">You must have an integration account to use the Decode AS2 message connector.</span></span>
* <span data-ttu-id="bd4bb-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="bd4bb-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="bd4bb-112">[Smlouvy AS2](logic-apps-enterprise-integration-as2.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="bd4bb-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="decode-as2-messages"></a><span data-ttu-id="bd4bb-113">Dekódovat AS2 zprávy</span><span class="sxs-lookup"><span data-stu-id="bd4bb-113">Decode AS2 messages</span></span>

1. <span data-ttu-id="bd4bb-114">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="bd4bb-114">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="bd4bb-115">Konektor dekódovat AS2 zpráva nemá aktivačních událostí, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-115">The Decode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="bd4bb-116">V návrháři aplikace logiky přidejte aktivační událost a potom přidat akci do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="bd4bb-117">Do vyhledávacího pole zadejte "AS2" filtru.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-117">In the search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="bd4bb-118">Vyberte **AS2 - dekódovat AS2 zpráva**.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-118">Select **AS2 - Decode AS2 message**.</span></span>
   
    ![Vyhledejte "AS2"](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. <span data-ttu-id="bd4bb-120">Pokud jste nevytvořili dříve všechna připojení k vašemu účtu integrace, se zobrazí výzva k vytvoření připojení nyní.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="bd4bb-121">Název připojení a vyberte integrační účet, který chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-121">Name your connection, and select the integration account that you want to connect.</span></span>
   
    ![Vytvoření připojení integrace](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    <span data-ttu-id="bd4bb-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="bd4bb-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bd4bb-124">Property</span></span> | <span data-ttu-id="bd4bb-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="bd4bb-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="bd4bb-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="bd4bb-126">Connection Name *</span></span> |<span data-ttu-id="bd4bb-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="bd4bb-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="bd4bb-128">Integration Account *</span></span> |<span data-ttu-id="bd4bb-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-129">Enter a name for your integration account.</span></span> <span data-ttu-id="bd4bb-130">Ujistěte se, že integrace účet a logiku aplikace jsou ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="bd4bb-131">Když jste hotovi, by měla vypadat podobně jako tento příklad podrobné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="bd4bb-132">Dokončete vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-132">To finish creating your connection, choose **Create**.</span></span>

    ![Podrobné informace o integraci připojení](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. <span data-ttu-id="bd4bb-134">Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte **textu** a **hlavičky** z výstupů požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd4bb-134">After your connection is created, as shown in this example, select **Body** and **Headers** from the Request outputs.</span></span>
   
    ![Vytvoření připojení integrace](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    <span data-ttu-id="bd4bb-136">Například:</span><span class="sxs-lookup"><span data-stu-id="bd4bb-136">For example:</span></span>

    ![Vyberte textu a hlaviček ze žádosti výstupy](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a><span data-ttu-id="bd4bb-138">Podrobnosti decoder AS2</span><span class="sxs-lookup"><span data-stu-id="bd4bb-138">AS2 decoder details</span></span>

<span data-ttu-id="bd4bb-139">Konektor dekódovat AS2 provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="bd4bb-139">The Decode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="bd4bb-140">Zpracuje hlavičky AS2/HTTP</span><span class="sxs-lookup"><span data-stu-id="bd4bb-140">Processes AS2/HTTP headers</span></span>
* <span data-ttu-id="bd4bb-141">Ověří podpis (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="bd4bb-141">Verifies the signature (if configured)</span></span>
* <span data-ttu-id="bd4bb-142">Dešifruje zprávy (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="bd4bb-142">Decrypts the messages (if configured)</span></span>
* <span data-ttu-id="bd4bb-143">Dekomprimuje zprávy (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="bd4bb-143">Decompresses the message (if configured)</span></span>
* <span data-ttu-id="bd4bb-144">Sloučí přijaté MDN s původní odchozí zprávy</span><span class="sxs-lookup"><span data-stu-id="bd4bb-144">Reconciles a received MDN with the original outbound message</span></span>
* <span data-ttu-id="bd4bb-145">Aktualizace a záznamy v databázi nepopiratelnosti korelaci</span><span class="sxs-lookup"><span data-stu-id="bd4bb-145">Updates and correlates records in the non-repudiation database</span></span>
* <span data-ttu-id="bd4bb-146">Zapíše záznamy pro generování sestav o stavu AS2</span><span class="sxs-lookup"><span data-stu-id="bd4bb-146">Writes records for AS2 status reporting</span></span>
* <span data-ttu-id="bd4bb-147">Výstupní datové části obsahu jsou kódováním base64</span><span class="sxs-lookup"><span data-stu-id="bd4bb-147">The output payload contents are base64 encoded</span></span>
* <span data-ttu-id="bd4bb-148">Určuje, jestli MDN je nutné a jestli MDN by měla být synchronní nebo asynchronní na základě konfigurace smlouvy AS2</span><span class="sxs-lookup"><span data-stu-id="bd4bb-148">Determines whether an MDN is required, and whether the MDN should be synchronous or asynchronous based on configuration in AS2 agreement</span></span>
* <span data-ttu-id="bd4bb-149">Generuje synchronní nebo asynchronní MDN (podle konfigurace smlouvy)</span><span class="sxs-lookup"><span data-stu-id="bd4bb-149">Generates a synchronous or asynchronous MDN (based on agreement configurations)</span></span>
* <span data-ttu-id="bd4bb-150">Nastaví na MDN korelace tokeny a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bd4bb-150">Sets the correlation tokens and properties on the MDN</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="bd4bb-151">Zkuste tuto ukázku</span><span class="sxs-lookup"><span data-stu-id="bd4bb-151">Try this sample</span></span>

<span data-ttu-id="bd4bb-152">Pokud chcete vyzkoušet nasazení plně funkční logiku aplikace a ukázkový scénář AS2, najdete v článku [AS2 šablona aplikace logiky a scénář](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="bd4bb-152">To try deploying a fully operational logic app and sample AS2 scenario, see the [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="bd4bb-153">Zobrazení swagger</span><span class="sxs-lookup"><span data-stu-id="bd4bb-153">View the swagger</span></span>
<span data-ttu-id="bd4bb-154">Najdete v článku [swagger podrobnosti](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="bd4bb-154">See the [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="bd4bb-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd4bb-155">Next steps</span></span>
[<span data-ttu-id="bd4bb-156">Další informace o Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="bd4bb-156">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md) 

