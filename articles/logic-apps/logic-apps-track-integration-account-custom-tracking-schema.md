---
title: "aaaCustom sledování schémata B2B monitorování - Azure Logic Apps | Microsoft Docs"
description: "Vytvořte vlastní sledování schémata toomonitor B2B zprávy z transakce ve vašem účtu integrace Azure."
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
ms.openlocfilehash: 8cf26a43d89f0414a2a8c5ef59d804235afeb5d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-tracking-toomonitor-your-complete-workflow-end-to-end"></a><span data-ttu-id="888dd-103">Povolit sledování toomonitor dokončení pracovního postupu, klient server</span><span class="sxs-lookup"><span data-stu-id="888dd-103">Enable tracking toomonitor your complete workflow, end-to-end</span></span>
<span data-ttu-id="888dd-104">Je integrované sledování, že můžete povolit pro různé části pracovního postupu business-to-business, jako je například sledování AS2 nebo X12 zprávy.</span><span class="sxs-lookup"><span data-stu-id="888dd-104">There is built-in tracking that you can enable for different parts of your business-to-business workflow, such as tracking AS2 or X12 messages.</span></span> <span data-ttu-id="888dd-105">Když vytvoříte pracovní postupy, které zahrnuje aplikace logiky, BizTalk Server, SQL Server nebo jinou vrstvu, můžete povolit vlastní sledování, který protokoluje události od hello začátku toohello konce pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="888dd-105">When you create workflows that includes a logic app, BizTalk Server, SQL Server, or any other layer, then you can enable custom tracking that logs events from hello beginning toohello end of your workflow.</span></span> 

<span data-ttu-id="888dd-106">Toto téma obsahuje vlastní kód, který můžete použít v hello vrstvy mimo aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="888dd-106">This topic provides custom code that you can use in hello layers outside of your logic app.</span></span> 

## <a name="custom-tracking-schema"></a><span data-ttu-id="888dd-107">Schéma vlastní sledování</span><span class="sxs-lookup"><span data-stu-id="888dd-107">Custom tracking schema</span></span>
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

| <span data-ttu-id="888dd-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="888dd-108">Property</span></span> | <span data-ttu-id="888dd-109">Typ</span><span class="sxs-lookup"><span data-stu-id="888dd-109">Type</span></span> | <span data-ttu-id="888dd-110">Popis</span><span class="sxs-lookup"><span data-stu-id="888dd-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="888dd-111">SourceType</span><span class="sxs-lookup"><span data-stu-id="888dd-111">sourceType</span></span> |   | <span data-ttu-id="888dd-112">Typ zdroje hello spustit.</span><span class="sxs-lookup"><span data-stu-id="888dd-112">Type of hello run source.</span></span> <span data-ttu-id="888dd-113">Povolené hodnoty jsou **Microsoft.Logic/workflows** a **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="888dd-113">Allowed values are **Microsoft.Logic/workflows** and **custom**.</span></span> <span data-ttu-id="888dd-114">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-114">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-115">Zdroj</span><span class="sxs-lookup"><span data-stu-id="888dd-115">Source</span></span> |   | <span data-ttu-id="888dd-116">Pokud je typ zdroje hello **Microsoft.Logic/workflows**, informace o zdroji hello musí toofollow toto schéma.</span><span class="sxs-lookup"><span data-stu-id="888dd-116">If hello source type is **Microsoft.Logic/workflows**, hello source information needs toofollow this schema.</span></span> <span data-ttu-id="888dd-117">Pokud je typ zdroje hello **vlastní**, schéma hello je JToken.</span><span class="sxs-lookup"><span data-stu-id="888dd-117">If hello source type is **custom**, hello schema is a JToken.</span></span> <span data-ttu-id="888dd-118">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-118">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-119">ID systému</span><span class="sxs-lookup"><span data-stu-id="888dd-119">systemId</span></span> | <span data-ttu-id="888dd-120">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-120">String</span></span> | <span data-ttu-id="888dd-121">ID logiku aplikace systému.</span><span class="sxs-lookup"><span data-stu-id="888dd-121">Logic app system ID.</span></span> <span data-ttu-id="888dd-122">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-122">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-123">identifikátor runId</span><span class="sxs-lookup"><span data-stu-id="888dd-123">runId</span></span> | <span data-ttu-id="888dd-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-124">String</span></span> | <span data-ttu-id="888dd-125">Aplikace logiky spustit ID.</span><span class="sxs-lookup"><span data-stu-id="888dd-125">Logic app run ID.</span></span> <span data-ttu-id="888dd-126">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-126">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-127">operationName</span><span class="sxs-lookup"><span data-stu-id="888dd-127">operationName</span></span> | <span data-ttu-id="888dd-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-128">String</span></span> | <span data-ttu-id="888dd-129">Název hello operace (například akce nebo aktivační událost).</span><span class="sxs-lookup"><span data-stu-id="888dd-129">Name of hello operation (for example, action or trigger).</span></span> <span data-ttu-id="888dd-130">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-130">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-131">repeatItemScopeName</span><span class="sxs-lookup"><span data-stu-id="888dd-131">repeatItemScopeName</span></span> | <span data-ttu-id="888dd-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-132">String</span></span> | <span data-ttu-id="888dd-133">Je-li akce hello je uvnitř opakujte název položky `foreach` / `until` smyčky.</span><span class="sxs-lookup"><span data-stu-id="888dd-133">Repeat item name if hello action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="888dd-134">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-134">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-135">repeatItemIndex</span><span class="sxs-lookup"><span data-stu-id="888dd-135">repeatItemIndex</span></span> | <span data-ttu-id="888dd-136">Integer</span><span class="sxs-lookup"><span data-stu-id="888dd-136">Integer</span></span> | <span data-ttu-id="888dd-137">Jestli je akce hello uvnitř `foreach` / `until` smyčky.</span><span class="sxs-lookup"><span data-stu-id="888dd-137">Whether hello action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="888dd-138">Určuje index opakovaných položky hello.</span><span class="sxs-lookup"><span data-stu-id="888dd-138">Indicates hello repeated item index.</span></span> <span data-ttu-id="888dd-139">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-139">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-140">trackingId</span><span class="sxs-lookup"><span data-stu-id="888dd-140">trackingId</span></span> | <span data-ttu-id="888dd-141">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-141">String</span></span> | <span data-ttu-id="888dd-142">ID sledování toocorrelate hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="888dd-142">Tracking ID, toocorrelate hello messages.</span></span> <span data-ttu-id="888dd-143">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="888dd-143">(Optional)</span></span> |
| <span data-ttu-id="888dd-144">correlationId</span><span class="sxs-lookup"><span data-stu-id="888dd-144">correlationId</span></span> | <span data-ttu-id="888dd-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-145">String</span></span> | <span data-ttu-id="888dd-146">ID korelace, toocorrelate hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="888dd-146">Correlation ID, toocorrelate hello messages.</span></span> <span data-ttu-id="888dd-147">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="888dd-147">(Optional)</span></span> |
| <span data-ttu-id="888dd-148">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="888dd-148">clientRequestId</span></span> | <span data-ttu-id="888dd-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="888dd-149">String</span></span> | <span data-ttu-id="888dd-150">Klient jej můžete naplnit toocorrelate zprávy.</span><span class="sxs-lookup"><span data-stu-id="888dd-150">Client can populate it toocorrelate messages.</span></span> <span data-ttu-id="888dd-151">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="888dd-151">(Optional)</span></span> |
| <span data-ttu-id="888dd-152">eventLevel</span><span class="sxs-lookup"><span data-stu-id="888dd-152">eventLevel</span></span> |   | <span data-ttu-id="888dd-153">Úroveň události hello.</span><span class="sxs-lookup"><span data-stu-id="888dd-153">Level of hello event.</span></span> <span data-ttu-id="888dd-154">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-154">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-155">eventTime</span><span class="sxs-lookup"><span data-stu-id="888dd-155">eventTime</span></span> |   | <span data-ttu-id="888dd-156">Čas události hello ve formátu RRRR-MM-DDTHH:MM:SS.00000Z UTC.</span><span class="sxs-lookup"><span data-stu-id="888dd-156">Time of hello event, in UTC format YYYY-MM-DDTHH:MM:SS.00000Z.</span></span> <span data-ttu-id="888dd-157">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-157">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-158">recordType</span><span class="sxs-lookup"><span data-stu-id="888dd-158">recordType</span></span> |   | <span data-ttu-id="888dd-159">Typ záznamu sledovat hello.</span><span class="sxs-lookup"><span data-stu-id="888dd-159">Type of hello track record.</span></span> <span data-ttu-id="888dd-160">Povolená hodnota je **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="888dd-160">Allowed value is **custom**.</span></span> <span data-ttu-id="888dd-161">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-161">(Mandatory)</span></span> |
| <span data-ttu-id="888dd-162">záznam</span><span class="sxs-lookup"><span data-stu-id="888dd-162">record</span></span> |   | <span data-ttu-id="888dd-163">Vlastní typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="888dd-163">Custom record type.</span></span> <span data-ttu-id="888dd-164">Povolený formát je JToken.</span><span class="sxs-lookup"><span data-stu-id="888dd-164">Allowed format is JToken.</span></span> <span data-ttu-id="888dd-165">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="888dd-165">(Mandatory)</span></span> |

## <a name="b2b-protocol-tracking-schemas"></a><span data-ttu-id="888dd-166">Schémata sledování protokol B2B</span><span class="sxs-lookup"><span data-stu-id="888dd-166">B2B protocol tracking schemas</span></span>
<span data-ttu-id="888dd-167">Informace o protokolu B2B sledování schémata najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="888dd-167">For information about B2B protocol tracking schemas, see:</span></span>
* [<span data-ttu-id="888dd-168">Schémata sledování AS2</span><span class="sxs-lookup"><span data-stu-id="888dd-168">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [<span data-ttu-id="888dd-169">Schémata sledování X12</span><span class="sxs-lookup"><span data-stu-id="888dd-169">X12 tracking schemas</span></span>](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="888dd-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="888dd-170">Next steps</span></span>
* <span data-ttu-id="888dd-171">Další informace o [sledování zpráv B2B](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="888dd-171">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="888dd-172">Další informace o [sledování zpráv B2B portálu Operations Management Suite hello](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="888dd-172">Learn about [tracking B2B messages in hello Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
* <span data-ttu-id="888dd-173">Další informace o hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="888dd-173">Learn more about hello [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>
