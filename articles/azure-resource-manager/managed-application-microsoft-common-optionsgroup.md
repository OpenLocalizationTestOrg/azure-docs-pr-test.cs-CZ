---
title: "Azure spravované aplikace OptionsGroup elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.OptionsGroup uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 6e147ed28c8248f7f17cb36fd7ae13468141dced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a><span data-ttu-id="f47d8-103">Element Microsoft.Common.OptionsGroup uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="f47d8-103">Microsoft.Common.OptionsGroup UI element</span></span>
<span data-ttu-id="f47d8-104">Ovládací prvek výběru s řádek dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="f47d8-104">A selection control with a row of available options.</span></span> <span data-ttu-id="f47d8-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="f47d8-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="f47d8-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="f47d8-106">UI sample</span></span>
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a><span data-ttu-id="f47d8-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="f47d8-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="f47d8-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f47d8-109">Remarks</span></span>
- <span data-ttu-id="f47d8-110">Popisek pro `constraints.allowedValues` je zobrazený text pro položku, a jeho hodnota může být výstupní hodnotu elementu při výběru.</span><span class="sxs-lookup"><span data-stu-id="f47d8-110">The label for `constraints.allowedValues` is the display text for an item, and its value is the output value of the element when selected.</span></span>
- <span data-ttu-id="f47d8-111">-Li zadána, výchozí hodnota musí být součástí štítek `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="f47d8-111">If specified, the default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="f47d8-112">Pokud není zadaný, první položky v `constraints.allowedValues` je standardně vybraná.</span><span class="sxs-lookup"><span data-stu-id="f47d8-112">If not specified, the first item in `constraints.allowedValues` is selected by default.</span></span> <span data-ttu-id="f47d8-113">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="f47d8-113">The default value is **null**.</span></span>
- <span data-ttu-id="f47d8-114">`constraints.allowedValues`musí obsahovat alespoň jednu položku.</span><span class="sxs-lookup"><span data-stu-id="f47d8-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="f47d8-115">Tento element nepodporuje `constraints.required` vlastnost; musí být vybrána položka úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="f47d8-115">This element doesn't support the `constraints.required` property; an item must be selected to validate successfully.</span></span>

## <a name="sample-output"></a><span data-ttu-id="f47d8-116">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="f47d8-116">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="f47d8-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f47d8-117">Next steps</span></span>
* <span data-ttu-id="f47d8-118">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f47d8-118">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="f47d8-119">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f47d8-119">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="f47d8-120">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="f47d8-120">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
