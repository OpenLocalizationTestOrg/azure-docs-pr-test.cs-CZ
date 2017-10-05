---
title: "Dekódovat EDIFACT zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a generování potvrzení s dekodér zpráva EDIFACT v podniku integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e3787b48037360bf6066ddce2bacba6842213b2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="5d5d8-103">Dekódovat EDIFACT zprávy pro Azure Logic Apps s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="5d5d8-103">Decode EDIFACT messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="5d5d8-104">Konektor dekódovat EDIFACT zpráva může ověřit EDI a vlastnosti specifické pro partnery, rozdělení mimoúrovňové křižovatky do nastaví transakce nebo zachovat celý mimoúrovňové křižovatky a generovat potvrzování pro zpracování transakcí.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-104">With the Decode EDIFACT message connector, you can validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="5d5d8-105">Pokud chcete použít tento konektor, je nutné přidat konektor k existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="5d5d8-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5d5d8-106">Before you start</span></span>

<span data-ttu-id="5d5d8-107">Tady je položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="5d5d8-107">Here's the items you need:</span></span>

* <span data-ttu-id="5d5d8-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="5d5d8-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="5d5d8-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="5d5d8-110">Musí mít účet integrace k používání konektoru dekódovat EDIFACT zprávy.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-110">You must have an integration account to use the Decode EDIFACT message connector.</span></span> 
* <span data-ttu-id="5d5d8-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="5d5d8-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="5d5d8-112">[Smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="5d5d8-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="decode-edifact-messages"></a><span data-ttu-id="5d5d8-113">Dekódovat EDIFACT zprávy</span><span class="sxs-lookup"><span data-stu-id="5d5d8-113">Decode EDIFACT messages</span></span>

1. <span data-ttu-id="5d5d8-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="5d5d8-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="5d5d8-115">Konektor dekódovat EDIFACT zpráva nemá aktivačních událostí, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-115">The Decode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="5d5d8-116">V návrháři aplikace logiky přidejte aktivační událost a potom přidat akci do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3. <span data-ttu-id="5d5d8-117">Do vyhledávacího pole zadejte "EDIFACT" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-117">In the search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="5d5d8-118">Vyberte **dekódovat EDIFACT zpráva**.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-118">Select **Decode EDIFACT Message**.</span></span>
   
    ![hledání EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. <span data-ttu-id="5d5d8-120">Pokud jste nevytvořili dříve všechna připojení k vašemu účtu integrace, se zobrazí výzva k vytvoření připojení nyní.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="5d5d8-121">Název připojení a vyberte integrační účet, který chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-121">Name your connection, and select the integration account that you want to connect.</span></span>
   
    ![Vytvoření účtu integrace](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    <span data-ttu-id="5d5d8-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="5d5d8-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5d5d8-124">Property</span></span> | <span data-ttu-id="5d5d8-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="5d5d8-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="5d5d8-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="5d5d8-126">Connection Name *</span></span> |<span data-ttu-id="5d5d8-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="5d5d8-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="5d5d8-128">Integration Account *</span></span> |<span data-ttu-id="5d5d8-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-129">Enter a name for your integration account.</span></span> <span data-ttu-id="5d5d8-130">Ujistěte se, že integrace účet a logiku aplikace jsou ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

4. <span data-ttu-id="5d5d8-131">Po dokončení vytvoření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-131">When you're done to finish creating your connection, choose **Create**.</span></span> <span data-ttu-id="5d5d8-132">Podrobné informace o připojení by měl vypadat podobně jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="5d5d8-132">Your connection details should look similar to this example:</span></span>

    ![Podrobnosti účtu integrace](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. <span data-ttu-id="5d5d8-134">Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte zprávu plochý soubor EDIFACT dekódovat.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-134">After your connection is created, as shown in this example, select the EDIFACT flat file message to decode.</span></span>

    ![Integrace připojení účet vytvořili](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    <span data-ttu-id="5d5d8-136">Například:</span><span class="sxs-lookup"><span data-stu-id="5d5d8-136">For example:</span></span>

    ![Vyberte zprávu plochý soubor EDIFACT pro dekódování](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a><span data-ttu-id="5d5d8-138">Podrobnosti decoder EDIFACT</span><span class="sxs-lookup"><span data-stu-id="5d5d8-138">EDIFACT decoder details</span></span>

<span data-ttu-id="5d5d8-139">Konektor dekódovat EDIFACT provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="5d5d8-139">The Decode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="5d5d8-140">Ověří obálky proti obchodování smlouvu pro partnery.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-140">Validates the envelope against trading partner agreement.</span></span>
* <span data-ttu-id="5d5d8-141">Přeloží smlouvu porovnáním odesílatele kvalifikátor & identifikátor a příjemce kvalifikátor & identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-141">Resolves the agreement by matching the sender qualifier & identifier and receiver qualifier & identifier.</span></span>
* <span data-ttu-id="5d5d8-142">Rozdělí výměnu do více transakcí, když výměnu má více než jednu transakci na základě smlouvy na získat konfiguraci nastavení.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-142">Splits an interchange into multiple transactions when the interchange has more than one transaction based on the agreement's receive settings configuration.</span></span>
* <span data-ttu-id="5d5d8-143">Provede zpětný překlad výměnu.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-143">Disassembles the interchange.</span></span>
* <span data-ttu-id="5d5d8-144">Ověří EDI a vlastnosti specifické pro partnery, včetně:</span><span class="sxs-lookup"><span data-stu-id="5d5d8-144">Validates EDI and partner-specific properties including:</span></span>
  * <span data-ttu-id="5d5d8-145">Ověření struktury výměnu obálky</span><span class="sxs-lookup"><span data-stu-id="5d5d8-145">Validation of the interchange envelope structure</span></span>
  * <span data-ttu-id="5d5d8-146">Ověření schématu pro schéma řízení obálky</span><span class="sxs-lookup"><span data-stu-id="5d5d8-146">Schema validation of the envelope against the control schema</span></span>
  * <span data-ttu-id="5d5d8-147">Ověření schématu elementů transakce sady dat pro schéma zpráv</span><span class="sxs-lookup"><span data-stu-id="5d5d8-147">Schema validation of the transaction-set data elements against the message schema</span></span>
  * <span data-ttu-id="5d5d8-148">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-148">EDI validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="5d5d8-149">Ověří, že čísla řízení sadu výměnu, skupiny a transakce není duplicitní položky (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="5d5d8-149">Verifies that the interchange, group, and transaction set control numbers are not duplicates (if configured)</span></span> 
  * <span data-ttu-id="5d5d8-150">Ověří číslo výměnu řízení proti dříve přijaté mimoúrovňové křižovatky.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-150">Checks the interchange control number against previously received interchanges.</span></span> 
  * <span data-ttu-id="5d5d8-151">Ověří číslo skupiny ovládací prvek pro další skupiny řízení čísla v výměnu.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-151">Checks the group control number against other group control numbers in the interchange.</span></span> 
  * <span data-ttu-id="5d5d8-152">Ověří, že transakce nastavit počet ovládacího prvku na jiné transakci sadu řízení čísla v této skupině.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-152">Checks the transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="5d5d8-153">Rozdělí výměnu do nastaví transakce, nebo zachovává celý výměnu:</span><span class="sxs-lookup"><span data-stu-id="5d5d8-153">Splits the interchange into transaction sets, or preserves the entire interchange:</span></span>
  * <span data-ttu-id="5d5d8-154">Rozdělení výměnu jako sady transakce – pozastavení sady transakce Chyba: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-154">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="5d5d8-155">X12 dekódování akce výstupy nastaví pouze tyto transakce, který selže ověření do `badMessages`a výstupy nastaví zbývající transakce `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-155">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="5d5d8-156">Rozdělení výměnu jako sady transakce – pozastavení výměnu o chybě: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-156">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="5d5d8-157">Pokud jeden nebo více transakcí nastaví v výměnu nezdaří ověření, X12 dekódování akce výstupy nastaví všechny transakce v tomto výměnu k `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-157">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
  * <span data-ttu-id="5d5d8-158">Zachovat výměnu – pozastavení sady transakce o chybě: zachování výměnu a celý výměnu dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-158">Preserve Interchange - suspend transaction sets on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="5d5d8-159">X12 dekódování akce výstupy nastaví pouze tyto transakce, který selže ověření do `badMessages`a výstupy nastaví zbývající transakce `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-159">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="5d5d8-160">Zachovat výměnu – pozastavení výměnu o chybě: zachování výměnu a celý výměnu dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-160">Preserve Interchange - suspend interchange on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="5d5d8-161">Pokud jeden nebo více transakcí nastaví v výměnu nezdaří ověření, X12 dekódování akce výstupy nastaví všechny transakce v tomto výměnu k `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-161">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
* <span data-ttu-id="5d5d8-162">Generuje Technical (řízení) nebo funkční potvrzení (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="5d5d8-162">Generates a Technical (control) and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="5d5d8-163">Potvrzení o technické nebo CONTRL ACK hlásí výsledky syntaktické kontrolu dokončení přijaté výměnu.</span><span class="sxs-lookup"><span data-stu-id="5d5d8-163">A Technical Acknowledgment or the CONTRL ACK reports the results of a syntactical check of the complete received interchange.</span></span>
  * <span data-ttu-id="5d5d8-164">Potvrzení o funkční uznává přijmout nebo odmítnout přijaté výměnu nebo skupinu</span><span class="sxs-lookup"><span data-stu-id="5d5d8-164">A functional acknowledgment acknowledges accept or reject a received interchange or a group</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="5d5d8-165">Soubor Swagger zobrazení</span><span class="sxs-lookup"><span data-stu-id="5d5d8-165">View Swagger file</span></span>
<span data-ttu-id="5d5d8-166">Chcete-li zobrazit podrobnosti Swagger pro konektor EDIFACT, najdete v části [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="5d5d8-166">To view the Swagger details for the EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d5d8-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d5d8-167">Next steps</span></span>
[<span data-ttu-id="5d5d8-168">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="5d5d8-168">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

