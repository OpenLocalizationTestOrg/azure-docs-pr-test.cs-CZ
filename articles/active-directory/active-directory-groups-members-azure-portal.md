---
title: "aaaManage hello členy skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Jak tooadd nebo odebrat uživatele a zařízení ze skupiny ve službě Azure Active Directory"
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
ms.openlocfilehash: 4cb16ee63828003da251423a04736f7174dd4896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a><span data-ttu-id="fd99f-103">Správa členství ve skupině pro uživatele v klientovi služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd99f-103">Manage group membership for users in your Azure Active Directory tenant</span></span>
<span data-ttu-id="fd99f-104">Tento článek vysvětluje, jak toomanage hello členy skupiny ve službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fd99f-104">This article explains how toomanage hello members for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-hello-members-and-manage-them"></a><span data-ttu-id="fd99f-105">Jak najít hello členy a spravovat?</span><span class="sxs-lookup"><span data-stu-id="fd99f-105">How do I find hello members and manage them?</span></span>
1. <span data-ttu-id="fd99f-106">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="fd99f-106">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="fd99f-107">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="fd99f-107">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. <span data-ttu-id="fd99f-109">Na hello **uživatelů a skupin** vyberte **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="fd99f-109">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Okno skupiny otevírání hello](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="fd99f-111">Na hello **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="fd99f-111">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="fd99f-112">Na hello **skupiny - *groupname***  vyberte **členy**.</span><span class="sxs-lookup"><span data-stu-id="fd99f-112">On hello **Group - *groupname*** blade, select **Members**.</span></span>

   ![Otevírání hello členy okno](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. <span data-ttu-id="fd99f-114">tooadd členy toohello skupiny, na hello **skupiny - Členové** vyberte **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="fd99f-114">tooadd members toohello group, on hello **Group - Members** blade, select **Add Members**.</span></span>

   ![Přidat členy – příkaz](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. <span data-ttu-id="fd99f-116">Na hello **členy** okně, vyberte jednu nebo více uživatelů nebo zařízení tooadd toohello skupinu a vyberte hello **vyberte** tlačítko hello dolní části okna tooadd hello je toohello skupiny.</span><span class="sxs-lookup"><span data-stu-id="fd99f-116">On hello **Members** blade, select one or more users or devices tooadd toohello group and select hello **Select** button at hello bottom of hello blade tooadd them toohello group.</span></span> <span data-ttu-id="fd99f-117">Hello **uživatele** pole filtry hello zobrazení na základě shody se vaše vstupní tooany součástí názvu uživatele nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="fd99f-117">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="fd99f-118">V tomto poli, jsou přijaty žádné zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="fd99f-118">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="fd99f-119">tooremove členy ze skupiny hello na hello **skupiny - Členové** okně vyberte člena.</span><span class="sxs-lookup"><span data-stu-id="fd99f-119">tooremove members from hello group, on hello **Group - Members** blade, select a member.</span></span>
9. <span data-ttu-id="fd99f-120">Na hello ***membername*** okně, vyberte hello **odebrat** příkazů a potvrďte svou volbu příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="fd99f-120">On hello ***membername*** blade, select hello **Remove** command, and confirm your choice at hello prompt.</span></span>

   ![odebrat členy příkaz](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. <span data-ttu-id="fd99f-122">Po dokončení změn členy hello skupiny, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fd99f-122">When you finish changing members for hello group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="fd99f-123">Další informace</span><span class="sxs-lookup"><span data-stu-id="fd99f-123">Additional information</span></span>
<span data-ttu-id="fd99f-124">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fd99f-124">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="fd99f-125">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="fd99f-125">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="fd99f-126">Vytvoření nové skupiny a přidávání členů</span><span class="sxs-lookup"><span data-stu-id="fd99f-126">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="fd99f-127">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="fd99f-127">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="fd99f-128">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="fd99f-128">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="fd99f-129">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="fd99f-129">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
