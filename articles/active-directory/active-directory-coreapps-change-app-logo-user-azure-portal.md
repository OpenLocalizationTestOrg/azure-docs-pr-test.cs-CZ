---
title: "Změna názvu nebo logo aplikace organizace v Azure Active Directory | Microsoft Docs"
description: "Jak změnit název nebo logo pro vlastní firemní aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 3e44e876dcbac704a9809ae5b3957bf94be21c48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="55b33-103">Změna názvu nebo logo aplikace organizace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="55b33-103">Change the name or logo of an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="55b33-104">Je snadno změnit název nebo logo pro vlastní podniková aplikace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="55b33-104">It's easy to change the name or logo for a custom enterprise application in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="55b33-105">Musí mít příslušná oprávnění k provedení těchto změn a musí být Tvůrce vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55b33-105">You must have the appropriate permissions to make these changes, and you must be the creator of the custom app.</span></span>

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a><span data-ttu-id="55b33-106">Změna názvem a logem firemní aplikace?</span><span class="sxs-lookup"><span data-stu-id="55b33-106">How do I change an enterprise app's name or logo?</span></span>
1. <span data-ttu-id="55b33-107">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="55b33-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="55b33-108">Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="55b33-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="55b33-109">Na **Azure Active Directory – *directoryname***  okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="55b33-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="55b33-111">Na **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="55b33-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="55b33-112">Zobrazí seznam aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="55b33-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="55b33-113">Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55b33-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="55b33-114">Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="55b33-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Properties**.</span></span>

    ![Výběr příkaz Vlastnosti](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. <span data-ttu-id="55b33-116">Na ***appname*** **-vlastnosti** okno, vyhledat soubor používat jako nové logo, nebo upravit název aplikace nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="55b33-116">On the ***appname*** **- Properties** blade, browse for a file to use as a new logo, or edit the app name, or both.</span></span>

    ![Změna příkaz logo nebo nameproperties aplikace](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. <span data-ttu-id="55b33-118">Vyberte **Uložit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="55b33-118">Select the **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55b33-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55b33-119">Next steps</span></span>
* [<span data-ttu-id="55b33-120">Zobrazit všechny moje skupin</span><span class="sxs-lookup"><span data-stu-id="55b33-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="55b33-121">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="55b33-121">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="55b33-122">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="55b33-122">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="55b33-123">Zakázat přihlášení uživatele pro aplikaci, enterprise</span><span class="sxs-lookup"><span data-stu-id="55b33-123">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
