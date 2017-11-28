---
title: "přiřazení přístupu prostředků Azure aaaView | Microsoft Docs"
description: "Zobrazit a spravovat všechna přiřazení hello řízení přístupu na základě Role u uživatele nebo skupiny v hello portálu Azure"
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
ms.openlocfilehash: ec96c9d4b9e2cb4925949b8bac78767bd564c8ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-hello-azure-portal"></a><span data-ttu-id="3f5e8-103">Zobrazení přiřazení přístupu pro uživatele a skupiny v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3f5e8-103">View access assignments for users and groups in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f5e8-104">Správa přístupu podle uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="3f5e8-104">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="3f5e8-105">Správa přístupu podle prostředku</span><span class="sxs-lookup"><span data-stu-id="3f5e8-105">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="3f5e8-106">Pomocí řízení přístupu na základě rolí (RBAC) v hello Azure Active Directory (Azure AD), můžete spravovat přístup tooyour Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-106">With role-based access control (RBAC) in hello Azure Active Directory (Azure AD), you can manage access tooyour Azure resources.</span></span> 

<span data-ttu-id="3f5e8-107">Přístup přiřazeny RBAC je podrobných, protože můžete omezit oprávnění hello dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="3f5e8-107">Access assigned with RBAC is fine-grained because there are two ways you can restrict hello permissions:</span></span>

* <span data-ttu-id="3f5e8-108">**Obor:** přiřazení rolí pro RBAC jsou vymezená tooa konkrétní předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-108">**Scope:** RBAC role assignments are scoped tooa specific subscription, resource group, or resource.</span></span> <span data-ttu-id="3f5e8-109">Uživatel zadaný přístup tooa jediný zdroj nelze přístup k jiným prostředkům v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-109">A user given access tooa single resource cannot access any other resources in hello same subscription.</span></span>
* <span data-ttu-id="3f5e8-110">**Role:** v rámci oboru hello hello přiřazení, je přístup zúžit ještě i přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-110">**Role:** Within hello scope of hello assignment, access is narrowed even further by assigning a role.</span></span> <span data-ttu-id="3f5e8-111">Role může být vysoké úrovně, jako je vlastník, nebo konkrétní, jako je virtuální počítač čtečky.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-111">Roles can be high-level, like owner, or specific, like virtual machine reader.</span></span>

<span data-ttu-id="3f5e8-112">Role můžete přiřadit pouze z v rámci hello předplatné, skupinu prostředků nebo prostředek, který je hello oboru pro přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-112">Roles can only be assigned from within hello subscription, resource group, or resource that is hello scope for hello assignment.</span></span> <span data-ttu-id="3f5e8-113">Ale můžete zobrazit všechna přiřazení hello přístup pro daného uživatele nebo skupinu na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-113">But you can view all hello access assignments for a given user or group in a single place.</span></span> <span data-ttu-id="3f5e8-114">Může obsahovat maximálně too2000 přiřazení rolí v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-114">You can have up too2000 role assignments in each subscription.</span></span> 

<span data-ttu-id="3f5e8-115">Získat další informace o příliš[používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="3f5e8-115">Get more information about how too[Use role assignments toomanage access tooyour Azure subscription resources](role-based-access-control-configure.md).</span></span>

## <a name="view-access-assignments"></a><span data-ttu-id="3f5e8-116">Přiřazení přístupu zobrazení</span><span class="sxs-lookup"><span data-stu-id="3f5e8-116">View access assignments</span></span>
<span data-ttu-id="3f5e8-117">toolook až hello přiřazení přístupu pro jednoho uživatele nebo skupinu, spustit v Azure Active Directory v hello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f5e8-117">toolook up hello access assignments for a single user or group, start in Azure Active Directory in hello [Azure portal](http://portal.azure.com).</span></span>

1. <span data-ttu-id="3f5e8-118">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-118">Select **Azure Active Directory**.</span></span> <span data-ttu-id="3f5e8-119">Pokud tato možnost není zobrazena v seznamu navigace, vyberte **více služeb** a posuňte se dolů toofind **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-119">If this option is not visible on your navigation list, select **More Services** and then scroll down toofind **Azure Active Directory**.</span></span>
2. <span data-ttu-id="3f5e8-120">Vyberte **uživatelů a skupin**a potom buď **všichni uživatelé** nebo **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-120">Select **Users and Groups**, and then either **All users** or **All groups**.</span></span> <span data-ttu-id="3f5e8-121">V tomto příkladu zaměříme na jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-121">For this example, we focus on individual users.</span></span>
    <span data-ttu-id="3f5e8-122">![Správa uživatelů a skupin v Azure Active Directory – snímek obrazovky](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span><span class="sxs-lookup"><span data-stu-id="3f5e8-122">![Manage users and groups in Azure Active Directory - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span></span>
3. <span data-ttu-id="3f5e8-123">Hledat hello uživatelské jméno nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-123">Search for hello user by name or username.</span></span>
4. <span data-ttu-id="3f5e8-124">Vyberte **prostředky Azure** v okně hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-124">Select **Azure resources** on hello user blade.</span></span> <span data-ttu-id="3f5e8-125">Zobrazí všechna přiřazení hello přístup pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-125">All hello access assignments for that user appear.</span></span>

### <a name="read-permissions-tooview-assignments"></a><span data-ttu-id="3f5e8-126">Oprávnění ke čtení tooview přiřazení</span><span class="sxs-lookup"><span data-stu-id="3f5e8-126">Read permissions tooview assignments</span></span>
<span data-ttu-id="3f5e8-127">Tato stránka zobrazuje pouze přiřazení přístupu hello, zda máte oprávnění tooread.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-127">This page only shows hello access assignments that you have permission tooread.</span></span> <span data-ttu-id="3f5e8-128">Například máte toosubscription přístup pro čtení A a přejděte toohello prostředky Azure okno toocheck přiřazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-128">For example, you have read access toosubscription A and go toohello Azure resources blade toocheck a user's assignments.</span></span> <span data-ttu-id="3f5e8-129">Můžete zobrazit jeho přiřazení přístupu pro předplatné A, ale nemůžete zobrazit také, že uživatel je přiřazení přístupu na základě předplatného B.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-129">You can see her access assignments for subscription A, but can't see that she also has access assignments on subscription B.</span></span>

## <a name="delete-access-assignments"></a><span data-ttu-id="3f5e8-130">Odstranit přiřazení přístupu</span><span class="sxs-lookup"><span data-stu-id="3f5e8-130">Delete access assignments</span></span>
<span data-ttu-id="3f5e8-131">V tomto okně můžete odstranit přiřazení přístupu, které byly přímo přiřazeny tooa uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-131">From this blade, you can delete access assignments that were assigned directly tooa user or group.</span></span> <span data-ttu-id="3f5e8-132">Pokud přiřazení přístupu hello byla zděděna od nadřazené skupiny, můžete potřebovat toogo toohello prostředků nebo předplatného a spravovat hello přiřazení došlo.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-132">If hello access assignment was inherited from a parent group, you need toogo toohello resource or subscription and manage hello assignment there.</span></span>

1. <span data-ttu-id="3f5e8-133">Hello seznamu všech hello přiřazení přístupu pro uživatele nebo skupinu vyberte hello jeden chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-133">From hello list of all hello access assignments for a user or group, select hello one you want toodelete.</span></span>
2. <span data-ttu-id="3f5e8-134">Vyberte **odebrat** a potom **Ano** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="3f5e8-134">Select **Remove** and then **Yes** tooconfirm.</span></span>
    <span data-ttu-id="3f5e8-135">![Odebrat přiřazení přístupu – snímek obrazovky](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span><span class="sxs-lookup"><span data-stu-id="3f5e8-135">![Remove access assignment - screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f5e8-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f5e8-136">Next steps</span></span>

* <span data-ttu-id="3f5e8-137">Začínáme s řízením přístupu na základě rolí příliš[používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="3f5e8-137">Get started with Role-Based Access Control too[Use role assignments toomanage access tooyour Azure subscription resources](role-based-access-control-configure.md)</span></span>
* <span data-ttu-id="3f5e8-138">V tématu hello [předdefinované role RBAC](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="3f5e8-138">See hello [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>

