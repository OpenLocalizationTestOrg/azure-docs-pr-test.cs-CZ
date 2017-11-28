---
title: "zásad testovacího toospecific oprávnění uživatele aaaGrant | Microsoft Docs"
description: "Zjistěte, jak toogrant testovacím zásady toospecific oprávnění uživatele v DevTest Labs podle potřeb jednotlivých uživatelů"
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
ms.openlocfilehash: 35647ab837243188f06566cdf365b67fe33a3865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="grant-user-permissions-toospecific-lab-policies"></a><span data-ttu-id="65f15-103">Udělení uživatelských oprávnění zásad testovacího toospecific</span><span class="sxs-lookup"><span data-stu-id="65f15-103">Grant user permissions toospecific lab policies</span></span>
## <a name="overview"></a><span data-ttu-id="65f15-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="65f15-104">Overview</span></span>
<span data-ttu-id="65f15-105">Tento článek ukazuje, jak toouse prostředí PowerShell toogrant uživatelé oprávnění tooa konkrétní testovacím zásad.</span><span class="sxs-lookup"><span data-stu-id="65f15-105">This article illustrates how toouse PowerShell toogrant users permissions tooa particular lab policy.</span></span> <span data-ttu-id="65f15-106">Toto oprávnění můžete být použito podle potřeb jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="65f15-106">That way, permissions can be applied based on each user's needs.</span></span> <span data-ttu-id="65f15-107">Například můžete chtít toogrant určitého uživatele hello možnost toochange hello virtuálního počítače nastavení zásad, ale není hello náklady zásady.</span><span class="sxs-lookup"><span data-stu-id="65f15-107">For example, you might want toogrant a particular user hello ability toochange hello VM policy settings, but not hello cost policies.</span></span>

## <a name="policies-as-resources"></a><span data-ttu-id="65f15-108">Zásady jako prostředky</span><span class="sxs-lookup"><span data-stu-id="65f15-108">Policies as resources</span></span>
<span data-ttu-id="65f15-109">Jak je popsáno v hello [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md) článku RBAC umožňuje přesnou správu přístupu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="65f15-109">As discussed in hello [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) article, RBAC enables fine-grained access management of resources for Azure.</span></span> <span data-ttu-id="65f15-110">RBAC můžete oddělit povinností v rámci týmu DevOps a udělit pouze hello množství toousers přístup, že potřebují tooperform svou práci.</span><span class="sxs-lookup"><span data-stu-id="65f15-110">Using RBAC, you can segregate duties within your DevOps team and grant only hello amount of access toousers that they need tooperform their jobs.</span></span>

<span data-ttu-id="65f15-111">V DevTest Labs zásady je typ prostředku, který umožňuje akci RBAC hello **Microsoft.DevTestLab/labs/policySets/policies/**.</span><span class="sxs-lookup"><span data-stu-id="65f15-111">In DevTest Labs, a policy is a resource type that enables hello RBAC action **Microsoft.DevTestLab/labs/policySets/policies/**.</span></span> <span data-ttu-id="65f15-112">Každá zásada testovacího prostředí je prostředek v hello typ prostředku zásad a lze přiřadit jako obor role RBAC tooan.</span><span class="sxs-lookup"><span data-stu-id="65f15-112">Each lab policy is a resource in hello Policy resource type, and can be assigned as a scope tooan RBAC role.</span></span>

<span data-ttu-id="65f15-113">Například v pořadí toogrant uživatelé pro čtení a zápis oprávnění toohello **povolené velikosti virtuálních počítačů** zásady, by vytvořit vlastní roli, která funguje s hello **Microsoft.DevTestLab/labs/policySets/policies/** * Akce a potom přiřadit hello příslušné uživatele toothis vlastní role v oboru hello **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span><span class="sxs-lookup"><span data-stu-id="65f15-113">For example, in order toogrant users read/write permission toohello **Allowed VM Sizes** policy, you would create a custom role that works with hello **Microsoft.DevTestLab/labs/policySets/policies/*** action, and then assign hello appropriate users toothis custom role in hello scope of **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span></span>

<span data-ttu-id="65f15-114">toolearn Další informace o vlastních rolí v RBAC, najdete v části hello [vlastní role řízení přístupu](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="65f15-114">toolearn more about custom roles in RBAC, see hello [Custom roles access control](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="creating-a-lab-custom-role-using-powershell"></a><span data-ttu-id="65f15-115">Vytvoření testovacího prostředí vlastní role pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="65f15-115">Creating a lab custom role using PowerShell</span></span>
<span data-ttu-id="65f15-116">V pořadí tooget spuštění, budete potřebovat tooread hello následujícího článku, který vysvětluje, jak tooinstall a nakonfigurujte rutin prostředí Azure PowerShell hello: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span><span class="sxs-lookup"><span data-stu-id="65f15-116">In order tooget started, you’ll need tooread hello following article, which will explain how tooinstall and configure hello Azure PowerShell cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span></span>

<span data-ttu-id="65f15-117">Jakmile jste nastavili hello rutin prostředí Azure PowerShell, můžete provést hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="65f15-117">Once you’ve set up hello Azure PowerShell cmdlets, you can perform hello following tasks:</span></span>

* <span data-ttu-id="65f15-118">Seznam všech hello operací na akcí pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="65f15-118">List all hello operations/actions for a resource provider</span></span>
* <span data-ttu-id="65f15-119">Seznam akcí v konkrétní roli:</span><span class="sxs-lookup"><span data-stu-id="65f15-119">List actions in a particular role:</span></span>
* <span data-ttu-id="65f15-120">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="65f15-120">Create a custom role</span></span>

<span data-ttu-id="65f15-121">Příklady, jak znázorňuje následující skript prostředí PowerShell Hello tooperform tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="65f15-121">hello following PowerShell script illustrates examples of how tooperform these tasks:</span></span>

    ‘List all hello operations/actions for a resource provider.
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

## <a name="assigning-permissions-tooa-user-for-a-specific-policy-using-custom-roles"></a><span data-ttu-id="65f15-122">Přiřazení oprávnění tooa uživatele pro konkrétní zásady pomocí vlastní role</span><span class="sxs-lookup"><span data-stu-id="65f15-122">Assigning permissions tooa user for a specific policy using custom roles</span></span>
<span data-ttu-id="65f15-123">Jakmile vlastní role, které jste definovali, můžete je přiřadit toousers.</span><span class="sxs-lookup"><span data-stu-id="65f15-123">Once you’ve defined your custom roles, you can assign them toousers.</span></span> <span data-ttu-id="65f15-124">V pořadí tooassign uživatel tooa vlastní roli, je nutné nejprve získat hello **ObjectId** představující tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="65f15-124">In order tooassign a custom role tooa user, you must first obtain hello **ObjectId** representing that user.</span></span> <span data-ttu-id="65f15-125">toodo, který použít hello **Get-AzureRmADUser** rutiny.</span><span class="sxs-lookup"><span data-stu-id="65f15-125">toodo that, use hello **Get-AzureRmADUser** cmdlet.</span></span>

<span data-ttu-id="65f15-126">V následujícím příkladu hello, hello **ObjectId** z hello *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 je uživatel.</span><span class="sxs-lookup"><span data-stu-id="65f15-126">In hello following example, hello **ObjectId** of hello *SomeUser* user is 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span></span>

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

<span data-ttu-id="65f15-127">Jakmile máte hello **ObjectId** hello uživatele a jeho název vlastní roli, můžete přiřadit tento uživatel toohello role s hello **New-AzureRmRoleAssignment** rutiny:</span><span class="sxs-lookup"><span data-stu-id="65f15-127">Once you have hello **ObjectId** for hello user and a custom role name, you can assign that role toohello user with hello **New-AzureRmRoleAssignment** cmdlet:</span></span>

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

<span data-ttu-id="65f15-128">V předchozím příkladu hello hello **AllowedVmSizesInLab** zásada se používá.</span><span class="sxs-lookup"><span data-stu-id="65f15-128">In hello previous example, hello **AllowedVmSizesInLab** policy is used.</span></span> <span data-ttu-id="65f15-129">Můžete použít některou z hello následující zásady:</span><span class="sxs-lookup"><span data-stu-id="65f15-129">You can use any of hello following polices:</span></span>

* <span data-ttu-id="65f15-130">MaxVmsAllowedPerUser</span><span class="sxs-lookup"><span data-stu-id="65f15-130">MaxVmsAllowedPerUser</span></span>
* <span data-ttu-id="65f15-131">MaxVmsAllowedPerLab</span><span class="sxs-lookup"><span data-stu-id="65f15-131">MaxVmsAllowedPerLab</span></span>
* <span data-ttu-id="65f15-132">AllowedVmSizesInLab</span><span class="sxs-lookup"><span data-stu-id="65f15-132">AllowedVmSizesInLab</span></span>
* <span data-ttu-id="65f15-133">LabVmsShutdown</span><span class="sxs-lookup"><span data-stu-id="65f15-133">LabVmsShutdown</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="65f15-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65f15-134">Next steps</span></span>
<span data-ttu-id="65f15-135">Jednou jste udělena zásad testovacího toospecific oprávnění uživatele, tady jsou některé další kroky tooconsider:</span><span class="sxs-lookup"><span data-stu-id="65f15-135">Once you've granted user permissions toospecific lab policies, here are some next steps tooconsider:</span></span>

* <span data-ttu-id="65f15-136">[Laboratoř tooa zabezpečeného přístupu](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="65f15-136">[Secure access tooa lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="65f15-137">[Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)</span><span class="sxs-lookup"><span data-stu-id="65f15-137">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="65f15-138">[Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)</span><span class="sxs-lookup"><span data-stu-id="65f15-138">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="65f15-139">[Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)</span><span class="sxs-lookup"><span data-stu-id="65f15-139">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="65f15-140">[Přidání virtuálního počítače s artefakty tooa testovacím](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="65f15-140">[Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

