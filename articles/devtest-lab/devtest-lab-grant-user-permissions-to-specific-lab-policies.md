---
title: "Udělení uživatelských oprávnění k zásad konkrétní testovacího | Microsoft Docs"
description: "Postup udělení uživatelských oprávnění k zásady konkrétní testovacího prostředí v DevTest Labs podle potřeb jednotlivých uživatelů"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 0bd9f83257834d9681479ba9117c48ffd6d6e166
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a><span data-ttu-id="30a41-103">Udělení uživatelských oprávnění zásad konkrétní testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="30a41-103">Grant user permissions to specific lab policies</span></span>
## <a name="overview"></a><span data-ttu-id="30a41-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="30a41-104">Overview</span></span>
<span data-ttu-id="30a41-105">Tento článek ukazuje, jak pomocí prostředí PowerShell k udělení oprávnění uživatelům zásadu konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="30a41-105">This article illustrates how to use PowerShell to grant users permissions to a particular lab policy.</span></span> <span data-ttu-id="30a41-106">Toto oprávnění můžete být použito podle potřeb jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="30a41-106">That way, permissions can be applied based on each user's needs.</span></span> <span data-ttu-id="30a41-107">Například můžete chtít udělit možnost měnit nastavení zásad virtuálních počítačů, ale není zásady náklady na konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="30a41-107">For example, you might want to grant a particular user the ability to change the VM policy settings, but not the cost policies.</span></span>

## <a name="policies-as-resources"></a><span data-ttu-id="30a41-108">Zásady jako prostředky</span><span class="sxs-lookup"><span data-stu-id="30a41-108">Policies as resources</span></span>
<span data-ttu-id="30a41-109">Jak je popsáno v [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md) článku RBAC umožňuje přesnou správu přístupu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="30a41-109">As discussed in the [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) article, RBAC enables fine-grained access management of resources for Azure.</span></span> <span data-ttu-id="30a41-110">RBAC můžete oddělit povinností v rámci týmu DevOps a poskytnout pouze takovou úroveň přístupu pro uživatele, kteří potřebují k provádění svých úloh.</span><span class="sxs-lookup"><span data-stu-id="30a41-110">Using RBAC, you can segregate duties within your DevOps team and grant only the amount of access to users that they need to perform their jobs.</span></span>

<span data-ttu-id="30a41-111">V DevTest Labs zásady je typ prostředku, který umožňuje akci RBAC **Microsoft.DevTestLab/labs/policySets/policies/**.</span><span class="sxs-lookup"><span data-stu-id="30a41-111">In DevTest Labs, a policy is a resource type that enables the RBAC action **Microsoft.DevTestLab/labs/policySets/policies/**.</span></span> <span data-ttu-id="30a41-112">Každá zásada testovacího prostředí je prostředek v typ prostředku zásad a můžete přiřadit jako obor do RBAC role.</span><span class="sxs-lookup"><span data-stu-id="30a41-112">Each lab policy is a resource in the Policy resource type, and can be assigned as a scope to an RBAC role.</span></span>

<span data-ttu-id="30a41-113">Třeba, aby bylo možné udělit oprávnění pro čtení a zápis uživatele **povolené velikosti virtuálních počítačů** zásady, by vytvořit vlastní roli, která funguje s **Microsoft.DevTestLab/labs/policySets/policies/*** Akce a pak mu přiřaďte příslušné uživatele pro tuto vlastní roli, v rámci oboru **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span><span class="sxs-lookup"><span data-stu-id="30a41-113">For example, in order to grant users read/write permission to the **Allowed VM Sizes** policy, you would create a custom role that works with the **Microsoft.DevTestLab/labs/policySets/policies/*** action, and then assign the appropriate users to this custom role in the scope of **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span></span>

<span data-ttu-id="30a41-114">Další informace o vlastních rolích v RBAC najdete v tématu [vlastní role řízení přístupu](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="30a41-114">To learn more about custom roles in RBAC, see the [Custom roles access control](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="creating-a-lab-custom-role-using-powershell"></a><span data-ttu-id="30a41-115">Vytvoření testovacího prostředí vlastní role pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="30a41-115">Creating a lab custom role using PowerShell</span></span>
<span data-ttu-id="30a41-116">Chcete-li začít, budete muset v následujícím článku se dozvíte, jak nainstalovat a nakonfigurovat rutin prostředí Azure PowerShell: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span><span class="sxs-lookup"><span data-stu-id="30a41-116">In order to get started, you’ll need to read the following article, which will explain how to install and configure the Azure PowerShell cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span></span>

<span data-ttu-id="30a41-117">Jakmile jste nastavili rutin prostředí Azure PowerShell, můžete provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="30a41-117">Once you’ve set up the Azure PowerShell cmdlets, you can perform the following tasks:</span></span>

* <span data-ttu-id="30a41-118">Seznam všech operací nebo akce pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="30a41-118">List all the operations/actions for a resource provider</span></span>
* <span data-ttu-id="30a41-119">Seznam akcí v konkrétní roli:</span><span class="sxs-lookup"><span data-stu-id="30a41-119">List actions in a particular role:</span></span>
* <span data-ttu-id="30a41-120">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="30a41-120">Create a custom role</span></span>

<span data-ttu-id="30a41-121">Následující skript prostředí PowerShell ukazuje příklady, jak provádět tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="30a41-121">The following PowerShell script illustrates examples of how to perform these tasks:</span></span>

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a><span data-ttu-id="30a41-122">Přiřazení oprávnění pro uživatele pro konkrétní zásady pomocí vlastní role</span><span class="sxs-lookup"><span data-stu-id="30a41-122">Assigning permissions to a user for a specific policy using custom roles</span></span>
<span data-ttu-id="30a41-123">Jakmile vlastní role, které jste definovali, můžete je přiřadit uživatelům.</span><span class="sxs-lookup"><span data-stu-id="30a41-123">Once you’ve defined your custom roles, you can assign them to users.</span></span> <span data-ttu-id="30a41-124">Chcete-li přiřadit vlastní role pro uživatele, je nutné nejprve získat **ObjectId** představující tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="30a41-124">In order to assign a custom role to a user, you must first obtain the **ObjectId** representing that user.</span></span> <span data-ttu-id="30a41-125">Chcete-li provést, použijte **Get-AzureRmADUser** rutiny.</span><span class="sxs-lookup"><span data-stu-id="30a41-125">To do that, use the **Get-AzureRmADUser** cmdlet.</span></span>

<span data-ttu-id="30a41-126">V následujícím příkladu **ObjectId** z *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 je uživatel.</span><span class="sxs-lookup"><span data-stu-id="30a41-126">In the following example, the **ObjectId** of the *SomeUser* user is 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span></span>

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

<span data-ttu-id="30a41-127">Jakmile máte **ObjectId** pro uživatele a název vlastní roli, lze přiřadit této role uživatele s **New-AzureRmRoleAssignment** rutiny:</span><span class="sxs-lookup"><span data-stu-id="30a41-127">Once you have the **ObjectId** for the user and a custom role name, you can assign that role to the user with the **New-AzureRmRoleAssignment** cmdlet:</span></span>

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

<span data-ttu-id="30a41-128">V předchozím příkladu **AllowedVmSizesInLab** zásada se používá.</span><span class="sxs-lookup"><span data-stu-id="30a41-128">In the previous example, the **AllowedVmSizesInLab** policy is used.</span></span> <span data-ttu-id="30a41-129">Můžete použít některou z následujících zásad:</span><span class="sxs-lookup"><span data-stu-id="30a41-129">You can use any of the following polices:</span></span>

* <span data-ttu-id="30a41-130">MaxVmsAllowedPerUser</span><span class="sxs-lookup"><span data-stu-id="30a41-130">MaxVmsAllowedPerUser</span></span>
* <span data-ttu-id="30a41-131">MaxVmsAllowedPerLab</span><span class="sxs-lookup"><span data-stu-id="30a41-131">MaxVmsAllowedPerLab</span></span>
* <span data-ttu-id="30a41-132">AllowedVmSizesInLab</span><span class="sxs-lookup"><span data-stu-id="30a41-132">AllowedVmSizesInLab</span></span>
* <span data-ttu-id="30a41-133">LabVmsShutdown</span><span class="sxs-lookup"><span data-stu-id="30a41-133">LabVmsShutdown</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="30a41-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30a41-134">Next steps</span></span>
<span data-ttu-id="30a41-135">Jednou jste udělena oprávnění uživatele k testovacím konkrétní zásady, tady jsou některé další kroky je potřeba vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="30a41-135">Once you've granted user permissions to specific lab policies, here are some next steps to consider:</span></span>

* <span data-ttu-id="30a41-136">[Zabezpečení přístupu k testovacímu prostředí](devtest-lab-add-devtest-user.md)</span><span class="sxs-lookup"><span data-stu-id="30a41-136">[Secure access to a lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="30a41-137">[Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)</span><span class="sxs-lookup"><span data-stu-id="30a41-137">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="30a41-138">[Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)</span><span class="sxs-lookup"><span data-stu-id="30a41-138">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="30a41-139">[Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)</span><span class="sxs-lookup"><span data-stu-id="30a41-139">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="30a41-140">[Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md)</span><span class="sxs-lookup"><span data-stu-id="30a41-140">[Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

