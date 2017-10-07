---
title: "skupiny hello aaaManage vaše skupina patří tooin Azure Active Directory | Microsoft Docs"
description: "Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory. Tady je způsob toomanage jejich členství."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0a0a1967084de0968e1e802559f9cdfd7ca6ae4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-toowhich-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a><span data-ttu-id="b9dda-104">Správa skupin toowhich, ke které patří skupiny v klientovi služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b9dda-104">Manage toowhich groups a group belongs in your Azure Active Directory tenant</span></span>
<span data-ttu-id="b9dda-105">Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b9dda-105">Groups can contain other groups in Azure Active Directory.</span></span> <span data-ttu-id="b9dda-106">Tady je způsob toomanage jejich členství.</span><span class="sxs-lookup"><span data-stu-id="b9dda-106">Here's how toomanage those memberships.</span></span>

## <a name="how-do-i-find-hello-groups-my-group-is-a-member-of"></a><span data-ttu-id="b9dda-107">Jak lze najít hello skupiny, které Moje skupina je členem skupiny?</span><span class="sxs-lookup"><span data-stu-id="b9dda-107">How do I find hello groups my group is a member of?</span></span>
1. <span data-ttu-id="b9dda-108">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="b9dda-108">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="b9dda-109">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b9dda-109">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. <span data-ttu-id="b9dda-111">Na hello **uživatelů a skupin** vyberte **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="b9dda-111">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Okno skupiny otevírání hello](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="b9dda-113">Na hello **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="b9dda-113">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="b9dda-114">Na hello **skupiny - *groupname***  vyberte **členství ve skupinách**.</span><span class="sxs-lookup"><span data-stu-id="b9dda-114">On hello **Group - *groupname*** blade, select **Group memberships**.</span></span>

   ![Okno členství skupiny otevírání hello](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. <span data-ttu-id="b9dda-116">tooadd skupině jako členem jiné skupiny, na hello **skupiny – členství ve skupinách** okně, vyberte hello **přidat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b9dda-116">tooadd your group as a member of another group, on hello **Group - Group memberships** blade, select hello **Add** command.</span></span>
7. <span data-ttu-id="b9dda-117">Vyberte skupinu z hello **vybrat skupinu** okna a potom vyberte hello **vyberte** tlačítko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="b9dda-117">Select a group from hello **Select Group** blade, and then select hello **Select** button at hello bottom of hello blade.</span></span> <span data-ttu-id="b9dda-118">Najednou můžete přidat vaší skupiny tooonly jednu skupinu.</span><span class="sxs-lookup"><span data-stu-id="b9dda-118">You can add your group tooonly one group at a time.</span></span> <span data-ttu-id="b9dda-119">Hello **uživatele** pole filtry hello zobrazení na základě shody se vaše vstupní tooany součástí názvu uživatele nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9dda-119">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="b9dda-120">V tomto poli, jsou přijaty žádné zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="b9dda-120">No wildcard characters are accepted in that box.</span></span>

   ![Přidat členství ve skupině](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. <span data-ttu-id="b9dda-122">tooremove skupině jako členem jiné skupiny, na hello **skupiny – členství ve skupinách** okně vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="b9dda-122">tooremove your group as a member of another group, on hello **Group - Group memberships** blade, select a group.</span></span>
9. <span data-ttu-id="b9dda-123">Na hello ***groupname*** okně, vyberte hello **odebrat** příkazů a potvrďte svou volbu příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="b9dda-123">On hello ***groupname*** blade, select hello **Remove** command, and confirm your choice at hello prompt.</span></span>

   ![Odeberte příkaz členství](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. <span data-ttu-id="b9dda-125">Po dokončení změn členství ve skupinách pro skupinu, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9dda-125">When you finish changing group memberships for your group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="b9dda-126">Další informace</span><span class="sxs-lookup"><span data-stu-id="b9dda-126">Additional information</span></span>
<span data-ttu-id="b9dda-127">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b9dda-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="b9dda-128">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="b9dda-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="b9dda-129">Vytvoření nové skupiny a přidávání členů</span><span class="sxs-lookup"><span data-stu-id="b9dda-129">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="b9dda-130">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="b9dda-130">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="b9dda-131">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="b9dda-131">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="b9dda-132">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="b9dda-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
