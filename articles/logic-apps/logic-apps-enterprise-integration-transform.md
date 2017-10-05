---
title: "Převést data XML s transformací - Azure Logic Apps | Microsoft Docs"
description: "Vytvořit transformací nebo mapps převést data XML mezi formáty v logiku aplikace pomocí sady SDK integrace Enterprise"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: fb6027769377b3527b11f7831dab3bb8d7061c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a><span data-ttu-id="52fdd-103">Enterprise integrace s transformace XML</span><span class="sxs-lookup"><span data-stu-id="52fdd-103">Enterprise integration with XML transforms</span></span>
## <a name="overview"></a><span data-ttu-id="52fdd-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="52fdd-104">Overview</span></span>
<span data-ttu-id="52fdd-105">Konektor Enterprise integrace transformace převádí data z jednoho formátu do jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="52fdd-105">The Enterprise integration Transform connector converts data from one format to another format.</span></span> <span data-ttu-id="52fdd-106">Například můžete mít příchozí zprávy obsahující aktuální datum ve formátu YearMonthDay.</span><span class="sxs-lookup"><span data-stu-id="52fdd-106">For example, you may have an incoming message that contains the current date in the YearMonthDay format.</span></span> <span data-ttu-id="52fdd-107">Transformace můžete přeformátujte datum ve formátu MonthDayYear.</span><span class="sxs-lookup"><span data-stu-id="52fdd-107">You can use a transform to reformat the date to be in the MonthDayYear format.</span></span>

## <a name="what-does-a-transform-do"></a><span data-ttu-id="52fdd-108">Transformace k čemu slouží?</span><span class="sxs-lookup"><span data-stu-id="52fdd-108">What does a transform do?</span></span>
<span data-ttu-id="52fdd-109">Transformace, která je také označována jako mapu, se skládá z schéma XML pro zdroj (vstup) a cíl XML schema (výstup).</span><span class="sxs-lookup"><span data-stu-id="52fdd-109">A Transform, which is also known as a map, consists of a Source XML schema (the input) and a Target XML schema (the output).</span></span> <span data-ttu-id="52fdd-110">Různé integrované funkce můžete použít k manipulaci nebo řízení dat, včetně manipulace s řetězci, podmíněného přiřazení, aritmetických výrazech, datum čas formátování a i opakování ve smyčce konstrukce.</span><span class="sxs-lookup"><span data-stu-id="52fdd-110">You can use different built-in functions to help manipulate or control the data, including string manipulations, conditional assignments, arithmetic expressions, date time formatters, and even looping constructs.</span></span>

## <a name="how-to-create-a-transform"></a><span data-ttu-id="52fdd-111">Postup vytvoření transformace?</span><span class="sxs-lookup"><span data-stu-id="52fdd-111">How to create a transform?</span></span>
<span data-ttu-id="52fdd-112">Transformace nebo mapu můžete vytvořit pomocí sady Visual Studio [Enterprise integrace SDK](https://aka.ms/vsmapsandschemas).</span><span class="sxs-lookup"><span data-stu-id="52fdd-112">You can create a transform/map by using the Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span></span> <span data-ttu-id="52fdd-113">Po dokončení vytváření a testování pro transformaci nahrajete do účtu integrace pro transformaci.</span><span class="sxs-lookup"><span data-stu-id="52fdd-113">When you are finished creating and testing the transform, you upload the transform into your integration account.</span></span> 

## <a name="how-to-use-a-transform"></a><span data-ttu-id="52fdd-114">Postup použití transformace</span><span class="sxs-lookup"><span data-stu-id="52fdd-114">How to use a transform</span></span>
<span data-ttu-id="52fdd-115">Po odeslání transformace nebo mapy ke svému účtu integrace, můžete k vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="52fdd-115">After you upload the transform/map into your integration account, you can use it to create a Logic app.</span></span> <span data-ttu-id="52fdd-116">Aplikace logiky běží vaše transformace vždy, když se aktivuje aplikaci logiky (a vstupní obsah, který potřebuje k transformaci).</span><span class="sxs-lookup"><span data-stu-id="52fdd-116">The Logic app runs your transformations whenever the Logic app is triggered (and there is input content that needs to be transformed).</span></span>

<span data-ttu-id="52fdd-117">**Tady jsou kroky při použití transformace**:</span><span class="sxs-lookup"><span data-stu-id="52fdd-117">**Here are the steps to use a transform**:</span></span>

### <a name="prerequisites"></a><span data-ttu-id="52fdd-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="52fdd-118">Prerequisites</span></span>

* <span data-ttu-id="52fdd-119">Vytvoření účtu integrace a přidejte do ní mapy</span><span class="sxs-lookup"><span data-stu-id="52fdd-119">Create an integration account and add a map to it</span></span>  

<span data-ttu-id="52fdd-120">Teď, když jste postaráno, požadované součásti, je čas vytvořit svou aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="52fdd-120">Now that you've taken care of the prerequisites, it's time to create your Logic app:</span></span>  

1. <span data-ttu-id="52fdd-121">Vytvoření aplikace logiky a [ho propojit se svým účtem integrace](../logic-apps/logic-apps-enterprise-integration-accounts.md "zjistěte, jak lze propojit účet integrace aplikace logiky") obsahující mapy.</span><span class="sxs-lookup"><span data-stu-id="52fdd-121">Create a Logic app and [link it to your integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn to link an integration account to a Logic app") that contains the map.</span></span>
2. <span data-ttu-id="52fdd-122">Přidat **požadavku** aktivační události do aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="52fdd-122">Add a **Request** trigger to your Logic app</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. <span data-ttu-id="52fdd-123">Přidat **transformace XML** akce první výběrem **přidat akci** </span><span class="sxs-lookup"><span data-stu-id="52fdd-123">Add the **Transform XML** action by first selecting **Add an action** </span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. <span data-ttu-id="52fdd-124">Zadejte slovo *transformace* do vyhledávacího pole vyfiltrujete všechny akce, které ten, který chcete použít</span><span class="sxs-lookup"><span data-stu-id="52fdd-124">Enter the word *transform* in the search box to filter all the actions to the one that you want to use</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. <span data-ttu-id="52fdd-125">Vyberte **transformace XML** akce</span><span class="sxs-lookup"><span data-stu-id="52fdd-125">Select the **Transform XML** action</span></span>   
6. <span data-ttu-id="52fdd-126">Přidání souboru XML **obsahu** , můžete transformace.</span><span class="sxs-lookup"><span data-stu-id="52fdd-126">Add the XML **CONTENT** that you transform.</span></span> <span data-ttu-id="52fdd-127">Můžete použít libovolná data XML, který se zobrazí v požadavku HTTP, jako **obsahu**.</span><span class="sxs-lookup"><span data-stu-id="52fdd-127">You can use any XML data you receive in the HTTP request as the **CONTENT**.</span></span> <span data-ttu-id="52fdd-128">V tomto příkladu vyberte textu požadavku HTTP, který aktivoval aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="52fdd-128">In this example, select the body of the HTTP request that triggered the Logic app.</span></span>
7. <span data-ttu-id="52fdd-129">Vyberte název **MAPY** , kterou chcete použít k provedení transformace.</span><span class="sxs-lookup"><span data-stu-id="52fdd-129">Select the name of the **MAP** that you want to use to perform the transformation.</span></span> <span data-ttu-id="52fdd-130">Mapy již musí být v účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="52fdd-130">The map must already be in your integration account.</span></span> <span data-ttu-id="52fdd-131">V předchozím kroku dáte již logiku aplikace přístup k vašemu účtu integrace, která obsahuje vaše mapy.</span><span class="sxs-lookup"><span data-stu-id="52fdd-131">In an earlier step, you already gave your Logic app access to your integration account that contains your map.</span></span>      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. <span data-ttu-id="52fdd-132">Uložte si práci</span><span class="sxs-lookup"><span data-stu-id="52fdd-132">Save your work</span></span>  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

<span data-ttu-id="52fdd-133">V tomto okamžiku jste dokončili nastavení mapy.</span><span class="sxs-lookup"><span data-stu-id="52fdd-133">At this point, you are finished setting up your map.</span></span> <span data-ttu-id="52fdd-134">V reálné aplikaci můžete ukládat Transformovaná data obchodní aplikace, například služby SalesForce.</span><span class="sxs-lookup"><span data-stu-id="52fdd-134">In a real world application, you may want to store the transformed data in an LOB application such as SalesForce.</span></span> <span data-ttu-id="52fdd-135">Můžete snadno jako akce pro odeslání výstupu transformace do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="52fdd-135">You can easily as an action to send the output of the transform to Salesforce.</span></span> 

<span data-ttu-id="52fdd-136">Nyní můžete otestovat váš transformace tak, že požadavek na koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="52fdd-136">You can now test your transform by making a request to the HTTP endpoint.</span></span>  

## <a name="features-and-use-cases"></a><span data-ttu-id="52fdd-137">Funkce a případy použití</span><span class="sxs-lookup"><span data-stu-id="52fdd-137">Features and use cases</span></span>
* <span data-ttu-id="52fdd-138">Transformace vytvořené v mapu může být jednoduchý, jako je například kopírování názvu a adresy z jednoho dokumentu do jiného.</span><span class="sxs-lookup"><span data-stu-id="52fdd-138">The transformation created in a map can be simple, such as copying a name and address from one document to another.</span></span> <span data-ttu-id="52fdd-139">Nebo můžete vytvořit složitější transformace pomocí operace, které se na pole mapy.</span><span class="sxs-lookup"><span data-stu-id="52fdd-139">Or, you can create more complex transformations using the out-of-the-box map operations.</span></span>  
* <span data-ttu-id="52fdd-140">Více mapy operace nebo funkce jsou snadno dostupné, včetně řetězce, datum časové funkce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="52fdd-140">Multiple map operations or functions are readily available, including strings, date time functions, and so on.</span></span>  
* <span data-ttu-id="52fdd-141">Můžete to udělat kopii přímá dat mezi schémat.</span><span class="sxs-lookup"><span data-stu-id="52fdd-141">You can do a direct data copy between the schemas.</span></span> <span data-ttu-id="52fdd-142">V Mapper zahrnutý v sadě SDK to je jednoduché, kreslení čáry propojující se svými protějšky ve schématu cílové elementy ve schématu zdroje.</span><span class="sxs-lookup"><span data-stu-id="52fdd-142">In the Mapper included in the SDK, this is as simple as drawing a line that connects the elements in the source schema with their counterparts in the destination schema.</span></span>  
* <span data-ttu-id="52fdd-143">Při vytváření mapy, zobrazí se grafické zobrazení mapy, které ukazují vztahy a odkazy, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="52fdd-143">When creating a map, you view a graphical representation of the map, which shows all the relationships and links you create.</span></span>
* <span data-ttu-id="52fdd-144">Použijte funkci Test mapy přidat ukázkovou zprávu XML.</span><span class="sxs-lookup"><span data-stu-id="52fdd-144">Use the Test Map feature to add a sample XML message.</span></span> <span data-ttu-id="52fdd-145">S jediným kliknutím můžete otestovat mapu, kterou jste vytvořili a najdete v části generovaný výstup.</span><span class="sxs-lookup"><span data-stu-id="52fdd-145">With a simple click, you can test the map you created, and see the generated output.</span></span>  
* <span data-ttu-id="52fdd-146">Nahrát existující mapy</span><span class="sxs-lookup"><span data-stu-id="52fdd-146">Upload existing maps</span></span>  
* <span data-ttu-id="52fdd-147">Zahrnuje podporu pro formát XML.</span><span class="sxs-lookup"><span data-stu-id="52fdd-147">Includes support for the XML format.</span></span>

## <a name="learn-more"></a><span data-ttu-id="52fdd-148">Další informace</span><span class="sxs-lookup"><span data-stu-id="52fdd-148">Learn more</span></span>
* [<span data-ttu-id="52fdd-149">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="52fdd-149">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [<span data-ttu-id="52fdd-150">Další informace o mapování</span><span class="sxs-lookup"><span data-stu-id="52fdd-150">Learn more about maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")  

