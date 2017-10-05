---
title: "Azure spravované aplikace SizeSelector elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Compute.SizeSelector uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: e54962f73028ada258a7faef271d66f0fbcef649
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Element Microsoft.Compute.SizeSelector uživatelského rozhraní
Ovládací prvek pro výběr velikost pro jeden nebo více instancí virtuálního počítače. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

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
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
