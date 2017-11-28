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
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a><span data-ttu-id="0f2a8-103">Element Microsoft.Network.PublicIpAddressCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="0f2a8-103">Microsoft.Network.PublicIpAddressCombo UI element</span></span>
<span data-ttu-id="0f2a8-104">Skupina ovládacích prvků pro výběr nový nebo existující veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-104">A group of controls for selecting a new or existing public IP address.</span></span> <span data-ttu-id="0f2a8-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="0f2a8-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="0f2a8-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="0f2a8-106">UI sample</span></span>
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- <span data-ttu-id="0f2a8-108">Pokud uživatel hello vybere 'None' pro veřejnou IP adresu, hello domény název popisku textového pole Skrytá.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-108">If hello user selects 'None' for public IP address, hello domain name label text box is hidden.</span></span>
- <span data-ttu-id="0f2a8-109">Pokud uživatel hello vybere stávající veřejnou IP adresu, do textového pole domény popisek hello je zakázané.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-109">If hello user selects an existing public IP address, hello domain name label text box is disabled.</span></span> <span data-ttu-id="0f2a8-110">Jeho hodnota může být popisek názvu domény hello hello vybrané IP adresy.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-110">Its value is hello domain name label of hello selected IP address.</span></span>
- <span data-ttu-id="0f2a8-111">aktualizace přípony (například westus.cloudapp.azure.com) název domény Hello automaticky na základě hello vybrané umístění.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-111">hello domain name suffix (for example, westus.cloudapp.azure.com) updates automatically based on hello selected location.</span></span>

## <a name="schema"></a><span data-ttu-id="0f2a8-112">Schéma</span><span class="sxs-lookup"><span data-stu-id="0f2a8-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="0f2a8-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0f2a8-113">Remarks</span></span>
- <span data-ttu-id="0f2a8-114">Pokud `constraints.required.domainNameLabel` je nastaven příliš**true**, hello uživatel musí poskytnout Popisek názvu domény, při vytváření nové veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-114">If `constraints.required.domainNameLabel` is set too**true**, hello user must provide a domain name label when creating a new public IP address.</span></span> <span data-ttu-id="0f2a8-115">Existující veřejné IP adresy bez štítek nejsou k dispozici pro výběr.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-115">Existing public IP addresses without a label are not available for selection.</span></span>
- <span data-ttu-id="0f2a8-116">Pokud `options.hideNone` je nastaven příliš**true**, pak hello možnost tooselect **žádné** pro veřejnou IP adresu hello adresu skryt.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-116">If `options.hideNone` is set too**true**, then hello option tooselect **None** for hello public IP address is hidden.</span></span> <span data-ttu-id="0f2a8-117">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-117">hello default value is **false**.</span></span>
- <span data-ttu-id="0f2a8-118">Pokud `options.hideDomainNameLabel` je nastaven příliš**true**, hello textové pole pro popisek názvu domény je skrytý.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-118">If `options.hideDomainNameLabel` is set too**true**, then hello text box for domain name label is hidden.</span></span> <span data-ttu-id="0f2a8-119">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-119">hello default value is **false**.</span></span>
- <span data-ttu-id="0f2a8-120">Pokud `options.hideExisting` má hodnotu true, pak uživatel hello není možné toochoose stávající veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-120">If `options.hideExisting` is true, then hello user is not able toochoose an existing public IP address.</span></span> <span data-ttu-id="0f2a8-121">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-121">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="0f2a8-122">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="0f2a8-122">Sample output</span></span>
<span data-ttu-id="0f2a8-123">Pokud uživatel hello vybere žádné veřejnou IP adresu, hello následující výstup se předpokládá, že:</span><span class="sxs-lookup"><span data-stu-id="0f2a8-123">If hello user selects no public IP address, hello following output is expected:</span></span>
```json
{
  "newOrExistingOrNone": "none"
}
```

<span data-ttu-id="0f2a8-124">Pokud uživatel hello vybere nový nebo existující IP adresu, hello následující výstup se předpokládá, že:</span><span class="sxs-lookup"><span data-stu-id="0f2a8-124">If hello user selects a new or existing IP address, hello following output is expected:</span></span>
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- <span data-ttu-id="0f2a8-125">Když `options.hideNone` není zadaný, `newOrExistingOrNone` vždy vrátí hodnotu **žádné**.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-125">When `options.hideNone` is specified, `newOrExistingOrNone` always returns **none**.</span></span>
- <span data-ttu-id="0f2a8-126">Když `options.hideDomainNameLabel` není zadaný, `domainNameLabel` není deklarován.</span><span class="sxs-lookup"><span data-stu-id="0f2a8-126">When `options.hideDomainNameLabel` is specified, `domainNameLabel` is undeclared.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f2a8-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f2a8-127">Next steps</span></span>
* <span data-ttu-id="0f2a8-128">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0f2a8-128">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="0f2a8-129">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0f2a8-129">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="0f2a8-130">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="0f2a8-130">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
