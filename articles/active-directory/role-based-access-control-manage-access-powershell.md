---
title: "Správa řízení přístupu na základě rolí (RBAC) pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Jak spravovat RBAC pomocí Azure Powershellu, včetně obsahující seznam rolí, přiřazení rolí a odstraňovat přiřazení role."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: d7b11df21650b5cb27f9c3dd8306f8d12664185e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a><span data-ttu-id="8d02d-103">Správa řízení přístupu podle role pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d02d-103">Manage Role-Based Access Control with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8d02d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d02d-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="8d02d-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8d02d-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="8d02d-106">REST API</span><span class="sxs-lookup"><span data-stu-id="8d02d-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="8d02d-107">Řízení přístupu na základě Role (RBAC) na portálu Azure a rozhraní API pro správu prostředků Azure můžete použít ke správě přístupu k vašemu předplatnému na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="8d02d-107">You can use Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Management API to manage access to your subscription at a fine-grained level.</span></span> <span data-ttu-id="8d02d-108">Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role je v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="8d02d-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

<span data-ttu-id="8d02d-109">Před prostředí PowerShell můžete použít ke správě RBAC, je třeba následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="8d02d-109">Before you can use PowerShell to manage RBAC, you need the following prerequisites:</span></span>

* <span data-ttu-id="8d02d-110">Azure PowerShell verze 0.8.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8d02d-110">Azure PowerShell version 0.8.8 or later.</span></span> <span data-ttu-id="8d02d-111">Nainstalujte nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8d02d-111">To install the latest version and associate it with your Azure subscription, see [how to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="8d02d-112">Rutiny Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8d02d-112">Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="8d02d-113">Nainstalujte [rutiny Azure Resource Manager](/powershell/azure/overview) v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8d02d-113">Install the [Azure Resource Manager cmdlets](/powershell/azure/overview) in PowerShell.</span></span>

## <a name="list-roles"></a><span data-ttu-id="8d02d-114">Seznam rolí</span><span class="sxs-lookup"><span data-stu-id="8d02d-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="8d02d-115">Zobrazí seznam všech dostupných rolí</span><span class="sxs-lookup"><span data-stu-id="8d02d-115">List all available roles</span></span>
<span data-ttu-id="8d02d-116">Rolím RBAC seznamu, které jsou k dispozici pro přiřazení a chcete-li prověřit operací, u kterých se udělit přístup, použijte `Get-AzureRmRoleDefinition`.</span><span class="sxs-lookup"><span data-stu-id="8d02d-116">To list RBAC roles that are available for assignment and to inspect the operations to which they grant access, use `Get-AzureRmRoleDefinition`.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="8d02d-118">Seznam akcí role</span><span class="sxs-lookup"><span data-stu-id="8d02d-118">List actions of a role</span></span>
<span data-ttu-id="8d02d-119">K zobrazení seznamu akce pro určité role, použijte `Get-AzureRmRoleDefinition <role name>`.</span><span class="sxs-lookup"><span data-stu-id="8d02d-119">To list the actions for a specific role, use `Get-AzureRmRoleDefinition <role name>`.</span></span>

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition pro určité role – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a><span data-ttu-id="8d02d-121">Zjistit, kdo má přístup</span><span class="sxs-lookup"><span data-stu-id="8d02d-121">See who has access</span></span>
<span data-ttu-id="8d02d-122">Přiřazení přístupu RBAC seznamu, použijte `Get-AzureRmRoleAssignment`.</span><span class="sxs-lookup"><span data-stu-id="8d02d-122">To list RBAC access assignments, use `Get-AzureRmRoleAssignment`.</span></span>

### <a name="list-role-assignments-at-a-specific-scope"></a><span data-ttu-id="8d02d-123">Seznam přiřazení rolí na konkrétní rozsah</span><span class="sxs-lookup"><span data-stu-id="8d02d-123">List role assignments at a specific scope</span></span>
<span data-ttu-id="8d02d-124">Můžete zobrazit všechna přiřazení přístupu pro zadané předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="8d02d-124">You can see all the access assignments for a specified subscription, resource group, or resource.</span></span> <span data-ttu-id="8d02d-125">Například pokud chcete zobrazit všech active přiřazení pro skupinu prostředků, použijte `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span><span class="sxs-lookup"><span data-stu-id="8d02d-125">For example, to see the all the active assignments for a resource group, use `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span></span>

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC prostředí PowerShell - Get-AzureRmRoleAssignment pro skupinu prostředků – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a><span data-ttu-id="8d02d-127">Seznam rolí přiřazen k uživateli</span><span class="sxs-lookup"><span data-stu-id="8d02d-127">List roles assigned to a user</span></span>
<span data-ttu-id="8d02d-128">K zobrazení seznamu všech rolí, které jsou přiřazeny pro zadaného uživatele a role, které jsou přiřazeny do skupin, do kterých uživatel patří, použijte `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span><span class="sxs-lookup"><span data-stu-id="8d02d-128">To list all the roles that are assigned to a specified user and the roles that are assigned to the groups to which the user belongs, use `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span></span>

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC prostředí PowerShell - Get-AzureRmRoleAssignment pro uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a><span data-ttu-id="8d02d-130">Seznam přiřazení rolí správce a coadmin classic služby</span><span class="sxs-lookup"><span data-stu-id="8d02d-130">List classic service administrator and coadmin role assignments</span></span>
<span data-ttu-id="8d02d-131">K vypsání přiřazení přístupu pro správce classic předplatného a coadministrators, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-131">To list access assignments for the classic subscription administrator and coadministrators, use:</span></span>

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a><span data-ttu-id="8d02d-132">Udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="8d02d-132">Grant access</span></span>
### <a name="search-for-object-ids"></a><span data-ttu-id="8d02d-133">Vyhledejte ID objektů</span><span class="sxs-lookup"><span data-stu-id="8d02d-133">Search for object IDs</span></span>
<span data-ttu-id="8d02d-134">Pokud chcete přiřadit roli, musíte určit objektu (uživatele, skupiny nebo aplikace) a obor.</span><span class="sxs-lookup"><span data-stu-id="8d02d-134">To assign a role, you need to identify both the object (user, group, or application) and the scope.</span></span>

<span data-ttu-id="8d02d-135">Pokud si nejste jisti ID předplatného, najdete ji v **odběry** okno na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d02d-135">If you don't know the subscription ID, you can find it in the **Subscriptions** blade on the Azure portal.</span></span> <span data-ttu-id="8d02d-136">Další postup dotazování pro ID předplatného najdete v tématu [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="8d02d-136">To learn how to query for the subscription ID, see [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) on MSDN.</span></span>

<span data-ttu-id="8d02d-137">ID objektu pro skupinu služby Azure AD, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-137">To get the object ID for an Azure AD group, use:</span></span>

    Get-AzureRmADGroup -SearchString <group name in quotes>

<span data-ttu-id="8d02d-138">ID objektu pro objekt zabezpečení služby Azure AD nebo aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-138">To get the object ID for an Azure AD service principal or application, use:</span></span>

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a><span data-ttu-id="8d02d-139">Přiřazení role k aplikaci v oboru předplatného</span><span class="sxs-lookup"><span data-stu-id="8d02d-139">Assign a role to an application at the subscription scope</span></span>
<span data-ttu-id="8d02d-140">Chcete-li udělit přístup k aplikaci v oboru předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-140">To grant access to an application at the subscription scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a><span data-ttu-id="8d02d-142">Přiřazení role uživatele v oboru skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8d02d-142">Assign a role to a user at the resource group scope</span></span>
<span data-ttu-id="8d02d-143">Pokud chcete udělit přístup na uživatele v oboru skupiny prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-143">To grant access to a user at the resource group scope, use:</span></span>

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a><span data-ttu-id="8d02d-145">Přiřazení role do skupiny v oboru prostředků</span><span class="sxs-lookup"><span data-stu-id="8d02d-145">Assign a role to a group at the resource scope</span></span>
<span data-ttu-id="8d02d-146">Pokud chcete udělit přístup do skupiny v oboru prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-146">To grant access to a group at the resource scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a><span data-ttu-id="8d02d-148">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="8d02d-148">Remove access</span></span>
<span data-ttu-id="8d02d-149">Pokud chcete odebrat přístup pro uživatele, skupiny a aplikace, použijte:</span><span class="sxs-lookup"><span data-stu-id="8d02d-149">To remove access for users, groups, and applications, use:</span></span>

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC prostředí PowerShell - Remove-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="8d02d-151">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="8d02d-151">Create a custom role</span></span>
<span data-ttu-id="8d02d-152">Chcete-li vytvořit vlastní roli, použijte ```New-AzureRmRoleDefinition``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8d02d-152">To create a custom role, use the ```New-AzureRmRoleDefinition``` command.</span></span> <span data-ttu-id="8d02d-153">Strukturování roli pomocí PSRoleDefinitionObject nebo šablonu JSON dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="8d02d-153">There are two methods of structuring the role, using PSRoleDefinitionObject or a JSON template.</span></span> 

## <a name="get-actions-for-a-resource-provider"></a><span data-ttu-id="8d02d-154">Získat akce pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="8d02d-154">Get Actions for a Resource Provider</span></span>
<span data-ttu-id="8d02d-155">Při vytváření vlastní role od začátku, je důležité vědět, všechny možné operace od poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="8d02d-155">When You are creating custom roles from scratch, it is important to know all the possible operations from the resource providers.</span></span>
<span data-ttu-id="8d02d-156">Použití ```Get-AzureRMProviderOperation``` získat tyto informace.</span><span class="sxs-lookup"><span data-stu-id="8d02d-156">Use the ```Get-AzureRMProviderOperation``` command to get this information.</span></span>
<span data-ttu-id="8d02d-157">Například pokud chcete zkontrolovat všechny operace, které jsou k dispozici pro virtuální počítač použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="8d02d-157">For example, if you want to check all the available operations for virtual Machine use this command:</span></span>

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a><span data-ttu-id="8d02d-158">Vytvořit roli s PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="8d02d-158">Create role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="8d02d-159">Pokud používáte PowerShell můžete vytvořit vlastní roli, můžete začít úplně od začátku nebo použijte jednu z [předdefinované role](role-based-access-built-in-roles.md) jako výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="8d02d-159">When you use PowerShell to create a custom role, you can start from scratch or use one of the [built-in roles](role-based-access-built-in-roles.md) as a starting point.</span></span> <span data-ttu-id="8d02d-160">Příklad v této části začíná předdefinovaná role a pak ho přizpůsobí s další oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8d02d-160">The example in this section starts with a built-in role and then customizes it with more privileges.</span></span> <span data-ttu-id="8d02d-161">Upravit atributy, které se přidat *akce*, *notActions*, nebo *obory* a potom uložte změny jako novou roli.</span><span class="sxs-lookup"><span data-stu-id="8d02d-161">Edit the attributes to add the *Actions*, *notActions*, or *scopes* that you want, and then save the changes as a new role.</span></span>

<span data-ttu-id="8d02d-162">V následujícím příkladu začíná *Přispěvatel virtuálních počítačů* role a používá, chcete-li vytvořit vlastní roli volají *operátor virtuální počítač*.</span><span class="sxs-lookup"><span data-stu-id="8d02d-162">The following example starts with the *Virtual Machine Contributor* role and uses that to create a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="8d02d-163">Nová role uděluje přístup ke všem operacím čtení z *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* zprostředkovatelé a uděluje přístup k prostředkům na spuštění, restartování a monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8d02d-163">The new role grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access to start, restart, and monitor virtual machines.</span></span> <span data-ttu-id="8d02d-164">Vlastní role lze použít ve dvou odběry.</span><span class="sxs-lookup"><span data-stu-id="8d02d-164">The custom role can be used in two subscriptions.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

### <a name="create-role-with-json-template"></a><span data-ttu-id="8d02d-166">Vytvoření role pomocí šablony JSON</span><span class="sxs-lookup"><span data-stu-id="8d02d-166">Create role with JSON template</span></span>
<span data-ttu-id="8d02d-167">Šablonu JSON slouží jako zdroj definice pro vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="8d02d-167">A JSON template can be used as the source definition for the custom role.</span></span> <span data-ttu-id="8d02d-168">Následující příklad vytvoří vlastní role, která umožňuje přístup pro čtení na úložiště a výpočetní prostředky, přístup k podporovat, a přidá do dvou odběry této role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-168">The following example creates a custom role that allows read access to storage and compute resources, access to support, and adds that role to two subscriptions.</span></span> <span data-ttu-id="8d02d-169">Vytvoření nového souboru `C:\CustomRoles\customrole1.json` s následující příklad.</span><span class="sxs-lookup"><span data-stu-id="8d02d-169">Create a new file `C:\CustomRoles\customrole1.json` with the following example.</span></span> <span data-ttu-id="8d02d-170">Id musí být nastavena na `null` na vytvoření počáteční role jako nové ID je generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="8d02d-170">The Id should be set to `null` on initial role creation as a new ID is generated automatically.</span></span> 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```
<span data-ttu-id="8d02d-171">Přidejte roli k předplatným, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8d02d-171">To add the role to the subscriptions, run the following PowerShell command:</span></span>
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a><span data-ttu-id="8d02d-172">Upravit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="8d02d-172">Modify a custom role</span></span>
<span data-ttu-id="8d02d-173">Podobně jako u vytváření vlastní roli, můžete upravit existující vlastní role pomocí PSRoleDefinitionObject nebo šablonu JSON.</span><span class="sxs-lookup"><span data-stu-id="8d02d-173">Similar to creating a custom role, you can modify an existing custom role using either the PSRoleDefinitionObject or a JSON template.</span></span>

### <a name="modify-role-with-psroledefinitionobject"></a><span data-ttu-id="8d02d-174">Upravit role s PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="8d02d-174">Modify role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="8d02d-175">Chcete-li upravit vlastní roli, nejprve použijte `Get-AzureRmRoleDefinition` příkaz načíst definici role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-175">To modify a custom role, first, use the `Get-AzureRmRoleDefinition` command to retrieve the role definition.</span></span> <span data-ttu-id="8d02d-176">Za druhé proveďte požadované změny k definici role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-176">Second, make the desired changes to the role definition.</span></span> <span data-ttu-id="8d02d-177">Nakonec použijte `Set-AzureRmRoleDefinition` příkaz pro uložení definice upravené role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-177">Finally, use the `Set-AzureRmRoleDefinition` command to save the modified role definition.</span></span>

<span data-ttu-id="8d02d-178">Následující příklad přidá `Microsoft.Insights/diagnosticSettings/*` operace *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-178">The following example adds the `Microsoft.Insights/diagnosticSettings/*` operation to the *Virtual Machine Operator* custom role.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell - Set-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

<span data-ttu-id="8d02d-180">Následující příklad přidá do přiřaditelnými obory z předplatného Azure *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-180">The following example adds an Azure subscription to the assignable scopes of the *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell - Set-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a><span data-ttu-id="8d02d-182">Upravit role pomocí šablony JSON</span><span class="sxs-lookup"><span data-stu-id="8d02d-182">Modify role with JSON template</span></span>
<span data-ttu-id="8d02d-183">Pomocí předchozího šablony JSON, můžete snadno upravit existující vlastní roli přidat nebo odebrat akce.</span><span class="sxs-lookup"><span data-stu-id="8d02d-183">Using the previous JSON template, you can easily modify an existing custom role to add or remove Actions.</span></span> <span data-ttu-id="8d02d-184">Aktualizovat šablonu JSON a přidejte čtení akce pro sítě, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8d02d-184">Update the JSON template and add the read action for networking as shown in the following example.</span></span> <span data-ttu-id="8d02d-185">Definice uvedené v šabloně nepoužívají kumulativně existující definice, což znamená, že role zobrazí přesně tak, jak zadáte v šabloně.</span><span class="sxs-lookup"><span data-stu-id="8d02d-185">The definitions listed in the template are not cumulatively applied to an existing definition, meaning that the role appears exactly as you specify in the template.</span></span> <span data-ttu-id="8d02d-186">Také musíte aktualizovat pole Id s ID role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-186">You also need to update the Id field with the ID of the role.</span></span> <span data-ttu-id="8d02d-187">Pokud si nejste jisti, co je tato hodnota, můžete použít `Get-AzureRmRoleDefinition` rutiny získat tyto informace.</span><span class="sxs-lookup"><span data-stu-id="8d02d-187">If you aren't sure what this value is, you can use the `Get-AzureRmRoleDefinition` cmdlet to get this information.</span></span>

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

<span data-ttu-id="8d02d-188">Pokud chcete aktualizovat existující roli, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8d02d-188">To update the existing role, run the following PowerShell command:</span></span>
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a><span data-ttu-id="8d02d-189">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="8d02d-189">Delete a custom role</span></span>
<span data-ttu-id="8d02d-190">Chcete-li odstranit vlastní roli, použijte `Remove-AzureRmRoleDefinition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8d02d-190">To delete a custom role, use the `Remove-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="8d02d-191">Následující příklad odebere *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-191">The following example removes the *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC prostředí PowerShell - Remove-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a><span data-ttu-id="8d02d-193">Vlastní role seznamu</span><span class="sxs-lookup"><span data-stu-id="8d02d-193">List custom roles</span></span>
<span data-ttu-id="8d02d-194">K zobrazení seznamu rolí, které jsou k dispozici pro přiřazení v oboru, použijte `Get-AzureRmRoleDefinition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8d02d-194">To list the roles that are available for assignment at a scope, use the `Get-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="8d02d-195">Následující příklad vypíše všechny role, které jsou k dispozici pro přiřazení ve vybraném předplatném.</span><span class="sxs-lookup"><span data-stu-id="8d02d-195">The following example lists all roles that are available for assignment in the selected subscription.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

<span data-ttu-id="8d02d-197">V následujícím příkladu *operátor virtuálního počítače* vlastní role není k dispozici v *Production4* předplatné vzhledem k tomu, že toto předplatné není v **AssignableScopes** role.</span><span class="sxs-lookup"><span data-stu-id="8d02d-197">In the following example, the *Virtual Machine Operator* custom role isn’t available in the *Production4* subscription because that subscription isn’t in the **AssignableScopes** of the role.</span></span>

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a><span data-ttu-id="8d02d-199">Viz také</span><span class="sxs-lookup"><span data-stu-id="8d02d-199">See also</span></span>
* <span data-ttu-id="8d02d-200">[Použití Azure PowerShell s Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span><span class="sxs-lookup"><span data-stu-id="8d02d-200">[Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span></span>

