---
title: "Vytvoření skupiny pro uživatele v Azure Active Directory | Microsoft Docs"
description: "Postup vytvoření skupiny ve službě Azure Active Directory a přidání členů do skupiny"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d3d37761a9fdf9bd9801396d45f2fcd47efb0be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a><span data-ttu-id="de23f-103">Vytvořte skupinu a přidejte členy v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="de23f-103">Create a group and add members in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="de23f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="de23f-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="de23f-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="de23f-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="de23f-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="de23f-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="de23f-107">Tento článek vysvětluje, jak vytvořit a naplnit nové skupiny v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="de23f-107">This article explains how to create and populate a new group in Azure Active Directory.</span></span> <span data-ttu-id="de23f-108">Použijte skupinu k provádění úloh správy, jako je například přiřazení licence nebo oprávnění k počtu uživatelů nebo zařízení najednou.</span><span class="sxs-lookup"><span data-stu-id="de23f-108">Use a group to perform management tasks such as assigning licenses or permissions to a number of users or devices at once.</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="de23f-109">Vytvoření skupiny</span><span class="sxs-lookup"><span data-stu-id="de23f-109">How do I create a group?</span></span>
1. <span data-ttu-id="de23f-110">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="de23f-110">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="de23f-111">Vyberte **další služby**, zadejte **uživatele a skupiny** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="de23f-111">Select **More services**, enter **User and groups** in the text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. <span data-ttu-id="de23f-113">Na **uživatelů a skupin** vyberte **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="de23f-113">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Otevření okna skupiny](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="de23f-115">Na **uživatelé a skupiny - všechny skupiny** okně, vyberte **přidat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="de23f-115">On the **Users and groups - All groups** blade, select the **Add** command.</span></span>

   ![Použití příkazu Přidat](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. <span data-ttu-id="de23f-117">Na **skupiny** okně Přidat název a popis skupiny.</span><span class="sxs-lookup"><span data-stu-id="de23f-117">On the **Group** blade, add a name and description for the group.</span></span>
6. <span data-ttu-id="de23f-118">Vyberte členy, které chcete přidat do skupiny, vyberte **přiřazeno** v **typ členství** a pak vyberte **členy**.</span><span class="sxs-lookup"><span data-stu-id="de23f-118">To select members to add to the group, select **Assigned** in the **Membership type** box, and then select **Members**.</span></span> <span data-ttu-id="de23f-119">Další informace o tom, jak dynamicky spravovat členství ve skupině najdete v tématu [vytváření rozšířených pravidel pro členství ve skupině pomocí atributů](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="de23f-119">For more information about how to manage the membership of a group dynamically, see [Using attributes to create advanced rules for group membership](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

   ![Výběr členů pro přidání](./media/active-directory-groups-create-azure-portal/select-members.png)
7. <span data-ttu-id="de23f-121">Na **členy** okně, vyberte jeden nebo více uživatelů nebo zařízení přidáte do skupiny a vyberte **vyberte** tlačítko v dolní části okna je přidejte do skupiny.</span><span class="sxs-lookup"><span data-stu-id="de23f-121">On the **Members** blade, select one or more users or devices to add to the group and select the **Select** button at the bottom of the blade to add them to the group.</span></span> <span data-ttu-id="de23f-122">**Uživatele** pole filtry zobrazení na základě shody zadání vašeho jakékoliv části názvu uživatele nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="de23f-122">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="de23f-123">V tomto poli, jsou přijaty žádné zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="de23f-123">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="de23f-124">Po dokončení přidávání členů do skupiny, vyberte **vytvořit** na **skupiny** okno.</span><span class="sxs-lookup"><span data-stu-id="de23f-124">When you finish adding members to the group, select **Create** on the **Group** blade.</span></span>    

   ![Vytvoření skupiny potvrzení](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a><span data-ttu-id="de23f-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de23f-126">Next steps</span></span>
<span data-ttu-id="de23f-127">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="de23f-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="de23f-128">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="de23f-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="de23f-129">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="de23f-129">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="de23f-130">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="de23f-130">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="de23f-131">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="de23f-131">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="de23f-132">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="de23f-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
