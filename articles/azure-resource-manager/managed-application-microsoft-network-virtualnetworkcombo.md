---
title: "Azure spravované aplikace VirtualNetworkCombo elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Network.VirtualNetworkCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 8bb255b76ac5c3de570fa569a1cfb3ee953f9687
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Element Microsoft.Network.VirtualNetworkCombo uživatelského rozhraní
Skupina ovládacích prvků pro výběr nový nebo existující virtuální síť. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- V horní obrázek uživatel vybral nové virtuální sítě, takže uživatel může přizpůsobit název a adresu předpona každé podsítě. Konfigurace podsítí v tomto případě je volitelné.
- V dolní obrázek uživatel vybral existující virtuální síť, takže uživatel musí být mapována každou podsíť, kterou šablonu nasazení vyžaduje existující podsítí. Podsítě v takovém případě se vyžaduje konfigurace.

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- -Li zadána, první nepřekrývají adres předpony velikosti `defaultValue.addressPrefixSize` je určen automaticky v závislosti na existující virtuální sítě v rámci předplatného uživatele.
- Výchozí hodnota pro `defaultValue.name` a `defaultValue.addressPrefixSize` je **null**.
- `constraints.minAddressPrefixSize`musí být zadán. Žádné existující virtuální sítě s adresním prostorem menší než zadaná hodnota jsou k dispozici pro výběr.
- `subnets`musí být zadán, a `constraints.minAddressPrefixSize` pro každou podsíť musí být zadána.
- Při vytváření nové virtuální sítě, předpona adresy každou podsíť je vypočtena automaticky na základě předponu adresy virtuální sítě a příslušné `addressPrefixSize`.
- Při použití existující virtuální sítě, podsítě, menší než příslušných `constraints.minAddressPrefixSize` jsou k dispozici pro výběr. Kromě toho-li zadána, podsítě, které neobsahují alespoň `minAddressCount` dostupné adresy jsou k dispozici pro výběr.
Výchozí hodnota je **0**. K zajištění, že jsou k dispozici adresy souvislé, zadejte **true** pro `requireContiguousAddresses`. Výchozí hodnota je **true**.
- Vytvoření podsítě v existující virtuální síť se nepodporuje.
- Pokud `options.hideExisting` je **true**, uživatel nemůže vybrat existující virtuální síť. Výchozí hodnota je **false**.

## <a name="sample-output"></a>Ukázkový výstup
```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Další kroky
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
