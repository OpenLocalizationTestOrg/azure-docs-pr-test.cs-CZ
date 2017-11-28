---
title: "aaaCreate skupinu pro uživatele v Azure Active Directory | Microsoft Docs"
description: "Jak toocreate skupinu v Azure Active Directory a přidání členů skupiny toohello"
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
ms.openlocfilehash: fc583a7f02ce50e7f3b2c8f97a9c032a3e2dc33a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a><span data-ttu-id="48c1e-103">Vytvořte skupinu a přidejte členy v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48c1e-103">Create a group and add members in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48c1e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48c1e-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="48c1e-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="48c1e-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="48c1e-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="48c1e-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="48c1e-107">Tento článek vysvětluje, jak toocreate a naplnit nové skupiny v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="48c1e-107">This article explains how toocreate and populate a new group in Azure Active Directory.</span></span> <span data-ttu-id="48c1e-108">Použití úlohy správy skupiny tooperform tak, jako je například přiřazení licence nebo oprávnění tooa počtu uživatelů nebo zařízení najednou.</span><span class="sxs-lookup"><span data-stu-id="48c1e-108">Use a group tooperform management tasks such as assigning licenses or permissions tooa number of users or devices at once.</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="48c1e-109">Vytvoření skupiny</span><span class="sxs-lookup"><span data-stu-id="48c1e-109">How do I create a group?</span></span>
1. <span data-ttu-id="48c1e-110">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="48c1e-110">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="48c1e-111">Vyberte **další služby**, zadejte **uživatele a skupiny** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="48c1e-111">Select **More services**, enter **User and groups** in hello text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. <span data-ttu-id="48c1e-113">Na hello **uživatelů a skupin** vyberte **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="48c1e-113">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Okno skupiny otevírání hello](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="48c1e-115">Na hello **uživatelé a skupiny - všechny skupiny** okně, vyberte hello **přidat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="48c1e-115">On hello **Users and groups - All groups** blade, select hello **Add** command.</span></span>

   ![Výběrem příkazu přidat hello](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. <span data-ttu-id="48c1e-117">Na hello **skupiny** okně Přidat název a popis skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="48c1e-117">On hello **Group** blade, add a name and description for hello group.</span></span>
6. <span data-ttu-id="48c1e-118">tooselect členy tooadd toohello skupiny, vyberte **přiřazeno** v hello **typ členství** a pak vyberte **členy**.</span><span class="sxs-lookup"><span data-stu-id="48c1e-118">tooselect members tooadd toohello group, select **Assigned** in hello **Membership type** box, and then select **Members**.</span></span> <span data-ttu-id="48c1e-119">Další informace o tom, jak toomanage dynamicky hello členství ve skupině najdete v tématu [pomocí atributů toocreate advanced pravidla pro členství ve skupině](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="48c1e-119">For more information about how toomanage hello membership of a group dynamically, see [Using attributes toocreate advanced rules for group membership](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

   ![Výběr členů tooadd](./media/active-directory-groups-create-azure-portal/select-members.png)
7. <span data-ttu-id="48c1e-121">Na hello **členy** okně, vyberte jednu nebo více uživatelů nebo zařízení tooadd toohello skupinu a vyberte hello **vyberte** tlačítko hello dolní části okna tooadd hello je toohello skupiny.</span><span class="sxs-lookup"><span data-stu-id="48c1e-121">On hello **Members** blade, select one or more users or devices tooadd toohello group and select hello **Select** button at hello bottom of hello blade tooadd them toohello group.</span></span> <span data-ttu-id="48c1e-122">Hello **uživatele** pole filtry hello zobrazení na základě shody se vaše vstupní tooany součástí názvu uživatele nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="48c1e-122">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="48c1e-123">V tomto poli, jsou přijaty žádné zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="48c1e-123">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="48c1e-124">Po dokončení přidávání členů toohello skupiny, vyberte **vytvořit** na hello **skupiny** okno.</span><span class="sxs-lookup"><span data-stu-id="48c1e-124">When you finish adding members toohello group, select **Create** on hello **Group** blade.</span></span>    

   ![Vytvoření skupiny potvrzení](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a><span data-ttu-id="48c1e-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48c1e-126">Next steps</span></span>
<span data-ttu-id="48c1e-127">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="48c1e-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="48c1e-128">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="48c1e-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="48c1e-129">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="48c1e-129">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="48c1e-130">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="48c1e-130">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="48c1e-131">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="48c1e-131">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="48c1e-132">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="48c1e-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
