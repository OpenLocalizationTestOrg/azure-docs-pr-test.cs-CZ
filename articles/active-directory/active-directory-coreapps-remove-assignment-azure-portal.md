---
title: "Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Postup přiřazení přístup uživatele nebo skupinu odebrat z podnikové aplikace v Azure Active Directory"
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
ms.openlocfilehash: 02f122acfb53c2107e2b0af66c6195aa127a2c77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="2a38c-103">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2a38c-103">Remove a user or group assignment from an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="2a38c-104">Je snadno odebrat uživatele nebo skupiny z se přiřadí přístup k jednomu z vaší podnikové aplikace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2a38c-104">It's easy to remove a user or a group from being assigned access to one of your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2a38c-105">Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="2a38c-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-remove-a-user-or-group-assignment"></a><span data-ttu-id="2a38c-106">Jak odebrat uživatele nebo přiřazení skupiny?</span><span class="sxs-lookup"><span data-stu-id="2a38c-106">How do I remove a user or group assignment?</span></span>
1. <span data-ttu-id="2a38c-107">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="2a38c-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="2a38c-108">Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2a38c-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="2a38c-109">Na **Azure Active Directory – *directoryname***  okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2a38c-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="2a38c-111">Na **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2a38c-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="2a38c-112">Zobrazí seznam aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="2a38c-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="2a38c-113">Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a38c-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="2a38c-114">Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="2a38c-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Users & Groups**.</span></span>

    ![Výběr uživatelů nebo skupin](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. <span data-ttu-id="2a38c-116">Na ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte jednu z další uživatele nebo skupiny a pak vyberte **odebrat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="2a38c-116">On the ***appname*** **- User & Group Assignment** blade, select one of more users or groups and then select the **Remove** command.</span></span> <span data-ttu-id="2a38c-117">Zkontrolujte vaše rozhodnutí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2a38c-117">Confirm your decision at the prompt.</span></span>

    ![Vyberte příkaz odebrat](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a><span data-ttu-id="2a38c-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a38c-119">Next steps</span></span>
* [<span data-ttu-id="2a38c-120">Zobrazit všechny moje skupin</span><span class="sxs-lookup"><span data-stu-id="2a38c-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="2a38c-121">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="2a38c-121">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="2a38c-122">Zakázat přihlášení uživatele pro aplikaci, enterprise</span><span class="sxs-lookup"><span data-stu-id="2a38c-122">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="2a38c-123">Změna názvu nebo logo aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="2a38c-123">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
