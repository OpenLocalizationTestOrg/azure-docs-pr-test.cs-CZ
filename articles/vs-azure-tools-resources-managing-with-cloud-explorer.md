---
title: "Správa prostředků Azure pomocí Průzkumníku cloudu | Microsoft Docs"
description: "Další informace o použití Průzkumníku cloudu k prohlížení a správě prostředků Azure v sadě Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: kraigb
ms.openlocfilehash: 6e6d8d559f0251b71bfa6d529ead82a246cb3235
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-the-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a><span data-ttu-id="a4fed-103">Spravovat prostředky přidružené k vaší účty Azure v cloudu Průzkumníka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4fed-103">Manage the resources associated with your Azure accounts in Visual Studio Cloud Explorer</span></span>
<span data-ttu-id="a4fed-104">Průzkumník cloudu umožňuje zobrazit skupiny prostředků a prostředků Azure, zkontrolujte jejich vlastnosti a provádět akce diagnostiky klíče vývojáře z Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="a4fed-104">Cloud Explorer enables you to view your Azure resources and resource groups, inspect their properties, and perform key developer diagnostics actions from within Visual Studio.</span></span> 

<span data-ttu-id="a4fed-105">Podobně jako [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), Průzkumník cloudu je založený na zásobník Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a4fed-105">Like the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer is built on the Azure Resource Manager stack.</span></span> <span data-ttu-id="a4fed-106">Proto Průzkumník cloudu rozumí prostředkům, například skupin prostředků Azure a službami Azure, jako třeba logiku aplikace a aplikace API a podporuje [řízení přístupu na základě role](active-directory/role-based-access-control-configure.md) (RBAC).</span><span class="sxs-lookup"><span data-stu-id="a4fed-106">Therefore, Cloud Explorer understands resources such as Azure resource groups and Azure services such as Logic apps and API apps, and it supports [role-based access control](active-directory/role-based-access-control-configure.md) (RBAC).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a4fed-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4fed-107">Prerequisites</span></span>
- <span data-ttu-id="a4fed-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) s **úloha Azure** vybrána, nebo starší verze sady Visual Studio s [Microsoft Azure SDK pro .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span><span class="sxs-lookup"><span data-stu-id="a4fed-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) with the **Azure workload** selected, or an earlier version of Visual Studio with the [Microsoft Azure SDK for .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span></span>
- <span data-ttu-id="a4fed-109">Účet Microsoft Azure – Pokud nemáte účet, můžete [zaregistrovat k bezplatné zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=623901) nebo [aktivovat výhody předplatitele sady Visual Studio](http://go.microsoft.com/fwlink/?LinkId=623901).</span><span class="sxs-lookup"><span data-stu-id="a4fed-109">Microsoft Azure account - If you don't have an account, you can [sign up for a free trial](http://go.microsoft.com/fwlink/?LinkId=623901) or [activate your Visual Studio subscriber benefits](http://go.microsoft.com/fwlink/?LinkId=623901).</span></span>

> [!NOTE]
> <span data-ttu-id="a4fed-110">Chcete-li zobrazit Průzkumník cloudu, vyberte **zobrazení** > **Průzkumník cloudu** v řádku nabídek.</span><span class="sxs-lookup"><span data-stu-id="a4fed-110">To view Cloud Explorer, select **View** > **Cloud Explorer** on the menu bar.</span></span>   
> 
> 

## <a name="add-an-azure-account-to-cloud-explorer"></a><span data-ttu-id="a4fed-111">Azure přidat účet Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="a4fed-111">Add an Azure account to Cloud Explorer</span></span>
<span data-ttu-id="a4fed-112">Chcete-li zobrazit prostředky přidružené k účtu Azure, je nejprve nutno přidat účet Průzkumník cloudu.</span><span class="sxs-lookup"><span data-stu-id="a4fed-112">To view the resources associated with an Azure account, you must first add the account to Cloud Explorer.</span></span> 

1. <span data-ttu-id="a4fed-113">V **Průzkumník cloudu**, vyberte **nastavení účtu Azure**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-113">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="a4fed-115">Vyberte **nový účet přidal**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-115">Select **Add new account**.</span></span> 

    ![Odkaz Přidat účet Průzkumník cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. <span data-ttu-id="a4fed-117">Přihlaste se k účtu Azure jehož prostředky, které chcete procházet.</span><span class="sxs-lookup"><span data-stu-id="a4fed-117">Log in to the Azure account whose resources you want to browse.</span></span> 

1. <span data-ttu-id="a4fed-118">Po přihlášení k účtu Azure, zobrazí se odběry přidružené k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="a4fed-118">Once logged in to an Azure account, the subscriptions associated with that account display.</span></span> <span data-ttu-id="a4fed-119">Zaškrtněte políčka pro předplatná účet chcete procházet a pak vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-119">Select the check boxes for the account subscriptions you want to browse and then select **Apply**.</span></span> 
 
    ![Průzkumník cloudu: Vyberte předplatná Azure k zobrazení](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. <span data-ttu-id="a4fed-121">Až vyberete předplatná, jejichž prostředky, které chcete procházet, tyto odběry a prostředky zobrazit v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="a4fed-121">After selecting the subscriptions whose resources you want to browse, those subscriptions and resources display in the Cloud Explorer.</span></span>

    ![Průzkumník prostředků výpis pro účet Azure v cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a><span data-ttu-id="a4fed-123">Odeberte účet Azure z Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="a4fed-123">Remove an Azure account from Cloud Explorer</span></span> 

1. <span data-ttu-id="a4fed-124">V **Průzkumník cloudu**, vyberte **nastavení účtu Azure**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-124">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="a4fed-126">U účtu, kterou chcete odebrat, vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-126">Next to the account you want to remove, select **Remove**.</span></span>

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a><span data-ttu-id="a4fed-128">Zobrazit typy prostředků nebo skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a4fed-128">View resource types or resource groups</span></span>
<span data-ttu-id="a4fed-129">Chcete-li zobrazit vašich prostředků Azure, můžete zvolit buď **typy prostředků** nebo **skupiny prostředků** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a4fed-129">To view your Azure resources, you can choose either **Resource Types** or **Resource Groups** view.</span></span>

1. <span data-ttu-id="a4fed-130">V **Průzkumník cloudu**, vyberte zobrazení rozevíracího seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a4fed-130">In **Cloud Explorer**, select the resource view dropdown.</span></span>

    ![Cloud Explorer rozevíracího seznamu vyberte zobrazení požadované prostředky](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. <span data-ttu-id="a4fed-132">V místní nabídce vyberte požadované zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a4fed-132">From the context menu, select the desired view:</span></span> 

    - <span data-ttu-id="a4fed-133">**Typy prostředků** zobrazení - běžné zobrazení použít na [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), zobrazuje vašich prostředků Azure uspořádány do kategorií podle jejich typu, jako jsou webové aplikace, účty úložiště a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a4fed-133">**Resource Types** view - The common view used on the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), shows your Azure resources categorized by their type, such as web apps, storage accounts, and virtual machines.</span></span> 
    - <span data-ttu-id="a4fed-134">**Skupiny prostředků** zobrazit - rozděluje Azure prostředky ve skupině prostředků Azure, ke kterému jsou přidružené.</span><span class="sxs-lookup"><span data-stu-id="a4fed-134">**Resource Groups** view - Categorizes Azure resources by the Azure resource group with which they're associated.</span></span> <span data-ttu-id="a4fed-135">Skupina prostředků je sada prostředky Azure, obvykle používají v konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4fed-135">A resource group is a bundle of Azure resources, typically used by a specific application.</span></span> <span data-ttu-id="a4fed-136">Další informace o skupinách prostředků Azure, najdete v části [přehled Azure Resource Manageru](./azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a4fed-136">To learn more about Azure resource groups, see [Azure Resource Manager Overview](./azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="a4fed-137">Následující obrázek znázorňuje porovnání zobrazení dvou zdrojů:</span><span class="sxs-lookup"><span data-stu-id="a4fed-137">The following image shows a comparison of the two resource views:</span></span>

    ![Porovnání zobrazení Průzkumníka prostředků cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a><span data-ttu-id="a4fed-139">Zobrazení a přejděte prostředky v Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="a4fed-139">View and navigate resources in Cloud Explorer</span></span>
<span data-ttu-id="a4fed-140">Přejděte na prostředek služby Azure a zobrazit její informace v Průzkumníku cloudu, rozbalte položky typu nebo související skupiny prostředků a pak vyberte prostředek.</span><span class="sxs-lookup"><span data-stu-id="a4fed-140">To navigate to an Azure resource and view its information in Cloud Explorer, expand the item's type or associated resource group and then select the resource.</span></span> <span data-ttu-id="a4fed-141">Když vyberete prostředek, informace se zobrazí v dvě karty - **akce** a **vlastnosti** – v dolní části Průzkumník cloudu.</span><span class="sxs-lookup"><span data-stu-id="a4fed-141">When you select a resource, information appears in the two tabs - **Actions** and **Properties** - at the bottom of Cloud Explorer.</span></span> 

- <span data-ttu-id="a4fed-142">**Akce** kartě – jsou uvedeny akce, můžete provést v Průzkumníku cloudu pro vybraný zdroj.</span><span class="sxs-lookup"><span data-stu-id="a4fed-142">**Actions** tab - Lists the actions you can take in Cloud Explorer for the selected resource.</span></span> <span data-ttu-id="a4fed-143">Tyto možnosti můžete zobrazit také kliknutím pravým tlačítkem na prostředek zobrazíte jeho kontextové nabídky.</span><span class="sxs-lookup"><span data-stu-id="a4fed-143">You can also view these options by right-clicking the resource to view its context menu.</span></span>

- <span data-ttu-id="a4fed-144">**Vlastnosti** kartě – se zobrazují vlastnosti prostředku, například jeho typ, národní prostředí a prostředků skupiny, ke kterému je přiřazeno.</span><span class="sxs-lookup"><span data-stu-id="a4fed-144">**Properties** tab - Shows the properties of the resource, such as its type, locale, and resource group with which it is associated.</span></span>

<span data-ttu-id="a4fed-145">Následující obrázek ukazuje příklad porovnání zobrazené na jednotlivých kartách pro aplikační službu:</span><span class="sxs-lookup"><span data-stu-id="a4fed-145">The following image shows an example comparison of what you see on each tab for an App Service:</span></span>

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

<span data-ttu-id="a4fed-146">Každý prostředek má akce **otevřít na portálu**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-146">Every resource has the action **Open in portal**.</span></span> <span data-ttu-id="a4fed-147">Pokud zvolíte tuto akci, Průzkumník cloudu zobrazí vybrané prostředky v [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a4fed-147">When you choose this action, Cloud Explorer displays the selected resource in the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="a4fed-148">**Otevřít na portálu** funkce je užitečný pro navigaci na hluboko vložené prostředky.</span><span class="sxs-lookup"><span data-stu-id="a4fed-148">The **Open in portal** feature is handy for navigating to deeply nested resources.</span></span>

<span data-ttu-id="a4fed-149">Další akce a hodnot vlastností mohou zobrazit i podle prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a4fed-149">Additional actions and property values may also appear based on the Azure resource.</span></span> <span data-ttu-id="a4fed-150">Webové aplikace a aplikace logiky také mít například akce **otevřít v prohlížeči** a **připojit ladicí program** kromě **otevřít na portálu**.</span><span class="sxs-lookup"><span data-stu-id="a4fed-150">For example, web apps and logic apps also have the actions **Open in browser** and **Attach debugger** in addition to **Open in portal**.</span></span> <span data-ttu-id="a4fed-151">Akce otevřete editory se zobrazí, když zvolíte účet úložiště objektů blob, fronty nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="a4fed-151">Actions to open editors appear when you choose a storage account blob, queue, or table.</span></span> <span data-ttu-id="a4fed-152">Azure aplikací mít **URL** a **stav** vlastností, zatímco prostředky úložiště mají vlastnosti klíče a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="a4fed-152">Azure apps have **URL** and **Status** properties, while storage resources have key and connection string properties.</span></span>

## <a name="find-resources-in-cloud-explorer"></a><span data-ttu-id="a4fed-153">Vyhledávání prostředků v Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="a4fed-153">Find resources in Cloud Explorer</span></span>
<span data-ttu-id="a4fed-154">Chcete-li vyhledat prostředky s konkrétním názvem v rámci vašich předplatných účet Azure, zadejte název v **vyhledávání** pole v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="a4fed-154">To locate resources with a specific name in your Azure account subscriptions, enter the name in the **Search** box in Cloud Explorer.</span></span>

![Vyhledávání prostředků v Průzkumníku cloudu](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

<span data-ttu-id="a4fed-156">Při zadávání znaků **vyhledávání** pole jenom prostředky, které splňují tyto znaky se zobrazí ve stromové struktuře prostředků.</span><span class="sxs-lookup"><span data-stu-id="a4fed-156">As you enter characters in the **Search** box, only resources that match those characters appear in the resource tree.</span></span>
