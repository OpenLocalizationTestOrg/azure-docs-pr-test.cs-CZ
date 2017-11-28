---
title: "aaaCreate a sdílet řídicí panely Azure portálu | Microsoft Docs"
description: "Tento článek vysvětluje, jak hello toocreate a upravit řídicí panely v portálu Azure."
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
ms.openlocfilehash: 0facd10fe3284d7ad9a9e29791e5af5b5b95c97f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-dashboards-in-hello-azure-portal"></a><span data-ttu-id="012d6-103">Vytvoření a sdílení řídicích panelů v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="012d6-103">Create and share dashboards in hello Azure portal</span></span>
<span data-ttu-id="012d6-104">Můžete vytvořit více řídicí panely a sdílet je s ostatními uživateli, kteří mají přístup tooyour Azure odběry.</span><span class="sxs-lookup"><span data-stu-id="012d6-104">You can create multiple dashboards and share them with others who have access tooyour Azure subscriptions.</span></span>  <span data-ttu-id="012d6-105">Tento článek probírá základní hello informace o vytváření, úpravy, publikování a správa toodashboards přístup.</span><span class="sxs-lookup"><span data-stu-id="012d6-105">This article goes through hello basics of creating, editing, publishing, and managing access toodashboards.</span></span>

## <a name="create-a-dashboard"></a><span data-ttu-id="012d6-106">Vytvořit řídicí panel</span><span class="sxs-lookup"><span data-stu-id="012d6-106">Create a dashboard</span></span>
<span data-ttu-id="012d6-107">toocreate řídicí panel, vyberte hello **novým řídicím panelem** tlačítko Další toohello aktuální na název řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="012d6-107">toocreate a dashboard, select hello **New dashboard** button next toohello current dashboard's name.</span></span>  

![Vytvořit řídicí panel](./media/azure-portal-dashboards/new-dashboard.png)

<span data-ttu-id="012d6-109">Tato akce vytvoří řídicí panel nový, prázdný, privátní a vloží je do režimu přizpůsobení, kde můžete název řídicího panelu a přidání nebo změna uspořádání dlaždic.</span><span class="sxs-lookup"><span data-stu-id="012d6-109">This action creates a new, empty, private dashboard and puts you into customization mode where you can name your dashboard and add or rearrange tiles.</span></span>  <span data-ttu-id="012d6-110">V tomto režimu hello sbalitelné dlaždici Galerie trvá přes hello levé navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="012d6-110">When in this mode, hello collapsible tile gallery takes over hello left navigation menu.</span></span>  <span data-ttu-id="012d6-111">Hello dlaždice galerie umožňuje najít dlaždice pro vaše prostředky Azure různými způsoby: můžete procházet podle [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups), pomocí typ prostředku, pomocí [značky](../azure-resource-manager/resource-group-using-tags.md), nebo pomocí vyhledávání podle názvu prostředku.</span><span class="sxs-lookup"><span data-stu-id="012d6-111">hello tile gallery lets you find tiles for your Azure resources in various ways: you can browse by [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups), by resource type, by [tag](../azure-resource-manager/resource-group-using-tags.md), or by searching for your resource by name.</span></span>  

![přizpůsobit řídicí panel](./media/azure-portal-dashboards/customize-dashboard.png)

<span data-ttu-id="012d6-113">Přidáte dlaždice přetahováním myší na plochu řídicí panel hello požadovaná místa.</span><span class="sxs-lookup"><span data-stu-id="012d6-113">Add tiles by dragging and dropping them onto hello dashboard surface wherever you want.</span></span>

<span data-ttu-id="012d6-114">Je nová kategorie **Obecné** pro dlaždice, které nejsou přidružené k určitému prostředku.</span><span class="sxs-lookup"><span data-stu-id="012d6-114">There's a new category called **General** for tiles that are not associated with a particular resource.</span></span>  <span data-ttu-id="012d6-115">V tomto příkladu jsme připnout dlaždice Markdownu hello.</span><span class="sxs-lookup"><span data-stu-id="012d6-115">In this example, we pin hello Markdown tile.</span></span>  <span data-ttu-id="012d6-116">Můžete použít tento řídicí panel dlaždice tooadd vlastní tooyour obsahu.</span><span class="sxs-lookup"><span data-stu-id="012d6-116">You use this tile tooadd custom content tooyour dashboard.</span></span>  <span data-ttu-id="012d6-117">Hello dlaždice podporuje prostý text, [syntaxe Markdownu](https://daringfireball.net/projects/markdown/syntax)a omezená sada HTML.</span><span class="sxs-lookup"><span data-stu-id="012d6-117">hello tile supports plain text, [Markdown syntax](https://daringfireball.net/projects/markdown/syntax), and a limited set of HTML.</span></span>  <span data-ttu-id="012d6-118">(Pro zabezpečení, že nemůžete provádět akce jako vložit `<script>` značky nebo použijte element určité stylů CSS, který může narušovat hello portálu.)</span><span class="sxs-lookup"><span data-stu-id="012d6-118">(For safety, you can't do things like inject `<script>` tags or use certain styling element of CSS that might interfere with hello portal.)</span></span> 

![Přidat markdownu](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a><span data-ttu-id="012d6-120">Úprava řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="012d6-120">Edit a dashboard</span></span>
<span data-ttu-id="012d6-121">Po vytvoření řídicího panelu, budete moct připnout dlaždice z Galerie dlaždice hello nebo hello dlaždice reprezentace okna.</span><span class="sxs-lookup"><span data-stu-id="012d6-121">After creating your dashboard, you can pin tiles from hello tile gallery or hello tile representation of blades.</span></span> <span data-ttu-id="012d6-122">Umožňuje připnout hello reprezentace naší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="012d6-122">Let's pin hello representation of our resource group.</span></span> <span data-ttu-id="012d6-123">Můžete buď připnout při procházení hello položek, nebo v okně skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="012d6-123">You can either pin when browsing hello item, or from hello resource group blade.</span></span> <span data-ttu-id="012d6-124">Obou přístupů za následek Připnutí hello dlaždice reprezentace hello skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="012d6-124">Both approaches result in pinning hello tile representation of hello resource group.</span></span>

![Toodashboard PIN kód](./media/azure-portal-dashboards/pin-to-dashboard.png)

<span data-ttu-id="012d6-126">Po Připnutí hello položku, zobrazí se na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="012d6-126">After pinning hello item, it appears on your dashboard.</span></span>

![Zobrazení řídicí panel](./media/azure-portal-dashboards/view-dashboard.png)

<span data-ttu-id="012d6-128">Teď, když máme dlaždici Markdownu a řídicí panel definovaného toohello skupiny prostředků, jsme změnit velikost a změna uspořádání hello dlaždice do vhodný rozložení.</span><span class="sxs-lookup"><span data-stu-id="012d6-128">Now that we have a Markdown tile and a resource group pinned toohello dashboard, we can resize and rearrange hello tiles into a suitable layout.</span></span>

<span data-ttu-id="012d6-129">Mohou přesouváním ukazatele myši a výběrem "...", nebo kliknete pravým tlačítkem na dlaždici se zobrazí všechny příkazy kontextové hello pro tuto dlaždici.</span><span class="sxs-lookup"><span data-stu-id="012d6-129">By hovering and selecting "…" or right-clicking on a tile you can see all hello contextual commands for that tile.</span></span> <span data-ttu-id="012d6-130">Ve výchozím nastavení existují dvě položky:</span><span class="sxs-lookup"><span data-stu-id="012d6-130">By default, there are two items:</span></span>

1. <span data-ttu-id="012d6-131">**Odepnout z řídicího panelu** – odebere hello dlaždice z řídicího panelu hello</span><span class="sxs-lookup"><span data-stu-id="012d6-131">**Unpin from dashboard** – removes hello tile from hello dashboard</span></span>
2. <span data-ttu-id="012d6-132">**Přizpůsobení** – vstupuje do režimu přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="012d6-132">**Customize** – enters customize mode</span></span>

![přizpůsobení dlaždice](./media/azure-portal-dashboards/customize-tile.png)

<span data-ttu-id="012d6-134">Výběrem přizpůsobit, můžete změnit velikost a změnit pořadí dlaždice.</span><span class="sxs-lookup"><span data-stu-id="012d6-134">By selecting customize, you can resize and reorder tiles.</span></span> <span data-ttu-id="012d6-135">tooresize dlaždice, vyberte hello novou velikost z hello kontextové nabídky, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="012d6-135">tooresize a tile, select hello new size from hello contextual menu, as shown in hello following image.</span></span>

![změnit velikost dlaždice](./media/azure-portal-dashboards/resize-tile.png)

<span data-ttu-id="012d6-137">Nebo, pokud hello dlaždice podporuje jakékoli velikosti, můžete přetáhnout hello dolní pravém rohu toohello potřeby velikost.</span><span class="sxs-lookup"><span data-stu-id="012d6-137">Or, if hello tile supports any size, you can drag hello bottom right-hand corner toohello desired size.</span></span>

![změnit velikost dlaždice](./media/azure-portal-dashboards/resize-corner.png)

<span data-ttu-id="012d6-139">Po změně velikosti dlaždic, zobrazení řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="012d6-139">After resizing tiles, view hello dashboard.</span></span>

![dlaždice zobrazení](./media/azure-portal-dashboards/view-tile.png)

<span data-ttu-id="012d6-141">Jakmile budete hotovi, přizpůsobení řídicí panel, jednoduše vyberte hello **provádí přizpůsobení** tooexit režimu přizpůsobení, nebo klikněte pravým tlačítkem a vyberte **provádí přizpůsobení** hello místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="012d6-141">Once you are finished customizing a dashboard, simply select hello **Done customizing** tooexit customize mode or right-click and select **Done customizing** from hello context menu.</span></span>

## <a name="publish-a-dashboard-and-manage-access-control"></a><span data-ttu-id="012d6-142">Řídicí panel publikování a správě řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="012d6-142">Publish a dashboard and manage access control</span></span>
<span data-ttu-id="012d6-143">Když vytváříte řídicí panel, je soukromé ve výchozím nastavení, což znamená, že jsou hello jediná osoba, která můžete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="012d6-143">When you create a dashboard, it is private by default, which means you are hello only person who can see it.</span></span>  <span data-ttu-id="012d6-144">toomake ho viditelné tooothers použít hello **sdílenou složku** hello tlačítko, které se zobrazí spolu s jinými příkazy řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="012d6-144">toomake it visible tooothers, use hello **Share** button that appears alongside hello other dashboard commands.</span></span>

![sdílení řídicího panelu.](./media/azure-portal-dashboards/share-dashboard.png)

<span data-ttu-id="012d6-146">Zobrazí se výzva toochoose skupinu předplatného a prostředků pro váš řídicí panel toobe publikovány na.</span><span class="sxs-lookup"><span data-stu-id="012d6-146">You are asked toochoose a subscription and resource group for your dashboard toobe published to.</span></span> <span data-ttu-id="012d6-147">tooseamlessly integrovat do hello ekosystém řídicí panely, implementovali jsme sdílené řídicí panely jako prostředky Azure (tak nemůžete sdílet zadáním e-mailovou adresu).</span><span class="sxs-lookup"><span data-stu-id="012d6-147">tooseamlessly integrate dashboards into hello ecosystem, we've implemented shared dashboards as Azure resources (so you can't share by typing an email address).</span></span>  <span data-ttu-id="012d6-148">Se řídí přístup k informacím toohello zobrazit většinou hello dlaždice portálu hello [řízení přístupu na základě Role Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="012d6-148">Access toohello information displayed by most of hello tiles in hello portal are governed by [Azure Role Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="012d6-149">Sdílené řídicí panely jsou z hlediska řízení k přístupu, nijak neliší od virtuálního počítače nebo účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="012d6-149">From an access control perspective, shared dashboards are no different from a virtual machine or a storage account.</span></span>  

<span data-ttu-id="012d6-150">Řekněme, že máte předplatné Azure a členy týmu přiřazení rolí hello **vlastníka**, **Přispěvatel**, nebo **čtečky** hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="012d6-150">Let's say you have an Azure subscription and members of your team have been assigned hello roles of **owner**, **contributor**, or **reader** of hello subscription.</span></span>  <span data-ttu-id="012d6-151">Uživatelé, kteří jsou vlastníci a přispěvatelé jsou možné toolist, zobrazení, vytvořit, upravit nebo odstranit řídicí panely v rámci tohoto předplatného.</span><span class="sxs-lookup"><span data-stu-id="012d6-151">Users who are owners or contributors are able toolist, view, create, modify, or delete dashboards within that subscription.</span></span>  <span data-ttu-id="012d6-152">Uživatelé, kteří jsou čtečky jsou možné toolist a zobrazení řídicích panelů, ale nelze upravit nebo odstranit je.</span><span class="sxs-lookup"><span data-stu-id="012d6-152">Users who are readers are able toolist and view dashboards, but cannot modify or delete them.</span></span>  <span data-ttu-id="012d6-153">Uživatelé s přístupem čtečky jsou sdílené řídicího panelu může toomake místní úpravy tooa, ale nebylo možné toopublish tyto změny zpět toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="012d6-153">Users with reader access are able toomake local edits tooa shared dashboard, but are not able toopublish those changes back toohello server.</span></span>  <span data-ttu-id="012d6-154">Ale udělat privátní kopie hello řídicího panelu pro vlastní použití.</span><span class="sxs-lookup"><span data-stu-id="012d6-154">However, they can make a private copy of hello dashboard for their own use.</span></span>  <span data-ttu-id="012d6-155">Jednotlivé dlaždice na řídicím panelu hello jako vždy vynutit svoje vlastní pravidla pro řízení přístupu na základě hello prostředků, které odpovídají.</span><span class="sxs-lookup"><span data-stu-id="012d6-155">As always, individual tiles on hello dashboard enforce their own access control rules based on hello resources they correspond to.</span></span>  

<span data-ttu-id="012d6-156">Pro usnadnění práce je portál hello publikování prostředí příručky směrem vzor umístění řídicí panely ve skupině prostředků jste volali metodu **řídicí panely**.</span><span class="sxs-lookup"><span data-stu-id="012d6-156">For convenience, hello portal's publishing experience guides you towards a pattern where you place dashboards in a resource group called **dashboards**.</span></span>  

![publikovat řídicí panel](./media/azure-portal-dashboards/publish-dashboard.png)

<span data-ttu-id="012d6-158">Můžete také toopublish řídicí panel tooa určité skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="012d6-158">You can also choose toopublish a dashboard tooa particular resource group.</span></span>  <span data-ttu-id="012d6-159">Hello řízení přístupu pro tento řídicí panel odpovídá hello řízení přístupu pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="012d6-159">hello access control for that dashboard matches hello access control for hello resource group.</span></span>  <span data-ttu-id="012d6-160">Uživatelé, kteří můžou spravovat hello prostředky v příslušné skupině prostředků mít také přístup toohello řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="012d6-160">Users that can manage hello resources in that resource group also have access toohello dashboards.</span></span>

![publikování skupiny tooresource řídicí panel](./media/azure-portal-dashboards/publish-to-resource-group.png)

<span data-ttu-id="012d6-162">Po publikování řídicího panelu hello **sdílení + přístup** ovládacího prvku podokno bude aktualizovat a můžete zobrazit informace o hello publikované řídicí panel, včetně odkaz přístup uživatelského toomanage toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="012d6-162">After your dashboard is published, hello **Sharing + access** control pane will refresh and show you information about hello published dashboard, including a link toomanage user access toohello dashboard.</span></span>  <span data-ttu-id="012d6-163">Tento odkaz spustí hello standardní řízení přístupu na základě Role okno používá toomanage přístup pro jakýmikoli prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="012d6-163">This link launches hello standard Role Based Access Control blade used toomanage access for any Azure resource.</span></span>  <span data-ttu-id="012d6-164">Vždy získáte zpět toothis zobrazení tak, že vyberete **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="012d6-164">You can always get back toothis view by selecting **Share**.</span></span>

![Správa řízení přístupu](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a><span data-ttu-id="012d6-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="012d6-166">Next steps</span></span>
* <span data-ttu-id="012d6-167">toomanage prostředky, najdete v části [Azure spravovat prostředky prostřednictvím portálu](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="012d6-167">toomanage resources, see [Manage Azure resources through portal](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="012d6-168">toodeploy prostředky, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](../azure-resource-manager/resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="012d6-168">toodeploy resources, see [Deploy resources with Resource Manager templates and Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).</span></span>

