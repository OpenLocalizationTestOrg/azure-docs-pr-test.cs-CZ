---
title: "chování aaaOverride HTTP pomocí stroj pravidel Azure CDN hello | Microsoft Docs"
description: "Stroj pravidel Hello vám umožní toocustomize zpracování požadavků HTTP pomocí Azure CDN, třeba blokování hello doručování určité typy obsahu, definovat zásady ukládání do mezipaměti a změnit hlavičky protokolu HTTP."
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
ms.openlocfilehash: dd7194be9dbda43180c64568d3e1f52c5c513a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="override-http-behavior-using-hello-azure-cdn-rules-engine"></a><span data-ttu-id="1002b-103">Potlačení chování HTTP pomocí stroj pravidel hello Azure CDN</span><span class="sxs-lookup"><span data-stu-id="1002b-103">Override HTTP behavior using hello Azure CDN rules engine</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="1002b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1002b-104">Overview</span></span>
<span data-ttu-id="1002b-105">Stroj pravidel Hello umožňuje toocustomize způsob zpracování požadavků HTTP, jako je například blokování hello doručování určité typy obsahu, definování zásady ukládání do mezipaměti a úpravy hlaviček protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1002b-105">hello rules engine allows you toocustomize how HTTP requests are handled, such as blocking hello delivery of certain types of content, defining a caching policy, and modifying HTTP headers.</span></span>  <span data-ttu-id="1002b-106">V tomto kurzu se ukazují, že vytvoření pravidla, která se změní hello ukládání do mezipaměti chování CDN prostředků.</span><span class="sxs-lookup"><span data-stu-id="1002b-106">This tutorial will demonstrate creating a rule that will change hello caching behavior of CDN assets.</span></span>  <span data-ttu-id="1002b-107">Je také video obsah k dispozici v hello "[viz také](#see-also)" oddílu.</span><span class="sxs-lookup"><span data-stu-id="1002b-107">There's also video content available in hello "[See also](#see-also)" section.</span></span>

   > [!TIP] 
   > <span data-ttu-id="1002b-108">Odkaz na syntaxi toohello podrobně naleznete v tématu [referenční dokumentace pravidel modul](cdn-rules-engine-reference.md).</span><span class="sxs-lookup"><span data-stu-id="1002b-108">For a reference toohello syntax in detail, see [Rules Engine Reference](cdn-rules-engine-reference.md).</span></span>
   > 


## <a name="tutorial"></a><span data-ttu-id="1002b-109">Kurz</span><span class="sxs-lookup"><span data-stu-id="1002b-109">Tutorial</span></span>
1. <span data-ttu-id="1002b-110">Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1002b-110">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    <span data-ttu-id="1002b-112">Otevře portál pro správu Hello CDN.</span><span class="sxs-lookup"><span data-stu-id="1002b-112">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="1002b-113">Klikněte na hello **HTTP velké** kartě, za nímž následuje **stroj pravidel**.</span><span class="sxs-lookup"><span data-stu-id="1002b-113">Click on hello **HTTP Large** tab, followed by **Rules Engine**.</span></span>
   
    <span data-ttu-id="1002b-114">Zobrazí se možnosti pro nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="1002b-114">Options for a new rule are displayed.</span></span>
   
    ![Nové možnosti pravidlo CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="1002b-116">Hello pořadí, ve kterém jsou uvedeny víc pravidel, ovlivňuje způsob zpracování.</span><span class="sxs-lookup"><span data-stu-id="1002b-116">hello order in which multiple rules are listed affects how they are handled.</span></span> <span data-ttu-id="1002b-117">Následující pravidlo může přepsat hello akce zadané předchozí pravidlem.</span><span class="sxs-lookup"><span data-stu-id="1002b-117">A subsequent rule may override hello actions specified by a previous rule.</span></span>
   > 
   > 
3. <span data-ttu-id="1002b-118">Zadejte název v hello **název nebo popis** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1002b-118">Enter a name in hello **Name / Description** textbox.</span></span>
4. <span data-ttu-id="1002b-119">Identifikace typu hello požadavků, které hello pravidlo se použije k.</span><span class="sxs-lookup"><span data-stu-id="1002b-119">Identify hello type of requests hello rule will apply to.</span></span>  <span data-ttu-id="1002b-120">Ve výchozím nastavení, hello **vždy** je vybraná podmínka shodu.</span><span class="sxs-lookup"><span data-stu-id="1002b-120">By default, hello **Always** match condition is selected.</span></span>  <span data-ttu-id="1002b-121">Budete používat **vždy** pro účely tohoto kurzu, takže políčko nechte který vybrali.</span><span class="sxs-lookup"><span data-stu-id="1002b-121">You'll use **Always** for this tutorial, so leave that selected.</span></span>
   
   ![Stav shody CDN](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > <span data-ttu-id="1002b-123">Existuje mnoho typů odpovídal podmínky, které jsou k dispozici v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="1002b-123">There are many types of match conditions available in hello dropdown.</span></span>  <span data-ttu-id="1002b-124">Kliknete na informační ikonu toohello hello blue nalevo od hello shodu podmínku vysvětluje podmínku hello aktuálně vybrané podrobně.</span><span class="sxs-lookup"><span data-stu-id="1002b-124">Clicking on hello blue informational icon toohello left of hello match condition will explain hello currently selected condition in detail.</span></span>
   > 
   >  <span data-ttu-id="1002b-125">Úplný seznam hello podmíněných výrazů podrobně najdete v tématu [pravidla modul podmíněné výrazy](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="1002b-125">For hello full list of conditional expressions in detail, see [Rules Engine Conditional Expressions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   >  
   > <span data-ttu-id="1002b-126">Úplný seznam hello shodu podmínek podrobně najdete v tématu [podmínky odpovídají stroj pravidel](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="1002b-126">For hello full list of match conditions in detail, see [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   > 
   > 
5. <span data-ttu-id="1002b-127">Klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce** tooadd nové funkce.</span><span class="sxs-lookup"><span data-stu-id="1002b-127">Click hello **+** button next too**Features** tooadd a new feature.</span></span>  <span data-ttu-id="1002b-128">V rozevírací nabídce hello na levé straně hello, vyberte **Force interní Max-Age**.</span><span class="sxs-lookup"><span data-stu-id="1002b-128">In hello dropdown on hello left, select **Force Internal Max-Age**.</span></span>  <span data-ttu-id="1002b-129">V textovém poli hello, který se zobrazí, zadejte **300**.</span><span class="sxs-lookup"><span data-stu-id="1002b-129">In hello textbox that appears, enter **300**.</span></span>  <span data-ttu-id="1002b-130">Ponechejte zbývající výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="1002b-130">Leave hello remaining default values.</span></span>
   
   ![Funkce CDN](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > <span data-ttu-id="1002b-132">Jako s podmínkami shoda, kliknete na informační ikonu toohello hello blue levé hello nové funkce zobrazí podrobnosti o této funkci.</span><span class="sxs-lookup"><span data-stu-id="1002b-132">As with match conditions, clicking hello blue informational icon toohello left of hello new feature will display details about this feature.</span></span>  <span data-ttu-id="1002b-133">V případě hello **Force interní Max-Age**, jsme jsou přepsání hello asset **Cache-Control** a **Expires** hlavičky toocontrol při hello CDN hraniční uzel aktualizuje hello Asset z počátku hello.</span><span class="sxs-lookup"><span data-stu-id="1002b-133">In hello case of **Force Internal Max-Age**, we are overriding hello asset's **Cache-Control** and **Expires** headers toocontrol when hello CDN edge node will refresh hello asset from hello origin.</span></span>  <span data-ttu-id="1002b-134">Náš příklad 300 sekund znamená, že hello CDN hraniční uzel po dobu 5 minut před aktualizací hello asset z jeho počátku mezipaměti hello asset.</span><span class="sxs-lookup"><span data-stu-id="1002b-134">Our example of 300 seconds means hello CDN edge node will cache hello asset for 5 minutes before refreshing hello asset from its origin.</span></span>
   > 
   > <span data-ttu-id="1002b-135">Hello úplný seznam funkcí podrobně najdete v tématu [podrobnosti o funkcích stroj pravidel](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="1002b-135">For hello full list of features in detail, see [Rules Engine Feature Details](cdn-rules-engine-reference-features.md).</span></span>
   > 
   > 
6. <span data-ttu-id="1002b-136">Klikněte na tlačítko hello **přidat** tlačítko toosave hello nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="1002b-136">Click hello **Add** button toosave hello new rule.</span></span>  <span data-ttu-id="1002b-137">nové pravidlo Hello je nyní čeká na schválení.</span><span class="sxs-lookup"><span data-stu-id="1002b-137">hello new rule is now awaiting approval.</span></span> <span data-ttu-id="1002b-138">Jakmile byla schválena, hello stav se změní z **čekající XML** příliš**Active XML**.</span><span class="sxs-lookup"><span data-stu-id="1002b-138">Once it has been approved, hello status will change from **Pending XML** too**Active XML**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="1002b-139">Změny pravidel může trvat až minut toopropagate too90 prostřednictvím hello CDN.</span><span class="sxs-lookup"><span data-stu-id="1002b-139">Rules changes may take up too90 minutes toopropagate through hello CDN.</span></span>
   > 
   > 

## <a name="see-also"></a><span data-ttu-id="1002b-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="1002b-140">See also</span></span>
* [<span data-ttu-id="1002b-141">Přehled Azure CDN</span><span class="sxs-lookup"><span data-stu-id="1002b-141">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="1002b-142">Referenční dokumentace pravidel modulu</span><span class="sxs-lookup"><span data-stu-id="1002b-142">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="1002b-143">Stav shody motoru pravidla</span><span class="sxs-lookup"><span data-stu-id="1002b-143">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="1002b-144">Podmíněné výrazy stroj pravidel</span><span class="sxs-lookup"><span data-stu-id="1002b-144">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="1002b-145">Funkce modulu pravidla</span><span class="sxs-lookup"><span data-stu-id="1002b-145">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="1002b-146">Přepsání výchozího nastavení HTTP používá stroj pravidel hello</span><span class="sxs-lookup"><span data-stu-id="1002b-146">Overriding default HTTP behavior using hello rules engine</span></span>](cdn-rules-engine.md)
* <span data-ttu-id="1002b-147">[Azure pátek: Azure CDN výkonné nové prémiové funkce](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span><span class="sxs-lookup"><span data-stu-id="1002b-147">[Azure Fridays: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span></span>