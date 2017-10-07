---
title: "element uživatelského rozhraní SizeSelector spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Compute.SizeSelector uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d93306135d9c6f9a83692766ce1ca7ea2b688086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
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
- `recommendedSizes`musí obsahovat alespoň jeden velikost. Hello nejprve doporučená velikost je používat jako výchozí hello.
- Pokud doporučená velikost není k dispozici v umístění hello vybrané, se automaticky přeskočí hello velikost. Místo toho hello Další doporučená velikost je použít.
- Jakékoli velikosti, nebyly zadány v hello `constraints.allowedSizes` skryt a jakékoli velikosti, nebyly zadány v `constraints.excludedSizes` se zobrazí.
`constraints.allowedSizes`a `constraints.excludedSizes` obě jsou nepovinné, ale nelze používat současně. můžete určit Hello seznam dostupných velikostí voláním [seznam dostupných velikostí virtuálních počítačů pro předplatné](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**. Použije se náklady na hardware hello toodetermine hello virtuálních počítačů.
- `imageReference`pro první strany Image, ale zadaná pro třetí strany bitové kopie je vynechán. Použije se náklady na software hello toodetermine hello virtuálních počítačů.
- `count`je použité tooset hello odpovídající multiplikátor pro hello element. Podporuje statické hodnoty, jako je třeba **2**, nebo jako dynamické hodnoty z jiný element `[steps('step1').vmCount]`. Hello výchozí hodnota je **1**.

## <a name="sample-output"></a>Ukázkový výstup
```json
"Standard_D1"
```

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
