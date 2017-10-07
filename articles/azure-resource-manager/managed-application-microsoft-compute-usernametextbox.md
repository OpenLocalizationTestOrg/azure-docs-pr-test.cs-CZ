---
title: "element uživatelského rozhraní UserNameTextBox spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Compute.UserNameTextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 33092014e804c4aabd56ba49144d9cd4d6a5fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Element Microsoft.Compute.UserNameTextBox uživatelského rozhraní
Ovládací prvek textové pole s integrované ověřování systému Windows a Linux uživatelská jména. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Pokud `constraints.required` je nastaven příliš**true**, pak hello textového pole musí obsahovat hodnotu toovalidate úspěšně. Hello výchozí hodnota je **true**.
- `osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.
- `constraints.regex`je vzor regulárního výrazu jazyka JavaScript. -Li zadána, pak hello textového pole Hodnota musí odpovídat hello vzor toovalidate úspěšně. Výchozí hodnota je **null**.
- `constraints.validationMessage`je toodisplay řetězec, pokud se nezdaří ověření hello určeného hello jeho hodnotu `constraints.regex`. Pokud není zadaný, bude hello textového pole integrované ověřování, které se používají zprávy. Hello výchozí hodnota je **null**.
- Tento element má integrované ověřování, který je založen na hello hodnota zadaná pro `osPlatform`. Hello integrované ověřování můžete použít vlastní regulární výraz.
Pokud nezadáte hodnotu `constraints.regex` je zadán, pak oba hello předdefinované a vlastní ověření se aktivují.

## <a name="sample-output"></a>Ukázkový výstup
```json
"tabrezm"
```

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
