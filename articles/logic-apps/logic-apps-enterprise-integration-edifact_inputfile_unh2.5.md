---
title: "aaaLogic aplikace B2B edifact dekódovat vyřešit UNH2.5 - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 6d85242d0f828fa52cdc9689938f3ba1e51b1183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toohandle-edifact-documents-having-unh25-segment"></a><span data-ttu-id="81b04-103">Jak toohandle EDIFACT dokumentů s UNH2.5 segmentu</span><span class="sxs-lookup"><span data-stu-id="81b04-103">How toohandle EDIFACT documents having UNH2.5 segment</span></span>
<span data-ttu-id="81b04-104">Když je k dispozici v dokumentu EDIFACT hello UNH2.5, ho se používá pro vyhledávání schématu.</span><span class="sxs-lookup"><span data-stu-id="81b04-104">When UNH2.5 is present in hello EDIFACT document, it is being used for schema lookup.</span></span> 

<span data-ttu-id="81b04-105">Příklad: pole UNH hello je **EAN008** ve zprávě EDIFACT hello</span><span class="sxs-lookup"><span data-stu-id="81b04-105">Example: hello UNH field is **EAN008** in hello EDIFACT message</span></span>  
<span data-ttu-id="81b04-106">UNH + SSDD1 + OBJEDNÁVKY: D: 03B: ZRUŠENÍ:**EAN008**.</span><span class="sxs-lookup"><span data-stu-id="81b04-106">UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'</span></span>  

<span data-ttu-id="81b04-107">Kroky toofollow toohandle uvítací zprávu</span><span class="sxs-lookup"><span data-stu-id="81b04-107">Steps toofollow toohandle hello message</span></span> 
1. <span data-ttu-id="81b04-108">Aktualizovat schéma hello</span><span class="sxs-lookup"><span data-stu-id="81b04-108">Update hello schema</span></span>
2. <span data-ttu-id="81b04-109">Zkontrolujte nastavení smlouvy hello</span><span class="sxs-lookup"><span data-stu-id="81b04-109">Check hello agreement settings</span></span>  

## <a name="update-hello-schema"></a><span data-ttu-id="81b04-110">Aktualizovat schéma hello</span><span class="sxs-lookup"><span data-stu-id="81b04-110">Update hello schema</span></span>
<span data-ttu-id="81b04-111">tooprocess uvítací zprávu, budete potřebovat toodeploy schématu s název hello UNH2.5 kořenového uzlu.</span><span class="sxs-lookup"><span data-stu-id="81b04-111">tooprocess hello message, you need toodeploy a schema with hello UNH2.5 root node name.</span></span>  <span data-ttu-id="81b04-112">Pro danou příklad, bude název kořenové schématu hello **EFACT_D03B_ORDERS_EAN008**</span><span class="sxs-lookup"><span data-stu-id="81b04-112">For given an example, hello schema root name would be **EFACT_D03B_ORDERS_EAN008**</span></span>  

<span data-ttu-id="81b04-113">Pro každý D03B_ORDERS různých UNH2.5 segmentu byste měli toodeploy jednotlivých schématu.</span><span class="sxs-lookup"><span data-stu-id="81b04-113">For each D03B_ORDERS with a different UNH2.5 segment, you would have toodeploy an individual schema.</span></span>  

## <a name="add-schema-toohello-edifact-agreement"></a><span data-ttu-id="81b04-114">Přidání smlouvy EDIFACT toohello schématu</span><span class="sxs-lookup"><span data-stu-id="81b04-114">Add schema toohello EDIFACT agreement</span></span>
### <a name="edifact-decode"></a><span data-ttu-id="81b04-115">Dekódovat EDIFACT</span><span class="sxs-lookup"><span data-stu-id="81b04-115">EDIFACT Decode</span></span>
<span data-ttu-id="81b04-116">tooDecode hello příchozí zprávy, nakonfigurujte hello schématu v hello EDIFACT smlouvy obdrží nastavení</span><span class="sxs-lookup"><span data-stu-id="81b04-116">tooDecode hello incoming message, configure hello schema in hello EDIFACT agreement receive settings</span></span>
1. <span data-ttu-id="81b04-117">Přidat účet integrace toohello schématu hello</span><span class="sxs-lookup"><span data-stu-id="81b04-117">Add hello schema toohello integration account</span></span>    
2. <span data-ttu-id="81b04-118">Konfigurace hello schématu v hello EDIFACT smlouvy obdrží nastavení.</span><span class="sxs-lookup"><span data-stu-id="81b04-118">Configure hello schema in hello EDIFACT agreement receive settings.</span></span> 
3. <span data-ttu-id="81b04-119">Vyberte smlouvy EDIFACT a klikněte na **upravit jako JSON**.</span><span class="sxs-lookup"><span data-stu-id="81b04-119">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="81b04-120">Přidejte hodnotu UNH2.5 v hello přijetí smlouvy **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span><span class="sxs-lookup"><span data-stu-id="81b04-120">Add UNH2.5 value in hello Receive Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span></span>

### <a name="edifact-encode"></a><span data-ttu-id="81b04-121">EDIFACT kódování</span><span class="sxs-lookup"><span data-stu-id="81b04-121">EDIFACT Encode</span></span>
<span data-ttu-id="81b04-122">tooEncode hello příchozí zprávy, nakonfigurujte hello schématu hello EDIFACT smlouvy odeslání</span><span class="sxs-lookup"><span data-stu-id="81b04-122">tooEncode hello incoming message, configure hello schema in hello EDIFACT agreement send settings</span></span>
1. <span data-ttu-id="81b04-123">Přidat účet integrace toohello schématu hello</span><span class="sxs-lookup"><span data-stu-id="81b04-123">Add hello schema toohello integration account</span></span>    
2. <span data-ttu-id="81b04-124">Nakonfigurujte hello schématu hello EDIFACT smlouvy odeslání.</span><span class="sxs-lookup"><span data-stu-id="81b04-124">Configure hello schema in hello EDIFACT agreement send settings.</span></span> 
3. <span data-ttu-id="81b04-125">Vyberte smlouvy EDIFACT a klikněte na **upravit jako JSON**.</span><span class="sxs-lookup"><span data-stu-id="81b04-125">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="81b04-126">Přidejte hodnotu UNH2.5 v hello odeslat smlouvy **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span><span class="sxs-lookup"><span data-stu-id="81b04-126">Add UNH2.5 value in hello Send Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="81b04-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81b04-127">Next Steps</span></span>
* [<span data-ttu-id="81b04-128">Další informace o integraci účet smlouvy</span><span class="sxs-lookup"><span data-stu-id="81b04-128">Learn more about integration account agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  