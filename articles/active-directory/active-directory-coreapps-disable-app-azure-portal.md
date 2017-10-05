---
title: "Zakázat přihlášení uživatele pro podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Zakázání podniková aplikace tak, aby žádní uživatelé můžou přihlásit k němu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 5d27046370eada0c371c94fb573fa1bcf536f7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="72f1b-103">Zakázat přihlášení uživatele pro podnikové aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="72f1b-103">Disable user sign-ins for an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="72f1b-104">Je snadné zakázat podniková aplikace tak, aby žádní uživatelé můžou přihlásit k němu v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="72f1b-104">It's easy to disable an enterprise application so that no users may sign in to it in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="72f1b-105">Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="72f1b-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-disable-user-sign-ins"></a><span data-ttu-id="72f1b-106">Jakým způsobem vypnout uživatelská přihlášení?</span><span class="sxs-lookup"><span data-stu-id="72f1b-106">How do I disable user sign-ins?</span></span>
1. <span data-ttu-id="72f1b-107">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="72f1b-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="72f1b-108">Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="72f1b-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="72f1b-109">Na **Azure Active Directory** -  ***directoryname*** okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="72f1b-109">On the **Azure Active Directory** -  ***directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="72f1b-111">Na **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="72f1b-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="72f1b-112">Zobrazí seznam aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="72f1b-112">You see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="72f1b-113">Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="72f1b-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="72f1b-114">Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="72f1b-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Properties**.</span></span>

    ![Výběr příkaz všechny aplikace](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. <span data-ttu-id="72f1b-116">Na ***appname*** - **vlastnosti** vyberte **ne** pro **povolit pro uživatele přihlásit?**.</span><span class="sxs-lookup"><span data-stu-id="72f1b-116">On the ***appname*** - **Properties** blade, select **No** for **Enabled for users to sign-in?**.</span></span>
8. <span data-ttu-id="72f1b-117">Vyberte **Uložit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="72f1b-117">Select the **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72f1b-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72f1b-118">Next steps</span></span>
* [<span data-ttu-id="72f1b-119">Zobrazení všech Moje skupin</span><span class="sxs-lookup"><span data-stu-id="72f1b-119">See all my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="72f1b-120">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="72f1b-120">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="72f1b-121">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="72f1b-121">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="72f1b-122">Změna názvu nebo logo aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="72f1b-122">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
