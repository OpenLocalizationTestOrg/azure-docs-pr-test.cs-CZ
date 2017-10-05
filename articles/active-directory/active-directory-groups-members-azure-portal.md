---
title: "Spravovat členy skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Postup přidání nebo odebrání uživatelů a zařízení ze skupiny ve službě Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 044e88f95712e1cc5b5532f5492c78d711a8d858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a><span data-ttu-id="ef3da-103">Správa členství ve skupině pro uživatele v klientovi služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ef3da-103">Manage group membership for users in your Azure Active Directory tenant</span></span>
<span data-ttu-id="ef3da-104">Tento článek vysvětluje, jak spravovat členy skupiny ve službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ef3da-104">This article explains how to manage the members for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-the-members-and-manage-them"></a><span data-ttu-id="ef3da-105">Jak najít členy a spravovat?</span><span class="sxs-lookup"><span data-stu-id="ef3da-105">How do I find the members and manage them?</span></span>
1. <span data-ttu-id="ef3da-106">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="ef3da-106">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="ef3da-107">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="ef3da-107">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. <span data-ttu-id="ef3da-109">Na **uživatelů a skupin** vyberte **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="ef3da-109">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Otevření okna skupiny](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="ef3da-111">Na **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="ef3da-111">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="ef3da-112">Na **skupiny - *groupname***  vyberte **členy**.</span><span class="sxs-lookup"><span data-stu-id="ef3da-112">On the **Group - *groupname*** blade, select **Members**.</span></span>

   ![Otevření okna členy](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. <span data-ttu-id="ef3da-114">Chcete-li přidat členy do skupiny, na **skupiny - Členové** vyberte **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="ef3da-114">To add members to the group, on the **Group - Members** blade, select **Add Members**.</span></span>

   ![Přidat členy – příkaz](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. <span data-ttu-id="ef3da-116">Na **členy** okně, vyberte jeden nebo více uživatelů nebo zařízení přidáte do skupiny a vyberte **vyberte** tlačítko v dolní části okna je přidejte do skupiny.</span><span class="sxs-lookup"><span data-stu-id="ef3da-116">On the **Members** blade, select one or more users or devices to add to the group and select the **Select** button at the bottom of the blade to add them to the group.</span></span> <span data-ttu-id="ef3da-117">**Uživatele** pole filtry zobrazení na základě shody zadání vašeho jakékoliv části názvu uživatele nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="ef3da-117">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="ef3da-118">V tomto poli, jsou přijaty žádné zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="ef3da-118">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="ef3da-119">Možnost odebrání členů ze skupiny, na **skupiny - Členové** okně vyberte člena.</span><span class="sxs-lookup"><span data-stu-id="ef3da-119">To remove members from the group, on the **Group - Members** blade, select a member.</span></span>
9. <span data-ttu-id="ef3da-120">Na ***membername*** okně, vyberte **odebrat** příkazů a potvrďte svou volbu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ef3da-120">On the ***membername*** blade, select the **Remove** command, and confirm your choice at the prompt.</span></span>

   ![odebrat členy příkaz](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. <span data-ttu-id="ef3da-122">Po dokončení změn členy do skupiny, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ef3da-122">When you finish changing members for the group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="ef3da-123">Další informace</span><span class="sxs-lookup"><span data-stu-id="ef3da-123">Additional information</span></span>
<span data-ttu-id="ef3da-124">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ef3da-124">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="ef3da-125">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="ef3da-125">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="ef3da-126">Vytvoření nové skupiny a přidávání členů</span><span class="sxs-lookup"><span data-stu-id="ef3da-126">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="ef3da-127">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="ef3da-127">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="ef3da-128">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="ef3da-128">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="ef3da-129">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="ef3da-129">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
