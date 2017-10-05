---
title: "Kódování zprávy EDIFACT – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a vygenerování XML pomocí kodéru zpráv EDIFACT v podniku integrační balíček pro Azure Logic Apps"
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
ms.openlocfilehash: b8d577326d23ec45cb4a9ec0e450ebf7afd945f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="6af3f-103">Kódování zprávy EDIFACT pro Azure Logic Apps s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="6af3f-103">Encode EDIFACT messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="6af3f-104">Konektor EDIFACT kódování zpráv může ověřit EDI a vlastnosti specifické pro partnery, Generovat dokument XML pro každou sadu transakce a vyžádat technických potvrzení, funkční potvrzení nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="6af3f-104">With the Encode EDIFACT message connector, you can validate EDI and partner-specific properties, generate an XML document for each transaction set, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="6af3f-105">Pokud chcete použít tento konektor, je nutné přidat konektor k existující aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="6af3f-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="6af3f-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6af3f-106">Before you start</span></span>

<span data-ttu-id="6af3f-107">Tady je položky, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6af3f-107">Here's the items you need:</span></span>

* <span data-ttu-id="6af3f-108">Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="6af3f-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="6af3f-109">[Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="6af3f-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="6af3f-110">Musí mít účet integrace k používání konektoru EDIFACT kódování zprávy.</span><span class="sxs-lookup"><span data-stu-id="6af3f-110">You must have an integration account to use the Encode EDIFACT message connector.</span></span> 
* <span data-ttu-id="6af3f-111">Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace</span><span class="sxs-lookup"><span data-stu-id="6af3f-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="6af3f-112">[Smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md) , již je definována v účtu integrace</span><span class="sxs-lookup"><span data-stu-id="6af3f-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="encode-edifact-messages"></a><span data-ttu-id="6af3f-113">Kódování zprávy EDIFACT</span><span class="sxs-lookup"><span data-stu-id="6af3f-113">Encode EDIFACT messages</span></span>

1. <span data-ttu-id="6af3f-114">[Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6af3f-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="6af3f-115">Konektor EDIFACT kódování zprávy nemá aktivačních událostí, proto musíte přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="6af3f-115">The Encode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="6af3f-116">V návrháři aplikace logiky přidejte aktivační událost a potom přidat akci do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="6af3f-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="6af3f-117">Do vyhledávacího pole zadejte "EDIFACT" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="6af3f-117">In the search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="6af3f-118">Vyberte buď **zakódovat zprávu EDIFACT název smlouvy** nebo **kódovat EDIFACT zprávu pomocí identity**.</span><span class="sxs-lookup"><span data-stu-id="6af3f-118">Select either **Encode EDIFACT Message by agreement name** or **Encode to EDIFACT message by identities**.</span></span>
   
    ![hledání EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. <span data-ttu-id="6af3f-120">Pokud jste nevytvořili dříve všechna připojení k vašemu účtu integrace, se zobrazí výzva k vytvoření připojení nyní.</span><span class="sxs-lookup"><span data-stu-id="6af3f-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="6af3f-121">Název připojení a vyberte integrační účet, který chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="6af3f-121">Name your connection, and select the integration account that you want to connect.</span></span>

    ![Vytvoření připojení účtu integrace](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    <span data-ttu-id="6af3f-123">Vyžadují se vlastnosti s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="6af3f-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="6af3f-124">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6af3f-124">Property</span></span> | <span data-ttu-id="6af3f-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6af3f-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="6af3f-126">Název připojení *</span><span class="sxs-lookup"><span data-stu-id="6af3f-126">Connection Name *</span></span> |<span data-ttu-id="6af3f-127">Zadejte libovolný název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="6af3f-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="6af3f-128">Integrace účet *</span><span class="sxs-lookup"><span data-stu-id="6af3f-128">Integration Account *</span></span> |<span data-ttu-id="6af3f-129">Zadejte název pro váš účet integrace.</span><span class="sxs-lookup"><span data-stu-id="6af3f-129">Enter a name for your integration account.</span></span> <span data-ttu-id="6af3f-130">Ujistěte se, že integrace účet a logiku aplikace jsou ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="6af3f-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="6af3f-131">Když jste hotovi, by měla vypadat podobně jako tento příklad podrobné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="6af3f-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="6af3f-132">Dokončete vytváření připojení, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6af3f-132">To finish creating your connection, choose **Create**.</span></span>

    ![Podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    <span data-ttu-id="6af3f-134">Připojení je nyní vytvořena.</span><span class="sxs-lookup"><span data-stu-id="6af3f-134">Your connection is now created.</span></span>

    ![Integrace připojení účet vytvořili](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a><span data-ttu-id="6af3f-136">Kódování zprávy EDIFACT podle název smlouvy</span><span class="sxs-lookup"><span data-stu-id="6af3f-136">Encode EDIFACT Message by agreement name</span></span>

<span data-ttu-id="6af3f-137">Pokud jste se rozhodli kódování zprávy EDIFACT podle názvu smlouvy, otevřete **název EDIFACT smlouvy** seznamu, zadejte nebo vyberte název smlouvy EDIFACT.</span><span class="sxs-lookup"><span data-stu-id="6af3f-137">If you chose to encode EDIFACT messages by agreement name, open the **Name of EDIFACT agreement** list, enter or select your EDIFACT agreement name.</span></span> <span data-ttu-id="6af3f-138">Zadejte zprávu XML ke kódování.</span><span class="sxs-lookup"><span data-stu-id="6af3f-138">Enter the XML message to encode.</span></span>

![Zadejte název smlouvy EDIFACT a zpráva XML ke kódování](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a><span data-ttu-id="6af3f-140">Kódování zprávy EDIFACT podle identity</span><span class="sxs-lookup"><span data-stu-id="6af3f-140">Encode EDIFACT Message by identities</span></span>

<span data-ttu-id="6af3f-141">Pokud si zvolíte ke kódování zprávy EDIFACT podle identity, zadejte identifikátor odesílatele, kvalifikátor odesílatel, příjemce identifikátor a příjemce kvalifikátor podle konfigurace v EDIFACT smlouvě.</span><span class="sxs-lookup"><span data-stu-id="6af3f-141">If you choose to encode EDIFACT messages by identities, enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your EDIFACT agreement.</span></span> <span data-ttu-id="6af3f-142">Vyberte zprávy XML ke kódování.</span><span class="sxs-lookup"><span data-stu-id="6af3f-142">Select the XML message to encode.</span></span>

![Zadejte identit pro odesílatele a příjemce, vyberte zprávy XML ke kódování](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a><span data-ttu-id="6af3f-144">Kódování EDIFACT podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6af3f-144">EDIFACT Encode details</span></span>

<span data-ttu-id="6af3f-145">Konektor kódování EDIFACT provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="6af3f-145">The Encode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="6af3f-146">Smlouvu můžete vyřešit odpovídající odesílatele kvalifikátor & identifikátor a kvalifikátor příjemce a identifikátor</span><span class="sxs-lookup"><span data-stu-id="6af3f-146">Resolve the agreement by matching the sender qualifier & identifier and receiver qualifier and identifier</span></span>
* <span data-ttu-id="6af3f-147">Serializuje výměnu EDI, převodu kódu XML zprávy EDI transakce sady v výměnu.</span><span class="sxs-lookup"><span data-stu-id="6af3f-147">Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.</span></span>
* <span data-ttu-id="6af3f-148">Použije sadu transakce hlavičku a přípojného segmenty</span><span class="sxs-lookup"><span data-stu-id="6af3f-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="6af3f-149">Generuje představuje počet řízení výměnu, číslem skupiny řízení a číslem transakce sadu ovládacího prvku pro každý odchozí výměnu</span><span class="sxs-lookup"><span data-stu-id="6af3f-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="6af3f-150">Nahradí oddělovače v datové části dat</span><span class="sxs-lookup"><span data-stu-id="6af3f-150">Replaces separators in the payload data</span></span>
* <span data-ttu-id="6af3f-151">Ověří EDI a vlastnosti specifické pro partnery</span><span class="sxs-lookup"><span data-stu-id="6af3f-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="6af3f-152">Ověření schématu elementů transakce sady dat pro schéma zprávy.</span><span class="sxs-lookup"><span data-stu-id="6af3f-152">Schema validation of the transaction-set data elements against the message schema.</span></span>
  * <span data-ttu-id="6af3f-153">Ověření EDI provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="6af3f-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="6af3f-154">Rozšířené ověření provádět transakce set datové prvky.</span><span class="sxs-lookup"><span data-stu-id="6af3f-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="6af3f-155">Vygeneruje dokument XML pro každou sadu transakce.</span><span class="sxs-lookup"><span data-stu-id="6af3f-155">Generates an XML document for each transaction set.</span></span>
* <span data-ttu-id="6af3f-156">Požádá o potvrzení o Technical nebo funkčnosti (Pokud je nakonfigurovaná).</span><span class="sxs-lookup"><span data-stu-id="6af3f-156">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="6af3f-157">Jako potvrzení o technické označuje zprávy CONTRL přijetí výměnu.</span><span class="sxs-lookup"><span data-stu-id="6af3f-157">As a technical acknowledgment, the CONTRL message indicates receipt of an interchange.</span></span>
  * <span data-ttu-id="6af3f-158">Jako potvrzení o funkční CONTRL zpráva znamená přijetí nebo odmítnutí přijaté výměnu, skupiny nebo zprávy, se seznamem chyby nebo nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="6af3f-158">As a functional acknowledgment, the CONTRL message indicates acceptance or rejection of the received interchange, group, or message, with a list of errors or unsupported functionality</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="6af3f-159">Soubor Swagger zobrazení</span><span class="sxs-lookup"><span data-stu-id="6af3f-159">View Swagger file</span></span>
<span data-ttu-id="6af3f-160">Chcete-li zobrazit podrobnosti Swagger pro konektor EDIFACT, najdete v části [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="6af3f-160">To view the Swagger details for the EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6af3f-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6af3f-161">Next steps</span></span>
[<span data-ttu-id="6af3f-162">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="6af3f-162">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

