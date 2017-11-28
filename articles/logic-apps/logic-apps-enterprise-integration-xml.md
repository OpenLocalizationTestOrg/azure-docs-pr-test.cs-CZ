---
title: "Práce s zpráv XML ve svých pracovních postupech - Azure Logic Apps | Microsoft Docs"
description: "Zpracovat, ověřit, transformace a zlepšit komunikaci oddělení zprávy XML aplikace logiky a business-pro scénáře s využitím Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 3fec4935f5317be4bf8c9e05f1c24a7c05381b1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a><span data-ttu-id="ede7d-103">Ověření a transformace XML, kódování a dekódování plochých souborů a rozšířit funkce zprávy v aplikacích logiky</span><span class="sxs-lookup"><span data-stu-id="ede7d-103">Validate and transform XML, encode and decode flat files, and enrich messages features in logic apps</span></span>

<span data-ttu-id="ede7d-104">Použití aplikace logiky, máte možnost zpracování zpráv XML, které odesílat a přijímat.</span><span class="sxs-lookup"><span data-stu-id="ede7d-104">Using logic apps, you have the ability to process XML messages that you send and receive.</span></span> <span data-ttu-id="ede7d-105">Tato funkce je součástí integračního balíčku Enterprise.</span><span class="sxs-lookup"><span data-stu-id="ede7d-105">This feature is included with the Enterprise Integration Pack.</span></span> <span data-ttu-id="ede7d-106">Pro tyto uživatele s pozadí BizTalk Server Enterprise integračního balíčku poskytuje podobné dalo transformace a ověření zprávy, práce s plochých souborů a i pomocí jazyka XPath zlepšit komunikaci oddělení nebo extrahuje specifické vlastnosti ze zprávy.</span><span class="sxs-lookup"><span data-stu-id="ede7d-106">For those users with a BizTalk Server background, the Enterprise Integration Pack gives you similar abilities to transform and validate messages, work with flat files, and even use XPath to enrich or extract specific properties from a message.</span></span> 

<span data-ttu-id="ede7d-107">Pro uživatele, kteří jsou nové pro tento prostor rozbalte tyto funkce, jak při zpracování zprávy v rámci pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="ede7d-107">For those users who are new to this space, these features expand how you process messages within your workflow.</span></span> <span data-ttu-id="ede7d-108">Například pokud se v případě business-to-business a pracovat s konkrétní schémat XML, integrační balíček Enterprise můžete použít ke zvýšení jak vaše společnost zpracovává tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="ede7d-108">For example, if you are in a business-to-business scenario, and work with specific XML schemas, then you can use the Enterprise Integration Pack to enhance how your company processes these messages.</span></span> 

<span data-ttu-id="ede7d-109">Integrační balíček Enterprise zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="ede7d-109">The Enterprise Integration Pack includes:</span></span> 

* <span data-ttu-id="ede7d-110">[Ověření XML](logic-apps-enterprise-integration-xml-validation.md "Další informace o ověření XML zprávy") -ověření příchozích nebo odchozích XML zprávy pro konkrétní schéma.</span><span class="sxs-lookup"><span data-stu-id="ede7d-110">[XML validation](logic-apps-enterprise-integration-xml-validation.md "Learn about XML message validation") - Validate an incoming or outgoing XML message against a specific schema.</span></span>
* <span data-ttu-id="ede7d-111">[Transformace XML](../logic-apps/logic-apps-enterprise-integration-transform.md "Další informace o mapování a transformace zprávy XML") – převést nebo úprava zprávy XML na základě vaše požadavky nebo požadavky na partnera.</span><span class="sxs-lookup"><span data-stu-id="ede7d-111">[XML transform](../logic-apps/logic-apps-enterprise-integration-transform.md "Learn about XML message transformations and maps") - Convert or customize an XML message based on your requirements, or the requirements of a partner.</span></span>
* <span data-ttu-id="ede7d-112">[Plochý soubor kódování a dekódování plochý soubor](logic-apps-enterprise-integration-flatfile.md "Další informace o plochý soubor kódování a dekódování") – zakódování nebo dekódování plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="ede7d-112">[Flat file encoding and flat file decoding](logic-apps-enterprise-integration-flatfile.md "Learn about flat file encoding/decoding") - Encode or decode a flat file.</span></span> <span data-ttu-id="ede7d-113">Například SAP přijímá a odesílá IDOC soubory ve formátu plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="ede7d-113">For example, SAP accepts and sends IDOC files in flat file format.</span></span> <span data-ttu-id="ede7d-114">Řada platforem integrace vytvořit XML zprávy, včetně Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="ede7d-114">Many integration platforms create XML messages, including Logic Apps.</span></span> <span data-ttu-id="ede7d-115">Ano můžete vytvořit aplikaci logiky, která používá kodér plochý soubor "převést" soubory XML do plochých souborů.</span><span class="sxs-lookup"><span data-stu-id="ede7d-115">So, you can create a logic app that uses the flat file encoder to "convert" XML files to flat files.</span></span> 
* <span data-ttu-id="ede7d-116">[Výraz XPath](https://msdn.microsoft.com/library/mt643789.aspx) – zlepšit komunikaci oddělení zprávu a extrahovat specifické vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="ede7d-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - Enrich a message and extract specific properties from the message.</span></span> <span data-ttu-id="ede7d-117">Potom můžete extrahované vlastnosti pro odesílání zpráv do cílového umístění, nebo zprostředkující koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ede7d-117">You can then use the extracted properties to route the message to a destination, or an intermediary endpoint.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="ede7d-118">Vyzkoušet</span><span class="sxs-lookup"><span data-stu-id="ede7d-118">Try it out</span></span>
<span data-ttu-id="ede7d-119">[Nasazení aplikace logiky plně funkční ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (ukázka Githubu) pomocí funkcí XML v Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="ede7d-119">[Deploy a fully operational logic app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub sample) by using the XML features in Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="ede7d-120">Další informace</span><span class="sxs-lookup"><span data-stu-id="ede7d-120">Learn more</span></span>
[<span data-ttu-id="ede7d-121">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="ede7d-121">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")
