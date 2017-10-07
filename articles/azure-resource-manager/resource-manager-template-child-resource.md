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
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Nastavte název a typ pro podřízený prostředek v šabloně Resource Manager
Při vytváření šablony, je často nutné tooinclude podřízené prostředku, který je související tooa nadřazený prostředek. Vaše šablona může obsahovat třeba SQL server a databáze. Hello SQL server je hello nadřazený prostředek a hello databáze je hello podřízených prostředků. 

Formát Hello typu prostředku podřízené hello je:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Hello formát názvu prostředku podřízené hello je:`{parent-resource-name}/{child-resource-name}`

Můžete však zadat hello typ a název v šabloně různě v závislosti na tom, jestli je vnořen do hello nadřazený prostředek, nebo na vlastní na nejvyšší úrovni hello. Toto téma ukazuje, jak se blíží toohandle obou.

Při vytváření prostředku tooa plně kvalifikovaný odkaz, hello pořadí toocombine segmenty ze hello typ a název je jednoduše zřetězení hello dva.  Místo toho po hello oboru názvů, použijte posloupnost *nebo název typu* páry z nejméně specifická toomost konkrétní:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Například:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`správnost `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` není správný

## <a name="nested-child-resource"></a>Vnořené podřízené prostředků
Nejjednodušší způsob, jak toodefine Hello podřízených prostředků je toonest ho v rámci hello nadřazený prostředek. Hello následující příklad ukazuje vnořené v systému SQL Server databáze SQL.

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

Pro podřízený prostředek hello, typ hello je nastaven příliš`databases` , ale jeho typ prostředku úplné `Microsoft.Sql/servers/databases`. Nezadáte `Microsoft.Sql/servers/` se předpokládá z typu prostředek nadřazené hello. název prostředku podřízené Hello je nastaven příliš`exampledatabase` ale hello úplný název obsahuje název nadřazené hello. Nezadáte `exampleserver` se předpokládá z hello nadřazený prostředek.

## <a name="top-level-child-resource"></a>Nejvyšší úrovně podřízené prostředků
Můžete definovat hello podřízených prostředků na nejvyšší úrovni hello. Můžete použít tuto metodu v případě hello nadřazený prostředek není nasazený ve hello stejné šablony, nebo, pokud chcete toouse `copy` toocreate více podřízené prostředky. S tímto přístupem musíte zadat typ prostředku úplné hello a zahrnout název prostředku nadřazené hello název prostředku podřízené hello.

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

Hello databáze je server toohello podřízených prostředků, i když jsou definovány na stejné úrovni v šabloně hello hello.

## <a name="next-steps"></a>Další kroky
* Pro doporučení, jak toocreate šablony, viz [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).
* Příklad vytvoření několika podřízenými prostředky, najdete v části [nasadit několik instancí prostředků v šablonách Azure Resource Manager](resource-group-create-multiple.md).
