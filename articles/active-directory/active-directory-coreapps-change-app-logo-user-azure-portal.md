---
title: "aaaChange hello názvem a logem podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak toochange hello názvem a logem vlastní podnikové aplikace v Azure Active Directory"
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
ms.openlocfilehash: b660e1f0f6c7ffd626e17e2e3399e7169f6e8bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="d7c3d-103">Změňte název hello nebo logo aplikace organizace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d7c3d-103">Change hello name or logo of an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="d7c3d-104">Je snadno toochange hello názvem a logem pro vlastní podniková aplikace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d7c3d-104">It's easy toochange hello name or logo for a custom enterprise application in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="d7c3d-105">Musí mít příslušná oprávnění toomake hello tyto změny, a musí být hello Tvůrce hello vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-105">You must have hello appropriate permissions toomake these changes, and you must be hello creator of hello custom app.</span></span>

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a><span data-ttu-id="d7c3d-106">Změna názvem a logem firemní aplikace?</span><span class="sxs-lookup"><span data-stu-id="d7c3d-106">How do I change an enterprise app's name or logo?</span></span>
1. <span data-ttu-id="d7c3d-107">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="d7c3d-108">Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="d7c3d-109">Na hello **Azure Active Directory - *directoryname***  okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="d7c3d-111">Na hello **podnikové aplikace, které** vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="d7c3d-112">Zobrazí se seznam hello aplikací, které můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="d7c3d-113">Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="d7c3d-114">Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Properties**.</span></span>

    ![Výběr příkaz Vlastnosti hello](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. <span data-ttu-id="d7c3d-116">Na hello ***appname*** **-vlastnosti** okno, vyhledejte soubor toouse jako nové logo nebo upravit název aplikace hello nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-116">On hello ***appname*** **- Properties** blade, browse for a file toouse as a new logo, or edit hello app name, or both.</span></span>

    ![Změna příkaz logo nebo nameproperties aplikace hello](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. <span data-ttu-id="d7c3d-118">Vyberte hello **Uložit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-118">Select hello **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7c3d-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7c3d-119">Next steps</span></span>
* [<span data-ttu-id="d7c3d-120">Zobrazit všechny moje skupin</span><span class="sxs-lookup"><span data-stu-id="d7c3d-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="d7c3d-121">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="d7c3d-121">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="d7c3d-122">Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d7c3d-122">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="d7c3d-123">Zakázat přihlášení uživatele pro aplikaci, enterprise</span><span class="sxs-lookup"><span data-stu-id="d7c3d-123">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
