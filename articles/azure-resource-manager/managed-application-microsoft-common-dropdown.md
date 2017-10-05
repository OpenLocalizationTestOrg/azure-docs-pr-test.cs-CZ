---
title: "Azure spravované aplikace rozevírací prvek uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.DropDown uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: a769e14efbae928b811fa1f1b1c2d4fba3c7692b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommondropdown-ui-element"></a><span data-ttu-id="b6d73-103">Element Microsoft.Common.DropDown uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b6d73-103">Microsoft.Common.DropDown UI element</span></span>
<span data-ttu-id="b6d73-104">Výběr ovládacího prvku s rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="b6d73-104">A selection control with a dropdown list.</span></span> <span data-ttu-id="b6d73-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b6d73-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="b6d73-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b6d73-106">UI sample</span></span>
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a><span data-ttu-id="b6d73-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="b6d73-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
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

## <a name="remarks"></a><span data-ttu-id="b6d73-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b6d73-109">Remarks</span></span>
- <span data-ttu-id="b6d73-110">Popisek pro `constraints.allowedValues` je zobrazený text pro položku, a jeho hodnota může být výstupní hodnotu elementu při výběru.</span><span class="sxs-lookup"><span data-stu-id="b6d73-110">The label for `constraints.allowedValues` is the display text for an item, and its value is the output value of the element when selected.</span></span>
- <span data-ttu-id="b6d73-111">-Li zadána, výchozí hodnota musí být součástí štítek `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="b6d73-111">If specified, the default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="b6d73-112">Pokud není zadaný, první položky v `constraints.allowedValues` je vybrána.</span><span class="sxs-lookup"><span data-stu-id="b6d73-112">If not specified, the first item in `constraints.allowedValues` is selected.</span></span> <span data-ttu-id="b6d73-113">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="b6d73-113">The default value is **null**.</span></span>
- <span data-ttu-id="b6d73-114">`constraints.allowedValues`musí obsahovat alespoň jednu položku.</span><span class="sxs-lookup"><span data-stu-id="b6d73-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="b6d73-115">Tento element nepodporuje `constraints.required` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b6d73-115">This element doesn't support the `constraints.required` property.</span></span> <span data-ttu-id="b6d73-116">Emulovat toto chování, přidejte položku s popisek a hodnota `""` (prázdný řetězec) k `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="b6d73-116">To emulate this behavior, add an item with a label and value of `""` (empty string) to `constraints.allowedValues`.</span></span>

## <a name="sample-output"></a><span data-ttu-id="b6d73-117">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="b6d73-117">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="b6d73-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6d73-118">Next steps</span></span>
* <span data-ttu-id="b6d73-119">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6d73-119">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="b6d73-120">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6d73-120">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="b6d73-121">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="b6d73-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>