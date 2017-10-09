---
title: "aaaAssign uživatel tooadministrator role v Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, jak v Azure Active Directory toochange informace o administrativní uživatele"
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
ms.openlocfilehash: 8bb6711c2570843962fe075b6026542a4bca525f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-tooadministrator-roles-in-azure-active-directory"></a><span data-ttu-id="63547-103">Přiřadit uživatele tooadministrator role v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63547-103">Assign a user tooadministrator roles in Azure Active Directory</span></span>
<span data-ttu-id="63547-104">Tento článek vysvětluje, jak tooassign tooa uživatel s rolí správce v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="63547-104">This article explains how tooassign an administrative role tooa user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="63547-105">Informace o přidávání nových uživatelů v organizaci najdete v tématu [přidat nové uživatele tooAzure služby Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="63547-105">For information about adding new users in your organization, see [Add new users tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span></span> <span data-ttu-id="63547-106">Přidaní uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete kdykoli přiřadit role toothem.</span><span class="sxs-lookup"><span data-stu-id="63547-106">Added users don't have administrator permissions by default, but you can assign roles toothem at any time.</span></span>

## <a name="assign-a-role-tooa-user"></a><span data-ttu-id="63547-107">Přiřazení role uživatele tooa</span><span class="sxs-lookup"><span data-stu-id="63547-107">Assign a role tooa user</span></span>
1. <span data-ttu-id="63547-108">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="63547-108">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="63547-109">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="63547-109">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="63547-111">Na hello **uživatelů a skupin** vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="63547-111">On hello **Users and groups** blade, select **All users**.</span></span>

   ![Otevírání hello všechny okno Uživatelé](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="63547-113">Na hello **uživatelé a skupiny - všichni uživatelé** okně vyberte uživatele ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="63547-113">On hello **Users and groups - All users** blade, select a user from hello list.</span></span>
5. <span data-ttu-id="63547-114">V okně hello hello vybraného uživatele, vyberte **Directory role**a pak mu přiřaďte role tooa hello uživatele z hello **Directory role** seznamu.</span><span class="sxs-lookup"><span data-stu-id="63547-114">On hello blade for hello selected user, select **Directory role**, and then assign hello user tooa role from hello **Directory role** list.</span></span> <span data-ttu-id="63547-115">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="63547-115">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>

      ![Přiřazení role tooa uživatele](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. <span data-ttu-id="63547-117">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="63547-117">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63547-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63547-118">Next steps</span></span>
* [<span data-ttu-id="63547-119">Přidat uživatele</span><span class="sxs-lookup"><span data-stu-id="63547-119">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="63547-120">Resetovat heslo uživatele v hello nový portál Azure</span><span class="sxs-lookup"><span data-stu-id="63547-120">Reset a user's password in hello new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="63547-121">Změnit informace o práci uživatele</span><span class="sxs-lookup"><span data-stu-id="63547-121">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="63547-122">Správa uživatelských profilů</span><span class="sxs-lookup"><span data-stu-id="63547-122">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="63547-123">Odstranění uživatele ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="63547-123">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
