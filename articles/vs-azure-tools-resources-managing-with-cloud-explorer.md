---
title: "aaaManaging Azure prostředků pomocí Průzkumníka cloudu | Microsoft Docs"
description: "Zjistěte, jak toouse Průzkumník cloudu toobrowse a spravovat prostředky Azure v sadě Visual Studio."
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
ms.openlocfilehash: 8a81660074d5d04be063df9e25076b7a97586514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a><span data-ttu-id="3a22a-103">Spravovat hello prostředky přidružené k vaší účty Azure v cloudu Průzkumníka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3a22a-103">Manage hello resources associated with your Azure accounts in Visual Studio Cloud Explorer</span></span>
<span data-ttu-id="3a22a-104">Průzkumník cloudu umožňuje vám tooview vašich prostředků Azure a skupiny prostředků, kontrolovat jejich vlastnosti a provádět akce diagnostiky klíče vývojáře z Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="3a22a-104">Cloud Explorer enables you tooview your Azure resources and resource groups, inspect their properties, and perform key developer diagnostics actions from within Visual Studio.</span></span> 

<span data-ttu-id="3a22a-105">Jako hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), Průzkumník cloudu je založený na Azure Resource Manager zásobníku hello.</span><span class="sxs-lookup"><span data-stu-id="3a22a-105">Like hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer is built on hello Azure Resource Manager stack.</span></span> <span data-ttu-id="3a22a-106">Proto Průzkumník cloudu rozumí prostředkům, například skupin prostředků Azure a službami Azure, jako třeba logiku aplikace a aplikace API a podporuje [řízení přístupu na základě role](active-directory/role-based-access-control-configure.md) (RBAC).</span><span class="sxs-lookup"><span data-stu-id="3a22a-106">Therefore, Cloud Explorer understands resources such as Azure resource groups and Azure services such as Logic apps and API apps, and it supports [role-based access control](active-directory/role-based-access-control-configure.md) (RBAC).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3a22a-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3a22a-107">Prerequisites</span></span>
- <span data-ttu-id="3a22a-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) s hello **úloha Azure** vybrána, nebo starší verze sady Visual Studio s hello [Microsoft Azure SDK pro .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span><span class="sxs-lookup"><span data-stu-id="3a22a-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello **Azure workload** selected, or an earlier version of Visual Studio with hello [Microsoft Azure SDK for .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span></span>
- <span data-ttu-id="3a22a-109">Účet Microsoft Azure – Pokud nemáte účet, můžete [zaregistrovat k bezplatné zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=623901) nebo [aktivovat výhody předplatitele sady Visual Studio](http://go.microsoft.com/fwlink/?LinkId=623901).</span><span class="sxs-lookup"><span data-stu-id="3a22a-109">Microsoft Azure account - If you don't have an account, you can [sign up for a free trial](http://go.microsoft.com/fwlink/?LinkId=623901) or [activate your Visual Studio subscriber benefits](http://go.microsoft.com/fwlink/?LinkId=623901).</span></span>

> [!NOTE]
> <span data-ttu-id="3a22a-110">Vyberte tooview Průzkumník cloudu **zobrazení** > **Průzkumník cloudu** v řádku nabídek hello.</span><span class="sxs-lookup"><span data-stu-id="3a22a-110">tooview Cloud Explorer, select **View** > **Cloud Explorer** on hello menu bar.</span></span>   
> 
> 

## <a name="add-an-azure-account-toocloud-explorer"></a><span data-ttu-id="3a22a-111">Přidat účtu Azure tooCloud Explorer</span><span class="sxs-lookup"><span data-stu-id="3a22a-111">Add an Azure account tooCloud Explorer</span></span>
<span data-ttu-id="3a22a-112">tooview hello prostředky přidružené k účtu Azure, je nejprve nutno přidat hello účet tooCloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="3a22a-112">tooview hello resources associated with an Azure account, you must first add hello account tooCloud Explorer.</span></span> 

1. <span data-ttu-id="3a22a-113">V **Průzkumník cloudu**, vyberte **nastavení účtu Azure**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-113">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="3a22a-115">Vyberte **nový účet přidal**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-115">Select **Add new account**.</span></span> 

    ![Odkaz Přidat účet Průzkumník cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. <span data-ttu-id="3a22a-117">Přihlaste se toohello účet Azure, jejichž zdroje, které má toobrowse.</span><span class="sxs-lookup"><span data-stu-id="3a22a-117">Log in toohello Azure account whose resources you want toobrowse.</span></span> 

1. <span data-ttu-id="3a22a-118">Po přihlášení tooan účet Azure, zobrazí se hello odběry přidružené k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="3a22a-118">Once logged in tooan Azure account, hello subscriptions associated with that account display.</span></span> <span data-ttu-id="3a22a-119">Vyberte hello zaškrtávací políčka pro předplatná účet hello chcete toobrowse a pak vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-119">Select hello check boxes for hello account subscriptions you want toobrowse and then select **Apply**.</span></span> 
 
    ![Průzkumník cloudu: Vyberte předplatná Azure toodisplay](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. <span data-ttu-id="3a22a-121">Po výběru hello odběry jejichž prostředků, které chcete zobrazit toobrowse, tyto odběry a prostředky v hello Průzkumník cloudu.</span><span class="sxs-lookup"><span data-stu-id="3a22a-121">After selecting hello subscriptions whose resources you want toobrowse, those subscriptions and resources display in hello Cloud Explorer.</span></span>

    ![Průzkumník prostředků výpis pro účet Azure v cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a><span data-ttu-id="3a22a-123">Odeberte účet Azure z Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="3a22a-123">Remove an Azure account from Cloud Explorer</span></span> 

1. <span data-ttu-id="3a22a-124">V **Průzkumník cloudu**, vyberte **nastavení účtu Azure**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-124">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="3a22a-126">Vyberte další toohello účet chcete tooremove, **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-126">Next toohello account you want tooremove, select **Remove**.</span></span>

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a><span data-ttu-id="3a22a-128">Zobrazit typy prostředků nebo skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3a22a-128">View resource types or resource groups</span></span>
<span data-ttu-id="3a22a-129">tooview prostředků Azure, můžete zvolit buď **typy prostředků** nebo **skupiny prostředků** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3a22a-129">tooview your Azure resources, you can choose either **Resource Types** or **Resource Groups** view.</span></span>

1. <span data-ttu-id="3a22a-130">V **Průzkumník cloudu**, vyberte hello rozevírací zobrazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="3a22a-130">In **Cloud Explorer**, select hello resource view dropdown.</span></span>

    ![Cloud Explorer rozevíracího seznamu tooselect hello požadovaného zobrazení prostředků](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. <span data-ttu-id="3a22a-132">Hello místní nabídce vyberte hello požadovaného zobrazení:</span><span class="sxs-lookup"><span data-stu-id="3a22a-132">From hello context menu, select hello desired view:</span></span> 

    - <span data-ttu-id="3a22a-133">**Typy prostředků** zobrazení – zobrazení běžných hello používá na hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), zobrazuje vašich prostředků Azure uspořádány do kategorií podle jejich typu, jako jsou webové aplikace, účty úložiště a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3a22a-133">**Resource Types** view - hello common view used on hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), shows your Azure resources categorized by their type, such as web apps, storage accounts, and virtual machines.</span></span> 
    - <span data-ttu-id="3a22a-134">**Skupiny prostředků** zobrazit - prostředky Azure rozděluje skupinou hello prostředků Azure, ke kterému jsou přidružené.</span><span class="sxs-lookup"><span data-stu-id="3a22a-134">**Resource Groups** view - Categorizes Azure resources by hello Azure resource group with which they're associated.</span></span> <span data-ttu-id="3a22a-135">Skupina prostředků je sada prostředky Azure, obvykle používají v konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3a22a-135">A resource group is a bundle of Azure resources, typically used by a specific application.</span></span> <span data-ttu-id="3a22a-136">toolearn Další informace o skupin prostředků Azure, najdete v části [přehled Azure Resource Manageru](./azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a22a-136">toolearn more about Azure resource groups, see [Azure Resource Manager Overview](./azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="3a22a-137">Hello následující obrázek ukazuje porovnání hello dvě zobrazení prostředků:</span><span class="sxs-lookup"><span data-stu-id="3a22a-137">hello following image shows a comparison of hello two resource views:</span></span>

    ![Porovnání zobrazení Průzkumníka prostředků cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a><span data-ttu-id="3a22a-139">Zobrazení a přejděte prostředky v Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="3a22a-139">View and navigate resources in Cloud Explorer</span></span>
<span data-ttu-id="3a22a-140">toonavigate tooan prostředků Azure a zobrazit její informace v Průzkumníku cloudu, rozbalte položku hello typu nebo související skupiny prostředků a pak vyberte hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="3a22a-140">toonavigate tooan Azure resource and view its information in Cloud Explorer, expand hello item's type or associated resource group and then select hello resource.</span></span> <span data-ttu-id="3a22a-141">Když vyberete prostředek, informace se zobrazí v hello dvě karty - **akce** a **vlastnosti** – dole hello v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="3a22a-141">When you select a resource, information appears in hello two tabs - **Actions** and **Properties** - at hello bottom of Cloud Explorer.</span></span> 

- <span data-ttu-id="3a22a-142">**Akce** kartě - seznamy hello akce může trvat v Průzkumníku cloudu pro hello vybraný prostředek.</span><span class="sxs-lookup"><span data-stu-id="3a22a-142">**Actions** tab - Lists hello actions you can take in Cloud Explorer for hello selected resource.</span></span> <span data-ttu-id="3a22a-143">Můžete také zobrazit tyto možnosti kliknutím pravým tlačítkem na hello prostředků tooview kontextovou nabídku.</span><span class="sxs-lookup"><span data-stu-id="3a22a-143">You can also view these options by right-clicking hello resource tooview its context menu.</span></span>

- <span data-ttu-id="3a22a-144">**Vlastnosti** kartě - zobrazí vlastnosti hello hello prostředků, například jeho typ, národní prostředí a prostředků skupiny, ke kterému je přiřazeno.</span><span class="sxs-lookup"><span data-stu-id="3a22a-144">**Properties** tab - Shows hello properties of hello resource, such as its type, locale, and resource group with which it is associated.</span></span>

<span data-ttu-id="3a22a-145">Hello následující obrázek ukazuje příklad porovnání zobrazené na jednotlivých kartách pro aplikační službu:</span><span class="sxs-lookup"><span data-stu-id="3a22a-145">hello following image shows an example comparison of what you see on each tab for an App Service:</span></span>

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

<span data-ttu-id="3a22a-146">Každý prostředek má akce hello **otevřít na portálu**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-146">Every resource has hello action **Open in portal**.</span></span> <span data-ttu-id="3a22a-147">Pokud zvolíte tuto akci, Průzkumník cloudu zobrazí hello vybrané prostředků v hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="3a22a-147">When you choose this action, Cloud Explorer displays hello selected resource in hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="3a22a-148">Hello **otevřít na portálu** funkce je užitečný pro navigaci toodeeply vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="3a22a-148">hello **Open in portal** feature is handy for navigating toodeeply nested resources.</span></span>

<span data-ttu-id="3a22a-149">Další akce a hodnot vlastností mohou zobrazit i podle hello prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3a22a-149">Additional actions and property values may also appear based on hello Azure resource.</span></span> <span data-ttu-id="3a22a-150">Webové aplikace a aplikace logiky také mít například hello akce **otevřít v prohlížeči** a **připojit ladicí program** kromě příliš**otevřít na portálu**.</span><span class="sxs-lookup"><span data-stu-id="3a22a-150">For example, web apps and logic apps also have hello actions **Open in browser** and **Attach debugger** in addition too**Open in portal**.</span></span> <span data-ttu-id="3a22a-151">Editory tooopen akce se zobrazí, když zvolíte účet úložiště objektů blob, fronty nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="3a22a-151">Actions tooopen editors appear when you choose a storage account blob, queue, or table.</span></span> <span data-ttu-id="3a22a-152">Azure aplikací mít **URL** a **stav** vlastností, zatímco prostředky úložiště mají vlastnosti klíče a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="3a22a-152">Azure apps have **URL** and **Status** properties, while storage resources have key and connection string properties.</span></span>

## <a name="find-resources-in-cloud-explorer"></a><span data-ttu-id="3a22a-153">Vyhledávání prostředků v Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="3a22a-153">Find resources in Cloud Explorer</span></span>
<span data-ttu-id="3a22a-154">toolocate prostředky s konkrétním názvem v rámci vašich předplatných účet Azure, zadejte název hello v hello **vyhledávání** pole v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="3a22a-154">toolocate resources with a specific name in your Azure account subscriptions, enter hello name in hello **Search** box in Cloud Explorer.</span></span>

![Vyhledávání prostředků v Průzkumníku cloudu](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

<span data-ttu-id="3a22a-156">Při zadávání znaků v hello **vyhledávání** pole jenom prostředky, které splňují tyto znaky se zobrazí ve stromu hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="3a22a-156">As you enter characters in hello **Search** box, only resources that match those characters appear in hello resource tree.</span></span>
