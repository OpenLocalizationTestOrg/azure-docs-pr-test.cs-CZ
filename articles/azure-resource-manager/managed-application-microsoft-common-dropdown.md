---
title: "element uživatelského rozhraní rozevírací seznam spravovaných aplikací aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.DropDown uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 1c07a48ad66b8e8b7fd8f59561776ecb1fc6224f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommondropdown-ui-element"></a>Element Microsoft.Common.DropDown uživatelského rozhraní
Výběr ovládacího prvku s rozevíracím seznamu. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Popisek Hello `constraints.allowedValues` je hello zobrazený text pro položku, a jeho hodnota může být hodnota výstup hello hello elementu při výběru.
- -Li zadána, hello výchozí hodnota musí být součástí štítek `constraints.allowedValues`. Pokud není zadaný, hello první položky v `constraints.allowedValues` je vybrána. Hello výchozí hodnota je **null**.
- `constraints.allowedValues`musí obsahovat alespoň jednu položku.
- Tento element nepodporuje hello `constraints.required` vlastnost. tooemulate toto chování přidat položku s popisek a hodnota `""` (prázdný řetězec) příliš`constraints.allowedValues`.

## <a name="sample-output"></a>Ukázkový výstup
```json
"Bar"
```

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
