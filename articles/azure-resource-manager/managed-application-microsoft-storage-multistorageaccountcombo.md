---
title: "element uživatelského rozhraní MultiStorageAccountCombo spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Storage.MultiStorageAccountCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 765be145b61c3dbf0a035a7a00aa18eee464a3eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Element Microsoft.Storage.MultiStorageAccountCombo uživatelského rozhraní
Skupina ovládacích prvků pro vytváření více účtů úložiště, jejichž názvy začínají s předponou běžné. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Hello hodnota `defaultValue.prefix` je zřetězen s jeden nebo více celých čísel toogenerate hello posloupnost názvy účtů úložiště. Například pokud `defaultValue.prefix` je **foobar** a `count` je **2**, pak názvy účtů úložiště **foobar1** a **foobar2** se generují. Názvy účtů úložiště generovaného ověření jedinečnosti automaticky.
- názvy účtů úložiště Hello jsou generovány lexicographically podle `count`. Například pokud `count` je 10 a názvy účtů úložiště hello končit celá čísla 2 číslice (01, 02, 03, atd.).
- Výchozí hodnota pro Hello `defaultValue.prefix` je **null**a pro `defaultValue.type` je **Premium_LRS**.
- Žádný typ, nebyly zadány v `constraints.allowedTypes` skryt a jakýmikoli nebyly zadány v `constraints.excludedTypes` se zobrazí.
`constraints.allowedTypes`a `constraints.excludedTypes` obě jsou nepovinné, ale nelze používat současně.
- V přidání toogenerating názvy účtů úložiště `count` je použité tooset odpovídající multiplikátor pro hello element. Podporuje statické hodnoty, jako je třeba **2**, nebo jako dynamické hodnoty z jiný element `[steps('step1').storageAccountCount]`. Hello výchozí hodnota je **1**.

## <a name="sample-output"></a>Ukázkový výstup
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
