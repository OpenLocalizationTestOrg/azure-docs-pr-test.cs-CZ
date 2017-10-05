---
title: "Potlačení chování HTTP pomocí stroj pravidel Azure CDN | Microsoft Docs"
description: "Stroj pravidel umožňuje přizpůsobit způsob zpracování požadavků HTTP pomocí Azure CDN, třeba blokování doručování určité typy obsahu, definovat zásady ukládání do mezipaměti a změnit hlavičky protokolu HTTP."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a><span data-ttu-id="f48e4-103">Potlačení chování HTTP pomocí stroj pravidel Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f48e4-103">Override HTTP behavior using the Azure CDN rules engine</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="f48e4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f48e4-104">Overview</span></span>
<span data-ttu-id="f48e4-105">Stroj pravidel umožňuje přizpůsobit způsob zpracování požadavků HTTP, jako je například blokování doručování určité typy obsahu, definování zásady ukládání do mezipaměti a úpravy hlaviček protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f48e4-105">The rules engine allows you to customize how HTTP requests are handled, such as blocking the delivery of certain types of content, defining a caching policy, and modifying HTTP headers.</span></span>  <span data-ttu-id="f48e4-106">V tomto kurzu se ukazují, že vytvoření pravidla, která se změní chování ukládání do mezipaměti CDN prostředků.</span><span class="sxs-lookup"><span data-stu-id="f48e4-106">This tutorial will demonstrate creating a rule that will change the caching behavior of CDN assets.</span></span>  <span data-ttu-id="f48e4-107">Také obsahu videa v není k dispozici "[viz také](#see-also)" oddílu.</span><span class="sxs-lookup"><span data-stu-id="f48e4-107">There's also video content available in the "[See also](#see-also)" section.</span></span>

   > [!TIP] 
   > <span data-ttu-id="f48e4-108">Odkaz na syntaxi podrobně, najdete v části [referenční dokumentace pravidel modul](cdn-rules-engine-reference.md).</span><span class="sxs-lookup"><span data-stu-id="f48e4-108">For a reference to the syntax in detail, see [Rules Engine Reference](cdn-rules-engine-reference.md).</span></span>
   > 


## <a name="tutorial"></a><span data-ttu-id="f48e4-109">Kurz</span><span class="sxs-lookup"><span data-stu-id="f48e4-109">Tutorial</span></span>
1. <span data-ttu-id="f48e4-110">Okno profil CDN, klikněte **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f48e4-110">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    <span data-ttu-id="f48e4-112">Otevře se na portálu pro správu CDN.</span><span class="sxs-lookup"><span data-stu-id="f48e4-112">The CDN management portal opens.</span></span>
2. <span data-ttu-id="f48e4-113">Klikněte na **HTTP velké** kartě, za nímž následuje **stroj pravidel**.</span><span class="sxs-lookup"><span data-stu-id="f48e4-113">Click on the **HTTP Large** tab, followed by **Rules Engine**.</span></span>
   
    <span data-ttu-id="f48e4-114">Zobrazí se možnosti pro nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="f48e4-114">Options for a new rule are displayed.</span></span>
   
    ![Nové možnosti pravidlo CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="f48e4-116">Pořadí, ve kterém jsou uvedeny víc pravidel, ovlivňuje způsob zpracování.</span><span class="sxs-lookup"><span data-stu-id="f48e4-116">The order in which multiple rules are listed affects how they are handled.</span></span> <span data-ttu-id="f48e4-117">Následující pravidlo může přepsat akce zadané předchozí pravidlem.</span><span class="sxs-lookup"><span data-stu-id="f48e4-117">A subsequent rule may override the actions specified by a previous rule.</span></span>
   > 
   > 
3. <span data-ttu-id="f48e4-118">Zadejte název do pole **název nebo popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f48e4-118">Enter a name in the **Name / Description** textbox.</span></span>
4. <span data-ttu-id="f48e4-119">Určete typ požadavky, bude pravidlo platí pro.</span><span class="sxs-lookup"><span data-stu-id="f48e4-119">Identify the type of requests the rule will apply to.</span></span>  <span data-ttu-id="f48e4-120">Ve výchozím nastavení **vždy** je vybraná podmínka shodu.</span><span class="sxs-lookup"><span data-stu-id="f48e4-120">By default, the **Always** match condition is selected.</span></span>  <span data-ttu-id="f48e4-121">Budete používat **vždy** pro účely tohoto kurzu, takže políčko nechte který vybrali.</span><span class="sxs-lookup"><span data-stu-id="f48e4-121">You'll use **Always** for this tutorial, so leave that selected.</span></span>
   
   ![Stav shody CDN](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > <span data-ttu-id="f48e4-123">Existuje mnoho typů odpovídal podmínky, které jsou k dispozici v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="f48e4-123">There are many types of match conditions available in the dropdown.</span></span>  <span data-ttu-id="f48e4-124">Aktuálně vybrané podmínky podrobně vysvětluje kliknutím na ikonu blue informační nalevo od podmínky shody.</span><span class="sxs-lookup"><span data-stu-id="f48e4-124">Clicking on the blue informational icon to the left of the match condition will explain the currently selected condition in detail.</span></span>
   > 
   >  <span data-ttu-id="f48e4-125">Úplný seznam podmíněné výrazy podrobně najdete v tématu [pravidla modul podmíněné výrazy](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="f48e4-125">For the full list of conditional expressions in detail, see [Rules Engine Conditional Expressions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   >  
   > <span data-ttu-id="f48e4-126">Úplný seznam podmínek shodu podrobně, najdete v části [podmínky odpovídají stroj pravidel](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="f48e4-126">For the full list of match conditions in detail, see [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   > 
   > 
5. <span data-ttu-id="f48e4-127">Klikněte  **+**  vedle položky **funkce** přidat nové funkce.</span><span class="sxs-lookup"><span data-stu-id="f48e4-127">Click the **+** button next to **Features** to add a new feature.</span></span>  <span data-ttu-id="f48e4-128">V rozevírací nabídce na levé straně vyberte **Force interní Max-Age**.</span><span class="sxs-lookup"><span data-stu-id="f48e4-128">In the dropdown on the left, select **Force Internal Max-Age**.</span></span>  <span data-ttu-id="f48e4-129">Do textového pole, která se zobrazí, zadejte **300**.</span><span class="sxs-lookup"><span data-stu-id="f48e4-129">In the textbox that appears, enter **300**.</span></span>  <span data-ttu-id="f48e4-130">Ponechejte zbývající výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f48e4-130">Leave the remaining default values.</span></span>
   
   ![Funkce CDN](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > <span data-ttu-id="f48e4-132">Jako s shodu podmínky, kliknutím na ikonu blue informační nalevo od novou funkci se zobrazí podrobnosti o této funkci.</span><span class="sxs-lookup"><span data-stu-id="f48e4-132">As with match conditions, clicking the blue informational icon to the left of the new feature will display details about this feature.</span></span>  <span data-ttu-id="f48e4-133">U **Force interní Max-Age**, jsme jsou přepsání assetu **Cache-Control** a **Expires** hlavičky řídit, kdy bude hraničního uzlu CDN aktualizace asset z tohoto počátku.</span><span class="sxs-lookup"><span data-stu-id="f48e4-133">In the case of **Force Internal Max-Age**, we are overriding the asset's **Cache-Control** and **Expires** headers to control when the CDN edge node will refresh the asset from the origin.</span></span>  <span data-ttu-id="f48e4-134">Náš příklad 300 sekund znamená, že CDN hraničního uzlu po dobu 5 minut před aktualizací asset z jeho počátku mezipaměti asset.</span><span class="sxs-lookup"><span data-stu-id="f48e4-134">Our example of 300 seconds means the CDN edge node will cache the asset for 5 minutes before refreshing the asset from its origin.</span></span>
   > 
   > <span data-ttu-id="f48e4-135">Úplný seznam funkcí podrobně, najdete v části [podrobnosti o funkcích stroj pravidel](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="f48e4-135">For the full list of features in detail, see [Rules Engine Feature Details](cdn-rules-engine-reference-features.md).</span></span>
   > 
   > 
6. <span data-ttu-id="f48e4-136">Klikněte **přidat** tlačítko pro uložení nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="f48e4-136">Click the **Add** button to save the new rule.</span></span>  <span data-ttu-id="f48e4-137">Nové pravidlo je nyní čeká na schválení.</span><span class="sxs-lookup"><span data-stu-id="f48e4-137">The new rule is now awaiting approval.</span></span> <span data-ttu-id="f48e4-138">Jakmile byla schválena, stav se změní z **čekající XML** k **Active XML**.</span><span class="sxs-lookup"><span data-stu-id="f48e4-138">Once it has been approved, the status will change from **Pending XML** to **Active XML**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="f48e4-139">Změny pravidel může trvat až 90 minut rozšíří v rámci CDN.</span><span class="sxs-lookup"><span data-stu-id="f48e4-139">Rules changes may take up to 90 minutes to propagate through the CDN.</span></span>
   > 
   > 

## <a name="see-also"></a><span data-ttu-id="f48e4-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="f48e4-140">See also</span></span>
* [<span data-ttu-id="f48e4-141">Přehled Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f48e4-141">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="f48e4-142">Referenční dokumentace pravidel modulu</span><span class="sxs-lookup"><span data-stu-id="f48e4-142">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="f48e4-143">Stav shody motoru pravidla</span><span class="sxs-lookup"><span data-stu-id="f48e4-143">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="f48e4-144">Podmíněné výrazy stroj pravidel</span><span class="sxs-lookup"><span data-stu-id="f48e4-144">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="f48e4-145">Funkce modulu pravidla</span><span class="sxs-lookup"><span data-stu-id="f48e4-145">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="f48e4-146">Přepsání výchozího nastavení HTTP používá stroj pravidel</span><span class="sxs-lookup"><span data-stu-id="f48e4-146">Overriding default HTTP behavior using the rules engine</span></span>](cdn-rules-engine.md)
* <span data-ttu-id="f48e4-147">[Azure pátek: Azure CDN výkonné nové prémiové funkce](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span><span class="sxs-lookup"><span data-stu-id="f48e4-147">[Azure Fridays: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span></span>