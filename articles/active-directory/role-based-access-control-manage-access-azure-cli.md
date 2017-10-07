---
title: "aaaManage na základě rolí řízení přístupu (RBAC) pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak rozhraní toomanage na základě rolí řízení přístupu (RBAC) pomocí příkazového řádku Azure hello seznam rolí a rolí akce a přiřazení rolí obory toohello předplatné a aplikace."
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
ms.openlocfilehash: 438418e5f6ee9b98908c9c264d516eb722a4e26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-azure-command-line-interface"></a><span data-ttu-id="f2fb0-103">Správa řízení přístupu na základě rolí pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="f2fb0-103">Manage Role-Based Access Control with hello Azure command-line interface</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f2fb0-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2fb0-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="f2fb0-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f2fb0-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="f2fb0-106">REST API</span><span class="sxs-lookup"><span data-stu-id="f2fb0-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)


<span data-ttu-id="f2fb0-107">Řízení přístupu na základě Role (RBAC) můžete použít v hello portál Azure a rozhraní API služby Azure Resource Manager toomanage přístup tooyour předplatného a prostředky na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-107">You can use Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Manager API toomanage access tooyour subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="f2fb0-108">Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role toothem v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

<span data-ttu-id="f2fb0-109">Než budete moci použít hello rozhraní příkazového řádku Azure (CLI) toomanage RBAC, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-109">Before you can use hello Azure command-line interface (CLI) toomanage RBAC, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="f2fb0-110">Azure CLI verze 0.8.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-110">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="f2fb0-111">tooinstall hello nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f2fb0-111">tooinstall hello latest version and associate it with your Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="f2fb0-112">Azure Resource Manager v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-112">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="f2fb0-113">Přejděte příliš[hello pomocí rozhraní příkazového řádku Azure s hello Resource Manager](../xplat-cli-azure-resource-manager.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-113">Go too[Using hello Azure CLI with hello Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

## <a name="list-roles"></a><span data-ttu-id="f2fb0-114">Seznam rolí</span><span class="sxs-lookup"><span data-stu-id="f2fb0-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="f2fb0-115">Zobrazí seznam všech dostupných rolí</span><span class="sxs-lookup"><span data-stu-id="f2fb0-115">List all available roles</span></span>
<span data-ttu-id="f2fb0-116">toolist všechny dostupné role, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-116">toolist all available roles, use:</span></span>

        azure role list

<span data-ttu-id="f2fb0-117">Hello následující příklad ukazuje hello seznam *všechny dostupné role*.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-117">hello following example shows hello list of *all available roles*.</span></span>

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="f2fb0-119">Seznam akcí role</span><span class="sxs-lookup"><span data-stu-id="f2fb0-119">List actions of a role</span></span>
<span data-ttu-id="f2fb0-120">Akce hello toolist role, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-120">toolist hello actions of a role, use:</span></span>

    azure role show "<role name>"

<span data-ttu-id="f2fb0-121">Hello následující příklad ukazuje hello akce hello *Přispěvatel* a *Přispěvatel virtuálních počítačů* role.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-121">hello following example shows hello actions of hello *Contributor* and *Virtual Machine Contributor* roles.</span></span>

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure příkazového řádku - azure role zobrazit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a><span data-ttu-id="f2fb0-123">Přístup k seznamu</span><span class="sxs-lookup"><span data-stu-id="f2fb0-123">List access</span></span>
### <a name="list-role-assignments-effective-on-a-resource-group"></a><span data-ttu-id="f2fb0-124">Přiřazení rolí seznamu efektivní ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-124">List role assignments effective on a resource group</span></span>
<span data-ttu-id="f2fb0-125">přiřazení rolí hello toolist které existují ve skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-125">toolist hello role assignments that exist in a resource group, use:</span></span>

    azure role assignment list --resource-group <resource group name>

<span data-ttu-id="f2fb0-126">Hello následující příklad ukazuje hello přiřazení rolí v hello *pharma. prodej projecforcast* skupiny.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-126">hello following example shows hello role assignments in hello *pharma-sales-projecforcast* group.</span></span>

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu podle skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a><span data-ttu-id="f2fb0-128">Seznam přiřazení role pro uživatele</span><span class="sxs-lookup"><span data-stu-id="f2fb0-128">List role assignments for a user</span></span>
<span data-ttu-id="f2fb0-129">přiřazení rolí hello toolist pro konkrétního uživatele a přiřazení hello, které jsou přiřazeny skupiny tooa uživatelů, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-129">toolist hello role assignments for a specific user and hello assignments that are assigned tooa user's groups, use:</span></span>

    azure role assignment list --signInName <user email>

<span data-ttu-id="f2fb0-130">Můžete také zjistit přiřazení rolí, které se dědí z skupin změnou hello příkaz:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-130">You can also see role assignments that are inherited from groups by modifying hello command:</span></span>

    azure role assignment list --expandPrincipalGroups --signInName <user email>

<span data-ttu-id="f2fb0-131">Hello následující příklad ukazuje hello přiřazení role, kterým je uděleno oprávnění toohello  *sameert@aaddemo.com*  uživatele.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-131">hello following example shows hello role assignments that are granted toohello *sameert@aaddemo.com* user.</span></span> <span data-ttu-id="f2fb0-132">To zahrnuje role, které jsou přiřazeny přímo toohello uživatele a role, které se dědí z skupin.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-132">This includes roles that are assigned directly toohello user and roles that are inherited from groups.</span></span>

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu uživatelem – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a><span data-ttu-id="f2fb0-134">Udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="f2fb0-134">Grant access</span></span>
<span data-ttu-id="f2fb0-135">toogrant přístup po zjištění hello role, které chcete tooassign, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-135">toogrant access after you have identified hello role that you want tooassign, use:</span></span>

    azure role assignment create

### <a name="assign-a-role-toogroup-at-hello-subscription-scope"></a><span data-ttu-id="f2fb0-136">Přiřazení role toogroup v oboru předplatné hello</span><span class="sxs-lookup"><span data-stu-id="f2fb0-136">Assign a role toogroup at hello subscription scope</span></span>
<span data-ttu-id="f2fb0-137">tooassign skupinu tooa role v hello obor předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-137">tooassign a role tooa group at hello subscription scope, use:</span></span>

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="f2fb0-138">Hello následující příklad přiřadí hello *čtečky* role příliš*Jana Koch Team* v hello *předplatné* oboru.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-138">hello following example assigns hello *Reader* role too*Christine Koch's Team* at hello *subscription* scope.</span></span>

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a><span data-ttu-id="f2fb0-140">Přiřadit k aplikaci tooan role v oboru předplatné hello</span><span class="sxs-lookup"><span data-stu-id="f2fb0-140">Assign a role tooan application at hello subscription scope</span></span>
<span data-ttu-id="f2fb0-141">tooassign aplikace tooan role za hello obor předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-141">tooassign a role tooan application at hello subscription scope, use:</span></span>

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="f2fb0-142">Hello následující příklad uděluje hello *Přispěvatel* role tooan *Azure AD* aplikace na hello vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-142">hello following example grants hello *Contributor* role tooan *Azure AD* application on hello selected subscription.</span></span>

 ![Příkaz Azure RBAC - přiřazení role v azure vytvořit aplikací](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a><span data-ttu-id="f2fb0-144">Přiřazení role uživatele tooa v oboru skupiny prostředků hello</span><span class="sxs-lookup"><span data-stu-id="f2fb0-144">Assign a role tooa user at hello resource group scope</span></span>
<span data-ttu-id="f2fb0-145">tooassign tooa role uživatele v oboru skupiny hello prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-145">tooassign a role tooa user at hello resource group scope, use:</span></span>

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

<span data-ttu-id="f2fb0-146">Hello následující příklad uděluje hello *Přispěvatel virtuálních počítačů* role příliš *samert@aaddemo.com*  uživatel na hello *Pharma. prodej ProjectForcast* oboru skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-146">hello following example grants hello *Virtual Machine Contributor* role too*samert@aaddemo.com* user at hello *Pharma-Sales-ProjectForcast* resource group scope.</span></span>

![Příkaz Azure RBAC - přiřazení role v azure vytvořit uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a><span data-ttu-id="f2fb0-148">Přiřadit role tooa skupinu v oboru prostředků hello</span><span class="sxs-lookup"><span data-stu-id="f2fb0-148">Assign a role tooa group at hello resource scope</span></span>
<span data-ttu-id="f2fb0-149">tooassign skupinu tooa role v oboru hello prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-149">tooassign a role tooa group at hello resource scope, use:</span></span>

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

<span data-ttu-id="f2fb0-150">Hello následující příklad uděluje hello *Přispěvatel virtuálních počítačů* role tooan *Azure AD* na *podsítě*.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-150">hello following example grants hello *Virtual Machine Contributor* role tooan *Azure AD* group on a *subnet*.</span></span>

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a><span data-ttu-id="f2fb0-152">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="f2fb0-152">Remove access</span></span>
<span data-ttu-id="f2fb0-153">tooremove přiřazení role, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-153">tooremove a role assignment, use:</span></span>

    azure role assignment delete --objectId <object id toofrom which tooremove role> --roleName "<role name>"

<span data-ttu-id="f2fb0-154">Hello následující příklad odebere hello *Přispěvatel virtuálních počítačů* přiřazení role z hello  *sammert@aaddemo.com*  uživatele na hello *Pharma. prodej ProjectForcast* Skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-154">hello following example removes hello *Virtual Machine Contributor* role assignment from hello *sammert@aaddemo.com* user on hello *Pharma-Sales-ProjectForcast* resource group.</span></span>
<span data-ttu-id="f2fb0-155">přiřazení role hello Hello příklad pak odebere ze skupiny v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-155">hello example then removes hello role assignment from a group on hello subscription.</span></span>

![RBAC Azure příkazového řádku - azure role přiřazení odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="f2fb0-157">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="f2fb0-157">Create a custom role</span></span>
<span data-ttu-id="f2fb0-158">toocreate vlastní roli, použijte:</span><span class="sxs-lookup"><span data-stu-id="f2fb0-158">toocreate a custom role, use:</span></span>

    azure role create --inputfile <file path>

<span data-ttu-id="f2fb0-159">Hello následující příklad vytvoří vlastní role s názvem *virtuální počítač operátor*.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-159">hello following example creates a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="f2fb0-160">Tato vlastní role uděluje přístup tooall číst operace *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* prostředků poskytovatelů a uděluje přístup toostart, restartování a monitorování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-160">This custom role grants access tooall read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access toostart, restart, and monitor virtual machines.</span></span> <span data-ttu-id="f2fb0-161">Tuto vlastní roli můžete použít ve dvou předplatných.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-161">This custom role can be used in two subscriptions.</span></span> <span data-ttu-id="f2fb0-162">Tento příklad používá soubor JSON jako vstup.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-162">This example uses a JSON file as an input.</span></span>

![JSON – definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure příkazového řádku - azure role vytvořit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a><span data-ttu-id="f2fb0-165">Upravit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="f2fb0-165">Modify a custom role</span></span>
<span data-ttu-id="f2fb0-166">toomodify vlastní role, nejprve použijte hello `azure role show` příkaz tooretrieve definice role.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-166">toomodify a custom role, first use hello `azure role show` command tooretrieve role definition.</span></span> <span data-ttu-id="f2fb0-167">Druhý zkontrolujte soubor definice role toohello požadované změny hello.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-167">Second, make hello desired changes toohello role definition file.</span></span> <span data-ttu-id="f2fb0-168">Nakonec použijte `azure role set` toosave hello změnil definici role.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-168">Finally, use `azure role set` toosave hello modified role definition.</span></span>

    azure role set --inputfile <file path>

<span data-ttu-id="f2fb0-169">Hello následující příklad přidá hello *Microsoft.Insights/diagnosticSettings/* operace toohello **akce**a předplatnému Azure toohello **AssignableScopes**vlastní role virtuálního počítače operátor hello.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-169">hello following example adds hello *Microsoft.Insights/diagnosticSettings/* operation toohello **Actions**, and an Azure subscription toohello **AssignableScopes** of hello Virtual Machine Operator custom role.</span></span>

![JSON - upravit definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Snímek obrazovky příkazového řádku - azure roli set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a><span data-ttu-id="f2fb0-172">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="f2fb0-172">Delete a custom role</span></span>
<span data-ttu-id="f2fb0-173">toodelete vlastní role, nejprve použijte hello `azure role show` příkaz toodetermine hello **ID** hello role.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-173">toodelete a custom role, first use hello `azure role show` command toodetermine hello **ID** of hello role.</span></span> <span data-ttu-id="f2fb0-174">Poté použijte hello `azure role delete` příkaz toodelete hello role zadáním hello **ID**.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-174">Then, use hello `azure role delete` command toodelete hello role by specifying hello **ID**.</span></span>

<span data-ttu-id="f2fb0-175">Hello následující příklad odebere hello *virtuální počítač operátor* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-175">hello following example removes hello *Virtual Machine Operator* custom role.</span></span>

![RBAC Azure příkazového řádku - azure role odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a><span data-ttu-id="f2fb0-177">Vlastní role seznamu</span><span class="sxs-lookup"><span data-stu-id="f2fb0-177">List custom roles</span></span>
<span data-ttu-id="f2fb0-178">toolist hello role, které jsou k dispozici pro přiřazení v oboru, které používají hello `azure role list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-178">toolist hello roles that are available for assignment at a scope, use hello `azure role list` command.</span></span>

<span data-ttu-id="f2fb0-179">Hello následující příkaz vypíše všechny role, které jsou k dispozici pro přiřazení v hello vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-179">hello following command lists all roles that are available for assignment in hello selected subscription.</span></span>

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

<span data-ttu-id="f2fb0-181">V následujícím příkladu hello, hello *virtuální počítač operátor* vlastní role není k dispozici v hello *Production4* předplatné vzhledem k tomu, že toto předplatné není v hello  **AssignableScopes** hello role.</span><span class="sxs-lookup"><span data-stu-id="f2fb0-181">In hello following example, hello *Virtual Machine Operator* custom role isn’t available in hello *Production4* subscription because that subscription isn’t in hello **AssignableScopes** of hello role.</span></span>

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure příkazového řádku - azure role seznam pro vlastní role – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a><span data-ttu-id="f2fb0-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2fb0-183">Next steps</span></span>
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

