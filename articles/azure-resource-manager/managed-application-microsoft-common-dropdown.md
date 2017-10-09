---
title: "element uživatelského rozhraní rozevírací seznam spravovaných aplikací aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.DropDown uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 1c07a48ad66b8e8b7fd8f59561776ecb1fc6224f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommondropdown-ui-element"></a><span data-ttu-id="c49a2-103">Element Microsoft.Common.DropDown uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c49a2-103">Microsoft.Common.DropDown UI element</span></span>
<span data-ttu-id="c49a2-104">Výběr ovládacího prvku s rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="c49a2-104">A selection control with a dropdown list.</span></span> <span data-ttu-id="c49a2-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c49a2-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="c49a2-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c49a2-106">UI sample</span></span>
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a><span data-ttu-id="c49a2-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="c49a2-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="c49a2-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c49a2-109">Remarks</span></span>
- <span data-ttu-id="c49a2-110">Popisek Hello `constraints.allowedValues` je hello zobrazený text pro položku, a jeho hodnota může být hodnota výstup hello hello elementu při výběru.</span><span class="sxs-lookup"><span data-stu-id="c49a2-110">hello label for `constraints.allowedValues` is hello display text for an item, and its value is hello output value of hello element when selected.</span></span>
- <span data-ttu-id="c49a2-111">-Li zadána, hello výchozí hodnota musí být součástí štítek `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="c49a2-111">If specified, hello default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="c49a2-112">Pokud není zadaný, hello první položky v `constraints.allowedValues` je vybrána.</span><span class="sxs-lookup"><span data-stu-id="c49a2-112">If not specified, hello first item in `constraints.allowedValues` is selected.</span></span> <span data-ttu-id="c49a2-113">Hello výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="c49a2-113">hello default value is **null**.</span></span>
- <span data-ttu-id="c49a2-114">`constraints.allowedValues`musí obsahovat alespoň jednu položku.</span><span class="sxs-lookup"><span data-stu-id="c49a2-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="c49a2-115">Tento element nepodporuje hello `constraints.required` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c49a2-115">This element doesn't support hello `constraints.required` property.</span></span> <span data-ttu-id="c49a2-116">tooemulate toto chování přidat položku s popisek a hodnota `""` (prázdný řetězec) příliš`constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="c49a2-116">tooemulate this behavior, add an item with a label and value of `""` (empty string) too`constraints.allowedValues`.</span></span>

## <a name="sample-output"></a><span data-ttu-id="c49a2-117">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="c49a2-117">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="c49a2-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c49a2-118">Next steps</span></span>
* <span data-ttu-id="c49a2-119">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c49a2-119">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="c49a2-120">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c49a2-120">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="c49a2-121">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="c49a2-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
