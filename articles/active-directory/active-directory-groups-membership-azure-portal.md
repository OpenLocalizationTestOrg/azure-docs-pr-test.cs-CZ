---
title: "Správa skupin vaše skupina patří v Azure Active Directory | Microsoft Docs"
description: "Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory. Chcete-li spravovat jejich členství."
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
ms.openlocfilehash: 08e04a6590176c4084ca47b4bd6cbb22500eca2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a><span data-ttu-id="b5b33-104">Spravovat do skupiny, které patří skupiny v klientovi služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b5b33-104">Manage to which groups a group belongs in your Azure Active Directory tenant</span></span>
<span data-ttu-id="b5b33-105">Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b5b33-105">Groups can contain other groups in Azure Active Directory.</span></span> <span data-ttu-id="b5b33-106">Chcete-li spravovat jejich členství.</span><span class="sxs-lookup"><span data-stu-id="b5b33-106">Here's how to manage those memberships.</span></span>

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a><span data-ttu-id="b5b33-107">Jak lze najít Moje skupina je členem skupiny?</span><span class="sxs-lookup"><span data-stu-id="b5b33-107">How do I find the groups my group is a member of?</span></span>
1. <span data-ttu-id="b5b33-108">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="b5b33-108">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="b5b33-109">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b5b33-109">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. <span data-ttu-id="b5b33-111">Na **uživatelů a skupin** vyberte **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="b5b33-111">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Otevření okna skupiny](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="b5b33-113">Na **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="b5b33-113">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="b5b33-114">Na **skupiny - *groupname***  vyberte **členství ve skupinách**.</span><span class="sxs-lookup"><span data-stu-id="b5b33-114">On the **Group - *groupname*** blade, select **Group memberships**.</span></span>

   ![Otevření okna členství ve skupině](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. <span data-ttu-id="b5b33-116">Přidání skupiny jako členem jiné skupiny, na **skupiny – členství ve skupinách** okně, vyberte **přidat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b5b33-116">To add your group as a member of another group, on the **Group - Group memberships** blade, select the **Add** command.</span></span>
7. <span data-ttu-id="b5b33-117">Vyberte skupinu z **vybrat skupinu** a pak vyberte **vyberte** tlačítko v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="b5b33-117">Select a group from the **Select Group** blade, and then select the **Select** button at the bottom of the blade.</span></span> <span data-ttu-id="b5b33-118">Skupiny můžete přidat pouze do jedné skupiny najednou.</span><span class="sxs-lookup"><span data-stu-id="b5b33-118">You can add your group to only one group at a time.</span></span> <span data-ttu-id="b5b33-119">**Uživatele** pole filtry zobrazení na základě shody zadání vašeho jakékoliv části názvu uživatele nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="b5b33-119">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="b5b33-120">V tomto poli, jsou přijaty žádné zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="b5b33-120">No wildcard characters are accepted in that box.</span></span>

   ![Přidat členství ve skupině](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. <span data-ttu-id="b5b33-122">Odebrání skupiny jako členem jiné skupiny, na **skupiny – členství ve skupinách** okně vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="b5b33-122">To remove your group as a member of another group, on the **Group - Group memberships** blade, select a group.</span></span>
9. <span data-ttu-id="b5b33-123">Na ***groupname*** okně, vyberte **odebrat** příkazů a potvrďte svou volbu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b5b33-123">On the ***groupname*** blade, select the **Remove** command, and confirm your choice at the prompt.</span></span>

   ![Odeberte příkaz členství](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. <span data-ttu-id="b5b33-125">Po dokončení změn členství ve skupinách pro skupinu, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b5b33-125">When you finish changing group memberships for your group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="b5b33-126">Další informace</span><span class="sxs-lookup"><span data-stu-id="b5b33-126">Additional information</span></span>
<span data-ttu-id="b5b33-127">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b5b33-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="b5b33-128">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="b5b33-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="b5b33-129">Vytvoření nové skupiny a přidávání členů</span><span class="sxs-lookup"><span data-stu-id="b5b33-129">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="b5b33-130">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="b5b33-130">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="b5b33-131">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="b5b33-131">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="b5b33-132">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="b5b33-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
