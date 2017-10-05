---
title: "Uložte hledání a připnout datových prostředků v Azure Data Catalog | Microsoft Docs"
description: "Postupy: článek zvýraznění možnosti v Azure Data Catalog pro ukládání zdroje dat a datových prostředků pro pozdější použití."
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
ms.openlocfilehash: 8c319d0dcbe8b95af11b8be2368a9348b260446c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a><span data-ttu-id="00558-103">Uložte hledání a kód pin datových prostředků v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="00558-103">Save searches and pin data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="00558-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="00558-104">Introduction</span></span>
<span data-ttu-id="00558-105">Azure Data Catalog poskytuje možnosti pro zjišťování zdroje.</span><span class="sxs-lookup"><span data-stu-id="00558-105">Azure Data Catalog provides capabilities for data source discovery.</span></span> <span data-ttu-id="00558-106">Můžete rychle vyhledat a filtrování katalogu pro vyhledání zdroje dat a pochopení jejich zamýšlený účel, snadněji najít správná data pro úlohu po ruce.</span><span class="sxs-lookup"><span data-stu-id="00558-106">You can quickly search and filter the catalog to locate data sources and understand their intended purpose, making it easier to find the right data for the job at hand.</span></span>

<span data-ttu-id="00558-107">Ale co když je nutné pravidelně pracovat se stejnými daty?</span><span class="sxs-lookup"><span data-stu-id="00558-107">But what if you need to regularly work with the same data?</span></span> <span data-ttu-id="00558-108">A co když vy a ostatní uživatelé pravidelně přispívat vašich znalostí do stejného zdroje dat v katalogu?</span><span class="sxs-lookup"><span data-stu-id="00558-108">And what if you and other users regularly contribute your knowledge to the same data sources in the catalog?</span></span> <span data-ttu-id="00558-109">V těchto situacích může být bylo nutné opakovaně vystavovat stejné hledání neefektivní.</span><span class="sxs-lookup"><span data-stu-id="00558-109">In these situations, having to repeatedly issue the same searches can be inefficient.</span></span> <span data-ttu-id="00558-110">Toto je, kde může pomoci uložené hledání a definovaného datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="00558-110">This is where saved search and pinned data assets can help.</span></span>

## <a name="saved-searches"></a><span data-ttu-id="00558-111">Uložená hledání</span><span class="sxs-lookup"><span data-stu-id="00558-111">Saved searches</span></span>
<span data-ttu-id="00558-112">Uložené hledání v katalogu Data Catalog je opakovaně využívat, definice vyhledávání na uživatele.</span><span class="sxs-lookup"><span data-stu-id="00558-112">A saved search in Data Catalog is a reusable, per-user search definition.</span></span> <span data-ttu-id="00558-113">Můžete definovat hledání, včetně podmínek vyhledávání, značky a ostatní filtry a pak ho uložte.</span><span class="sxs-lookup"><span data-stu-id="00558-113">You can define a search, including search terms, tags, and other filters, and then save it.</span></span> <span data-ttu-id="00558-114">Uložené hledání definici později vrátit všechny datových prostředků, které odpovídají jeho kritéria vyhledávání můžete znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="00558-114">You can re-run the saved search definition later to return any data assets that match its search criteria.</span></span>

### <a name="create-a-saved-search"></a><span data-ttu-id="00558-115">Vytvoření uloženého hledání</span><span class="sxs-lookup"><span data-stu-id="00558-115">Create a saved search</span></span>
<span data-ttu-id="00558-116">K vytvoření uloženého hledání, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="00558-116">To create a saved search, do the following:</span></span>
1. <span data-ttu-id="00558-117">Na portálu Azure Data Catalog v **aktuální vyhledávání** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="00558-117">In the Azure Data Catalog portal, in the **Current Search** window, click **Save**.</span></span> 

    ![Aktuální odkaz Uložit nastavení vyhledávání](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. <span data-ttu-id="00558-119">Zadejte kritéria hledání, které chcete znovu použít a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="00558-119">Enter the search criteria that you want to reuse, and then click **Save**.</span></span>

    ![Aktuální nastavení vyhledávání uložit název vyhledávání](./media/data-catalog-how-to-save-pin/02-name.png)

3. <span data-ttu-id="00558-121">Když se zobrazí výzva, zadejte název pro uložené hledání.</span><span class="sxs-lookup"><span data-stu-id="00558-121">When you are prompted, enter a name for the saved search.</span></span> <span data-ttu-id="00558-122">Vyberte název, který má smysl a která popisuje datové prostředky, které vrátí vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="00558-122">Pick a name that is meaningful and that describes the data assets that will be returned by the search.</span></span>

### <a name="manage-saved-searches"></a><span data-ttu-id="00558-123">Spravovat uložená hledání</span><span class="sxs-lookup"><span data-stu-id="00558-123">Manage saved searches</span></span>
<span data-ttu-id="00558-124">Po uložení jeden nebo více hledání **uložená hledání** možnost se zobrazí pod **aktuální vyhledávání** pole.</span><span class="sxs-lookup"><span data-stu-id="00558-124">After you have saved one or more searches, a **Saved Searches** option is displayed beneath the **Current Search** box.</span></span> <span data-ttu-id="00558-125">Po rozbalení seznamu se zobrazí všechna uložená hledání.</span><span class="sxs-lookup"><span data-stu-id="00558-125">When the list is expanded, all saved searches are displayed.</span></span>

 ![Seznam uložených hledání](./media/data-catalog-how-to-save-pin/03-list.png)

<span data-ttu-id="00558-127">Proveďte jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="00558-127">Do any of the following:</span></span>

* <span data-ttu-id="00558-128">Provést hledání, vyberte v seznamu uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="00558-128">To execute a search, select a saved search in the list.</span></span>

* <span data-ttu-id="00558-129">Chcete-li zobrazit seznam možností správy pro uložené hledání, klikněte na šipku dolů vedle názvu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="00558-129">To view a list of management options for a saved search, click the down arrow next to the search name.</span></span>

    ![Možnosti pro správu uložených hledání](./media/data-catalog-how-to-save-pin/04-managing.png)

* <span data-ttu-id="00558-131">Pokud chcete zadat nový název pro uložené hledání, vyberte **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="00558-131">To enter a new name for the saved search, select **Rename**.</span></span> <span data-ttu-id="00558-132">Definice vyhledávání se nezmění.</span><span class="sxs-lookup"><span data-stu-id="00558-132">The search definition is not changed.</span></span>

* <span data-ttu-id="00558-133">Chcete-li odebrat uloženého hledání ze seznamu, vyberte **odstranit**a poté potvrďte odstranění.</span><span class="sxs-lookup"><span data-stu-id="00558-133">To remove the saved search from your list, select **Delete**, and then confirm the deletion.</span></span>

* <span data-ttu-id="00558-134">Chcete-li označit uloženého hledání jako výchozí hledání, vyberte **uložit jako výchozí**.</span><span class="sxs-lookup"><span data-stu-id="00558-134">To mark the saved search as your default search, select **Save As Default**.</span></span> <span data-ttu-id="00558-135">Pokud provádíte "prázdný" vyhledávání z domovské stránky Azure Data Catalog, se spustí vyhledávání výchozí.</span><span class="sxs-lookup"><span data-stu-id="00558-135">If you perform an “empty” search from the Azure Data Catalog home page, your default search is executed.</span></span> <span data-ttu-id="00558-136">Kromě toho vyhledávání, který je označený jako výchozí nastavení hledání se zobrazí v horní části **uložená hledání** seznamu.</span><span class="sxs-lookup"><span data-stu-id="00558-136">In addition, the search that's marked as the default search is displayed at the top of the **Saved Searches** list.</span></span>

### <a name="organizational-saved-searches"></a><span data-ttu-id="00558-137">Organizační uložených hledání</span><span class="sxs-lookup"><span data-stu-id="00558-137">Organizational saved searches</span></span>
<span data-ttu-id="00558-138">Všechny uživatele ve vaší organizaci můžete uložit hledání pro vlastní použití.</span><span class="sxs-lookup"><span data-stu-id="00558-138">All user in your organization can save searches for their own use.</span></span> <span data-ttu-id="00558-139">Správci katalogu dat můžete také uložit hledání pro všechny uživatele v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="00558-139">Data Catalog administrators can also save searches for all users within the organization.</span></span> <span data-ttu-id="00558-140">Když správci uložte hledání, že máte na výběr **sdílenou složku v rámci společnosti** možnost.</span><span class="sxs-lookup"><span data-stu-id="00558-140">When administrators save a search, they're presented with a **Share within the company** option.</span></span> <span data-ttu-id="00558-141">Výběrem této možnosti sdílí uloženého hledání pro všechny uživatele v organizaci.</span><span class="sxs-lookup"><span data-stu-id="00558-141">Selecting this option shares the saved search for all users in the organization.</span></span>

 ![Organizační uložených hledání](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a><span data-ttu-id="00558-143">Definovaného datových prostředků</span><span class="sxs-lookup"><span data-stu-id="00558-143">Pinned data assets</span></span>
<span data-ttu-id="00558-144">S uložená hledání můžete uložit a opakovaně používat definice vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="00558-144">With saved searches, you can save and reuse search definitions.</span></span> <span data-ttu-id="00558-145">Časem jako obsah katalogu změny může změnit datové prostředky, které se vrátí pomocí vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="00558-145">The data assets that are returned by the searches might change over time as the contents of the catalog change.</span></span> <span data-ttu-id="00558-146">Pokud připnete datových prostředků, můžete explicitně identifikovat konkrétní datové prostředky se mají usnadňuje jejich přístup bez nutnosti použít vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="00558-146">When you pin data assets, you can explicitly identify specific data assets to make them easier to access without needing to use a search.</span></span>

<span data-ttu-id="00558-147">Připnutí datový prostředek je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="00558-147">Pinning a data asset is straightforward.</span></span> <span data-ttu-id="00558-148">Chcete-li přidat datový prostředek do definovaného seznamu, klepněte na tlačítko **pin** ikonu.</span><span class="sxs-lookup"><span data-stu-id="00558-148">To add the data asset to your pinned list, you simply click the **pin** icon.</span></span> <span data-ttu-id="00558-149">Na ikonu se zobrazí v horním rohu na dlaždici asset v dlaždici zobrazení a ve sloupci nejvíce vlevo v zobrazení seznamu na portálu Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="00558-149">The icon is displayed in the corner of the asset tile in the tile view, and in the left-most column in the list view in the Azure Data Catalog portal.</span></span>

![Na ikonu Připnutí datový prostředek](./media/data-catalog-how-to-save-pin/05-pinning.png)

<span data-ttu-id="00558-151">Odepnutí datový prostředek je stejně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="00558-151">Unpinning a data asset is equally straightforward.</span></span> <span data-ttu-id="00558-152">Jednoduše klikněte **Odepnout** ikonu k přepnutí nastavení pro vybraný prostředek.</span><span class="sxs-lookup"><span data-stu-id="00558-152">Simply click the **unpin** icon to toggle the setting for the selected asset.</span></span>

![Data-asset Odepnout ikonu](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a><span data-ttu-id="00558-154">V části Moje prostředky</span><span class="sxs-lookup"><span data-stu-id="00558-154">The My Assets section</span></span>
<span data-ttu-id="00558-155">Zahrnuje domovské stránky portálu katalogu Data Catalog **Moje prostředky** oddíl, který zobrazuje prostředky, které vás zajímají s aktuálním uživatelem.</span><span class="sxs-lookup"><span data-stu-id="00558-155">The Data Catalog portal home page includes a **My Assets** section that displays assets of interest to the current user.</span></span> <span data-ttu-id="00558-156">Tato část zahrnuje obě definovaného prostředky a uložená hledání.</span><span class="sxs-lookup"><span data-stu-id="00558-156">This section includes both pinned assets and saved searches.</span></span>

![V části Moje prostředky na domovské stránce](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a><span data-ttu-id="00558-158">Souhrn</span><span class="sxs-lookup"><span data-stu-id="00558-158">Summary</span></span>
<span data-ttu-id="00558-159">Azure Data Catalog poskytuje možnosti, které usnadňují zjistit zdroje dat, které budete potřebovat, takže vám i ostatním členům organizace může trávit méně času, hledá dat a další čas pracovat s ním.</span><span class="sxs-lookup"><span data-stu-id="00558-159">Azure Data Catalog provides capabilities that make it easier to discover the data sources you need, so you and other organization members can spend less time looking for data and more time working with it.</span></span> <span data-ttu-id="00558-160">Uložená hledání a připnuté data, která prostředky sestavení na tyto základní možnosti, aby uživatelé mohli snadno identifikovat zdrojů dat, které pracují s, opakovaně.</span><span class="sxs-lookup"><span data-stu-id="00558-160">Saved searches and pinned data assets build on these core capabilities so users can easily identify data sources that they work with repeatedly.</span></span>
