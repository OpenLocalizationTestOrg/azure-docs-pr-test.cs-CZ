---
title: "aaaDisable uživatelská přihlášení pro podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak toodisable podniková aplikace tak, aby žádní uživatelé můžou přihlásit tooit v Azure Active Directory"
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
ms.openlocfilehash: 4c560b59359d433b0852a7606cc2cc0204866234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="1db4b-103">Zakázat přihlášení uživatele pro podnikové aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1db4b-103">Disable user sign-ins for an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="1db4b-104">Je snadno toodisable podniková aplikace tak, aby žádní uživatelé můžou přihlásit tooit v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1db4b-104">It's easy toodisable an enterprise application so that no users may sign in tooit in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="1db4b-105">Musíte mít hello příslušná oprávnění toomanage hello firemní aplikace a musí být globální správce adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="1db4b-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-disable-user-sign-ins"></a><span data-ttu-id="1db4b-106">Jakým způsobem vypnout uživatelská přihlášení?</span><span class="sxs-lookup"><span data-stu-id="1db4b-106">How do I disable user sign-ins?</span></span>
1. <span data-ttu-id="1db4b-107">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="1db4b-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="1db4b-108">Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1db4b-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="1db4b-109">Na hello **Azure Active Directory** -  ***directoryname*** okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1db4b-109">On hello **Azure Active Directory** -  ***directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="1db4b-111">Na hello **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1db4b-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="1db4b-112">Zobrazí seznam hello aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="1db4b-112">You see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="1db4b-113">Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1db4b-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="1db4b-114">Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1db4b-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Properties**.</span></span>

    ![Výběr hello příkaz všechny aplikace](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. <span data-ttu-id="1db4b-116">Na hello ***appname*** - **vlastnosti** vyberte **ne** pro **povolit pro uživatele v toosign?**.</span><span class="sxs-lookup"><span data-stu-id="1db4b-116">On hello ***appname*** - **Properties** blade, select **No** for **Enabled for users toosign-in?**.</span></span>
8. <span data-ttu-id="1db4b-117">Vyberte hello **Uložit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="1db4b-117">Select hello **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1db4b-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1db4b-118">Next steps</span></span>
* [<span data-ttu-id="1db4b-119">Zobrazení všech Moje skupin</span><span class="sxs-lookup"><span data-stu-id="1db4b-119">See all my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="1db4b-120">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="1db4b-120">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="1db4b-121">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1db4b-121">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="1db4b-122">Změňte název hello nebo logo aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="1db4b-122">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
