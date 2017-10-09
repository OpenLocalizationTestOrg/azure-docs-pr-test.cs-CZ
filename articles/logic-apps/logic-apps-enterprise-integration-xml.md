---
title: "aaaWorking se souborem XML zprávy ve svých pracovních postupech - Azure Logic Apps | Microsoft Docs"
description: "Zpracovat, ověřit, transformace a zlepšit komunikaci oddělení XML zprávy aplikace logiky a obchodní tooscenarios pomocí hello Enterprise integračního balíčku"
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
ms.openlocfilehash: f90ae89fef0a4bd17286adbce398e573940bb790
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a><span data-ttu-id="f1494-103">Ověření a transformace XML, kódování a dekódování plochých souborů a rozšířit funkce zprávy v aplikacích logiky</span><span class="sxs-lookup"><span data-stu-id="f1494-103">Validate and transform XML, encode and decode flat files, and enrich messages features in logic apps</span></span>

<span data-ttu-id="f1494-104">Použití aplikace logiky, máte hello možnost tooprocess XML zprávy, které odesílat a přijímat.</span><span class="sxs-lookup"><span data-stu-id="f1494-104">Using logic apps, you have hello ability tooprocess XML messages that you send and receive.</span></span> <span data-ttu-id="f1494-105">Tato funkce je součástí hello Enterprise integračního balíčku.</span><span class="sxs-lookup"><span data-stu-id="f1494-105">This feature is included with hello Enterprise Integration Pack.</span></span> <span data-ttu-id="f1494-106">Pro tyto uživatele s pozadí BizTalk Server hello Enterprise integrační balíček poskytuje podobné tootransform dalo a ověření zprávy, pracovat s plochých souborů a i pomocí jazyka XPath tooenrich nebo extrahuje specifické vlastnosti ze zprávy.</span><span class="sxs-lookup"><span data-stu-id="f1494-106">For those users with a BizTalk Server background, hello Enterprise Integration Pack gives you similar abilities tootransform and validate messages, work with flat files, and even use XPath tooenrich or extract specific properties from a message.</span></span> 

<span data-ttu-id="f1494-107">Pro uživatele, kteří jsou nový prostor toothis rozbalte tyto funkce, jak při zpracování zprávy v rámci pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="f1494-107">For those users who are new toothis space, these features expand how you process messages within your workflow.</span></span> <span data-ttu-id="f1494-108">Například pokud se v případě business-to-business a pracovat s konkrétní schémat XML, můžete použít hello Enterprise integračního balíčku tooenhance jak vaše společnost zpracovává tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="f1494-108">For example, if you are in a business-to-business scenario, and work with specific XML schemas, then you can use hello Enterprise Integration Pack tooenhance how your company processes these messages.</span></span> 

<span data-ttu-id="f1494-109">Hello Enterprise integrační balíček obsahuje:</span><span class="sxs-lookup"><span data-stu-id="f1494-109">hello Enterprise Integration Pack includes:</span></span> 

* <span data-ttu-id="f1494-110">[Ověření XML](logic-apps-enterprise-integration-xml-validation.md "Další informace o ověření XML zprávy") -ověření příchozích nebo odchozích XML zprávy pro konkrétní schéma.</span><span class="sxs-lookup"><span data-stu-id="f1494-110">[XML validation](logic-apps-enterprise-integration-xml-validation.md "Learn about XML message validation") - Validate an incoming or outgoing XML message against a specific schema.</span></span>
* <span data-ttu-id="f1494-111">[Transformace XML](../logic-apps/logic-apps-enterprise-integration-transform.md "Další informace o mapování a transformace zprávy XML") – převést nebo úprava zprávy XML na základě požadavky nebo požadavky hello partnera.</span><span class="sxs-lookup"><span data-stu-id="f1494-111">[XML transform](../logic-apps/logic-apps-enterprise-integration-transform.md "Learn about XML message transformations and maps") - Convert or customize an XML message based on your requirements, or hello requirements of a partner.</span></span>
* <span data-ttu-id="f1494-112">[Plochý soubor kódování a dekódování plochý soubor](logic-apps-enterprise-integration-flatfile.md "Další informace o plochý soubor kódování a dekódování") – zakódování nebo dekódování plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="f1494-112">[Flat file encoding and flat file decoding](logic-apps-enterprise-integration-flatfile.md "Learn about flat file encoding/decoding") - Encode or decode a flat file.</span></span> <span data-ttu-id="f1494-113">Například SAP přijímá a odesílá IDOC soubory ve formátu plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="f1494-113">For example, SAP accepts and sends IDOC files in flat file format.</span></span> <span data-ttu-id="f1494-114">Řada platforem integrace vytvořit XML zprávy, včetně Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="f1494-114">Many integration platforms create XML messages, including Logic Apps.</span></span> <span data-ttu-id="f1494-115">Ano můžete vytvořit aplikaci logiky, že kodér plochý soubor hello používá příliš "převést" soubory tooflat soubory XML.</span><span class="sxs-lookup"><span data-stu-id="f1494-115">So, you can create a logic app that uses hello flat file encoder too"convert" XML files tooflat files.</span></span> 
* <span data-ttu-id="f1494-116">[Výraz XPath](https://msdn.microsoft.com/library/mt643789.aspx) – zlepšit komunikaci oddělení zprávu a extrahovat specifické vlastnosti z uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="f1494-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - Enrich a message and extract specific properties from hello message.</span></span> <span data-ttu-id="f1494-117">Můžete pak použijte hello extrahovat vlastnosti tooroute hello zpráva tooa cílového nebo zprostředkující koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f1494-117">You can then use hello extracted properties tooroute hello message tooa destination, or an intermediary endpoint.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="f1494-118">Vyzkoušet</span><span class="sxs-lookup"><span data-stu-id="f1494-118">Try it out</span></span>
<span data-ttu-id="f1494-119">[Nasazení aplikace logiky plně funkční ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (ukázka Githubu) pomocí funkcí hello XML v Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="f1494-119">[Deploy a fully operational logic app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub sample) by using hello XML features in Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="f1494-120">Další informace</span><span class="sxs-lookup"><span data-stu-id="f1494-120">Learn more</span></span>
[<span data-ttu-id="f1494-121">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="f1494-121">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")
