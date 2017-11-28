---
title: "Přiřazení uživatele k rolí správce ve službě Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, jak změnit správce informace o uživateli v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: bfadf133154488f9827cfbeaa98ddb0eb84b52f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a><span data-ttu-id="23765-103">Přiřazení uživatele k rolí správce ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23765-103">Assign a user to administrator roles in Azure Active Directory</span></span>
<span data-ttu-id="23765-104">Tento článek vysvětluje, jak přiřadit roli správce pro uživatele v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="23765-104">This article explains how to assign an administrative role to a user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="23765-105">Informace o přidávání nových uživatelů v organizaci najdete v tématu [přidání nových uživatelů do služby Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="23765-105">For information about adding new users in your organization, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span> <span data-ttu-id="23765-106">Přidaní uživatelé nemají ve výchozím nastavení oprávnění správce, ale příslušné role jim můžete kdykoli přiřadit.</span><span class="sxs-lookup"><span data-stu-id="23765-106">Added users don't have administrator permissions by default, but you can assign roles to them at any time.</span></span>

## <a name="assign-a-role-to-a-user"></a><span data-ttu-id="23765-107">Přiřazení role uživatele</span><span class="sxs-lookup"><span data-stu-id="23765-107">Assign a role to a user</span></span>
1. <span data-ttu-id="23765-108">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="23765-108">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="23765-109">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="23765-109">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="23765-111">Na **uživatelů a skupin** vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="23765-111">On the **Users and groups** blade, select **All users**.</span></span>

   ![Otevření okna všechny uživatele](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="23765-113">Na **uživatelé a skupiny - všichni uživatelé** okně vyberte uživatele ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="23765-113">On the **Users and groups - All users** blade, select a user from the list.</span></span>
5. <span data-ttu-id="23765-114">V okně pro vybraného uživatele, vyberte **Directory role**a pak mu přiřaďte uživatele k roli z **Directory role** seznamu.</span><span class="sxs-lookup"><span data-stu-id="23765-114">On the blade for the selected user, select **Directory role**, and then assign the user to a role from the **Directory role** list.</span></span> <span data-ttu-id="23765-115">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="23765-115">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>

      ![Přiřazení uživatele k roli](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. <span data-ttu-id="23765-117">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="23765-117">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23765-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23765-118">Next steps</span></span>
* [<span data-ttu-id="23765-119">Přidat uživatele</span><span class="sxs-lookup"><span data-stu-id="23765-119">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="23765-120">Resetovat heslo uživatele v nový portál Azure</span><span class="sxs-lookup"><span data-stu-id="23765-120">Reset a user's password in the new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="23765-121">Změnit informace o práci uživatele</span><span class="sxs-lookup"><span data-stu-id="23765-121">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="23765-122">Správa uživatelských profilů</span><span class="sxs-lookup"><span data-stu-id="23765-122">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="23765-123">Odstranění uživatele ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="23765-123">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
