---
title: "aaaEncode EDIFACT zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a vygenerování XML pomocí kodéru zpráv EDIFACT v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b3799dbd2492adef597022d017cf28f5ceb1085c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="f0509-103">Kódování zprávy EDIFACT pro Azure Logic Apps s hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="f0509-103">Encode EDIFACT messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="f0509-104">Hello EDIFACT kódování zpráv konektor může ověřit EDI a vlastnosti specifické pro partnery, Generovat dokument XML pro každou sadu transakce a vyžádat technických potvrzení, funkční potvrzení nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="f0509-104">With hello Encode EDIFACT message connector, you can validate EDI and partner-specific properties, generate an XML document for each transaction set, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="f0509-105">toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="f0509-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="f0509-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f0509-106">Before you start</span></span>

<span data-ttu-id="f0509-107">Tady je hello položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f0509-107">Here's hello items you need:</span></span>

* <span data-ttu-id="f0509-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="f0509-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="f0509-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="f0509-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="f0509-110">Musíte mít konektoru integrace účet toouse hello EDIFACT kódování zprávy.</span><span class="sxs-lookup"><span data-stu-id="f0509-110">You must have an integration account toouse hello Encode EDIFACT message connector.</span></span> 
* <span data-ttu-id="f0509-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="f0509-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="f0509-112">[Smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="f0509-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="encode-edifact-messages"></a><span data-ttu-id="f0509-113">Kódování zprávy EDIFACT</span><span class="sxs-lookup"><span data-stu-id="f0509-113">Encode EDIFACT messages</span></span>

1. <span data-ttu-id="f0509-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f0509-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="f0509-115">Hello EDIFACT kódování zpráv konektor nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="f0509-115">hello Encode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="f0509-116">V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="f0509-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="f0509-117">Hello vyhledávacího pole zadejte "EDIFACT" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="f0509-117">In hello search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="f0509-118">Vyberte buď **zakódovat zprávu EDIFACT název smlouvy** nebo **kódovat tooEDIFACT zprávu identity**.</span><span class="sxs-lookup"><span data-stu-id="f0509-118">Select either **Encode EDIFACT Message by agreement name** or **Encode tooEDIFACT message by identities**.</span></span>
   
    ![hledání EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. <span data-ttu-id="f0509-120">Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="f0509-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="f0509-121">Název připojení a vyberte účet hello integrace, které chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="f0509-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>

    ![Vytvoření připojení účtu integrace](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    <span data-ttu-id="f0509-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="f0509-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="f0509-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f0509-124">Property</span></span> | <span data-ttu-id="f0509-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="f0509-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="f0509-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="f0509-126">Connection Name *</span></span> |<span data-ttu-id="f0509-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="f0509-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="f0509-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="f0509-128">Integration Account *</span></span> |<span data-ttu-id="f0509-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="f0509-129">Enter a name for your integration account.</span></span> <span data-ttu-id="f0509-130">Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="f0509-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="f0509-131">Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis.</span><span class="sxs-lookup"><span data-stu-id="f0509-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="f0509-132">toofinish vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f0509-132">toofinish creating your connection, choose **Create**.</span></span>

    ![Podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    <span data-ttu-id="f0509-134">Připojení je nyní vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f0509-134">Your connection is now created.</span></span>

    ![Integrace připojení účet vytvořili](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a><span data-ttu-id="f0509-136">Kódování zprávy EDIFACT podle název smlouvy</span><span class="sxs-lookup"><span data-stu-id="f0509-136">Encode EDIFACT Message by agreement name</span></span>

<span data-ttu-id="f0509-137">Pokud jste zvolili tooencode EDIFACT zprávy pomocí název smlouvy, otevřete hello **název EDIFACT smlouvy** seznamu, zadejte nebo vyberte název smlouvy EDIFACT.</span><span class="sxs-lookup"><span data-stu-id="f0509-137">If you chose tooencode EDIFACT messages by agreement name, open hello **Name of EDIFACT agreement** list, enter or select your EDIFACT agreement name.</span></span> <span data-ttu-id="f0509-138">Zadejte tooencode zprávy XML hello.</span><span class="sxs-lookup"><span data-stu-id="f0509-138">Enter hello XML message tooencode.</span></span>

![Zadejte název smlouvy EDIFACT a tooencode zprávy XML](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a><span data-ttu-id="f0509-140">Kódování zprávy EDIFACT podle identity</span><span class="sxs-lookup"><span data-stu-id="f0509-140">Encode EDIFACT Message by identities</span></span>

<span data-ttu-id="f0509-141">Pokud si zvolíte tooencode EDIFACT zprávy pomocí identity, zadejte identifikátor odesílatele hello, kvalifikátor odesílatel, příjemce identifikátor a příjemce kvalifikátor jako nakonfigurovaný v vaší smlouvy EDIFACT.</span><span class="sxs-lookup"><span data-stu-id="f0509-141">If you choose tooencode EDIFACT messages by identities, enter hello sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your EDIFACT agreement.</span></span> <span data-ttu-id="f0509-142">Vyberte tooencode zprávy XML hello.</span><span class="sxs-lookup"><span data-stu-id="f0509-142">Select hello XML message tooencode.</span></span>

![Zadejte identit pro odesílatele a příjemce, vyberte tooencode zprávy XML](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a><span data-ttu-id="f0509-144">Kódování EDIFACT podrobnosti</span><span class="sxs-lookup"><span data-stu-id="f0509-144">EDIFACT Encode details</span></span>

<span data-ttu-id="f0509-145">konektor kódování EDIFACT Hello provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="f0509-145">hello Encode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="f0509-146">Hello smlouvu můžete vyřešit odpovídající hello odesílatele kvalifikátor & identifikátor a kvalifikátor příjemce a identifikátor</span><span class="sxs-lookup"><span data-stu-id="f0509-146">Resolve hello agreement by matching hello sender qualifier & identifier and receiver qualifier and identifier</span></span>
* <span data-ttu-id="f0509-147">Serializuje hello EDI výměnu, převodu kódu XML zprávy EDI transakce sady v hello výměnu.</span><span class="sxs-lookup"><span data-stu-id="f0509-147">Serializes hello EDI interchange, converting XML-encoded messages into EDI transaction sets in hello interchange.</span></span>
* <span data-ttu-id="f0509-148">Použije sadu transakce hlavičku a přípojného segmenty</span><span class="sxs-lookup"><span data-stu-id="f0509-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="f0509-149">Generuje představuje počet řízení výměnu, číslem skupiny řízení a číslem transakce sadu ovládacího prvku pro každý odchozí výměnu</span><span class="sxs-lookup"><span data-stu-id="f0509-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="f0509-150">Nahradí oddělovače v datové části hello</span><span class="sxs-lookup"><span data-stu-id="f0509-150">Replaces separators in hello payload data</span></span>
* <span data-ttu-id="f0509-151">Ověří EDI a vlastnosti specifické pro partnery</span><span class="sxs-lookup"><span data-stu-id="f0509-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="f0509-152">Ověření schématu elementů hello transakce sady dat pro schéma zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="f0509-152">Schema validation of hello transaction-set data elements against hello message schema.</span></span>
  * <span data-ttu-id="f0509-153">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="f0509-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="f0509-154">Rozšířené ověření provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="f0509-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="f0509-155">Vygeneruje dokument XML pro každou sadu transakce.</span><span class="sxs-lookup"><span data-stu-id="f0509-155">Generates an XML document for each transaction set.</span></span>
* <span data-ttu-id="f0509-156">Požádá o potvrzení o Technical nebo funkčnosti (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="f0509-156">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="f0509-157">Jako potvrzení o technické hello CONTRL zpráva znamená přijetí výměnu.</span><span class="sxs-lookup"><span data-stu-id="f0509-157">As a technical acknowledgment, hello CONTRL message indicates receipt of an interchange.</span></span>
  * <span data-ttu-id="f0509-158">Jako potvrzení o funkční hello CONTRL zpráva znamená přijetí nebo odmítnutí výměnu hello přijata, skupiny nebo zprávy, se seznamem chyby nebo nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="f0509-158">As a functional acknowledgment, hello CONTRL message indicates acceptance or rejection of hello received interchange, group, or message, with a list of errors or unsupported functionality</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="f0509-159">Soubor Swagger zobrazení</span><span class="sxs-lookup"><span data-stu-id="f0509-159">View Swagger file</span></span>
<span data-ttu-id="f0509-160">Podrobnosti Swagger hello tooview hello EDIFACT konektor, najdete v tématu [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="f0509-160">tooview hello Swagger details for hello EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0509-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0509-161">Next steps</span></span>
[<span data-ttu-id="f0509-162">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="f0509-162">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

