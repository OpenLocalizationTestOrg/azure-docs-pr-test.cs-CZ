---
title: "element uživatelského rozhraní SizeSelector spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Compute.SizeSelector uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d93306135d9c6f9a83692766ce1ca7ea2b688086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a><span data-ttu-id="66f47-103">Element Microsoft.Compute.SizeSelector uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="66f47-103">Microsoft.Compute.SizeSelector UI element</span></span>
<span data-ttu-id="66f47-104">Ovládací prvek pro výběr velikost pro jeden nebo více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="66f47-104">A control for selecting a size for one or more virtual machine instances.</span></span> <span data-ttu-id="66f47-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="66f47-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="66f47-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="66f47-106">UI sample</span></span>
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a><span data-ttu-id="66f47-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="66f47-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="66f47-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="66f47-109">Remarks</span></span>
- <span data-ttu-id="66f47-110">`recommendedSizes`musí obsahovat alespoň jeden velikost.</span><span class="sxs-lookup"><span data-stu-id="66f47-110">`recommendedSizes` should contain at least one size.</span></span> <span data-ttu-id="66f47-111">Hello nejprve doporučená velikost je používat jako výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="66f47-111">hello first recommended size is used as hello default.</span></span>
- <span data-ttu-id="66f47-112">Pokud doporučená velikost není k dispozici v umístění hello vybrané, se automaticky přeskočí hello velikost.</span><span class="sxs-lookup"><span data-stu-id="66f47-112">If a recommended size is not available in hello selected location, hello size is automatically skipped.</span></span> <span data-ttu-id="66f47-113">Místo toho hello Další doporučená velikost je použít.</span><span class="sxs-lookup"><span data-stu-id="66f47-113">Instead, hello next recommended size is used.</span></span>
- <span data-ttu-id="66f47-114">Jakékoli velikosti, nebyly zadány v hello `constraints.allowedSizes` skryt a jakékoli velikosti, nebyly zadány v `constraints.excludedSizes` se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="66f47-114">Any size not specified in hello `constraints.allowedSizes` is hidden, and any size not specified in `constraints.excludedSizes` is shown.</span></span>
<span data-ttu-id="66f47-115">`constraints.allowedSizes`a `constraints.excludedSizes` obě jsou nepovinné, ale nelze používat současně.</span><span class="sxs-lookup"><span data-stu-id="66f47-115">`constraints.allowedSizes` and `constraints.excludedSizes` are both optional, but cannot be used simultaneously.</span></span> <span data-ttu-id="66f47-116">můžete určit Hello seznam dostupných velikostí voláním [seznam dostupných velikostí virtuálních počítačů pro předplatné](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span><span class="sxs-lookup"><span data-stu-id="66f47-116">hello list of available sizes can be determined by calling [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span></span>
- <span data-ttu-id="66f47-117">`osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="66f47-117">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span> <span data-ttu-id="66f47-118">Použije se náklady na hardware hello toodetermine hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="66f47-118">It's used toodetermine hello hardware costs of hello virtual machines.</span></span>
- <span data-ttu-id="66f47-119">`imageReference`pro první strany Image, ale zadaná pro třetí strany bitové kopie je vynechán.</span><span class="sxs-lookup"><span data-stu-id="66f47-119">`imageReference` is omitted for first-party images, but provided for third-party images.</span></span> <span data-ttu-id="66f47-120">Použije se náklady na software hello toodetermine hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="66f47-120">It's used toodetermine hello software costs of hello virtual machines.</span></span>
- <span data-ttu-id="66f47-121">`count`je použité tooset hello odpovídající multiplikátor pro hello element.</span><span class="sxs-lookup"><span data-stu-id="66f47-121">`count` is used tooset hello appropriate multiplier for hello element.</span></span> <span data-ttu-id="66f47-122">Podporuje statické hodnoty, jako je třeba **2**, nebo jako dynamické hodnoty z jiný element `[steps('step1').vmCount]`.</span><span class="sxs-lookup"><span data-stu-id="66f47-122">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').vmCount]`.</span></span> <span data-ttu-id="66f47-123">Hello výchozí hodnota je **1**.</span><span class="sxs-lookup"><span data-stu-id="66f47-123">hello default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="66f47-124">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="66f47-124">Sample output</span></span>
```json
"Standard_D1"
```

## <a name="next-steps"></a><span data-ttu-id="66f47-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66f47-125">Next steps</span></span>
* <span data-ttu-id="66f47-126">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66f47-126">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="66f47-127">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66f47-127">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="66f47-128">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="66f47-128">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
