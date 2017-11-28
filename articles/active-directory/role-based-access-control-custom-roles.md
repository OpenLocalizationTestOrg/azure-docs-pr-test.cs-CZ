---
title: "vlastní role aaaCreate pro Azure RBAC | Microsoft Docs"
description: "Zjistěte, jak toodefine vlastní role pomocí řízení přístupu pro přesnější správu identit ve vašem předplatném Azure."
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
ms.openlocfilehash: 60df12632ef6c086d5feeb1809196d7c4ee5e021
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a><span data-ttu-id="92863-103">Vytvořit vlastní role pro řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="92863-103">Create custom roles for Azure Role-Based Access Control</span></span>
<span data-ttu-id="92863-104">Vytvořte vlastní roli v řízení řízení přístupu (RBAC), pokud žádná z předdefinovaných rolí hello podle svých potřeb konkrétní přístup.</span><span class="sxs-lookup"><span data-stu-id="92863-104">Create a custom role in Azure Role-Based Access Control (RBAC) if none of hello built-in roles meet your specific access needs.</span></span> <span data-ttu-id="92863-105">Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](role-based-access-control-manage-access-powershell.md), [rozhraní příkazového řádku Azure](role-based-access-control-manage-access-azure-cli.md) (CLI) a hello [REST API](role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="92863-105">Custom roles can be created using [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface](role-based-access-control-manage-access-azure-cli.md) (CLI), and hello [REST API](role-based-access-control-manage-access-rest.md).</span></span> <span data-ttu-id="92863-106">Stejně jako předdefinované role můžete přiřadit toousers vlastní role, skupiny a aplikace na předplatné, skupinu prostředků a prostředků obory.</span><span class="sxs-lookup"><span data-stu-id="92863-106">Just like built-in roles, you can assign custom roles toousers, groups, and applications at subscription, resource group, and resource scopes.</span></span> <span data-ttu-id="92863-107">Vlastní role jsou uloženy v klient služby Azure AD a mohlo sdílet víc předplatných.</span><span class="sxs-lookup"><span data-stu-id="92863-107">Custom roles are stored in an Azure AD tenant and can be shared across subscriptions.</span></span>

<span data-ttu-id="92863-108">Každý klient můžete vytvořit až too2000 vlastní role.</span><span class="sxs-lookup"><span data-stu-id="92863-108">Each tenant can create up too2000 custom roles.</span></span> 

<span data-ttu-id="92863-109">Hello následující příklad ukazuje vlastní role pro monitorování a restartování virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="92863-109">hello following example shows a custom role for monitoring and restarting virtual machines:</span></span>

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
## <a name="actions"></a><span data-ttu-id="92863-110">Akce</span><span class="sxs-lookup"><span data-stu-id="92863-110">Actions</span></span>
<span data-ttu-id="92863-111">Hello **akce** vlastnost vlastní role určuje hello Azure operations toowhich hello role uděluje přístup.</span><span class="sxs-lookup"><span data-stu-id="92863-111">hello **Actions** property of a custom role specifies hello Azure operations toowhich hello role grants access.</span></span> <span data-ttu-id="92863-112">Jedná se o kolekci operaci řetězců, které identifikují zabezpečitelné operations poskytovatelů prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="92863-112">It is a collection of operation strings that identify securable operations of Azure resource providers.</span></span> <span data-ttu-id="92863-113">Operace řetězce podle hello formát `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span><span class="sxs-lookup"><span data-stu-id="92863-113">Operation strings follow hello format of `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span></span> <span data-ttu-id="92863-114">Operace řetězce, které obsahují zástupné znaky (\*) udělit přístup tooall operace, které odpovídají řetězci operace hello.</span><span class="sxs-lookup"><span data-stu-id="92863-114">Operation strings that contain wildcards (\*) grant access tooall operations that match hello operation string.</span></span> <span data-ttu-id="92863-115">Například:</span><span class="sxs-lookup"><span data-stu-id="92863-115">For instance:</span></span>

* <span data-ttu-id="92863-116">`*/read`uděluje přístup k tooread operací pro všechny typy prostředků všech poskytovatelů prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="92863-116">`*/read` grants access tooread operations for all resource types of all Azure resource providers.</span></span>
* <span data-ttu-id="92863-117">`Microsoft.Compute/*`uděluje přístup k tooall operací pro všechny typy prostředků v hello zprostředkovateli prostředků Microsoft.Compute.</span><span class="sxs-lookup"><span data-stu-id="92863-117">`Microsoft.Compute/*` grants access tooall operations for all resource types in hello Microsoft.Compute resource provider.</span></span>
* <span data-ttu-id="92863-118">`Microsoft.Network/*/read`uděluje přístup k tooread operací pro všechny typy prostředků v poskytovatel prostředků Microsoft.Network hello Azure.</span><span class="sxs-lookup"><span data-stu-id="92863-118">`Microsoft.Network/*/read` grants access tooread operations for all resource types in hello Microsoft.Network resource provider of Azure.</span></span>
* <span data-ttu-id="92863-119">`Microsoft.Compute/virtualMachines/*`uděluje přístup k tooall operace virtuálních počítačů a jeho podřízené typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="92863-119">`Microsoft.Compute/virtualMachines/*` grants access tooall operations of virtual machines and its child resource types.</span></span>
* <span data-ttu-id="92863-120">`Microsoft.Web/sites/restart/Action`uděluje přístup k webům toorestart.</span><span class="sxs-lookup"><span data-stu-id="92863-120">`Microsoft.Web/sites/restart/Action` grants access toorestart websites.</span></span>

<span data-ttu-id="92863-121">Použití `Get-AzureRmProviderOperation` (v prostředí PowerShell) nebo `azure provider operations show` (v Azure CLI) toolist operations poskytovatelů prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="92863-121">Use `Get-AzureRmProviderOperation` (in PowerShell) or `azure provider operations show` (in Azure CLI) toolist operations of Azure resource providers.</span></span> <span data-ttu-id="92863-122">Můžete také používat tyto příkazy tooverify, zda je řetězec v operaci platný a tooexpand zástupný znak operaci řetězce.</span><span class="sxs-lookup"><span data-stu-id="92863-122">You may also use these commands tooverify that an operation string is valid, and tooexpand wildcard operation strings.</span></span>

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Snímek obrazovky prostředí PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![<span data-ttu-id="92863-124">Azure CLI – snímek obrazovky - azure zprostředkovatele operations zobrazit "Microsoft.Compute/virtualMachines/\*/action"</span><span class="sxs-lookup"><span data-stu-id="92863-124">Azure CLI screenshot - azure provider operations show "Microsoft.Compute/virtualMachines/\*/action"</span></span> ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a><span data-ttu-id="92863-125">NotActions</span><span class="sxs-lookup"><span data-stu-id="92863-125">NotActions</span></span>
<span data-ttu-id="92863-126">Použití hello **NotActions** vlastnost Pokud hello sadu operací, které chcete tooallow je snadno definovanou s výjimkou operací s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="92863-126">Use hello **NotActions** property if hello set of operations that you wish tooallow is more easily defined by excluding restricted operations.</span></span> <span data-ttu-id="92863-127">Hello udělení přístupu podle vlastní role se počítá odečtením hello **NotActions** operací z hello **akce** operace.</span><span class="sxs-lookup"><span data-stu-id="92863-127">hello access granted by a custom role is computed by subtracting hello **NotActions** operations from hello **Actions** operations.</span></span>

> [!NOTE]
> <span data-ttu-id="92863-128">Pokud je uživatel přiřazenou roli, která nezahrnuje operace v **NotActions**a je mu přiřazená druhá role, která uděluje přístup toohello stejné operace, hello uživatele je povoleno tooperform tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="92863-128">If a user is assigned a role that excludes an operation in **NotActions**, and is assigned a second role that grants access toohello same operation, hello user is allowed tooperform that operation.</span></span> <span data-ttu-id="92863-129">**NotActions** není odepření pravidlo – je jednoduše to pohodlný způsob toocreate sadu povolených operací při konkrétních operací potřebovat toobe vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="92863-129">**NotActions** is not a deny rule – it is simply a convenient way toocreate a set of allowed operations when specific operations need toobe excluded.</span></span>
>
>

## <a name="assignablescopes"></a><span data-ttu-id="92863-130">AssignableScopes</span><span class="sxs-lookup"><span data-stu-id="92863-130">AssignableScopes</span></span>
<span data-ttu-id="92863-131">Hello **AssignableScopes** vlastnost hello vlastní role určuje hello obory (odběry, skupiny prostředků nebo prostředky) v rámci které hello vlastní role je k dispozici pro přiřazení.</span><span class="sxs-lookup"><span data-stu-id="92863-131">hello **AssignableScopes** property of hello custom role specifies hello scopes (subscriptions, resource groups, or resources) within which hello custom role is available for assignment.</span></span> <span data-ttu-id="92863-132">Můžete vytvořit vlastní role hello k dispozici pro přiřazení v pouze hello předplatných nebo skupiny prostředků, které vyžadují a ne zbytečné soubory uživatele prostředí pro zbytek hello předplatná hello nebo skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="92863-132">You can make hello custom role available for assignment in only hello subscriptions or resource groups that require it, and not clutter user experience for hello rest of hello subscriptions or resource groups.</span></span>

<span data-ttu-id="92863-133">Příklady platný přiřaditelnými obory:</span><span class="sxs-lookup"><span data-stu-id="92863-133">Examples of valid assignable scopes include:</span></span>

* <span data-ttu-id="92863-134">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - zpřístupní hello role pro přiřazení do dvou předplatných.</span><span class="sxs-lookup"><span data-stu-id="92863-134">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - makes hello role available for assignment in two subscriptions.</span></span>
* <span data-ttu-id="92863-135">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - zpřístupní hello role pro přiřazení v rámci jednoho předplatného.</span><span class="sxs-lookup"><span data-stu-id="92863-135">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - makes hello role available for assignment in a single subscription.</span></span>
* <span data-ttu-id="92863-136">"/ odběry nebo c276fc76-9cd4-44c9-99a7-4fd71546436e/Skupinyprostředků/síťových" - Díky hello role, které jsou k dispozici pro přiřazení pouze ve skupině prostředků sítě hello.</span><span class="sxs-lookup"><span data-stu-id="92863-136">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network” - makes hello role available for assignment only in hello Network resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="92863-137">Musíte použít aspoň jeden předplatné, skupinu prostředků nebo ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="92863-137">You must use at least one subscription, resource group, or resource ID.</span></span>
>
>

## <a name="custom-roles-access-control"></a><span data-ttu-id="92863-138">Vlastní role řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="92863-138">Custom roles access control</span></span>
<span data-ttu-id="92863-139">Hello **AssignableScopes** vlastnost hello vlastní role také určuje, kdo může zobrazit, upravit a odstranit roli hello.</span><span class="sxs-lookup"><span data-stu-id="92863-139">hello **AssignableScopes** property of hello custom role also controls who can view, modify, and delete hello role.</span></span>

* <span data-ttu-id="92863-140">Kdo mohou vytvářet vlastní role?</span><span class="sxs-lookup"><span data-stu-id="92863-140">Who can create a custom role?</span></span>
    <span data-ttu-id="92863-141">Vlastníci (a správcům přístup uživatele) předplatných, skupiny prostředků a prostředků můžete vytvořit vlastní role pro použití v tyto obory.</span><span class="sxs-lookup"><span data-stu-id="92863-141">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can create custom roles for use in those scopes.</span></span>
    <span data-ttu-id="92863-142">Hello uživatel vytváření hello role musí mít tooperform toobe `Microsoft.Authorization/roleDefinition/write` operace na všechny hello **AssignableScopes** hello role.</span><span class="sxs-lookup"><span data-stu-id="92863-142">hello user creating hello role needs toobe able tooperform `Microsoft.Authorization/roleDefinition/write` operation on all hello **AssignableScopes** of hello role.</span></span>
* <span data-ttu-id="92863-143">Kdo může upravit vlastní roli?</span><span class="sxs-lookup"><span data-stu-id="92863-143">Who can modify a custom role?</span></span>
    <span data-ttu-id="92863-144">Vlastníci (a správcům přístup uživatele) předplatných, skupiny prostředků a prostředků můžete upravit vlastní role v tyto obory.</span><span class="sxs-lookup"><span data-stu-id="92863-144">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can modify custom roles in those scopes.</span></span> <span data-ttu-id="92863-145">Uživatelé potřebují hello možné tooperform toobe `Microsoft.Authorization/roleDefinition/write` operace na všechny hello **AssignableScopes** vlastní role.</span><span class="sxs-lookup"><span data-stu-id="92863-145">Users need toobe able tooperform hello `Microsoft.Authorization/roleDefinition/write` operation on all hello **AssignableScopes** of a custom role.</span></span>
* <span data-ttu-id="92863-146">Kdo může zobrazit vlastní role?</span><span class="sxs-lookup"><span data-stu-id="92863-146">Who can view custom roles?</span></span>
    <span data-ttu-id="92863-147">Všechny předdefinované role v Azure RBAC povolí zobrazení rolí, které jsou k dispozici pro přiřazení.</span><span class="sxs-lookup"><span data-stu-id="92863-147">All built-in roles in Azure RBAC allow viewing of roles that are available for assignment.</span></span> <span data-ttu-id="92863-148">Uživatelé, kteří mohou provádět hello `Microsoft.Authorization/roleDefinition/read` operace v oboru můžete zobrazit role RBAC hello, které jsou k dispozici pro přiřazení v tomto oboru.</span><span class="sxs-lookup"><span data-stu-id="92863-148">Users who can perform hello `Microsoft.Authorization/roleDefinition/read` operation at a scope can view hello RBAC roles that are available for assignment at that scope.</span></span>

## <a name="see-also"></a><span data-ttu-id="92863-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="92863-149">See also</span></span>
* <span data-ttu-id="92863-150">[Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="92863-150">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in hello Azure portal.</span></span>
* <span data-ttu-id="92863-151">Zjistěte, jak přistupovat k toomanage se:</span><span class="sxs-lookup"><span data-stu-id="92863-151">Learn how toomanage access with:</span></span>
  * [<span data-ttu-id="92863-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="92863-152">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
  * [<span data-ttu-id="92863-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="92863-153">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
  * [<span data-ttu-id="92863-154">REST API</span><span class="sxs-lookup"><span data-stu-id="92863-154">REST API</span></span>](role-based-access-control-manage-access-rest.md)
* <span data-ttu-id="92863-155">[Předdefinované role](role-based-access-built-in-roles.md): získání podrobností o hello rolí, které jsou v RBAC standardní.</span><span class="sxs-lookup"><span data-stu-id="92863-155">[Built-in roles](role-based-access-built-in-roles.md): Get details about hello roles that come standard in RBAC.</span></span>
