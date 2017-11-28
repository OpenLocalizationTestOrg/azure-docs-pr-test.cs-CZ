---
title: "Dekódovat X12 zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a generování potvrzení s X12 decoder zpráva v podniku integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 18719a8f49c74973947517161f7306c233a9323f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="68e0f-103">Dekódovat X12 zprávy pro Azure Logic Apps s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="68e0f-103">Decode X12 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="68e0f-104">S konektorem dekódování X12 zprávy můžete ověřit obálky proti smlouvy s obchodním partnerem, ověření EDI a vlastnosti specifické pro partnery, rozdělení mimoúrovňové křižovatky do nastaví transakce nebo zachovat celý mimoúrovňové křižovatky a generovat potvrzování pro zpracování transakcí.</span><span class="sxs-lookup"><span data-stu-id="68e0f-104">With the Decode X12 message connector, you can validate the envelope against a trading partner agreement, validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="68e0f-105">Pokud chcete použít tento konektor, je nutné přidat konektor k existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="68e0f-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="68e0f-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="68e0f-106">Before you start</span></span>

<span data-ttu-id="68e0f-107">Tady je položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="68e0f-107">Here's the items you need:</span></span>

* <span data-ttu-id="68e0f-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="68e0f-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="68e0f-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0f-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="68e0f-110">Musí mít účet integrace k používání konektoru dekódování X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="68e0f-110">You must have an integration account to use the Decode X12 message connector.</span></span>
* <span data-ttu-id="68e0f-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="68e0f-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="68e0f-112">[X12 smlouvy](logic-apps-enterprise-integration-x12.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="68e0f-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="decode-x12-messages"></a><span data-ttu-id="68e0f-113">Dekódovat X12 zprávy</span><span class="sxs-lookup"><span data-stu-id="68e0f-113">Decode X12 messages</span></span>

1. <span data-ttu-id="68e0f-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="68e0f-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="68e0f-115">Konektor dekódování X12 zpráva nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="68e0f-115">The Decode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="68e0f-116">V návrháři aplikace logiky přidejte aktivační událost a potom přidat akci do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="68e0f-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="68e0f-117">Do vyhledávacího pole zadejte "x12" filtru.</span><span class="sxs-lookup"><span data-stu-id="68e0f-117">In the search box, enter "x12" for your filter.</span></span> <span data-ttu-id="68e0f-118">Vyberte **X12-dekódovat X12 zpráva**.</span><span class="sxs-lookup"><span data-stu-id="68e0f-118">Select **X12 - Decode X12 message**.</span></span>
   
    ![Vyhledejte "x12"](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. <span data-ttu-id="68e0f-120">Pokud jste nevytvořili dříve všechna připojení k vašemu účtu integrace, se zobrazí výzva k vytvoření připojení nyní.</span><span class="sxs-lookup"><span data-stu-id="68e0f-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="68e0f-121">Název připojení a vyberte integrační účet, který chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="68e0f-121">Name your connection, and select the integration account that you want to connect.</span></span> 

    ![Zadejte podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    <span data-ttu-id="68e0f-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="68e0f-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="68e0f-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="68e0f-124">Property</span></span> | <span data-ttu-id="68e0f-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="68e0f-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="68e0f-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="68e0f-126">Connection Name *</span></span> |<span data-ttu-id="68e0f-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="68e0f-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="68e0f-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="68e0f-128">Integration Account *</span></span> |<span data-ttu-id="68e0f-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="68e0f-129">Enter a name for your integration account.</span></span> <span data-ttu-id="68e0f-130">Ujistěte se, že integrace účet a logiku aplikace jsou ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="68e0f-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="68e0f-131">Když jste hotovi, by měla vypadat podobně jako tento příklad podrobné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="68e0f-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="68e0f-132">Dokončete vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="68e0f-132">To finish creating your connection, choose **Create**.</span></span>
   
    ![Podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. <span data-ttu-id="68e0f-134">Po připojení se vytvoří, jak je znázorněno v tomto příkladu vyberte X12 plochý soubor zprávy dekódovat.</span><span class="sxs-lookup"><span data-stu-id="68e0f-134">After your connection is created, as shown in this example, select the X12 flat file message to decode.</span></span>

    ![Integrace připojení účet vytvořili](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    <span data-ttu-id="68e0f-136">Například:</span><span class="sxs-lookup"><span data-stu-id="68e0f-136">For example:</span></span>

    ![Vyberte X12 s plochou zpráva souboru pro dekódování](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a><span data-ttu-id="68e0f-138">X12 dekódovat podrobnosti</span><span class="sxs-lookup"><span data-stu-id="68e0f-138">X12 Decode details</span></span>

<span data-ttu-id="68e0f-139">X12 dekódování konektor provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="68e0f-139">The X12 Decode connector performs these tasks:</span></span>

* <span data-ttu-id="68e0f-140">Ověří obálky proti obchodování smlouvu pro partnery</span><span class="sxs-lookup"><span data-stu-id="68e0f-140">Validates the envelope against trading partner agreement</span></span>
* <span data-ttu-id="68e0f-141">Ověří EDI a vlastnosti specifické pro partnery</span><span class="sxs-lookup"><span data-stu-id="68e0f-141">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="68e0f-142">Ověření strukturální EDI a rozšířeného schématu ověření</span><span class="sxs-lookup"><span data-stu-id="68e0f-142">EDI structural validation, and extended schema validation</span></span>
  * <span data-ttu-id="68e0f-143">Ověření struktury obálky výměnu.</span><span class="sxs-lookup"><span data-stu-id="68e0f-143">Validation of the structure of the interchange envelope.</span></span>
  * <span data-ttu-id="68e0f-144">Ověření schématu obálky pro schéma řízení.</span><span class="sxs-lookup"><span data-stu-id="68e0f-144">Schema validation of the envelope against the control schema.</span></span>
  * <span data-ttu-id="68e0f-145">Ověření schématu elementů transakce sady dat pro schéma zprávy.</span><span class="sxs-lookup"><span data-stu-id="68e0f-145">Schema validation of the transaction-set data elements against the message schema.</span></span>
  * <span data-ttu-id="68e0f-146">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="68e0f-146">EDI validation performed on transaction-set data elements</span></span> 
* <span data-ttu-id="68e0f-147">Ověří, zda jsou čísla řízení sadu výměnu, skupiny a transakce není duplicitní položky</span><span class="sxs-lookup"><span data-stu-id="68e0f-147">Verifies that the interchange, group, and transaction set control numbers are not duplicates</span></span>
  * <span data-ttu-id="68e0f-148">Ověří číslo výměnu řízení proti dříve přijaté mimoúrovňové křižovatky.</span><span class="sxs-lookup"><span data-stu-id="68e0f-148">Checks the interchange control number against previously received interchanges.</span></span>
  * <span data-ttu-id="68e0f-149">Ověří číslo skupiny ovládací prvek pro další skupiny řízení čísla v výměnu.</span><span class="sxs-lookup"><span data-stu-id="68e0f-149">Checks the group control number against other group control numbers in the interchange.</span></span>
  * <span data-ttu-id="68e0f-150">Ověří, že transakce nastavit počet ovládacího prvku na jiné transakci sadu řízení čísla v této skupině.</span><span class="sxs-lookup"><span data-stu-id="68e0f-150">Checks the transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="68e0f-151">Rozdělí výměnu do nastaví transakce, nebo zachovává celý výměnu:</span><span class="sxs-lookup"><span data-stu-id="68e0f-151">Splits the interchange into transaction sets, or preserves the entire interchange:</span></span>
  * <span data-ttu-id="68e0f-152">Rozdělení výměnu jako sady transakce – pozastavení sady transakce Chyba: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="68e0f-152">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="68e0f-153">X12 dekódování akce výstupy nastaví pouze tyto transakce, který selže ověření do `badMessages`a výstupy nastaví zbývající transakce `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="68e0f-153">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="68e0f-154">Rozdělení výměnu jako sady transakce – pozastavení výměnu o chybě: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="68e0f-154">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="68e0f-155">Pokud jeden nebo více transakcí nastaví v výměnu nezdaří ověření, X12 dekódování akce výstupy nastaví všechny transakce v tomto výměnu k `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="68e0f-155">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
  * <span data-ttu-id="68e0f-156">Zachovat výměnu – pozastavení sady transakce o chybě: zachování výměnu a celý výměnu dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="68e0f-156">Preserve Interchange - suspend transaction sets on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="68e0f-157">X12 dekódování akce výstupy nastaví pouze tyto transakce, který selže ověření do `badMessages`a výstupy nastaví zbývající transakce `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="68e0f-157">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="68e0f-158">Zachovat výměnu – pozastavení výměnu o chybě: zachování výměnu a celý výměnu dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="68e0f-158">Preserve Interchange - suspend interchange on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="68e0f-159">Pokud jeden nebo více transakcí nastaví v výměnu nezdaří ověření, X12 dekódování akce výstupy nastaví všechny transakce v tomto výměnu k `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="68e0f-159">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span> 
* <span data-ttu-id="68e0f-160">Generuje Technical nebo funkčnosti potvrzení (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="68e0f-160">Generates a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="68e0f-161">Potvrzení o technické generuje v důsledku hlavičky ověření.</span><span class="sxs-lookup"><span data-stu-id="68e0f-161">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="68e0f-162">Technické potvrzení hlásí stav zpracování služby výměnu záhlaví a přípojného adresu příjemce.</span><span class="sxs-lookup"><span data-stu-id="68e0f-162">The technical acknowledgment reports the status of the processing of an interchange header and trailer by the address receiver.</span></span>
  * <span data-ttu-id="68e0f-163">Potvrzení o funkční generuje v důsledku ověření obsahu.</span><span class="sxs-lookup"><span data-stu-id="68e0f-163">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="68e0f-164">Funkční potvrzení sestavy každou došlo k chybě při zpracování přijatých dokumentu</span><span class="sxs-lookup"><span data-stu-id="68e0f-164">The functional acknowledgment reports each error encountered while processing the received document</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="68e0f-165">Zobrazení swagger</span><span class="sxs-lookup"><span data-stu-id="68e0f-165">View the swagger</span></span>
<span data-ttu-id="68e0f-166">Najdete v článku [swagger podrobnosti](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="68e0f-166">See the [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="68e0f-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68e0f-167">Next steps</span></span>
[<span data-ttu-id="68e0f-168">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="68e0f-168">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

