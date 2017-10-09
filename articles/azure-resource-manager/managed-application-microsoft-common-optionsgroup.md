---
title: "element uživatelského rozhraní OptionsGroup spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.OptionsGroup uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: e222468009c8db567bdde9f42836a48fb791da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a><span data-ttu-id="651fd-103">Element Microsoft.Common.OptionsGroup uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="651fd-103">Microsoft.Common.OptionsGroup UI element</span></span>
<span data-ttu-id="651fd-104">Ovládací prvek výběru s řádek dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="651fd-104">A selection control with a row of available options.</span></span> <span data-ttu-id="651fd-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="651fd-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="651fd-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="651fd-106">UI sample</span></span>
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a><span data-ttu-id="651fd-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="651fd-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="651fd-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="651fd-109">Remarks</span></span>
- <span data-ttu-id="651fd-110">Popisek Hello `constraints.allowedValues` je hello zobrazený text pro položku, a jeho hodnota může být hodnota výstup hello hello elementu při výběru.</span><span class="sxs-lookup"><span data-stu-id="651fd-110">hello label for `constraints.allowedValues` is hello display text for an item, and its value is hello output value of hello element when selected.</span></span>
- <span data-ttu-id="651fd-111">-Li zadána, hello výchozí hodnota musí být součástí štítek `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="651fd-111">If specified, hello default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="651fd-112">Pokud není zadaný, hello první položky v `constraints.allowedValues` je standardně vybraná.</span><span class="sxs-lookup"><span data-stu-id="651fd-112">If not specified, hello first item in `constraints.allowedValues` is selected by default.</span></span> <span data-ttu-id="651fd-113">Hello výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="651fd-113">hello default value is **null**.</span></span>
- <span data-ttu-id="651fd-114">`constraints.allowedValues`musí obsahovat alespoň jednu položku.</span><span class="sxs-lookup"><span data-stu-id="651fd-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="651fd-115">Tento element nepodporuje hello `constraints.required` vlastnost; položky musí být vybraný toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="651fd-115">This element doesn't support hello `constraints.required` property; an item must be selected toovalidate successfully.</span></span>

## <a name="sample-output"></a><span data-ttu-id="651fd-116">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="651fd-116">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="651fd-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="651fd-117">Next steps</span></span>
* <span data-ttu-id="651fd-118">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="651fd-118">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="651fd-119">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="651fd-119">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="651fd-120">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="651fd-120">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
