---
title: "aaaDecode EDIFACT zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a generování potvrzení s decoder zpráva hello EDIFACT v hello Enterprise integrační balíček pro Azure Logic Apps"
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
ms.openlocfilehash: 94faebdec4e4ffc8ad76ad1609495ddf9f002250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="c95d5-103">Dekódovat EDIFACT zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="c95d5-103">Decode EDIFACT messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="c95d5-104">Hello dekódovat EDIFACT zpráva konektor může ověřit EDI a vlastnosti specifické pro partnery, rozdělení mimoúrovňové křižovatky do nastaví transakce nebo zachovat celý mimoúrovňové křižovatky a generovat potvrzování pro zpracování transakcí.</span><span class="sxs-lookup"><span data-stu-id="c95d5-104">With hello Decode EDIFACT message connector, you can validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="c95d5-105">toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c95d5-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="c95d5-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c95d5-106">Before you start</span></span>

<span data-ttu-id="c95d5-107">Tady je hello položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c95d5-107">Here's hello items you need:</span></span>

* <span data-ttu-id="c95d5-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="c95d5-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="c95d5-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="c95d5-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="c95d5-110">Musíte mít konektoru integrace účet toouse hello dekódovat EDIFACT zprávy.</span><span class="sxs-lookup"><span data-stu-id="c95d5-110">You must have an integration account toouse hello Decode EDIFACT message connector.</span></span> 
* <span data-ttu-id="c95d5-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c95d5-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="c95d5-112">[Smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c95d5-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="decode-edifact-messages"></a><span data-ttu-id="c95d5-113">Dekódovat EDIFACT zprávy</span><span class="sxs-lookup"><span data-stu-id="c95d5-113">Decode EDIFACT messages</span></span>

1. <span data-ttu-id="c95d5-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c95d5-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="c95d5-115">Hello dekódovat EDIFACT zpráva konektor nemá aktivačních událostí, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="c95d5-115">hello Decode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="c95d5-116">V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="c95d5-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3. <span data-ttu-id="c95d5-117">Hello vyhledávacího pole zadejte "EDIFACT" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="c95d5-117">In hello search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="c95d5-118">Vyberte **dekódovat EDIFACT zpráva**.</span><span class="sxs-lookup"><span data-stu-id="c95d5-118">Select **Decode EDIFACT Message**.</span></span>
   
    ![hledání EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. <span data-ttu-id="c95d5-120">Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="c95d5-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="c95d5-121">Název připojení a vyberte účet hello integrace, které chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="c95d5-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>
   
    ![Vytvoření účtu integrace](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    <span data-ttu-id="c95d5-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="c95d5-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="c95d5-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c95d5-124">Property</span></span> | <span data-ttu-id="c95d5-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c95d5-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="c95d5-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="c95d5-126">Connection Name *</span></span> |<span data-ttu-id="c95d5-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="c95d5-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="c95d5-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="c95d5-128">Integration Account *</span></span> |<span data-ttu-id="c95d5-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="c95d5-129">Enter a name for your integration account.</span></span> <span data-ttu-id="c95d5-130">Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="c95d5-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

4. <span data-ttu-id="c95d5-131">Po dokončení toofinish vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c95d5-131">When you're done toofinish creating your connection, choose **Create**.</span></span> <span data-ttu-id="c95d5-132">Podrobné informace o připojení by měl vypadat podobně jako příklad toothis:</span><span class="sxs-lookup"><span data-stu-id="c95d5-132">Your connection details should look similar toothis example:</span></span>

    ![Podrobnosti účtu integrace](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. <span data-ttu-id="c95d5-134">Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte hello EDIFACT plochý soubor zprávy toodecode.</span><span class="sxs-lookup"><span data-stu-id="c95d5-134">After your connection is created, as shown in this example, select hello EDIFACT flat file message toodecode.</span></span>

    ![Integrace připojení účet vytvořili](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    <span data-ttu-id="c95d5-136">Například:</span><span class="sxs-lookup"><span data-stu-id="c95d5-136">For example:</span></span>

    ![Vyberte zprávu plochý soubor EDIFACT pro dekódování](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a><span data-ttu-id="c95d5-138">Podrobnosti decoder EDIFACT</span><span class="sxs-lookup"><span data-stu-id="c95d5-138">EDIFACT decoder details</span></span>

<span data-ttu-id="c95d5-139">konektor dekódovat EDIFACT Hello provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="c95d5-139">hello Decode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="c95d5-140">Ověří hello obálky proti obchodování smlouvu pro partnery.</span><span class="sxs-lookup"><span data-stu-id="c95d5-140">Validates hello envelope against trading partner agreement.</span></span>
* <span data-ttu-id="c95d5-141">Přeloží hello smlouvy porovnáním hello odesílatele kvalifikátor & identifikátor a příjemce kvalifikátor & identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c95d5-141">Resolves hello agreement by matching hello sender qualifier & identifier and receiver qualifier & identifier.</span></span>
* <span data-ttu-id="c95d5-142">Rozdělí výměnu do více transakcí, když hello výměnu má více než jednu transakci na základě smlouvy hello získat konfiguraci nastavení.</span><span class="sxs-lookup"><span data-stu-id="c95d5-142">Splits an interchange into multiple transactions when hello interchange has more than one transaction based on hello agreement's receive settings configuration.</span></span>
* <span data-ttu-id="c95d5-143">Provede zpětný překlad hello výměnu.</span><span class="sxs-lookup"><span data-stu-id="c95d5-143">Disassembles hello interchange.</span></span>
* <span data-ttu-id="c95d5-144">Ověří EDI a vlastnosti specifické pro partnery, včetně:</span><span class="sxs-lookup"><span data-stu-id="c95d5-144">Validates EDI and partner-specific properties including:</span></span>
  * <span data-ttu-id="c95d5-145">Ověření struktury hello výměnu obálky</span><span class="sxs-lookup"><span data-stu-id="c95d5-145">Validation of hello interchange envelope structure</span></span>
  * <span data-ttu-id="c95d5-146">Ověření schématu hello obálky pro schéma řízení hello</span><span class="sxs-lookup"><span data-stu-id="c95d5-146">Schema validation of hello envelope against hello control schema</span></span>
  * <span data-ttu-id="c95d5-147">Ověření schématu elementů hello transakce sady dat pro schéma zprávy hello</span><span class="sxs-lookup"><span data-stu-id="c95d5-147">Schema validation of hello transaction-set data elements against hello message schema</span></span>
  * <span data-ttu-id="c95d5-148">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="c95d5-148">EDI validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="c95d5-149">Ověří, že hello výměnu, skupiny a transakce sadu řízení čísla není duplicitní položky (Pokud je nakonfigurováno)</span><span class="sxs-lookup"><span data-stu-id="c95d5-149">Verifies that hello interchange, group, and transaction set control numbers are not duplicates (if configured)</span></span> 
  * <span data-ttu-id="c95d5-150">Zkontroluje hello výměnu řízení číslo proti dříve přijaté mimoúrovňové křižovatky.</span><span class="sxs-lookup"><span data-stu-id="c95d5-150">Checks hello interchange control number against previously received interchanges.</span></span> 
  * <span data-ttu-id="c95d5-151">Zkontroluje hello skupiny řízení číslo pro jiné skupiny řízení čísel ve hello výměnu.</span><span class="sxs-lookup"><span data-stu-id="c95d5-151">Checks hello group control number against other group control numbers in hello interchange.</span></span> 
  * <span data-ttu-id="c95d5-152">Ověří, že transakce hello nastavit počet ovládacího prvku na jiné transakci sadu řízení čísla v této skupině.</span><span class="sxs-lookup"><span data-stu-id="c95d5-152">Checks hello transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="c95d5-153">Rozdělí hello výměnu do nastaví transakce, nebo zachovává celý výměnu hello:</span><span class="sxs-lookup"><span data-stu-id="c95d5-153">Splits hello interchange into transaction sets, or preserves hello entire interchange:</span></span>
  * <span data-ttu-id="c95d5-154">Rozdělení výměnu jako sady transakce – pozastavení sady transakce Chyba: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="c95d5-154">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="c95d5-155">Akce dekódování Hello X12 výstupy pouze transakce sady, které nesplní ověření příliš`badMessages`a výstupy hello zbývající transakce se nastaví příliš`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="c95d5-155">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="c95d5-156">Rozdělení výměnu jako sady transakce – pozastavení výměnu o chybě: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="c95d5-156">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="c95d5-157">Pokud jeden nebo více transakcí nastaví hello výměnu selhání ověřování, hello X12 dekódování akce vypíše všechny transakce hello nastaví v tomto výměnu příliš`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="c95d5-157">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
  * <span data-ttu-id="c95d5-158">Zachovat výměnu – pozastavení sady transakce o chybě: proces a zachovat hello výměnu hello celý dávkové výměnu.</span><span class="sxs-lookup"><span data-stu-id="c95d5-158">Preserve Interchange - suspend transaction sets on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="c95d5-159">Akce dekódování Hello X12 výstupy pouze transakce sady, které nesplní ověření příliš`badMessages`a výstupy hello zbývající transakce se nastaví příliš`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="c95d5-159">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="c95d5-160">Zachovat výměnu – pozastavení výměnu o chybě: proces a zachovat hello výměnu hello celý dávkové výměnu.</span><span class="sxs-lookup"><span data-stu-id="c95d5-160">Preserve Interchange - suspend interchange on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="c95d5-161">Pokud jeden nebo více transakcí nastaví hello výměnu selhání ověřování, hello X12 dekódování akce vypíše všechny transakce hello nastaví v tomto výměnu příliš`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="c95d5-161">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
* <span data-ttu-id="c95d5-162">Generuje Technical (řízení) nebo funkční potvrzení (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="c95d5-162">Generates a Technical (control) and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="c95d5-163">Technické potvrzení nebo hello CONTRL ACK hlásí výsledky hello syntaktické kontrolu výměnu hello dokončení přijata.</span><span class="sxs-lookup"><span data-stu-id="c95d5-163">A Technical Acknowledgment or hello CONTRL ACK reports hello results of a syntactical check of hello complete received interchange.</span></span>
  * <span data-ttu-id="c95d5-164">Potvrzení o funkční uznává přijmout nebo odmítnout přijaté výměnu nebo skupinu</span><span class="sxs-lookup"><span data-stu-id="c95d5-164">A functional acknowledgment acknowledges accept or reject a received interchange or a group</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="c95d5-165">Soubor Swagger zobrazení</span><span class="sxs-lookup"><span data-stu-id="c95d5-165">View Swagger file</span></span>
<span data-ttu-id="c95d5-166">Podrobnosti Swagger hello tooview hello EDIFACT konektor, najdete v tématu [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="c95d5-166">tooview hello Swagger details for hello EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c95d5-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c95d5-167">Next steps</span></span>
[<span data-ttu-id="c95d5-168">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="c95d5-168">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

