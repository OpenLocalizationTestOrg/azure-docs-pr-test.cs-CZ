---
title: "aaaDefine podřízený prostředek šablony Azure | Microsoft Docs"
description: "Ukazuje, jak tooset hello typ prostředku a název pro podřízený prostředek v šablonu Azure Resource Manager"
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
ms.openlocfilehash: c502c589100d7ae864d7fb01b5ba10ddfaf92592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a><span data-ttu-id="0fb54-103">Nastavte název a typ pro podřízený prostředek v šabloně Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0fb54-103">Set name and type for child resource in Resource Manager template</span></span>
<span data-ttu-id="0fb54-104">Při vytváření šablony, je často nutné tooinclude podřízené prostředku, který je související tooa nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="0fb54-104">When creating a template, you frequently need tooinclude a child resource that is related tooa parent resource.</span></span> <span data-ttu-id="0fb54-105">Vaše šablona může obsahovat třeba SQL server a databáze.</span><span class="sxs-lookup"><span data-stu-id="0fb54-105">For example, your template may include a SQL server and a database.</span></span> <span data-ttu-id="0fb54-106">Hello SQL server je hello nadřazený prostředek a hello databáze je hello podřízených prostředků.</span><span class="sxs-lookup"><span data-stu-id="0fb54-106">hello SQL server is hello parent resource, and hello database is hello child resource.</span></span> 

<span data-ttu-id="0fb54-107">Formát Hello typu prostředku podřízené hello je:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span><span class="sxs-lookup"><span data-stu-id="0fb54-107">hello format of hello child resource type is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span></span>

<span data-ttu-id="0fb54-108">Hello formát názvu prostředku podřízené hello je:`{parent-resource-name}/{child-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="0fb54-108">hello format of hello child resource name is: `{parent-resource-name}/{child-resource-name}`</span></span>

<span data-ttu-id="0fb54-109">Můžete však zadat hello typ a název v šabloně různě v závislosti na tom, jestli je vnořen do hello nadřazený prostředek, nebo na vlastní na nejvyšší úrovni hello.</span><span class="sxs-lookup"><span data-stu-id="0fb54-109">However, you specify hello type and name in a template differently based on whether it is nested within hello parent resource, or on its own at hello top level.</span></span> <span data-ttu-id="0fb54-110">Toto téma ukazuje, jak se blíží toohandle obou.</span><span class="sxs-lookup"><span data-stu-id="0fb54-110">This topic shows how toohandle both approaches.</span></span>

<span data-ttu-id="0fb54-111">Při vytváření prostředku tooa plně kvalifikovaný odkaz, hello pořadí toocombine segmenty ze hello typ a název je jednoduše zřetězení hello dva.</span><span class="sxs-lookup"><span data-stu-id="0fb54-111">When constructing a fully qualified reference tooa resource, hello order toocombine segments from hello type and name  is not simply a concatenation of hello two.</span></span>  <span data-ttu-id="0fb54-112">Místo toho po hello oboru názvů, použijte posloupnost *nebo název typu* páry z nejméně specifická toomost konkrétní:</span><span class="sxs-lookup"><span data-stu-id="0fb54-112">Instead, after hello namespace, use a sequence of *type/name* pairs from least specific toomost specific:</span></span>

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

<span data-ttu-id="0fb54-113">Například:</span><span class="sxs-lookup"><span data-stu-id="0fb54-113">For example:</span></span>

<span data-ttu-id="0fb54-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`správnost `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` není správný</span><span class="sxs-lookup"><span data-stu-id="0fb54-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` is correct `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is not correct</span></span>

## <a name="nested-child-resource"></a><span data-ttu-id="0fb54-115">Vnořené podřízené prostředků</span><span class="sxs-lookup"><span data-stu-id="0fb54-115">Nested child resource</span></span>
<span data-ttu-id="0fb54-116">Nejjednodušší způsob, jak toodefine Hello podřízených prostředků je toonest ho v rámci hello nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="0fb54-116">hello easiest way toodefine a child resource is toonest it within hello parent resource.</span></span> <span data-ttu-id="0fb54-117">Hello následující příklad ukazuje vnořené v systému SQL Server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="0fb54-117">hello following example shows a SQL database nested within in a SQL Server.</span></span>

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

<span data-ttu-id="0fb54-118">Pro podřízený prostředek hello, typ hello je nastaven příliš`databases` , ale jeho typ prostředku úplné `Microsoft.Sql/servers/databases`.</span><span class="sxs-lookup"><span data-stu-id="0fb54-118">For hello child resource, hello type is set too`databases` but its full resource type is `Microsoft.Sql/servers/databases`.</span></span> <span data-ttu-id="0fb54-119">Nezadáte `Microsoft.Sql/servers/` se předpokládá z typu prostředek nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="0fb54-119">You do not provide `Microsoft.Sql/servers/` because it is assumed from hello parent resource type.</span></span> <span data-ttu-id="0fb54-120">název prostředku podřízené Hello je nastaven příliš`exampledatabase` ale hello úplný název obsahuje název nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="0fb54-120">hello child resource name is set too`exampledatabase` but hello full name includes hello parent name.</span></span> <span data-ttu-id="0fb54-121">Nezadáte `exampleserver` se předpokládá z hello nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="0fb54-121">You do not provide `exampleserver` because it is assumed from hello parent resource.</span></span>

## <a name="top-level-child-resource"></a><span data-ttu-id="0fb54-122">Nejvyšší úrovně podřízené prostředků</span><span class="sxs-lookup"><span data-stu-id="0fb54-122">Top-level child resource</span></span>
<span data-ttu-id="0fb54-123">Můžete definovat hello podřízených prostředků na nejvyšší úrovni hello.</span><span class="sxs-lookup"><span data-stu-id="0fb54-123">You can define hello child resource at hello top level.</span></span> <span data-ttu-id="0fb54-124">Můžete použít tuto metodu v případě hello nadřazený prostředek není nasazený ve hello stejné šablony, nebo, pokud chcete toouse `copy` toocreate více podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="0fb54-124">You might use this approach if hello parent resource is not deployed in hello same template, or if want toouse `copy` toocreate multiple child resources.</span></span> <span data-ttu-id="0fb54-125">S tímto přístupem musíte zadat typ prostředku úplné hello a zahrnout název prostředku nadřazené hello název prostředku podřízené hello.</span><span class="sxs-lookup"><span data-stu-id="0fb54-125">With this approach, you must provide hello full resource type, and include hello parent resource name in hello child resource name.</span></span>

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

<span data-ttu-id="0fb54-126">Hello databáze je server toohello podřízených prostředků, i když jsou definovány na stejné úrovni v šabloně hello hello.</span><span class="sxs-lookup"><span data-stu-id="0fb54-126">hello database is a child resource toohello server even though they are defined on hello same level in hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0fb54-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fb54-127">Next steps</span></span>
* <span data-ttu-id="0fb54-128">Pro doporučení, jak toocreate šablony, viz [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="0fb54-128">For recommendations about how toocreate templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>
* <span data-ttu-id="0fb54-129">Příklad vytvoření několika podřízenými prostředky, najdete v části [nasadit několik instancí prostředků v šablonách Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="0fb54-129">For an example of creating multiple child resources, see [Deploy multiple instances of resources in Azure Resource Manager templates](resource-group-create-multiple.md).</span></span>
