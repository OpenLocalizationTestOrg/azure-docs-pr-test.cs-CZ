---
title: "element uživatelského rozhraní PublicIpAddressCombo spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Network.PublicIpAddressCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 8ba689005c0eccda0a57bf628de4b5197886a950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Element Microsoft.Network.PublicIpAddressCombo uživatelského rozhraní
Skupina ovládacích prvků pro výběr nový nebo existující veřejnou IP adresu. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Pokud uživatel hello vybere 'None' pro veřejnou IP adresu, hello domény název popisku textového pole Skrytá.
- Pokud uživatel hello vybere stávající veřejnou IP adresu, do textového pole domény popisek hello je zakázané. Jeho hodnota může být popisek názvu domény hello hello vybrané IP adresy.
- aktualizace přípony (například westus.cloudapp.azure.com) název domény Hello automaticky na základě hello vybrané umístění.

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
- Pokud `constraints.required.domainNameLabel` je nastaven příliš**true**, hello uživatel musí poskytnout Popisek názvu domény, při vytváření nové veřejnou IP adresu. Existující veřejné IP adresy bez štítek nejsou k dispozici pro výběr.
- Pokud `options.hideNone` je nastaven příliš**true**, pak hello možnost tooselect **žádné** pro veřejnou IP adresu hello adresu skryt. Hello výchozí hodnota je **false**.
- Pokud `options.hideDomainNameLabel` je nastaven příliš**true**, hello textové pole pro popisek názvu domény je skrytý. Hello výchozí hodnota je **false**.
- Pokud `options.hideExisting` má hodnotu true, pak uživatel hello není možné toochoose stávající veřejnou IP adresu. Hello výchozí hodnota je **false**.

## <a name="sample-output"></a>Ukázkový výstup
Pokud uživatel hello vybere žádné veřejnou IP adresu, hello následující výstup se předpokládá, že:
```json
{
  "newOrExistingOrNone": "none"
}
```

Pokud uživatel hello vybere nový nebo existující IP adresu, hello následující výstup se předpokládá, že:
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
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
