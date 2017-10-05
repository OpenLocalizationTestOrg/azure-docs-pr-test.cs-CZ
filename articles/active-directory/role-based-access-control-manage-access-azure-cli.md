---
title: "Správa řízení přístupu na základě rolí (RBAC) pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se spravovat na základě rolí řízení přístupu (RBAC) pomocí rozhraní příkazového řádku Azure tak, že uvedete role a role akce a přiřazení rolí k oborům předplatné a aplikace."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: ad644de6d23950e699d99042d27381336626caab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a><span data-ttu-id="aa15e-103">Správa řízení přístupu na základě rolí pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="aa15e-103">Manage Role-Based Access Control with the Azure command-line interface</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aa15e-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa15e-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="aa15e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aa15e-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="aa15e-106">REST API</span><span class="sxs-lookup"><span data-stu-id="aa15e-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)


<span data-ttu-id="aa15e-107">Řízení přístupu na základě Role (RBAC) na portálu Azure a rozhraní API služby Azure Resource Manager můžete použít ke správě přístupu k vaše předplatné a prostředky na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="aa15e-107">You can use Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Manager API to manage access to your subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="aa15e-108">Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role je v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="aa15e-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

<span data-ttu-id="aa15e-109">Ke správě RBAC mohli používat rozhraní příkazového řádku Azure (CLI), musíte mít následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="aa15e-109">Before you can use the Azure command-line interface (CLI) to manage RBAC, you must have the following prerequisites:</span></span>

* <span data-ttu-id="aa15e-110">Azure CLI verze 0.8.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="aa15e-110">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="aa15e-111">Nainstalujte nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="aa15e-111">To install the latest version and associate it with your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="aa15e-112">Azure Resource Manager v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="aa15e-112">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="aa15e-113">Přejděte na [pomocí rozhraní příkazového řádku Azure s Resource Managerem](../xplat-cli-azure-resource-manager.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="aa15e-113">Go to [Using the Azure CLI with the Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

## <a name="list-roles"></a><span data-ttu-id="aa15e-114">Seznam rolí</span><span class="sxs-lookup"><span data-stu-id="aa15e-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="aa15e-115">Zobrazí seznam všech dostupných rolí</span><span class="sxs-lookup"><span data-stu-id="aa15e-115">List all available roles</span></span>
<span data-ttu-id="aa15e-116">K zobrazení seznamu všech dostupných rolí, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-116">To list all available roles, use:</span></span>

        azure role list

<span data-ttu-id="aa15e-117">Následující příklad ukazuje seznam *všechny dostupné role*.</span><span class="sxs-lookup"><span data-stu-id="aa15e-117">The following example shows the list of *all available roles*.</span></span>

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="aa15e-119">Seznam akcí role</span><span class="sxs-lookup"><span data-stu-id="aa15e-119">List actions of a role</span></span>
<span data-ttu-id="aa15e-120">K zobrazení seznamu akce role, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-120">To list the actions of a role, use:</span></span>

    azure role show "<role name>"

<span data-ttu-id="aa15e-121">Následující příklad ukazuje akce *Přispěvatel* a *Přispěvatel virtuálních počítačů* role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-121">The following example shows the actions of the *Contributor* and *Virtual Machine Contributor* roles.</span></span>

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure příkazového řádku - azure role zobrazit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a><span data-ttu-id="aa15e-123">Přístup k seznamu</span><span class="sxs-lookup"><span data-stu-id="aa15e-123">List access</span></span>
### <a name="list-role-assignments-effective-on-a-resource-group"></a><span data-ttu-id="aa15e-124">Přiřazení rolí seznamu efektivní ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa15e-124">List role assignments effective on a resource group</span></span>
<span data-ttu-id="aa15e-125">K zobrazení seznamu přiřazení rolí, které existují ve skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-125">To list the role assignments that exist in a resource group, use:</span></span>

    azure role assignment list --resource-group <resource group name>

<span data-ttu-id="aa15e-126">Následující příklad ukazuje přiřazení rolí v *pharma. prodej projecforcast* skupiny.</span><span class="sxs-lookup"><span data-stu-id="aa15e-126">The following example shows the role assignments in the *pharma-sales-projecforcast* group.</span></span>

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu podle skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a><span data-ttu-id="aa15e-128">Seznam přiřazení role pro uživatele</span><span class="sxs-lookup"><span data-stu-id="aa15e-128">List role assignments for a user</span></span>
<span data-ttu-id="aa15e-129">K zobrazení seznamu přiřazení rolí pro konkrétního uživatele a přiřazení, které jsou přiřazeny k skupin uživatelů, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-129">To list the role assignments for a specific user and the assignments that are assigned to a user's groups, use:</span></span>

    azure role assignment list --signInName <user email>

<span data-ttu-id="aa15e-130">Můžete také zjistit přiřazení rolí, které se dědí z skupin změnou příkaz:</span><span class="sxs-lookup"><span data-stu-id="aa15e-130">You can also see role assignments that are inherited from groups by modifying the command:</span></span>

    azure role assignment list --expandPrincipalGroups --signInName <user email>

<span data-ttu-id="aa15e-131">Následující příklad ukazuje, kterým je uděleno oprávnění k přiřazení rolí  *sameert@aaddemo.com*  uživatele.</span><span class="sxs-lookup"><span data-stu-id="aa15e-131">The following example shows the role assignments that are granted to the *sameert@aaddemo.com* user.</span></span> <span data-ttu-id="aa15e-132">To zahrnuje role, které jsou přiřazeny přímo na uživatele a role, které se dědí z skupin.</span><span class="sxs-lookup"><span data-stu-id="aa15e-132">This includes roles that are assigned directly to the user and roles that are inherited from groups.</span></span>

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu uživatelem – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a><span data-ttu-id="aa15e-134">Udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="aa15e-134">Grant access</span></span>
<span data-ttu-id="aa15e-135">Po zjištění roli, kterou chcete přiřadit udělit přístup, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-135">To grant access after you have identified the role that you want to assign, use:</span></span>

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a><span data-ttu-id="aa15e-136">Přiřazení role do skupiny v oboru předplatného</span><span class="sxs-lookup"><span data-stu-id="aa15e-136">Assign a role to group at the subscription scope</span></span>
<span data-ttu-id="aa15e-137">K přiřazení role do skupiny v oboru předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-137">To assign a role to a group at the subscription scope, use:</span></span>

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="aa15e-138">Následující příklad přiřadí *čtečky* role *Jana Koch Team* na *předplatné* oboru.</span><span class="sxs-lookup"><span data-stu-id="aa15e-138">The following example assigns the *Reader* role to *Christine Koch's Team* at the *subscription* scope.</span></span>

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a><span data-ttu-id="aa15e-140">Přiřazení role k aplikaci v oboru předplatného</span><span class="sxs-lookup"><span data-stu-id="aa15e-140">Assign a role to an application at the subscription scope</span></span>
<span data-ttu-id="aa15e-141">K přiřazení role k aplikaci v oboru předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-141">To assign a role to an application at the subscription scope, use:</span></span>

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="aa15e-142">Následující příklad povolí *Přispěvatel* role *Azure AD* aplikace na vybraném předplatném.</span><span class="sxs-lookup"><span data-stu-id="aa15e-142">The following example grants the *Contributor* role to an *Azure AD* application on the selected subscription.</span></span>

 ![Příkaz Azure RBAC - přiřazení role v azure vytvořit aplikací](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a><span data-ttu-id="aa15e-144">Přiřazení role uživatele v oboru skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="aa15e-144">Assign a role to a user at the resource group scope</span></span>
<span data-ttu-id="aa15e-145">Pokud chcete přiřadit role pro uživatele v oboru skupiny prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-145">To assign a role to a user at the resource group scope, use:</span></span>

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

<span data-ttu-id="aa15e-146">Následující příklad povolí *Přispěvatel virtuálních počítačů* role  *samert@aaddemo.com*  uživatel na *Pharma. prodej ProjectForcast* oboru skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa15e-146">The following example grants the *Virtual Machine Contributor* role to *samert@aaddemo.com* user at the *Pharma-Sales-ProjectForcast* resource group scope.</span></span>

![Příkaz Azure RBAC - přiřazení role v azure vytvořit uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a><span data-ttu-id="aa15e-148">Přiřazení role do skupiny v oboru prostředků</span><span class="sxs-lookup"><span data-stu-id="aa15e-148">Assign a role to a group at the resource scope</span></span>
<span data-ttu-id="aa15e-149">K přiřazení role do skupiny v oboru prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-149">To assign a role to a group at the resource scope, use:</span></span>

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

<span data-ttu-id="aa15e-150">Následující příklad povolí *Přispěvatel virtuálních počítačů* role *Azure AD* na *podsítě*.</span><span class="sxs-lookup"><span data-stu-id="aa15e-150">The following example grants the *Virtual Machine Contributor* role to an *Azure AD* group on a *subnet*.</span></span>

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a><span data-ttu-id="aa15e-152">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="aa15e-152">Remove access</span></span>
<span data-ttu-id="aa15e-153">Chcete-li odebrat přiřazení role, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-153">To remove a role assignment, use:</span></span>

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

<span data-ttu-id="aa15e-154">Následující příklad odebere *Přispěvatel virtuálních počítačů* přiřazení role z  *sammert@aaddemo.com*  uživatele na *Pharma. prodej ProjectForcast* prostředků Skupina.</span><span class="sxs-lookup"><span data-stu-id="aa15e-154">The following example removes the *Virtual Machine Contributor* role assignment from the *sammert@aaddemo.com* user on the *Pharma-Sales-ProjectForcast* resource group.</span></span>
<span data-ttu-id="aa15e-155">Přiřazení role v příkladu pak odebere ze skupiny v odběru.</span><span class="sxs-lookup"><span data-stu-id="aa15e-155">The example then removes the role assignment from a group on the subscription.</span></span>

![RBAC Azure příkazového řádku - azure role přiřazení odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="aa15e-157">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="aa15e-157">Create a custom role</span></span>
<span data-ttu-id="aa15e-158">Chcete-li vytvořit vlastní roli, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa15e-158">To create a custom role, use:</span></span>

    azure role create --inputfile <file path>

<span data-ttu-id="aa15e-159">Následující příklad vytvoří vlastní role s názvem *virtuální počítač operátor*.</span><span class="sxs-lookup"><span data-stu-id="aa15e-159">The following example creates a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="aa15e-160">Tato vlastní role uděluje přístup ke všem operacím čtení z *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* zprostředkovatelé prostředků a uděluje přístup k spuštění, restartování a monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aa15e-160">This custom role grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access to start, restart, and monitor virtual machines.</span></span> <span data-ttu-id="aa15e-161">Tuto vlastní roli můžete použít ve dvou předplatných.</span><span class="sxs-lookup"><span data-stu-id="aa15e-161">This custom role can be used in two subscriptions.</span></span> <span data-ttu-id="aa15e-162">Tento příklad používá soubor JSON jako vstup.</span><span class="sxs-lookup"><span data-stu-id="aa15e-162">This example uses a JSON file as an input.</span></span>

![JSON – definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure příkazového řádku - azure role vytvořit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a><span data-ttu-id="aa15e-165">Upravit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="aa15e-165">Modify a custom role</span></span>
<span data-ttu-id="aa15e-166">Chcete-li upravit vlastní roli, nejprve pomocí `azure role show` příkaz načíst definici role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-166">To modify a custom role, first use the `azure role show` command to retrieve role definition.</span></span> <span data-ttu-id="aa15e-167">Za druhé proveďte požadované změny do souboru definice role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-167">Second, make the desired changes to the role definition file.</span></span> <span data-ttu-id="aa15e-168">Nakonec použijte `azure role set` se uložit definici upravené role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-168">Finally, use `azure role set` to save the modified role definition.</span></span>

    azure role set --inputfile <file path>

<span data-ttu-id="aa15e-169">Následující příklad přidá *Microsoft.Insights/diagnosticSettings/* operace **akce**a předplatné Azure k **AssignableScopes** z Vlastní role operátora virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aa15e-169">The following example adds the *Microsoft.Insights/diagnosticSettings/* operation to the **Actions**, and an Azure subscription to the **AssignableScopes** of the Virtual Machine Operator custom role.</span></span>

![JSON - upravit definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Snímek obrazovky příkazového řádku - azure roli set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a><span data-ttu-id="aa15e-172">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="aa15e-172">Delete a custom role</span></span>
<span data-ttu-id="aa15e-173">Chcete-li odstranit vlastní roli, nejprve pomocí `azure role show` příkaz, abyste zjistili **ID** role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-173">To delete a custom role, first use the `azure role show` command to determine the **ID** of the role.</span></span> <span data-ttu-id="aa15e-174">Potom použít `azure role delete` příkaz k odstranění role zadáním **ID**.</span><span class="sxs-lookup"><span data-stu-id="aa15e-174">Then, use the `azure role delete` command to delete the role by specifying the **ID**.</span></span>

<span data-ttu-id="aa15e-175">Následující příklad odebere *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-175">The following example removes the *Virtual Machine Operator* custom role.</span></span>

![RBAC Azure příkazového řádku - azure role odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a><span data-ttu-id="aa15e-177">Vlastní role seznamu</span><span class="sxs-lookup"><span data-stu-id="aa15e-177">List custom roles</span></span>
<span data-ttu-id="aa15e-178">K zobrazení seznamu rolí, které jsou k dispozici pro přiřazení v oboru, použijte `azure role list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="aa15e-178">To list the roles that are available for assignment at a scope, use the `azure role list` command.</span></span>

<span data-ttu-id="aa15e-179">Následující příkaz vypíše všechny role, které jsou k dispozici pro přiřazení ve vybraném předplatném.</span><span class="sxs-lookup"><span data-stu-id="aa15e-179">The following command lists all roles that are available for assignment in the selected subscription.</span></span>

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

<span data-ttu-id="aa15e-181">V následujícím příkladu *operátor virtuálního počítače* vlastní role není k dispozici v *Production4* předplatné vzhledem k tomu, že toto předplatné není v **AssignableScopes** role.</span><span class="sxs-lookup"><span data-stu-id="aa15e-181">In the following example, the *Virtual Machine Operator* custom role isn’t available in the *Production4* subscription because that subscription isn’t in the **AssignableScopes** of the role.</span></span>

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure příkazového řádku - azure role seznam pro vlastní role – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a><span data-ttu-id="aa15e-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa15e-183">Next steps</span></span>
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

