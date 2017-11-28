---
title: "aaaRemove uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak tooremove hello přistupovat k přiřazení uživatele nebo skupiny z podnikové aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: c067ecf59b4dedfe8f848357ca8bd545bdc610eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="7eb86-103">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7eb86-103">Remove a user or group assignment from an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="7eb86-104">Je snadno tooremove uživatele nebo skupiny z přiřazení přístupu tooone podnikových aplikací ve službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7eb86-104">It's easy tooremove a user or a group from being assigned access tooone of your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="7eb86-105">Musíte mít hello příslušná oprávnění toomanage hello firemní aplikace a musí být globální správce adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="7eb86-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-remove-a-user-or-group-assignment"></a><span data-ttu-id="7eb86-106">Jak odebrat uživatele nebo přiřazení skupiny?</span><span class="sxs-lookup"><span data-stu-id="7eb86-106">How do I remove a user or group assignment?</span></span>
1. <span data-ttu-id="7eb86-107">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="7eb86-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="7eb86-108">Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="7eb86-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="7eb86-109">Na hello **Azure Active Directory - *directoryname***  okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7eb86-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="7eb86-111">Na hello **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7eb86-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="7eb86-112">Zobrazí se seznam hello aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="7eb86-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="7eb86-113">Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7eb86-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="7eb86-114">Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="7eb86-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Users & Groups**.</span></span>

    ![Výběr uživatelů nebo skupin](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. <span data-ttu-id="7eb86-116">Na hello ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte jednu z další uživatele nebo skupiny a pak vyberte hello **odebrat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="7eb86-116">On hello ***appname*** **- User & Group Assignment** blade, select one of more users or groups and then select hello **Remove** command.</span></span> <span data-ttu-id="7eb86-117">Zkontrolujte vaše rozhodnutí hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="7eb86-117">Confirm your decision at hello prompt.</span></span>

    ![Vyberte příkaz odebrat hello](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a><span data-ttu-id="7eb86-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7eb86-119">Next steps</span></span>
* [<span data-ttu-id="7eb86-120">Zobrazit všechny moje skupin</span><span class="sxs-lookup"><span data-stu-id="7eb86-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="7eb86-121">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="7eb86-121">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="7eb86-122">Zakázat přihlášení uživatele pro aplikaci, enterprise</span><span class="sxs-lookup"><span data-stu-id="7eb86-122">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="7eb86-123">Změňte název hello nebo logo aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="7eb86-123">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
