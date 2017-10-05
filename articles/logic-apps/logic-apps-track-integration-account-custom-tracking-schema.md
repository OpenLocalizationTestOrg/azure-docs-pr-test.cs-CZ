---
title: "Vlastní sledování schémata pro monitorování B2B - Azure Logic Apps | Microsoft Docs"
description: "Vytvořte vlastní sledování schémata ke sledování zpráv B2B z transakcí ve vašem účtu integrace Azure."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b71a4938dde2a71f1ce29403af7aa9101358d64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-tracking-to-monitor-your-complete-workflow-end-to-end"></a><span data-ttu-id="135cb-103">Povolení sledování monitorování dokončení pracovního postupu, klient server</span><span class="sxs-lookup"><span data-stu-id="135cb-103">Enable tracking to monitor your complete workflow, end-to-end</span></span>
<span data-ttu-id="135cb-104">Je integrované sledování, že můžete povolit pro různé části pracovního postupu business-to-business, jako je například sledování AS2 nebo X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="135cb-104">There is built-in tracking that you can enable for different parts of your business-to-business workflow, such as tracking AS2 or X12 messages.</span></span> <span data-ttu-id="135cb-105">Při vytváření pracovních postupů, které obsahuje aplikace logiky, BizTalk Server, SQL Server nebo jinou vrstvu, pak můžete povolit vlastní sledování, který protokoluje události od začátku do konce pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="135cb-105">When you create workflows that includes a logic app, BizTalk Server, SQL Server, or any other layer, then you can enable custom tracking that logs events from the beginning to the end of your workflow.</span></span> 

<span data-ttu-id="135cb-106">Toto téma obsahuje vlastní kód, který můžete použít v vrstvy mimo aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="135cb-106">This topic provides custom code that you can use in the layers outside of your logic app.</span></span> 

## <a name="custom-tracking-schema"></a><span data-ttu-id="135cb-107">Schéma vlastní sledování</span><span class="sxs-lookup"><span data-stu-id="135cb-107">Custom tracking schema</span></span>
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| <span data-ttu-id="135cb-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="135cb-108">Property</span></span> | <span data-ttu-id="135cb-109">Typ</span><span class="sxs-lookup"><span data-stu-id="135cb-109">Type</span></span> | <span data-ttu-id="135cb-110">Popis</span><span class="sxs-lookup"><span data-stu-id="135cb-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="135cb-111">SourceType</span><span class="sxs-lookup"><span data-stu-id="135cb-111">sourceType</span></span> |   | <span data-ttu-id="135cb-112">Typ spuštění zdroje.</span><span class="sxs-lookup"><span data-stu-id="135cb-112">Type of the run source.</span></span> <span data-ttu-id="135cb-113">Povolené hodnoty jsou **Microsoft.Logic/workflows** a **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="135cb-113">Allowed values are **Microsoft.Logic/workflows** and **custom**.</span></span> <span data-ttu-id="135cb-114">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-114">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-115">Zdroj</span><span class="sxs-lookup"><span data-stu-id="135cb-115">Source</span></span> |   | <span data-ttu-id="135cb-116">Pokud je typ zdroje **Microsoft.Logic/workflows**, informace o zdroji musí postupujte podle tohoto schématu.</span><span class="sxs-lookup"><span data-stu-id="135cb-116">If the source type is **Microsoft.Logic/workflows**, the source information needs to follow this schema.</span></span> <span data-ttu-id="135cb-117">Pokud je typ zdroje **vlastní**, schéma je JToken.</span><span class="sxs-lookup"><span data-stu-id="135cb-117">If the source type is **custom**, the schema is a JToken.</span></span> <span data-ttu-id="135cb-118">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-118">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-119">ID systému</span><span class="sxs-lookup"><span data-stu-id="135cb-119">systemId</span></span> | <span data-ttu-id="135cb-120">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-120">String</span></span> | <span data-ttu-id="135cb-121">ID logiku aplikace systému.</span><span class="sxs-lookup"><span data-stu-id="135cb-121">Logic app system ID.</span></span> <span data-ttu-id="135cb-122">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-122">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-123">identifikátor runId</span><span class="sxs-lookup"><span data-stu-id="135cb-123">runId</span></span> | <span data-ttu-id="135cb-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-124">String</span></span> | <span data-ttu-id="135cb-125">Aplikace logiky spustit ID.</span><span class="sxs-lookup"><span data-stu-id="135cb-125">Logic app run ID.</span></span> <span data-ttu-id="135cb-126">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-126">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-127">operationName</span><span class="sxs-lookup"><span data-stu-id="135cb-127">operationName</span></span> | <span data-ttu-id="135cb-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-128">String</span></span> | <span data-ttu-id="135cb-129">Název operace (například akce nebo aktivační událost).</span><span class="sxs-lookup"><span data-stu-id="135cb-129">Name of the operation (for example, action or trigger).</span></span> <span data-ttu-id="135cb-130">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-130">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-131">repeatItemScopeName</span><span class="sxs-lookup"><span data-stu-id="135cb-131">repeatItemScopeName</span></span> | <span data-ttu-id="135cb-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-132">String</span></span> | <span data-ttu-id="135cb-133">Název položky opakovat, pokud je akce uvnitř `foreach` / `until` smyčky.</span><span class="sxs-lookup"><span data-stu-id="135cb-133">Repeat item name if the action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="135cb-134">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-134">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-135">repeatItemIndex</span><span class="sxs-lookup"><span data-stu-id="135cb-135">repeatItemIndex</span></span> | <span data-ttu-id="135cb-136">Integer</span><span class="sxs-lookup"><span data-stu-id="135cb-136">Integer</span></span> | <span data-ttu-id="135cb-137">Jestli je akce uvnitř `foreach` / `until` smyčky.</span><span class="sxs-lookup"><span data-stu-id="135cb-137">Whether the action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="135cb-138">Určuje index opakovaných položky.</span><span class="sxs-lookup"><span data-stu-id="135cb-138">Indicates the repeated item index.</span></span> <span data-ttu-id="135cb-139">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-139">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-140">trackingId</span><span class="sxs-lookup"><span data-stu-id="135cb-140">trackingId</span></span> | <span data-ttu-id="135cb-141">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-141">String</span></span> | <span data-ttu-id="135cb-142">ID sledování ke korelaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="135cb-142">Tracking ID, to correlate the messages.</span></span> <span data-ttu-id="135cb-143">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="135cb-143">(Optional)</span></span> |
| <span data-ttu-id="135cb-144">correlationId</span><span class="sxs-lookup"><span data-stu-id="135cb-144">correlationId</span></span> | <span data-ttu-id="135cb-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-145">String</span></span> | <span data-ttu-id="135cb-146">ID korelace ke korelaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="135cb-146">Correlation ID, to correlate the messages.</span></span> <span data-ttu-id="135cb-147">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="135cb-147">(Optional)</span></span> |
| <span data-ttu-id="135cb-148">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="135cb-148">clientRequestId</span></span> | <span data-ttu-id="135cb-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="135cb-149">String</span></span> | <span data-ttu-id="135cb-150">Klienta můžete naplnit ji ke korelaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="135cb-150">Client can populate it to correlate messages.</span></span> <span data-ttu-id="135cb-151">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="135cb-151">(Optional)</span></span> |
| <span data-ttu-id="135cb-152">eventLevel</span><span class="sxs-lookup"><span data-stu-id="135cb-152">eventLevel</span></span> |   | <span data-ttu-id="135cb-153">Úroveň události.</span><span class="sxs-lookup"><span data-stu-id="135cb-153">Level of the event.</span></span> <span data-ttu-id="135cb-154">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-154">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-155">eventTime</span><span class="sxs-lookup"><span data-stu-id="135cb-155">eventTime</span></span> |   | <span data-ttu-id="135cb-156">Čas události ve formátu RRRR-MM-DDTHH:MM:SS.00000Z UTC.</span><span class="sxs-lookup"><span data-stu-id="135cb-156">Time of the event, in UTC format YYYY-MM-DDTHH:MM:SS.00000Z.</span></span> <span data-ttu-id="135cb-157">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-157">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-158">recordType</span><span class="sxs-lookup"><span data-stu-id="135cb-158">recordType</span></span> |   | <span data-ttu-id="135cb-159">Typ sledování záznamu.</span><span class="sxs-lookup"><span data-stu-id="135cb-159">Type of the track record.</span></span> <span data-ttu-id="135cb-160">Povolená hodnota je **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="135cb-160">Allowed value is **custom**.</span></span> <span data-ttu-id="135cb-161">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-161">(Mandatory)</span></span> |
| <span data-ttu-id="135cb-162">záznam</span><span class="sxs-lookup"><span data-stu-id="135cb-162">record</span></span> |   | <span data-ttu-id="135cb-163">Vlastní typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="135cb-163">Custom record type.</span></span> <span data-ttu-id="135cb-164">Povolený formát je JToken.</span><span class="sxs-lookup"><span data-stu-id="135cb-164">Allowed format is JToken.</span></span> <span data-ttu-id="135cb-165">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="135cb-165">(Mandatory)</span></span> |

## <a name="b2b-protocol-tracking-schemas"></a><span data-ttu-id="135cb-166">Schémata sledování protokol B2B</span><span class="sxs-lookup"><span data-stu-id="135cb-166">B2B protocol tracking schemas</span></span>
<span data-ttu-id="135cb-167">Informace o protokolu B2B sledování schémata najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="135cb-167">For information about B2B protocol tracking schemas, see:</span></span>
* [<span data-ttu-id="135cb-168">Schémata sledování AS2</span><span class="sxs-lookup"><span data-stu-id="135cb-168">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [<span data-ttu-id="135cb-169">Schémata sledování X12</span><span class="sxs-lookup"><span data-stu-id="135cb-169">X12 tracking schemas</span></span>](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="135cb-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="135cb-170">Next steps</span></span>
* <span data-ttu-id="135cb-171">Další informace o [sledování zpráv B2B](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="135cb-171">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="135cb-172">Další informace o [sledování zpráv B2B na portálu služby Operations Management Suite](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="135cb-172">Learn about [tracking B2B messages in the Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
* <span data-ttu-id="135cb-173">Další informace o [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="135cb-173">Learn more about the [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>
