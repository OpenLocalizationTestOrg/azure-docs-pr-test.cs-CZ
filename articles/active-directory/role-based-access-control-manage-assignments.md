---
title: "Zobrazení prostředků Azure přístup k přiřazení | Microsoft Docs"
description: "Zobrazit a spravovat všechna přiřazení na základě rolí řízení přístupu pro všechny uživatele nebo skupinu na portálu Azure"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: 72c695d08bdf5de003d51ffb0768184e1e4d00ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal"></a><span data-ttu-id="574d3-103">Zobrazení přiřazení přístupu pro uživatele a skupiny na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="574d3-103">View access assignments for users and groups in the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="574d3-104">Správa přístupu podle uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="574d3-104">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="574d3-105">Správa přístupu podle prostředku</span><span class="sxs-lookup"><span data-stu-id="574d3-105">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="574d3-106">Pomocí řízení přístupu na základě rolí (RBAC) ve službě Azure Active Directory (Azure AD) můžete spravovat přístup k prostředkům Azure.</span><span class="sxs-lookup"><span data-stu-id="574d3-106">With role-based access control (RBAC) in the Azure Active Directory (Azure AD), you can manage access to your Azure resources.</span></span> 

<span data-ttu-id="574d3-107">Přístup přiřazeny RBAC je podrobných, protože existují dva způsoby, můžete omezit oprávnění:</span><span class="sxs-lookup"><span data-stu-id="574d3-107">Access assigned with RBAC is fine-grained because there are two ways you can restrict the permissions:</span></span>

* <span data-ttu-id="574d3-108">**Obor:** přiřazení rolí pro RBAC jsou omezená na konkrétní předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="574d3-108">**Scope:** RBAC role assignments are scoped to a specific subscription, resource group, or resource.</span></span> <span data-ttu-id="574d3-109">Uživatel poskytnut přístup k jedinému prostředku nemůže získat přístup k žádné další prostředky ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="574d3-109">A user given access to a single resource cannot access any other resources in the same subscription.</span></span>
* <span data-ttu-id="574d3-110">**Role:** v rámci oboru přiřazení přístupu zúžit ještě i přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="574d3-110">**Role:** Within the scope of the assignment, access is narrowed even further by assigning a role.</span></span> <span data-ttu-id="574d3-111">Role může být vysoké úrovně, jako je vlastník, nebo konkrétní, jako je virtuální počítač čtečky.</span><span class="sxs-lookup"><span data-stu-id="574d3-111">Roles can be high-level, like owner, or specific, like virtual machine reader.</span></span>

<span data-ttu-id="574d3-112">Role můžete přiřadit pouze z v rámci předplatné, skupinu prostředků nebo prostředek, který je v rozsahu přiřazení.</span><span class="sxs-lookup"><span data-stu-id="574d3-112">Roles can only be assigned from within the subscription, resource group, or resource that is the scope for the assignment.</span></span> <span data-ttu-id="574d3-113">Ale můžete zobrazit všechna přiřazení přístupu pro daného uživatele nebo skupinu na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="574d3-113">But you can view all the access assignments for a given user or group in a single place.</span></span> <span data-ttu-id="574d3-114">Můžete mít až 2000 přiřazení rolí v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="574d3-114">You can have up to 2000 role assignments in each subscription.</span></span> 

<span data-ttu-id="574d3-115">Další informace o tom, jak [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="574d3-115">Get more information about how to [Use role assignments to manage access to your Azure subscription resources](role-based-access-control-configure.md).</span></span>

## <a name="view-access-assignments"></a><span data-ttu-id="574d3-116">Přiřazení přístupu zobrazení</span><span class="sxs-lookup"><span data-stu-id="574d3-116">View access assignments</span></span>
<span data-ttu-id="574d3-117">K vyhledání přiřazení přístupu pro jednoho uživatele nebo skupinu, spustit v Azure Active Directory v [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="574d3-117">To look up the access assignments for a single user or group, start in Azure Active Directory in the [Azure portal](http://portal.azure.com).</span></span>

1. <span data-ttu-id="574d3-118">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="574d3-118">Select **Azure Active Directory**.</span></span> <span data-ttu-id="574d3-119">Pokud tato možnost není zobrazena v seznamu navigace, vyberte **více služeb** a posuňte se dolů najít **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="574d3-119">If this option is not visible on your navigation list, select **More Services** and then scroll down to find **Azure Active Directory**.</span></span>
2. <span data-ttu-id="574d3-120">Vyberte **uživatelů a skupin**a potom buď **všichni uživatelé** nebo **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="574d3-120">Select **Users and Groups**, and then either **All users** or **All groups**.</span></span> <span data-ttu-id="574d3-121">V tomto příkladu zaměříme na jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="574d3-121">For this example, we focus on individual users.</span></span>
    <span data-ttu-id="574d3-122">![Správa uživatelů a skupin v Azure Active Directory – snímek obrazovky](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span><span class="sxs-lookup"><span data-stu-id="574d3-122">![Manage users and groups in Azure Active Directory - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span></span>
3. <span data-ttu-id="574d3-123">Vyhledat uživatele podle názvu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="574d3-123">Search for the user by name or username.</span></span>
4. <span data-ttu-id="574d3-124">V okně uživatele vyberte **Prostředky Azure**.</span><span class="sxs-lookup"><span data-stu-id="574d3-124">Select **Azure resources** on the user blade.</span></span> <span data-ttu-id="574d3-125">Zobrazí všechna přiřazení přístupu pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="574d3-125">All the access assignments for that user appear.</span></span>

### <a name="read-permissions-to-view-assignments"></a><span data-ttu-id="574d3-126">Oprávnění ke čtení pro zobrazení přiřazení</span><span class="sxs-lookup"><span data-stu-id="574d3-126">Read permissions to view assignments</span></span>
<span data-ttu-id="574d3-127">Tato stránka se zobrazí pouze přiřazení přístupu, které máte oprávnění ke čtení.</span><span class="sxs-lookup"><span data-stu-id="574d3-127">This page only shows the access assignments that you have permission to read.</span></span> <span data-ttu-id="574d3-128">Například oprávnění ke čtení pro předplatné A a přejděte do okna prostředků Azure zkontrolujte přiřazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="574d3-128">For example, you have read access to subscription A and go to the Azure resources blade to check a user's assignments.</span></span> <span data-ttu-id="574d3-129">Můžete zobrazit jeho přiřazení přístupu pro předplatné A, ale nemůžete zobrazit také, že uživatel je přiřazení přístupu na základě předplatného B.</span><span class="sxs-lookup"><span data-stu-id="574d3-129">You can see her access assignments for subscription A, but can't see that she also has access assignments on subscription B.</span></span>

## <a name="delete-access-assignments"></a><span data-ttu-id="574d3-130">Odstranit přiřazení přístupu</span><span class="sxs-lookup"><span data-stu-id="574d3-130">Delete access assignments</span></span>
<span data-ttu-id="574d3-131">V tomto okně můžete odstranit přiřazení přístupu, které byly přiřazeny přímo na uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="574d3-131">From this blade, you can delete access assignments that were assigned directly to a user or group.</span></span> <span data-ttu-id="574d3-132">Pokud přístup přiřazení byla zděděna od nadřazené skupiny, budete muset přejít do prostředků nebo předplatného a spravovat přiřazení existuje.</span><span class="sxs-lookup"><span data-stu-id="574d3-132">If the access assignment was inherited from a parent group, you need to go to the resource or subscription and manage the assignment there.</span></span>

1. <span data-ttu-id="574d3-133">Seznam všech přiřazení přístupu pro uživatele nebo skupinu vyberte ten, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="574d3-133">From the list of all the access assignments for a user or group, select the one you want to delete.</span></span>
2. <span data-ttu-id="574d3-134">Vyberte **odebrat** a potom **Ano** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="574d3-134">Select **Remove** and then **Yes** to confirm.</span></span>
    <span data-ttu-id="574d3-135">![Odebrat přiřazení přístupu – snímek obrazovky](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span><span class="sxs-lookup"><span data-stu-id="574d3-135">![Remove access assignment - screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="574d3-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="574d3-136">Next steps</span></span>

* <span data-ttu-id="574d3-137">Začínáme s řízením přístupu na základě rolí k [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="574d3-137">Get started with Role-Based Access Control to [Use role assignments to manage access to your Azure subscription resources](role-based-access-control-configure.md)</span></span>
* <span data-ttu-id="574d3-138">Další informace najdete v článku [Vestavěné role řízení přístupu na základě role v Azure](role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="574d3-138">See the [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>

