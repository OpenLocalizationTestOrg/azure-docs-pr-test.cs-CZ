---
title: "Zásady prostředků Azure | Microsoft Docs"
description: "Popisuje, jak pomocí Azure Resource Manager zásad na Ujistěte se, že jsou nastaveny vlastnosti konzistentní prostředku během nasazení. Zásady můžete použít na předplatné nebo prostředek skupiny."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: tomfitz
ms.openlocfilehash: 0ee2624f45a1de0c23cae4538a38ae3e302eedd3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="resource-policy-overview"></a><span data-ttu-id="5c7b1-104">Přehled zásad prostředků</span><span class="sxs-lookup"><span data-stu-id="5c7b1-104">Resource policy overview</span></span>
<span data-ttu-id="5c7b1-105">Zásady prostředků umožňují vytvořit konvence pro prostředky ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-105">Resource policies enable you to establish conventions for resources in your organization.</span></span> <span data-ttu-id="5c7b1-106">Definováním konvence můžete řídit náklady a snadněji spravovat vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-106">By defining conventions, you can control costs and more easily manage your resources.</span></span> <span data-ttu-id="5c7b1-107">Například můžete zadat, že jsou povoleny pouze určité typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-107">For example, you can specify that only certain types of virtual machines are allowed.</span></span> <span data-ttu-id="5c7b1-108">Nebo můžete vyžadovat, aby všechny prostředky měli konkrétní značku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-108">Or, you can require that all resources have a particular tag.</span></span> <span data-ttu-id="5c7b1-109">Zásady jsou zdědí všechny podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-109">Policies are inherited by all child resources.</span></span> <span data-ttu-id="5c7b1-110">Ano Pokud je zásada pro skupinu prostředků, se vztahuje na všechny prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-110">So, if a policy is applied to a resource group, it is applicable to all the resources in that resource group.</span></span>

<span data-ttu-id="5c7b1-111">Existují dvě koncepty týkající se zásad:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-111">There are two concepts to understand about policies:</span></span>

* <span data-ttu-id="5c7b1-112">Definice zásad - popisují když je tato zásada vynucená a jaká opatření se mají provést</span><span class="sxs-lookup"><span data-stu-id="5c7b1-112">policy definition - you describe when the policy is enforced and what action to take</span></span>
* <span data-ttu-id="5c7b1-113">přiřazení zásady - použít definice zásady oboru (předplatné nebo skupinu prostředků)</span><span class="sxs-lookup"><span data-stu-id="5c7b1-113">policy assignment - you apply the policy definition to a scope (subscription or resource group)</span></span>

<span data-ttu-id="5c7b1-114">Toto téma se zaměřuje na definice zásady.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-114">This topic focuses on policy definition.</span></span> <span data-ttu-id="5c7b1-115">Informace o přiřazení zásad najdete v tématu [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md) nebo [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-115">For information about policy assignment, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md) or [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>

<span data-ttu-id="5c7b1-116">Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-116">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

> [!NOTE]
> <span data-ttu-id="5c7b1-117">Zásady v současné době nelze vyhodnotit typů prostředků, které nepodporují značky, typ a umístění, jako například Microsoft.Resources/deployments typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-117">Currently, policy does not evaluate resource types that do not support tags, kind, and location, such as the Microsoft.Resources/deployments resource type.</span></span> <span data-ttu-id="5c7b1-118">Tato podpora se přidá na datum v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-118">This support will be added at a future time.</span></span> <span data-ttu-id="5c7b1-119">Abyste předešli problémům s zpětné kompatibility, byste měli explicitně zadat typ, při vytváření zásad.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-119">To avoid backward compatibility issues, you should explicitly specify type when authoring policies.</span></span> <span data-ttu-id="5c7b1-120">Například značky zásadu, která neurčuje typy platí pro všechny typy.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-120">For example, a tag policy that does not specify types is applied for all types.</span></span> <span data-ttu-id="5c7b1-121">V takovém případě šablony nasazení může selhat, pokud je vnořeného prostředku, který nepodporuje značky a typ nasazení prostředku byla přidaná k vyhodnocení zásad.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-121">In that case, a template deployment may fail if there is a nested resource that doesn't support tags, and the deployment resource type has been added to policy evaluation.</span></span> 
> 
> 

## <a name="how-is-it-different-from-rbac"></a><span data-ttu-id="5c7b1-122">Jak se liší od RBAC?</span><span class="sxs-lookup"><span data-stu-id="5c7b1-122">How is it different from RBAC?</span></span>
<span data-ttu-id="5c7b1-123">Existuje několik hlavní rozdíly mezi zásadami a řízení přístupu na základě role (RBAC).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-123">There are a few key differences between policy and role-based access control (RBAC).</span></span> <span data-ttu-id="5c7b1-124">RBAC se zaměřuje na **uživatele** akce na různých místech.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-124">RBAC focuses on **user** actions at different scopes.</span></span> <span data-ttu-id="5c7b1-125">Například jste přidáni do role Přispěvatel pro skupinu prostředků na požadovaný rozsah, umožní vám provádět změny do této skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-125">For example, you are added to the contributor role for a resource group at the desired scope, so you can make changes to that resource group.</span></span> <span data-ttu-id="5c7b1-126">Zásady se zaměřuje na **prostředků** vlastnosti během nasazení.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-126">Policy focuses on **resource** properties during deployment.</span></span> <span data-ttu-id="5c7b1-127">Například prostřednictvím zásad, můžete řídit typy prostředků, které mohou být zřízeny.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-127">For example, through policies, you can control the types of resources that can be provisioned.</span></span> <span data-ttu-id="5c7b1-128">Nebo můžete omezit umístění, ve kterých se dá zřídit prostředky.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-128">Or, you can restrict the locations in which the resources can be provisioned.</span></span> <span data-ttu-id="5c7b1-129">Na rozdíl od RBAC, je povolit výchozí zásady a explicitní odepřít systému.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-129">Unlike RBAC, policy is a default allow and explicit deny system.</span></span> 

<span data-ttu-id="5c7b1-130">Pokud chcete používat zásady, musí být ověřeny pomocí RBAC.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-130">To use policies, you must be authenticated through RBAC.</span></span> <span data-ttu-id="5c7b1-131">Konkrétně, musí váš účet:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-131">Specifically, your account needs the:</span></span>

* <span data-ttu-id="5c7b1-132">`Microsoft.Authorization/policydefinitions/write`oprávnění k definování zásad</span><span class="sxs-lookup"><span data-stu-id="5c7b1-132">`Microsoft.Authorization/policydefinitions/write` permission to define a policy</span></span>
* <span data-ttu-id="5c7b1-133">`Microsoft.Authorization/policyassignments/write`oprávnění k přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="5c7b1-133">`Microsoft.Authorization/policyassignments/write` permission to assign a policy</span></span> 

<span data-ttu-id="5c7b1-134">Tato oprávnění nejsou součástí **Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-134">These permissions are not included in the **Contributor** role.</span></span>

## <a name="built-in-policies"></a><span data-ttu-id="5c7b1-135">Předdefinované zásady</span><span class="sxs-lookup"><span data-stu-id="5c7b1-135">Built-in policies</span></span>

<span data-ttu-id="5c7b1-136">Azure poskytuje definice předdefinované zásady, které může snížit počet zásad, které budete muset definovat.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-136">Azure provides some built-in policy definitions that may reduce the number of policies you have to define.</span></span> <span data-ttu-id="5c7b1-137">Před pokračováním definice zásady, byste měli zvážit, zda předdefinovaných zásad již obsahuje definice, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-137">Before proceeding with policy definitions, you should consider whether a built-in policy already provides the definition you need.</span></span> <span data-ttu-id="5c7b1-138">Definice předdefinovaných zásad jsou:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-138">The built-in policy definitions are:</span></span>

* <span data-ttu-id="5c7b1-139">Povolených umístění</span><span class="sxs-lookup"><span data-stu-id="5c7b1-139">Allowed locations</span></span>
* <span data-ttu-id="5c7b1-140">Typy povolené prostředků</span><span class="sxs-lookup"><span data-stu-id="5c7b1-140">Allowed resource types</span></span>
* <span data-ttu-id="5c7b1-141">Povolené účet úložiště SKU</span><span class="sxs-lookup"><span data-stu-id="5c7b1-141">Allowed storage account SKUs</span></span>
* <span data-ttu-id="5c7b1-142">Povolená SKU virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5c7b1-142">Allowed virtual machine SKUs</span></span>
* <span data-ttu-id="5c7b1-143">Použít značky a výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="5c7b1-143">Apply tag and default value</span></span>
* <span data-ttu-id="5c7b1-144">Vynutit značky a hodnota</span><span class="sxs-lookup"><span data-stu-id="5c7b1-144">Enforce tag and value</span></span>
* <span data-ttu-id="5c7b1-145">Není povoleno typy prostředků</span><span class="sxs-lookup"><span data-stu-id="5c7b1-145">Not allowed resource types</span></span>
* <span data-ttu-id="5c7b1-146">Vyžaduje systém SQL Server verze 12.0</span><span class="sxs-lookup"><span data-stu-id="5c7b1-146">Require SQL Server version 12.0</span></span>
* <span data-ttu-id="5c7b1-147">Vyžadovat šifrování účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="5c7b1-147">Require storage account encryption</span></span>

<span data-ttu-id="5c7b1-148">Můžete přiřadit některé z těchto zásad prostřednictvím [portál](resource-manager-policy-portal.md), [prostředí PowerShell](resource-manager-policy-create-assign.md#powershell), nebo [rozhraní příkazového řádku Azure](resource-manager-policy-create-assign.md#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-148">You can assign any of these policies through the [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), or [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span></span>

## <a name="policy-definition-structure"></a><span data-ttu-id="5c7b1-149">Definice strukturu zásad.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-149">Policy definition structure</span></span>
<span data-ttu-id="5c7b1-150">JSON použijete k vytvoření definice zásady.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-150">You use JSON to create a policy definition.</span></span> <span data-ttu-id="5c7b1-151">Definice zásad obsahuje prvky pro:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-151">The policy definition contains elements for:</span></span>

* <span data-ttu-id="5c7b1-152">Parametry</span><span class="sxs-lookup"><span data-stu-id="5c7b1-152">parameters</span></span>
* <span data-ttu-id="5c7b1-153">Zobrazovaný název</span><span class="sxs-lookup"><span data-stu-id="5c7b1-153">display name</span></span>
* <span data-ttu-id="5c7b1-154">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-154">description</span></span>
* <span data-ttu-id="5c7b1-155">Pravidlo zásad</span><span class="sxs-lookup"><span data-stu-id="5c7b1-155">policy rule</span></span>
  * <span data-ttu-id="5c7b1-156">logické vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="5c7b1-156">logical evaluation</span></span>
  * <span data-ttu-id="5c7b1-157">Platnost</span><span class="sxs-lookup"><span data-stu-id="5c7b1-157">effect</span></span>

<span data-ttu-id="5c7b1-158">Následující příklad ukazuje zásadu, která omezuje, kde jsou nasazené prostředky:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-158">The following example shows a policy that limits where resources are deployed:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="parameters"></a><span data-ttu-id="5c7b1-159">Parametry</span><span class="sxs-lookup"><span data-stu-id="5c7b1-159">Parameters</span></span>
<span data-ttu-id="5c7b1-160">Pomocí parametrů pomáhá zjednodušit vaší zásady správy, protože snižuje počet definice zásady.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-160">Using parameters helps simplify your policy management by reducing the number of policy definitions.</span></span> <span data-ttu-id="5c7b1-161">Můžete definovat zásady pro vlastnost prostředku (například omezení umístění, kde můžete nasadit prostředky) a zahrnout parametry v definici.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-161">You define a policy for a resource property (such as limiting the locations where resources can be deployed), and include parameters in the definition.</span></span> <span data-ttu-id="5c7b1-162">Pak můžete znovu použít, aby definice zásady pro různé scénáře předávání různé hodnoty (například určení jednu sadu umístění pro předplatné) při přiřazování zásady.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-162">Then, you reuse that policy definition for different scenarios by passing in different values (such as specifying one set of locations for a subscription) when assigning the policy.</span></span>

<span data-ttu-id="5c7b1-163">Parametry deklarovat, při vytváření definice zásad.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-163">You declare parameters when you create policy definitions.</span></span>

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

<span data-ttu-id="5c7b1-164">Typ parametru může být řetězec nebo pole.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-164">The type of a parameter can be either string or array.</span></span> <span data-ttu-id="5c7b1-165">Vlastnost metadat slouží k nástroje, například portál Azure a zobrazte uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-165">The metadata property is used for tools like Azure portal to display user-friendly information.</span></span> 

<span data-ttu-id="5c7b1-166">V pravidle zásady můžete odkazovat na parametry s následující syntaxí:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-166">In the policy rule, you reference parameters with the following syntax:</span></span> 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a><span data-ttu-id="5c7b1-167">Zobrazovaný název a popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-167">Display name and description</span></span>

<span data-ttu-id="5c7b1-168">Můžete použít **displayName** a **popis** identifikovat definice zásady a poskytují kontext pro při použití.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-168">You use the **displayName** and **description** to identify the policy definition, and provide context for when it is used.</span></span>

## <a name="policy-rule"></a><span data-ttu-id="5c7b1-169">Pravidlo zásad</span><span class="sxs-lookup"><span data-stu-id="5c7b1-169">Policy rule</span></span>

<span data-ttu-id="5c7b1-170">Pravidla zásad se skládá z **Pokud** a **pak** bloky.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-170">The policy rule consists of **If** and **Then** blocks.</span></span> <span data-ttu-id="5c7b1-171">V **Pokud** blok, můžete definovat jednu nebo víc podmínek, které určují, kdy je tato zásada vynucená.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-171">In the **If** block, you define one or more conditions that specify when the policy is enforced.</span></span> <span data-ttu-id="5c7b1-172">Logické operátory můžete použít pro tyto podmínky přesně definovat, tento scénář pro zásady.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-172">You can apply logical operators to these conditions to precisely define the scenario for a policy.</span></span> <span data-ttu-id="5c7b1-173">V **pak** bloku, definujete o tom, že se stane, když **Pokud** podmínky jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-173">In the **Then** block, you define the effect that happens when the **If** conditions are fulfilled.</span></span>

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a><span data-ttu-id="5c7b1-174">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="5c7b1-174">Logical operators</span></span>
<span data-ttu-id="5c7b1-175">Jsou podporované logické operátory:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-175">The supported logical operators are:</span></span>

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

<span data-ttu-id="5c7b1-176">**Není** syntaxe Invertuje výběr výsledek podmínku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-176">The **not** syntax inverts the result of the condition.</span></span> <span data-ttu-id="5c7b1-177">**AllOf** syntaxe (podobně jako logické **a** operaci) vyžaduje všechny podmínek.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-177">The **allOf** syntax (similar to the logical **And** operation) requires all conditions to be true.</span></span> <span data-ttu-id="5c7b1-178">**AnyOf** syntaxe (podobně jako logické **nebo** operaci) vyžaduje jeden nebo více podmínek.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-178">The **anyOf** syntax (similar to the logical **Or** operation) requires one or more conditions to be true.</span></span>

<span data-ttu-id="5c7b1-179">Logické operátory lze vnořit.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-179">You can nest logical operators.</span></span> <span data-ttu-id="5c7b1-180">Následující příklad ukazuje **není** operace, která je vnořená v rámci **allOf** operaci.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-180">The following example shows a **not** operation that is nested within an **allOf** operation.</span></span> 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a><span data-ttu-id="5c7b1-181">Podmínky</span><span class="sxs-lookup"><span data-stu-id="5c7b1-181">Conditions</span></span>
<span data-ttu-id="5c7b1-182">Je podmínka vyhodnocena jako jestli **pole** splňuje určitá kritéria.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-182">The condition evaluates whether a **field** meets certain criteria.</span></span> <span data-ttu-id="5c7b1-183">Jsou podporované podmínky:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-183">The supported conditions are:</span></span>

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

<span data-ttu-id="5c7b1-184">Při použití **jako** podmínku, můžete zadat zástupný znak (*) v hodnotě.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-184">When using the **like** condition, you can provide a wildcard (*) in the value.</span></span>

<span data-ttu-id="5c7b1-185">Při použití **odpovídat** podmínky, zadejte `#` představují číslice, `?` písmeno a libovolný znak představují tento skutečný znak.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-185">When using the **match** condition, provide `#` to represent a digit, `?` for a letter, and any other character to represent that actual character.</span></span> <span data-ttu-id="5c7b1-186">Příklady najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-186">For examples, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>

### <a name="fields"></a><span data-ttu-id="5c7b1-187">Pole</span><span class="sxs-lookup"><span data-stu-id="5c7b1-187">Fields</span></span>
<span data-ttu-id="5c7b1-188">Podmínky se vytváří pomocí pole.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-188">Conditions are formed by using fields.</span></span> <span data-ttu-id="5c7b1-189">Pole představuje vlastnosti v datová část požadavku prostředku, který se používá k popisu stavu prostředku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-189">A field represents properties in the resource request payload that is used to describe the state of the resource.</span></span>  

<span data-ttu-id="5c7b1-190">Podporovány jsou následující pole:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-190">The following fields are supported:</span></span>

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* <span data-ttu-id="5c7b1-191">Vlastnost aliasy – seznam najdete v tématu [aliasy](#aliases).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-191">property aliases - for a list, see [Aliases](#aliases).</span></span>

### <a name="effect"></a><span data-ttu-id="5c7b1-192">Efekt</span><span class="sxs-lookup"><span data-stu-id="5c7b1-192">Effect</span></span>
<span data-ttu-id="5c7b1-193">Zásady podporuje tři typy efekt – `deny`, `audit`, a `append`.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-193">Policy supports three types of effect - `deny`, `audit`, and `append`.</span></span> 

* <span data-ttu-id="5c7b1-194">**Odepřít** vygeneruje událost v protokolu auditování a požadavek se nezdaří</span><span class="sxs-lookup"><span data-stu-id="5c7b1-194">**Deny** generates an event in the audit log and fails the request</span></span>
* <span data-ttu-id="5c7b1-195">**Audit** vygeneruje událost upozornění v protokolu auditu ale nesplní žádost</span><span class="sxs-lookup"><span data-stu-id="5c7b1-195">**Audit** generates a warning event in audit log but does not fail the request</span></span>
* <span data-ttu-id="5c7b1-196">**Připojit** přidá definovanou sadu pole k této žádosti</span><span class="sxs-lookup"><span data-stu-id="5c7b1-196">**Append** adds the defined set of fields to the request</span></span> 

<span data-ttu-id="5c7b1-197">Pro **připojit**, je nutné zadat následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-197">For **append**, you must provide the following details:</span></span>

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

<span data-ttu-id="5c7b1-198">Hodnota může být řetězec nebo objekt formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-198">The value can be either a string or a JSON format object.</span></span> 

## <a name="aliases"></a><span data-ttu-id="5c7b1-199">Aliasy</span><span class="sxs-lookup"><span data-stu-id="5c7b1-199">Aliases</span></span>

<span data-ttu-id="5c7b1-200">Vlastnost aliasy používáte pro přístup k vlastnosti specifické pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-200">You use property aliases to access specific properties for a resource type.</span></span> <span data-ttu-id="5c7b1-201">Aliasy umožňují omezit, jaké hodnoty nebo podmínky jsou povoleny pro vlastnost prostředku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-201">Aliases enable you to restrict what values or conditions are permitted for a property on a resource.</span></span> <span data-ttu-id="5c7b1-202">Každý alias mapuje cesty v různých verzích rozhraní API pro typ daného prostředku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-202">Each alias maps to paths in different API versions for a given resource type.</span></span> <span data-ttu-id="5c7b1-203">Modul zásad během hodnocení zásad, získá vlastnost cesty pro tuto verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-203">During policy evaluation, the policy engine gets the property path for that API version.</span></span>

<span data-ttu-id="5c7b1-204">**Microsoft.Cache/Redis**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-204">**Microsoft.Cache/Redis**</span></span>

| <span data-ttu-id="5c7b1-205">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-205">Alias</span></span> | <span data-ttu-id="5c7b1-206">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-206">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-207">Microsoft.Cache/Redis/enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="5c7b1-207">Microsoft.Cache/Redis/enableNonSslPort</span></span> | <span data-ttu-id="5c7b1-208">Jestli port serveru Redis bez ssl (6379) je povoleno nastavení.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-208">Set whether the non-ssl Redis server port (6379) is enabled.</span></span> |
| <span data-ttu-id="5c7b1-209">Microsoft.Cache/Redis/shardCount</span><span class="sxs-lookup"><span data-stu-id="5c7b1-209">Microsoft.Cache/Redis/shardCount</span></span> | <span data-ttu-id="5c7b1-210">Nastavte počet horizontálních oddílů má být vytvořena na clusteru Cache ve verzi Premium.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-210">Set the number of shards to be created on a Premium Cluster Cache.</span></span>  |
| <span data-ttu-id="5c7b1-211">Microsoft.Cache/Redis/sku.capacity</span><span class="sxs-lookup"><span data-stu-id="5c7b1-211">Microsoft.Cache/Redis/sku.capacity</span></span> | <span data-ttu-id="5c7b1-212">Nastavení velikosti mezipaměti Redis k nasazení.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-212">Set the size of the Redis cache to deploy.</span></span>  |
| <span data-ttu-id="5c7b1-213">Microsoft.Cache/Redis/sku.family</span><span class="sxs-lookup"><span data-stu-id="5c7b1-213">Microsoft.Cache/Redis/sku.family</span></span> | <span data-ttu-id="5c7b1-214">Nastavte řada SKU používat.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-214">Set the SKU family to use.</span></span> |
| <span data-ttu-id="5c7b1-215">Microsoft.Cache/Redis/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-215">Microsoft.Cache/Redis/sku.name</span></span> | <span data-ttu-id="5c7b1-216">Nastavte typ Redis Cache k nasazení.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-216">Set the type of Redis Cache to deploy.</span></span> |

<span data-ttu-id="5c7b1-217">**Microsoft.Cdn/profiles**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-217">**Microsoft.Cdn/profiles**</span></span>

| <span data-ttu-id="5c7b1-218">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-218">Alias</span></span> | <span data-ttu-id="5c7b1-219">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-219">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-220">Microsoft.CDN/profiles/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-220">Microsoft.CDN/profiles/sku.name</span></span> | <span data-ttu-id="5c7b1-221">Nastavte název cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-221">Set the name of the pricing tier.</span></span> |

<span data-ttu-id="5c7b1-222">**Microsoft.Compute/disks**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-222">**Microsoft.Compute/disks**</span></span>

| <span data-ttu-id="5c7b1-223">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-223">Alias</span></span> | <span data-ttu-id="5c7b1-224">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-224">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-225">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="5c7b1-225">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="5c7b1-226">Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-226">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-227">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="5c7b1-227">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="5c7b1-228">Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-228">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-229">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="5c7b1-229">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="5c7b1-230">Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-230">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-231">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="5c7b1-231">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="5c7b1-232">Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-232">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |


<span data-ttu-id="5c7b1-233">**Microsoft.Compute/virtualMachines**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-233">**Microsoft.Compute/virtualMachines**</span></span>

| <span data-ttu-id="5c7b1-234">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-234">Alias</span></span> | <span data-ttu-id="5c7b1-235">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-235">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-236">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="5c7b1-236">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="5c7b1-237">Nastavte identifikátor image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-237">Set the identifier of the image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-238">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="5c7b1-238">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="5c7b1-239">Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-239">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-240">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="5c7b1-240">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="5c7b1-241">Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-241">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-242">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="5c7b1-242">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="5c7b1-243">Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-243">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-244">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="5c7b1-244">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="5c7b1-245">Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-245">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-246">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="5c7b1-246">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="5c7b1-247">Nastavení, která obrázku nebo je licencovaný místní disk.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-247">Set that the image or disk is licensed on-premises.</span></span> <span data-ttu-id="5c7b1-248">Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-248">This value is only used for images that contain the Windows Server operating system.</span></span>  |
| <span data-ttu-id="5c7b1-249">Microsoft.Compute/virtualMachines/imageOffer</span><span class="sxs-lookup"><span data-stu-id="5c7b1-249">Microsoft.Compute/virtualMachines/imageOffer</span></span> | <span data-ttu-id="5c7b1-250">Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-250">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-251">Microsoft.Compute/virtualMachines/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="5c7b1-251">Microsoft.Compute/virtualMachines/imagePublisher</span></span> | <span data-ttu-id="5c7b1-252">Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-252">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-253">Microsoft.Compute/virtualMachines/imageSku</span><span class="sxs-lookup"><span data-stu-id="5c7b1-253">Microsoft.Compute/virtualMachines/imageSku</span></span> | <span data-ttu-id="5c7b1-254">Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-254">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-255">Microsoft.Compute/virtualMachines/imageVersion</span><span class="sxs-lookup"><span data-stu-id="5c7b1-255">Microsoft.Compute/virtualMachines/imageVersion</span></span> | <span data-ttu-id="5c7b1-256">Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-256">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span><span class="sxs-lookup"><span data-stu-id="5c7b1-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span></span> | <span data-ttu-id="5c7b1-258">Nastavte identifikátor URI virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-258">Set the vhd URI.</span></span> |
| <span data-ttu-id="5c7b1-259">Microsoft.Compute/virtualMachines/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-259">Microsoft.Compute/virtualMachines/sku.name</span></span> | <span data-ttu-id="5c7b1-260">Nastavení velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-260">Set the size of the virtual machine.</span></span> |

<span data-ttu-id="5c7b1-261">**Microsoft.Compute/virtualMachines/extensions**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-261">**Microsoft.Compute/virtualMachines/extensions**</span></span>

| <span data-ttu-id="5c7b1-262">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-262">Alias</span></span> | <span data-ttu-id="5c7b1-263">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-263">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-264">Microsoft.Compute/virtualMachines/extensions/publisher</span><span class="sxs-lookup"><span data-stu-id="5c7b1-264">Microsoft.Compute/virtualMachines/extensions/publisher</span></span> | <span data-ttu-id="5c7b1-265">Nastavte název rozšíření vydavatele.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-265">Set the name of the extension’s publisher.</span></span> |
| <span data-ttu-id="5c7b1-266">Microsoft.Compute/virtualMachines/extensions/type</span><span class="sxs-lookup"><span data-stu-id="5c7b1-266">Microsoft.Compute/virtualMachines/extensions/type</span></span> | <span data-ttu-id="5c7b1-267">Nastavte typ rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-267">Set the type of extension.</span></span> |
| <span data-ttu-id="5c7b1-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="5c7b1-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span></span> | <span data-ttu-id="5c7b1-269">Nastavte verzi rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-269">Set the version of the extension.</span></span> |

<span data-ttu-id="5c7b1-270">**Microsoft.Compute/virtualMachineScaleSets**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-270">**Microsoft.Compute/virtualMachineScaleSets**</span></span>

| <span data-ttu-id="5c7b1-271">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-271">Alias</span></span> | <span data-ttu-id="5c7b1-272">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-272">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-273">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="5c7b1-273">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="5c7b1-274">Nastavte identifikátor image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-274">Set the identifier of the image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-275">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="5c7b1-275">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="5c7b1-276">Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-276">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-277">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="5c7b1-277">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="5c7b1-278">Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-278">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-279">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="5c7b1-279">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="5c7b1-280">Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-280">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-281">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="5c7b1-281">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="5c7b1-282">Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-282">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="5c7b1-283">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="5c7b1-283">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="5c7b1-284">Nastavení, která obrázku nebo je licencovaný místní disk.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-284">Set that the image or disk is licensed on-premises.</span></span> <span data-ttu-id="5c7b1-285">Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-285">This value is only used for images that contain the Windows Server operating system.</span></span> |
| <span data-ttu-id="5c7b1-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="5c7b1-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span></span> | <span data-ttu-id="5c7b1-287">Nastavte předpony názvu počítače pro všechny virtuální počítače v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-287">Set the computer name prefix for all  the virtual machines in the scale set.</span></span> |
| <span data-ttu-id="5c7b1-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span><span class="sxs-lookup"><span data-stu-id="5c7b1-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span></span> | <span data-ttu-id="5c7b1-289">Nastavte identifikátor URI objektu blob bitové kopie uživatele.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-289">Set the blob URI for user image.</span></span> |
| <span data-ttu-id="5c7b1-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span><span class="sxs-lookup"><span data-stu-id="5c7b1-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span></span> | <span data-ttu-id="5c7b1-291">Nastavte adresy URL kontejneru, které se používají k uložení disku operačního systému pro škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-291">Set the container URLs that are used to store operating system disks for the scale set.</span></span> |
| <span data-ttu-id="5c7b1-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span></span> | <span data-ttu-id="5c7b1-293">Nastavení velikosti virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-293">Set the size of virtual machines in a scale set.</span></span> |
| <span data-ttu-id="5c7b1-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span><span class="sxs-lookup"><span data-stu-id="5c7b1-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span></span> | <span data-ttu-id="5c7b1-295">Nastavte úroveň virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-295">Set the tier of virtual machines in a scale set.</span></span> |
  
<span data-ttu-id="5c7b1-296">**Microsoft.Network/applicationGateways**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-296">**Microsoft.Network/applicationGateways**</span></span>

| <span data-ttu-id="5c7b1-297">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-297">Alias</span></span> | <span data-ttu-id="5c7b1-298">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-298">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-299">Microsoft.Network/applicationGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-299">Microsoft.Network/applicationGateways/sku.name</span></span> | <span data-ttu-id="5c7b1-300">Nastavení velikosti brány.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-300">Set the size of the gateway.</span></span> |

<span data-ttu-id="5c7b1-301">**Microsoft.Network/virtualNetworkGateways**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-301">**Microsoft.Network/virtualNetworkGateways**</span></span>

| <span data-ttu-id="5c7b1-302">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-302">Alias</span></span> | <span data-ttu-id="5c7b1-303">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-303">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span><span class="sxs-lookup"><span data-stu-id="5c7b1-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span></span> | <span data-ttu-id="5c7b1-305">Nastavte typ tuto bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-305">Set the type of this virtual network gateway.</span></span> |
| <span data-ttu-id="5c7b1-306">Microsoft.Network/virtualNetworkGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-306">Microsoft.Network/virtualNetworkGateways/sku.name</span></span> | <span data-ttu-id="5c7b1-307">Nastavte název SKU brány.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-307">Set the gateway SKU name.</span></span> |

<span data-ttu-id="5c7b1-308">**Microsoft.Sql/servers**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-308">**Microsoft.Sql/servers**</span></span>

| <span data-ttu-id="5c7b1-309">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-309">Alias</span></span> | <span data-ttu-id="5c7b1-310">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-310">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-311">Microsoft.Sql/servers/version</span><span class="sxs-lookup"><span data-stu-id="5c7b1-311">Microsoft.Sql/servers/version</span></span> | <span data-ttu-id="5c7b1-312">Nastavte verzi serveru.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-312">Set the version of the server.</span></span> |

<span data-ttu-id="5c7b1-313">**Microsoft.Sql/databases**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-313">**Microsoft.Sql/databases**</span></span>

| <span data-ttu-id="5c7b1-314">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-314">Alias</span></span> | <span data-ttu-id="5c7b1-315">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-315">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-316">Microsoft.Sql/servers/databases/edition</span><span class="sxs-lookup"><span data-stu-id="5c7b1-316">Microsoft.Sql/servers/databases/edition</span></span> | <span data-ttu-id="5c7b1-317">Nastavte edici databáze.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-317">Set the edition of the database.</span></span> |
| <span data-ttu-id="5c7b1-318">Microsoft.Sql/servers/databases/elasticPoolName</span><span class="sxs-lookup"><span data-stu-id="5c7b1-318">Microsoft.Sql/servers/databases/elasticPoolName</span></span> | <span data-ttu-id="5c7b1-319">Sada název fondu elastické databáze je v.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-319">Set the name of the elastic pool the database is in.</span></span> |
| <span data-ttu-id="5c7b1-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span><span class="sxs-lookup"><span data-stu-id="5c7b1-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span></span> | <span data-ttu-id="5c7b1-321">Nastavte úroveň cíle ID databáze nakonfigurované služby.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-321">Set the configured service level objective ID of the database.</span></span> |
| <span data-ttu-id="5c7b1-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span><span class="sxs-lookup"><span data-stu-id="5c7b1-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span></span> | <span data-ttu-id="5c7b1-323">Nastavte název cíle na úrovni služby nakonfigurované databáze.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-323">Set the name of the configured service level objective of the database.</span></span>  |

<span data-ttu-id="5c7b1-324">**Microsoft.Sql/elasticpools**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-324">**Microsoft.Sql/elasticpools**</span></span>

| <span data-ttu-id="5c7b1-325">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-325">Alias</span></span> | <span data-ttu-id="5c7b1-326">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-326">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-327">servery nebo elasticpools</span><span class="sxs-lookup"><span data-stu-id="5c7b1-327">servers/elasticpools</span></span> | <span data-ttu-id="5c7b1-328">Microsoft.Sql/servers/elasticPools/dtu</span><span class="sxs-lookup"><span data-stu-id="5c7b1-328">Microsoft.Sql/servers/elasticPools/dtu</span></span> | <span data-ttu-id="5c7b1-329">Nastavte celkový sdílené DTU fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-329">Set the total shared DTU for the database elastic pool.</span></span> |
| <span data-ttu-id="5c7b1-330">servery nebo elasticpools</span><span class="sxs-lookup"><span data-stu-id="5c7b1-330">servers/elasticpools</span></span> | <span data-ttu-id="5c7b1-331">Microsoft.Sql/servers/elasticPools/edition</span><span class="sxs-lookup"><span data-stu-id="5c7b1-331">Microsoft.Sql/servers/elasticPools/edition</span></span> | <span data-ttu-id="5c7b1-332">Nastavte edici elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-332">Set the edition of the elastic pool.</span></span> |

<span data-ttu-id="5c7b1-333">**Microsoft.Storage/storageAccounts.**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-333">**Microsoft.Storage/storageAccounts**</span></span>

| <span data-ttu-id="5c7b1-334">Alias</span><span class="sxs-lookup"><span data-stu-id="5c7b1-334">Alias</span></span> | <span data-ttu-id="5c7b1-335">Popis</span><span class="sxs-lookup"><span data-stu-id="5c7b1-335">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="5c7b1-336">Microsoft.Storage/storageAccounts/accessTier</span><span class="sxs-lookup"><span data-stu-id="5c7b1-336">Microsoft.Storage/storageAccounts/accessTier</span></span> | <span data-ttu-id="5c7b1-337">Nastavte úroveň přístupu, které se používá pro fakturaci.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-337">Set the access tier used for billing.</span></span> |
| <span data-ttu-id="5c7b1-338">Microsoft.Storage/storageAccounts/accountType</span><span class="sxs-lookup"><span data-stu-id="5c7b1-338">Microsoft.Storage/storageAccounts/accountType</span></span> | <span data-ttu-id="5c7b1-339">Název SKU nastavte.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-339">Set the SKU name.</span></span> |
| <span data-ttu-id="5c7b1-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span><span class="sxs-lookup"><span data-stu-id="5c7b1-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span></span> | <span data-ttu-id="5c7b1-341">Nastavte, jestli služba šifruje data, jak je uložen v úložišti služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-341">Set whether the service encrypts the data as it is stored in the blob storage service.</span></span> |
| <span data-ttu-id="5c7b1-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span><span class="sxs-lookup"><span data-stu-id="5c7b1-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span></span> | <span data-ttu-id="5c7b1-343">Nastavte, jestli služba šifruje data, jak je uložen v úložišti služby souboru.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-343">Set whether the service encrypts the data as it is stored in the file storage service.</span></span> |
| <span data-ttu-id="5c7b1-344">Microsoft.Storage/storageAccounts/sku.name</span><span class="sxs-lookup"><span data-stu-id="5c7b1-344">Microsoft.Storage/storageAccounts/sku.name</span></span> | <span data-ttu-id="5c7b1-345">Název SKU nastavte.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-345">Set the SKU name.</span></span> |
| <span data-ttu-id="5c7b1-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span><span class="sxs-lookup"><span data-stu-id="5c7b1-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span></span> | <span data-ttu-id="5c7b1-347">Nastavte, aby umožňovala pouze provoz https pro službu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-347">Set to allow only https traffic to storage service.</span></span> |


## <a name="policy-examples"></a><span data-ttu-id="5c7b1-348">Příklady zásad</span><span class="sxs-lookup"><span data-stu-id="5c7b1-348">Policy examples</span></span>

<span data-ttu-id="5c7b1-349">Následující témata obsahují příklady zásad:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-349">The following topics contain policy examples:</span></span>

* <span data-ttu-id="5c7b1-350">Příklady značky zásady, najdete v části [zásad prostředků pro značky](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-350">For examples of tag polices, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>
* <span data-ttu-id="5c7b1-351">Příklady vzory pojmenovávání a text najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-351">For examples of naming and text patterns, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>
* <span data-ttu-id="5c7b1-352">Příklady úložiště zásad najdete v tématu [účty úložiště použít zásady prostředků](resource-manager-policy-storage.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-352">For examples of storage policies, see [Apply resource policies to storage accounts](resource-manager-policy-storage.md).</span></span>
* <span data-ttu-id="5c7b1-353">Příklady zásad virtuálního počítače najdete v tématu [platí zásady prostředků pro virtuální počítače s Linuxem](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) a [platí zásady prostředků pro virtuální počítače Windows](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="5c7b1-353">For examples of virtual machine policies, see [Apply resource policies to Linux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) and [Apply resource policies to Windows VMs](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span></span>


## <a name="next-steps"></a><span data-ttu-id="5c7b1-354">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c7b1-354">Next steps</span></span>
* <span data-ttu-id="5c7b1-355">Po definování pravidla zásad, přiřaďte ho do oboru.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-355">After defining a policy rule, assign it to a scope.</span></span> <span data-ttu-id="5c7b1-356">K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-356">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="5c7b1-357">K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-357">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="5c7b1-358">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-358">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="5c7b1-359">Schéma zásad je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-359">The policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

