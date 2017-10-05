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
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a><span data-ttu-id="9a681-103">Element Microsoft.Network.PublicIpAddressCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="9a681-103">Microsoft.Network.PublicIpAddressCombo UI element</span></span>
<span data-ttu-id="9a681-104">Skupina ovládacích prvků pro výběr nový nebo existující veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="9a681-104">A group of controls for selecting a new or existing public IP address.</span></span> <span data-ttu-id="9a681-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="9a681-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="9a681-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="9a681-106">UI sample</span></span>
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- <span data-ttu-id="9a681-108">Pokud uživatel vybere 'None' pro veřejnou IP adresu, název domény popisek textového pole Skrytá.</span><span class="sxs-lookup"><span data-stu-id="9a681-108">If the user selects 'None' for public IP address, the domain name label text box is hidden.</span></span>
- <span data-ttu-id="9a681-109">Pokud uživatel vybere stávající veřejnou IP adresu, textového pole Popisek názvu domény je zakázané.</span><span class="sxs-lookup"><span data-stu-id="9a681-109">If the user selects an existing public IP address, the domain name label text box is disabled.</span></span> <span data-ttu-id="9a681-110">Jeho hodnota může být popisek názvu domény vybraného IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9a681-110">Its value is the domain name label of the selected IP address.</span></span>
- <span data-ttu-id="9a681-111">Aktualizace pro přípony (například westus.cloudapp.azure.com) název se domény, automaticky v závislosti na vybraném umístění.</span><span class="sxs-lookup"><span data-stu-id="9a681-111">The domain name suffix (for example, westus.cloudapp.azure.com) updates automatically based on the selected location.</span></span>

## <a name="schema"></a><span data-ttu-id="9a681-112">Schéma</span><span class="sxs-lookup"><span data-stu-id="9a681-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="9a681-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a681-113">Remarks</span></span>
- <span data-ttu-id="9a681-114">Pokud `constraints.required.domainNameLabel` je nastaven na **true**, musí uživatel zadat popisek názvu domény, při vytváření nové veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="9a681-114">If `constraints.required.domainNameLabel` is set to **true**, the user must provide a domain name label when creating a new public IP address.</span></span> <span data-ttu-id="9a681-115">Existující veřejné IP adresy bez štítek nejsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="9a681-115">Existing public IP addresses without a label are not available for selection.</span></span>
- <span data-ttu-id="9a681-116">Pokud `options.hideNone` je nastaven na **true**, pak možnost vybrat **žádné** pro veřejnou IP adresu skryt.</span><span class="sxs-lookup"><span data-stu-id="9a681-116">If `options.hideNone` is set to **true**, then the option to select **None** for the public IP address is hidden.</span></span> <span data-ttu-id="9a681-117">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="9a681-117">The default value is **false**.</span></span>
- <span data-ttu-id="9a681-118">Pokud `options.hideDomainNameLabel` je nastaven na **true**, textové pole pro popisek názvu domény je skrytý.</span><span class="sxs-lookup"><span data-stu-id="9a681-118">If `options.hideDomainNameLabel` is set to **true**, then the text box for domain name label is hidden.</span></span> <span data-ttu-id="9a681-119">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="9a681-119">The default value is **false**.</span></span>
- <span data-ttu-id="9a681-120">Pokud `options.hideExisting` má hodnotu true, pak uživatel není možné vybrat stávající veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="9a681-120">If `options.hideExisting` is true, then the user is not able to choose an existing public IP address.</span></span> <span data-ttu-id="9a681-121">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="9a681-121">The default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="9a681-122">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="9a681-122">Sample output</span></span>
<span data-ttu-id="9a681-123">Pokud uživatel vybere žádné veřejnou IP adresu, se předpokládá, že následující výstup:</span><span class="sxs-lookup"><span data-stu-id="9a681-123">If the user selects no public IP address, the following output is expected:</span></span>
```json
{
  "newOrExistingOrNone": "none"
}
```

<span data-ttu-id="9a681-124">Pokud uživatel vybere nový nebo existující IP adresu, se předpokládá, že následující výstup:</span><span class="sxs-lookup"><span data-stu-id="9a681-124">If the user selects a new or existing IP address, the following output is expected:</span></span>
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- <span data-ttu-id="9a681-125">Když `options.hideNone` není zadaný, `newOrExistingOrNone` vždy vrátí hodnotu **žádné**.</span><span class="sxs-lookup"><span data-stu-id="9a681-125">When `options.hideNone` is specified, `newOrExistingOrNone` always returns **none**.</span></span>
- <span data-ttu-id="9a681-126">Když `options.hideDomainNameLabel` není zadaný, `domainNameLabel` není deklarován.</span><span class="sxs-lookup"><span data-stu-id="9a681-126">When `options.hideDomainNameLabel` is specified, `domainNameLabel` is undeclared.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a681-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9a681-127">Next steps</span></span>
* <span data-ttu-id="9a681-128">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9a681-128">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="9a681-129">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9a681-129">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="9a681-130">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="9a681-130">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
