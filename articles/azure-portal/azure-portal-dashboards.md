---
title: "Vytvoření a sdílení Azure portálu řídicí panely | Microsoft Docs"
description: "Tento článek vysvětluje, jak vytvořit a upravit řídicí panely na portálu Azure."
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 5429e68723448ff5db6ef0ed8da1b927e97e6dd9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a><span data-ttu-id="bf0fb-103">Vytvářet a sdílet řídicí panely na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bf0fb-103">Create and share dashboards in the Azure portal</span></span>
<span data-ttu-id="bf0fb-104">Můžete vytvořit více řídicí panely a sdílet je s ostatními uživateli, kteří mají přístup k vašemu předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-104">You can create multiple dashboards and share them with others who have access to your Azure subscriptions.</span></span>  <span data-ttu-id="bf0fb-105">Tento článek probírá základní informace o vytváření, úpravy, publikování a správa přístupu k řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-105">This article goes through the basics of creating, editing, publishing, and managing access to dashboards.</span></span>

## <a name="create-a-dashboard"></a><span data-ttu-id="bf0fb-106">Vytvořit řídicí panel</span><span class="sxs-lookup"><span data-stu-id="bf0fb-106">Create a dashboard</span></span>
<span data-ttu-id="bf0fb-107">Chcete-li vytvořit řídicí panel, vyberte **novým řídicím panelem** tlačítko vedle názvu aktuálního řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-107">To create a dashboard, select the **New dashboard** button next to the current dashboard's name.</span></span>  

![Vytvořit řídicí panel](./media/azure-portal-dashboards/new-dashboard.png)

<span data-ttu-id="bf0fb-109">Tato akce vytvoří řídicí panel nový, prázdný, privátní a vloží je do režimu přizpůsobení, kde můžete název řídicího panelu a přidání nebo změna uspořádání dlaždic.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-109">This action creates a new, empty, private dashboard and puts you into customization mode where you can name your dashboard and add or rearrange tiles.</span></span>  <span data-ttu-id="bf0fb-110">V tomto režimu galerii sbalitelné dlaždice má levé navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-110">When in this mode, the collapsible tile gallery takes over the left navigation menu.</span></span>  <span data-ttu-id="bf0fb-111">Galerie dlaždice umožňuje najít dlaždice pro vaše prostředky Azure různými způsoby: můžete procházet podle [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups), pomocí typ prostředku, pomocí [značky](../azure-resource-manager/resource-group-using-tags.md), nebo pomocí vyhledávání podle názvu prostředku.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-111">The tile gallery lets you find tiles for your Azure resources in various ways: you can browse by [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups), by resource type, by [tag](../azure-resource-manager/resource-group-using-tags.md), or by searching for your resource by name.</span></span>  

![přizpůsobit řídicí panel](./media/azure-portal-dashboards/customize-dashboard.png)

<span data-ttu-id="bf0fb-113">Přetahování a vkládání na plochu řídicí panel, kde chcete přidáte dlaždice.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-113">Add tiles by dragging and dropping them onto the dashboard surface wherever you want.</span></span>

<span data-ttu-id="bf0fb-114">Je nová kategorie **Obecné** pro dlaždice, které nejsou přidružené k určitému prostředku.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-114">There's a new category called **General** for tiles that are not associated with a particular resource.</span></span>  <span data-ttu-id="bf0fb-115">V tomto příkladu jsme připněte si dlaždici Markdownu.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-115">In this example, we pin the Markdown tile.</span></span>  <span data-ttu-id="bf0fb-116">Tato dlaždice je možné použít k přidání vlastního obsahu na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-116">You use this tile to add custom content to your dashboard.</span></span>  <span data-ttu-id="bf0fb-117">Dlaždice podporuje prostý text, [syntaxe Markdownu](https://daringfireball.net/projects/markdown/syntax)a omezená sada HTML.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-117">The tile supports plain text, [Markdown syntax](https://daringfireball.net/projects/markdown/syntax), and a limited set of HTML.</span></span>  <span data-ttu-id="bf0fb-118">(Pro zabezpečení, že nemůžete provádět akce jako vložit `<script>` značky nebo použijte element určité stylů CSS, který může narušovat portálu.)</span><span class="sxs-lookup"><span data-stu-id="bf0fb-118">(For safety, you can't do things like inject `<script>` tags or use certain styling element of CSS that might interfere with the portal.)</span></span> 

![Přidat markdownu](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a><span data-ttu-id="bf0fb-120">Úprava řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="bf0fb-120">Edit a dashboard</span></span>
<span data-ttu-id="bf0fb-121">Po vytvoření řídicího panelu, budete moct připnout dlaždice z Galerie dlaždice nebo dlaždice reprezentace okna.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-121">After creating your dashboard, you can pin tiles from the tile gallery or the tile representation of blades.</span></span> <span data-ttu-id="bf0fb-122">Umožňuje připnout reprezentace naší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-122">Let's pin the representation of our resource group.</span></span> <span data-ttu-id="bf0fb-123">Můžete buď kódu pin při procházení položka, nebo v okně skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-123">You can either pin when browsing the item, or from the resource group blade.</span></span> <span data-ttu-id="bf0fb-124">Obou přístupů za následek Připnutí reprezentace dlaždice skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-124">Both approaches result in pinning the tile representation of the resource group.</span></span>

![Připnout na řídicí panel](./media/azure-portal-dashboards/pin-to-dashboard.png)

<span data-ttu-id="bf0fb-126">Po Připnutí položku, zobrazí se na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-126">After pinning the item, it appears on your dashboard.</span></span>

![Zobrazení řídicí panel](./media/azure-portal-dashboards/view-dashboard.png)

<span data-ttu-id="bf0fb-128">Teď, když máme dlaždici Markdownu a skupinu prostředků připnuli k řídicímu panelu, jsme změnit velikost a změna uspořádání dlaždice do vhodný rozložení.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-128">Now that we have a Markdown tile and a resource group pinned to the dashboard, we can resize and rearrange the tiles into a suitable layout.</span></span>

<span data-ttu-id="bf0fb-129">Mohou přesouváním ukazatele myši a výběrem "...", nebo kliknete pravým tlačítkem na dlaždici se zobrazí všechny kontextové příkazy pro tuto dlaždici.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-129">By hovering and selecting "…" or right-clicking on a tile you can see all the contextual commands for that tile.</span></span> <span data-ttu-id="bf0fb-130">Ve výchozím nastavení existují dvě položky:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-130">By default, there are two items:</span></span>

1. <span data-ttu-id="bf0fb-131">**Odepnout z řídicího panelu** – odebere dlaždice z řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="bf0fb-131">**Unpin from dashboard** – removes the tile from the dashboard</span></span>
2. <span data-ttu-id="bf0fb-132">**Přizpůsobení** – vstupuje do režimu přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="bf0fb-132">**Customize** – enters customize mode</span></span>

![přizpůsobení dlaždice](./media/azure-portal-dashboards/customize-tile.png)

<span data-ttu-id="bf0fb-134">Výběrem přizpůsobit, můžete změnit velikost a změnit pořadí dlaždice.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-134">By selecting customize, you can resize and reorder tiles.</span></span> <span data-ttu-id="bf0fb-135">Ke změně velikosti dlaždici, vyberte novou velikost z kontextové nabídky, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-135">To resize a tile, select the new size from the contextual menu, as shown in the following image.</span></span>

![změnit velikost dlaždice](./media/azure-portal-dashboards/resize-tile.png)

<span data-ttu-id="bf0fb-137">Nebo, pokud dlaždice podporuje jakékoli velikosti, můžete přetáhnout pravém dolním na požadovanou velikost.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-137">Or, if the tile supports any size, you can drag the bottom right-hand corner to the desired size.</span></span>

![změnit velikost dlaždice](./media/azure-portal-dashboards/resize-corner.png)

<span data-ttu-id="bf0fb-139">Po změně velikosti dlaždic, zobrazení řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-139">After resizing tiles, view the dashboard.</span></span>

![dlaždice zobrazení](./media/azure-portal-dashboards/view-tile.png)

<span data-ttu-id="bf0fb-141">Jakmile budete hotovi, přizpůsobení řídicí panel, jednoduše vyberte **provádí přizpůsobení** ukončíte režimu přizpůsobení, nebo klikněte pravým tlačítkem a vyberte **provádí přizpůsobení** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-141">Once you are finished customizing a dashboard, simply select the **Done customizing** to exit customize mode or right-click and select **Done customizing** from the context menu.</span></span>

## <a name="publish-a-dashboard-and-manage-access-control"></a><span data-ttu-id="bf0fb-142">Řídicí panel publikování a správě řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="bf0fb-142">Publish a dashboard and manage access control</span></span>
<span data-ttu-id="bf0fb-143">Když vytváříte řídicí panel, je soukromé ve výchozím nastavení, což znamená, že jste jediná osoba, která můžete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-143">When you create a dashboard, it is private by default, which means you are the only person who can see it.</span></span>  <span data-ttu-id="bf0fb-144">Chcete-li vidět další uživatelé, použijte **sdílenou složku** tlačítko, které se zobrazí spolu s jinými příkazy řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-144">To make it visible to others, use the **Share** button that appears alongside the other dashboard commands.</span></span>

![sdílení řídicího panelu.](./media/azure-portal-dashboards/share-dashboard.png)

<span data-ttu-id="bf0fb-146">Zobrazí se výzva vybrat skupinu prostředků pro řídicí panel publikování a odběru.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-146">You are asked to choose a subscription and resource group for your dashboard to be published to.</span></span> <span data-ttu-id="bf0fb-147">Řídicí panely se bezproblémově integruje do ekosystému, implementovali jsme sdílené řídicí panely jako prostředky Azure (tak nemůžete sdílet zadáním e-mailovou adresu).</span><span class="sxs-lookup"><span data-stu-id="bf0fb-147">To seamlessly integrate dashboards into the ecosystem, we've implemented shared dashboards as Azure resources (so you can't share by typing an email address).</span></span>  <span data-ttu-id="bf0fb-148">Přístup k informacím zobrazit většinou dlaždice na portálu se řídí [řízení přístupu na základě Role Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="bf0fb-148">Access to the information displayed by most of the tiles in the portal are governed by [Azure Role Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="bf0fb-149">Sdílené řídicí panely jsou z hlediska řízení k přístupu, nijak neliší od virtuálního počítače nebo účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-149">From an access control perspective, shared dashboards are no different from a virtual machine or a storage account.</span></span>  

<span data-ttu-id="bf0fb-150">Řekněme, že máte předplatné Azure a členy týmu přiřazení rolí **vlastníka**, **Přispěvatel**, nebo **čtečky** předplatného.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-150">Let's say you have an Azure subscription and members of your team have been assigned the roles of **owner**, **contributor**, or **reader** of the subscription.</span></span>  <span data-ttu-id="bf0fb-151">Uživatelé, kteří jsou vlastníci a přispěvatelé dokážou seznamu, zobrazení, vytvořit, upravit nebo odstranit řídicí panely v rámci tohoto předplatného.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-151">Users who are owners or contributors are able to list, view, create, modify, or delete dashboards within that subscription.</span></span>  <span data-ttu-id="bf0fb-152">Uživatelé, kteří jsou čtečky dokážou seznamu a zobrazení řídicích panelů, ale nelze upravit nebo odstranit je.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-152">Users who are readers are able to list and view dashboards, but cannot modify or delete them.</span></span>  <span data-ttu-id="bf0fb-153">Uživatelé s přístupem čtečky jsou možnost provádět místní úpravy sdílené řídicího panelu, ale nejsou možné publikovat tyto změny zpět na server.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-153">Users with reader access are able to make local edits to a shared dashboard, but are not able to publish those changes back to the server.</span></span>  <span data-ttu-id="bf0fb-154">Ale udělat privátní kopie řídicího panelu pro vlastní použití.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-154">However, they can make a private copy of the dashboard for their own use.</span></span>  <span data-ttu-id="bf0fb-155">Jednotlivé dlaždice na řídicím panelu jako vždy vynutit svoje vlastní pravidla pro řízení přístupu na základě prostředků, které odpovídají.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-155">As always, individual tiles on the dashboard enforce their own access control rules based on the resources they correspond to.</span></span>  

<span data-ttu-id="bf0fb-156">Pro usnadnění práce je portálu publikování prostředí příručky směrem vzor umístění řídicí panely ve skupině prostředků jste volali metodu **řídicí panely**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-156">For convenience, the portal's publishing experience guides you towards a pattern where you place dashboards in a resource group called **dashboards**.</span></span>  

![publikovat řídicí panel](./media/azure-portal-dashboards/publish-dashboard.png)

<span data-ttu-id="bf0fb-158">Můžete také publikovat na řídicí panel do určité skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-158">You can also choose to publish a dashboard to a particular resource group.</span></span>  <span data-ttu-id="bf0fb-159">Řízení přístupu pro tento řídicí panel odpovídá řízení přístupu pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-159">The access control for that dashboard matches the access control for the resource group.</span></span>  <span data-ttu-id="bf0fb-160">Uživatelé, kteří můžou spravovat prostředky v příslušné skupině prostředků také mít přístup k řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-160">Users that can manage the resources in that resource group also have access to the dashboards.</span></span>

![publikování řídicího panelu do skupiny prostředků](./media/azure-portal-dashboards/publish-to-resource-group.png)

<span data-ttu-id="bf0fb-162">Po publikování řídicího panelu **sdílení + přístup** ovládacího prvku podokno bude aktualizovat a můžete zobrazit informace o publikovaných řídicího panelu, včetně odkaz na spravovat přístup uživatelů k řídicímu panelu.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-162">After your dashboard is published, the **Sharing + access** control pane will refresh and show you information about the published dashboard, including a link to manage user access to the dashboard.</span></span>  <span data-ttu-id="bf0fb-163">Tento odkaz spustí standardní okno řízení přístupu na základě rolí používá k řízení přístupu pro jakýmikoli prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-163">This link launches the standard Role Based Access Control blade used to manage access for any Azure resource.</span></span>  <span data-ttu-id="bf0fb-164">Vždy získáte zpět na toto zobrazení tak, že vyberete **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-164">You can always get back to this view by selecting **Share**.</span></span>

![Správa řízení přístupu](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a><span data-ttu-id="bf0fb-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf0fb-166">Next steps</span></span>
* <span data-ttu-id="bf0fb-167">Ke správě prostředků, najdete v části [Azure spravovat prostředky prostřednictvím portálu](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bf0fb-167">To manage resources, see [Manage Azure resources through portal](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="bf0fb-168">Nasazení prostředků najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](../azure-resource-manager/resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bf0fb-168">To deploy resources, see [Deploy resources with Resource Manager templates and Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).</span></span>

