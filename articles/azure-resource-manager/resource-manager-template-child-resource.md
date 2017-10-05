---
title: "Definování podřízených prostředků v šablony Azure | Microsoft Docs"
description: "Ukazuje, jak nastavit typ prostředku a název pro podřízený prostředek v šablonu Azure Resource Manager"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a><span data-ttu-id="ce58e-103">Nastavte název a typ pro podřízený prostředek v šabloně Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ce58e-103">Set name and type for child resource in Resource Manager template</span></span>
<span data-ttu-id="ce58e-104">Při vytváření šablony, musíte často obsahují podřízený prostředek, který souvisí se nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="ce58e-104">When creating a template, you frequently need to include a child resource that is related to a parent resource.</span></span> <span data-ttu-id="ce58e-105">Vaše šablona může obsahovat třeba SQL server a databáze.</span><span class="sxs-lookup"><span data-stu-id="ce58e-105">For example, your template may include a SQL server and a database.</span></span> <span data-ttu-id="ce58e-106">SQL server je nadřazený prostředek a databáze je podřízený prostředek.</span><span class="sxs-lookup"><span data-stu-id="ce58e-106">The SQL server is the parent resource, and the database is the child resource.</span></span> 

<span data-ttu-id="ce58e-107">Není ve formátu podřízený typ prostředku:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span><span class="sxs-lookup"><span data-stu-id="ce58e-107">The format of the child resource type is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span></span>

<span data-ttu-id="ce58e-108">Formát názvu podřízené prostředků je:`{parent-resource-name}/{child-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="ce58e-108">The format of the child resource name is: `{parent-resource-name}/{child-resource-name}`</span></span>

<span data-ttu-id="ce58e-109">Však zadejte název a typ v šabloně různě v závislosti na tom, jestli je vnořen do nadřazený prostředek, nebo na vlastní na nejvyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="ce58e-109">However, you specify the type and name in a template differently based on whether it is nested within the parent resource, or on its own at the top level.</span></span> <span data-ttu-id="ce58e-110">Toto téma ukazuje, jak zpracovat obou přístupů.</span><span class="sxs-lookup"><span data-stu-id="ce58e-110">This topic shows how to handle both approaches.</span></span>

<span data-ttu-id="ce58e-111">Při vytváření plně kvalifikovaný odkaz na prostředek, není jednoduše zřetězení těchto dvou pořadí kombinovat segmenty z typu a název.</span><span class="sxs-lookup"><span data-stu-id="ce58e-111">When constructing a fully qualified reference to a resource, the order to combine segments from the type and name  is not simply a concatenation of the two.</span></span>  <span data-ttu-id="ce58e-112">Místo toho po oboru názvů, použijte posloupnost *nebo název typu* dvojice z nejméně specifická k nejvíce:</span><span class="sxs-lookup"><span data-stu-id="ce58e-112">Instead, after the namespace, use a sequence of *type/name* pairs from least specific to most specific:</span></span>

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

<span data-ttu-id="ce58e-113">Například:</span><span class="sxs-lookup"><span data-stu-id="ce58e-113">For example:</span></span>

<span data-ttu-id="ce58e-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`správnost `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` není správný</span><span class="sxs-lookup"><span data-stu-id="ce58e-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` is correct `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is not correct</span></span>

## <a name="nested-child-resource"></a><span data-ttu-id="ce58e-115">Vnořené podřízené prostředků</span><span class="sxs-lookup"><span data-stu-id="ce58e-115">Nested child resource</span></span>
<span data-ttu-id="ce58e-116">Nejjednodušší způsob, jak definovat podřízených prostředků je vnořit v nadřazeném prostředku.</span><span class="sxs-lookup"><span data-stu-id="ce58e-116">The easiest way to define a child resource is to nest it within the parent resource.</span></span> <span data-ttu-id="ce58e-117">Následující příklad ukazuje vnořené v systému SQL Server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="ce58e-117">The following example shows a SQL database nested within in a SQL Server.</span></span>

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

<span data-ttu-id="ce58e-118">Pro podřízený prostředek, je typ nastaven `databases` , ale jeho typ prostředku úplné `Microsoft.Sql/servers/databases`.</span><span class="sxs-lookup"><span data-stu-id="ce58e-118">For the child resource, the type is set to `databases` but its full resource type is `Microsoft.Sql/servers/databases`.</span></span> <span data-ttu-id="ce58e-119">Nezadáte `Microsoft.Sql/servers/` se předpokládá z nadřazeného typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="ce58e-119">You do not provide `Microsoft.Sql/servers/` because it is assumed from the parent resource type.</span></span> <span data-ttu-id="ce58e-120">Název prostředku podřízené je nastaven `exampledatabase` ale úplný název obsahuje název nadřazené.</span><span class="sxs-lookup"><span data-stu-id="ce58e-120">The child resource name is set to `exampledatabase` but the full name includes the parent name.</span></span> <span data-ttu-id="ce58e-121">Nezadáte `exampleserver` se předpokládá z nadřazené prostředku.</span><span class="sxs-lookup"><span data-stu-id="ce58e-121">You do not provide `exampleserver` because it is assumed from the parent resource.</span></span>

## <a name="top-level-child-resource"></a><span data-ttu-id="ce58e-122">Nejvyšší úrovně podřízené prostředků</span><span class="sxs-lookup"><span data-stu-id="ce58e-122">Top-level child resource</span></span>
<span data-ttu-id="ce58e-123">Můžete definovat podřízených prostředků na nejvyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="ce58e-123">You can define the child resource at the top level.</span></span> <span data-ttu-id="ce58e-124">Tento postup můžete použít, pokud nadřazený prostředek není nasazený ve stejné šablony, nebo pokud chcete použít `copy` vytvořit více podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="ce58e-124">You might use this approach if the parent resource is not deployed in the same template, or if want to use `copy` to create multiple child resources.</span></span> <span data-ttu-id="ce58e-125">S tímto přístupem musíte zadat typ prostředku úplné a zahrnout název nadřazené prostředku v názvu prostředku podřízené.</span><span class="sxs-lookup"><span data-stu-id="ce58e-125">With this approach, you must provide the full resource type, and include the parent resource name in the child resource name.</span></span>

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

<span data-ttu-id="ce58e-126">Databáze je podřízený prostředek k serveru, i když jsou definovány na stejné úrovni v šabloně.</span><span class="sxs-lookup"><span data-stu-id="ce58e-126">The database is a child resource to the server even though they are defined on the same level in the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce58e-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce58e-127">Next steps</span></span>
* <span data-ttu-id="ce58e-128">Doporučení, jak vytvořit šablony, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="ce58e-128">For recommendations about how to create templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>
* <span data-ttu-id="ce58e-129">Příklad vytvoření několika podřízenými prostředky, najdete v části [nasadit několik instancí prostředků v šablonách Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="ce58e-129">For an example of creating multiple child resources, see [Deploy multiple instances of resources in Azure Resource Manager templates](resource-group-create-multiple.md).</span></span>
