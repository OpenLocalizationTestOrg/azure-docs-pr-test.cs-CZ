---
title: "Azure spravované aplikace část elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.Section uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 34c0f2f88e6af5a0f822ec116e7e2334e4e29e8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonsection-ui-element"></a><span data-ttu-id="7a9b5-103">Element Microsoft.Common.Section uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="7a9b5-103">Microsoft.Common.Section UI element</span></span>
<span data-ttu-id="7a9b5-104">Ovládací prvek, který seskupuje jeden či více elementů pod nadpisem.</span><span class="sxs-lookup"><span data-stu-id="7a9b5-104">A control that groups one or more elements under a heading.</span></span> <span data-ttu-id="7a9b5-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="7a9b5-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="7a9b5-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="7a9b5-106">UI sample</span></span>
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a><span data-ttu-id="7a9b5-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="7a9b5-108">Schema</span></span>
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="7a9b5-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7a9b5-109">Remarks</span></span>
- <span data-ttu-id="7a9b5-110">`elements`musí obsahovat alespoň jeden element a může obsahovat všechny typy element kromě `Microsoft.Common.Section`.</span><span class="sxs-lookup"><span data-stu-id="7a9b5-110">`elements` must contain at least one element, and can contain all element types except `Microsoft.Common.Section`.</span></span>
- <span data-ttu-id="7a9b5-111">Tento element nepodporuje `toolTip` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7a9b5-111">This element doesn't support the `toolTip` property.</span></span>

## <a name="sample-output"></a><span data-ttu-id="7a9b5-112">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="7a9b5-112">Sample output</span></span>
<span data-ttu-id="7a9b5-113">Pro přístup k výstupu hodnoty elementů v `elements`, použijte [basics()](managed-application-createuidefinition-functions.md#basics) nebo [steps()](managed-application-createuidefinition-functions.md#steps) funkce a s tečkami:</span><span class="sxs-lookup"><span data-stu-id="7a9b5-113">To access the output values of elements in `elements`, use the [basics()](managed-application-createuidefinition-functions.md#basics) or [steps()](managed-application-createuidefinition-functions.md#steps) functions and dot notation:</span></span>

```json
basics('section1').element1
```

<span data-ttu-id="7a9b5-114">Elementy typu `Microsoft.Common.Section` mít žádné hodnoty výstup, sami.</span><span class="sxs-lookup"><span data-stu-id="7a9b5-114">Elements of type `Microsoft.Common.Section` have no output values themselves.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a9b5-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a9b5-115">Next steps</span></span>
* <span data-ttu-id="7a9b5-116">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7a9b5-116">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="7a9b5-117">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7a9b5-117">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="7a9b5-118">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="7a9b5-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
