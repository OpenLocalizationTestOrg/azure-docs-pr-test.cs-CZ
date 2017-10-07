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
# <a name="grant-user-permissions-toospecific-lab-policies"></a>Udělení uživatelských oprávnění zásad testovacího toospecific
## <a name="overview"></a>Přehled
Tento článek ukazuje, jak toouse prostředí PowerShell toogrant uživatelé oprávnění tooa konkrétní testovacím zásad. Toto oprávnění můžete být použito podle potřeb jednotlivých uživatelů. Například můžete chtít toogrant určitého uživatele hello možnost toochange hello virtuálního počítače nastavení zásad, ale není hello náklady zásady.

## <a name="policies-as-resources"></a>Zásady jako prostředky
Jak je popsáno v hello [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md) článku RBAC umožňuje přesnou správu přístupu prostředků Azure. RBAC můžete oddělit povinností v rámci týmu DevOps a udělit pouze hello množství toousers přístup, že potřebují tooperform svou práci.

V DevTest Labs zásady je typ prostředku, který umožňuje akci RBAC hello **Microsoft.DevTestLab/labs/policySets/policies/**. Každá zásada testovacího prostředí je prostředek v hello typ prostředku zásad a lze přiřadit jako obor role RBAC tooan.

Například v pořadí toogrant uživatelé pro čtení a zápis oprávnění toohello **povolené velikosti virtuálních počítačů** zásady, by vytvořit vlastní roli, která funguje s hello **Microsoft.DevTestLab/labs/policySets/policies/** * Akce a potom přiřadit hello příslušné uživatele toothis vlastní role v oboru hello **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

toolearn Další informace o vlastních rolí v RBAC, najdete v části hello [vlastní role řízení přístupu](../active-directory/role-based-access-control-custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>Vytvoření testovacího prostředí vlastní role pomocí prostředí PowerShell
V pořadí tooget spuštění, budete potřebovat tooread hello následujícího článku, který vysvětluje, jak tooinstall a nakonfigurujte rutin prostředí Azure PowerShell hello: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Jakmile jste nastavili hello rutin prostředí Azure PowerShell, můžete provést hello následující úlohy:

* Seznam všech hello operací na akcí pro poskytovatele prostředků
* Seznam akcí v konkrétní roli:
* Vytvořit vlastní roli

Příklady, jak znázorňuje následující skript prostředí PowerShell Hello tooperform tyto úlohy:

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

## <a name="assigning-permissions-tooa-user-for-a-specific-policy-using-custom-roles"></a>Přiřazení oprávnění tooa uživatele pro konkrétní zásady pomocí vlastní role
Jakmile vlastní role, které jste definovali, můžete je přiřadit toousers. V pořadí tooassign uživatel tooa vlastní roli, je nutné nejprve získat hello **ObjectId** představující tohoto uživatele. toodo, který použít hello **Get-AzureRmADUser** rutiny.

V následujícím příkladu hello, hello **ObjectId** z hello *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 je uživatel.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Jakmile máte hello **ObjectId** hello uživatele a jeho název vlastní roli, můžete přiřadit tento uživatel toohello role s hello **New-AzureRmRoleAssignment** rutiny:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

V předchozím příkladu hello hello **AllowedVmSizesInLab** zásada se používá. Můžete použít některou z hello následující zásady:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Jednou jste udělena zásad testovacího toospecific oprávnění uživatele, tady jsou některé další kroky tooconsider:

* [Laboratoř tooa zabezpečeného přístupu](devtest-lab-add-devtest-user.md).
* [Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)
* [Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)
* [Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)
* [Přidání virtuálního počítače s artefakty tooa testovacím](devtest-lab-add-vm-with-artifacts.md).

