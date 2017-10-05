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
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a><span data-ttu-id="fec13-103">Element Microsoft.Network.VirtualNetworkCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="fec13-103">Microsoft.Network.VirtualNetworkCombo UI element</span></span>
<span data-ttu-id="fec13-104">Skupina ovládacích prvků pro výběr nový nebo existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="fec13-104">A group of controls for selecting a new or existing virtual network.</span></span> <span data-ttu-id="fec13-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="fec13-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="fec13-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="fec13-106">UI sample</span></span>
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- <span data-ttu-id="fec13-108">V horní obrázek uživatel vybral nové virtuální sítě, takže uživatel může přizpůsobit název a adresu předpona každé podsítě.</span><span class="sxs-lookup"><span data-stu-id="fec13-108">In the top wireframe, the user has picked a new virtual network, so the user can customize each subnet's name and address prefix.</span></span> <span data-ttu-id="fec13-109">Konfigurace podsítí v tomto případě je volitelné.</span><span class="sxs-lookup"><span data-stu-id="fec13-109">Configuring subnets in this case is optional.</span></span>
- <span data-ttu-id="fec13-110">V dolní obrázek uživatel vybral existující virtuální síť, takže uživatel musí být mapována každou podsíť, kterou šablonu nasazení vyžaduje existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="fec13-110">In the bottom wireframe, the user has picked an existing virtual network, so the user must map each subnet the deployment template requires to an existing subnet.</span></span> <span data-ttu-id="fec13-111">Podsítě v takovém případě se vyžaduje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fec13-111">Configuring subnets in this case is required.</span></span>

## <a name="schema"></a><span data-ttu-id="fec13-112">Schéma</span><span class="sxs-lookup"><span data-stu-id="fec13-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="fec13-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="fec13-113">Remarks</span></span>
- <span data-ttu-id="fec13-114">-Li zadána, první nepřekrývají adres předpony velikosti `defaultValue.addressPrefixSize` je určen automaticky v závislosti na existující virtuální sítě v rámci předplatného uživatele.</span><span class="sxs-lookup"><span data-stu-id="fec13-114">If specified, the first non-overlapping address prefix of size `defaultValue.addressPrefixSize` is determined automatically based on the existing virtual networks in the user's subscription.</span></span>
- <span data-ttu-id="fec13-115">Výchozí hodnota pro `defaultValue.name` a `defaultValue.addressPrefixSize` je **null**.</span><span class="sxs-lookup"><span data-stu-id="fec13-115">The default value for `defaultValue.name` and `defaultValue.addressPrefixSize` is **null**.</span></span>
- <span data-ttu-id="fec13-116">`constraints.minAddressPrefixSize`musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="fec13-116">`constraints.minAddressPrefixSize` must be specified.</span></span> <span data-ttu-id="fec13-117">Žádné existující virtuální sítě s adresním prostorem menší než zadaná hodnota jsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="fec13-117">Any existing virtual networks with an address space smaller than the specified value are unavailable for selection.</span></span>
- <span data-ttu-id="fec13-118">`subnets`musí být zadán, a `constraints.minAddressPrefixSize` pro každou podsíť musí být zadána.</span><span class="sxs-lookup"><span data-stu-id="fec13-118">`subnets` must be specified, and `constraints.minAddressPrefixSize` must be specified for each subnet.</span></span>
- <span data-ttu-id="fec13-119">Při vytváření nové virtuální sítě, předpona adresy každou podsíť je vypočtena automaticky na základě předponu adresy virtuální sítě a příslušné `addressPrefixSize`.</span><span class="sxs-lookup"><span data-stu-id="fec13-119">When creating a new virtual network, each subnet's address prefix is calculated automatically based on the virtual network's address prefix and the respective `addressPrefixSize`.</span></span>
- <span data-ttu-id="fec13-120">Při použití existující virtuální sítě, podsítě, menší než příslušných `constraints.minAddressPrefixSize` jsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="fec13-120">When using an existing virtual network, any subnets smaller than the respective `constraints.minAddressPrefixSize` are unavailable for selection.</span></span> <span data-ttu-id="fec13-121">Kromě toho-li zadána, podsítě, které neobsahují alespoň `minAddressCount` dostupné adresy jsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="fec13-121">Additionally, if specified, subnets that do not contain at least `minAddressCount` available addresses are unavailable for selection.</span></span>
<span data-ttu-id="fec13-122">Výchozí hodnota je **0**.</span><span class="sxs-lookup"><span data-stu-id="fec13-122">The default value is **0**.</span></span> <span data-ttu-id="fec13-123">K zajištění, že jsou k dispozici adresy souvislé, zadejte **true** pro `requireContiguousAddresses`.</span><span class="sxs-lookup"><span data-stu-id="fec13-123">To ensure that the available addresses are contiguous, specify **true** for `requireContiguousAddresses`.</span></span> <span data-ttu-id="fec13-124">Výchozí hodnota je **true**.</span><span class="sxs-lookup"><span data-stu-id="fec13-124">The default value is **true**.</span></span>
- <span data-ttu-id="fec13-125">Vytvoření podsítě v existující virtuální síť se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="fec13-125">Creating subnets in an existing virtual network is not supported.</span></span>
- <span data-ttu-id="fec13-126">Pokud `options.hideExisting` je **true**, uživatel nemůže vybrat existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="fec13-126">If `options.hideExisting` is **true**, the user can't choose an existing virtual network.</span></span> <span data-ttu-id="fec13-127">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="fec13-127">The default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="fec13-128">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="fec13-128">Sample output</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="fec13-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fec13-129">Next steps</span></span>
* <span data-ttu-id="fec13-130">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fec13-130">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="fec13-131">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fec13-131">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="fec13-132">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="fec13-132">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
