---
title: "Azure spravované aplikace SizeSelector elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Compute.SizeSelector uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: e54962f73028ada258a7faef271d66f0fbcef649
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a><span data-ttu-id="2e33f-103">Element Microsoft.Compute.SizeSelector uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2e33f-103">Microsoft.Compute.SizeSelector UI element</span></span>
<span data-ttu-id="2e33f-104">Ovládací prvek pro výběr velikost pro jeden nebo více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e33f-104">A control for selecting a size for one or more virtual machine instances.</span></span> <span data-ttu-id="2e33f-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="2e33f-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="2e33f-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2e33f-106">UI sample</span></span>
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a><span data-ttu-id="2e33f-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="2e33f-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="2e33f-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="2e33f-109">Remarks</span></span>
- <span data-ttu-id="2e33f-110">`recommendedSizes`musí obsahovat alespoň jeden velikost.</span><span class="sxs-lookup"><span data-stu-id="2e33f-110">`recommendedSizes` should contain at least one size.</span></span> <span data-ttu-id="2e33f-111">První doporučená velikost je použita jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="2e33f-111">The first recommended size is used as the default.</span></span>
- <span data-ttu-id="2e33f-112">Pokud doporučená velikost není k dispozici ve vybraném umístění, velikost automaticky přeskočen.</span><span class="sxs-lookup"><span data-stu-id="2e33f-112">If a recommended size is not available in the selected location, the size is automatically skipped.</span></span> <span data-ttu-id="2e33f-113">Místo toho se používá další doporučená velikost.</span><span class="sxs-lookup"><span data-stu-id="2e33f-113">Instead, the next recommended size is used.</span></span>
- <span data-ttu-id="2e33f-114">Jakékoli velikosti, nebyly zadány v `constraints.allowedSizes` skryt a jakékoli velikosti, nebyly zadány v `constraints.excludedSizes` se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="2e33f-114">Any size not specified in the `constraints.allowedSizes` is hidden, and any size not specified in `constraints.excludedSizes` is shown.</span></span>
<span data-ttu-id="2e33f-115">`constraints.allowedSizes`a `constraints.excludedSizes` obě jsou nepovinné, ale nelze používat současně.</span><span class="sxs-lookup"><span data-stu-id="2e33f-115">`constraints.allowedSizes` and `constraints.excludedSizes` are both optional, but cannot be used simultaneously.</span></span> <span data-ttu-id="2e33f-116">Seznam dostupných velikostí se dá určit pomocí volání [seznam dostupných velikostí virtuálních počítačů pro předplatné](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span><span class="sxs-lookup"><span data-stu-id="2e33f-116">The list of available sizes can be determined by calling [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span></span>
- <span data-ttu-id="2e33f-117">`osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="2e33f-117">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span> <span data-ttu-id="2e33f-118">Slouží k určení náklady na hardware virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e33f-118">It's used to determine the hardware costs of the virtual machines.</span></span>
- <span data-ttu-id="2e33f-119">`imageReference`pro první strany Image, ale zadaná pro třetí strany bitové kopie je vynechán.</span><span class="sxs-lookup"><span data-stu-id="2e33f-119">`imageReference` is omitted for first-party images, but provided for third-party images.</span></span> <span data-ttu-id="2e33f-120">Slouží k určení náklady na software virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e33f-120">It's used to determine the software costs of the virtual machines.</span></span>
- <span data-ttu-id="2e33f-121">`count`slouží k nastavení odpovídající multiplikátor pro element.</span><span class="sxs-lookup"><span data-stu-id="2e33f-121">`count` is used to set the appropriate multiplier for the element.</span></span> <span data-ttu-id="2e33f-122">Podporuje statické hodnoty, jako je třeba **2**, nebo jako dynamické hodnoty z jiný element `[steps('step1').vmCount]`.</span><span class="sxs-lookup"><span data-stu-id="2e33f-122">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').vmCount]`.</span></span> <span data-ttu-id="2e33f-123">Výchozí hodnota je **1**.</span><span class="sxs-lookup"><span data-stu-id="2e33f-123">The default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="2e33f-124">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="2e33f-124">Sample output</span></span>
```json
"Standard_D1"
```

## <a name="next-steps"></a><span data-ttu-id="2e33f-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e33f-125">Next steps</span></span>
* <span data-ttu-id="2e33f-126">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2e33f-126">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="2e33f-127">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2e33f-127">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="2e33f-128">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="2e33f-128">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
