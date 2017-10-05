---
title: "B2B aplikace logiky edifact dekódovat vyřešit UNH2.5 - Azure Logic Apps | Microsoft Docs"
description: "Azure B2B aplikace logiky edifact dekódovat vyřešit UNH2.5"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 62ad8183cc6e9f56255b2729a04ee7710d00a21a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-handle-edifact-documents-having-unh25-segment"></a><span data-ttu-id="6ce79-103">Postupy: zpracování EDIFACT dokumenty s UNH2.5 segmentu</span><span class="sxs-lookup"><span data-stu-id="6ce79-103">How to handle EDIFACT documents having UNH2.5 segment</span></span>
<span data-ttu-id="6ce79-104">Pokud je k dispozici v dokumentu EDIFACT UNH2.5, ho se používá pro vyhledávání schématu.</span><span class="sxs-lookup"><span data-stu-id="6ce79-104">When UNH2.5 is present in the EDIFACT document, it is being used for schema lookup.</span></span> 

<span data-ttu-id="6ce79-105">Příklad: Pole UNH je **EAN008** ve zprávě EDIFACT</span><span class="sxs-lookup"><span data-stu-id="6ce79-105">Example: The UNH field is **EAN008** in the EDIFACT message</span></span>  
<span data-ttu-id="6ce79-106">UNH + SSDD1 + OBJEDNÁVKY: D: 03B: ZRUŠENÍ:**EAN008**.</span><span class="sxs-lookup"><span data-stu-id="6ce79-106">UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'</span></span>  

<span data-ttu-id="6ce79-107">Postup pro zpracování zprávy</span><span class="sxs-lookup"><span data-stu-id="6ce79-107">Steps to follow to handle the message</span></span> 
1. <span data-ttu-id="6ce79-108">Aktualizovat schéma.</span><span class="sxs-lookup"><span data-stu-id="6ce79-108">Update the schema</span></span>
2. <span data-ttu-id="6ce79-109">Zkontrolujte nastavení smlouvy</span><span class="sxs-lookup"><span data-stu-id="6ce79-109">Check the agreement settings</span></span>  

## <a name="update-the-schema"></a><span data-ttu-id="6ce79-110">Aktualizovat schéma.</span><span class="sxs-lookup"><span data-stu-id="6ce79-110">Update the schema</span></span>
<span data-ttu-id="6ce79-111">Zpracovat zprávu, budete muset nasadit schématu s UNH2.5 název kořenového uzlu.</span><span class="sxs-lookup"><span data-stu-id="6ce79-111">To process the message, you need to deploy a schema with the UNH2.5 root node name.</span></span>  <span data-ttu-id="6ce79-112">Pro danou příklad, název kořenového schématu by **EFACT_D03B_ORDERS_EAN008**</span><span class="sxs-lookup"><span data-stu-id="6ce79-112">For given an example, the schema root name would be **EFACT_D03B_ORDERS_EAN008**</span></span>  

<span data-ttu-id="6ce79-113">Pro každý D03B_ORDERS různých UNH2.5 segmentu je třeba nasadit jednotlivých schématu.</span><span class="sxs-lookup"><span data-stu-id="6ce79-113">For each D03B_ORDERS with a different UNH2.5 segment, you would have to deploy an individual schema.</span></span>  

## <a name="add-schema-to-the-edifact-agreement"></a><span data-ttu-id="6ce79-114">Přidání schématu do smlouvy EDIFACT</span><span class="sxs-lookup"><span data-stu-id="6ce79-114">Add schema to the EDIFACT agreement</span></span>
### <a name="edifact-decode"></a><span data-ttu-id="6ce79-115">Dekódovat EDIFACT</span><span class="sxs-lookup"><span data-stu-id="6ce79-115">EDIFACT Decode</span></span>
<span data-ttu-id="6ce79-116">Dekódování příchozí zprávy, se konfigurace schématu v EDIFACT smlouvy obdrží nastavení</span><span class="sxs-lookup"><span data-stu-id="6ce79-116">To Decode the incoming message, configure the schema in the EDIFACT agreement receive settings</span></span>
1. <span data-ttu-id="6ce79-117">Přidat schéma k účtu integrace</span><span class="sxs-lookup"><span data-stu-id="6ce79-117">Add the schema to the integration account</span></span>    
2. <span data-ttu-id="6ce79-118">Konfigurace schématu v EDIFACT smlouvy obdrží nastavení.</span><span class="sxs-lookup"><span data-stu-id="6ce79-118">Configure the schema in the EDIFACT agreement receive settings.</span></span> 
3. <span data-ttu-id="6ce79-119">Vyberte smlouvy EDIFACT a klikněte na **upravit jako JSON**.</span><span class="sxs-lookup"><span data-stu-id="6ce79-119">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="6ce79-120">Přidejte hodnotu UNH2.5 Smlouvy přijímat **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6ce79-120">Add UNH2.5 value in the Receive Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span></span>

### <a name="edifact-encode"></a><span data-ttu-id="6ce79-121">EDIFACT kódování</span><span class="sxs-lookup"><span data-stu-id="6ce79-121">EDIFACT Encode</span></span>
<span data-ttu-id="6ce79-122">Ke kódování příchozí zpráva, nakonfigurujte v nastavení odesílání smlouvy EDIFACT schéma</span><span class="sxs-lookup"><span data-stu-id="6ce79-122">To Encode the incoming message, configure the schema in the EDIFACT agreement send settings</span></span>
1. <span data-ttu-id="6ce79-123">Přidat schéma k účtu integrace</span><span class="sxs-lookup"><span data-stu-id="6ce79-123">Add the schema to the integration account</span></span>    
2. <span data-ttu-id="6ce79-124">V nastavení odesílání smlouvy EDIFACT nakonfigurujte schématu.</span><span class="sxs-lookup"><span data-stu-id="6ce79-124">Configure the schema in the EDIFACT agreement send settings.</span></span> 
3. <span data-ttu-id="6ce79-125">Vyberte smlouvy EDIFACT a klikněte na **upravit jako JSON**.</span><span class="sxs-lookup"><span data-stu-id="6ce79-125">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="6ce79-126">Přidejte hodnotu UNH2.5 Smlouvy odeslat **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span><span class="sxs-lookup"><span data-stu-id="6ce79-126">Add UNH2.5 value in the Send Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ce79-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ce79-127">Next Steps</span></span>
* [<span data-ttu-id="6ce79-128">Další informace o integraci účet smlouvy</span><span class="sxs-lookup"><span data-stu-id="6ce79-128">Learn more about integration account agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  