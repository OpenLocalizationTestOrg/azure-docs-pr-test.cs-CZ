---
title: "Přiřadit uživatele nebo skupinu enterprise aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak vybrat aplikaci enterprise přiřadit uživatele nebo skupinu k němu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: ee784704ada9238b5cd048f99aaa4cb192ec7d57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="9f1d5-103">Přiřadit uživatele nebo skupinu enterprise aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f1d5-103">Assign a user or group to an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="9f1d5-104">Je snadné přiřadit uživatele nebo skupinu pro vaše podnikové aplikace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f1d5-104">It's easy to assign a user or a group to your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9f1d5-105">Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a><span data-ttu-id="9f1d5-106">Přiřazení přístupu uživatelů k aplikaci enterprise</span><span class="sxs-lookup"><span data-stu-id="9f1d5-106">How do I assign user access to an enterprise app?</span></span>
1. <span data-ttu-id="9f1d5-107">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="9f1d5-108">Vyberte **další služby**, zadejte do textového pole Azure Active Directory a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-108">Select **More services**, enter Azure Active Directory in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="9f1d5-109">Na **Azure Active Directory – *directoryname***  okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="9f1d5-111">Na **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="9f1d5-112">Zobrazí seznam aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="9f1d5-113">Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="9f1d5-114">Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Users & Groups**.</span></span>

    ![Výběr příkaz všechny aplikace](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. <span data-ttu-id="9f1d5-116">Na ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte **přidat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-116">On the ***appname*** **- User & Group Assignment** blade, select the **Add** command.</span></span>
8. <span data-ttu-id="9f1d5-117">Na **přidat přiřazení** vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-117">On the **Add Assignment** blade, select **Users and groups**.</span></span>

    ![Přiřazení uživatele nebo skupiny do aplikace](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. <span data-ttu-id="9f1d5-119">Na **uživatelů a skupin** okně vyberte jeden nebo více uživatelů nebo skupin ze seznamu a pak vyberte **vyberte** tlačítko v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-119">On the **Users and groups** blade, select one or more users or groups from the list and then select the **Select** button at the bottom of the blade.</span></span>
10. <span data-ttu-id="9f1d5-120">Na **přidat přiřazení** vyberte **Role**.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-120">On the **Add Assignment** blade, select **Role**.</span></span> <span data-ttu-id="9f1d5-121">Potom na **vybrat roli** okně, vyberte roli, které chcete použít na vybrané uživatele nebo skupiny a pak vyberte **OK** tlačítko v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-121">Then, on the **Select Role** blade, select a role to apply to the selected users or groups, and then select the **OK** button at the bottom of the blade.</span></span>
11. <span data-ttu-id="9f1d5-122">Na **přidat přiřazení** okně, vyberte **přiřadit** tlačítko v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-122">On the **Add Assignment** blade, select the **Assign** button at the bottom of the blade.</span></span> <span data-ttu-id="9f1d5-123">Přiřazené uživatele nebo skupiny, bude mít oprávnění definovaná podle vybrané role pro tuto aplikaci enterprise.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-123">The assigned users or groups will have the permissions defined by the selected role for this enterprise app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f1d5-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f1d5-124">Next steps</span></span>
* [<span data-ttu-id="9f1d5-125">Zobrazit všechny moje skupin</span><span class="sxs-lookup"><span data-stu-id="9f1d5-125">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="9f1d5-126">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f1d5-126">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="9f1d5-127">Zakázat přihlášení uživatele pro aplikaci, enterprise</span><span class="sxs-lookup"><span data-stu-id="9f1d5-127">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="9f1d5-128">Změna názvu nebo logo aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="9f1d5-128">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
