---
title: "Kódování X12 zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a převést kódováním XML zprávy s X12 zprávy kodér v podniku integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 29d19364b9a98e351c95f13e68a2e63b9f6439f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="aff0d-103">Kódování X12 zprávy pro Azure Logic Apps s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="aff0d-103">Encode X12 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="aff0d-104">Konektor kódovat X12 zpráva může ověřit EDI a vlastnosti specifické pro partnery, převést kódováním XML zprávy EDI transakce sady v výměnu a vyžádat technických potvrzení, funkční potvrzení nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="aff0d-104">With the Encode X12 message connector, you can validate EDI and partner-specific properties, convert XML-encoded messages into EDI transaction sets in the interchange, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="aff0d-105">Pokud chcete použít tento konektor, je nutné přidat konektor k existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="aff0d-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="aff0d-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="aff0d-106">Before you start</span></span>

<span data-ttu-id="aff0d-107">Tady je položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="aff0d-107">Here's the items you need:</span></span>

* <span data-ttu-id="aff0d-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="aff0d-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="aff0d-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="aff0d-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="aff0d-110">Musí mít účet integrace k používání konektoru kódovat X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="aff0d-110">You must have an integration account to use the Encode X12 message connector.</span></span>
* <span data-ttu-id="aff0d-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="aff0d-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="aff0d-112">[X12 smlouvy](logic-apps-enterprise-integration-x12.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="aff0d-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="encode-x12-messages"></a><span data-ttu-id="aff0d-113">Kódování X12 zprávy</span><span class="sxs-lookup"><span data-stu-id="aff0d-113">Encode X12 messages</span></span>

1. <span data-ttu-id="aff0d-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="aff0d-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="aff0d-115">Konektor kódovat X12 zpráva nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="aff0d-115">The Encode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="aff0d-116">V návrháři aplikace logiky přidejte aktivační událost a potom přidat akci do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="aff0d-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="aff0d-117">Do vyhledávacího pole zadejte "x12" filtru.</span><span class="sxs-lookup"><span data-stu-id="aff0d-117">In the search box, enter "x12" for your filter.</span></span> <span data-ttu-id="aff0d-118">Vyberte buď **X12-dekódovat X12 zprávu název smlouvy** nebo **X12-dekódovat X12 zprávu identity**.</span><span class="sxs-lookup"><span data-stu-id="aff0d-118">Select either **X12 - Encode to X12 message by agreement name** or **X12 - Encode to X12 message by identities**.</span></span>
   
    ![Vyhledejte "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. <span data-ttu-id="aff0d-120">Pokud jste nevytvořili dříve všechna připojení k vašemu účtu integrace, se zobrazí výzva k vytvoření připojení nyní.</span><span class="sxs-lookup"><span data-stu-id="aff0d-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="aff0d-121">Název připojení a vyberte integrační účet, který chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="aff0d-121">Name your connection, and select the integration account that you want to connect.</span></span> 
   
    ![účet připojení integrace](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    <span data-ttu-id="aff0d-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="aff0d-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="aff0d-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="aff0d-124">Property</span></span> | <span data-ttu-id="aff0d-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="aff0d-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="aff0d-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="aff0d-126">Connection Name *</span></span> |<span data-ttu-id="aff0d-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="aff0d-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="aff0d-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="aff0d-128">Integration Account *</span></span> |<span data-ttu-id="aff0d-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="aff0d-129">Enter a name for your integration account.</span></span> <span data-ttu-id="aff0d-130">Ujistěte se, že integrace účet a logiku aplikace jsou ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="aff0d-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="aff0d-131">Když jste hotovi, by měla vypadat podobně jako tento příklad podrobné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="aff0d-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="aff0d-132">Dokončete vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aff0d-132">To finish creating your connection, choose **Create**.</span></span>

    ![Integrace připojení účet vytvořili](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    <span data-ttu-id="aff0d-134">Připojení je nyní vytvořena.</span><span class="sxs-lookup"><span data-stu-id="aff0d-134">Your connection is now created.</span></span>

    ![Podrobnosti připojení účtu integrace](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a><span data-ttu-id="aff0d-136">Kódování X12 zprávy podle název smlouvy</span><span class="sxs-lookup"><span data-stu-id="aff0d-136">Encode X12 messages by agreement name</span></span>

<span data-ttu-id="aff0d-137">Pokud jste zvolili pro kódování X12 zprávy podle název smlouvy, otevřete **název X12 smlouvy** seznamu, zadejte nebo vyberte vaše stávající X12 smlouvy.</span><span class="sxs-lookup"><span data-stu-id="aff0d-137">If you chose to encode X12 messages by agreement name, open the **Name of X12 agreement** list, enter or select your existing X12 agreement.</span></span> <span data-ttu-id="aff0d-138">Zadejte zprávu XML ke kódování.</span><span class="sxs-lookup"><span data-stu-id="aff0d-138">Enter the XML message to encode.</span></span>

![Zadejte X12 název smlouvy a zpráva XML ke kódování](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a><span data-ttu-id="aff0d-140">Kódování X12 zprávy podle identity</span><span class="sxs-lookup"><span data-stu-id="aff0d-140">Encode X12 messages by identities</span></span>

<span data-ttu-id="aff0d-141">Pokud zvolíte možnost kódování X12 zprávy podle identity, zadejte identifikátor odesílatele, kvalifikátor odesílatel, příjemce identifikátor a příjemce kvalifikátor jako nakonfigurovaný v vaší X12 smlouvy.</span><span class="sxs-lookup"><span data-stu-id="aff0d-141">If you choose to encode X12 messages by identities, enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your X12 agreement.</span></span> <span data-ttu-id="aff0d-142">Vyberte zprávy XML ke kódování.</span><span class="sxs-lookup"><span data-stu-id="aff0d-142">Select the XML message to encode.</span></span>
   
![Zadejte identit pro odesílatele a příjemce, vyberte zprávy XML ke kódování](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a><span data-ttu-id="aff0d-144">X12 kódování podrobnosti</span><span class="sxs-lookup"><span data-stu-id="aff0d-144">X12 Encode details</span></span>

<span data-ttu-id="aff0d-145">X12 kódovat konektor provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="aff0d-145">The X12 Encode connector performs these tasks:</span></span>

* <span data-ttu-id="aff0d-146">Smlouva řešení pomocí odpovídajících vlastností kontextu odesílatele a příjemce.</span><span class="sxs-lookup"><span data-stu-id="aff0d-146">Agreement resolution by matching sender and receiver context properties.</span></span>
* <span data-ttu-id="aff0d-147">Serializuje výměnu EDI, převodu kódu XML zprávy EDI transakce sady v výměnu.</span><span class="sxs-lookup"><span data-stu-id="aff0d-147">Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.</span></span>
* <span data-ttu-id="aff0d-148">Použije sadu transakce hlavičku a přípojného segmenty</span><span class="sxs-lookup"><span data-stu-id="aff0d-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="aff0d-149">Generuje představuje počet řízení výměnu, číslem skupiny řízení a číslem transakce sadu ovládacího prvku pro každý odchozí výměnu</span><span class="sxs-lookup"><span data-stu-id="aff0d-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="aff0d-150">Nahradí oddělovače v datové části dat</span><span class="sxs-lookup"><span data-stu-id="aff0d-150">Replaces separators in the payload data</span></span>
* <span data-ttu-id="aff0d-151">Ověří EDI a vlastnosti specifické pro partnery</span><span class="sxs-lookup"><span data-stu-id="aff0d-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="aff0d-152">Ověření schématu elementů transakce sady dat proti zpráva schématu</span><span class="sxs-lookup"><span data-stu-id="aff0d-152">Schema validation of the transaction-set data elements against the message Schema</span></span>
  * <span data-ttu-id="aff0d-153">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="aff0d-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="aff0d-154">Rozšířené ověření provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="aff0d-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="aff0d-155">Požádá o potvrzení o Technical nebo funkčnosti (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="aff0d-155">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="aff0d-156">Potvrzení o technické generuje v důsledku hlavičky ověření.</span><span class="sxs-lookup"><span data-stu-id="aff0d-156">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="aff0d-157">Technické potvrzení hlásí stav zpracování služby výměnu záhlaví a přípojného adresa příjemce</span><span class="sxs-lookup"><span data-stu-id="aff0d-157">The technical acknowledgment reports the status of the processing of an interchange header and trailer by the address receiver</span></span>
  * <span data-ttu-id="aff0d-158">Potvrzení o funkční generuje v důsledku ověření obsahu.</span><span class="sxs-lookup"><span data-stu-id="aff0d-158">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="aff0d-159">Funkční potvrzení sestavy každou došlo k chybě při zpracování přijatých dokumentu</span><span class="sxs-lookup"><span data-stu-id="aff0d-159">The functional acknowledgment reports each error encountered while processing the received document</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="aff0d-160">Zobrazení swagger</span><span class="sxs-lookup"><span data-stu-id="aff0d-160">View the swagger</span></span>
<span data-ttu-id="aff0d-161">Najdete v článku [swagger podrobnosti](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="aff0d-161">See the [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="aff0d-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aff0d-162">Next steps</span></span>
[<span data-ttu-id="aff0d-163">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="aff0d-163">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

