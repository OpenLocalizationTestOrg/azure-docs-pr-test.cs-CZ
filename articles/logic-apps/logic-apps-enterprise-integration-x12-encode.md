---
title: "zprávy aaaEncode X12 – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a převést kódováním XML zprávy s X12 zprávy kodér v hello Enterprise integrační balíček pro Azure Logic Apps"
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
ms.openlocfilehash: 785dbd2c7c82463154732921e07e3586d2307840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="958ee-103">Kódování X12 zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="958ee-103">Encode X12 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="958ee-104">Hello kódovat X12 zpráva konektor může ověřit EDI a vlastnosti specifické pro partnery, převést kódováním XML zprávy EDI transakce sady v hello výměnu a vyžádat technických potvrzení, funkční potvrzení nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="958ee-104">With hello Encode X12 message connector, you can validate EDI and partner-specific properties, convert XML-encoded messages into EDI transaction sets in hello interchange, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="958ee-105">toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="958ee-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="958ee-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="958ee-106">Before you start</span></span>

<span data-ttu-id="958ee-107">Tady je hello položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="958ee-107">Here's hello items you need:</span></span>

* <span data-ttu-id="958ee-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="958ee-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="958ee-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="958ee-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="958ee-110">Musíte mít konektoru integrace účet toouse hello kódovat X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="958ee-110">You must have an integration account toouse hello Encode X12 message connector.</span></span>
* <span data-ttu-id="958ee-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="958ee-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="958ee-112">[X12 smlouvy](logic-apps-enterprise-integration-x12.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="958ee-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="encode-x12-messages"></a><span data-ttu-id="958ee-113">Kódování X12 zprávy</span><span class="sxs-lookup"><span data-stu-id="958ee-113">Encode X12 messages</span></span>

1. <span data-ttu-id="958ee-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="958ee-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="958ee-115">Hello kódovat X12 zpráva konektor nemá aktivačních událostí, proto musíte přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="958ee-115">hello Encode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="958ee-116">V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="958ee-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="958ee-117">Hello vyhledávacího pole zadejte "x12" filtru.</span><span class="sxs-lookup"><span data-stu-id="958ee-117">In hello search box, enter "x12" for your filter.</span></span> <span data-ttu-id="958ee-118">Vyberte buď **X12-kódování zpráv tooX12 podle názvu smlouvy** nebo **X12-kódování zpráv tooX12 podle identity**.</span><span class="sxs-lookup"><span data-stu-id="958ee-118">Select either **X12 - Encode tooX12 message by agreement name** or **X12 - Encode tooX12 message by identities**.</span></span>
   
    ![Vyhledejte "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. <span data-ttu-id="958ee-120">Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="958ee-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="958ee-121">Název připojení a vyberte účet hello integrace, které chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="958ee-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 
   
    ![účet připojení integrace](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    <span data-ttu-id="958ee-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="958ee-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="958ee-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="958ee-124">Property</span></span> | <span data-ttu-id="958ee-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="958ee-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="958ee-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="958ee-126">Connection Name *</span></span> |<span data-ttu-id="958ee-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="958ee-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="958ee-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="958ee-128">Integration Account *</span></span> |<span data-ttu-id="958ee-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="958ee-129">Enter a name for your integration account.</span></span> <span data-ttu-id="958ee-130">Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="958ee-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="958ee-131">Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis.</span><span class="sxs-lookup"><span data-stu-id="958ee-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="958ee-132">toofinish vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="958ee-132">toofinish creating your connection, choose **Create**.</span></span>

    ![Integrace připojení účet vytvořili](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    <span data-ttu-id="958ee-134">Připojení je nyní vytvořena.</span><span class="sxs-lookup"><span data-stu-id="958ee-134">Your connection is now created.</span></span>

    ![Podrobnosti připojení účtu integrace](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a><span data-ttu-id="958ee-136">Kódování X12 zprávy podle název smlouvy</span><span class="sxs-lookup"><span data-stu-id="958ee-136">Encode X12 messages by agreement name</span></span>

<span data-ttu-id="958ee-137">Pokud jste zvolili tooencode X12 zprávy název smlouvy, otevřete hello **název X12 smlouvy** seznamu, zadejte nebo vyberte vaše stávající X12 smlouvy.</span><span class="sxs-lookup"><span data-stu-id="958ee-137">If you chose tooencode X12 messages by agreement name, open hello **Name of X12 agreement** list, enter or select your existing X12 agreement.</span></span> <span data-ttu-id="958ee-138">Zadejte tooencode zprávy XML hello.</span><span class="sxs-lookup"><span data-stu-id="958ee-138">Enter hello XML message tooencode.</span></span>

![Zadejte X12 smlouvy, název a XML zprávy tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a><span data-ttu-id="958ee-140">Kódování X12 zprávy podle identity</span><span class="sxs-lookup"><span data-stu-id="958ee-140">Encode X12 messages by identities</span></span>

<span data-ttu-id="958ee-141">Pokud si zvolíte tooencode X12 zprávy pomocí identity, zadejte identifikátor odesílatele hello, kvalifikátor odesílatele, identifikátor příjemce a příjemce kvalifikátor jako nakonfigurovaný v vaší X12 smlouvy.</span><span class="sxs-lookup"><span data-stu-id="958ee-141">If you choose tooencode X12 messages by identities, enter hello sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your X12 agreement.</span></span> <span data-ttu-id="958ee-142">Vyberte tooencode zprávy XML hello.</span><span class="sxs-lookup"><span data-stu-id="958ee-142">Select hello XML message tooencode.</span></span>
   
![Zadejte identit pro odesílatele a příjemce, vyberte tooencode zprávy XML](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a><span data-ttu-id="958ee-144">X12 kódování podrobnosti</span><span class="sxs-lookup"><span data-stu-id="958ee-144">X12 Encode details</span></span>

<span data-ttu-id="958ee-145">konektor kódovat Hello X12 provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="958ee-145">hello X12 Encode connector performs these tasks:</span></span>

* <span data-ttu-id="958ee-146">Smlouva řešení pomocí odpovídajících vlastností kontextu odesílatele a příjemce.</span><span class="sxs-lookup"><span data-stu-id="958ee-146">Agreement resolution by matching sender and receiver context properties.</span></span>
* <span data-ttu-id="958ee-147">Serializuje hello EDI výměnu, převodu kódu XML zprávy EDI transakce sady v hello výměnu.</span><span class="sxs-lookup"><span data-stu-id="958ee-147">Serializes hello EDI interchange, converting XML-encoded messages into EDI transaction sets in hello interchange.</span></span>
* <span data-ttu-id="958ee-148">Použije sadu transakce hlavičku a přípojného segmenty</span><span class="sxs-lookup"><span data-stu-id="958ee-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="958ee-149">Generuje představuje počet řízení výměnu, číslem skupiny řízení a číslem transakce sadu ovládacího prvku pro každý odchozí výměnu</span><span class="sxs-lookup"><span data-stu-id="958ee-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="958ee-150">Nahradí oddělovače v datové části hello</span><span class="sxs-lookup"><span data-stu-id="958ee-150">Replaces separators in hello payload data</span></span>
* <span data-ttu-id="958ee-151">Ověří EDI a vlastnosti specifické pro partnery</span><span class="sxs-lookup"><span data-stu-id="958ee-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="958ee-152">Ověření schématu hello data transakce sady elementů na uvítací zprávu schématu</span><span class="sxs-lookup"><span data-stu-id="958ee-152">Schema validation of hello transaction-set data elements against hello message Schema</span></span>
  * <span data-ttu-id="958ee-153">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="958ee-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="958ee-154">Rozšířené ověření provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="958ee-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="958ee-155">Požádá o potvrzení o Technical nebo funkčnosti (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="958ee-155">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="958ee-156">Potvrzení o technické generuje v důsledku hlavičky ověření.</span><span class="sxs-lookup"><span data-stu-id="958ee-156">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="958ee-157">Hello technické potvrzení sestavy hello stav zpracování hello výměnu záhlaví a přípojného podle hello adresa příjemce</span><span class="sxs-lookup"><span data-stu-id="958ee-157">hello technical acknowledgment reports hello status of hello processing of an interchange header and trailer by hello address receiver</span></span>
  * <span data-ttu-id="958ee-158">Potvrzení o funkční generuje v důsledku ověření obsahu.</span><span class="sxs-lookup"><span data-stu-id="958ee-158">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="958ee-159">Hello funkční potvrzení sestavy každou při došlo k chybě zpracování hello přijatých dokumentu</span><span class="sxs-lookup"><span data-stu-id="958ee-159">hello functional acknowledgment reports each error encountered while processing hello received document</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="958ee-160">Zobrazení hello swagger</span><span class="sxs-lookup"><span data-stu-id="958ee-160">View hello swagger</span></span>
<span data-ttu-id="958ee-161">V tématu hello [swagger podrobnosti](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="958ee-161">See hello [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="958ee-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="958ee-162">Next steps</span></span>
[<span data-ttu-id="958ee-163">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="958ee-163">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

