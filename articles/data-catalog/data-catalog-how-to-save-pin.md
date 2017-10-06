---
title: "aaaSave hledání a kód pin datových prostředcích v Azure Data Catalog | Microsoft Docs"
description: "Jak tooarticle zvýraznění možnosti v Azure Data Catalog pro ukládání zdroje dat a datových prostředků pro pozdější použití."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0ad0a31d4b84782fed9d80acc2734912eecd6d74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a><span data-ttu-id="ce1be-103">Uložte hledání a kód pin datových prostředků v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="ce1be-103">Save searches and pin data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="ce1be-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="ce1be-104">Introduction</span></span>
<span data-ttu-id="ce1be-105">Azure Data Catalog poskytuje možnosti pro zjišťování zdroje.</span><span class="sxs-lookup"><span data-stu-id="ce1be-105">Azure Data Catalog provides capabilities for data source discovery.</span></span> <span data-ttu-id="ce1be-106">Můžete rychle vyhledávání a filtrování hello katalogu toolocate datové zdroje a pochopení jejich zamýšlený účel, takže je jednodušší toofind hello správná data pro úlohu hello po ruce.</span><span class="sxs-lookup"><span data-stu-id="ce1be-106">You can quickly search and filter hello catalog toolocate data sources and understand their intended purpose, making it easier toofind hello right data for hello job at hand.</span></span>

<span data-ttu-id="ce1be-107">Ale co když musíte tooregularly pracovat s hello stejná data?</span><span class="sxs-lookup"><span data-stu-id="ce1be-107">But what if you need tooregularly work with hello same data?</span></span> <span data-ttu-id="ce1be-108">A co když vy a ostatní uživatelé pravidelně přispívat vaší toohello znalostní báze stejného zdroje dat v katalogu hello?</span><span class="sxs-lookup"><span data-stu-id="ce1be-108">And what if you and other users regularly contribute your knowledge toohello same data sources in hello catalog?</span></span> <span data-ttu-id="ce1be-109">V těchto situacích s toorepeatedly problém hello stejné hledání může být neefektivní.</span><span class="sxs-lookup"><span data-stu-id="ce1be-109">In these situations, having toorepeatedly issue hello same searches can be inefficient.</span></span> <span data-ttu-id="ce1be-110">Toto je, kde může pomoci uložené hledání a definovaného datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="ce1be-110">This is where saved search and pinned data assets can help.</span></span>

## <a name="saved-searches"></a><span data-ttu-id="ce1be-111">Uložená hledání</span><span class="sxs-lookup"><span data-stu-id="ce1be-111">Saved searches</span></span>
<span data-ttu-id="ce1be-112">Uložené hledání v katalogu Data Catalog je opakovaně využívat, definice vyhledávání na uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce1be-112">A saved search in Data Catalog is a reusable, per-user search definition.</span></span> <span data-ttu-id="ce1be-113">Můžete definovat hledání, včetně podmínek vyhledávání, značky a ostatní filtry a pak ho uložte.</span><span class="sxs-lookup"><span data-stu-id="ce1be-113">You can define a search, including search terms, tags, and other filters, and then save it.</span></span> <span data-ttu-id="ce1be-114">Můžete znovu spustit hello uložené hledání definice novější tooreturn žádné datových prostředků, které odpovídají jeho kritéria vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ce1be-114">You can re-run hello saved search definition later tooreturn any data assets that match its search criteria.</span></span>

### <a name="create-a-saved-search"></a><span data-ttu-id="ce1be-115">Vytvoření uloženého hledání</span><span class="sxs-lookup"><span data-stu-id="ce1be-115">Create a saved search</span></span>
<span data-ttu-id="ce1be-116">toocreate uloženého hledání hello následující:</span><span class="sxs-lookup"><span data-stu-id="ce1be-116">toocreate a saved search, do hello following:</span></span>
1. <span data-ttu-id="ce1be-117">V portálu Azure Data Catalog hello v hello **aktuální vyhledávání** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ce1be-117">In hello Azure Data Catalog portal, in hello **Current Search** window, click **Save**.</span></span> 

    ![Aktuální odkaz Uložit nastavení vyhledávání](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. <span data-ttu-id="ce1be-119">Zadejte kritéria hledání hello má tooreuse a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ce1be-119">Enter hello search criteria that you want tooreuse, and then click **Save**.</span></span>

    ![Aktuální nastavení vyhledávání uložit název vyhledávání](./media/data-catalog-how-to-save-pin/02-name.png)

3. <span data-ttu-id="ce1be-121">Když se zobrazí výzva, zadejte název pro uložené hledání hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-121">When you are prompted, enter a name for hello saved search.</span></span> <span data-ttu-id="ce1be-122">Vyberte název, který má smysl a která popisuje hello datové prostředky, které vrátí vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-122">Pick a name that is meaningful and that describes hello data assets that will be returned by hello search.</span></span>

### <a name="manage-saved-searches"></a><span data-ttu-id="ce1be-123">Spravovat uložená hledání</span><span class="sxs-lookup"><span data-stu-id="ce1be-123">Manage saved searches</span></span>
<span data-ttu-id="ce1be-124">Po uložení jeden nebo více hledání **uložená hledání** možnost se zobrazí pod hello **aktuální vyhledávání** pole.</span><span class="sxs-lookup"><span data-stu-id="ce1be-124">After you have saved one or more searches, a **Saved Searches** option is displayed beneath hello **Current Search** box.</span></span> <span data-ttu-id="ce1be-125">Po rozbalení seznamu hello se zobrazují všechna uložená hledání.</span><span class="sxs-lookup"><span data-stu-id="ce1be-125">When hello list is expanded, all saved searches are displayed.</span></span>

 ![Seznam uložených hledání](./media/data-catalog-how-to-save-pin/03-list.png)

<span data-ttu-id="ce1be-127">Proveďte jeden z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="ce1be-127">Do any of hello following:</span></span>

* <span data-ttu-id="ce1be-128">tooexecute vyhledávání, vyberte v seznamu hello uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="ce1be-128">tooexecute a search, select a saved search in hello list.</span></span>

* <span data-ttu-id="ce1be-129">tooview seznam možností správy pro uložené hledání, klikněte na tlačítko hello dolů šipku další toohello vyhledávací název.</span><span class="sxs-lookup"><span data-stu-id="ce1be-129">tooview a list of management options for a saved search, click hello down arrow next toohello search name.</span></span>

    ![Možnosti pro správu uložených hledání](./media/data-catalog-how-to-save-pin/04-managing.png)

* <span data-ttu-id="ce1be-131">Vyberte tooenter nový název pro vyhledávání uložit hello **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="ce1be-131">tooenter a new name for hello saved search, select **Rename**.</span></span> <span data-ttu-id="ce1be-132">definice vyhledávání Hello se nezmění.</span><span class="sxs-lookup"><span data-stu-id="ce1be-132">hello search definition is not changed.</span></span>

* <span data-ttu-id="ce1be-133">tooremove hello uložené hledání v seznamu vyberte **odstranit**a pak potvrďte odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-133">tooremove hello saved search from your list, select **Delete**, and then confirm hello deletion.</span></span>

* <span data-ttu-id="ce1be-134">hledání toomark hello uložit jako výchozí hledání, vyberte **uložit jako výchozí**.</span><span class="sxs-lookup"><span data-stu-id="ce1be-134">toomark hello saved search as your default search, select **Save As Default**.</span></span> <span data-ttu-id="ce1be-135">Pokud provádíte "prázdný" vyhledávání z domovské stránky hello Azure Data Catalog, se spustí vyhledávání výchozí.</span><span class="sxs-lookup"><span data-stu-id="ce1be-135">If you perform an “empty” search from hello Azure Data Catalog home page, your default search is executed.</span></span> <span data-ttu-id="ce1be-136">Kromě toho hello vyhledávání, který je označen jako první hello hello je zobrazeno výchozí vyhledávání hello **uložená hledání** seznamu.</span><span class="sxs-lookup"><span data-stu-id="ce1be-136">In addition, hello search that's marked as hello default search is displayed at hello top of hello **Saved Searches** list.</span></span>

### <a name="organizational-saved-searches"></a><span data-ttu-id="ce1be-137">Organizační uložených hledání</span><span class="sxs-lookup"><span data-stu-id="ce1be-137">Organizational saved searches</span></span>
<span data-ttu-id="ce1be-138">Všechny uživatele ve vaší organizaci můžete uložit hledání pro vlastní použití.</span><span class="sxs-lookup"><span data-stu-id="ce1be-138">All user in your organization can save searches for their own use.</span></span> <span data-ttu-id="ce1be-139">Správci katalogu dat můžete také uložit hledání pro všechny uživatele v rámci organizace hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-139">Data Catalog administrators can also save searches for all users within hello organization.</span></span> <span data-ttu-id="ce1be-140">Když správci uložte hledání, že máte na výběr **sdílenou složku v rámci společnosti hello** možnost.</span><span class="sxs-lookup"><span data-stu-id="ce1be-140">When administrators save a search, they're presented with a **Share within hello company** option.</span></span> <span data-ttu-id="ce1be-141">Výběrem této možnosti hello sdílené složky uložené hledání pro všechny uživatele v organizaci hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-141">Selecting this option shares hello saved search for all users in hello organization.</span></span>

 ![Organizační uložených hledání](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a><span data-ttu-id="ce1be-143">Definovaného datových prostředků</span><span class="sxs-lookup"><span data-stu-id="ce1be-143">Pinned data assets</span></span>
<span data-ttu-id="ce1be-144">S uložená hledání můžete uložit a opakovaně používat definice vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ce1be-144">With saved searches, you can save and reuse search definitions.</span></span> <span data-ttu-id="ce1be-145">časem jako obsah hello hello katalogu změny může změnit Hello datové prostředky, které jsou výsledky hledání hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-145">hello data assets that are returned by hello searches might change over time as hello contents of hello catalog change.</span></span> <span data-ttu-id="ce1be-146">Při připnete datových prostředků, můžete identifikovat explicitně toomake prostředky údaje specifické pro jejich snazší tooaccess bez nutnosti toouse vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ce1be-146">When you pin data assets, you can explicitly identify specific data assets toomake them easier tooaccess without needing toouse a search.</span></span>

<span data-ttu-id="ce1be-147">Připnutí datový prostředek je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="ce1be-147">Pinning a data asset is straightforward.</span></span> <span data-ttu-id="ce1be-148">seznam tooadd hello data asset tooyour připnutý, klepněte na tlačítko hello **pin** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ce1be-148">tooadd hello data asset tooyour pinned list, you simply click hello **pin** icon.</span></span> <span data-ttu-id="ce1be-149">Zobrazí se ikona Hello v hello rohu hello asset dlaždici v zobrazení tile hello a ve sloupci nejvíce vlevo hello v zobrazení seznamu hello v portálu Azure Data Catalog hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-149">hello icon is displayed in hello corner of hello asset tile in hello tile view, and in hello left-most column in hello list view in hello Azure Data Catalog portal.</span></span>

![ikonu Připnutí datový prostředek Hello](./media/data-catalog-how-to-save-pin/05-pinning.png)

<span data-ttu-id="ce1be-151">Odepnutí datový prostředek je stejně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="ce1be-151">Unpinning a data asset is equally straightforward.</span></span> <span data-ttu-id="ce1be-152">Jednoduše klikněte na tlačítko hello **Odepnout** ikonu tootoggle hello nastavení pro vybraný prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="ce1be-152">Simply click hello **unpin** icon tootoggle hello setting for hello selected asset.</span></span>

![Ikona Odepnout Hello datový – prostředek](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="hello-my-assets-section"></a><span data-ttu-id="ce1be-154">Hello části Moje prostředky</span><span class="sxs-lookup"><span data-stu-id="ce1be-154">hello My Assets section</span></span>
<span data-ttu-id="ce1be-155">Hello katalogu Data Catalog zahrnuje domovské stránky portálu **Moje prostředky** oddíl, který zobrazuje prostředky, které vás zajímají toohello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce1be-155">hello Data Catalog portal home page includes a **My Assets** section that displays assets of interest toohello current user.</span></span> <span data-ttu-id="ce1be-156">Tato část zahrnuje obě definovaného prostředky a uložená hledání.</span><span class="sxs-lookup"><span data-stu-id="ce1be-156">This section includes both pinned assets and saved searches.</span></span>

![Hello části Moje prostředky na domovskou stránku hello](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a><span data-ttu-id="ce1be-158">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ce1be-158">Summary</span></span>
<span data-ttu-id="ce1be-159">Azure Data Catalog poskytuje možnosti, které jednodušší zdroje dat hello toodiscover, které budete potřebovat, takže vám i ostatním členům organizace může trávit méně času, hledá dat a další čas pracovat s ním.</span><span class="sxs-lookup"><span data-stu-id="ce1be-159">Azure Data Catalog provides capabilities that make it easier toodiscover hello data sources you need, so you and other organization members can spend less time looking for data and more time working with it.</span></span> <span data-ttu-id="ce1be-160">Uložená hledání a připnuté data, která prostředky sestavení na tyto základní možnosti, aby uživatelé mohli snadno identifikovat zdrojů dat, které pracují s, opakovaně.</span><span class="sxs-lookup"><span data-stu-id="ce1be-160">Saved searches and pinned data assets build on these core capabilities so users can easily identify data sources that they work with repeatedly.</span></span>
