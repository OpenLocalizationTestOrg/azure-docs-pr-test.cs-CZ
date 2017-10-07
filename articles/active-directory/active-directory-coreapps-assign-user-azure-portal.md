---
title: "aaaAssign aplikace enterprise uživatele nebo skupinu tooan v Azure Active Directory | Microsoft Docs"
description: "Jak tooselect podnikové aplikace tooassign uživatele nebo skupinu tooit v Azure Active Directory"
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
ms.openlocfilehash: 86c11f19892b9c947a5331677c17759178ed2806
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-or-group-tooan-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="f6ead-103">Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f6ead-103">Assign a user or group tooan enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="f6ead-104">Je snadno tooassign uživatele nebo skupiny tooyour podnikové aplikace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f6ead-104">It's easy tooassign a user or a group tooyour enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="f6ead-105">Musíte mít hello příslušná oprávnění toomanage hello firemní aplikace a musí být globální správce adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="f6ead-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-assign-user-access-tooan-enterprise-app"></a><span data-ttu-id="f6ead-106">Přiřazení uživatelů přístup tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="f6ead-106">How do I assign user access tooan enterprise app?</span></span>
1. <span data-ttu-id="f6ead-107">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="f6ead-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="f6ead-108">Vyberte **další služby**, zadejte do textového pole hello Azure Active Directory a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f6ead-108">Select **More services**, enter Azure Active Directory in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="f6ead-109">Na hello **Azure Active Directory - *directoryname***  okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f6ead-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="f6ead-111">Na hello **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f6ead-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="f6ead-112">Zobrazí se seznam hello aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="f6ead-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="f6ead-113">Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f6ead-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="f6ead-114">Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="f6ead-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Users & Groups**.</span></span>

    ![Výběr hello příkaz všechny aplikace](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. <span data-ttu-id="f6ead-116">Na hello ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte hello **přidat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="f6ead-116">On hello ***appname*** **- User & Group Assignment** blade, select hello **Add** command.</span></span>
8. <span data-ttu-id="f6ead-117">Na hello **přidat přiřazení** vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f6ead-117">On hello **Add Assignment** blade, select **Users and groups**.</span></span>

    ![Přiřadit uživatele nebo skupiny toohello aplikace](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. <span data-ttu-id="f6ead-119">Na hello **uživatelů a skupin** okně, vyberte jeden nebo více uživatele nebo skupiny z hello seznamu a pak vyberte hello **vyberte** tlačítko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="f6ead-119">On hello **Users and groups** blade, select one or more users or groups from hello list and then select hello **Select** button at hello bottom of hello blade.</span></span>
10. <span data-ttu-id="f6ead-120">Na hello **přidat přiřazení** vyberte **Role**.</span><span class="sxs-lookup"><span data-stu-id="f6ead-120">On hello **Add Assignment** blade, select **Role**.</span></span> <span data-ttu-id="f6ead-121">Potom na hello **vybrat roli** okně, vyberte toohello tooapply role vybrané uživatele nebo skupiny a potom vyberte hello **OK** tlačítko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="f6ead-121">Then, on hello **Select Role** blade, select a role tooapply toohello selected users or groups, and then select hello **OK** button at hello bottom of hello blade.</span></span>
11. <span data-ttu-id="f6ead-122">Na hello **přidat přiřazení** okně, vyberte hello **přiřadit** tlačítko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="f6ead-122">On hello **Add Assignment** blade, select hello **Assign** button at hello bottom of hello blade.</span></span> <span data-ttu-id="f6ead-123">Hello přiřadit uživatele nebo skupiny budou mít oprávnění hello definované hello vybranou roli pro tuto aplikaci enterprise.</span><span class="sxs-lookup"><span data-stu-id="f6ead-123">hello assigned users or groups will have hello permissions defined by hello selected role for this enterprise app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6ead-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6ead-124">Next steps</span></span>
* [<span data-ttu-id="f6ead-125">Zobrazit všechny moje skupin</span><span class="sxs-lookup"><span data-stu-id="f6ead-125">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="f6ead-126">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f6ead-126">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="f6ead-127">Zakázat přihlášení uživatele pro aplikaci, enterprise</span><span class="sxs-lookup"><span data-stu-id="f6ead-127">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="f6ead-128">Změňte název hello nebo logo aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="f6ead-128">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
