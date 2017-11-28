---
title: "data XML aaaConvert s transformací - Azure Logic Apps | Microsoft Docs"
description: "Vytvořit transformací nebo mapps data XML tooconvert mezi formáty v aplikacích logiky pomocí hello Enterprise integraci sady SDK"
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
ms.openlocfilehash: b56ec1072c5058d3aefc7f88ac9b2748ebe56456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a><span data-ttu-id="5f854-103">Enterprise integrace s transformace XML</span><span class="sxs-lookup"><span data-stu-id="5f854-103">Enterprise integration with XML transforms</span></span>
## <a name="overview"></a><span data-ttu-id="5f854-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5f854-104">Overview</span></span>
<span data-ttu-id="5f854-105">Hello Enterprise integrace transformace konektor převádí data z jednoho formátu tooanother formátu.</span><span class="sxs-lookup"><span data-stu-id="5f854-105">hello Enterprise integration Transform connector converts data from one format tooanother format.</span></span> <span data-ttu-id="5f854-106">Například můžete mít příchozí zprávy, která obsahuje aktuální datum ve formátu YearMonthDay hello hello.</span><span class="sxs-lookup"><span data-stu-id="5f854-106">For example, you may have an incoming message that contains hello current date in hello YearMonthDay format.</span></span> <span data-ttu-id="5f854-107">Můžete vytvořit toobe transformace tooreformat hello datum ve formátu MonthDayYear hello.</span><span class="sxs-lookup"><span data-stu-id="5f854-107">You can use a transform tooreformat hello date toobe in hello MonthDayYear format.</span></span>

## <a name="what-does-a-transform-do"></a><span data-ttu-id="5f854-108">Transformace k čemu slouží?</span><span class="sxs-lookup"><span data-stu-id="5f854-108">What does a transform do?</span></span>
<span data-ttu-id="5f854-109">Transformace, která je také označována jako mapu, se skládá z schéma XML pro zdroj (hello vstup) a cíl XML schema (výstup hello).</span><span class="sxs-lookup"><span data-stu-id="5f854-109">A Transform, which is also known as a map, consists of a Source XML schema (hello input) and a Target XML schema (hello output).</span></span> <span data-ttu-id="5f854-110">Různé integrované funkce toohelp pracovat s nebo řízení hello data, můžete použít, včetně manipulace s řetězci, podmíněného přiřazení, aritmetických výrazech, datum čas formátování a i opakování ve smyčce konstrukce.</span><span class="sxs-lookup"><span data-stu-id="5f854-110">You can use different built-in functions toohelp manipulate or control hello data, including string manipulations, conditional assignments, arithmetic expressions, date time formatters, and even looping constructs.</span></span>

## <a name="how-toocreate-a-transform"></a><span data-ttu-id="5f854-111">Jak toocreate transformace?</span><span class="sxs-lookup"><span data-stu-id="5f854-111">How toocreate a transform?</span></span>
<span data-ttu-id="5f854-112">Transformace nebo mapu můžete vytvořit pomocí sady Visual Studio hello [Enterprise integrace SDK](https://aka.ms/vsmapsandschemas).</span><span class="sxs-lookup"><span data-stu-id="5f854-112">You can create a transform/map by using hello Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span></span> <span data-ttu-id="5f854-113">Po dokončení vytváření a testování hello transformace nahrajete do účtu integrace hello transformace.</span><span class="sxs-lookup"><span data-stu-id="5f854-113">When you are finished creating and testing hello transform, you upload hello transform into your integration account.</span></span> 

## <a name="how-toouse-a-transform"></a><span data-ttu-id="5f854-114">Jak toouse transformace</span><span class="sxs-lookup"><span data-stu-id="5f854-114">How toouse a transform</span></span>
<span data-ttu-id="5f854-115">Po odeslání hello transformace/mapovat do účtu integrace, můžete ji použít toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5f854-115">After you upload hello transform/map into your integration account, you can use it toocreate a Logic app.</span></span> <span data-ttu-id="5f854-116">aplikace logiky Hello běží vaše transformace vždy, když se aktivuje aplikaci logiky hello (a vstupní obsah, který potřebuje toobe Transformovat).</span><span class="sxs-lookup"><span data-stu-id="5f854-116">hello Logic app runs your transformations whenever hello Logic app is triggered (and there is input content that needs toobe transformed).</span></span>

<span data-ttu-id="5f854-117">**Tady jsou kroky toouse hello transformace**:</span><span class="sxs-lookup"><span data-stu-id="5f854-117">**Here are hello steps toouse a transform**:</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5f854-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f854-118">Prerequisites</span></span>

* <span data-ttu-id="5f854-119">Vytvoření účtu integrace a přidejte tooit mapy</span><span class="sxs-lookup"><span data-stu-id="5f854-119">Create an integration account and add a map tooit</span></span>  

<span data-ttu-id="5f854-120">Teď, když jste postaráno, hello požadavky, je čas toocreate svou aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="5f854-120">Now that you've taken care of hello prerequisites, it's time toocreate your Logic app:</span></span>  

1. <span data-ttu-id="5f854-121">Vytvoření aplikace logiky a [propojit účet integrace tooyour](../logic-apps/logic-apps-enterprise-integration-accounts.md "další toolink aplikace logiky integrace účet tooa") obsahující hello mapy.</span><span class="sxs-lookup"><span data-stu-id="5f854-121">Create a Logic app and [link it tooyour integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app") that contains hello map.</span></span>
2. <span data-ttu-id="5f854-122">Přidat **požadavku** aplikace logiky tooyour aktivační události</span><span class="sxs-lookup"><span data-stu-id="5f854-122">Add a **Request** trigger tooyour Logic app</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. <span data-ttu-id="5f854-123">Přidat hello **transformace XML** akce první výběrem **přidat akci** </span><span class="sxs-lookup"><span data-stu-id="5f854-123">Add hello **Transform XML** action by first selecting **Add an action** </span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. <span data-ttu-id="5f854-124">Zadejte slova hello *transformace* v toofilter pole hledání hello všechny akce toohello, jednu, které chcete toouse hello</span><span class="sxs-lookup"><span data-stu-id="5f854-124">Enter hello word *transform* in hello search box toofilter all hello actions toohello one that you want toouse</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. <span data-ttu-id="5f854-125">Vyberte hello **transformace XML** akce</span><span class="sxs-lookup"><span data-stu-id="5f854-125">Select hello **Transform XML** action</span></span>   
6. <span data-ttu-id="5f854-126">Přidat hello XML **obsahu** , můžete transformace.</span><span class="sxs-lookup"><span data-stu-id="5f854-126">Add hello XML **CONTENT** that you transform.</span></span> <span data-ttu-id="5f854-127">Můžete použít libovolná data XML, který se zobrazí v žádosti o hello HTTP jako hello **obsahu**.</span><span class="sxs-lookup"><span data-stu-id="5f854-127">You can use any XML data you receive in hello HTTP request as hello **CONTENT**.</span></span> <span data-ttu-id="5f854-128">V tomto příkladu vyberte hello textu hello požadavku protokolu HTTP, který aktivoval aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="5f854-128">In this example, select hello body of hello HTTP request that triggered hello Logic app.</span></span>
7. <span data-ttu-id="5f854-129">Vyberte hello název hello **MAPY** , které chcete toouse tooperform hello transformace.</span><span class="sxs-lookup"><span data-stu-id="5f854-129">Select hello name of hello **MAP** that you want toouse tooperform hello transformation.</span></span> <span data-ttu-id="5f854-130">Mapa Hello již musí být v účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="5f854-130">hello map must already be in your integration account.</span></span> <span data-ttu-id="5f854-131">V předchozím kroku dáte již vaše logiku aplikace přístup tooyour integrace účet, který obsahuje mapu.</span><span class="sxs-lookup"><span data-stu-id="5f854-131">In an earlier step, you already gave your Logic app access tooyour integration account that contains your map.</span></span>      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. <span data-ttu-id="5f854-132">Uložte si práci</span><span class="sxs-lookup"><span data-stu-id="5f854-132">Save your work</span></span>  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

<span data-ttu-id="5f854-133">V tomto okamžiku jste dokončili nastavení mapy.</span><span class="sxs-lookup"><span data-stu-id="5f854-133">At this point, you are finished setting up your map.</span></span> <span data-ttu-id="5f854-134">V reálné aplikaci můžete chtít toostore hello transformovat data obchodní aplikace, například služby SalesForce.</span><span class="sxs-lookup"><span data-stu-id="5f854-134">In a real world application, you may want toostore hello transformed data in an LOB application such as SalesForce.</span></span> <span data-ttu-id="5f854-135">Můžete snadno jako výstup hello toosend akce hello transformace tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="5f854-135">You can easily as an action toosend hello output of hello transform tooSalesforce.</span></span> 

<span data-ttu-id="5f854-136">Nyní můžete otestovat váš transformace tak, že koncový bod žádosti toohello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f854-136">You can now test your transform by making a request toohello HTTP endpoint.</span></span>  

## <a name="features-and-use-cases"></a><span data-ttu-id="5f854-137">Funkce a případy použití</span><span class="sxs-lookup"><span data-stu-id="5f854-137">Features and use cases</span></span>
* <span data-ttu-id="5f854-138">Transformace Hello vytvořené v mapu může být jednoduchý, jako je například kopírování názvu a adresy z jednoho tooanother dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5f854-138">hello transformation created in a map can be simple, such as copying a name and address from one document tooanother.</span></span> <span data-ttu-id="5f854-139">Nebo můžete vytvořit složitější transformace pomocí operace hello se na pole mapy.</span><span class="sxs-lookup"><span data-stu-id="5f854-139">Or, you can create more complex transformations using hello out-of-the-box map operations.</span></span>  
* <span data-ttu-id="5f854-140">Více mapy operace nebo funkce jsou snadno dostupné, včetně řetězce, datum časové funkce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5f854-140">Multiple map operations or functions are readily available, including strings, date time functions, and so on.</span></span>  
* <span data-ttu-id="5f854-141">Můžete to udělat kopii přímá dat mezi hello schémat.</span><span class="sxs-lookup"><span data-stu-id="5f854-141">You can do a direct data copy between hello schemas.</span></span> <span data-ttu-id="5f854-142">V hello Mapper součástí hello SDK to je jednoduché, kreslení čáry propojující hello elementy ve schématu zdroje hello se svými protějšky ve schématu cílové hello.</span><span class="sxs-lookup"><span data-stu-id="5f854-142">In hello Mapper included in hello SDK, this is as simple as drawing a line that connects hello elements in hello source schema with their counterparts in hello destination schema.</span></span>  
* <span data-ttu-id="5f854-143">Při vytváření mapy, zobrazí grafické reprezentace hello map, který zobrazuje všechny vztahy hello a odkazy, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="5f854-143">When creating a map, you view a graphical representation of hello map, which shows all hello relationships and links you create.</span></span>
* <span data-ttu-id="5f854-144">Použijte hello testovací mapy funkce tooadd ukázkovou zprávu XML.</span><span class="sxs-lookup"><span data-stu-id="5f854-144">Use hello Test Map feature tooadd a sample XML message.</span></span> <span data-ttu-id="5f854-145">S jediným kliknutím můžete otestovat hello mapy jste vytvořili a výstup hello vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="5f854-145">With a simple click, you can test hello map you created, and see hello generated output.</span></span>  
* <span data-ttu-id="5f854-146">Nahrát existující mapy</span><span class="sxs-lookup"><span data-stu-id="5f854-146">Upload existing maps</span></span>  
* <span data-ttu-id="5f854-147">Zahrnuje podporu pro formát XML hello.</span><span class="sxs-lookup"><span data-stu-id="5f854-147">Includes support for hello XML format.</span></span>

## <a name="learn-more"></a><span data-ttu-id="5f854-148">Další informace</span><span class="sxs-lookup"><span data-stu-id="5f854-148">Learn more</span></span>
* [<span data-ttu-id="5f854-149">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="5f854-149">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [<span data-ttu-id="5f854-150">Další informace o mapování</span><span class="sxs-lookup"><span data-stu-id="5f854-150">Learn more about maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")  

