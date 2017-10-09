---
title: "aaaAzure spravované aplikace textového elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.TextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 11771cd1d689b720384df98b8d1465703068af37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommontextbox-ui-element"></a>Element Microsoft.Common.TextBox uživatelského rozhraní
Ovládací prvek, který lze použít tooedit neformátovaný text. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Pokud `constraints.required` je nastaven příliš**true**, pak hello textového pole musí obsahovat hodnotu toovalidate úspěšně. Hello výchozí hodnota je **false**.
- `constraints.regex`je vzor regulárního výrazu jazyka JavaScript. -Li zadána, pak hello textového pole Hodnota musí odpovídat hello vzor toovalidate úspěšně. Výchozí hodnota je **null**.
- `constraints.validationMessage`je řetězec toodisplay při ověřování se nezdaří, hello jeho hodnotu. Pokud není zadaný, bude hello textového pole integrované ověřování, které se používají zprávy. Hello výchozí hodnota je **null**.
- Jeho možných toospecify hodnota pro `constraints.regex` při `constraints.required` je nastaven příliš**false**. V tomto scénáři hodnota není potřeba hello textové pole toovalidate úspěšně. Pokud je zadaná, musí se shodovat hello vzor regulárního výrazu.

## <a name="sample-output"></a>Ukázkový výstup

```json
"foobar"
```

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
