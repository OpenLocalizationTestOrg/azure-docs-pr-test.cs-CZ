---
title: "element uživatelského rozhraní VirtualNetworkCombo spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Network.VirtualNetworkCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 1b0fa5360d93306f7a814723f77e42540bdaaa9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a><span data-ttu-id="c7fa2-103">Element Microsoft.Network.VirtualNetworkCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c7fa2-103">Microsoft.Network.VirtualNetworkCombo UI element</span></span>
<span data-ttu-id="c7fa2-104">Skupina ovládacích prvků pro výběr nový nebo existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-104">A group of controls for selecting a new or existing virtual network.</span></span> <span data-ttu-id="c7fa2-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c7fa2-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="c7fa2-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c7fa2-106">UI sample</span></span>
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- <span data-ttu-id="c7fa2-108">V horní obrázek hello hello uživatel vybral nové virtuální sítě, takže hello uživatele můžete přizpůsobit název a adresu předpona každé podsítě.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-108">In hello top wireframe, hello user has picked a new virtual network, so hello user can customize each subnet's name and address prefix.</span></span> <span data-ttu-id="c7fa2-109">Konfigurace podsítí v tomto případě je volitelné.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-109">Configuring subnets in this case is optional.</span></span>
- <span data-ttu-id="c7fa2-110">V dolní obrázek hello hello uživatel vybral existující virtuální síť, proto musí být mapována hello uživatele každé podsítě hello nasazení šablony vyžaduje tooan existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-110">In hello bottom wireframe, hello user has picked an existing virtual network, so hello user must map each subnet hello deployment template requires tooan existing subnet.</span></span> <span data-ttu-id="c7fa2-111">Podsítě v takovém případě se vyžaduje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-111">Configuring subnets in this case is required.</span></span>

## <a name="schema"></a><span data-ttu-id="c7fa2-112">Schéma</span><span class="sxs-lookup"><span data-stu-id="c7fa2-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="c7fa2-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c7fa2-113">Remarks</span></span>
- <span data-ttu-id="c7fa2-114">-Li zadána, hello první předpona adresy nepřekrývají velikosti `defaultValue.addressPrefixSize` je určen automaticky v závislosti na existující virtuální sítě v rámci předplatného hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-114">If specified, hello first non-overlapping address prefix of size `defaultValue.addressPrefixSize` is determined automatically based on the existing virtual networks in hello user's subscription.</span></span>
- <span data-ttu-id="c7fa2-115">Výchozí hodnota pro Hello `defaultValue.name` a `defaultValue.addressPrefixSize` je **null**.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-115">hello default value for `defaultValue.name` and `defaultValue.addressPrefixSize` is **null**.</span></span>
- <span data-ttu-id="c7fa2-116">`constraints.minAddressPrefixSize`musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-116">`constraints.minAddressPrefixSize` must be specified.</span></span> <span data-ttu-id="c7fa2-117">Žádné existující virtuální sítě s menší než hello zadaná hodnota je k dispozici pro výběr adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-117">Any existing virtual networks with an address space smaller than hello specified value are unavailable for selection.</span></span>
- <span data-ttu-id="c7fa2-118">`subnets`musí být zadán, a `constraints.minAddressPrefixSize` pro každou podsíť musí být zadána.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-118">`subnets` must be specified, and `constraints.minAddressPrefixSize` must be specified for each subnet.</span></span>
- <span data-ttu-id="c7fa2-119">Při vytváření nové virtuální sítě, předpona adresy každou podsíť je vypočtena automaticky na základě předponu adresy hello virtuální sítě a příslušné `addressPrefixSize`.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-119">When creating a new virtual network, each subnet's address prefix is calculated automatically based on hello virtual network's address prefix and the respective `addressPrefixSize`.</span></span>
- <span data-ttu-id="c7fa2-120">Při použití existující virtuální sítě, podsítě, menší než příslušných `constraints.minAddressPrefixSize` jsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-120">When using an existing virtual network, any subnets smaller than the respective `constraints.minAddressPrefixSize` are unavailable for selection.</span></span> <span data-ttu-id="c7fa2-121">Kromě toho-li zadána, podsítě, které neobsahují alespoň `minAddressCount` dostupné adresy jsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-121">Additionally, if specified, subnets that do not contain at least `minAddressCount` available addresses are unavailable for selection.</span></span>
<span data-ttu-id="c7fa2-122">Hello výchozí hodnota je **0**.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-122">hello default value is **0**.</span></span> <span data-ttu-id="c7fa2-123">tooensure, který hello dostupné adresy jsou souvislé, zadejte **true** pro `requireContiguousAddresses`.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-123">tooensure that hello available addresses are contiguous, specify **true** for `requireContiguousAddresses`.</span></span> <span data-ttu-id="c7fa2-124">Hello výchozí hodnota je **true**.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-124">hello default value is **true**.</span></span>
- <span data-ttu-id="c7fa2-125">Vytvoření podsítě v existující virtuální síť se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-125">Creating subnets in an existing virtual network is not supported.</span></span>
- <span data-ttu-id="c7fa2-126">Pokud `options.hideExisting` je **true**, uživatel hello nelze vybrat existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-126">If `options.hideExisting` is **true**, hello user can't choose an existing virtual network.</span></span> <span data-ttu-id="c7fa2-127">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="c7fa2-127">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="c7fa2-128">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="c7fa2-128">Sample output</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="c7fa2-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7fa2-129">Next steps</span></span>
* <span data-ttu-id="c7fa2-130">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7fa2-130">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="c7fa2-131">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7fa2-131">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="c7fa2-132">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="c7fa2-132">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
