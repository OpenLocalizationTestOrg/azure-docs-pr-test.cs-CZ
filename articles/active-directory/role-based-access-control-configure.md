---
title: "Řízení přístupu na základě aaaRole v hello portálu Azure | Microsoft Docs"
description: "Začínáme se správou přístupu pomocí řízení přístupu na základě rolí v hello portálu Azure. Použijte roli přiřazení tooassign oprávnění tooyour prostředky."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b87e00089b0fc93fb212b318330a6f22bfbf59e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-access-tooyour-azure-subscription-resources"></a><span data-ttu-id="64c6e-104">Používat řízení přístupu na základě Role toomanage přístup tooyour předplatného Azure prostředky</span><span class="sxs-lookup"><span data-stu-id="64c6e-104">Use Role-Based Access Control toomanage access tooyour Azure subscription resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="64c6e-105">Správa přístupu podle uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="64c6e-105">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="64c6e-106">Správa přístupu podle prostředku</span><span class="sxs-lookup"><span data-stu-id="64c6e-106">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="64c6e-107">Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu.</span><span class="sxs-lookup"><span data-stu-id="64c6e-107">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure.</span></span> <span data-ttu-id="64c6e-108">Pomocí RBAC, můžete udělit pouze hello množství přístup tito uživatelé si musí tooperform svou práci.</span><span class="sxs-lookup"><span data-stu-id="64c6e-108">Using RBAC, you can grant only hello amount of access that users need tooperform their jobs.</span></span> <span data-ttu-id="64c6e-109">Tento článek vám pomůže provozu s RBAC v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64c6e-109">This article helps you get up and running with RBAC in hello Azure portal.</span></span> <span data-ttu-id="64c6e-110">Další informace o tom, jak vám řízení přístupu na základě role v Azure pomůže spravovat přístup uživatelů najdete v článku [Co je řízení přístupu na základě role](role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="64c6e-110">If you want more details about how RBAC helps you manage access, see [What is Role-Based Access Control](role-based-access-control-what-is.md).</span></span>

<span data-ttu-id="64c6e-111">V rámci každé předplatné můžete udělit too2000 přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="64c6e-111">Within each subscription, you can grant up too2000 role assignments.</span></span> 

## <a name="view-access"></a><span data-ttu-id="64c6e-112">Zobrazení informací o přístupu</span><span class="sxs-lookup"><span data-stu-id="64c6e-112">View access</span></span>
<span data-ttu-id="64c6e-113">Můžete zjistit, kdo má přístup tooa prostředku, skupiny prostředků nebo předplatného v hlavním okně v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="64c6e-113">You can see who has access tooa resource, resource group, or subscription from its main blade in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="64c6e-114">Například chceme toosee, který má přístup tooone ze skupin prostředků:</span><span class="sxs-lookup"><span data-stu-id="64c6e-114">For example, we want toosee who has access tooone of our resource groups:</span></span>

1. <span data-ttu-id="64c6e-115">Vyberte **skupiny prostředků** hello navigačním panelu na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-115">Select **Resource groups** in hello navigation bar on hello left.</span></span>  
    <span data-ttu-id="64c6e-116">![Skupiny prostředků – ikona](./media/role-based-access-control-configure/resourcegroups_icon.png)</span><span class="sxs-lookup"><span data-stu-id="64c6e-116">![Resource groups - icon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span></span>
2. <span data-ttu-id="64c6e-117">Vyberte hello název skupiny prostředků hello z hello **skupiny prostředků** okno.</span><span class="sxs-lookup"><span data-stu-id="64c6e-117">Select hello name of hello resource group from hello **Resource groups** blade.</span></span>
3. <span data-ttu-id="64c6e-118">Vyberte **přístup k ovládacímu prvku (IAM)** hello levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="64c6e-118">Select **Access control (IAM)** from hello left menu.</span></span>  
4. <span data-ttu-id="64c6e-119">okno pro řízení přístupu Hello obsahuje seznam všech uživatelů, skupin a aplikací, kterým byl udělen přístup skupiny prostředků toohello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-119">hello Access control blade lists all users, groups, and applications that have been granted access toohello resource group.</span></span>  
   
    ![Snímek obrazovky s oknem uživatelé – zděděný a přiřazený přístup](./media/role-based-access-control-configure/view-access.png)

<span data-ttu-id="64c6e-121">Všimněte si, že některé role jsou vymezeny příliš**tento prostředek** zase **zděděné** z jiného oboru.</span><span class="sxs-lookup"><span data-stu-id="64c6e-121">Notice that some roles are scoped too**This resource** while others are **Inherited** it from another scope.</span></span> <span data-ttu-id="64c6e-122">Přístup je přiřazen konkrétně toohello skupinu prostředků nebo zděděná z nadřazené předplatného toohello přiřazení.</span><span class="sxs-lookup"><span data-stu-id="64c6e-122">Access is either assigned specifically toohello resource group or inherited from an assignment toohello parent subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="64c6e-123">Classic správci a pomocní správci jsou považovány za vlastníky hello předplatného v novém modelu RBAC hello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-123">Classic subscription admins and co-admins are considered owners of hello subscription in hello new RBAC model.</span></span>

## <a name="add-access"></a><span data-ttu-id="64c6e-124">Přidání přístupu</span><span class="sxs-lookup"><span data-stu-id="64c6e-124">Add Access</span></span>
<span data-ttu-id="64c6e-125">Udělíte přístup z v rámci hello prostředku, skupině prostředků nebo předplatného, které je hello rozsah přiřazení role hello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-125">You grant access from within hello resource, resource group, or subscription that is hello scope of hello role assignment.</span></span>

1. <span data-ttu-id="64c6e-126">Vyberte **přidat** v okně řízení přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-126">Select **Add** on hello Access control blade.</span></span>  
2. <span data-ttu-id="64c6e-127">Vyberte hello role, které chcete tooassign z hello **vyberte roli** okno.</span><span class="sxs-lookup"><span data-stu-id="64c6e-127">Select hello role that you wish tooassign from hello **Select a role** blade.</span></span>
3. <span data-ttu-id="64c6e-128">Vyberte v adresáři, který chcete toogrant přístup k hello uživatele, skupiny nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="64c6e-128">Select hello user, group, or application in your directory that you wish toogrant access to.</span></span> <span data-ttu-id="64c6e-129">Můžete hledat hello adresář se zobrazovaných názvů, e-mailové adresy a identifikátory objektů.</span><span class="sxs-lookup"><span data-stu-id="64c6e-129">You can search hello directory with display names, email addresses, and object identifiers.</span></span>  
   
    ![Snímek obrazovky s oknem Přidat uživatele – vyhledávání](./media/role-based-access-control-configure/grant-access2.png)
4. <span data-ttu-id="64c6e-131">Vyberte **OK** toocreate hello přiřazení.</span><span class="sxs-lookup"><span data-stu-id="64c6e-131">Select **OK** toocreate hello assignment.</span></span> <span data-ttu-id="64c6e-132">Hello **přidává se uživatel** otevřeném hello průběh.</span><span class="sxs-lookup"><span data-stu-id="64c6e-132">hello **Adding user** popup tracks hello progress.</span></span>  
    <span data-ttu-id="64c6e-133">![Ukazatel průběhu přidávání uživatele – snímek obrazovky](./media/role-based-access-control-configure/addinguser_popup.png)</span><span class="sxs-lookup"><span data-stu-id="64c6e-133">![Adding user progress bar - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)</span></span>

<span data-ttu-id="64c6e-134">Po úspěšném přiřazení role, zobrazí se v hello **uživatelé** okno.</span><span class="sxs-lookup"><span data-stu-id="64c6e-134">After successfully adding a role assignment, it will appear on hello **Users** blade.</span></span>

## <a name="remove-access"></a><span data-ttu-id="64c6e-135">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="64c6e-135">Remove Access</span></span>
1. <span data-ttu-id="64c6e-136">Ukazatele myši hello název hello přiřazení, které chcete tooremove.</span><span class="sxs-lookup"><span data-stu-id="64c6e-136">Hover your cursor over hello name of hello assignment that you want tooremove.</span></span> <span data-ttu-id="64c6e-137">Zaškrtávací políčko se zobrazí další toohello název.</span><span class="sxs-lookup"><span data-stu-id="64c6e-137">A check box appears next toohello name.</span></span>
2. <span data-ttu-id="64c6e-138">Pomocí zaškrtávacích políček tooselect hello jeden nebo více přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="64c6e-138">Use hello check boxes tooselect one or more role assignments.</span></span>
2. <span data-ttu-id="64c6e-139">Vyberte **Odebrat**.</span><span class="sxs-lookup"><span data-stu-id="64c6e-139">Select **Remove**.</span></span>  
3. <span data-ttu-id="64c6e-140">Vyberte **Ano** tooconfirm hello odebrání.</span><span class="sxs-lookup"><span data-stu-id="64c6e-140">Select **Yes** tooconfirm hello removal.</span></span>

<span data-ttu-id="64c6e-141">Zděděná přiřazení nelze odebrat.</span><span class="sxs-lookup"><span data-stu-id="64c6e-141">Inherited assignments cannot be removed.</span></span> <span data-ttu-id="64c6e-142">Pokud potřebujete tooremove zděděná přiřazení, je nutné toodo v hello oboru, které bylo vytvořeno hello přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="64c6e-142">If you need tooremove an inherited assignment, you need toodo it at hello scope where hello role assignment was created.</span></span> <span data-ttu-id="64c6e-143">V hello **oboru** sloupce, další příliš**zděděné** je odkaz, který přejdete toohello prostředky kde byl přiřazen této role.</span><span class="sxs-lookup"><span data-stu-id="64c6e-143">In hello **Scope** column, next too**Inherited** there is a link that takes you toohello resources where this role was assigned.</span></span> <span data-ttu-id="64c6e-144">Přejděte v seznamu uvedeno prostředků toohello přiřazení role tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-144">Go toohello resource listed there tooremove hello role assignment.</span></span>

![Snímek obrazovky s oknem Uživatelé – neaktivní tlačítko odebrání u zděděného přístupu](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-toomanage-access"></a><span data-ttu-id="64c6e-146">Jiný přístup toomanage nástroje</span><span class="sxs-lookup"><span data-stu-id="64c6e-146">Other tools toomanage access</span></span>
<span data-ttu-id="64c6e-147">Můžete přiřadit role a spravovat přístup pomocí příkazů Azure RBAC v jiných nástrojích než hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64c6e-147">You can assign roles and manage access with Azure RBAC commands in tools other than hello Azure portal.</span></span>  <span data-ttu-id="64c6e-148">Postupujte podle hello odkazy toolearn více informací o hello požadavky a začít pracovat s příkazy Azure RBAC hello.</span><span class="sxs-lookup"><span data-stu-id="64c6e-148">Follow hello links toolearn more about hello prerequisites and get started with hello Azure RBAC commands.</span></span>

* [<span data-ttu-id="64c6e-149">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="64c6e-149">Azure PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
* [<span data-ttu-id="64c6e-150">Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="64c6e-150">Azure Command-Line Interface</span></span>](role-based-access-control-manage-access-azure-cli.md)
* [<span data-ttu-id="64c6e-151">REST API</span><span class="sxs-lookup"><span data-stu-id="64c6e-151">REST API</span></span>](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a><span data-ttu-id="64c6e-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64c6e-152">Next Steps</span></span>
* [<span data-ttu-id="64c6e-153">Vytvoření sestavy historie změn přístupu</span><span class="sxs-lookup"><span data-stu-id="64c6e-153">Create an access change history report</span></span>](role-based-access-control-access-change-history-report.md)
* <span data-ttu-id="64c6e-154">V tématu hello [předdefinované role RBAC](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="64c6e-154">See hello [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>
* <span data-ttu-id="64c6e-155">Definujte své [Vlastní role funkce řízení přístupu na základě role v Azure](role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="64c6e-155">Define your own [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>

