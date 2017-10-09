---
title: "zprávy aaaDecode X12 – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a generování potvrzení s hello X12 zpráva decoder v hello Enterprise integrační balíček pro Azure Logic Apps"
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
ms.openlocfilehash: 1ffececca1ff835b319b64c85f86c421395833c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="9bdf3-103">Dekódovat X12 zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="9bdf3-103">Decode X12 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="9bdf3-104">S hello dekódování X12 zpráva konektor můžete ověřit hello obálky proti smlouvy s obchodním partnerem, ověření EDI a vlastnosti specifické pro partnery, rozdělení mimoúrovňové křižovatky do nastaví transakce nebo zachovat celý mimoúrovňové křižovatky a generovat potvrzování pro zpracování transakcí.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-104">With hello Decode X12 message connector, you can validate hello envelope against a trading partner agreement, validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="9bdf3-105">toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="9bdf3-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9bdf3-106">Before you start</span></span>

<span data-ttu-id="9bdf3-107">Tady je hello položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9bdf3-107">Here's hello items you need:</span></span>

* <span data-ttu-id="9bdf3-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="9bdf3-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="9bdf3-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="9bdf3-110">Musíte mít konektoru integrace účet toouse hello dekódování X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-110">You must have an integration account toouse hello Decode X12 message connector.</span></span>
* <span data-ttu-id="9bdf3-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="9bdf3-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="9bdf3-112">[X12 smlouvy](logic-apps-enterprise-integration-x12.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="9bdf3-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="decode-x12-messages"></a><span data-ttu-id="9bdf3-113">Dekódovat X12 zprávy</span><span class="sxs-lookup"><span data-stu-id="9bdf3-113">Decode X12 messages</span></span>

1. <span data-ttu-id="9bdf3-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9bdf3-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="9bdf3-115">Hello dekódování X12 zpráva konektor nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-115">hello Decode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="9bdf3-116">V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="9bdf3-117">Hello vyhledávacího pole zadejte "x12" filtru.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-117">In hello search box, enter "x12" for your filter.</span></span> <span data-ttu-id="9bdf3-118">Vyberte **X12-dekódovat X12 zpráva**.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-118">Select **X12 - Decode X12 message**.</span></span>
   
    ![Vyhledejte "x12"](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. <span data-ttu-id="9bdf3-120">Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="9bdf3-121">Název připojení a vyberte účet hello integrace, které chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 

    ![Zadejte podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    <span data-ttu-id="9bdf3-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="9bdf3-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9bdf3-124">Property</span></span> | <span data-ttu-id="9bdf3-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9bdf3-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="9bdf3-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="9bdf3-126">Connection Name *</span></span> |<span data-ttu-id="9bdf3-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="9bdf3-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="9bdf3-128">Integration Account *</span></span> |<span data-ttu-id="9bdf3-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-129">Enter a name for your integration account.</span></span> <span data-ttu-id="9bdf3-130">Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="9bdf3-131">Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="9bdf3-132">toofinish vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-132">toofinish creating your connection, choose **Create**.</span></span>
   
    ![Podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. <span data-ttu-id="9bdf3-134">Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte hello X12 plochý soubor zprávy toodecode.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-134">After your connection is created, as shown in this example, select hello X12 flat file message toodecode.</span></span>

    ![Integrace připojení účet vytvořili](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    <span data-ttu-id="9bdf3-136">Například:</span><span class="sxs-lookup"><span data-stu-id="9bdf3-136">For example:</span></span>

    ![Vyberte X12 s plochou zpráva souboru pro dekódování](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a><span data-ttu-id="9bdf3-138">X12 dekódovat podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9bdf3-138">X12 Decode details</span></span>

<span data-ttu-id="9bdf3-139">konektor dekódování Hello X12 provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="9bdf3-139">hello X12 Decode connector performs these tasks:</span></span>

* <span data-ttu-id="9bdf3-140">Ověří hello obálky proti obchodování smlouvu pro partnery</span><span class="sxs-lookup"><span data-stu-id="9bdf3-140">Validates hello envelope against trading partner agreement</span></span>
* <span data-ttu-id="9bdf3-141">Ověří EDI a vlastnosti specifické pro partnery</span><span class="sxs-lookup"><span data-stu-id="9bdf3-141">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="9bdf3-142">Ověření strukturální EDI a rozšířeného schématu ověření</span><span class="sxs-lookup"><span data-stu-id="9bdf3-142">EDI structural validation, and extended schema validation</span></span>
  * <span data-ttu-id="9bdf3-143">Ověření struktury hello obálky výměnu hello.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-143">Validation of hello structure of hello interchange envelope.</span></span>
  * <span data-ttu-id="9bdf3-144">Ověření schématu hello obálky pro schéma řízení hello.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-144">Schema validation of hello envelope against hello control schema.</span></span>
  * <span data-ttu-id="9bdf3-145">Ověření schématu elementů hello transakce sady dat pro schéma zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-145">Schema validation of hello transaction-set data elements against hello message schema.</span></span>
  * <span data-ttu-id="9bdf3-146">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-146">EDI validation performed on transaction-set data elements</span></span> 
* <span data-ttu-id="9bdf3-147">Ověřuje, že hello výměnu, skupiny a transakce sadu řízení čísla nejsou duplicitní</span><span class="sxs-lookup"><span data-stu-id="9bdf3-147">Verifies that hello interchange, group, and transaction set control numbers are not duplicates</span></span>
  * <span data-ttu-id="9bdf3-148">Zkontroluje hello výměnu řízení číslo proti dříve přijaté mimoúrovňové křižovatky.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-148">Checks hello interchange control number against previously received interchanges.</span></span>
  * <span data-ttu-id="9bdf3-149">Zkontroluje hello skupiny řízení číslo pro jiné skupiny řízení čísel ve hello výměnu.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-149">Checks hello group control number against other group control numbers in hello interchange.</span></span>
  * <span data-ttu-id="9bdf3-150">Ověří, že transakce hello nastavit počet ovládacího prvku na jiné transakci sadu řízení čísla v této skupině.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-150">Checks hello transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="9bdf3-151">Rozdělí hello výměnu do nastaví transakce, nebo zachovává celý výměnu hello:</span><span class="sxs-lookup"><span data-stu-id="9bdf3-151">Splits hello interchange into transaction sets, or preserves hello entire interchange:</span></span>
  * <span data-ttu-id="9bdf3-152">Rozdělení výměnu jako sady transakce – pozastavení sady transakce Chyba: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-152">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="9bdf3-153">Akce dekódování Hello X12 výstupy pouze transakce sady, které nesplní ověření příliš`badMessages`a výstupy hello zbývající transakce se nastaví příliš`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-153">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="9bdf3-154">Rozdělení výměnu jako sady transakce – pozastavení výměnu o chybě: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-154">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="9bdf3-155">Pokud jeden nebo více transakcí nastaví hello výměnu selhání ověřování, hello X12 dekódování akce vypíše všechny transakce hello nastaví v tomto výměnu příliš`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-155">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
  * <span data-ttu-id="9bdf3-156">Zachovat výměnu – pozastavení sady transakce o chybě: proces a zachovat hello výměnu hello celý dávkové výměnu.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-156">Preserve Interchange - suspend transaction sets on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="9bdf3-157">Akce dekódování Hello X12 výstupy pouze transakce sady, které nesplní ověření příliš`badMessages`a výstupy hello zbývající transakce se nastaví příliš`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-157">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="9bdf3-158">Zachovat výměnu – pozastavení výměnu o chybě: proces a zachovat hello výměnu hello celý dávkové výměnu.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-158">Preserve Interchange - suspend interchange on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="9bdf3-159">Pokud jeden nebo více transakcí nastaví hello výměnu selhání ověřování, hello X12 dekódování akce vypíše všechny transakce hello nastaví v tomto výměnu příliš`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-159">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span> 
* <span data-ttu-id="9bdf3-160">Generuje Technical nebo funkčnosti potvrzení (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="9bdf3-160">Generates a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="9bdf3-161">Potvrzení o technické generuje v důsledku hlavičky ověření.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-161">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="9bdf3-162">Hello technické potvrzení sestavy hello stav zpracování hello výměnu záhlaví a přípojného podle hello adresu příjemce.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-162">hello technical acknowledgment reports hello status of hello processing of an interchange header and trailer by hello address receiver.</span></span>
  * <span data-ttu-id="9bdf3-163">Potvrzení o funkční generuje v důsledku ověření obsahu.</span><span class="sxs-lookup"><span data-stu-id="9bdf3-163">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="9bdf3-164">Hello funkční potvrzení sestavy každou při došlo k chybě zpracování hello přijatých dokumentu</span><span class="sxs-lookup"><span data-stu-id="9bdf3-164">hello functional acknowledgment reports each error encountered while processing hello received document</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="9bdf3-165">Zobrazení hello swagger</span><span class="sxs-lookup"><span data-stu-id="9bdf3-165">View hello swagger</span></span>
<span data-ttu-id="9bdf3-166">V tématu hello [swagger podrobnosti](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="9bdf3-166">See hello [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9bdf3-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bdf3-167">Next steps</span></span>
[<span data-ttu-id="9bdf3-168">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="9bdf3-168">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

