---
title: "Azure spravované aplikace SizeSelector elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Compute.SizeSelector uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 72278b1999f89e5bd5f203794ba3a403a695c933
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Element Microsoft.Compute.SizeSelector uživatelského rozhraní
Ovládací prvek pro výběr velikost pro jeden nebo více instancí virtuálního počítače. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- `recommendedSizes`musí obsahovat alespoň jeden velikost. První doporučená velikost je použita jako výchozí.
- Pokud doporučená velikost není k dispozici ve vybraném umístění, velikost automaticky přeskočen. Místo toho se používá další doporučená velikost.
- Jakékoli velikosti, nebyly zadány v `constraints.allowedSizes` skryt a jakékoli velikosti, nebyly zadány v `constraints.excludedSizes` se zobrazí.
`constraints.allowedSizes`a `constraints.excludedSizes` obě jsou nepovinné, ale nelze používat současně. Seznam dostupných velikostí se dá určit pomocí volání [seznam dostupných velikostí virtuálních počítačů pro předplatné](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**. Slouží k určení náklady na hardware virtuálních počítačů.
- `imageReference`pro první strany Image, ale zadaná pro třetí strany bitové kopie je vynechán. Slouží k určení náklady na software virtuálních počítačů.
- `count`slouží k nastavení odpovídající multiplikátor pro element. Podporuje statické hodnoty, jako je třeba **2**, nebo jako dynamické hodnoty z jiný element `[steps('step1').vmCount]`. Výchozí hodnota je **1**.

## <a name="sample-output"></a>Ukázkový výstup
```json
"Standard_D1"
```

## <a name="next-steps"></a>Další kroky
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](create-uidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](create-uidefinition-elements.md).
