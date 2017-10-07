---
title: "aaaAzure spravované aplikace část elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.Section uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d20b365b12fab66177e1a12db2ebbeefe507212e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonsection-ui-element"></a>Element Microsoft.Common.Section uživatelského rozhraní
Ovládací prvek, který seskupuje jeden či více elementů pod nadpisem. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- `elements`musí obsahovat alespoň jeden element a může obsahovat všechny typy element kromě `Microsoft.Common.Section`.
- Tento element nepodporuje hello `toolTip` vlastnost.

## <a name="sample-output"></a>Ukázkový výstup
hodnoty elementů v výstup tooaccess hello `elements`, použijte hello [basics()](managed-application-createuidefinition-functions.md#basics) nebo [steps()](managed-application-createuidefinition-functions.md#steps) funkce a s tečkami:

```json
basics('section1').element1
```

Elementy typu `Microsoft.Common.Section` mít žádné hodnoty výstup, sami.

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
