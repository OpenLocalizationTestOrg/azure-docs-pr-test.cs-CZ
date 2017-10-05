---
title: "Vytvoření vlastních rolí pro Azure RBAC | Microsoft Docs"
description: "Další informace jak definovat vlastní role pomocí řízení přístupu pro přesnější správu identit ve vašem předplatném Azure."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8e72f2c8095d13c4b6df3c6576bd58806a3c0f2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a><span data-ttu-id="23d61-103">Vytvořit vlastní role pro řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="23d61-103">Create custom roles for Azure Role-Based Access Control</span></span>
<span data-ttu-id="23d61-104">Vytvořte vlastní roli v řízení řízení přístupu (RBAC), pokud žádná z předdefinovaných rolí podle svých potřeb konkrétní přístup.</span><span class="sxs-lookup"><span data-stu-id="23d61-104">Create a custom role in Azure Role-Based Access Control (RBAC) if none of the built-in roles meet your specific access needs.</span></span> <span data-ttu-id="23d61-105">Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](role-based-access-control-manage-access-powershell.md), [rozhraní příkazového řádku Azure](role-based-access-control-manage-access-azure-cli.md) (CLI) a [REST API](role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="23d61-105">Custom roles can be created using [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface](role-based-access-control-manage-access-azure-cli.md) (CLI), and the [REST API](role-based-access-control-manage-access-rest.md).</span></span> <span data-ttu-id="23d61-106">Stejně jako předdefinované role můžete přiřadit vlastní role pro uživatele, skupiny a aplikace na předplatné, skupinu prostředků a prostředků obory.</span><span class="sxs-lookup"><span data-stu-id="23d61-106">Just like built-in roles, you can assign custom roles to users, groups, and applications at subscription, resource group, and resource scopes.</span></span> <span data-ttu-id="23d61-107">Vlastní role jsou uloženy v klient služby Azure AD a mohlo sdílet víc předplatných.</span><span class="sxs-lookup"><span data-stu-id="23d61-107">Custom roles are stored in an Azure AD tenant and can be shared across subscriptions.</span></span>

<span data-ttu-id="23d61-108">Každý klient můžete vytvořit až 2 000 vlastní role.</span><span class="sxs-lookup"><span data-stu-id="23d61-108">Each tenant can create up to 2000 custom roles.</span></span> 

<span data-ttu-id="23d61-109">Následující příklad ukazuje vlastní role pro monitorování a restartování virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="23d61-109">The following example shows a custom role for monitoring and restarting virtual machines:</span></span>

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a><span data-ttu-id="23d61-110">Akce</span><span class="sxs-lookup"><span data-stu-id="23d61-110">Actions</span></span>
<span data-ttu-id="23d61-111">**Akce** vlastnost vlastní role určuje činnosti Azure, ke kterým roli udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="23d61-111">The **Actions** property of a custom role specifies the Azure operations to which the role grants access.</span></span> <span data-ttu-id="23d61-112">Jedná se o kolekci operaci řetězců, které identifikují zabezpečitelné operations poskytovatelů prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="23d61-112">It is a collection of operation strings that identify securable operations of Azure resource providers.</span></span> <span data-ttu-id="23d61-113">Operace řetězce použijte formát `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span><span class="sxs-lookup"><span data-stu-id="23d61-113">Operation strings follow the format of `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span></span> <span data-ttu-id="23d61-114">Operace řetězce, které obsahují zástupné znaky (\*) udělit přístup ke všem operacím, které odpovídají řetězci operace.</span><span class="sxs-lookup"><span data-stu-id="23d61-114">Operation strings that contain wildcards (\*) grant access to all operations that match the operation string.</span></span> <span data-ttu-id="23d61-115">Například:</span><span class="sxs-lookup"><span data-stu-id="23d61-115">For instance:</span></span>

* <span data-ttu-id="23d61-116">`*/read`uděluje přístup ke čtení pro všechny typy prostředků všech poskytovatelů prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="23d61-116">`*/read` grants access to read operations for all resource types of all Azure resource providers.</span></span>
* <span data-ttu-id="23d61-117">`Microsoft.Compute/*`uděluje přístup ke všem operacím pro všechny typy prostředků ve zprostředkovateli prostředků Microsoft.Compute.</span><span class="sxs-lookup"><span data-stu-id="23d61-117">`Microsoft.Compute/*` grants access to all operations for all resource types in the Microsoft.Compute resource provider.</span></span>
* <span data-ttu-id="23d61-118">`Microsoft.Network/*/read`uděluje přístup ke čtení pro všechny typy prostředků v poskytovatel prostředků Microsoft.Network Azure.</span><span class="sxs-lookup"><span data-stu-id="23d61-118">`Microsoft.Network/*/read` grants access to read operations for all resource types in the Microsoft.Network resource provider of Azure.</span></span>
* <span data-ttu-id="23d61-119">`Microsoft.Compute/virtualMachines/*`uděluje přístup ke všem operacím virtuálních počítačů a jeho podřízené typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="23d61-119">`Microsoft.Compute/virtualMachines/*` grants access to all operations of virtual machines and its child resource types.</span></span>
* <span data-ttu-id="23d61-120">`Microsoft.Web/sites/restart/Action`uděluje přístup k restartování weby.</span><span class="sxs-lookup"><span data-stu-id="23d61-120">`Microsoft.Web/sites/restart/Action` grants access to restart websites.</span></span>

<span data-ttu-id="23d61-121">Použití `Get-AzureRmProviderOperation` (v prostředí PowerShell) nebo `azure provider operations show` (v Azure CLI) k operacím seznamu zprostředkovatelů prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="23d61-121">Use `Get-AzureRmProviderOperation` (in PowerShell) or `azure provider operations show` (in Azure CLI) to list operations of Azure resource providers.</span></span> <span data-ttu-id="23d61-122">Ověřte, zda je řetězec v operaci platný a rozbalte řetězce operaci zástupný znak, může pomocí následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="23d61-122">You may also use these commands to verify that an operation string is valid, and to expand wildcard operation strings.</span></span>

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Snímek obrazovky prostředí PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![<span data-ttu-id="23d61-124">Azure CLI – snímek obrazovky - azure zprostředkovatele operations zobrazit "Microsoft.Compute/virtualMachines/\*/action"</span><span class="sxs-lookup"><span data-stu-id="23d61-124">Azure CLI screenshot - azure provider operations show "Microsoft.Compute/virtualMachines/\*/action"</span></span> ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a><span data-ttu-id="23d61-125">NotActions</span><span class="sxs-lookup"><span data-stu-id="23d61-125">NotActions</span></span>
<span data-ttu-id="23d61-126">Použití **NotActions** vlastnost Pokud sadu operací, které chcete povolit je snadno definovanou s výjimkou operací s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="23d61-126">Use the **NotActions** property if the set of operations that you wish to allow is more easily defined by excluding restricted operations.</span></span> <span data-ttu-id="23d61-127">Udělení přístupu podle vlastní role se počítá odečtením **NotActions** operací **akce** operace.</span><span class="sxs-lookup"><span data-stu-id="23d61-127">The access granted by a custom role is computed by subtracting the **NotActions** operations from the **Actions** operations.</span></span>

> [!NOTE]
> <span data-ttu-id="23d61-128">Pokud je uživatel přiřazenou roli, která nezahrnuje operace v **NotActions**a je mu přiřazená druhá role, který uděluje přístup ke stejné operace, má uživatel k provedení této operace.</span><span class="sxs-lookup"><span data-stu-id="23d61-128">If a user is assigned a role that excludes an operation in **NotActions**, and is assigned a second role that grants access to the same operation, the user is allowed to perform that operation.</span></span> <span data-ttu-id="23d61-129">**NotActions** není odepření pravidlo – je jenom pohodlný způsob, jak vytvořit sadu povolených operací při konkrétních operací muset vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="23d61-129">**NotActions** is not a deny rule – it is simply a convenient way to create a set of allowed operations when specific operations need to be excluded.</span></span>
>
>

## <a name="assignablescopes"></a><span data-ttu-id="23d61-130">AssignableScopes</span><span class="sxs-lookup"><span data-stu-id="23d61-130">AssignableScopes</span></span>
<span data-ttu-id="23d61-131">**AssignableScopes** vlastnost vlastní role určuje obory (odběry, skupiny prostředků nebo prostředky), ve které je k dispozici pro přiřazení vlastní role.</span><span class="sxs-lookup"><span data-stu-id="23d61-131">The **AssignableScopes** property of the custom role specifies the scopes (subscriptions, resource groups, or resources) within which the custom role is available for assignment.</span></span> <span data-ttu-id="23d61-132">Můžete zpřístupnit vlastní role pro přiřazení v jenom na předplatná nebo skupiny prostředků, které je vyžadují a není zbytečných souborů činnost koncového uživatele pro zbytek předplatná nebo skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="23d61-132">You can make the custom role available for assignment in only the subscriptions or resource groups that require it, and not clutter user experience for the rest of the subscriptions or resource groups.</span></span>

<span data-ttu-id="23d61-133">Příklady platný přiřaditelnými obory:</span><span class="sxs-lookup"><span data-stu-id="23d61-133">Examples of valid assignable scopes include:</span></span>

* <span data-ttu-id="23d61-134">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - zpřístupní role pro přiřazení do dvou předplatných.</span><span class="sxs-lookup"><span data-stu-id="23d61-134">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - makes the role available for assignment in two subscriptions.</span></span>
* <span data-ttu-id="23d61-135">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - zpřístupní role pro přiřazení v rámci jednoho předplatného.</span><span class="sxs-lookup"><span data-stu-id="23d61-135">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - makes the role available for assignment in a single subscription.</span></span>
* <span data-ttu-id="23d61-136">"/ odběry nebo c276fc76-9cd4-44c9-99a7-4fd71546436e/Skupinyprostředků/síťových" - je k dispozici pro přiřazení role pouze ve skupině prostředků sítě.</span><span class="sxs-lookup"><span data-stu-id="23d61-136">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network” - makes the role available for assignment only in the Network resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="23d61-137">Musíte použít aspoň jeden předplatné, skupinu prostředků nebo ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="23d61-137">You must use at least one subscription, resource group, or resource ID.</span></span>
>
>

## <a name="custom-roles-access-control"></a><span data-ttu-id="23d61-138">Vlastní role řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="23d61-138">Custom roles access control</span></span>
<span data-ttu-id="23d61-139">**AssignableScopes** vlastnost vlastní role také určuje, kdo může zobrazit, upravit a odstranit roli.</span><span class="sxs-lookup"><span data-stu-id="23d61-139">The **AssignableScopes** property of the custom role also controls who can view, modify, and delete the role.</span></span>

* <span data-ttu-id="23d61-140">Kdo mohou vytvářet vlastní role?</span><span class="sxs-lookup"><span data-stu-id="23d61-140">Who can create a custom role?</span></span>
    <span data-ttu-id="23d61-141">Vlastníci (a správcům přístup uživatele) předplatných, skupiny prostředků a prostředků můžete vytvořit vlastní role pro použití v tyto obory.</span><span class="sxs-lookup"><span data-stu-id="23d61-141">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can create custom roles for use in those scopes.</span></span>
    <span data-ttu-id="23d61-142">Vytvoření role uživatele musí být schopni provést `Microsoft.Authorization/roleDefinition/write` operaci na všech **AssignableScopes** role.</span><span class="sxs-lookup"><span data-stu-id="23d61-142">The user creating the role needs to be able to perform `Microsoft.Authorization/roleDefinition/write` operation on all the **AssignableScopes** of the role.</span></span>
* <span data-ttu-id="23d61-143">Kdo může upravit vlastní roli?</span><span class="sxs-lookup"><span data-stu-id="23d61-143">Who can modify a custom role?</span></span>
    <span data-ttu-id="23d61-144">Vlastníci (a správcům přístup uživatele) předplatných, skupiny prostředků a prostředků můžete upravit vlastní role v tyto obory.</span><span class="sxs-lookup"><span data-stu-id="23d61-144">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can modify custom roles in those scopes.</span></span> <span data-ttu-id="23d61-145">Uživatelé musí být schopni provést `Microsoft.Authorization/roleDefinition/write` operaci na všech **AssignableScopes** vlastní role.</span><span class="sxs-lookup"><span data-stu-id="23d61-145">Users need to be able to perform the `Microsoft.Authorization/roleDefinition/write` operation on all the **AssignableScopes** of a custom role.</span></span>
* <span data-ttu-id="23d61-146">Kdo může zobrazit vlastní role?</span><span class="sxs-lookup"><span data-stu-id="23d61-146">Who can view custom roles?</span></span>
    <span data-ttu-id="23d61-147">Všechny předdefinované role v Azure RBAC povolí zobrazení rolí, které jsou k dispozici pro přiřazení.</span><span class="sxs-lookup"><span data-stu-id="23d61-147">All built-in roles in Azure RBAC allow viewing of roles that are available for assignment.</span></span> <span data-ttu-id="23d61-148">Uživatelé, kteří mohou provádět `Microsoft.Authorization/roleDefinition/read` operace v oboru můžete zobrazit role RBAC, které jsou k dispozici pro přiřazení v tomto oboru.</span><span class="sxs-lookup"><span data-stu-id="23d61-148">Users who can perform the `Microsoft.Authorization/roleDefinition/read` operation at a scope can view the RBAC roles that are available for assignment at that scope.</span></span>

## <a name="see-also"></a><span data-ttu-id="23d61-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="23d61-149">See also</span></span>
* <span data-ttu-id="23d61-150">[Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="23d61-150">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in the Azure portal.</span></span>
* <span data-ttu-id="23d61-151">Zjistěte, jak spravovat přístup pomocí:</span><span class="sxs-lookup"><span data-stu-id="23d61-151">Learn how to manage access with:</span></span>
  * [<span data-ttu-id="23d61-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="23d61-152">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
  * [<span data-ttu-id="23d61-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="23d61-153">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
  * [<span data-ttu-id="23d61-154">REST API</span><span class="sxs-lookup"><span data-stu-id="23d61-154">REST API</span></span>](role-based-access-control-manage-access-rest.md)
* <span data-ttu-id="23d61-155">[Předdefinované role](role-based-access-built-in-roles.md): získat tak podrobné údaje o rolích, které jsou v RBAC standardní.</span><span class="sxs-lookup"><span data-stu-id="23d61-155">[Built-in roles](role-based-access-built-in-roles.md): Get details about the roles that come standard in RBAC.</span></span>
