---
title: "Azure spravované aplikace PublicIpAddressCombo elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Network.PublicIpAddressCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 2eb773f5f0cf389fc39bc3a0f5fbf9ac726d1949
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Element Microsoft.Network.PublicIpAddressCombo uživatelského rozhraní
Skupina ovládacích prvků pro výběr nový nebo existující veřejnou IP adresu. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Pokud uživatel vybere 'None' pro veřejnou IP adresu, název domény popisek textového pole Skrytá.
- Pokud uživatel vybere stávající veřejnou IP adresu, textového pole Popisek názvu domény je zakázané. Jeho hodnota může být popisek názvu domény vybraného IP adresy.
- Aktualizace pro přípony (například westus.cloudapp.azure.com) název se domény, automaticky v závislosti na vybraném umístění.

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "foobar"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Pokud `constraints.required.domainNameLabel` je nastaven na **true**, musí uživatel zadat popisek názvu domény, při vytváření nové veřejnou IP adresu. Existující veřejné IP adresy bez štítek nejsou k dispozici pro výběr.
- Pokud `options.hideNone` je nastaven na **true**, pak možnost vybrat **žádné** pro veřejnou IP adresu skryt. Výchozí hodnota je **false**.
- Pokud `options.hideDomainNameLabel` je nastaven na **true**, textové pole pro popisek názvu domény je skrytý. Výchozí hodnota je **false**.
- Pokud `options.hideExisting` má hodnotu true, pak uživatel není možné vybrat stávající veřejnou IP adresu. Výchozí hodnota je **false**.

## <a name="sample-output"></a>Ukázkový výstup
Pokud uživatel vybere žádné veřejnou IP adresu, se předpokládá, že následující výstup:
```json
{
  "newOrExistingOrNone": "none"
}
```

Pokud uživatel vybere nový nebo existující IP adresu, se předpokládá, že následující výstup:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- Když `options.hideNone` není zadaný, `newOrExistingOrNone` vždy vrátí hodnotu **žádné**.
- Když `options.hideDomainNameLabel` není zadaný, `domainNameLabel` není deklarován.

## <a name="next-steps"></a>Další kroky
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
