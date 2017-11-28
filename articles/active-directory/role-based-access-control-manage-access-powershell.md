---
title: "aaaManage na základě rolí řízení přístupu (RBAC) pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Jak toomanage RBAC pomocí Azure Powershellu, včetně obsahující seznam rolí, přiřazení rolí a odstraňovat přiřazení role."
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
ms.openlocfilehash: fa44991113e75b345177867b0bede38de4373e04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a><span data-ttu-id="85adf-103">Správa řízení přístupu podle role pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="85adf-103">Manage Role-Based Access Control with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="85adf-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="85adf-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="85adf-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="85adf-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="85adf-106">REST API</span><span class="sxs-lookup"><span data-stu-id="85adf-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="85adf-107">Řízení přístupu na základě Role (RBAC) můžete použít v hello portál Azure a rozhraní API pro správu prostředků Azure toomanage přístup tooyour předplatné na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="85adf-107">You can use Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Management API toomanage access tooyour subscription at a fine-grained level.</span></span> <span data-ttu-id="85adf-108">Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role toothem v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="85adf-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

<span data-ttu-id="85adf-109">Než budete moct použít PowerShell toomanage RBAC, je třeba hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="85adf-109">Before you can use PowerShell toomanage RBAC, you need hello following prerequisites:</span></span>

* <span data-ttu-id="85adf-110">Azure PowerShell verze 0.8.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="85adf-110">Azure PowerShell version 0.8.8 or later.</span></span> <span data-ttu-id="85adf-111">tooinstall hello nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85adf-111">tooinstall hello latest version and associate it with your Azure subscription, see [how tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="85adf-112">Rutiny Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="85adf-112">Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="85adf-113">Nainstalujte hello [rutiny Azure Resource Manager](/powershell/azure/overview) v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="85adf-113">Install hello [Azure Resource Manager cmdlets](/powershell/azure/overview) in PowerShell.</span></span>

## <a name="list-roles"></a><span data-ttu-id="85adf-114">Seznam rolí</span><span class="sxs-lookup"><span data-stu-id="85adf-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="85adf-115">Zobrazí seznam všech dostupných rolí</span><span class="sxs-lookup"><span data-stu-id="85adf-115">List all available roles</span></span>
<span data-ttu-id="85adf-116">role RBAC toolist, které jsou k dispozici pro přiřazení a tooinspect hello operations toowhich jejich udělit přístup, používají `Get-AzureRmRoleDefinition`.</span><span class="sxs-lookup"><span data-stu-id="85adf-116">toolist RBAC roles that are available for assignment and tooinspect hello operations toowhich they grant access, use `Get-AzureRmRoleDefinition`.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="85adf-118">Seznam akcí role</span><span class="sxs-lookup"><span data-stu-id="85adf-118">List actions of a role</span></span>
<span data-ttu-id="85adf-119">toolist hello akce pro určité role, využívají `Get-AzureRmRoleDefinition <role name>`.</span><span class="sxs-lookup"><span data-stu-id="85adf-119">toolist hello actions for a specific role, use `Get-AzureRmRoleDefinition <role name>`.</span></span>

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition pro určité role – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a><span data-ttu-id="85adf-121">Zjistit, kdo má přístup</span><span class="sxs-lookup"><span data-stu-id="85adf-121">See who has access</span></span>
<span data-ttu-id="85adf-122">přiřazení přístupu RBAC toolist, použijte `Get-AzureRmRoleAssignment`.</span><span class="sxs-lookup"><span data-stu-id="85adf-122">toolist RBAC access assignments, use `Get-AzureRmRoleAssignment`.</span></span>

### <a name="list-role-assignments-at-a-specific-scope"></a><span data-ttu-id="85adf-123">Seznam přiřazení rolí na konkrétní rozsah</span><span class="sxs-lookup"><span data-stu-id="85adf-123">List role assignments at a specific scope</span></span>
<span data-ttu-id="85adf-124">Můžete zobrazit všechny hello přiřazení přístupu pro zadané předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="85adf-124">You can see all hello access assignments for a specified subscription, resource group, or resource.</span></span> <span data-ttu-id="85adf-125">Například toosee hello všechna přiřazení active hello pro skupinu prostředků, použijte `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span><span class="sxs-lookup"><span data-stu-id="85adf-125">For example, toosee hello all hello active assignments for a resource group, use `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span></span>

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC prostředí PowerShell - Get-AzureRmRoleAssignment pro skupinu prostředků – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-tooa-user"></a><span data-ttu-id="85adf-127">Seznam role přiřazené tooa uživatele</span><span class="sxs-lookup"><span data-stu-id="85adf-127">List roles assigned tooa user</span></span>
<span data-ttu-id="85adf-128">toolist všechny hello role, které jsou přiřazeny tooa zadané uživatele a hello role, které jsou přiřazeny skupiny toohello toowhich hello uživatel patří, použijte `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span><span class="sxs-lookup"><span data-stu-id="85adf-128">toolist all hello roles that are assigned tooa specified user and hello roles that are assigned toohello groups toowhich hello user belongs, use `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span></span>

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC prostředí PowerShell - Get-AzureRmRoleAssignment pro uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a><span data-ttu-id="85adf-130">Seznam přiřazení rolí správce a coadmin classic služby</span><span class="sxs-lookup"><span data-stu-id="85adf-130">List classic service administrator and coadmin role assignments</span></span>
<span data-ttu-id="85adf-131">přiřazení přístupu toolist pro správce hello classic předplatného a coadministrators, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-131">toolist access assignments for hello classic subscription administrator and coadministrators, use:</span></span>

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a><span data-ttu-id="85adf-132">Udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="85adf-132">Grant access</span></span>
### <a name="search-for-object-ids"></a><span data-ttu-id="85adf-133">Vyhledejte ID objektů</span><span class="sxs-lookup"><span data-stu-id="85adf-133">Search for object IDs</span></span>
<span data-ttu-id="85adf-134">tooassign roli, je nutné tooidentify hello objektu (uživatele, skupiny nebo aplikace) a hello oboru.</span><span class="sxs-lookup"><span data-stu-id="85adf-134">tooassign a role, you need tooidentify both hello object (user, group, or application) and hello scope.</span></span>

<span data-ttu-id="85adf-135">Pokud si nejste jisti hello ID předplatného, můžete nějakého najít v hello **odběry** okně hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="85adf-135">If you don't know hello subscription ID, you can find it in hello **Subscriptions** blade on hello Azure portal.</span></span> <span data-ttu-id="85adf-136">jak zjistit, tooquery pro ID předplatného hello, toolearn [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="85adf-136">toolearn how tooquery for hello subscription ID, see [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) on MSDN.</span></span>

<span data-ttu-id="85adf-137">ID objektu hello tooget pro skupiny služby Azure AD, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-137">tooget hello object ID for an Azure AD group, use:</span></span>

    Get-AzureRmADGroup -SearchString <group name in quotes>

<span data-ttu-id="85adf-138">ID objektu hello tooget pro objekt zabezpečení služby Azure AD nebo aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-138">tooget hello object ID for an Azure AD service principal or application, use:</span></span>

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a><span data-ttu-id="85adf-139">Přiřadit k aplikaci tooan role v oboru předplatné hello</span><span class="sxs-lookup"><span data-stu-id="85adf-139">Assign a role tooan application at hello subscription scope</span></span>
<span data-ttu-id="85adf-140">toogrant přístup tooan aplikaci v hello obor předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-140">toogrant access tooan application at hello subscription scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a><span data-ttu-id="85adf-142">Přiřazení role uživatele tooa v oboru skupiny prostředků hello</span><span class="sxs-lookup"><span data-stu-id="85adf-142">Assign a role tooa user at hello resource group scope</span></span>
<span data-ttu-id="85adf-143">uživatel tooa toogrant přístupu v oboru skupiny hello prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-143">toogrant access tooa user at hello resource group scope, use:</span></span>

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a><span data-ttu-id="85adf-145">Přiřadit role tooa skupinu v oboru prostředků hello</span><span class="sxs-lookup"><span data-stu-id="85adf-145">Assign a role tooa group at hello resource scope</span></span>
<span data-ttu-id="85adf-146">Skupina tooa toogrant přístupu v oboru hello prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-146">toogrant access tooa group at hello resource scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a><span data-ttu-id="85adf-148">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="85adf-148">Remove access</span></span>
<span data-ttu-id="85adf-149">tooremove přístup pro uživatele, skupiny a aplikace, použijte:</span><span class="sxs-lookup"><span data-stu-id="85adf-149">tooremove access for users, groups, and applications, use:</span></span>

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC prostředí PowerShell - Remove-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="85adf-151">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="85adf-151">Create a custom role</span></span>
<span data-ttu-id="85adf-152">toocreate vlastní roli, použijte hello ```New-AzureRmRoleDefinition``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="85adf-152">toocreate a custom role, use hello ```New-AzureRmRoleDefinition``` command.</span></span> <span data-ttu-id="85adf-153">Strukturování hello role pomocí PSRoleDefinitionObject nebo šablonu JSON dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="85adf-153">There are two methods of structuring hello role, using PSRoleDefinitionObject or a JSON template.</span></span> 

## <a name="get-actions-for-a-resource-provider"></a><span data-ttu-id="85adf-154">Získat akce pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="85adf-154">Get Actions for a Resource Provider</span></span>
<span data-ttu-id="85adf-155">Při vytváření vlastní role od začátku, je důležité tooknow všechny možné operace od poskytovatelů prostředků hello hello.</span><span class="sxs-lookup"><span data-stu-id="85adf-155">When You are creating custom roles from scratch, it is important tooknow all hello possible operations from hello resource providers.</span></span>
<span data-ttu-id="85adf-156">Použití hello ```Get-AzureRMProviderOperation``` příkaz tooget tyto informace.</span><span class="sxs-lookup"><span data-stu-id="85adf-156">Use hello ```Get-AzureRMProviderOperation``` command tooget this information.</span></span>
<span data-ttu-id="85adf-157">Například pokud chcete, aby toocheck všechny operace hello k dispozici pro virtuální počítač použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="85adf-157">For example, if you want toocheck all hello available operations for virtual Machine use this command:</span></span>

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a><span data-ttu-id="85adf-158">Vytvořit roli s PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="85adf-158">Create role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="85adf-159">Pokud používáte PowerShell toocreate vlastní roli, můžete začít úplně od začátku nebo použijte jednu z hello [předdefinované role](role-based-access-built-in-roles.md) jako výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="85adf-159">When you use PowerShell toocreate a custom role, you can start from scratch or use one of hello [built-in roles](role-based-access-built-in-roles.md) as a starting point.</span></span> <span data-ttu-id="85adf-160">Hello příklad v této části začíná předdefinovaná role a pak ho přizpůsobí s další oprávnění.</span><span class="sxs-lookup"><span data-stu-id="85adf-160">hello example in this section starts with a built-in role and then customizes it with more privileges.</span></span> <span data-ttu-id="85adf-161">Upravit hello atributy tooadd hello *akce*, *notActions*, nebo *obory* a potom uložte změny hello jako novou roli.</span><span class="sxs-lookup"><span data-stu-id="85adf-161">Edit hello attributes tooadd hello *Actions*, *notActions*, or *scopes* that you want, and then save hello changes as a new role.</span></span>

<span data-ttu-id="85adf-162">Hello následující příklad začíná textem hello *Přispěvatel virtuálních počítačů* role a používá tento toocreate vlastní roli volají *virtuální počítač operátor*.</span><span class="sxs-lookup"><span data-stu-id="85adf-162">hello following example starts with hello *Virtual Machine Contributor* role and uses that toocreate a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="85adf-163">Hello nové role uděluje přístup tooall číst operace *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* prostředků poskytovatelů a uděluje přístup toostart, restartování a monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="85adf-163">hello new role grants access tooall read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access toostart, restart, and monitor virtual machines.</span></span> <span data-ttu-id="85adf-164">vlastní role Hello lze použít ve dvou odběry.</span><span class="sxs-lookup"><span data-stu-id="85adf-164">hello custom role can be used in two subscriptions.</span></span>

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

### <a name="create-role-with-json-template"></a><span data-ttu-id="85adf-166">Vytvoření role pomocí šablony JSON</span><span class="sxs-lookup"><span data-stu-id="85adf-166">Create role with JSON template</span></span>
<span data-ttu-id="85adf-167">Šablonu JSON slouží jako zdroj definice hello hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="85adf-167">A JSON template can be used as hello source definition for hello custom role.</span></span> <span data-ttu-id="85adf-168">Hello následující příklad vytvoří vlastní roli, která umožňuje přístup pro čtení toostorage a výpočetní prostředky, přístup k toosupport tuto roli přidá tootwo odběry.</span><span class="sxs-lookup"><span data-stu-id="85adf-168">hello following example creates a custom role that allows read access toostorage and compute resources, access toosupport, and adds that role tootwo subscriptions.</span></span> <span data-ttu-id="85adf-169">Vytvoření nového souboru `C:\CustomRoles\customrole1.json` s hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="85adf-169">Create a new file `C:\CustomRoles\customrole1.json` with hello following example.</span></span> <span data-ttu-id="85adf-170">Hello Id by mělo být nastavené příliš`null` na vytvoření počáteční role jako nové ID je generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="85adf-170">hello Id should be set too`null` on initial role creation as a new ID is generated automatically.</span></span> 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
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
<span data-ttu-id="85adf-171">tooadd hello role toohello odběry, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="85adf-171">tooadd hello role toohello subscriptions, run hello following PowerShell command:</span></span>
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a><span data-ttu-id="85adf-172">Upravit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="85adf-172">Modify a custom role</span></span>
<span data-ttu-id="85adf-173">Podobně jako toocreating vlastní roli, můžete upravit existující vlastní role pomocí hello PSRoleDefinitionObject nebo šablonu JSON.</span><span class="sxs-lookup"><span data-stu-id="85adf-173">Similar toocreating a custom role, you can modify an existing custom role using either hello PSRoleDefinitionObject or a JSON template.</span></span>

### <a name="modify-role-with-psroledefinitionobject"></a><span data-ttu-id="85adf-174">Upravit role s PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="85adf-174">Modify role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="85adf-175">toomodify vlastní role, nejprve pomocí hello `Get-AzureRmRoleDefinition` příkaz definice role tooretrieve hello.</span><span class="sxs-lookup"><span data-stu-id="85adf-175">toomodify a custom role, first, use hello `Get-AzureRmRoleDefinition` command tooretrieve hello role definition.</span></span> <span data-ttu-id="85adf-176">Druhý proveďte změny hello potřeby toohello definice role.</span><span class="sxs-lookup"><span data-stu-id="85adf-176">Second, make hello desired changes toohello role definition.</span></span> <span data-ttu-id="85adf-177">Nakonec použijte hello `Set-AzureRmRoleDefinition` příkaz toosave hello změnil definici role.</span><span class="sxs-lookup"><span data-stu-id="85adf-177">Finally, use hello `Set-AzureRmRoleDefinition` command toosave hello modified role definition.</span></span>

<span data-ttu-id="85adf-178">Hello následující příklad přidá hello `Microsoft.Insights/diagnosticSettings/*` operace toohello *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="85adf-178">hello following example adds hello `Microsoft.Insights/diagnosticSettings/*` operation toohello *Virtual Machine Operator* custom role.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell - Set-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

<span data-ttu-id="85adf-180">Hello následující příklad přidá předplatnému Azure toohello přiřaditelnými obory Dobrý den *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="85adf-180">hello following example adds an Azure subscription toohello assignable scopes of hello *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell - Set-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a><span data-ttu-id="85adf-182">Upravit role pomocí šablony JSON</span><span class="sxs-lookup"><span data-stu-id="85adf-182">Modify role with JSON template</span></span>
<span data-ttu-id="85adf-183">Pomocí šablony JSON předchozí hello, můžete snadno upravit existující tooadd vlastní role nebo odebrat akce.</span><span class="sxs-lookup"><span data-stu-id="85adf-183">Using hello previous JSON template, you can easily modify an existing custom role tooadd or remove Actions.</span></span> <span data-ttu-id="85adf-184">Aktualizace šablony JSON hello a přidejte hello čtení akce pro sítě, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="85adf-184">Update hello JSON template and add hello read action for networking as shown in hello following example.</span></span> <span data-ttu-id="85adf-185">definice Hello uvedené v šabloně hello nejsou stávající definici kumulativně použité tooan, což znamená, že tato role hello se zobrazí přesně tak, jak zadáte v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="85adf-185">hello definitions listed in hello template are not cumulatively applied tooan existing definition, meaning that hello role appears exactly as you specify in hello template.</span></span> <span data-ttu-id="85adf-186">Budete také potřebovat tooupdate hello Id pole s ID hello hello role.</span><span class="sxs-lookup"><span data-stu-id="85adf-186">You also need tooupdate hello Id field with hello ID of hello role.</span></span> <span data-ttu-id="85adf-187">Pokud si nejste jisti, co je tato hodnota, můžete použít hello `Get-AzureRmRoleDefinition` rutiny tooget tyto informace.</span><span class="sxs-lookup"><span data-stu-id="85adf-187">If you aren't sure what this value is, you can use hello `Get-AzureRmRoleDefinition` cmdlet tooget this information.</span></span>

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
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

<span data-ttu-id="85adf-188">tooupdate hello existující roli, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="85adf-188">tooupdate hello existing role, run hello following PowerShell command:</span></span>
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a><span data-ttu-id="85adf-189">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="85adf-189">Delete a custom role</span></span>
<span data-ttu-id="85adf-190">toodelete vlastní roli, použijte hello `Remove-AzureRmRoleDefinition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="85adf-190">toodelete a custom role, use hello `Remove-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="85adf-191">Hello následující příklad odebere hello *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="85adf-191">hello following example removes hello *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC prostředí PowerShell - Remove-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a><span data-ttu-id="85adf-193">Vlastní role seznamu</span><span class="sxs-lookup"><span data-stu-id="85adf-193">List custom roles</span></span>
<span data-ttu-id="85adf-194">toolist hello role, které jsou k dispozici pro přiřazení v oboru, které používají hello `Get-AzureRmRoleDefinition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="85adf-194">toolist hello roles that are available for assignment at a scope, use hello `Get-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="85adf-195">Hello následující příklad uvádí všechny role, které jsou k dispozici pro přiřazení v hello vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="85adf-195">hello following example lists all roles that are available for assignment in hello selected subscription.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

<span data-ttu-id="85adf-197">V následujícím příkladu hello, hello *virtuální počítač operátor* vlastní role není k dispozici v hello *Production4* předplatné vzhledem k tomu, že toto předplatné není v hello  **AssignableScopes** hello role.</span><span class="sxs-lookup"><span data-stu-id="85adf-197">In hello following example, hello *Virtual Machine Operator* custom role isn’t available in hello *Production4* subscription because that subscription isn’t in hello **AssignableScopes** of hello role.</span></span>

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a><span data-ttu-id="85adf-199">Viz také</span><span class="sxs-lookup"><span data-stu-id="85adf-199">See also</span></span>
* <span data-ttu-id="85adf-200">[Použití Azure PowerShell s Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span><span class="sxs-lookup"><span data-stu-id="85adf-200">[Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span></span>

