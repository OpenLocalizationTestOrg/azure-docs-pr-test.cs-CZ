---
title: "zásady prostředků aaaAzure | Microsoft Docs"
description: "Popisuje, jak se vlastností konzistentní prostředku tooensure zásady Azure Resource Manager toouse nastaví během nasazení. Zásady můžete použít na hello předplatné nebo prostředek skupiny."
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
ms.openlocfilehash: f1b0bbb5f838f6bb70721e1040ad3eac2d881cea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-policy-overview"></a><span data-ttu-id="ee6d3-104">Přehled zásad prostředků</span><span class="sxs-lookup"><span data-stu-id="ee6d3-104">Resource policy overview</span></span>
<span data-ttu-id="ee6d3-105">Zásady prostředků povolit tooestablish konvence pro prostředky ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-105">Resource policies enable you tooestablish conventions for resources in your organization.</span></span> <span data-ttu-id="ee6d3-106">Definováním konvence můžete řídit náklady a snadněji spravovat vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-106">By defining conventions, you can control costs and more easily manage your resources.</span></span> <span data-ttu-id="ee6d3-107">Například můžete zadat, že jsou povoleny pouze určité typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-107">For example, you can specify that only certain types of virtual machines are allowed.</span></span> <span data-ttu-id="ee6d3-108">Nebo můžete vyžadovat, aby všechny prostředky měli konkrétní značku.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-108">Or, you can require that all resources have a particular tag.</span></span> <span data-ttu-id="ee6d3-109">Zásady jsou zdědí všechny podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-109">Policies are inherited by all child resources.</span></span> <span data-ttu-id="ee6d3-110">Proto pokud je zásada použitá tooa skupinu prostředků, je použít tooall hello prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-110">So, if a policy is applied tooa resource group, it is applicable tooall hello resources in that resource group.</span></span>

<span data-ttu-id="ee6d3-111">Existují dvě toounderstand koncepty týkající se zásad:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-111">There are two concepts toounderstand about policies:</span></span>

* <span data-ttu-id="ee6d3-112">Definice zásad - popisují při hello zásady se vynucují a jaké akce tootake</span><span class="sxs-lookup"><span data-stu-id="ee6d3-112">policy definition - you describe when hello policy is enforced and what action tootake</span></span>
* <span data-ttu-id="ee6d3-113">přiřazení zásady - použijete hello zásady definice tooa oboru (předplatné nebo skupinu prostředků)</span><span class="sxs-lookup"><span data-stu-id="ee6d3-113">policy assignment - you apply hello policy definition tooa scope (subscription or resource group)</span></span>

<span data-ttu-id="ee6d3-114">Toto téma se zaměřuje na definice zásady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-114">This topic focuses on policy definition.</span></span> <span data-ttu-id="ee6d3-115">Informace o přiřazení zásad najdete v tématu [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md) nebo [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-115">For information about policy assignment, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md) or [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>

<span data-ttu-id="ee6d3-116">Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-116">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

> [!NOTE]
> <span data-ttu-id="ee6d3-117">Zásady v současné době nelze vyhodnotit typů prostředků, které nepodporují značky, typ a umístění, jako je například typ prostředku Microsoft.Resources/deployments hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-117">Currently, policy does not evaluate resource types that do not support tags, kind, and location, such as hello Microsoft.Resources/deployments resource type.</span></span> <span data-ttu-id="ee6d3-118">Tato podpora se přidá na datum v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-118">This support will be added at a future time.</span></span> <span data-ttu-id="ee6d3-119">problémy s kompatibilitou zpětné tooavoid, byste měli explicitně zadat typ při vytváření zásad.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-119">tooavoid backward compatibility issues, you should explicitly specify type when authoring policies.</span></span> <span data-ttu-id="ee6d3-120">Například značky zásadu, která neurčuje typy platí pro všechny typy.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-120">For example, a tag policy that does not specify types is applied for all types.</span></span> <span data-ttu-id="ee6d3-121">V takovém případě šablony nasazení může selhat, pokud je vnořeného prostředku, který nepodporuje značky a typ prostředku nasazení hello přidala toopolicy vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-121">In that case, a template deployment may fail if there is a nested resource that doesn't support tags, and hello deployment resource type has been added toopolicy evaluation.</span></span> 
> 
> 

## <a name="how-is-it-different-from-rbac"></a><span data-ttu-id="ee6d3-122">Jak se liší od RBAC?</span><span class="sxs-lookup"><span data-stu-id="ee6d3-122">How is it different from RBAC?</span></span>
<span data-ttu-id="ee6d3-123">Existuje několik hlavní rozdíly mezi zásadami a řízení přístupu na základě role (RBAC).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-123">There are a few key differences between policy and role-based access control (RBAC).</span></span> <span data-ttu-id="ee6d3-124">RBAC se zaměřuje na **uživatele** akce na různých místech.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-124">RBAC focuses on **user** actions at different scopes.</span></span> <span data-ttu-id="ee6d3-125">Například se přidají toohello role Přispěvatel pro skupinu prostředků v hello požadovaného oboru, umožní vám provádět změny toothat prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-125">For example, you are added toohello contributor role for a resource group at hello desired scope, so you can make changes toothat resource group.</span></span> <span data-ttu-id="ee6d3-126">Zásady se zaměřuje na **prostředků** vlastnosti během nasazení.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-126">Policy focuses on **resource** properties during deployment.</span></span> <span data-ttu-id="ee6d3-127">Například prostřednictvím zásad, můžete řídit hello typů prostředků, které mohou být zřízeny.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-127">For example, through policies, you can control hello types of resources that can be provisioned.</span></span> <span data-ttu-id="ee6d3-128">Nebo můžete omezit hello umístění, ve kterých se dá zřídit prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-128">Or, you can restrict hello locations in which hello resources can be provisioned.</span></span> <span data-ttu-id="ee6d3-129">Na rozdíl od RBAC, je povolit výchozí zásady a explicitní odepřít systému.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-129">Unlike RBAC, policy is a default allow and explicit deny system.</span></span> 

<span data-ttu-id="ee6d3-130">toouse zásady, které musí být ověřeny pomocí RBAC.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-130">toouse policies, you must be authenticated through RBAC.</span></span> <span data-ttu-id="ee6d3-131">Konkrétně, musí váš účet:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-131">Specifically, your account needs the:</span></span>

* <span data-ttu-id="ee6d3-132">`Microsoft.Authorization/policydefinitions/write`oprávnění toodefine zásadu</span><span class="sxs-lookup"><span data-stu-id="ee6d3-132">`Microsoft.Authorization/policydefinitions/write` permission toodefine a policy</span></span>
* <span data-ttu-id="ee6d3-133">`Microsoft.Authorization/policyassignments/write`oprávnění tooassign zásadu</span><span class="sxs-lookup"><span data-stu-id="ee6d3-133">`Microsoft.Authorization/policyassignments/write` permission tooassign a policy</span></span> 

<span data-ttu-id="ee6d3-134">Tato oprávnění nejsou součástí hello **Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-134">These permissions are not included in hello **Contributor** role.</span></span>

## <a name="built-in-policies"></a><span data-ttu-id="ee6d3-135">Předdefinované zásady</span><span class="sxs-lookup"><span data-stu-id="ee6d3-135">Built-in policies</span></span>

<span data-ttu-id="ee6d3-136">Azure poskytuje definice předdefinované zásady, které může snížit počet hello zásad máte toodefine.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-136">Azure provides some built-in policy definitions that may reduce hello number of policies you have toodefine.</span></span> <span data-ttu-id="ee6d3-137">Před pokračováním definice zásady, byste měli zvážit, zda předdefinovaných zásad již poskytuje hello definice, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-137">Before proceeding with policy definitions, you should consider whether a built-in policy already provides hello definition you need.</span></span> <span data-ttu-id="ee6d3-138">definice Hello předdefinovaných zásad jsou:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-138">hello built-in policy definitions are:</span></span>

* <span data-ttu-id="ee6d3-139">Povolených umístění</span><span class="sxs-lookup"><span data-stu-id="ee6d3-139">Allowed locations</span></span>
* <span data-ttu-id="ee6d3-140">Typy povolené prostředků</span><span class="sxs-lookup"><span data-stu-id="ee6d3-140">Allowed resource types</span></span>
* <span data-ttu-id="ee6d3-141">Povolené účet úložiště SKU</span><span class="sxs-lookup"><span data-stu-id="ee6d3-141">Allowed storage account SKUs</span></span>
* <span data-ttu-id="ee6d3-142">Povolená SKU virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ee6d3-142">Allowed virtual machine SKUs</span></span>
* <span data-ttu-id="ee6d3-143">Použít značky a výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ee6d3-143">Apply tag and default value</span></span>
* <span data-ttu-id="ee6d3-144">Vynutit značky a hodnota</span><span class="sxs-lookup"><span data-stu-id="ee6d3-144">Enforce tag and value</span></span>
* <span data-ttu-id="ee6d3-145">Není povoleno typy prostředků</span><span class="sxs-lookup"><span data-stu-id="ee6d3-145">Not allowed resource types</span></span>
* <span data-ttu-id="ee6d3-146">Vyžaduje systém SQL Server verze 12.0</span><span class="sxs-lookup"><span data-stu-id="ee6d3-146">Require SQL Server version 12.0</span></span>
* <span data-ttu-id="ee6d3-147">Vyžadovat šifrování účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="ee6d3-147">Require storage account encryption</span></span>

<span data-ttu-id="ee6d3-148">Můžete přiřadit některé z těchto zásad prostřednictvím hello [portál](resource-manager-policy-portal.md), [prostředí PowerShell](resource-manager-policy-create-assign.md#powershell), nebo [rozhraní příkazového řádku Azure](resource-manager-policy-create-assign.md#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-148">You can assign any of these policies through hello [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), or [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span></span>

## <a name="policy-definition-structure"></a><span data-ttu-id="ee6d3-149">Definice strukturu zásad.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-149">Policy definition structure</span></span>
<span data-ttu-id="ee6d3-150">Používáte JSON toocreate definici zásady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-150">You use JSON toocreate a policy definition.</span></span> <span data-ttu-id="ee6d3-151">Definice zásad Hello obsahuje prvky pro:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-151">hello policy definition contains elements for:</span></span>

* <span data-ttu-id="ee6d3-152">parameters</span><span class="sxs-lookup"><span data-stu-id="ee6d3-152">parameters</span></span>
* <span data-ttu-id="ee6d3-153">Zobrazovaný název</span><span class="sxs-lookup"><span data-stu-id="ee6d3-153">display name</span></span>
* <span data-ttu-id="ee6d3-154">description</span><span class="sxs-lookup"><span data-stu-id="ee6d3-154">description</span></span>
* <span data-ttu-id="ee6d3-155">Pravidlo zásad</span><span class="sxs-lookup"><span data-stu-id="ee6d3-155">policy rule</span></span>
  * <span data-ttu-id="ee6d3-156">logické vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="ee6d3-156">logical evaluation</span></span>
  * <span data-ttu-id="ee6d3-157">Platnost</span><span class="sxs-lookup"><span data-stu-id="ee6d3-157">effect</span></span>

<span data-ttu-id="ee6d3-158">Hello následující příklad ukazuje zásadu, která omezuje, kde jsou nasazené prostředky:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-158">hello following example shows a policy that limits where resources are deployed:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
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

## <a name="parameters"></a><span data-ttu-id="ee6d3-159">Parametry</span><span class="sxs-lookup"><span data-stu-id="ee6d3-159">Parameters</span></span>
<span data-ttu-id="ee6d3-160">Pomocí parametrů pomáhá zjednodušit vaší zásady správy, protože snižuje počet hello definice zásady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-160">Using parameters helps simplify your policy management by reducing hello number of policy definitions.</span></span> <span data-ttu-id="ee6d3-161">Můžete definovat zásady pro vlastnost prostředku (například omezení hello umístění, kde můžete nasadit prostředky) a zahrnout parametry v definici hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-161">You define a policy for a resource property (such as limiting hello locations where resources can be deployed), and include parameters in hello definition.</span></span> <span data-ttu-id="ee6d3-162">Pak můžete znovu použít, aby definice zásady pro různé scénáře předávání různé hodnoty (například určení jednu sadu umístění pro předplatné) při přiřazování zásady hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-162">Then, you reuse that policy definition for different scenarios by passing in different values (such as specifying one set of locations for a subscription) when assigning hello policy.</span></span>

<span data-ttu-id="ee6d3-163">Parametry deklarovat, při vytváření definice zásad.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-163">You declare parameters when you create policy definitions.</span></span>

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "hello list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

<span data-ttu-id="ee6d3-164">Hello typ parametru může být řetězec nebo pole.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-164">hello type of a parameter can be either string or array.</span></span> <span data-ttu-id="ee6d3-165">Vlastnost metadat Hello se používá pro nástroje, například Azure portálu toodisplay uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-165">hello metadata property is used for tools like Azure portal toodisplay user-friendly information.</span></span> 

<span data-ttu-id="ee6d3-166">Pravidlo zásad hello odkazujete parametry s hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-166">In hello policy rule, you reference parameters with hello following syntax:</span></span> 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a><span data-ttu-id="ee6d3-167">Zobrazovaný název a popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-167">Display name and description</span></span>

<span data-ttu-id="ee6d3-168">Použít hello **displayName** a **popis** tooidentify hello definice zásady a poskytují kontext pro při použití.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-168">You use hello **displayName** and **description** tooidentify hello policy definition, and provide context for when it is used.</span></span>

## <a name="policy-rule"></a><span data-ttu-id="ee6d3-169">Pravidlo zásad</span><span class="sxs-lookup"><span data-stu-id="ee6d3-169">Policy rule</span></span>

<span data-ttu-id="ee6d3-170">pravidlo zásad Hello se skládá z **Pokud** a **pak** bloky.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-170">hello policy rule consists of **If** and **Then** blocks.</span></span> <span data-ttu-id="ee6d3-171">V hello **Pokud** blok, můžete definovat jednu nebo víc podmínek, které určují, kdy hello zásady se vynucují.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-171">In hello **If** block, you define one or more conditions that specify when hello policy is enforced.</span></span> <span data-ttu-id="ee6d3-172">Logické operátory toothese podmínky lze použít tooprecisely definovat hello scénář pro zásady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-172">You can apply logical operators toothese conditions tooprecisely define hello scenario for a policy.</span></span> <span data-ttu-id="ee6d3-173">V hello **pak** blok, můžete definovat hello vliv, který se stane, když hello **Pokud** jsou splněny podmínky.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-173">In hello **Then** block, you define hello effect that happens when hello **If** conditions are fulfilled.</span></span>

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

### <a name="logical-operators"></a><span data-ttu-id="ee6d3-174">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="ee6d3-174">Logical operators</span></span>
<span data-ttu-id="ee6d3-175">logické operátory Hello podporována jsou:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-175">hello supported logical operators are:</span></span>

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

<span data-ttu-id="ee6d3-176">Hello **není** syntaxe Invertuje výběr hello výsledek hello podmínku.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-176">hello **not** syntax inverts hello result of hello condition.</span></span> <span data-ttu-id="ee6d3-177">Hello **allOf** syntaxe (podobně jako toohello logické **a** operaci) vyžaduje všechny podmínky toobe true.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-177">hello **allOf** syntax (similar toohello logical **And** operation) requires all conditions toobe true.</span></span> <span data-ttu-id="ee6d3-178">Hello **anyOf** syntaxe (podobně jako toohello logické **nebo** operaci) vyžaduje jeden nebo více podmínek toobe true.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-178">hello **anyOf** syntax (similar toohello logical **Or** operation) requires one or more conditions toobe true.</span></span>

<span data-ttu-id="ee6d3-179">Logické operátory lze vnořit.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-179">You can nest logical operators.</span></span> <span data-ttu-id="ee6d3-180">Následující příklad ukazuje Hello **není** operace, která je vnořen do **allOf** operaci.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-180">hello following example shows a **not** operation that is nested within an **allOf** operation.</span></span> 

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

### <a name="conditions"></a><span data-ttu-id="ee6d3-181">Podmínky</span><span class="sxs-lookup"><span data-stu-id="ee6d3-181">Conditions</span></span>
<span data-ttu-id="ee6d3-182">Hello podmínka vyhodnocena jako jestli **pole** splňuje určitá kritéria.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-182">hello condition evaluates whether a **field** meets certain criteria.</span></span> <span data-ttu-id="ee6d3-183">jsou podporovány Hello podmínky:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-183">hello supported conditions are:</span></span>

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

<span data-ttu-id="ee6d3-184">Při použití hello **jako** podmínku, můžete zadat zástupný znak (*) v hodnotě hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-184">When using hello **like** condition, you can provide a wildcard (*) in hello value.</span></span>

<span data-ttu-id="ee6d3-185">Při použití hello **odpovídat** podmínky, zadejte `#` toorepresent číslici, `?` pro písmeno a všechny další znak toorepresent tento skutečný znak.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-185">When using hello **match** condition, provide `#` toorepresent a digit, `?` for a letter, and any other character toorepresent that actual character.</span></span> <span data-ttu-id="ee6d3-186">Příklady najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-186">For examples, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>

### <a name="fields"></a><span data-ttu-id="ee6d3-187">Pole</span><span class="sxs-lookup"><span data-stu-id="ee6d3-187">Fields</span></span>
<span data-ttu-id="ee6d3-188">Podmínky se vytváří pomocí pole.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-188">Conditions are formed by using fields.</span></span> <span data-ttu-id="ee6d3-189">Pole představuje vlastnosti v datová část požadavku prostředků hello tedy stav hello použité toodescribe hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-189">A field represents properties in hello resource request payload that is used toodescribe hello state of hello resource.</span></span>  

<span data-ttu-id="ee6d3-190">jsou podporovány Hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-190">hello following fields are supported:</span></span>

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* <span data-ttu-id="ee6d3-191">Vlastnost aliasy – seznam najdete v tématu [aliasy](#aliases).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-191">property aliases - for a list, see [Aliases](#aliases).</span></span>

### <a name="effect"></a><span data-ttu-id="ee6d3-192">Efekt</span><span class="sxs-lookup"><span data-stu-id="ee6d3-192">Effect</span></span>
<span data-ttu-id="ee6d3-193">Zásady podporuje tři typy efekt – `deny`, `audit`, a `append`.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-193">Policy supports three types of effect - `deny`, `audit`, and `append`.</span></span> 

* <span data-ttu-id="ee6d3-194">**Odepřít** vygeneruje událost v protokolu auditu hello a selže hello požadavku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-194">**Deny** generates an event in hello audit log and fails hello request</span></span>
* <span data-ttu-id="ee6d3-195">**Audit** vygeneruje událost upozornění v protokolu auditu ale neselže hello požadavku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-195">**Audit** generates a warning event in audit log but does not fail hello request</span></span>
* <span data-ttu-id="ee6d3-196">**Připojit** přidá hello definované sadu pole toohello požadavku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-196">**Append** adds hello defined set of fields toohello request</span></span> 

<span data-ttu-id="ee6d3-197">Pro **připojit**, je nutné zadat hello následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-197">For **append**, you must provide hello following details:</span></span>

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of hello field"
  }
]
```

<span data-ttu-id="ee6d3-198">Hello hodnota může být řetězec nebo objekt formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-198">hello value can be either a string or a JSON format object.</span></span> 

## <a name="aliases"></a><span data-ttu-id="ee6d3-199">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ee6d3-199">Aliases</span></span>

<span data-ttu-id="ee6d3-200">Vlastnost aliasy tooaccess specifické vlastnosti se používá pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-200">You use property aliases tooaccess specific properties for a resource type.</span></span> <span data-ttu-id="ee6d3-201">Aliasy umožňují toorestrict jsou povoleny jaké hodnoty nebo podmínky pro vlastnost prostředku.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-201">Aliases enable you toorestrict what values or conditions are permitted for a property on a resource.</span></span> <span data-ttu-id="ee6d3-202">Každý alias mapuje toopaths v různých verzích rozhraní API pro typ daného prostředku.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-202">Each alias maps toopaths in different API versions for a given resource type.</span></span> <span data-ttu-id="ee6d3-203">Během hodnocení zásad získá modul zásad hello hello vlastnost cesty pro tuto verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-203">During policy evaluation, hello policy engine gets hello property path for that API version.</span></span>

<span data-ttu-id="ee6d3-204">**Microsoft.Cache/Redis**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-204">**Microsoft.Cache/Redis**</span></span>

| <span data-ttu-id="ee6d3-205">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-205">Alias</span></span> | <span data-ttu-id="ee6d3-206">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-206">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-207">Microsoft.Cache/Redis/enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="ee6d3-207">Microsoft.Cache/Redis/enableNonSslPort</span></span> | <span data-ttu-id="ee6d3-208">Jestli port serveru Redis bez ssl hello (6379) je povoleno nastavení.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-208">Set whether hello non-ssl Redis server port (6379) is enabled.</span></span> |
| <span data-ttu-id="ee6d3-209">Microsoft.Cache/Redis/shardCount</span><span class="sxs-lookup"><span data-stu-id="ee6d3-209">Microsoft.Cache/Redis/shardCount</span></span> | <span data-ttu-id="ee6d3-210">Nastavte hello počet horizontálních oddílů toobe vytvořen na clusteru Cache ve verzi Premium.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-210">Set hello number of shards toobe created on a Premium Cluster Cache.</span></span>  |
| <span data-ttu-id="ee6d3-211">Microsoft.Cache/Redis/sku.capacity</span><span class="sxs-lookup"><span data-stu-id="ee6d3-211">Microsoft.Cache/Redis/sku.capacity</span></span> | <span data-ttu-id="ee6d3-212">Nastavit velikost hello toodeploy mezipaměti Redis hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-212">Set hello size of hello Redis cache toodeploy.</span></span>  |
| <span data-ttu-id="ee6d3-213">Microsoft.Cache/Redis/sku.family</span><span class="sxs-lookup"><span data-stu-id="ee6d3-213">Microsoft.Cache/Redis/sku.family</span></span> | <span data-ttu-id="ee6d3-214">Nastavit rodiny toouse hello SKU.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-214">Set hello SKU family toouse.</span></span> |
| <span data-ttu-id="ee6d3-215">Microsoft.Cache/Redis/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-215">Microsoft.Cache/Redis/sku.name</span></span> | <span data-ttu-id="ee6d3-216">Nastavte typ hello toodeploy Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-216">Set hello type of Redis Cache toodeploy.</span></span> |

<span data-ttu-id="ee6d3-217">**Microsoft.Cdn/profiles**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-217">**Microsoft.Cdn/profiles**</span></span>

| <span data-ttu-id="ee6d3-218">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-218">Alias</span></span> | <span data-ttu-id="ee6d3-219">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-219">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-220">Microsoft.CDN/profiles/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-220">Microsoft.CDN/profiles/sku.name</span></span> | <span data-ttu-id="ee6d3-221">Nastavte název hello hello cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-221">Set hello name of hello pricing tier.</span></span> |

<span data-ttu-id="ee6d3-222">**Microsoft.Compute/disks**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-222">**Microsoft.Compute/disks**</span></span>

| <span data-ttu-id="ee6d3-223">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-223">Alias</span></span> | <span data-ttu-id="ee6d3-224">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-224">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-225">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="ee6d3-225">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="ee6d3-226">Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-226">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-227">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="ee6d3-227">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="ee6d3-228">Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-228">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-229">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-229">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="ee6d3-230">Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-230">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-231">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="ee6d3-231">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="ee6d3-232">Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-232">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |


<span data-ttu-id="ee6d3-233">**Microsoft.Compute/virtualMachines**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-233">**Microsoft.Compute/virtualMachines**</span></span>

| <span data-ttu-id="ee6d3-234">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-234">Alias</span></span> | <span data-ttu-id="ee6d3-235">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-235">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-236">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="ee6d3-236">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="ee6d3-237">Nastavit identifikátor hello hello bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-237">Set hello identifier of hello image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-238">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="ee6d3-238">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="ee6d3-239">Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-239">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-240">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="ee6d3-240">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="ee6d3-241">Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-241">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-242">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-242">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="ee6d3-243">Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-243">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-244">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="ee6d3-244">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="ee6d3-245">Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-245">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-246">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="ee6d3-246">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="ee6d3-247">Nastavení této bitové kopie hello nebo disk je licencovaný místní.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-247">Set that hello image or disk is licensed on-premises.</span></span> <span data-ttu-id="ee6d3-248">Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-248">This value is only used for images that contain hello Windows Server operating system.</span></span>  |
| <span data-ttu-id="ee6d3-249">Microsoft.Compute/virtualMachines/imageOffer</span><span class="sxs-lookup"><span data-stu-id="ee6d3-249">Microsoft.Compute/virtualMachines/imageOffer</span></span> | <span data-ttu-id="ee6d3-250">Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-250">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-251">Microsoft.Compute/virtualMachines/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="ee6d3-251">Microsoft.Compute/virtualMachines/imagePublisher</span></span> | <span data-ttu-id="ee6d3-252">Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-252">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-253">Microsoft.Compute/virtualMachines/imageSku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-253">Microsoft.Compute/virtualMachines/imageSku</span></span> | <span data-ttu-id="ee6d3-254">Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-254">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-255">Microsoft.Compute/virtualMachines/imageVersion</span><span class="sxs-lookup"><span data-stu-id="ee6d3-255">Microsoft.Compute/virtualMachines/imageVersion</span></span> | <span data-ttu-id="ee6d3-256">Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-256">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span><span class="sxs-lookup"><span data-stu-id="ee6d3-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span></span> | <span data-ttu-id="ee6d3-258">Nastavte hello vhd identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-258">Set hello vhd URI.</span></span> |
| <span data-ttu-id="ee6d3-259">Microsoft.Compute/virtualMachines/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-259">Microsoft.Compute/virtualMachines/sku.name</span></span> | <span data-ttu-id="ee6d3-260">Nastavení velikosti hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-260">Set hello size of hello virtual machine.</span></span> |

<span data-ttu-id="ee6d3-261">**Microsoft.Compute/virtualMachines/extensions**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-261">**Microsoft.Compute/virtualMachines/extensions**</span></span>

| <span data-ttu-id="ee6d3-262">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-262">Alias</span></span> | <span data-ttu-id="ee6d3-263">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-263">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-264">Microsoft.Compute/virtualMachines/extensions/publisher</span><span class="sxs-lookup"><span data-stu-id="ee6d3-264">Microsoft.Compute/virtualMachines/extensions/publisher</span></span> | <span data-ttu-id="ee6d3-265">Název sady hello hello rozšíření vydavatele.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-265">Set hello name of hello extension’s publisher.</span></span> |
| <span data-ttu-id="ee6d3-266">Microsoft.Compute/virtualMachines/extensions/type</span><span class="sxs-lookup"><span data-stu-id="ee6d3-266">Microsoft.Compute/virtualMachines/extensions/type</span></span> | <span data-ttu-id="ee6d3-267">Nastavit hello typ rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-267">Set hello type of extension.</span></span> |
| <span data-ttu-id="ee6d3-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="ee6d3-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span></span> | <span data-ttu-id="ee6d3-269">Nastavit verzi hello hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-269">Set hello version of hello extension.</span></span> |

<span data-ttu-id="ee6d3-270">**Microsoft.Compute/virtualMachineScaleSets**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-270">**Microsoft.Compute/virtualMachineScaleSets**</span></span>

| <span data-ttu-id="ee6d3-271">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-271">Alias</span></span> | <span data-ttu-id="ee6d3-272">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-272">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-273">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="ee6d3-273">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="ee6d3-274">Nastavit identifikátor hello hello bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-274">Set hello identifier of hello image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-275">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="ee6d3-275">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="ee6d3-276">Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-276">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-277">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="ee6d3-277">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="ee6d3-278">Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-278">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-279">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="ee6d3-279">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="ee6d3-280">Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-280">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-281">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="ee6d3-281">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="ee6d3-282">Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-282">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="ee6d3-283">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="ee6d3-283">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="ee6d3-284">Nastavení této bitové kopie hello nebo disk je licencovaný místní.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-284">Set that hello image or disk is licensed on-premises.</span></span> <span data-ttu-id="ee6d3-285">Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-285">This value is only used for images that contain hello Windows Server operating system.</span></span> |
| <span data-ttu-id="ee6d3-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="ee6d3-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span></span> | <span data-ttu-id="ee6d3-287">Nastavit hello předpony názvu počítače pro všechny virtuální počítače hello v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-287">Set hello computer name prefix for all  hello virtual machines in hello scale set.</span></span> |
| <span data-ttu-id="ee6d3-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span><span class="sxs-lookup"><span data-stu-id="ee6d3-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span></span> | <span data-ttu-id="ee6d3-289">Nastavte hello identifikátor URI objektu blob bitové kopie uživatele.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-289">Set hello blob URI for user image.</span></span> |
| <span data-ttu-id="ee6d3-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span><span class="sxs-lookup"><span data-stu-id="ee6d3-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span></span> | <span data-ttu-id="ee6d3-291">Nastavit adresy URL hello kontejneru, které jsou použité toostore disky operačního systému pro sadu škálování hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-291">Set hello container URLs that are used toostore operating system disks for hello scale set.</span></span> |
| <span data-ttu-id="ee6d3-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span></span> | <span data-ttu-id="ee6d3-293">Nastavení velikosti hello virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-293">Set hello size of virtual machines in a scale set.</span></span> |
| <span data-ttu-id="ee6d3-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span><span class="sxs-lookup"><span data-stu-id="ee6d3-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span></span> | <span data-ttu-id="ee6d3-295">Nastavit úroveň hello virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-295">Set hello tier of virtual machines in a scale set.</span></span> |
  
<span data-ttu-id="ee6d3-296">**Microsoft.Network/applicationGateways**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-296">**Microsoft.Network/applicationGateways**</span></span>

| <span data-ttu-id="ee6d3-297">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-297">Alias</span></span> | <span data-ttu-id="ee6d3-298">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-298">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-299">Microsoft.Network/applicationGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-299">Microsoft.Network/applicationGateways/sku.name</span></span> | <span data-ttu-id="ee6d3-300">Nastavení velikosti hello hello brány.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-300">Set hello size of hello gateway.</span></span> |

<span data-ttu-id="ee6d3-301">**Microsoft.Network/virtualNetworkGateways**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-301">**Microsoft.Network/virtualNetworkGateways**</span></span>

| <span data-ttu-id="ee6d3-302">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-302">Alias</span></span> | <span data-ttu-id="ee6d3-303">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-303">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span><span class="sxs-lookup"><span data-stu-id="ee6d3-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span></span> | <span data-ttu-id="ee6d3-305">Nastavte typ hello tuto bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-305">Set hello type of this virtual network gateway.</span></span> |
| <span data-ttu-id="ee6d3-306">Microsoft.Network/virtualNetworkGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-306">Microsoft.Network/virtualNetworkGateways/sku.name</span></span> | <span data-ttu-id="ee6d3-307">Nastavte název SKU brány hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-307">Set hello gateway SKU name.</span></span> |

<span data-ttu-id="ee6d3-308">**Microsoft.Sql/servers**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-308">**Microsoft.Sql/servers**</span></span>

| <span data-ttu-id="ee6d3-309">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-309">Alias</span></span> | <span data-ttu-id="ee6d3-310">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-310">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-311">Microsoft.Sql/servers/version</span><span class="sxs-lookup"><span data-stu-id="ee6d3-311">Microsoft.Sql/servers/version</span></span> | <span data-ttu-id="ee6d3-312">Nastavit hello verzi serveru hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-312">Set hello version of hello server.</span></span> |

<span data-ttu-id="ee6d3-313">**Microsoft.Sql/databases**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-313">**Microsoft.Sql/databases**</span></span>

| <span data-ttu-id="ee6d3-314">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-314">Alias</span></span> | <span data-ttu-id="ee6d3-315">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-315">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-316">Microsoft.Sql/servers/databases/edition</span><span class="sxs-lookup"><span data-stu-id="ee6d3-316">Microsoft.Sql/servers/databases/edition</span></span> | <span data-ttu-id="ee6d3-317">Nastavit edici hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-317">Set hello edition of hello database.</span></span> |
| <span data-ttu-id="ee6d3-318">Microsoft.Sql/servers/databases/elasticPoolName</span><span class="sxs-lookup"><span data-stu-id="ee6d3-318">Microsoft.Sql/servers/databases/elasticPoolName</span></span> | <span data-ttu-id="ee6d3-319">Název sady hello hello elastického fondu hello databáze je v.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-319">Set hello name of hello elastic pool hello database is in.</span></span> |
| <span data-ttu-id="ee6d3-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span><span class="sxs-lookup"><span data-stu-id="ee6d3-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span></span> | <span data-ttu-id="ee6d3-321">Nastavit hello nakonfigurované služby cíle ID úrovně hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-321">Set hello configured service level objective ID of hello database.</span></span> |
| <span data-ttu-id="ee6d3-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span><span class="sxs-lookup"><span data-stu-id="ee6d3-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span></span> | <span data-ttu-id="ee6d3-323">Název sady hello hello nakonfigurované cíle na úrovni služby databáze hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-323">Set hello name of hello configured service level objective of hello database.</span></span>  |

<span data-ttu-id="ee6d3-324">**Microsoft.Sql/elasticpools**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-324">**Microsoft.Sql/elasticpools**</span></span>

| <span data-ttu-id="ee6d3-325">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-325">Alias</span></span> | <span data-ttu-id="ee6d3-326">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-326">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-327">servery nebo elasticpools</span><span class="sxs-lookup"><span data-stu-id="ee6d3-327">servers/elasticpools</span></span> | <span data-ttu-id="ee6d3-328">Microsoft.Sql/servers/elasticPools/dtu</span><span class="sxs-lookup"><span data-stu-id="ee6d3-328">Microsoft.Sql/servers/elasticPools/dtu</span></span> | <span data-ttu-id="ee6d3-329">Sada hello celkem sdílené DTU fondu elastické databáze hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-329">Set hello total shared DTU for hello database elastic pool.</span></span> |
| <span data-ttu-id="ee6d3-330">servery nebo elasticpools</span><span class="sxs-lookup"><span data-stu-id="ee6d3-330">servers/elasticpools</span></span> | <span data-ttu-id="ee6d3-331">Microsoft.Sql/servers/elasticPools/edition</span><span class="sxs-lookup"><span data-stu-id="ee6d3-331">Microsoft.Sql/servers/elasticPools/edition</span></span> | <span data-ttu-id="ee6d3-332">Nastavit edici hello hello elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-332">Set hello edition of hello elastic pool.</span></span> |

<span data-ttu-id="ee6d3-333">**Microsoft.Storage/storageAccounts.**</span><span class="sxs-lookup"><span data-stu-id="ee6d3-333">**Microsoft.Storage/storageAccounts**</span></span>

| <span data-ttu-id="ee6d3-334">Alias</span><span class="sxs-lookup"><span data-stu-id="ee6d3-334">Alias</span></span> | <span data-ttu-id="ee6d3-335">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6d3-335">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ee6d3-336">Microsoft.Storage/storageAccounts/accessTier</span><span class="sxs-lookup"><span data-stu-id="ee6d3-336">Microsoft.Storage/storageAccounts/accessTier</span></span> | <span data-ttu-id="ee6d3-337">Úroveň přístupu hello sady používané pro fakturaci.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-337">Set hello access tier used for billing.</span></span> |
| <span data-ttu-id="ee6d3-338">Microsoft.Storage/storageAccounts/accountType</span><span class="sxs-lookup"><span data-stu-id="ee6d3-338">Microsoft.Storage/storageAccounts/accountType</span></span> | <span data-ttu-id="ee6d3-339">Název SKU hello sady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-339">Set hello SKU name.</span></span> |
| <span data-ttu-id="ee6d3-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span><span class="sxs-lookup"><span data-stu-id="ee6d3-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span></span> | <span data-ttu-id="ee6d3-341">Nastavte, jestli služba hello šifruje hello data, jak je uložen v úložišti služby objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-341">Set whether hello service encrypts hello data as it is stored in hello blob storage service.</span></span> |
| <span data-ttu-id="ee6d3-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span><span class="sxs-lookup"><span data-stu-id="ee6d3-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span></span> | <span data-ttu-id="ee6d3-343">Nastavte, jestli služba hello šifruje hello data, jak je uložen v úložišti služby hello souboru.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-343">Set whether hello service encrypts hello data as it is stored in hello file storage service.</span></span> |
| <span data-ttu-id="ee6d3-344">Microsoft.Storage/storageAccounts/sku.name</span><span class="sxs-lookup"><span data-stu-id="ee6d3-344">Microsoft.Storage/storageAccounts/sku.name</span></span> | <span data-ttu-id="ee6d3-345">Název SKU hello sady.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-345">Set hello SKU name.</span></span> |
| <span data-ttu-id="ee6d3-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span><span class="sxs-lookup"><span data-stu-id="ee6d3-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span></span> | <span data-ttu-id="ee6d3-347">Nastavení tooallow pouze https služby toostorage provoz.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-347">Set tooallow only https traffic toostorage service.</span></span> |


## <a name="policy-examples"></a><span data-ttu-id="ee6d3-348">Příklady zásad</span><span class="sxs-lookup"><span data-stu-id="ee6d3-348">Policy examples</span></span>

<span data-ttu-id="ee6d3-349">Hello následující témata obsahují příklady zásad:</span><span class="sxs-lookup"><span data-stu-id="ee6d3-349">hello following topics contain policy examples:</span></span>

* <span data-ttu-id="ee6d3-350">Příklady značky zásady, najdete v části [zásad prostředků pro značky](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-350">For examples of tag polices, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>
* <span data-ttu-id="ee6d3-351">Příklady vzory pojmenovávání a text najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-351">For examples of naming and text patterns, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>
* <span data-ttu-id="ee6d3-352">Příklady úložiště zásad najdete v tématu [použít účty toostorage zásady prostředků](resource-manager-policy-storage.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-352">For examples of storage policies, see [Apply resource policies toostorage accounts](resource-manager-policy-storage.md).</span></span>
* <span data-ttu-id="ee6d3-353">Příklady zásad virtuálního počítače najdete v tématu [použít zásady tooLinux prostředků virtuálních počítačů](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) a [použít zásady tooWindows prostředků virtuálních počítačů](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="ee6d3-353">For examples of virtual machine policies, see [Apply resource policies tooLinux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) and [Apply resource policies tooWindows VMs](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span></span>


## <a name="next-steps"></a><span data-ttu-id="ee6d3-354">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee6d3-354">Next steps</span></span>
* <span data-ttu-id="ee6d3-355">Po definování pravidla zásad, přiřaďte ji tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="ee6d3-355">After defining a policy rule, assign it tooa scope.</span></span> <span data-ttu-id="ee6d3-356">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-356">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="ee6d3-357">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-357">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="ee6d3-358">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-358">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="ee6d3-359">schéma zásad Hello je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="ee6d3-359">hello policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

